# Changelog

All notable changes to this project are documented here.
The format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/), and this project adheres to [Semantic Versioning](https://semver.org/).

## [1.5.6] - 2026-08-29

### Fixed
- Removed the nonce requirement from the three read-only diagnostic actions ("تست زنده‌ی اتصال به گیت‌هاب", "تست اتصال به سرویس‌های ضروری", "بررسی بروزرسانی جدید") — they only display information or trigger a re-check, they never change any setting, so they no longer depend on WordPress's nonce system. This fixes a "The link you followed has expired" error some sites showed for these buttons (typically caused by unstable secret keys in `wp-config.php` or a caching/security plugin interfering with nonce validation) while keeping full nonce protection on the actual settings-save form, which does modify data.

## [1.5.5] - 2026-08-29

### Added
- New "تست اتصال به سرویس‌های حیاتی افزونه" (Test connection to the plugin's critical services) panel on the settings dashboard: checks live connectivity to CoinGecko, BscScan, TronGrid (the actual payment-detection engine), in addition to GitHub/jsDelivr (update-only). Flags clearly if a critical service is unreachable, since that means automatic payment detection may currently be broken — a more urgent issue than the updater itself.

### Fixed
- `Plugin URI` in the plugin header updated from the placeholder `https://github.com/YOUR-USERNAME/cryptovali` to the real repository address `https://github.com/silent4time/Cryptovali` — this is what "دیدن خانهٔ افزونه" (Visit plugin site) on the Plugins page links to.
- Added `Author URI` pointing to the same GitHub repository, so "بدست Cryptovali" (By Cryptovali) on the Plugins page is now a working link.

## [1.5.4] - 2026-08-29

### Changed
- Plugin display name (shown on the Plugins page) changed from "Cryptovali - Crypto Payment Gateway for WooCommerce" to "CryptoVali | درگاه پرداخت ارز دیجیتال".

## [1.5.3] - 2026-08-29

### Changed
- The installable plugin zip is now always named `cryptovali.zip` — no version number in the filename. This lets the repository owner simply overwrite the same file on every release instead of accumulating a new versioned zip (`cryptovali-vX.Y.Z.zip`) each time.
- The jsDelivr fallback now looks for this fixed filename (`cryptovali.zip`) at each release tag, instead of guessing a versioned filename pattern.

## [1.5.2] - 2026-08-29

### Added
- Automatic fallback to jsDelivr (a free public CDN that mirrors GitHub repository content) when direct access to `api.github.com` fails from the hosting server's network — a common issue on hosts subject to firewalling or network filtering. The update checker now tries GitHub first, then transparently falls back to jsDelivr for both version detection and package download.
- The live diagnostic panel now reports which source succeeded (GitHub or jsDelivr), and shows both error messages if neither is reachable.

### Fixed
- Release/version tags are now validated against a strict `vX.Y.Z` pattern before being considered, preventing a misnamed release title or tag from being misread as a version number.

## [1.5.1] - 2026-08-29

### Fixed
- The GitHub updater now resolves the plugin's basename dynamically (via `plugin_basename(__FILE__)`) instead of assuming a hardcoded `cryptovali/cryptovali.php` path, making update detection reliable regardless of the actual installed folder name.

### Added
- New live diagnostic panel on the Cryptovali settings dashboard ("تست زنده‌ی اتصال به گیت‌هاب"): performs an uncached call to the GitHub Releases API and displays the exact result (success/failure, detected version, HTTP error code, or connection error) directly on the page, to make troubleshooting update-detection issues straightforward without needing server access.

## [1.5.0] - 2026-08-29

### Added
- Native **TRX** (Tron) support as a fourth payment option, alongside BNB-BEP20, USDT-BEP20, and USDT-TRC20.
- New independent "TRX wallet address" field with its own "show to customer" toggle, following the same pattern as the other three networks.
- Automatic detection of incoming TRX transfers via TronGrid's native transactions endpoint (separate from the existing TRC20 token-transfer detection used for USDT).
- TRX price feed added to the CoinGecko exchange-rate lookup.

## [1.4.0] - 2026-08-29

### Added
- Automatic update checker: the plugin now polls GitHub Releases (`silent4time/Cryptovali`) and shows a native WordPress "update available" notice when a newer version is published, with one-click update support.
- "View version details" popup on the Plugins page, populated from the GitHub release notes.
- "بررسی بروزرسانی جدید" (Check for new update) link under the plugin entry on the Plugins page, for forcing an immediate re-check.
- Automatic folder-name correction so updates installed from GitHub's auto-generated source zip land in the correct `cryptovali/` directory.

## [1.3.0] - 2026-08-29

### Added
- Dedicated "Cryptovali" admin dashboard with its own real settings page (no longer a redirect into WooCommerce → Payments).
- New "تراکنش‌ها" (Transactions) admin page: a table listing order number, date, product(s), amount, network/currency, and payment status (confirmed / pending / expired) for every Cryptovali order.
- Contextual "؟" help icon next to every settings field; clicking it opens a popup with a plain-Persian explanation of how to fill that field.
- Centralized field definitions (`CP_Fields`) shared between the WooCommerce settings screen and the new dashboard, so the two never drift out of sync.

## [1.2.0] - 2026-08-29

### Changed
- Wallet address settings split into three independent fields: BNB (BEP20), USDT (BEP20), and USDT (TRC20) — previously BNB and USDT-BEP20 shared one address field.
- Each network now has its own "show to customer" toggle, independent of the others, so any combination of payment options can be enabled or hidden at checkout regardless of whether an address is configured.

### Migration note
- Existing installs upgrading from 1.1.x or earlier must re-enter wallet addresses: the old shared "BEP20 address" field is replaced by separate "BNB (BEP20)" and "USDT (BEP20)" address fields.

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
