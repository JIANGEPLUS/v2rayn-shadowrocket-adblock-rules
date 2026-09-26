# v2rayN 黑名单与广告拦截规则 | Johnshall Shadowrocket 转换版

可直接导入 **v2rayN** 的 JSON 路由规则。基于 [Johnshall/Shadowrocket-ADBlock-Rules-Forever](https://github.com/Johnshall/Shadowrocket-ADBlock-Rules-Forever) 的「黑名单过滤 + 广告」配置，保留代理、拦截、直连的规则顺序，适用于 v2rayN 的 Xray / sing-box 路由。

**English:** Ready-to-import v2rayN routing rules converted from Johnshall's Shadowrocket blacklist + adblock configuration. This is a dated snapshot, not a live subscription.

> 规则快照：上游文件标注 `2026-09-25 23:04:05 UTC`。本仓库目前不自动更新规则。

## 下载与导入

下载 [`rules/v2rayn_johnshall_blacklist_ad_2026-09-25.json`](rules/v2rayn_johnshall_blacklist_ad_2026-09-25.json)。也可以复制[原始 JSON 直链](https://raw.githubusercontent.com/JIANGEPLUS/v2rayn-shadowrocket-adblock-rules/main/rules/v2rayn_johnshall_blacklist_ad_2026-09-25.json)到浏览器另存为文件。

1. 打开 v2rayN 的**设置 → 路由设置**，新建并选中一个自定义规则集。
2. 在**规则集设置**中选择**从文件中导入规则**，选中下载的 JSON。若询问追加还是替换，建议在新规则集中选择**替换**，避免重复或旧规则抢先匹配。
3. 保存规则集，并设为当前启用的路由规则。需要接管其他应用流量时，按自己的需求启用系统代理或 TUN。
4. 若希望 IP-CIDR 规则也用于域名流量，在路由设置中将**域名解析策略**设为 `IPOnDemand`。这可能增加 DNS 查询；请按自己的网络环境检查效果。

**不要**将此 JSON 当作订阅节点链接，也不要直接粘贴到 v2rayN 的完整核心配置中。它是 v2rayN「从文件中导入规则」所用的 `RulesItem` 数组。

## 规则内容

| 上游 Shadowrocket 规则 | v2rayN 规则 |
| --- | --- |
| `DOMAIN-SUFFIX` | `domain:` 域名后缀匹配 |
| `DOMAIN-KEYWORD` | `keyword:` 域名关键词匹配 |
| `IP-CIDR` | `ip` 网段匹配 |
| `Proxy` | `proxy` 出站 |
| `Reject` | `block` 出站 |
| `FINAL,direct` | 最后一条 `direct` 规则 |
| Apple News 外部规则集 | 展开为 2 条精确域名规则 |
| `skip-proxy` | 尽量转为排在最前面的直连例外 |

保留上游的 **代理 → 拦截 → 代理 → 默认直连** 顺序。连续、同动作且同类型的条件分批合并，每项最多 1,000 个条件，避免合并域名/IP 时改变规则优先级。

本快照包含 **148 个 v2rayN 规则项**：89,946 条具体匹配条件，另有 1 条最终直连。转换后 JSON 约 3 MB；大规则集的加载和匹配性能取决于设备与核心版本。

## 能力边界

这份文件只负责**域名和 IP 路由/拦截**。Shadowrocket 配置里的 `bypass-tun`、DNS 服务器、IPv6 开关、`[URL Rewrite]` 和 `[MITM]` 无法由 v2rayN 路由 JSON 等价表达。`skip-proxy` 的系统代理绕过语义也只能近似为靠前的直连规则。需要 URL 改写或 HTTPS 解密的广告过滤不会因此生效。

它是固定快照；上游规则或 Apple News 列表更新后，本文件不会自动同步。命中效果还取决于流量是否进入 v2rayN、DNS 解析策略以及所用核心。

## 来源、校验与许可

| 项目 | 记录 |
| --- | --- |
| 主规则 | [`sr_top500_banlist_ad.conf`](https://github.com/Johnshall/Shadowrocket-ADBlock-Rules-Forever/blob/8be377d6dce85152f7bc7fffe2fda17efbccda94/sr_top500_banlist_ad.conf) |
| 上游提交 | `8be377d6dce85152f7bc7fffe2fda17efbccda94` |
| 主规则 SHA-256 | `4556df5bd9464fb598f70f0810f8b33322f84bb00a88db0fac4c98be2fcafb40` |
| Apple News | [blackmatrix7/ios_rule_script](https://github.com/blackmatrix7/ios_rule_script/tree/master/rule/Shadowrocket/AppleNews)，列表标注更新于 `2026-01-26 02:06:40` |
| 转换 JSON SHA-256 | `88bafb98ad3d5e7de949815a40e442ef9ca65bb8daf0283c6e2816d14e6b9745` |

转换文件经过顺序比对：89,946 条具体条件与 1 条最终直连均有对应结果，没有遗漏、重排或因转换新增的重复条件。原规则本身可能包含重复项；转换时予以保留。

上游 Johnshall 仓库采用 [Creative Commons Attribution-ShareAlike 4.0 International（CC BY-SA 4.0）](https://creativecommons.org/licenses/by-sa/4.0/) 许可。本仓库转换规则继续按相同许可分享；使用和再分发时请保留上游署名、来源和许可信息。详见 [LICENSE](LICENSE)。

## 常见问题

**这是 v2rayN 节点订阅吗？** 不是。它是路由规则 JSON，需从「路由设置」导入。

**能像 Shadowrocket 一样完成全部去广告吗？** 不能。域名/IP 层面的阻断可以转换，URL Rewrite 与 MITM 不能转换成 v2rayN 路由规则。

**规则会自动更新吗？** 不会。文件名中的日期表示快照日期；需要新版本时应重新转换并验证上游规则。
