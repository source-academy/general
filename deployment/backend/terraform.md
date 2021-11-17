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

2. Generate an SSH RSA key locally using `ssh-keygen`, and upload the public key to AWS EC2. We will refer to this as
   the "API SSH key". Also, upload your own SSH public key (which must be RSA since EC2 does not support Ed25519
   keys...) to EC2. We will refer to this as the "Bastion SSH key".

   The API SSH key is used to SSH from the Bastion node to the API nodes. The Bastion SSH key is used to SSH to the
   Bastion.

   Note: when uploading the public keys, be sure to switch to the correct AWS region!

3. In AWS Lambda, install the XVFB Lambda layer, and name it "xvfb". Also, download the compiled grader Lambda ZIP archive.

   (If you are not in the Source Academy organisation, please contact us for these archives. These are built on GitHub
   CI, but we are yet to publish these build artifacts in a way accessible to the public.)

4. Think of a unique identifier for your deployment. It will be used to name S3 buckets, so it needs to be something
   that is likely to be unique e.g. your module/course code, or some random identifier. (It must be alphanumeric, since
   it is used in S3 bucket names.)

   The identifier will be referred to using `<unique-identifier>` below.

5. Create the assets bucket, which contains game/story assets. If you already have an assets bucket from a previous
   deployment, you can use that.

   Go to AWS S3, and select Create bucket. Name it `<unique-identifier>-assets`, and select the correct AWS region.

   Uncheck Block all public access, and tick the acknowledgement. The remaining settings can be left at their default.

   Once the bucket is created, open the bucket and go to the Permissions tab. In the Bucket policy panel, select Edit.
   Paste the following, filling in `<unique-identifier>`:

   ```json
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "AddPerm",
               "Effect": "Allow",
               "Principal": "*",
               "Action": "s3:GetObject",
               "Resource": "arn:aws:s3:::<unique-identifier>-assets/*"
           }
       ]
   }
   ```

   This bucket policy makes all objects accessible, which is required as this bucket serves game/story assets.

6. Create the configuration bucket, which contains the backend configuration. If you already have a configuration bucket from a previous deployment, you can use that.

   Go to AWS S3, and select Create bucket. Name it `<unique-identifier>-config`, and select the correct AWS region.

   **Select** Block all public access. The remaining settings can be left at their default.

7. [Configure the backend](shared.md#configuring-the-backend). Then upload the file to the configuration bucket created
   in the last step. (You can name the file anything; you might use `cadet.exs`, for example.)

8. Enter the `deployment/terraform` directory of your copy of the backend repository.

   Create a file `terraform.tfvars` in the directory, with these contents:

   ```
   api_ssh_key_name     = "API SSH key name here (as named in EC2)"
   bastion_ssh_key_name = "Bastion SSH key name here (as named in EC2)"
   lambda_filename      = "path/to/grader.zip (on your local machine)"
   env                  = "<unique-identifier>"
   assets_bucket        = "<unique-identifier>-assets (as in step 4)"
   config_bucket        = "<unique-identifier>-config (as in step 5)"
   config_object        = "cadet.exs (as in step 5)"
   ```

   Fill in the fields. You can also specify the values of other parameters. See `inputs.tf`.

9. Open `main.tf`.

   Modify the details under the "backend" key as needed. The defaults specify the S3 bucket used by the CS1101S
   deployment. You can also use [other backends](https://www.terraform.io/docs/language/settings/backends/index.html).

   Note: "backend" in Terraform parlance refers to the location Terraform persists its state.

   Modify the details under the "provider" key as needed. (Mainly the AWS region.)

10. Initialise Terraform by running `terraform init` (in the current directory i.e. `backend/deployment/terraform`).

11. Provision the resources by running `terraform apply`.

    Note: if you are running on Mac, ensure that you have granted your Terminal and/or whatever application you are
    running Terraform through (e.g. VS Code using its built-in terminal) has the Full Disk Access permission.

    Note: if you encounter an error like this:

    > error creating Secrets Manager Secret: InvalidRequestException: You can't create this secret because a secret with
    > this name is already scheduled for deletion

    you will have to use the AWS CLI to force-delete the secret:

    ```
    aws secretsmanager delete-secret --force-delete-without-recovery --secret-id secret-id-here
    ```

12. In AWS IoT, create a Thing Group and name it `<unique-identifier>-sling`, if a thing group by that name does not
    already exist. Go to the Security tab and attach the `<unique-identifier>-sling` policy to it.

    (This is done because Terraform's AWS provider does not yet support thing groups.)

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
