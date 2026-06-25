# Mirrored Rule Sets

The files in `rules/` are local mirrors of the rule sets used by `lite_optimized.conf`.

Original upstream:

```text
https://github.com/blackmatrix7/ios_rule_script
```

Mirrored files:

- `AdvertisingLite.list`
- `Telegram.list`
- `OpenAI.list`
- `AppleNews.list`
- `YouTube.list`
- `Netflix.list`
- `Disney.list`
- `Apple.list`

Purpose:

- Keep Shadowrocket usable even if the upstream repository is temporarily unavailable.
- Make the config depend on `auboray-cloud/shadowrocket-config` instead of directly depending on upstream raw URLs.

This mirror is refreshed weekly by GitHub Actions. If upstream disappears later, the latest mirrored files remain in this repository.
