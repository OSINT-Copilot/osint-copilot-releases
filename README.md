# OSINT Copilot — releases

Downloads for **OSINT Copilot**, an OSINT investigation workspace built on plain
markdown files.

This repository exists only to distribute builds. The source lives in a private
repository; nothing here but release artifacts and this file. (GitHub adds a
"Source code" link to every release page. It holds this README and nothing else.)

Take the newest build from the [Releases](../../releases) page.

## Installing the application

| File | For |
| --- | --- |
| `osint-copilot_<version>_amd64.deb` | Debian / Ubuntu, x86-64 |
| `osint-copilot-<version>-x86_64.AppImage` | Any Linux, x86-64 |
| `osint-copilot-<version>-arm64.AppImage` | Any Linux, arm64 |
| `OSINT-Copilot-Setup-<version>.exe` | Windows, installs |
| `OSINT-Copilot-Portable-<version>.exe` | Windows, runs without installing |

There is no macOS build. It needs an Apple Developer ID certificate and notarization,
without which macOS will not open the application at all.

The `latest*.yml` files are metadata for the in-app updater. They are not downloads;
leave them attached to the release or updates will stop working.

### The .deb, which needs no setup

```bash
sudo apt install ./osint-copilot_0.14.0_amd64.deb
```

Then start OSINT Copilot from the applications menu. Installing also sets up the
Chromium sandbox the application runs in, and its menu entry passes the display
flag the next section explains.

### The AppImage

```bash
chmod +x osint-copilot-0.14.0-x86_64.AppImage
./osint-copilot-0.14.0-x86_64.AppImage --ozone-platform=x11
```

`--ozone-platform=x11` is not optional on most systems: without it the window can
fail to appear at all, with no error, because Electron's Wayland backend crashes as
it opens one.

On **Ubuntu 24.04 and later** the AppImage stops before any window appears:

```
FATAL:sandbox/linux/suid/client/setuid_sandbox_host.cc:166] The SUID sandbox helper
binary was found, but is not configured correctly.
```

Ubuntu blocks the user namespace the sandbox normally uses, so Chromium looks for
its setuid helper — which cannot work from inside an AppImage, because the
filesystem an AppImage mounts itself on ignores the setuid bit. Running `chmod` on
the `.AppImage` file does not help; the file in the message is inside that mount.

Either install the .deb instead, or allow the namespace:

```bash
sudo sysctl -w kernel.apparmor_restrict_unprivileged_userns=0
echo kernel.apparmor_restrict_unprivileged_userns=0 \
  | sudo tee /etc/sysctl.d/60-apparmor-namespace.conf     # survives a reboot
```

If the AppImage instead reports `Cannot mount AppImage, please check your FUSE
setup`, run it as `APPIMAGE_EXTRACT_AND_RUN=1 ./osint-copilot-0.14.0-x86_64.AppImage
--ozone-platform=x11`.

### Windows

Run `OSINT-Copilot-Setup-<version>.exe`, or `OSINT-Copilot-Portable-<version>.exe` to run
it without installing.

Windows will stop you the first time, with **"Windows protected your PC"**. The builds are
not signed yet, and SmartScreen warns about any installer it has not seen signed. Choose
**More info**, then **Run anyway**. A code-signing certificate is what removes this, and
getting one is on the list.

## Installing the browser extension

`osint-copilot-extension-<version>.zip` sends notes, entities, page captures and
highlights from a web page into an investigation. It talks only to the application
on the same computer.

It works in **Chrome 140+ and other Chromium browsers** (Edge, Brave, Helium).
There is no Firefox build. It is not in any web store yet, so install it by hand:

1. Unzip it somewhere you will keep — removing the folder uninstalls the extension.
2. Open `chrome://extensions` (Brave: `brave://extensions`, Edge: `edge://extensions`).
3. Turn on **Developer mode**, top right.
4. **Load unpacked**, and pick the unzipped folder.

Then pair it with the application, which is what lets the two talk:

1. In OSINT Copilot, open the OSINT settings. Under **Browser extension**, turn on
   **Accept items from the browser extension**.
2. Choose **Pair new extension**. The application shows a one-time pairing string
   starting with `ocpair1.`, good for five minutes and one use.
3. Click the extension's toolbar button to open its side panel, paste the string and
   choose **Pair**. OSINT Copilot then asks you to confirm, naming the extension.
4. Pick the investigation in the side panel. What you send goes there until you pick
   another.

Pairing is per browser profile, and the application accepts nothing from the
extension while it is unlicensed or while that setting is off.

Two permissions stay optional, and the extension asks only when you first use the
feature: capturing a page, and letting highlights reappear when you return to a
page. Both show the "read your data on all sites" warning, because that is the
access they need.

## A licence key is required

The application asks for a key before it will open a vault. There is no trial,
and a packaged build has no bypass. Keys are verified on your own machine against
a public key compiled into the build, so they work with no network connection —
and nothing about your licence is ever sent anywhere.

If you need a key, contact Probe Point Analytics.
