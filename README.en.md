# Browser Product Compare Assistant

![浏览器商品对比助手](assets/promo-1600x900.png)

Free and no third-party API key: reuse an already signed-in browser to compare visible price, specifications, after-sales details, and use cases across 2–5 product pages. It produces a Markdown report with sources and read date. EasyBR multi-account profiles remain isolated by one explicit `browserId`.

The workflow is read-only: no purchase, cart action, payment, third-party API, or bulk crawling.

## Quick start

Install Easy WebBridge from [GitHub](https://github.com/xxjrq/easy-webbridge), or use the [Gitee mirror](https://gitee.com/xxjrq/easy-webbridge). Load its extension in the browser profile that already has the desired login state, then provide the exact `browserId` and 2–5 product URLs to the Skill.

Each run creates one task session, reuses it for the requested pages, and closes only that self-created session in `finally`. Existing tabs and other EasyBR accounts are left untouched.

Run the repository checks with `node scripts/self-test.mjs`. The success input and resulting copy-ready comparison are in `fixtures/`.

## License and sources

This is an original workflow. Adopted external integration details and license checks are recorded in [sources.md](sources.md). Licensed under [MIT](LICENSE).

## Install

```bash
npx skills add xxjrq/web-product-compare-free
```

Alternatively, copy this repository folder into your Agent’s skill directory, then invoke `$web-product-compare-free` with your input.
