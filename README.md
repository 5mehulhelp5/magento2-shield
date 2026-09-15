# Magento 2 Shield (IPS/WAF) — maintained fork (SISL)

An **intrusion detection and prevention system (IPS/WAF)** for Magento 2. It analyses incoming
requests (GET/POST/COOKIE parameters) for known attack patterns — **SQL injection**, **XSS**,
tampering attempts — and can block them before they reach the application. It uses an SQL parser
(`phpmyadmin/sql-parser`) for real analysis of potential queries, not just simple regexes.

Part of the **MageSpecialist Security Suite**. This is a maintained fork of the abandoned
`msp/shield` (last release 2017, `php ^7.1` — **does not run on any PHP 8.x**). The fork loosens the
dependencies, updates the CLI command to Symfony Console 7 (shipped with 2.4.9) and is verified on
**Magento 2.4.9 / PHP 8.4** (di:compile + a real test: SQLi and XSS payloads detected, a clean
payload let through).

## Compatibility
- Magento **2.4.4 – 2.4.9** (Open Source / Adobe Commerce)
- PHP **8.1 – 8.4**
- Requires `sisl-source/magento2-security-suite-common` (our fork) + `phpmyadmin/sql-parser`

## Installation

```bash
composer require sisl-source/magento2-shield
bin/magento module:enable MSP_SecuritySuiteCommon MSP_Shield
bin/magento setup:upgrade
bin/magento setup:di:compile   # production mode
```

## Command-line test
Check whether the engine detects a threat in a given parameter:
```bash
# Detects SQL injection:
bin/magento msp:shield:test GET id "1 UNION SELECT username,password FROM admin_user--"
# Detects XSS:
bin/magento msp:shield:test GET s "<script>alert(document.cookie)</script>"
# Clean payload -> no threats (empty result):
bin/magento msp:shield:test GET q "an ordinary query"
```

## Configuration
**Stores → Configuration → MSP Security Suite → Shield** — operating mode (log / block), thresholds,
rules. Events go to the Security Suite log. Recommended rollout: start in log mode (watch for false
positives on your traffic), then switch to block.

## Note
An application-level WAF is a **complementary layer**, not a replacement for patching Magento, a
strong password and [admin panel IP
restriction](https://github.com/SISL-source/magento2-admin-restriction). Treat it as part of
defence in depth.

## License
OSL-3.0 (same as upstream). Fork maintained by [SISL](https://sisl.pl).
