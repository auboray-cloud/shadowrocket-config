# Shadowrocket Config

Personal Shadowrocket rule configuration for mainland China networks.

## Files

- `lite_optimized.conf`: main Shadowrocket configuration.
- `RULES.md`: routing rule guide and troubleshooting notes.

## Update URL

After this repository is published to GitHub, use this URL in Shadowrocket:

```text
https://raw.githubusercontent.com/auboray-cloud/shadowrocket-config/main/lite_optimized.conf
```

The same URL should be written into `lite_optimized.conf` as:

```conf
update-url = https://raw.githubusercontent.com/auboray-cloud/shadowrocket-config/main/lite_optimized.conf
```

## Notes

- This config does not include proxy nodes.
- China mainland domains and IP ranges are routed direct.
- Unknown overseas traffic falls back to proxy.
- MITM is disabled by default.
