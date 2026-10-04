# 浏览器商品对比助手

![浏览器商品对比助手](assets/promo-1600x900.png)

免费、免第三方 API Key：复用你已登录的浏览器状态，对比 2–5 个商品页面的可见价格、规格、售后与适用场景，直接得到带来源和读取日期的 Markdown 报告。支持 EasyBR 多账号隔离，始终按一个明确 `browserId` 工作，不会广播到所有账号。

它只读你指定的页面：不购买、不加购、不支付、不调用第三方 API，也不批量爬取。

## 立即使用

先安装 Easy WebBridge：GitHub 主下载地址 <https://github.com/xxjrq/easy-webbridge>，Gitee 备用地址 <https://gitee.com/xxjrq/easy-webbridge>。在你已登录的浏览器 Profile 中加载扩展并连接；EasyBR 的每个环境都有独立登录态和 `browserId`。

将下面这段交给支持 Skill 的 Agent（把 <browserId> 换成你选定的在线浏览器 ID，并把两个测试 URL 换成真实商品 URL；测试域名不能在线访问）：

```text
使用 $web-product-compare-free，在 browserId <browserId>中只读对比这两个商品页：
https://shop.example.test/products/light-a
https://shop.example.test/products/light-b
读取可见价格、规格、售后和适用场景，输出带来源与今天日期的 Markdown。不要购买、加购或调用第三方 API。
```

离线试用：让 Agent 读取 `fixtures/success-input.json`，只整理其中的合成商品资料，不打开测试域名；这是离线报告样例，不是在线采集证明。

以本仓库成功样例为输入时，最终可复制的报告见 [fixtures/expected-report.md](fixtures/expected-report.md)。样例中 A 券后 ¥79，适合预算优先的桌面口播；B 为 ¥109，适合更看重电池容量与色温范围的场景。真实页面价格、库存及售后以读取时可见内容为准。

## 使用方式

运行前在本机启动 Bridge：

```bash
node /path/to/easy-webbridge/cli/easy-webbridge.mjs start
node /path/to/easy-webbridge/cli/easy-webbridge.mjs list
```

每次任务只选一个在线 `browserId`，创建一个 `web-product-compare-free-<run-id>` session。同一任务的 2–5 个页面均在该分组中打开；完成或失败时仅关闭此自建分组，保留用户已有标签页和其他账号环境。

报告固定包含页面名称、原样可见价格及条件、关键规格、售后/配送、适用场景、来源 URL、读取日期和明确的待确认事项。缺失字段会写“页面未显示/未确认”，不会臆测。

## 限制与安全

- 仅限用户指定的 2–5 个商品页和页面可见信息。
- 登录、验证码、账号安全验证、非商品页或动态规格价格无法确认时会停止并报告原因。
- 不记录或输出 Cookie、Token、密码、账户数据或 session 内容。

## 验证

```bash
node scripts/self-test.mjs
node ../skill-seo/scripts/audit.mjs --root . --strict
```

## 来源与许可

本仓库采用原创实现，实际参考范围与外部项目许可证核对见 [sources.md](sources.md)。本项目以 [MIT](LICENSE) 发布。

## 安装

```bash
npx skills add xxjrq/web-product-compare-free
```

也可以把本仓库目录复制到 Agent 的 Skills 目录，再用 `$web-product-compare-free` 加上你的输入调用。
