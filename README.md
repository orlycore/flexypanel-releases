# Flexypanel releases

Signed releases of [Flexypanel](https://github.com/orlycore/flexypanel), a web hosting control panel for one administrator: every site runs in its own Podman containers behind nginx on the host.

## Install

On a fresh AlmaLinux 10 (x86_64) server, as root:

```sh
curl -fsSL https://github.com/orlycore/flexypanel-releases/releases/latest/download/install.sh | sudo bash
```

Options go after `-s --`, for example the panel's hostname:

```sh
curl -fsSL https://github.com/orlycore/flexypanel-releases/releases/latest/download/install.sh | sudo bash -s -- --hostname panel.example.com
```

The commands above install the newest release that has passed the full test (marked "Latest"); newer pre-releases are listed under Releases. To install a specific version, download that tag's installer and pass the same version:

```sh
curl -fsSL https://github.com/orlycore/flexypanel-releases/releases/download/vX.Y.Z/install.sh | sudo bash -s -- --version X.Y.Z
```

## Update

`flexypanelctl self-update`, or the Update button in the panel. Updates are verified, the panel database and configuration are backed up first, and a failed update is rolled back automatically. Sites keep running. The update channel is `stable` (tested releases, the default) or `beta` (also pre-releases).

## What a release contains

- `flexypanel_<version>_linux_amd64.tar.gz`: the agent, the CLI and the panel, the systemd units, the version list and the installer (x86_64 only).
- `manifest.json`: the version, its channel, the commit and the SHA-256 of every bundle.
- `manifest.json.sig`: the Ed25519 signature of the manifest.
- `install.sh`: the installer. It downloads a release, checks the signature with the key below, checks the bundle against the manifest, then installs it.

The release key:

```
-----BEGIN PUBLIC KEY-----
MCowBQYDK2VwAyEA8uTKypJvX+l7XZHMz7Y78zf7NamR/ovpUeSsGcQLI1Q=
-----END PUBLIC KEY-----
```

Check a release by hand:

```sh
openssl pkeyutl -verify -pubin -inkey release.pub -rawin -in manifest.json -sigfile manifest.json.sig
sha256sum flexypanel_*_linux_amd64.tar.gz   # compare with manifest.json
```

Tags `vX.Y.Z-alpha.N` and `vX.Y.Z-beta.N` are published as pre-releases (the beta channel); after a full test they are promoted to final releases (the stable channel), as every `vX.Y.Z` tag is.
