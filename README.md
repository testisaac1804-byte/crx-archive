# School-blocked Chrome extensions — 129 packed `.crx` bundles

A school `ExtensionSettings` policy removed 131 extensions by ID. An extension ID is also the token a
browser sends when it asks Google for an update, so most of those "removed" extensions are still
downloadable binaries. **129 of the 131 were recovered** from Google's own CRX update service and are
attached to this release, unmodified, signature intact.

The other two — **Gauth** (`aahodnigalncogkhfolddkmkklijeoap`) and **ZenMate Free VPN**
(`fdcgdnkidjaadafnichfpabhfomcebme`) — answer `HTTP 204` with zero bytes on every Chrome version and
404 on every mirror. They have been unpublished. They are not in this archive.

Full write-up: https://isaac1804.com/portfolio/analysis/chrome-crx-archive/report.html

## Files

| Asset | What it is |
|---|---|
| `*.crx` | 129 packed extensions, `Name__<extension-id>.crx` |
| `crx-manifest.json` | SHA-256 of every `.crx` above, plus the 2 delisted IDs marked `status: delisted` |
| `crx-catalogue.csv` | 131 rows — category, name, ID, version, size, file count, status |
| `downloads.json` | machine-readable `id → asset name → URL` index |

1.46 GiB total. Largest: Friday Night Funkin' (159.5 MB), MEOWSING (135.9 MB), Extreme Car Traffic (69 MB).

## Installing one

`.crx` side-loading is blocked on a managed profile (that is the same policy that removed them), so:

1. Open `chrome://extensions` in a **personal** Chrome profile.
2. Turn on **Developer mode**.
3. Drag the `.crx` onto the page. Chrome will ask to confirm — accept.

Or use the unpacked route below, then **Load unpacked**.

## Unpacking one

A `.crx` is a ZIP with a signature header glued to the front; `unzip` skips it and warns. That warning
is expected:

```sh
unzip -q "2048__ijkmjnaahlnmdjjlbhbjbhlnmadmmlgg.crx" -d 2048
# warning: 1061 extra bytes at beginning or within zipfile  ← the CRX3 header, ignore it
```

Anything that reads ZIPs works (7-Zip, Keka, macOS Archive Utility, Python's `zipfile`). Picking a
sub-directory first is fine too — the extension's `manifest.json` is at the root of the payload.

To verify a download is byte-identical to what was captured:

```sh
shasum -a 256 "2048__ijkmjnaa....crx"   # compare against crx-manifest.json
```

## Fetching many at once

```sh
curl -sL https://raw.githubusercontent.com/testisaac1804-byte/crx-archive/main/downloads.json \
  | python3 -c "import json,sys;[print(a['url']) for a in json.load(sys.stdin)['assets']]" \
  | xargs -n1 -P4 curl -sLOJ
```

## Provenance

- Fetched 30 Oct 2026 through the Web Store update service, as a macOS arm64 Chrome 126 client:
  `clients2.google.com/service/update2/crx?response=redirect&prodversion=126.0.0.0&acceptformat=crx2,crx3&x=id%3D<ID>%26uc`
- Every file validated by CRX3 magic bytes (`Cr24`), header length, and a full ZIP CRC sweep over the
  stripped payload: **129/129 pass, 0 corrupt**.
- Versions parsed from the redirect filename and cross-checked against the `manifest.json` inside each
  archive. All 129 are Manifest V3.
- Nothing here was altered, re-signed, repacked or renamed inside the archive. Only the file names are
  normalised (spaces → `_`, one `é` → `e`).

## What this is not

Not a bypass. Nothing here installs on a machine the policy manages, and no account, session or policy
was touched to obtain these. They are the same public downloads any browser makes when it checks for an
extension update.
