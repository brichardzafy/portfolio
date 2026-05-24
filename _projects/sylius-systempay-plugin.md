---
layout: page
title: Sylius Systempay Plugin
description: Open-source Sylius plugin that integrates the Systempay (Lyra) payment gateway into Symfony / Sylius e-commerce stores.
img: assets/img/sylius-systempay.png
importance: 6
category: fun
giscus_comments: false
---

[Sylius Systempay Plugin](https://github.com/brichardzafy/sylius-systempay-plugin) is an open-source payment plugin
that plugs the [Systempay](https://systempay.fr) gateway (operated by **Lyra Network**) into any
[Sylius](https://sylius.com)-based e-commerce store. It exposes Systempay as a first-class Sylius payment method,
handles the hosted-payment redirect flow, processes the asynchronous **IPN webhook** to confirm orders, and ships the
configuration UI directly inside the Sylius admin.

Sylius is the reference open-source headless-commerce framework on top of Symfony, but it does not natively support
Systempay — a gateway widely used by French and European merchants (BNP Paribas, Banque Populaire, Caisse d'Épargne,
Crédit Agricole, Crédit Mutuel, La Banque Postale, Société Générale…). This plugin fills that gap with a
production-ready, Sylius-idiomatic integration.

## Stack

| Layer | Tech |
| --- | --- |
| Language | PHP 8.1+ (79% of the codebase) |
| Framework | Symfony / Sylius |
| Templating | Twig |
| Frontend assets | JavaScript, Webpack Encore |
| Payment SDK | [`lyracom/rest-php-sdk`](https://packagist.org/packages/lyracom/rest-php-sdk) `^4.0` |
| Persistence | Doctrine ORM + database migrations |
| Tooling | PHPUnit, PHPSpec, Behat, PHPStan, ECS / PHPCS, Makefile, Docker Compose |
| CI | GitHub Actions |

## What the plugin does

- Registers **Systempay** as a Sylius payment method with full admin configuration (shop ID, API keys, HMAC,
  environment toggle TEST / PRODUCTION).
- Overrides the Sylius shop checkout flow to redirect the customer to the Systempay hosted payment page with a
  correctly signed request.
- Exposes a secure **webhook endpoint** that validates the Lyra signature and transitions the Sylius order /
  payment state machine on success, failure or cancellation.
- Ships **Doctrine migrations** and a Twig / form translation bundle (admin form translations) out of the box.
- Provides a typical Sylius plugin layout (`src/`, `tests/`, `migrations/`, `bin/`, `composer.json`,
  `webpack.config.js`, full QA toolchain) so it can be installed as a path repository inside any Sylius project.

## Integration model

The plugin is meant to be cloned into `ProjectDir/src/Plugins/sylius-systempay-plugin` and wired through:

- `composer.json` — declare the `lyracom/rest-php-sdk` dependency and a PSR-4 autoload entry for
  `Sylius\SystempayPlugin\` pointing at the plugin's `src/`.
- `config/bundles.php` — register `Sylius\SystempayPlugin\SyliusSystempayPlugin`.
- `routes/sylius_shop.yaml` — import `@SyliusSystempayPlugin/Resources/config/shop_routing.yaml` under the locale
  prefix to override the checkout *complete* action.
- `routes.yaml` — import `@SyliusSystempayPlugin/Resources/config/webhook_routing.yaml` to expose the IPN endpoint.
- `php bin/console doctrine:migrations:migrate` — apply the bundled migrations.

## My role

I am the **author and maintainer** of the plugin: design of the Sylius integration (payment method, gateway factory,
checkout override, webhook controller), wiring of the Lyra REST SDK, Doctrine schema and migrations, admin form &
translations, and the full QA / CI setup (PHPUnit, PHPSpec, Behat, PHPStan, ECS, GitHub Actions).
