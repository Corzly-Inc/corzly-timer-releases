# Corzly Timer

Corzly Timer is the Corzly desktop time and activity tracker. Install it on the computer you work from, sign in with your **Corzly Command Center** email and password, and press **Play** to start tracking.

**⬇️ [Download the latest version](https://github.com/Corzly-Inc/corzly-timer-releases/releases/latest)**

- [macOS: quick install with Terminal](#macos--quick-install-with-terminal-recommended) (recommended)
- [macOS: manual install](#macos--manual-install)
- [macOS: permissions](#macos--permissions-required)
- [Windows](#windows)
- [Updating](#updating)
- [Troubleshooting](#troubleshooting)
- [Uninstall](#uninstall)

---

## macOS: quick install with Terminal (recommended)

This picks the right version for your Mac (Intel or Apple Silicon), installs it in **Applications**, and opens it.

1. Open **Terminal**: press `⌘ Space`, type **Terminal**, press **Return**.
2. Copy the whole block below, paste it into Terminal, and press **Return**:

```bash
(
set -e
ARCH=$(uname -m | sed 's/x86_64/x64/')
URL=$(curl -fsSL https://api.github.com/repos/Corzly-Inc/corzly-timer-releases/releases/latest | grep -o "https://[^\"]*-${ARCH}\.dmg" | head -1)
[ -n "$URL" ] || { echo "❌ Could not find the download. Check your internet connection."; exit 1; }
TMP=$(mktemp -d)
echo "Downloading $URL"
curl -fL --progress-bar -o "$TMP/CorzlyTimer.dmg" "$URL"
hdiutil attach "$TMP/CorzlyTimer.dmg" -nobrowse -quiet -mountpoint "$TMP/dmg"
osascript -e 'quit app "Corzly Timer"' 2>/dev/null || true
sleep 2
rm -rf "/Applications/Corzly Timer.app"
cp -R "$TMP/dmg/Corzly Timer.app" /Applications/
hdiutil detach "$TMP/dmg" -quiet
xattr -dr com.apple.quarantine "/Applications/Corzly Timer.app" 2>/dev/null || true
open "/Applications/Corzly Timer.app"
echo "✅ Corzly Timer installed"
)
```

3. Sign in with your Command Center email and password.
4. Grant the two permissions in [macOS: permissions](#macos--permissions-required).

> If you see `Permission denied`, your Mac account isn't an administrator. Ask your IT admin to run it, or put `sudo ` in front of the `rm -rf` and `cp -R` lines and enter your Mac password when asked.

---

## macOS: manual install

### 1. Download the right file

| Your Mac | File to download |
|---|---|
| **Apple Silicon** (M1, M2, M3, M4…) | `Corzly-Timer-<version>-arm64.dmg` |
| **Intel** | `Corzly-Timer-<version>-x64.dmg` |

Not sure? Apple menu  → **About This Mac**. **Chip: Apple M…** means Apple Silicon, and **Processor: Intel…** means Intel.
Or run this in Terminal. `arm64` means Apple Silicon, and `x86_64` means Intel:

```bash
uname -m
```

### 2. Install

1. Open the downloaded `.dmg`.
2. Drag **Corzly Timer** into the **Applications** folder.
3. Eject the disk image. Its eject button is in Finder's sidebar.

### 3. Open it the first time

macOS will say **"Corzly Timer can't be opened because Apple cannot check it for malicious software."** This is expected for apps distributed outside the App Store. Allow it **once** using either option:

**Option A: Terminal (quickest)**

```bash
xattr -dr com.apple.quarantine "/Applications/Corzly Timer.app" && open "/Applications/Corzly Timer.app"
```

**Option B: System Settings**

1. Try to open **Corzly Timer** and click **Done** or **OK** on the warning.
2. Open **System Settings → Privacy & Security**.
3. Scroll down to *"Corzly Timer was blocked…"* and click **Open Anyway**.
4. Enter your Mac password, then click **Open**.

---

## macOS: permissions (required)

Corzly Timer needs two macOS permissions. Without them it can't see which app you're working in or take screenshots, and your activity won't sync.

| Permission | Why it's needed |
|---|---|
| **Accessibility** | Lets Corzly Timer read which app and window you're using, so your time is logged against the right app. |
| **Screen Recording** | Takes periodic screenshots **only while the timer is running**. |

The app shows a banner with **Open Settings** and **View instructions** buttons until both are granted.

### Grant Accessibility

1. Open the settings page from the app's banner, or paste this in Terminal:
   ```bash
   open "x-apple.systempreferences:com.apple.preference.security?Privacy_Accessibility"
   ```
2. Turn on **Corzly Timer**. If it isn't listed, click **+**, choose **Applications → Corzly Timer**, and click **Open**.
3. Enter your Mac password if asked.

### Grant Screen Recording

1. Open the settings page from the app's banner, or paste this in Terminal:
   ```bash
   open "x-apple.systempreferences:com.apple.preference.security?Privacy_ScreenCapture"
   ```
2. Turn on **Corzly Timer**. If it isn't listed, click **+** and add it from **Applications**.
3. When macOS asks, click **Quit & Reopen**. If it doesn't ask, quit Corzly Timer from the menu-bar icon and open it again.

### Permissions already on but the app keeps asking?

This can happen after installing a new version over an older one. macOS remembers the old copy of the app, so the switches look on but don't apply to the new one. Reset both permissions and grant them again:

1. Quit Corzly Timer.
2. Run this in Terminal:
   ```bash
   tccutil reset Accessibility com.corzly.desktop-agent
   tccutil reset ScreenCapture com.corzly.desktop-agent
   open "/Applications/Corzly Timer.app"
   ```
3. Grant **Accessibility** and **Screen Recording** again as described above.

---

## Windows

1. Download **`Corzly-Timer-Setup-<version>.exe`** from the [latest release](https://github.com/Corzly-Inc/corzly-timer-releases/releases/latest).
2. Run it. If Windows SmartScreen says *"Windows protected your PC"*, click **More info → Run anyway**.
3. Follow the installer, then open **Corzly Timer** from the Start menu.
4. Sign in with your Command Center email and password.

Windows doesn't need any extra permissions.

---

## Updating

- Corzly Timer checks for updates when it starts and every few hours. When an update is ready you'll see **Restart now**.
- To check now: click the Corzly Timer icon in the menu bar (Mac) or system tray (Windows) → **Check for Updates**.
- **On version 0.1.1 or older?** Those versions can't update themselves. Install the latest version once using the steps above. Your data and login are kept. On Mac, then do the [permission reset](#permissions-already-on-but-the-app-keeps-asking).
- On Mac, if an update doesn't install, run the [quick install](#macos--quick-install-with-terminal-recommended) command again. It always installs the newest version.

Check which version you have:

```bash
defaults read "/Applications/Corzly Timer.app/Contents/Info.plist" CFBundleShortVersionString
```

---

## Troubleshooting

| Problem | Fix |
|---|---|
| "Apple cannot check it for malicious software" | See [Open it the first time](#3-open-it-the-first-time). |
| "Corzly Timer is damaged and can't be opened" | Run `xattr -dr com.apple.quarantine "/Applications/Corzly Timer.app"` and open it again. |
| *"A JavaScript error occurred… incompatible architecture"* | You installed the wrong file for your Mac. Download the other `.dmg` (see [the table above](#1-download-the-right-file)), or use the [quick install](#macos--quick-install-with-terminal-recommended). |
| The app keeps asking for permissions that are already on | Do the [permission reset](#permissions-already-on-but-the-app-keeps-asking). |
| No apps or screenshots in Command Center | Check both [permissions](#macos--permissions-required) are on, then quit and reopen the app. |
| "Check for Updates" shows a 404 error | You're on an old version. Install the latest version manually once. |
| Asked to log in again after restarting | Make sure you're connected to the internet. If it keeps happening, update to the latest version. |

Still stuck? Contact your Corzly admin and include your app version, your Mac model (Intel or Apple Silicon), and a screenshot of the error.

---

## Uninstall

**macOS:** quit Corzly Timer, then run:

```bash
osascript -e 'quit app "Corzly Timer"' 2>/dev/null
rm -rf "/Applications/Corzly Timer.app"
tccutil reset Accessibility com.corzly.desktop-agent
tccutil reset ScreenCapture com.corzly.desktop-agent
```

**Windows:** Settings → Apps → **Corzly Timer** → Uninstall.

Stop the timer first so your last session syncs.
