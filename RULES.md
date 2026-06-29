# Shadowrocket Rules Guide

This document explains the current routing logic used by `Auboray_SmartLink.conf`.

## Update URL

Use this URL in Shadowrocket:

```text
https://raw.githubusercontent.com/auboray-cloud/shadowrocket-config/main/Auboray_SmartLink.conf
```

The same URL is also written into `update-url`, so the profile can update itself later.

## Current Design

Rules are matched from top to bottom. The current design is intentionally simple:

1. Local network, NAS, `*.auboray.com`, `*.auboray.org`, and Synology QuickConnect direct
2. Mainland daily app allowlist direct
3. AI services through `AI-SG`
4. Telegram, Google/YouTube, developer sites, and privacy checks through the main proxy
5. Apple core services direct
6. Everything else through the main proxy

There is no broad `.cn`, China IP, or `GEOIP,CN` direct fallback. Unknown traffic should use the main proxy.

## Local Direct

Always direct:

- `localhost`
- `.local`
- `.lan`
- `.internal`
- `.home`
- `.home.arpa`
- `.localdomain`
- `.localnet`
- `auboray.com`
- `*.auboray.com`
- `auboray.org`
- `*.auboray.org`
- `quickconnect.to`
- `quickconnect.cn`
- Private IPv4 ranges such as `10.0.0.0/8`, `172.16.0.0/12`, and `192.168.0.0/16`
- Local IPv6 ranges such as `::1/128`, `fc00::/7`, and `fe80::/10`

Purpose: routers, NAS, printers, Home Assistant, local devices, and local services should never go through proxy.

## Mainland Daily App Allowlist

The profile uses:

```conf
RULE-SET,https://raw.githubusercontent.com/auboray-cloud/shadowrocket-config/main/rules/ChinaDaily.list,DIRECT
```

Covered categories:

- Payments and messaging: Alipay, WeChat Pay, QQ, UnionPay
- Banks: ICBC, CCB, ABC, BOC, CMB, PSBC, CIB, CEB, CITIC, SPDB, Ping An, WeBank, MYbank
- Government and public services: `gov.cn`, `12306.cn`, tax, healthcare, MIIT, public security, social security
- Video and social apps: Douyin, Kuaishou, Bilibili, Xiaohongshu, Xigua
- Shopping and local life: Taobao, Tmall, 1688, JD, Pinduoduo, Suning, Vipshop, Meituan, Dianping, Ele.me

Purpose: only trusted daily mainland services are direct. This keeps payment, banking, government, shopping, and short-video apps fast and less likely to trigger risk checks.

## AI Routing

International AI services use the `AI-SG` policy group:

```conf
AI-SG = select,policy-regex-filter=新加坡,url=http://www.gstatic.com/generate_204,interval=600,timeout=5,select=0
RULE-SET,https://raw.githubusercontent.com/auboray-cloud/shadowrocket-config/main/rules/AI.list,AI-SG
```

Covered examples:

- ChatGPT / OpenAI
- Claude / Anthropic
- Gemini
- Grok / xAI
- Perplexity
- Poe
- GitHub Copilot
- Mistral, Meta AI, Character.AI, Hugging Face, Groq, OpenRouter, Midjourney

OpenAI supplemental dependencies are merged into `AI.list`, so OpenAI and ChatGPT traffic use `AI-SG` consistently.

Domestic AI services are not added to `AI.list`; they should use the normal mainland daily/direct path only when explicitly allowlisted.

## Main Proxy Fallback

The final rule is:

```conf
FINAL,PROXY
```

This means all unmatched traffic uses the main proxy selected in Shadowrocket. If you want Futu, Longbridge, Google, YouTube, X, GitHub, and other overseas or unknown services to use Hong Kong, select a stable Hong Kong node as the main proxy.

## DNS

Current DNS:

```conf
dns-server = system, h3://dns.alidns.com/dns-query, https://doh.pub/dns-query
fallback-dns-server = https://1.1.1.1/dns-query, https://8.8.8.8/dns-query
always-real-ip = auboray.com, *.auboray.com
dns-direct-system = true
```

Design:

- System DNS stays available for local and home-network names.
- Domestic encrypted DNS helps mainland services.
- Overseas DNS is fallback.
- `auboray.com` and `*.auboray.com` use real DNS results for IPv6 direct access.

## IPv6

Current setting:

```conf
ipv6 = true
```

Reason: the home NAS domain uses IPv6 direct access. Keep IPv6 leak tests in mind when changing nodes or DNS settings.

## MITM

MITM is disabled:

```conf
hostname =
```

Keep it disabled unless HTTPS rewrite, script filtering, or debugging is intentionally needed.

## Troubleshooting

If a mainland daily app is slow or cannot log in:

1. Check Shadowrocket recent requests.
2. If the app is going through `PROXY`, add its domain to `ChinaDaily.list`.
3. If a domain is missing, add it to `ChinaDaily.list` only when it should clearly stay direct.

If an overseas service does not work:

1. Check whether the selected main proxy node works.
2. For AI services, confirm the request matches `AI-SG`.
3. Add a missing AI domain to `AI.list` only when the service should use Singapore.

If Futu or Longbridge is slow:

1. Select a stable Hong Kong node as the main proxy.
2. Keep the profile in config mode.
3. Check that the request falls through to `FINAL,PROXY` instead of direct.

If local NAS or LAN access fails:

1. Check whether the hostname is under `*.auboray.com`, `.local`, `.lan`, `.home`, or another local suffix.
2. Make sure Chrome Secure DNS is disabled if Chrome bypasses system DNS.
3. Verify IPv6 is available on the local network.
