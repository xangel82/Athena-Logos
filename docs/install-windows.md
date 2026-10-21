# Installing Athena Logos on Windows

For Windows 10 and 11 on x64 PCs. You need
[Athena-Engine](https://github.com/xangel82/Athena-Engine) running on this PC or
on another machine of your network.

## 1. Download and unpack

1. Download `athena-logos-<version>-win-x64.zip` from the
   [latest release](https://github.com/xangel82/athena-logos/releases/latest).
2. Right-click the file → **Extract All…** and choose a folder, for example
   `C:\Athena`. Keep the whole folder together: the licence and the notices
   travel with the program.

## 2. First start

Open **PowerShell** in that folder (Shift + right-click → **Open PowerShell
window here**) and start Athena, telling her where the engine is:

```powershell
$env:ATHENA_ENGINE_URL = "http://192.168.1.10:30007"
.\athena-logos-win-x64.exe
```

If the engine is on this same PC with its default port, the first line is not
needed.

- **SmartScreen** may say «Windows protected your PC», because the program is
  not signed with a Windows certificate: click **More info** → **Run anyway**.
- **Windows Firewall** asks whether the program may use the network. Allow
  **private networks** if you want to open Athena from other devices (phone,
  tablet, another computer); if you use her only from this PC you can deny it.

The terminal prints the address and a **6-digit setup code**. Leave the window
open: closing it stops Athena.

## 3. Open the app

Go to `https://localhost:8080` (from another device:
`https://<this PC's address>:8080`). The browser warns about the certificate,
because Athena makes her own: choose **Advanced** → **Continue**.

Type the setup code, create the administrator and choose the folder Athena can
work in. Done.

## Engine with an API key or its own certificate

```powershell
$env:ATHENA_ENGINE_URL = "https://192.168.1.10:30443"
$env:ATHENA_API_KEY = "<the engine's key>"
.\athena-logos-win-x64.exe
```

For a certificate of your own (for example Caddy `tls internal`), copy its root
certificate to `%USERPROFILE%\.athena-voice\engine-ca.pem`. The same values can
be changed later in **Settings → AI engine**.

## Start with Windows (optional)

Create a shortcut to `athena-logos-win-x64.exe`, set **Start in** to its folder,
and put it in the Startup folder (Win + R → `shell:startup`). To pass the engine
address, set `ATHENA_ENGINE_URL` once as a user environment variable (Settings →
System → About → Advanced system settings → Environment Variables).

## Update and remove

- **Update:** close the PowerShell window, replace the folder with the new
  version, start again. Your data is in `%USERPROFILE%\.athena-voice`.
- **Remove:** delete the folder, and `%USERPROFILE%\.athena-voice` to erase all
  data.
