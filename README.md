# Shadowrocket Config

Personal Shadowrocket rule configuration for mainland China networks.

## Files

- `lite_optimized.conf`: main Shadowrocket configuration.
- `rules/`: mirrored rule-set files used by `lite_optimized.conf`.
- `MIRROR.md`: rule mirror source and refresh notes.
- `RULES.md`: routing rule guide and troubleshooting notes.
- `规则说明.md`: 中文规则说明和排查指南。

## Update URL

Use this URL in Shadowrocket:

```text
https://raw.githubusercontent.com/auboray-cloud/shadowrocket-config/main/lite_optimized.conf
```

The same URL is written into `lite_optimized.conf` as:

```conf
update-url = https://raw.githubusercontent.com/auboray-cloud/shadowrocket-config/main/lite_optimized.conf
```

## Notes

- This config does not include proxy nodes.
- Rule sets are mirrored into this repository, so the config no longer directly depends on upstream raw rule URLs.
- China mainland domains and IP ranges are routed direct.
- Unknown overseas traffic falls back to proxy.
- MITM is disabled by default.
