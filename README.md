# Shopify GEO (Cursor / Grok Bot plugin)

Score a Shopify store for **GEO** (generative-engine readiness). Sellers install this **GEO** connector from the marketplace, then the Bot guides signup and scoring in chat — no terminal, and no need to know the middleware name.

卖家安装市场里的 **GEO** 连接器后，Bot 在聊天里引导注册与打分；不需要终端，也不需要知道底层中间件叫什么。

## What you get / 包含内容

| Piece | Role |
| --- | --- |
| `mcp.json` | Streamable HTTP MCP → kit host (`shopify-geo` server) |
| `skills/shopify-geo-onboard` | After install → register → bind key → usage |
| `skills/shopify-geo-eval` | Run `geo_eval` → poll → plain-language result |
| `variables.BEANKIT_API_KEY` | **Optional at install** — Configure after guided signup |

## Install / 安装（卖家视角）

1. Install **Shopify GEO** from the Cursor marketplace (search GEO / Shopify GEO).
2. No API key needed to finish install.
3. Right after install, the Bot **starts guiding** signup (email in chat, password via secure prompt). Seller does **not** need to say a product codename.
4. After they get `bk_live_…`, open **Plugins → Configure** (or let the Bot set it) and paste the key once.

中文：市场搜 GEO / Shopify GEO 安装 → Bot 自动引导注册 → Configure 填密钥 → 发店铺链接打分。

## Example utterances / 示例说法

- 「给我的站 https://example.com 打 GEO 分」
- 「开通 GEO」/ 「注册一下」
- 「重新接上」/ 「连不上了」

Do **not** require sellers to say middleware brand names.

## Privacy / 隐私

- This repository contains **no secrets**.
- Seller API keys live in Cursor / Grok Bot plugin configuration only.

## For marketplace reviewers / 审核说明

1. Register at https://bots.beansmile.ai (email verify required).
2. Create an application with bot type `shopify_geo_eval`.
3. Mint an API secret (`bk_live_…`).
4. Configure this plugin’s API key, then ask the agent to score a public Shopify storefront URL.

Contact: Beansmile — https://bots.beansmile.ai

## License

MIT © Beansmile
