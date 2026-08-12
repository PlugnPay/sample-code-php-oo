# sample-code-php-oo
PHP API Object-Oriented (3rd party / Remote Client)

**************************************************************************
** This code is NOT officially supported by Plug 'n Pay Technologies Inc. **
**************************************************************************

**Deprecated:** This community OO PHP wrapper (`PnP`) is deprecated. New
integrations should call the normal Remote API (`pnpremote.cgi`) with a
current HTTPS client library (see [sample-code-php-source](https://github.com/PlugnPay/sample-code-php-source)).
Existing implementations may continue to work, but this 2008-era Google
Code project is no longer recommended.

This repository does **not** vendor the library. The original project lived
on Google Code and is now in the [Google Code Archive](https://code.google.com/archive/p/plugnpay-php/).
A GitHub export of that archive is at [honguyenbinh/plugnpay-php](https://github.com/honguyenbinh/plugnpay-php).

The library was written by Rich Bowen (EAS Technologies LLC) and released
as Apache License 2.0 (ASL2). It is a community project, not a PlugnPay
product.

You will need to understand the Remote Client Specification documentation
in order to use this module. It is located in the Documentation/FAQ section
of the admin area.

## Requirements

* PHP with cURL (the `PnP` class posts to the gateway over HTTPS)
* HTTPS outbound access to `https://pay1.plugnpay.com/payment/pnpremote.cgi`
* A live PlugnPay gateway account
* Your account's **Remote Client Password** (sent as `publisher-password`,
  next to `publisher-name` in the request)

Copy `PnP.php` from the archived project and set `publisher_name` /
`publisher_password` on the class (or pass them per call) before running
anything against a live account.

## Installation

1. Download `PnP.php` from the [Google Code Archive](https://code.google.com/archive/p/plugnpay-php/downloads)
   or the [GitHub export](https://github.com/honguyenbinh/plugnpay-php).
2. Place it somewhere your application can `require_once`.
3. Set gateway credentials on the class (do not leave the sample
   placeholders in production).

## Usage

Construct `PnP`, then call a Remote Client mode as a method. Explicit
methods cover common modes; anything else is forwarded through `__call`,
so undocumented modes can still be invoked if you pass the right fields.

```php
require_once 'path/to/PnP.php';

$p = new PnP();
$p->auth(array(
  'publisher-name'     => 'pnpdemo',
  'publisher-password' => 'your_remote_client_password',
  'publisher-email'    => 'test@plugnpay.com',
  'card-number'        => '4111111111111111',
  'card-name'          => 'cardtest',
  'card-amount'        => '1.00',
  'card-exp'           => '12/30',
  'card-cvv'           => '123',
));

print_r($p->query_trans);
```

To inspect an existing transaction, construct with an `orderID`:

```php
$p = new PnP(array('orderID' => '12345'));
$status = $p->query_trans();
```

Required / common fields:

* `publisher-name` — your PlugnPay gateway account username
* `publisher-password` — your Remote Client Password (same place/manner as
  `publisher-name`)
* `publisher-email` — contact email for the transaction
* `card-name`, `card-number`, `card-exp`, `card-amount`, and other Remote
  Client fields as needed

The archived project also has `docs/` and `tests/` with further examples.

### Troubleshooting

| Message / symptom | Cause |
| --- | --- |
| `PnP.php` not found | `require_once` path does not point at the downloaded file |
| cURL / SSL errors | PHP cURL or CA certs missing; cannot reach `pay1.plugnpay.com` |
| `Invalid Mode` | Method name is not a valid Remote Client `mode` |
| Blank / missing auth fields | Missing `publisher-name` or `publisher-password` |

Common problems: see the API FAQ in Merchant Admin.

## Repository layout

```
sample-code-php-oo/
  README.md    # this file (pointer to the archived 3rd-party library)
```

The `PnP` class itself is not stored here. Get it from the Google Code
Archive or the GitHub export linked above.

## Support

Provided AS IS. This 3rd-party library is not supported by PlugnPay.
Limited technical support for the Remote API itself may be available via
PlugnPay's Online Helpdesk after checking this README and the API FAQ.
See [PlugnPay docs](https://docs.plugnpay.com/) and the Remote Client
Specification in Merchant Admin for field details.

For issues with the archived `plugnpay-php` project, use that project's
pages — not PlugnPay support.

Customized versions of this sample are not supported.
