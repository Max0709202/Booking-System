# WP Traveler Project

WordPress project built around the `Traveler` theme with bundled payment plugins.

## Repository Structure

- `theme/traveler` - main Traveler theme source.
- `plugins/traveler-paypal-v2` - custom PayPal gateway integration used by Traveler checkout flow.
- `plugins/woocommerce-paypal-payments` - official WooCommerce PayPal plugin (separate flow from Traveler AJAX checkout).
- `.trunk` - linting/tooling configuration.

## Requirements

- WordPress
- PHP 7.4+
- MySQL/MariaDB
- WooCommerce (recommended if using WooCommerce checkout pages)

## Local Setup

1. Place `theme/traveler` in your WordPress `wp-content/themes/`.
2. Place plugins in `wp-content/plugins/`.
3. Activate the theme and required plugins from WordPress admin.
4. Configure Traveler settings:
   - `Traveler -> Theme Options -> Booking Options`
   - `Traveler -> Theme Options -> Payment Options`
5. Configure WooCommerce pages:
   - `WooCommerce -> Settings -> Advanced`
   - Set separate pages for Cart and Checkout.

## Important Checkout Notes

This project can run two different checkout paths:

- Traveler AJAX checkout (`action=booking_form_direct_submit`)
- WooCommerce checkout (`wc-ajax` / standard WooCommerce flow)

If requests still use `booking_form_direct_submit`, the page is still using Traveler checkout templates/scripts even when WooCommerce options are enabled.

## Known Issue: PayPal Division by Zero

If checkout fails with:

`DivisionByZeroError` in `plugins/traveler-paypal-v2/inc/paypal.php`

The currency rate used in conversion is zero or missing.

Check:

- `Traveler -> Theme Options -> Booking Options -> List of currencies`
- Ensure each currency `rate` is a valid positive number.
- Ensure the base currency rate is usually `1`.

## Debug Logging

To enable WordPress logs, set in `wp-config.php`:

```php
define('WP_DEBUG', true);
define('WP_DEBUG_LOG', true);
define('WP_DEBUG_DISPLAY', false);
```

Then inspect:

- `wp-content/debug.log`

## Security Note

This repository contains values that may be detected as secrets by GitHub push protection. Rotate/revoke any real credentials and tokens before production use.

