# Shadowrocket Rules

Shadowrocket configuration files for routing selected traffic through a proxy while keeping trusted, local, or high-volume traffic direct.

## Files

- `restrictive.conf`: restrictive profile with curated `DIRECT`, `PROXY`, and reject behavior. It includes local/Iran-hosted direct routes, Apple service rules, Copilot proxy rules, and a final restrictive policy.
- `telegram-only.conf`: minimal profile that proxies Telegram domains and IP ranges, allows LAN traffic directly, and rejects everything else.

## Rule Ordering

Shadowrocket evaluates rules from top to bottom and stops at the first match. More specific exceptions must stay above broader suffix or keyword rules.

Preferred precedence when rules overlap:

1. `DOMAIN`
2. `DOMAIN-SUFFIX`, with longer suffixes before shorter suffixes
3. `DOMAIN-KEYWORD`

For example, Apple exception hosts such as `gateway.icloud.com` and `olympus.itunes.apple.com` must remain above broader Apple suffix rules if their policies differ.

## Using

Import the desired `.conf` file into Shadowrocket and attach your own proxy node or subscription as needed. These files define routing behavior; they do not include proxy server credentials.

## Editing

Before adding or moving a rule, check whether it overlaps with an existing `DOMAIN-SUFFIX` or `DOMAIN-KEYWORD` rule. Do not change a rule policy or type unless that behavior change is intentional.

## License

Released under the MIT License. See `LICENSE`.
