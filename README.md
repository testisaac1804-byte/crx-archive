# School-blocked Chrome extensions — 129 folders + 129 bundles

A school `ExtensionSettings` policy removed 131 extensions by ID. An extension ID is also the token a
browser sends when it asks Google for an update — so the blocklist doubles as a list of download
addresses. **129 of the 131 still resolve.** These are those 129, published two ways.

- **[Download the release →](https://github.com/testisaac1804-byte/crx-archive/releases/tag/v1.0)**

## Folders (start here)

`<Name>__<id>.zip` — each zip's single top-level entry is the extension's own directory.

```
unzip 2048__ijkmjnaahlnmdjjlbhbjbhlnmadmmlgg.zip
# -> 2048__ijkmjnaahlnmdjjlbjbhlnmadmmlgg/manifest.json, assets/, _locales/ ...
```

Then: `chrome://extensions` → **Developer mode** → **Load unpacked** → pick that directory.

No signature warning, no "damaged file" dialog. 129 folders · 28,887 files · 1.46 GiB zipped ·
2.18 GiB unpacked.

## Bundles

`<Name>__<id>.crx` — the original signed CRX3 package from Google's update service, byte-for-byte,
never re-signed or repacked. Drag onto `chrome://extensions` with Developer mode on.

## What is not here

Two IDs answer **HTTP 204, zero bytes** on every Chrome version tried — they were delisted from the
Web Store and no mirror has a copy:

| Extension | ID |
|---|---|
| Gauth – AI Homework & Study Helper | `aahodnigalncogkhfolddkmkklijeoap` |
| ZenMate Free VPN – Best VPN for Chrome | `fdcgdnkidjaadafnichfpabhfomcebme` |

129 of 131, not 131.

## Files

| file | contents |
|---|---|
| `downloads.json` | 129 entries: id → folder zip → crx → GitHub `sha256:` digests → tree hash |
| `crx-catalogue.csv` | 131 rows: category, name, ID, version, size, files, hashes, both URLs |
| `crx-manifest.json` | 129 full SHA-256 + byte length; the 2 delisted marked `status: delisted` |

`tree_sha256` is SHA-256 over the sorted `relpath\0filehash\n` lines of the unpacked folder — a single
hash for a directory, so an unpacked copy can be proven identical to what was published.

## Where this came from

The school profile's cached cloud policy (`ExtensionSettings`, `installation_mode: removed`). Each ID
was requested from the Web Store CRX update service as macOS arm64 / Chrome 126, validated by magic
number, CRX3 header length and a full ZIP CRC sweep. Nothing here bypasses the policy on a managed
machine — that profile blocks side-loading too. Use a personal profile.
