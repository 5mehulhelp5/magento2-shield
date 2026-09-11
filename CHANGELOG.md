# Changelog

## [Unreleased] — fork SISL (2026-09)
- Zgodność z Magento **2.4.9 / PHP 8.4** (ostatni publiczny `msp/shield` 2.1.0 z 2017 pinuje `php ^7.1` = blok całego 8.x).
- `composer.json`: `php ~8.1.0 || … || ~8.5.0`, `magento/framework >=103.0.4 <104`, `phpmyadmin/sql-parser ^5.0 || ^4.1`.
- `Command/Test.php`: dodane `execute(): int` + `Cli::RETURN_SUCCESS` (Symfony Console 7).
- Zweryfikowane: wykrywanie SQLi i XSS, czysty payload przepuszczony.

## Oryginał (msp/shield) — MageSpecialist, porzucone 2017.
