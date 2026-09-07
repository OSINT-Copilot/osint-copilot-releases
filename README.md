# OSINT Copilot — releases

Downloads for **OSINT Copilot**, an OSINT investigation workspace built on plain
markdown files.

This repository exists only to distribute builds. The source lives in a private
repository; nothing here but release artifacts and this file.

## Downloading

Take the newest build from the [Releases](../../releases) page.

| File | For |
| --- | --- |
| `osint-copilot_<version>_amd64.deb` | Debian / Ubuntu, x86-64 |
| `osint-copilot-<version>-x64.AppImage` | Any Linux, x86-64 |
| `osint-copilot-<version>-arm64.AppImage` | Any Linux, arm64 |

The `latest-linux*.yml` files are metadata for the in-app updater. They are not
downloads; leave them attached to the release or updates will stop working.

## A licence key is required

The application asks for a key before it will open a vault. There is no trial,
and a packaged build has no bypass. Keys are verified on your own machine against
a public key compiled into the build, so they work with no network connection —
and nothing about your licence is ever sent anywhere.

If you need a key, contact Probe Point Analytics.

## Linux notes

The packaged desktop entry launches with `--ozone-platform=x11`. Under a Wayland
session the app otherwise initialises and then crashes in the compositor, and the
platform cannot be selected from within the application — Chromium fixes it
before the main script runs. Launching through the desktop entry (your
applications menu) is therefore the supported route.
