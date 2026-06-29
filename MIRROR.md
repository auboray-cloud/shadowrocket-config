# Mirrored Rule Sets

Only `Telegram.list` is mirrored from upstream. Other files in `rules/` are curated for `Auboray_SmartLink.conf`.

Original upstream:

```text
https://github.com/blackmatrix7/ios_rule_script
```

Mirrored upstream files:

- `Telegram.list`

Purpose:

- Keep Shadowrocket usable even if the upstream repository is temporarily unavailable.
- Make the config depend on `auboray-cloud/shadowrocket-config` instead of directly depending on upstream raw URLs.

This mirror is refreshed weekly by GitHub Actions. If upstream disappears later, the latest mirrored file remains in this repository.
