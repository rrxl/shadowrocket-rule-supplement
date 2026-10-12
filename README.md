# Shadowrocket Rule Supplement

用于维护 [blackmatrix7/ios_rule_script](https://github.com/blackmatrix7/ios_rule_script) 的 Shadowrocket 补充规则。

本仓库以维护上游补充规则为主，不复制上游完整规则集；为需要 Claude 强制代理的场景，另提供自包含的 `ClaudeFull.list`。

## 规则优先级

Shadowrocket 按规则顺序匹配，建议：

1. 本仓库补充规则
2. blackmatrix7 主规则
3. 其他规则
4. FINAL

示例：

```ini
RULE-SET,https://raw.githubusercontent.com/rrxl/shadowrocket-rule-supplement/main/rule/Shadowrocket/Netflix/NetflixSupplement.list,Netflix
RULE-SET,https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/Shadowrocket/Netflix/Netflix.list,Netflix
```

## 目录结构

```text
rule/Shadowrocket/
├── Netflix/
│   └── NetflixSupplement.list
├── OpenAI/
│   └── OpenAISupplement.list
├── Claude/
│   ├── ClaudeSupplement.list
│   └── ClaudeFull.list
└── Oyunfor/
    └── OyunforSupplement.list
```

## Oyunfor 规则

Oyunfor 当前主站、登录、静态资源、API、客服等第一方服务均位于 `oyunfor.com` 及其子域名下，因此使用一条 `DOMAIN-SUFFIX` 即可覆盖现有及后续新增的第一方子域名，避免逐个维护 `www`、`livechat`、`cdn`、`assets`、`api` 等主机名。

使用：

```ini
RULE-SET,https://raw.githubusercontent.com/rrxl/shadowrocket-rule-supplement/main/rule/Shadowrocket/Oyunfor/OyunforSupplement.list,Oyunfor
```

如果用于地区分流，建议将 `Oyunfor` 分组固定到同一稳定的土耳其出口，避免登录/会话过程中因出口 IP 或地区变化触发重复重定向。

PAYTR、iyzico/iyzipay、Paycell 等第三方支付域名暂不并入 Oyunfor 规则，以免影响这些支付服务在其他网站上的分流；如有需要可单独建立土耳其支付规则集。

## Claude 规则

2026-10-12 对照 Anthropic 官方网络要求、v2fly 和近期社区规则完成复查。规则覆盖 Claude Web、Desktop、Claude Code、API、MCP / Artifacts 资源，以及经核对的 Anthropic IP / ASN。完整规则已单独提供，原补充规则继续保留。

### 推荐：完整规则集（ClaudeFull.list）

`ClaudeFull.list` 是 **自包含** 规则集，不依赖 blackmatrix7 的旧版 Claude.list。为满足 Claude **只走代理、代理故障不回退直连** 的使用场景，建议在 Shadowrocket 的 `[Rule]` 开头、所有地区直连与 `FINAL` 规则之前加入：

```ini
RULE-SET,https://raw.githubusercontent.com/rrxl/shadowrocket-rule-supplement/main/rule/Shadowrocket/Claude/ClaudeFull.list,PROXY,force-remote-dns
```

`PROXY` 可替换为明确存在的 Claude 专用策略组，但**该组不能包含 DIRECT 或故障时的 DIRECT 回退**。启用 Shadowrocket 的「全局路由：配置」，保持 VPN 运行；代理节点不可用时，匹配的 Claude 流量应连接失败，而不是自动直连。此约束仅在流量仍受 Shadowrocket 接管时成立；手动关闭 VPN、iOS 隧道断线或应用绕过 VPN，不受此规则集保护。请通过 Shadowrocket 连接日志和禁用节点测试实际行为。

规则集只包含匹配条件；**国内 DNS 优先、国内 DNS 失败使用境外 DNS** 应在使用者自己的 `[General]` 中配置 `direct-dns-server` 与 `fallback-dns-server`，不要把 DNS 地址写入 `.list`。

为避免误伤其他网站，没有收录 `storage.googleapis.com`、`registry.npmjs.org`、`github.com` 等 Claude Code 会使用、但并非 Claude 专属的共享域名。

### 兼容旧用法：仅订阅补充规则

仓库原有的 `ClaudeSupplement.list` 只包含 blackmatrix7 上游缺少的条目，**必须与上游 Claude.list 同时订阅**：

```ini
RULE-SET,https://raw.githubusercontent.com/rrxl/shadowrocket-rule-supplement/main/rule/Shadowrocket/Claude/ClaudeSupplement.list,PROXY,force-remote-dns
RULE-SET,https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/Shadowrocket/Claude/Claude.list,PROXY,force-remote-dns
```

两种用法择一即可，避免重复。若希望减少外部依赖并明确防直连策略，优先使用完整规则集。

### 参考资料

- [Anthropic Claude Code 网络要求](https://code.claude.com/docs/en/network-config)
- [Anthropic Desktop 网络要求](https://code.claude.com/docs/en/desktop)
- [Anthropic 官方 IP 列表](https://platform.claude.com/docs/en/api/ip-addresses)
- [Cloudflare Radar：AS399358](https://radar.cloudflare.com/routing/as399358)
- [v2fly Anthropic 域名集](https://github.com/v2fly/domain-list-community/blob/master/data/anthropic)
- [ma-wenqian/ai-proxy-rules](https://github.com/ma-wenqian/ai-proxy-rules)

## 收录标准

- 上游缺失且有实际命中记录的规则
- 可复现故障补充
- 社区验证待合并规则

不复制完整规则，避免与上游重复维护。

## 注意

IP 规则可能随 CDN 调整失效，请结合抓包和连接日志验证。