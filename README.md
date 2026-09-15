# Magento 2 Shield (IPS/WAF) — utrzymywany fork (SISL)

**System wykrywania i zapobiegania włamaniom (IPS/WAF)** dla Magento 2. Analizuje przychodzące
żądania (parametry GET/POST/COOKIE) pod kątem znanych wzorców ataków — **SQL injection**, **XSS**,
próby manipulacji — i potrafi je zablokować, zanim trafią do aplikacji. Wykorzystuje parser SQL
(`phpmyadmin/sql-parser`) do realnej analizy potencjalnych zapytań, nie tylko proste regexy.

Część **MageSpecialist Security Suite**. To utrzymywany fork porzuconego `msp/shield` (ostatnie
wydanie 2017, `php ^7.1` — **nie wchodzi na żadne PHP 8.x**). Fork rozluźnia zależności, aktualizuje
komendę CLI do Symfony Console 7 (z 2.4.9) i jest zweryfikowany na **Magento 2.4.9 / PHP 8.4**
(di:compile + realny test: payload SQLi i XSS wykryte, czysty payload przepuszczony).

## Zgodność
- Magento **2.4.4 – 2.4.9** (Open Source / Adobe Commerce)
- PHP **8.1 – 8.4**
- Wymaga `msp/security-suite-common` (nasz fork) + `phpmyadmin/sql-parser`

## Instalacja

```bash
composer require sisl-source/magento2-shield
bin/magento module:enable MSP_SecuritySuiteCommon MSP_Shield
bin/magento setup:upgrade
bin/magento setup:di:compile   # tryb produkcyjny
```

## Test z linii poleceń
Sprawdź, czy silnik wykrywa zagrożenie w danym parametrze:
```bash
# Wykryje SQL injection:
bin/magento msp:shield:test GET id "1 UNION SELECT username,password FROM admin_user--"
# Wykryje XSS:
bin/magento msp:shield:test GET s "<script>alert(document.cookie)</script>"
# Czysty payload -> brak zagrożeń (pusty wynik):
bin/magento msp:shield:test GET q "zwykle zapytanie"
```

## Konfiguracja
**Sklep → Konfiguracja → MSP Security Suite → Shield** — tryb działania (log / block), progi,
reguły. Zdarzenia trafiają do logu Security Suite. Zalecane wdrożenie: najpierw tryb log
(obserwacja fałszywych alarmów na Twoim ruchu), potem block.

## Uwaga
WAF na poziomie aplikacji to **warstwa uzupełniająca**, nie zamiennik łatania Magento, silnego
hasła i [ograniczenia dostępu do panelu po IP](https://github.com/SISL-source/magento2-admin-restriction).
Traktuj go jako element obrony w głąb.

## Licencja
OSL-3.0 (jak oryginał). Fork utrzymywany przez [SISL](https://sisl.pl).
