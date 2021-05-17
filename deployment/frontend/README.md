# Frontend deployment

The frontend can be deployed on any static site hosting service that supports SPAs.

## Deployment methods

- Local development: see the [cadet-frontend
  README](https://github.com/source-academy/cadet-frontend#installation-of-course-edition).
- [Manual build and deploy to a web server](#manual-deployment)
- [Deployment to a static site host](#static-site-host-deployment)
- [Amazon S3 + CloudFront deployment](aws.md) (This is what is used by CS1101S.)

## Manual deployment

1. Clone the cadet-frontend repository: `git clone https://github.com/source-academy/cadet-frontend.git`

2. Enter the directory and install dependencies: `yarn install --frozen-lockfile`

3. Configure the frontend for your deployment. [See the cadet-frontend
   README](https://github.com/source-academy/cadet-frontend#setting-up-your-environment), as well as the
   [production-specific configuration](https://github.com/source-academy/cadet-frontend#build-and-deployment). Also set
   up the authentication configuration; you may wish to check out the [authentication guide](../auth/index.md).

   Additionally set (in the `.env` file) `PUBLIC_URL` to be the URL of your deployment. For example,
   `https://source-academy.github.io`. If you are not deploying to the root of the domain, you must specify the full
   path as well. (Note that deploying to a subdirectory is untested and may not work.)

4. Run the build: `yarn run build`

5. The build is output to `build/`. Upload the files to your web host.

6. Ensure that your web host is configured to route all non-existent paths to the root (i.e. `/` or `/index.html`). This
   might be done through a `.htaccess` file if your host uses the Apache httpd.

7. Done!

To update the site, simply repeat steps 4 onwards after pulling and/or making your desired changes.

## Static site host deployment

The stack used by cadet-frontend is very common and quite well-supported. Some services include
[Netlify](https://www.netlify.com/), [Vercel](https://vercel.com/), [Render](https://render.com/),
[Surge](https://surge.sh/), and [Cloudflare Pages](https://pages.cloudflare.com/).

Most of these services are able to integrate with a GitHub repository and deploy automatically on push.

1. First, [fork the repository](https://github.com/source-academy/cadet-frontend).

2. Create a new site in your preferred service, and select your forked repository.

3. Set the build command to `yarn run build`, and the build output directory to `build/`. If needed, specify the command
   to install dependencies as `yarn install --frozen-lockfile`. Consult your service's documentation on deploying Yarn
   (or Node.js) projects for more information.

4. Configure the frontend. Instead of using a `.env`, you should be able to set environment variables using the
   service's interface or configuration file.

   [See the cadet-frontend README](https://github.com/source-academy/cadet-frontend#setting-up-your-environment), as
   well as the [production-specific
   configuration](https://github.com/source-academy/cadet-frontend#build-and-deployment), for the environment variables
   to configure.

   Additionally set `PUBLIC_URL` to be the URL of your deployment. For example, `https://source-academy.github.io`.

5. Trigger a build using the service.

7. Ensure that your web host is configured to route all non-existent paths to the root (i.e. `/` or `/index.html`).
   Consult your service's documentation on deploying single-page apps (SPAs) for more information. (Note: GitHub Pages
   does not support this.)

8. Done!

To update the site, you should be able to push directly to your fork.
