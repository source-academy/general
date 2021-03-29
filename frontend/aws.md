# Frontend Amazon S3 + CloudFront deployment

If you are using an AWS IAM user instead of the root user to access your
account, ensure you have access to S3 and CloudFront.

1. If you have not already done so, [install the AWS
   CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html). Then, [generate an access key for your
   user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html#Using_CreateAccessKey) and
   [configure the AWS
   CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html#cli-configure-quickstart-config)
   with those credentials.

   For the default AWS region, you should [pick the AWS
   region](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html#concepts-available-regions)
   closest to you and your users. Use this region for all AWS services that you use.

2. Follow AWS's tutorial on [creating an S3 bucket for static website
   hosting](https://docs.aws.amazon.com/AmazonS3/latest/userguide/HostingWebsiteOnS3Setup.html).

   **Do only steps 1 to 4** ("Create a bucket", "Enable static website
   hosting", "Edit Block Public Access settings", and "Add a bucket policy that
   makes your bucket content publicly available").

   In step two, fill in `index.html` for both the index and error document.

   Do not upload files yet.

3. Follow steps 1 to 4 of the [manual deployment
   guide](index.md#manual-deployment) to configure and build the frontend.

   Then, in your shell, enter the `build` directory and run the following commands to upload the files to the bucket.

   ```
   aws s3 sync --delete --exclude '*.wasm' . "YOUR BUCKET NAME HERE"
   aws s3 sync --exclude '*' --include '*.wasm' --content-type 'application/wasm' . "YOUR BUCKET NAME HERE"
   ```

   Replace `YOUR BUCKET NAME HERE` with the name of your bucket.

4. Follow AWS's tutorial on [using CloudFront to serve a static website hosted on
   S3](https://aws.amazon.com/premiumsupport/knowledge-center/cloudfront-serve-static-website/#Using_a_website_endpoint_as_the_origin.2C_with_anonymous_.28public.29_access_allowed).

   You have already done steps 1 and 3 in that tutorial in step 2 of this guide.

   When creating the CloudFront distribution, under "Default Cache Behavior Settings", select "Yes" for "Compress
   Objects Automatically".

5. Done!

To update the site, simply repeat step 3 of this guide. Additionally, run the following command:

```
aws cloudfront create-invalidation --distribution-id "CLOUDFRONT ID HERE" --paths '/externalLibs/*' '/manifest.json' '/asset-manifest.json' '/service-worker.js' '/index.html' '/assets/*' '/'
```

Replace `CLOUDFRONT ID HERE` with your distribution ID (something like `EA3DF4YZDML1G`).

This command creates an _invalidation_, clearing the CloudFront cache so updated contents are served.
