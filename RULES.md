# Shadowrocket Rules Guide

This document explains the routing logic used by `lite_optimized.conf`.

## Update URL

Use this URL in Shadowrocket:

```text
https://raw.githubusercontent.com/auboray-cloud/shadowrocket-config/main/lite_optimized.conf
```

The config also contains the same `update-url`, so Shadowrocket can update it later.

## Routing Order

Shadowrocket rules are matched from top to bottom. The first matching rule wins.

Current order:

1. Local network direct
2. Mainland daily apps direct
3. Lightweight ad blocking
4. Important overseas services proxy
5. Manual AI, developer, and privacy-check proxy rules
6. Apple core services direct
7. Mainland China domains and IPs direct
8. Everything else proxy

## 1. Local Network Direct

These are always direct:

- `localhost`
- `.local`
- `.lan`
- `.internal`
- Private IPv4 ranges:
  - `10.0.0.0/8`
  - `100.64.0.0/10`
  - `127.0.0.0/8`
  - `169.254.0.0/16`
  - `172.16.0.0/12`
  - `192.168.0.0/16`
- Local and reserved IPv6 ranges:
  - `::1/128`
  - `fc00::/7`
  - `fe80::/10`

Purpose: LAN devices, routers, NAS, printers, local services, and captive portals should not use the proxy.

## 2. Mainland Daily Apps Direct

These services are explicitly direct before proxy rules.

### Payment

- Alipay
- WeChat Pay
- Tenpay
- UnionPay

Main domains include:

- `alipay.com`
- `alipayobjects.com`
- `tenpay.com`
- `wechatpay.cn`
- `95516.com`
- `unionpay.com`
- `unionpayintl.com`

### Banks

Main covered banks:

- ICBC
- CCB
- ABC
- Bank of China
- Bank of Communications
- CMB
- PSBC
- CIB
- CEB
- CITIC
- SPDB
- Ping An
- WeBank
- MYbank

### Government

Main covered services:

- `gov.cn`
- `12306.cn`
- Tax services
- Medical insurance services
- MIIT
- Public security
- Human resources and social security
- National government service platforms

### Short Video And Content

Main covered services:

- Douyin
- Kuaishou
- Bilibili
- Xiaohongshu
- Xigua

### Shopping And Local Life

Main covered services:

- Taobao
- Tmall
- 1688
- JD
- Pinduoduo
- Suning
- Vipshop
- Meituan
- Dianping
- Ele.me

Purpose: banks, payment, government apps, shopping, and domestic content platforms should avoid proxy-related risk checks, region mismatch, or login verification problems.

## 3. Lightweight Ad Blocking

The config uses:

```conf
RULE-SET,https://raw.githubusercontent.com/auboray-cloud/shadowrocket-config/main/rules/AdvertisingLite.list,REJECT
```

Purpose:

- Block common ads and trackers.
- Keep the list lighter than a full ad list.
- Reduce mobile overhead and false positives.

If an app behaves strangely, this ad rule is one of the first places to check.

## 4. Important Overseas Services Proxy

These rule sets are explicitly proxied:

- Telegram
- OpenAI / ChatGPT
- Apple News
- YouTube
- Netflix
- Disney+

Example:

```conf
RULE-SET,https://raw.githubusercontent.com/auboray-cloud/shadowrocket-config/main/rules/OpenAI.list,PROXY
```

Purpose: these services are usually blocked, region-sensitive, or need overseas routing.

## 5. Manual Proxy Rules

These are manually proxied:

- Anthropic / Claude
- Perplexity
- Poe
- Gemini
- GitHub
- GitLab
- DNS leak and browser leak test sites

Main domains include:

- `anthropic.com`
- `claude.ai`
- `perplexity.ai`
- `poe.com`
- `gemini.google.com`
- `github.com`
- `githubusercontent.com`
- `gitlab.com`
- `dnsleaktest.com`
- `ipleak.net`
- `browserleaks.com`

Purpose: AI tools, developer resources, and privacy-check websites should use the proxy.

## 6. Apple Core Services Direct

The config uses the Apple rule set as direct:

```conf
RULE-SET,https://raw.githubusercontent.com/auboray-cloud/shadowrocket-config/main/rules/Apple.list,DIRECT
```

Purpose:

- App Store
- iCloud
- Apple Push
- Apple system services
- Apple CDN

These usually work best direct in mainland China.

Apple News is handled earlier as proxy, so it will not be swallowed by the Apple direct rule.

## 7. Mainland China Direct

The config uses:

```conf
RULE-SET,https://raw.githubusercontent.com/auboray-cloud/shadowrocket-config/main/rules/ChinaMax.list,DIRECT
DOMAIN-SUFFIX,cn,DIRECT
GEOIP,CN,DIRECT
```

Purpose:

- Mainland domains direct.
- Mainland IP ranges direct.
- Avoid proxying ordinary Chinese websites and apps.

## 8. Final Rule

The final rule is:

```conf
FINAL,PROXY
```

Any traffic that does not match earlier rules uses the proxy.

This means:

- Most unknown overseas sites use proxy.
- Google services generally use proxy.
- New overseas services are more likely to work without adding extra rules.

## DNS

Current DNS:

```conf
dns-server = h3://dns.alidns.com/dns-query, https://doh.pub/dns-query
fallback-dns-server = https://1.1.1.1/dns-query, https://8.8.8.8/dns-query
```

Design:

- Domestic encrypted DNS first for mainland speed and compatibility.
- Overseas encrypted DNS fallback for foreign domains.

## IPv6

Current setting:

```conf
ipv6 = false
```

Recommendation: keep IPv6 disabled unless the proxy node clearly supports IPv6 and leak tests pass.

## MITM

Current setting:

```conf
hostname =
```

MITM is disabled by default.

Recommendation: keep it disabled unless you specifically need HTTPS rewrite, script filtering, or debugging.

## Quick Test Checklist

After importing or updating the config, test:

- Baidu or Taobao: should be direct.
- Alipay or bank app: should be direct.
- Government service or 12306: should be direct.
- YouTube: should use proxy.
- ChatGPT: should use proxy.
- Telegram: should use proxy.
- App Store or iCloud: usually should be direct.
- `ipleak.net`: should show the expected proxy IP when visited through proxy.

## Troubleshooting

If a domestic app has login, payment, or verification issues:

1. Check recent requests in Shadowrocket.
2. If the app domain is going through `PROXY`, add it to the direct section.
3. If the app is blocked by `REJECT`, check the `AdvertisingLite` rule.

If an overseas app does not work:

1. Check whether it is direct because of `ChinaMax`.
2. Add a manual `DOMAIN-SUFFIX,...,PROXY` rule above Apple and China direct rules.
3. Verify the selected proxy node is working.

If DNS or location looks wrong:

1. Test with `ipleak.net`.
2. Try another proxy node.
3. Keep IPv6 disabled unless you know the node supports it.

## Maintenance

The main config is maintained here:

```text
https://github.com/auboray-cloud/shadowrocket-config
```

Rule sets are mirrored into this repository under `rules/`, with upstream source noted in `MIRROR.md`.

To update:

1. Edit `lite_optimized.conf`.
2. Push the change to GitHub.
3. In Shadowrocket, update the config.
