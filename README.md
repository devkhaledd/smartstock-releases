# SmartStock v4 — Releases

Public distribution point for **SmartStock v4** desktop installers and their
signed update manifests. **This repository contains no source code.**

It exists so that an installed copy of SmartStock can check for, download and
verify an update **anonymously** — no credential of any kind is embedded in the
application, because everything it fetches is public.

## What each release contains

| Asset | Purpose |
|---|---|
| `SmartStock-<version>-win-x64.exe` | The Windows installer (NSIS). |
| `SmartStock-<version>-win-x64.exe.blockmap` | Differential download — an existing install fetches only changed blocks. |
| `latest.yml` | The update manifest `electron-updater` reads. |
| `latest.yml.sig` | Base64 **Ed25519** detached signature over the exact bytes of `latest.yml`. |
| `SmartStock-<version>-win-x64.exe.sha256` | First-install integrity check, for a human. |

## How authenticity is established

The application embeds only the **public** half of an Ed25519 key pair. The
private half exists solely as a CI secret and is never shipped, never committed
and never present on a customer machine.

Before a single byte of an installer is downloaded, the desktop verifies
`latest.yml.sig` against `latest.yml`. It **fails closed**: a missing,
malformed or invalid signature means no update is applied. The `sha512` of the
installer is carried inside that signed manifest, so artifact integrity is
transitively covered by the same signature.

A release is published only after CI has produced the signature, re-verified it
against the public half compiled into that very build, and asserted the asset
list is complete. Every failure before that point leaves a draft nobody can
download — the outcome is always "nothing was published", never "something
unverifiable was".

## Installing

Download the `.exe` from the [latest release](../../releases/latest) and run it.
Existing customer data — database, attachments and backups — lives outside the
install directory and is preserved across upgrades and reinstalls.

SmartStock is licence-gated at startup: an installer without a valid licence is
inert.

---

Source code is maintained privately. Issues and support are handled through
your SmartStock support channel, not this repository.
