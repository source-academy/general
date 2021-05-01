# Authentication services

* [In-house service](#in-house-service)
* [Google](#google)
* [Amazon Cognito](#amazon-cognito)
* [Auth0](#auth0)

## In-house service

If you wish to integrate with your organisation/institution's pre-existing authentication service, you will likely have
to write a custom authentication provider yourself.

In general, it should be simple to integrate any service that supports the OAuth2 authorisation code flow. You will then
need to write an authentication provider that receives the authorisation code and then exchanges it for a authorisation
token, and retrieves the user's username, name, and role via API calls or by extracting it from the token.

See our [LumiNUS authentication
provider](https://github.com/source-academy/cadet/blob/master/lib/cadet/auth/providers/luminus.ex) for an example.

If your authentication provider supports OpenID Connect and exposes an OpenID discovery document, then most of this is
handled for you via the OpenID authentication provider; you will only need to write an extractor that, given the access
token, returns the user's username, name, and role. See our [Cognito claim
extractor](https://github.com/source-academy/cadet/blob/master/lib/cadet/auth/providers/cognito_claim_extractor.ex) for
an example.

## Google

You can use Google as an authentication service. This means that users will sign in using their Google account. The
Google account can be a normal @gmail.com account, or it can be an organisation/G Suite account.

Note that because it is not possible to restrict who has a Google account, you must manually create users in the
database if you are using Google as an authentication service.

1. Go to the [Google API Console](https://console.cloud.google.com/apis/dashboard), and create a project (or use one you
   already have, if you wish).

2. Go to the [credentials](https://console.cloud.google.com/apis/credentials) page. At the top, select Create
   credentials, then select the OAuth client ID option. Select Web application as the application type. Give it any
   name.

   Under Authorised redirect URIs, add the URI to the `/login?provider=google` route of your deployment. For example, if
   your deployment is accessible at `https://sourceacademy.nus.edu.sg`, add `https://sourceacademy.nus.edu.sg/login`.
   Note: if Google is not your first authentication provider in the frontend configuration, add `?provider=google` to
   the URI.

   You can also add localhost e.g. `http://localhost:8000/login` for the local frontend development
   server.

3. Go to the [OAuth consent screen](https://console.cloud.google.com/apis/credentials/consent) page and configure it to
   your liking. There are no special scopes required.

4. Note the client ID and client secret created in step 2.

   In your frontend configuration, add the following variables, changing the number and `CLIENT_ID_HERE` accordingly. Note: if the number is not 1, make sure you followed the note in step 2.

   ```
   REACT_APP_OAUTH2_PROVIDER1=google
   REACT_APP_OAUTH2_PROVIDER1_NAME=Google
   REACT_APP_OAUTH2_PROVIDER1_ENDPOINT=https://accounts.google.com/o/oauth2/v2/auth?response_type=code&client_id=CLIENT_ID_HERE&scope=openid%20email
   ```

   In your backend configuration (`cadet.exs`), add the following to the `identity_providers` map:

   ```elixir
   "google" =>
     {Cadet.Auth.Providers.OpenID,
      %{
        openid_provider: :google,
        claim_extractor: Cadet.Auth.Providers.GoogleClaimExtractor
      }},
   ```

   and the following to the `openid_connect_providers` list:

   ```elixir
   google: [
     discovery_document_uri: "https://accounts.google.com/.well-known/openid-configuration",
     client_id: "CLIENT_ID_HERE",
     client_secret: "CLIENT_SECRET_HERE",
     response_type: "code",
     scope: "openid email"
   ],
   ```

5. Done! Note that you need to [add users manually](../maint/add-users.md). For Google, the username is the primary
   email of the Google account.

## Amazon Cognito

TODO.

## Auth0

TODO.
