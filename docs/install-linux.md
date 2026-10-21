# Installing Athena Logos on Linux

For Linux on arm64: NVIDIA DGX Spark, ASUS Ascent GX10 and other ARM 64-bit
machines (tested on Ubuntu 24.04). It can run on the same machine as
[Athena-Engine](https://github.com/xangel82/Athena-Engine) or on another one.

## 1. Download and unpack

```sh
mkdir -p ~/athena-logos && cd ~/athena-logos
# download athena-logos-<version>-linux-arm64.tar.gz from
# https://github.com/xangel82/athena-logos/releases/latest
tar xzf athena-logos-<version>-linux-arm64.tar.gz
cd linux-arm64
sha256sum -c SHA256SUMS
```

## 2. First start

On the same machine as the engine, with its default port:

```sh
./athena-logos-linux-arm64
```

With the engine elsewhere, or with an API key:

```sh
ATHENA_ENGINE_URL=https://192.168.1.10:30443 ATHENA_API_KEY='<the engine key>' ./athena-logos-linux-arm64
```

The terminal prints the address and a **6-digit setup code**.

If a firewall is active, open the port for your network, for example with
`sudo ufw allow from 192.168.1.0/24 to any port 8080 proto tcp`.

## 3. Open the app

From any device of your network go to `https://<this machine's address>:8080`.
The browser warns about the certificate, because Athena makes her own: accept
it once. Type the setup code, create the administrator and choose the folder
Athena can work in.

For a certificate of the engine's own (for example Caddy `tls internal`), copy
its root certificate to `~/.athena-voice/engine-ca.pem`.

## Run as a service (optional)

A user service that starts with the machine:

```sh
mkdir -p ~/.config/systemd/user
cat > ~/.config/systemd/user/athena-logos.service <<'EOF'
[Unit]
Description=Athena Logos
After=network-online.target

[Service]
WorkingDirectory=%h/athena-logos/linux-arm64
ExecStart=%h/athena-logos/linux-arm64/athena-logos-linux-arm64
Environment=ATHENA_ENGINE_URL=http://localhost:30007
Restart=on-failure

[Install]
WantedBy=default.target
EOF
systemctl --user daemon-reload
systemctl --user enable --now athena-logos
loginctl enable-linger "$USER"     # keep it running after you log out
journalctl --user -u athena-logos -f   # the setup code is printed here
```

On a machine that also runs the engine, mind the memory: on DGX Spark and GX10
memory is shared between CPU and GPU, and Athena Logos itself needs little,
but large jobs of the engine (videos, big models) need room.

## Update and remove

- **Update:** `systemctl --user stop athena-logos` (or Ctrl + C), replace the
  folder with the new version, start again. Your data is in `~/.athena-voice`.
- **Remove:** disable the service if you created it, delete
  `~/athena-logos`, and `~/.athena-voice` to erase all data.
