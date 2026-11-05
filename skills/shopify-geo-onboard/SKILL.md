---
name: shopify-geo-onboard
description: >-
  Use when a seller just installed the Shopify GEO marketplace connector/MCP,
  or wants to 开通 / 注册 / 打通 GEO / set up GEO scoring. Guide signup on the
  kit host, bind the API key into plugin config, then teach scoring — chat only,
  no terminal. Never require sellers to know or say the middleware brand name.
---

# Shopify GEO onboard (install → register → bind key → use)

## Target seller experience

1. **Install** — seller already installed (or is installing) the **Shopify GEO** Cursor / Grok Bot plugin / MCP. They may have found it as “GEO” alone; they do **not** need to say or know any middleware product name.
2. **Guided registration** — Bot walks email → password → verify email → create GEO app → mint secret.
3. **Bind key** — `bk_live_…` goes into plugin Configure / InstallPlugin values (seller does not paste into a terminal).
4. **Usage guide** — send store URL, wait 1–2 minutes, say 「重新接上」 if access breaks.

Base URL: `https://kit.beansmile.com` (ops only; do not brand this to sellers unless they ask where accounts live).

Do **not** teach curl/Terminal as the primary path. Do **not** echo full secrets, passwords, JWTs, or `bk_live_…` values in chat (at most a short prefix when confirming success).

## When to start

Trigger **immediately after GEO MCP / plugin install**, or when the seller says 开通、注册、打通 GEO、set up GEO, pastes a store URL before auth works, or when MCP tools return auth errors and no key is configured.

Never tell the seller they must “install beankit” or learn an internal product codename.

## Flow (v1: key optional at install)

`BEANKIT_API_KEY` is **optional** in plugin variables so install can succeed first. Hosted MCP at `/mcp` still needs a Bearer key before tools work — complete registration, then Configure.

### 1. Confirm install

- GEO connector is installed (marketplace “Shopify GEO” / GEO MCP). Skills work even before the key is set.
- One short sentence: next you will create their account, then bind the key so scoring works.

### 2. Register

1. Collect **email** in chat.
2. Collect **password** only via a secret-request / secure form that lands in `process.env.BEANKIT_SELLER_PASSWORD` (or the platform equivalent). Never ask them to paste the password into plain chat if a secret widget exists; never print it.
3. `POST https://kit.beansmile.com/auth/register` with email + password (from env).
4. Tell them to open the verification email. Wait for 「已验证」 / "verified", or accept a pasted verify URL when the API allows.
5. `POST https://kit.beansmile.com/auth/login` → keep JWT only in env; never show it.

### 3. Create app + mint secret

1. `POST https://kit.beansmile.com/apps` with `bot_type`: `shopify_geo_eval` (default). Reuse an existing app of that type if one already exists.
2. `POST https://kit.beansmile.com/apps/{slug}/secrets` → receive `bk_live_…`. Keep it in env only.

### 4. Bind key into the plugin (no terminal)

Prefer, in order:

1. **Plugins → Configure** (or Bot `InstallPlugin` / configure with `values.BEANKIT_API_KEY`) so `${BEANKIT_API_KEY}` substitutes into `mcp.json`.
2. If Configure is unavailable, `AddMcpServer` name `shopify-geo`, url `https://kit.beansmile.com/mcp`, header `Authorization: Bearer <key from env>`.

Ask before writing the connector. After bind, confirm tools are reachable without dumping the key.

If they **already have** `bk_live_…`, secret-request it and jump to bind + usage guide.

### 5. Usage guide (required same turn or next)

Tell them roughly:

- 直接发店铺链接，例如：「给我的站 https://example.com 打 GEO 分」
- 评测要一两分钟，完成后给你分数和报告（临时有免费额度）
- 出问题可以说「重新接上」或找运营加额度

Then offer to run one score if they paste a URL. Hand off scoring details to skill `shopify-geo-eval`.

## Rules

- No billing_mode / unlimited-switch talk; prepaid + signup bonus only.
- Never echo full secrets or JWT in chat.
- Default `bot_type`: `shopify_geo_eval`.
- Chat-first only; terminal/curl is fallback for operators debugging, never the seller path.
- Do not brand the middleware to sellers. OAuth / zero-paste login is out of scope for v1 — escalate to PM if needed.
