# Nuvols Agentic Commerce for PrestaShop

> **Early Access** — This module is under active development and not yet production-ready. APIs, features, and data formats may change without notice. We welcome testers and feedback via the [contact form on nuvols.app](https://nuvols.app/prestashop).

Connect your PrestaShop store to [nuvols.app](https://nuvols.app) for AI-powered commerce. Enables your products to be discovered and purchased through AI agents like Google Gemini and ChatGPT via the Universal Commerce Protocol (UCP) and Agentic Commerce Protocol (ACP).

## How It Works

This lightweight module connects your PrestaShop store to [nuvols.app](https://nuvols.app), which handles all protocol logic. Your store's product data is synced to nuvols.app, and AI shopping agents communicate with nuvols.app to browse, build carts, and complete purchases on behalf of customers.

- No protocol logic runs on your server
- Automatic compliance with UCP and ACP spec updates
- AI agents never access your store directly

## Requirements

- PrestaShop 1.6.1.0 -- 9.x
- PHP 7.1+
- SSL/HTTPS enabled

## Installation

1. Download the latest release ZIP from [Releases](../../releases)
2. In your PrestaShop Back Office, go to **Modules > Module Manager > Upload a module**
3. Upload the ZIP file
4. Go to the module configuration page
5. Click **Connect to nuvols.app**

That's it. Your store is now AI-discoverable.

## Features

**Handled by nuvols.app (SaaS):**
- UCP and ACP protocol endpoints
- AI agent checkout sessions
- Google Pay and payment gateway mapping
- Business profile and discovery responses
- Analytics dashboard
- Spec version compliance
- Marketing consent labels shown to shoppers at agent checkout
- Seller-backed payment options (gift cards, store credit, saved cards, points)

**Handled by the module (on your server):**
- One-click connection to nuvols.app
- Product data sync (push on save)
- Per-product AI commerce toggle in product catalog
- Bulk enable/disable via product grid actions
- Google Shopping feed rewriting
- Discovery profile at `/.well-known/ucp`
- Discount code resolution — AI-proposed codes are matched against your PrestaShop cart rules and applied to the cart
- Marketing consent forwarded to a PrestaShop hook so your newsletter module can subscribe the shopper
- Deep-link admin panels that open the nuvols.app dashboard for marketing-consent and seller-backed-payment configuration

## Developer Hooks

### Marketing consent

When a shopper opts into marketing at AI-agent checkout, the module fires one hook per consent entry during order creation. The hook `actionNuvolsMarketingConsentGiven` is registered automatically on module install:

```php
Hook::exec('actionNuvolsMarketingConsentGiven', array(
    'channel'  => $channel,   // e.g. 'marketing', 'newsletter'
    'opted_in' => $opted_in,  // bool
    'order_id' => $order_id,  // int
    'order'    => $order,     // Order object
));
```

Wire this into your mailing-list integration (Mailchimp, Sendinblue/Brevo, Klaviyo, etc.) from a module of your own:

```php
public function hookActionNuvolsMarketingConsentGiven($params)
{
    if (empty($params['opted_in'])) {
        return;
    }
    $order = $params['order'];
    $customer = new Customer((int) $order->id_customer);
    // Subscribe $customer->email to your newsletter list
}
```

`channel` identifies which opt-in was accepted (e.g. `marketing`, `newsletter`) as configured on the nuvols.app dashboard.

## Documentation

Visit [nuvols.app/prestashop](https://nuvols.app/prestashop) for full documentation, FAQs, and support.

## Support

For questions or issues, visit [nuvols.app](https://nuvols.app) and use the contact form, or open an issue in this repository.

## License

This module is licensed under the [Academic Free License 3.0 (AFL-3.0)](https://opensource.org/licenses/AFL-3.0).
