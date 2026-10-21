# Installing Athena Logos on macOS

For Macs with Apple Silicon (M1 or later). The program is signed with a
Developer ID and notarized by Apple. You need
[Athena-Engine](https://github.com/xangel82/Athena-Engine) running on a machine
of your network.

## 1. Download and unpack

1. Download `athena-logos-<version>-macos-arm64.zip` from the
   [latest release](https://github.com/xangel82/athena-logos/releases/latest).
2. Double-click it: you get a folder `macos-arm64`. Move it where you like, for
   example to `~/Applications/Athena`.

## 2. First start

Open **Terminal**, go to the folder and start Athena, telling her where the
engine is:

```sh
cd ~/Applications/Athena/macos-arm64
ATHENA_ENGINE_URL=http://192.168.1.10:30007 ./athena-logos-macos-arm64
```

The first start needs an internet connection: macOS checks the notarization with
Apple. If macOS asks whether the program may accept incoming network
connections, allow it if you want to open Athena from other devices.

The terminal prints the address and a **6-digit setup code**. Leave the window
open: closing it stops Athena.

## 3. Open the app

Go to `https://localhost:8080` (from another device:
`https://<this Mac's address>:8080`). The browser warns about the certificate,
because Athena makes her own: in Safari choose **Show Details** → **visit this
website**; in Chrome **Advanced** → **Proceed**.

Type the setup code, create the administrator and choose the folder Athena can
work in. Done.

## Engine with an API key or its own certificate

```sh
ATHENA_ENGINE_URL=https://192.168.1.10:30443 ATHENA_API_KEY='<the engine key>' ./athena-logos-macos-arm64
```

For a certificate of your own (for example Caddy `tls internal`), copy its root
certificate to `~/.athena-voice/engine-ca.pem`. The same values can be changed
later in **Settings → AI engine**.

## Update and remove

- **Update:** stop Athena (Ctrl + C in the Terminal), replace the folder with
  the new version, start again. Your data is in `~/.athena-voice`.
- **Remove:** delete the folder, and `~/.athena-voice` to erase all data.
