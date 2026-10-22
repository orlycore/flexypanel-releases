# Flexypanel releases

Signed releases of [Flexypanel](https://github.com/orlycore/flexypanel), a web hosting control panel for one administrator: every site runs in its own Docker containers behind one shared nginx.

## Install

On a fresh AlmaLinux 10 or Ubuntu 26.04 LTS server, as root:

```sh
curl -fsSL https://github.com/orlycore/flexypanel-releases/releases/latest/download/install.sh | sudo bash
```

Options go after `-s --`, for example a specific release or the panel's hostname:

```sh
curl -fsSL https://github.com/orlycore/flexypanel-releases/releases/latest/download/install.sh | sudo bash -s -- --version 1.0.0 --hostname panel.example.com
```

## Update

`flexypanelctl self-update`, or the Update button in the panel. Updates are verified, the panel database and configuration are backed up first, and a failed update is rolled back automatically. Sites keep running.

## What a release contains

- `flexypanel_<version>_linux_<amd64|arm64>.tar.gz`: the agent, the CLI and the panel, the systemd units, the version list and the installer.
- `manifest.json`: the version, the SHA-256 of every bundle and the panel image (`ghcr.io/orlycore/flexypanel`) pinned by digest.
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

Tags `vX.Y.Z` are the stable channel; `vX.Y.Z-beta.N` (prereleases) are the beta channel.
