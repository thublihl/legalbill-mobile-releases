# LegalBill Mobile — Release / Update channel

Public update feed for **LegalBill Mobile** (Android APK + iOS PWA). The app checks
`updates.json` here on launch and offers any newer version to the user.
Source code lives in the private repo `thublihl/legalbill-mobile`.

## How the app uses this repo

On startup LegalBill Mobile fetches:

```
https://raw.githubusercontent.com/thublihl/legalbill-mobile-releases/main/updates.json
```

It shows an "Install update" banner for every entry whose `version` is greater than
the installed version and which the user's licence allows.

## Publishing a new version

1. Build the APK: `cd legalbill-mobile/android-build && ./build.sh` → `out/LegalBill.apk`.
2. Create a GitHub Release here tagged `vX.Y.Z` and attach `LegalBill.apk`:
   ```
   gh release create vX.Y.Z out/LegalBill.apk -R thublihl/legalbill-mobile-releases \
     --title "LegalBill Mobile vX.Y.Z" --notes "What changed"
   ```
3. Add an entry to `updates.json` (newest first) and push:
   ```json
   {
     "version": "X.Y.Z",
     "type": "free",
     "title": "Short description of the update",
     "url": "https://github.com/thublihl/legalbill-mobile-releases/releases/download/vX.Y.Z/LegalBill.apk"
   }
   ```
   - `type`: `free` (everyone), `paid` (valid licence), or `upgrade` (licence tier ≥ `minTier`).
   - Bump `latest` to the new version.

## updates.json schema

| field | meaning |
|-------|---------|
| `channel` | release channel id (`stable-mobile`) |
| `latest`  | newest published version string |
| `updates[]` | list of `{version, type, title, url[, minTier]}` offered to clients |
