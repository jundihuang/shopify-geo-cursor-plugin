---
name: shopify-geo-eval
description: >-
  Use when a seller wants a Shopify GEO score / 打分 / 评测店铺 / geo_eval for a
  store URL. Ensure the Shopify GEO MCP is connected with a valid key, run
  geo_eval, poll status, return the result in plain language. Do not require
  sellers to know any middleware brand name.
---

# Shopify GEO eval (score a store)

## When to use

Seller asks to 打分、评测、GEO 分、AI 可见性, pastes a store URL after onboard, or says 「给我的站 … 打 GEO 分」.

## Preconditions

1. Shopify GEO MCP / plugin is installed.
2. API key (`bk_live_…`) is configured on the plugin, or an equivalent MCP header is set.
3. If tools are missing, return 401 / unauthorized, or auth clearly failed → stop and run skill `shopify-geo-onboard` (register or reconfigure). Do not invent scores.

## How to run

1. Confirm the store URL (https). If missing, ask once for it.
2. Call MCP tool **`geo_eval`** with the store URL (and any required args the tool schema lists).
3. Poll **`geo_eval_status`** until finished or failed (expect about **1–2 minutes**; say that up front in plain language).
4. Call **`geo_eval_result`** and summarize for the seller: overall score, main strengths/gaps, and what to fix first. Prefer report artifacts (links or files) over dumping raw JSON.
5. Mention temporary free quota briefly if relevant; do not discuss internal billing modes.

## Plain-language copy

- 评测大概一两分钟，请稍等
- 新账号通常有临时免费额度；不够了可以说「重新接上」或联系运营
- 失败 / 未授权：先检查密钥是否 Configure 进插件，或说「重新接上」走 onboard

## Rules

- Never print full `bk_live_…` or JWT.
- Do not use curl/terminal as the seller path; use MCP tools.
- Never tell sellers they must “install beankit” or learn an internal codename.
- If the seller only wants setup, defer to `shopify-geo-onboard`.
- One store at a time unless they clearly ask for a batch and the tools support it.
