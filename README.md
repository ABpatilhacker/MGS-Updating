# Mercury Global Systems — storefront and PHP/MySQL starter

This package contains the MGS company pages, updated shop, browser preview flows, and a PHP/MySQL API starter for shared hosting. It is written in plain HTML, CSS, JavaScript, PHP and SQL, with comments around the places intended for future edits.

## 1. Preview it locally

1. Extract the ZIP.
2. Open the `MGS` folder in VS Code.
3. Start VS Code Live Server from `index.html`.
4. Use `Shop` to search the catalogue, open product details, add items to the basket and try the quote form. The admin editor can be previewed locally at `admin.html`.

When there is no configured database, localhost uses browser-only demo data. Browser edits, accounts and enquiries do not update the live website or other browsers.

## 2. Products and pricing

- `js/catalog.js` is the starting catalogue source (12 Microsoft software listings and 10 SSD capacity options).
- Every price starts unset intentionally. MGS must approve the EUR and INR prices before publishing them.
- Images under `assets/images/products/` are representative examples, not exact packshots. Replace them with MGS-owned product photos when available.
- Product names such as `Office 2026` and `Office 24 Home Bind` are preserved from the supplied list; confirm the exact manufacturer SKU and licence rights before taking an order.
- Do not promise a fixed lifetime/user/device licence unless the exact SKU licence terms support it.

## 3. PHP/MySQL server setup

The API lives at `api/index.php`; schema and maintenance tools live in `private/`. It requires PHP 8.1+ with PDO MySQL and a MySQL/MariaDB database.

1. Create a database and database user in the hosting panel; grant the user access only to that database.
2. Import `private/schema.sql` using the hosting database tool.
3. Copy `private/config.example.php` to `private/config.php` and add the actual DB host, name, username and password. Never place real credentials in a public repository. Where the hosting environment allows it, place the secret config outside the document root and adjust the config path in `api/index.php` and the CLI scripts.
4. Upload the `MGS` site to the hosting document root over SFTP. Keep HTTPS enabled. `private/.htaccess` blocks browser access on Apache, but secrets outside the web root are preferable.
5. From SSH/CLI, run `php private/seed-products.php` to insert the starter catalogue. Re-running the script updates product details but preserves the EUR/INR price columns.
6. Create the first administrator from SSH/CLI with `php private/create-admin.php admin@example.com "MGS Administrator"`. It prompts for a 12+ character password when interactive terminal input is available. Do not run admin creation through a browser. Keep the credentials private.
7. Open the public site and test sign-up, login, product edits, settings, and quote enquiries. With the database configured, the same frontend automatically uses the server API instead of browser-only accounts/catalogue edits.

The API includes server-side password hashing, session cookies, CSRF checks for POST requests, role checks for admin endpoints, parameterised SQL, product/price storage and saved quote enquiries. This is a starter and still needs a security review on the target hosting environment before handling real customer data.

## 4. Before taking paid orders

**Card payments are not connected.** The checkout currently creates a quote/enquiry record and explicitly does not take a payment. Select a supported payment provider, obtain merchant credentials, and integrate its hosted checkout/webhooks server-side before charging customers. Never collect raw card numbers in these HTML forms or store them in MySQL.

Before a real public launch also configure transactional email and account recovery/verification, test HTTPS/session cookie settings, set local VAT/tax and delivery/refund policies with the business, review the software licence terms/privacy/cookie notice for the markets served, verify stock and exact SKU/licence rights, add backups/logging/rate limiting and perform a production security test. This ZIP does not claim those tasks are completed or that payment is live.

## 5. Quick map of editable files

- `css/storefront.css` — the latest responsive visual overrides, header/footer, shop grid and admin layout.
- `js/catalog.js` — starting product catalogue.
- `js/storefront.js` — shop/cart/account/admin interactions and API connection.
- `js/main.js` — site navigation and contact email-draft behaviour.
- `admin.html` and `admin-product.html` — admin list/settings and full individual product editor.
- `api/index.php` — same-origin PHP API.
- `private/schema.sql` — database tables.
- `private/config.example.php` — configuration template only; create private `config.php` with real credentials on the server.

All CSS override changes are grouped under the `MGS VISUAL REFINEMENT` comment, and the JavaScript/PHP include explanatory comments to help future editing.
