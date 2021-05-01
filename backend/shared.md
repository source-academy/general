# Compiling the backend

You should do this on a system similar to the system you will be using the backend on i.e. use same Linux distribution.
Compiling on a different Linux distribution should work as long as the Elixir you use to compile does not depend on
too-new a glibc. (Of course, you can just do this on the server you will be running the backend on.)

1. Install Elixir on your system. Make sure the versions are at least those listed
   [here](https://github.com/source-academy/cadet#system-requirements). Following the instructions
   [here](https://elixir-lang.org/install.html) is fine. If you want to use a version manager like
   [asdf](https://github.com/asdf-vm/asdf), that should be fine too. (But we do the former.)

   After installing Elixir, set up the dependency manager: `mix local.hex --force && mix local.rebar --force`

2. Clone the cadet repository: `git clone https://github.com/source-academy/cadet.git`

3. Enter the directory and install dependencies: `mix deps.get`

4. Build: `MIX_ENV=prod mix release`

5. The release archive is at `_build/prod/cadet-0.0.1.tar.gz`.

6. Return to the guide you came from.

# Configuring the backend

The backend in production mode loads its configuration from `/etc/cadet.exs` by default. You can specify a path in the
environment variable `CONFIG` to override this.

Copy the [example configuration](https://github.com/source-academy/cadet/blob/master/config/cadet.exs.example) to
`/etc/cadet.exs` (or some other location, if you are overriding the default), and edit the values.

In particular, you should edit the CORS allowed origins (`cors_endpoints`), the database credentials, as well as all the
AWS resource names and ARNs to match those you have created, if any. Also set up the authentication configuration; you
may wish to check out the [authentication guide](../auth/index.md).

Once done, return to the guide you came from.
