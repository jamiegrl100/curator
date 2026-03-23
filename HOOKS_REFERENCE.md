
Copy/paste this into docs/HOOKS_REFERENCE.md.
# Hooks Reference

Curator uses WordPress-style hooks to let plugins extend core behavior safely.

- **Actions**: run code at specific points
- **Filters**: modify values before core uses them

> If a hook isn’t listed here, assume it’s not stable yet.

---

## Quick examples

### Actions
```php
add_action('init', function () {
  // plugin boot logic
});
Copy
Filters
add_filter('search_results', function (array $results, array $context) {
  return $results;
});
Copy
Hook API
add_action(string $hook, callable $fn, int $priority = 10): void
Registers an action callback.
do_action(string $hook, ...$args): void
Runs all callbacks for an action hook.
add_filter(string $hook, callable $fn, int $priority = 10): void
Registers a filter callback.
apply_filters(string $hook, mixed $value, ...$args): mixed
Passes a value through all filter callbacks and returns the final value.
Core lifecycle hooks
init
Runs after core boot, before routing.
Use for:
registering services
reading settings
preparing caches
Args: none
Routing hooks
routes_register
Plugins register routes here.
Use for:
adding public endpoints under /p/<plugin>/...
Args:
$router (your router instance)
Example:
add_action('routes_register', function ($router) {
  $router->get('/p/my-plugin/health', function () {
    header('Content-Type: application/json');
    echo json_encode(['ok' => true]);
  });
});
Copy
Notes:
Plugin routes should be prefixed: /p/<plugin-slug>/...
Core routes must not be overridden in v1.
Admin hooks
admin_menu_register
Register admin pages and menu items.
Args:
$adminMenu (menu registry)
Use for:
adding plugin settings pages
Rendering hooks
render_head
Runs inside <head> after core meta tags, before closing </head>.
Args:
$context (array: route, user, page type)
Use for:
enqueueing plugin CSS
adding meta tags
render_footer
Runs before </body>.
Use for:
enqueueing plugin JS
adding small widgets
Theme hooks (if enabled in your build)
theme_templates_register
Allow plugins to register or override template keys (advanced).
Args:
$themeRegistry
Keep this limited. Template overrides should usually be done by themes, not plugins.
Catalog + search hooks
search_query_build
Modify the search query context before execution.
Args:
$query (string)
$filters (array)
$sort (string)
Return (filter):
modified query/filters/sort (depending on your implementation)
search_results
Modify results after query execution.
Args:
$results (array)
$context (array: q, filters, sort, page)
Return:
updated results array
product_view
Runs when a product page is viewed.
Args:
$productId
$context (session/referrer/utm if available)
Cart + checkout hooks (digital core)
cart_item_add
Runs when a product is added to cart.
Args:
$productId
$quantity
$context
checkout_started
Runs when checkout begins (before redirect to hosted gateway).
Args:
$orderId
$gatewayId
$context
order_paid
Runs when an order becomes paid (typically from webhook).
Args:
$orderId
$gatewayId
$context
Use for:
granting entitlements
sending extra emails
analytics tracking
Downloads hooks
download_token_created
Runs when a download token is created.
Args:
$tokenId
$orderId
$productFileId
download_served
Runs when a file is successfully served.
Args:
$token
$orderId
$productFileId
download_denied
Runs when a download attempt is denied.
Args:
$token
$reason (expired/revoked/maxed/unauthorized)
$context
Email hooks
email_queued
Runs when an email is queued.
Args:
$emailId
$type (order_paid, downloads_ready, password_reset, etc.)
email_sent
Runs when an email is successfully sent.
email_failed
Runs when an email fails.
Marketplace pipeline hooks (if marketplace enabled)
submission_received
Runs when a developer submits a listing or release.
release_checks_completed
Runs after automated checks complete.
release_published
Runs when a release is published.
Best practices
Keep hooks fast. Don’t do heavy work inline; queue jobs if needed.
Validate and sanitize all input.
Avoid writing files outside plugin storage.
Prefer filters for data shaping, actions for side effects.
