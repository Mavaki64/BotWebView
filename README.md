# Bot WebView

Adds a button to Discord’s account panel (bottom-left) to open an external bot dashboard in a **native window** that looks and behaves like a Discord modal.

This is implemented as a Vencord **userplugin** (source build required).

## Why a native window?

Discord’s CSP often blocks external sites from being embedded in `iframe`s (`frame-src`).  
Using an Electron `BrowserWindow` avoids `iframe` / CSP / `X-Frame-Options` limitations.

## Features

- Account panel button (bottom-left)
- Configurable **Dashboard URL**
- Modal-like window: dark backdrop, Discord-like header, `Esc` to close
- Click outside to close (backdrop)
- Hidden scrollbars (mouse wheel / trackpad scrolling still works)
- Discord OAuth handling (keeps cookies/session for the configured dashboard)

## Windows installation (from source)

### 0) Prerequisites

- Discord Desktop installed
- Git installed
- Node.js \(>= 18\)
- pnpm

<details>
<summary>How do I know if Node.js is already installed?</summary>

Run in PowerShell:

```powershell
node -v
```

If it prints a version (e.g. `v20.11.1`), you can **skip Node installation**.

</details>

<details>
<summary>How do I know if pnpm is already installed?</summary>

Run in PowerShell:

```powershell
pnpm -v
```

If it prints a version, you can **skip pnpm installation**.

</details>

<details>
<summary>How do I know if Git is already installed?</summary>

Run in PowerShell:

```powershell
git -v
```

If it prints a version, you can **skip git installation**.

</details>

### 1) Install Node.js (skip if already installed)

Install the Node.js LTS version from the official website, then restart your terminal.

[Download Node.js](https://nodejs.org/en/download)

On the download page, select **Windows Installer (.msi)** and install it.

### 2) Install pnpm (skip if already installed)

Vencord uses pnpm. The recommended way is via Corepack (bundled with Node):

```powershell
corepack enable
corepack prepare pnpm@10 --activate
pnpm -v
```

If you have trouble installing pnpm via Corepack, you can install it via npm instead:

```powershell
npm install -g pnpm
```

### 3) Install git (skip if already installed)

Install Git using `winget`, then open a new terminal.

```powershell
winget install --id Git.Git -e
```

If you don't have `winget`, install Git from [git-scm.com](https://git-scm.com/download/win).

### 4) Clone Vencord source

Open a terminal in the folder where you want to install Vencord (e.g. right-click a folder and choose “Open in Terminal”), then run:

```powershell
git clone https://github.com/Vendicated/Vencord
cd Vencord
```

### 5) Add this userplugin

Download the plugin ZIP archive: --> [HERE](https://github.com/Mavaki64/BotWebView/archive/refs/heads/main.zip) <--

Open the archive and open the `BotWebView-main` folder. Copy the `botWebView` folder.

Paste this folder into (if the `userplugins` folder doesn't exist, you need to create it):

`Vencord\src\userplugins`

You’ll find this folder in the directory you chose earlier when cloning/installing Vencord.

You should end up with:

- `src/userplugins/botWebView/index.tsx`
- `src/userplugins/botWebView/native.ts`
- `src/userplugins/botWebView/README.md`

### 6) Install dependencies

From the Vencord repository folder, run:

```powershell
pnpm install
```

### 7) Build Vencord

From the Vencord repository folder, run:

```powershell
pnpm build
```

### 8) Inject into Discord

Fully close Discord (quit it from the system tray), then run from the Vencord repository folder:

```powershell
pnpm inject
```

When you run this command, the installer will print the Discord installation folder. By default, it is: `C:\Users\<YourName>\AppData\Local\Discord`

Re-open Discord.

## Configuration

Go to:
**Settings → Vencord → Plugins → Bot WebView**

Set:

- **Dashboard URL** (example: `https://bot.example.com/login`)

If the URL is empty, clicking the button will show an error toast.

## Updating Vencord (and rebuilding)

When Discord updates, you may need to update/rebuild Vencord from source.

1. Fully close Discord (quit it from the system tray).
2. Open a terminal in your Vencord repository folder.
3. Pull updates, reinstall deps, rebuild, then re-inject:

```powershell
git pull
pnpm install
pnpm build
pnpm inject
```

Notes:

- If you have local changes, `git pull` may fail. Commit/stash your changes first.
- Re-injecting after a rebuild is the simplest way to ensure the new build is loaded.

## Security notes

- The plugin does not read or store your credentials.
- Only set `dashboardUrl` to websites you trust.
- Sessions/cookies are stored in an Electron persistent partition derived from the dashboard domain.

## Author

Mavaki
