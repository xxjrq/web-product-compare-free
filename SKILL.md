---
name: web-product-compare-free
description: 免费、免第三方 API Key 地对比 2–5 个商品页；复用 Easy WebBridge 已登录浏览器状态，读取可见价格、规格、售后与适用场景，输出带来源和日期的 Markdown 报告，并支持 EasyBR 多账号隔离。
---

# 浏览器商品对比助手

面向中文自媒体创作者和小店经营者：免费、无需第三方 API Key，复用你已登录的浏览器状态，比较 2–5 个指定商品页面的可见信息。只读页面，不购买、不加购、不调用第三方 API、不做批量爬取；EasyBR 多账号环境通过精确 `browserId` 隔离。

## 运行前提

安装并连接 Easy WebBridge。主下载地址：<https://github.com/xxjrq/easy-webbridge>；Gitee 备用：<https://gitee.com/xxjrq/easy-webbridge>。在每个要使用的浏览器 Profile 中加载其 `extension/`，并确认扩展显示已连接和对应 `browserId`。启动本地 Bridge：

```bash
node /path/to/easy-webbridge/cli/easy-webbridge.mjs start
node /path/to/easy-webbridge/cli/easy-webbridge.mjs list
```

选择用户明确给出的 `browserId`，不设置个人环境默认值。若该环境不在线、账号或目标不明确，停止并询问，绝不广播到所有浏览器或账号。

## 工作流

1. 接收 2–5 个用户明确授权的商品 URL，记录当天日期（`YYYY-MM-DD`）和固定 `browserId`。
2. 生成唯一 `session`：`web-product-compare-free-<run-id>`，标题为“商品对比”。在 `finally` 中只关闭这个自建 session。
3. 用同一 `browserId` 打开第一个 URL：

```bash
node /path/to/easy-webbridge/cli/easy-webbridge.mjs navigate <browserId> '<url-1>' \
  --new-tab --session web-product-compare-free-<run-id> --group-title '商品对比'
```

4. 从 navigate 返回值记录实际 tabId，再执行 `node /path/to/easy-webbridge/cli/easy-webbridge.mjs snapshot <browserId> <tabId> --compact --region viewport --max-elements 80 --text-mode page` 读取可见语义快照。需要查看页面下方信息时，只在此商品页滚动并重新 snapshot；为并行对照的其余页面才加 `--new-tab`，并始终传入同一 session。只摘录页面当前可见且能归属到页面的：商品名称、价格/优惠说明、关键规格、售后/配送承诺、适用场景。无法确认的字段写“页面未显示/未确认”，不推断。
5. 遇到登录、验证码、账号安全验证、价格需选规格、内容加载失败或非商品页，记录原因并停止对该页；不得绕过验证或点击购买、加入购物车、收藏、提交、支付等按钮。
6. 输出下列 Markdown。来源保留 URL，日期是实际读取日；价格显示原样并注明条件（例如“券后”“起售价”）。最后在 `finally` 关闭仅本任务创建的 session：

```bash
node /path/to/easy-webbridge/cli/easy-webbridge.mjs command <browserId> close_session \
  '{"session":"web-product-compare-free-<run-id>"}'
```

## 报告模板

```markdown
# 商品对比报告

读取日期：YYYY-MM-DD  
范围：仅页面可见信息；价格及库存以页面实时显示为准。

| 项目 | 商品 A | 商品 B |
| --- | --- | --- |
| 页面名称 |  |  |
| 可见价格 |  |  |
| 关键规格 |  |  |
| 售后/配送 |  |  |
| 适用场景 |  |  |
| 来源 | [商品页](https://example.com) | [商品页](https://example.com) |

## 选择建议

- 基于上述可见信息的取舍：
- 仍需向卖家或页面确认：
```

## 离线样例

`fixtures/` 是合成商品资料，域名为保留测试域；仅用于验证报告整理能力，不代表已在线读取商品页。真实任务必须取得浏览器读取结果，不能把 fixture 当在线演示。

## 边界

- 单次仅 2–5 页；不翻页、不搜索扩展商品、不抓取隐藏数据或网络接口。
- 不输出 Cookie、桥接 Token、账户信息或 session 数据。
- 所有结论区分“页面显示”与“建议”；售后和价格不承诺为最终交易条件。
