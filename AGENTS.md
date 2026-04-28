# Shadowrocket Rules Guide

## Purpose
This repository contains a Shadowrocket configuration where routing behavior is controlled by ordered rules in the `[Rule]` section.

## How Matching Works
Shadowrocket evaluates rules from top to bottom and stops at the first match.

Example:

```ini
DOMAIN-SUFFIX,icloud.com,DIRECT
DOMAIN-SUFFIX,gateway.icloud.com,PROXY
```

In this order, `gateway.icloud.com` matches `icloud.com` first and becomes `DIRECT` (wrong for this intent).
Correct order:

```ini
DOMAIN-SUFFIX,gateway.icloud.com,PROXY
DOMAIN-SUFFIX,icloud.com,DIRECT
```

## Rule Types Used Here
- `DOMAIN`: exact hostname match (most specific)
- `DOMAIN-SUFFIX`: matches a domain and all subdomains
- `DOMAIN-KEYWORD`: matches if keyword appears in hostname (least specific, broadest)

## Policies Used Here
- `PROXY`: route through proxy
- `DIRECT`: connect directly
- `REJECT` / `REJECT-NO-DROP`: block traffic

## Ordering Principles
- Put more specific rules before broader rules.
- Keep exception rules before catch-all rules.
- If two rules overlap and actions differ, the intended winner must be higher.
- Prefer this precedence when overlaps exist:
  1. `DOMAIN` (exact host)
  2. `DOMAIN-SUFFIX` (longer suffix before shorter suffix)
  3. `DOMAIN-KEYWORD`

## Apple Rules Notes
- Apple entries include both `PROXY` control-plane endpoints and `DIRECT` heavy-content endpoints.
- Overlap exists in families like:
  - `olympus.itunes.apple.com` (`PROXY`) vs `DOMAIN-SUFFIX,itunes.apple.com` (`DIRECT`)
  - `gateway.icloud.com` (`PROXY`) vs `DOMAIN-SUFFIX,icloud.com` (`DIRECT`)
- Exception hostnames must stay above their broader Apple suffix rules.

## Safe Edit Checklist
- Do not change policy (`PROXY`/`DIRECT`/`REJECT`) unless explicitly intended.
- Do not change rule type (`DOMAIN` vs `DOMAIN-SUFFIX` vs `DOMAIN-KEYWORD`) unless explicitly intended.
- After adding a new rule, check for overlap with existing suffix/keyword rules.
- Reorder only as needed to preserve intended first-match behavior.
