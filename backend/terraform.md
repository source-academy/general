# Full Terraform deployment

1. [Install Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli#install-terraform).

   Generate an AWS access key and secret for Terraform, and specify them [either as environment variables or using the
   AWS CLI configuration file](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#environment-variables).

   The access key must be able to access AWS IAM, Secrets Manager, RDS, S3, Lambda, STS, EC2, ELB, Autoscaling, and IoT.
   This policy works:

   ```json
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "VisualEditor0",
               "Effect": "Allow",
               "Action": [
                   "iam:*",
                   "secretsmanager:*",
                   "rds:*",
                   "s3:*",
                   "lambda:*",
                   "sts:*",
                   "ec2:*",
                   "elasticloadbalancing:*",
                   "autoscaling:*",
                   "iot:*"
               ],
               "Resource": "*"
           }
       ]
   }
   ```

2. Generate an SSH RSA key, and upload the public key to AWS EC2. We will refer to this as the "API SSH key". Also,
   upload your own SSH public key (which must be RSA since EC2 does not support Ed25519 keys...) to EC2. We will refer
   to this as the "Bastion SSH key".

3. In AWS Lambda, install the XVFB Lambda layer, and name it "xvfb". Also, download the compiled grader Lambda ZIP archive.

   (If you are not in the Source Academy organisation, please contact us for these archives. These are built on GitHub
   CI, but we are yet to publish these build artifacts in a way accessible to the public.)

4. Enter the `deployment` directory of your copy of the `cadet` repository.

5. Create a file `terraform.tfvars` in the directory, with these contents:

   ```
   api_ssh_key_name     = "API SSH key name here (as named in EC2)"
   bastion_ssh_key_name = "Bastion SSH key name here (as named in EC2)"
   lambda_filename      = "path/to/grader.zip (on your local machine)"
   env                  = "unique-identifier"
   assets_bucket        = "unique-identifier-assets"
   ```

   Fill in the fields. The `env` value is used to name S3 buckets, which must be unique within an AWS region (not just
   within your account!), so use something that is likely to be unique e.g. your module/course code, or some random
   identifier. (It must be alphanumeric, since it is used in S3 bucket names.)

   The `assets_bucket` value is the name of the bucket containing game/story assets. (It is not generated from `env` as
   this is likely to persist across runs of a module/course, while the other resources are torn down once a run of a
   module/course ends.)

   You can also specify the values of other parameters. See `inputs.tf`.

6. Open `main.tf`.

   Modify the details under the "backend" key as needed. The defaults specify the S3 bucket used by the CS1101S
   deployment. You can also use [other backends](https://www.terraform.io/docs/language/settings/backends/index.html).

   Note: "backend" in Terraform parlance refers to the location Terraform persists its state.

   Modify the details under the "provider" key as needed. (Mainly the AWS region.)

7. Initialise Terraform by running `terraform init` (in the current directory i.e. `cadet/deployment`).

7. [Import](https://www.terraform.io/docs/cli/commands/import.html) any existing resources, if needed. (This most likely
   applies to any existing S3 buckets that you would like to re-use.)

   If you are starting from scratch, then there is nothing to import.

8. Provision the resources by running `terraform apply`.

9. In AWS IoT, create a Thing Group and name it `<env>-sling`. Go to the Security tab and attach the `<env>-sling` policy to it.

   (This is done because Terraform's AWS provider does not yet support thing groups.)

10. Go to the EC2 control panel, and open up the autoscaling group created by Terraform. Under Details, scroll down to
    Advanced configuration and click Edit. Under Suspended process, suspend InstanceRefresh, AZRebalance, Terminate and
    HealthCheck.

    (This is done because we don't currently automatically configure instances automatically, so if the ASG were to
    actually autoscale or terminate instances, the new instance would be empty.)

10. Go to the EC2 control panel. Find the public IP of the Bastion instance.

    Using your Bastion SSH key, SSH to it. Install the API SSH key's private key into the Bastion (i.e. copy it to `.ssh/id_rsa` and `.ssh/id_rsa.pub`).

11. On the Bastion, SSH to API A using the private/internal IP as displayed in EC2.

    [Install the backend on it](index#install-on-linux-server). Set API A as the leader. (See the remark in step 4 of the linked guide.)

12. Repeat for API B. API B is **not** the leader.

13. Go to CloudFront. Create or update the distribution for the backend.

    If you already have a distribution configured for the backend in the past, simply update the origin to the ELB that
    was just created.

    If not, create a new CloudFront distribution. Select the ELB created in step 8 as the origin.

    Leave the rest of the origin settings as default. In particular:

    - Origin protocol policy: HTTP only
    - HTTP port: 80

    For the default cache behaviour:

    - Viewer protocol policy: HTTPS only
    - Allowed HTTP methods: GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE
    - Cached HTTP methods: Tick OPTIONS
    - Select Use legacy cache settings
    - Cache based on selected request headers: Whitelist
    - Add Authorization and Host
    - Query string forwarding and caching: Forward all, cache based on all
    - Compress objects automatically: Yes

    Any other options can be left at the default.

14. Wait for the distribution to propagate. Then, test that the backend can be reached.

15. Done!

## Zero-downtime update

1. Go to the EC2 control panel. Under Load Balancing, select Target Groups. Open the target group for the backend deployment.

2. Go to the Targets tab. Select API A, and select Deregister.

3. SSH to API A via the Bastion. Monitor the backend access log using `journalctl -fu cadet`.

   Once there are no more requests, update the backend by uploading the new package and running the script again as
   described in [the installation guide](index#install-on-linux-server).

4. Go back to the Targets tab. Select Register targets. Select the API A instance, and ensure the port is correct. (Our
   default configuration uses 4000.) Then click Include as pending below, and then Register pending targets.

5. Check that API A is serving requests using `journalctl -fu cadet`. (IF there are any issues, they will probably show
   up too.)

6. Once API A is up, repeat steps 2 to 5 with API B.

If a new database migration needs to be run, run it after deregistering API B. (In this situation, just be as quick as
possible to minimise downtime, since it is likely that the updated node will have some endpoints broken until the
migration is run, and then once it is run the not-updated node may start failing.)
