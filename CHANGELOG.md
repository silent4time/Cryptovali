# Changelog

All notable changes to this project are documented here.
The format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/), and this project adheres to [Semantic Versioning](https://semver.org/).

## [1.1.0] - 2026-08-29

### Added
- Dedicated "Cryptovali" item in the WordPress admin sidebar menu, linking directly to the gateway's settings page.

### Changed
- Gateway display name in WooCommerce → Settings → Payments changed to "درگاه پرداخت ارز دیجیتال Cryptovali" (was: "پرداخت ارز دیجیتال (BEP20 / TRC20)").

## [1.0.0] - 2026-08-29

### Added
- Initial release of Cryptovali.
- WooCommerce payment gateway supporting BNB (BEP20), USDT (BEP20), and USDT (TRC20).
- Admin-configurable wallet addresses for BEP20 and TRC20 networks.
- Live exchange rate lookup via CoinGecko with per-order unique amount nonce.
- Fully automatic deposit detection via BscScan (BEP20) and TronGrid (TRC20) APIs, polled every minute via WP-Cron.
- Customer-facing payment page with QR code, copy buttons, countdown timer, and live status polling.
- Automatic order cancellation on payment expiry.
- WooCommerce HPOS (High-Performance Order Storage) compatibility declaration.
