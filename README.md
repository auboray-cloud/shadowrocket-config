# Shadowrocket Config

Personal Shadowrocket rule configuration for mainland China networks.

## Files

- `Auboray_SmartLink.conf`: main Shadowrocket configuration.
- `rules/`: mirrored rule-set files used by `Auboray_SmartLink.conf`.
- `MIRROR.md`: rule mirror source and refresh notes.
- `RULES.md`: routing rule guide and troubleshooting notes.
- `规则说明.md`: 中文规则说明和排查指南。

## Update URL

Use this URL in Shadowrocket:

```text
https://raw.githubusercontent.com/auboray-cloud/shadowrocket-config/main/Auboray_SmartLink.conf
```

The same URL is written into the profile as:

```conf
update-url = https://raw.githubusercontent.com/auboray-cloud/shadowrocket-config/main/Auboray_SmartLink.conf
```

## Notes

- This config does not include proxy nodes.
- Rule sets are mirrored into this repository, so the config no longer directly depends on upstream raw rule URLs.
- Only explicit mainland daily-app allowlists are routed direct.
- There is no broad `.cn`, China IP, or `GEOIP,CN` direct fallback.
- Unknown traffic falls back to the main proxy.
- International AI services use the Singapore `AI-SG` policy group.
- MITM is disabled by default.
