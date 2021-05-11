# Backend

The backend can be deployed on any Linux server. (It is, in theory, possible to
deploy it on Windows and macOS as well, as Elixir runs on those platforms, but
we do not test that the backend works those platforms.)

## Deployment methods

- Local development: see the [cadet README](https://github.com/source-academy/cadet#developer-setup)
- [Install on Linux server](#install-on-linux-server)
- [Full Terraform deployment](terraform.md) (This is what is used by CS1101S.)

## Scripted install on Linux server

1. On the server, [configure the backend](shared.md#configuring-the-backend).

2. If you are running on Ubuntu 20.04, you can run the automated installation script:

   ```bash
   curl -LO https://raw.githubusercontent.com/source-academy/cadet/stable/deployment/init.sh
   # inspect init.sh
   sudo bash init.sh
   ```

   The script downloads and extracts the precompiled Linux release, sets up a systemd unit file, and starts the backend.
   The configuration file should be at `/etc/cadet.exs`.

   If you are not running Ubuntu 20.04, you may try the script, but the precompiled script may not work if your glibc
   version is not new enough. In that case, [manually compile](#manual-compile) the backend and install it.

3. Check that the backend is accessible at http://localhost:4000 (on the server).

4. Note that you need a service to act as a TLS termination proxy for the backend. If you are following our Terraform
   deployment, this will be AWS's Elastic Load Balancer.

   If not, consider using Nginx. [This
   guide](https://www.digitalocean.com/community/tutorials/how-to-set-up-nginx-load-balancing-with-ssl-termination) may
   be helpful, but instead of having an upstream with multiple endpoints, just `proxy_pass http://localhost:4000`.

   Alternatively, you can terminate SSL directly at the backend; follow [this
   guide](https://hexdocs.pm/phoenix/using_ssl.html) (the configuration changes can be merged into the endpoint key in
   `cadet.exs`).

5. Check that the backend is accessible over HTTPS from your own computer.

6. Next, follow the guide to [manually setup the optional AWS services](aws-manual.md), if needed.

To update the backend, repeat steps 2, 3, 4, and 6.

## Manual compile

1. [Compile the backend](shared.md#compiling-the-backend).

2. Transfer the package to the server the backend will be run on.

3. On the server, [configure the backend](shared.md#configuring-the-backend).

4. Install this systemd unit file onto the server, at `/etc/systemd/system/cadet.service`.

   ```ini
   [Unit]
   After=network.target
   Requires=network.target
   StartLimitIntervalSec=0

   [Service]
   Type=simple
   TimeoutStartSec=0
   Restart=always
   RestartSec=5
   ExecStart=/opt/cadet/bin/cadet start
   User=nobody
   Environment=HOME=/opt/cadet/tmp
   Environment=PORT=4000

   [Install]
   WantedBy=multi-user.target
   ```

5. Place this Bash script into the same directory as the package.

   ```bash
   #!/bin/bash

   BASEDIR=/opt/cadet

   sudo systemctl stop cadet
   sudo rm -rf "$BASEDIR"
   sudo mkdir -p "$BASEDIR"
   sudo tar -zxf cadet-0.0.1.tar.gz -C "$BASEDIR" --no-same-owner
   sudo mkdir -p "$BASEDIR/tmp"
   sudo chmod 1777 "$BASEDIR"/{tmp,lib/tzdata-*/priv/tmp_downloads}
   sudo chmod a+x "$BASEDIR"/{bin/cadet,erts-*/bin/*,releases/*/{iex,elixir}}
   sudo chown -R nobody:nogroup "$BASEDIR"/lib/tzdata-*/priv/{release_ets,latest_remote_poll.txt}
   /opt/cadet/bin/cadet eval Cadet.Release.migrate
   sudo systemctl start cadet
   ```

   (Note: adapt as needed. This script makes it so the extracted files are owned by root, but gives write access to
   certain files which are modified at runtime. It assumes that the backend is going to be run as `nobody`.)

   Make it executable and execute the script.

   The script extracts the release package to `/opt/cadet`. If you wish to install it somewhere else, change the script
   and systemd service file accordingly.

6. Continue with the rest of the steps in the scripted install guide.
