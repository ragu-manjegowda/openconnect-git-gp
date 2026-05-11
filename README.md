# openconnect-git-gp

Arch `makepkg` project for a patched upstream OpenConnect build with GlobalProtect CAS/default-browser support.

See [`.SRCINFO`](.SRCINFO) for the pinned upstream OpenConnect source and package metadata. This package applies `gp-cas-callback.patch` on top of that source.

## What This Adds

- GlobalProtect-specific external browser callback handling.
- `globalprotectcallback:` helper installation via `gp_browser_helper`.
- CAS callback parsing for GlobalProtect default-browser SAML flow.
- Gateway CAS login support used by GlobalProtect gateways.

The package does not hardcode a browser. Pass the browser at runtime with OpenConnect, for example:

```bash
openconnect --protocol=gp --external-browser /usr/bin/firefox ...
```

## Build And Install

```bash
makepkg -si
```

To rebuild without installing:

```bash
makepkg -sf
```

## Files

- `PKGBUILD`: Arch package build definition.
- `gp-cas-callback.patch`: Patch applied to upstream OpenConnect.
- `gp_browser_helper.c`: Callback helper for the `globalprotectcallback:` URI scheme.
- `gp_browser_helper.desktop`: Desktop MIME handler registration.
