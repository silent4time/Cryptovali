# Cryptovali

[![Version](https://img.shields.io/badge/version-1.1.0-blue.svg)](CHANGELOG.md)
[![WordPress](https://img.shields.io/badge/WordPress-6.0%2B-21759b.svg)](https://wordpress.org)
[![WooCommerce](https://img.shields.io/badge/WooCommerce-8.0%2B-96588a.svg)](https://woocommerce.com)
[![License](https://img.shields.io/badge/license-GPL--2.0%2B-green.svg)](LICENSE)

A self-hosted, direct-to-wallet cryptocurrency payment gateway for WooCommerce. No third-party processor, no license server, no fees beyond the network's own gas fee.

Persian documentation: [README-fa.md](README-fa.md)

## Features

- **Networks supported:** BNB (BEP20), USDT (BEP20), USDT (TRC20)
- **Fully automatic payment detection** — no manual TxHash entry required
- **Free data sources only** — BscScan API (free tier), TronGrid (public), CoinGecko (public) for live exchange rates
- Wallet addresses configurable from the WordPress admin panel (no code editing needed)
- Amount-based order matching via a small unique decimal nonce, so multiple orders can share the same static wallet address
- Countdown timer + automatic order cancellation on expiry
- WooCommerce HPOS (High-Performance Order Storage) compatible

## Requirements

- WordPress 6.0+
- WooCommerce 8.0+
- A free [BscScan](https://bscscan.com/register) API key (for BNB / USDT-BEP20 detection)
- (Optional) A free [TronGrid](https://www.trongrid.io/) API key for faster TRC20 lookups

## Installation

1. Download the latest release from the [Releases](../../releases) page, or clone this repo.
2. Zip the `cryptovali` folder if installing manually via the WP admin.
3. WordPress Admin → Plugins → Add New → Upload Plugin → select the zip → Install → Activate.

## Configuration

WooCommerce → Settings → Payments → **Cryptovali**

1. Enable the gateway.
2. Enter your BEP20 wallet address (for BNB and USDT-BEP20).
3. Enter your TRC20 wallet address (for USDT-TRC20).
4. Add your BscScan API key.
5. (Optional) Add your TronGrid API key.
6. Adjust payment expiry time and amount tolerance if needed.

## How it works

1. Customer selects a network/currency at checkout.
2. The plugin fetches a live rate from CoinGecko and computes the exact crypto amount, appending a small unique decimal nonce so orders remain distinguishable even on a single static address.
3. Customer sees the address, amount, and a QR code, and pays from their own wallet.
4. A WP-Cron job runs every minute, checks BscScan/TronGrid for new incoming transfers to your address, and matches them against pending orders.
5. On a match, the order is marked as paid automatically and the customer's payment page redirects to the thank-you page.
6. Unpaid orders are automatically cancelled after the configured expiry window.

## Project structure

```
cryptovali/
├── cryptovali.php              # Plugin bootstrap, gateway registration, cron scheduling
├── includes/
│   ├── class-cp-gateway.php    # WC_Payment_Gateway implementation, admin settings, checkout fields
│   ├── class-cp-rate.php       # CoinGecko exchange rate + nonce-based amount calculation
│   ├── class-cp-chain-api.php  # BscScan / TronGrid API calls
│   ├── class-cp-cron.php       # Polls chain APIs, matches transfers to pending orders
│   └── class-cp-ajax.php       # Frontend polling endpoint for order status
├── templates/
│   └── receipt-page.php        # Customer-facing payment page (address, amount, QR, countdown)
└── assets/
    ├── css/cp-style.css
    └── js/cp-checkout.js        # QR rendering, countdown, status polling
```

## Roadmap

- [ ] Additional networks (Polygon, Ethereum)
- [ ] Per-order HD wallet address generation instead of a shared static address
- [ ] Webhook-based detection as an alternative to polling

## Contributing

Issues and pull requests are welcome. Please update `CHANGELOG.md` with any user-facing change.

## Disclaimer

This is a standard, transparent blockchain payment gateway. It does not implement, and will not implement, any identity-obfuscation or sanctions/regulation-evasion mechanism. Wallet address accuracy is the site owner's responsibility — the plugin cannot recover funds sent to a misconfigured address.

## License

GPL-2.0+, consistent with the WordPress plugin ecosystem. See [LICENSE](LICENSE).
