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
sudo apt install ./osint-copilot_0.14.2_amd64.deb
```

Then start OSINT Copilot from the applications menu. Installing also sets up the
Chromium sandbox the application runs in, and its menu entry picks the display
backend directly.

### The AppImage

```bash
chmod +x osint-copilot-0.14.2-x86_64.AppImage
./osint-copilot-0.14.2-x86_64.AppImage
```

No flags, no environment variables. From 0.14.2 the application handles the two things that
used to stop it on Linux:

* **Wayland.** Electron's Wayland backend crashes as it opens a window on some systems, so on a
  Wayland session the application restarts once with the X11 backend.
* **The Chromium sandbox.** Ubuntu 24.04 and later forbid unprivileged user namespaces, and the
  setuid helper inside an AppImage cannot work, because AppImage contents are mounted `nosuid`.
  Chromium aborted rather than run unprotected. The application now unpacks itself once into
  `~/.cache/OSINT Copilot/0.14.2` — an ordinary filesystem, where the sandbox works — and runs
  from there. The first start says so and takes a few seconds; later starts reuse it, and old
  versions are deleted.

`OC_NO_SELF_INSTALL=1` skips the unpacking and `OC_OZONE=native` skips the restart, if you would
rather choose for yourself.

If the AppImage will not mount at all — `Cannot mount AppImage, please check your FUSE setup` —
run `APPIMAGE_EXTRACT_AND_RUN=1 ./osint-copilot-0.14.2-x86_64.AppImage`, or install `libfuse2`.

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

## Uninstalling

**If you installed the .deb:**

```bash
sudo apt remove osint-copilot
```

**If you used the AppImage**, delete the file, and the copy it unpacks on Ubuntu 24.04 and later:

```bash
rm -rf ~/.cache/osint-copilot ~/.cache/osint-copilot-updater
```

That copy is around 400 MB and is recreated on the next start, so it is also the thing to delete
if a start ever goes wrong and you want a clean one.

**Your own data lives separately**, in `~/.config/OSINT Copilot`: the licence key, the list of
workspaces, and the browser extension's pairing. Removing the application leaves it alone. Delete
it only for a completely fresh start — you will have to enter the licence key and pair the
extension again:

```bash
rm -rf ~/.config/"OSINT Copilot"
```

Investigations themselves are ordinary folders wherever you created them, and are never touched by
any of this.

## A licence key is required

The application asks for a key before it will open a vault. There is no trial,
and a packaged build has no bypass. Keys are verified on your own machine against
a public key compiled into the build, so they work with no network connection —
and nothing about your licence is ever sent anywhere.

If you need a key, contact Probe Point Analytics.
