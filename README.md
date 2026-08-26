# Shadowrocket Rule Supplement

用于维护 [blackmatrix7/ios_rule_script](https://github.com/blackmatrix7/ios_rule_script) 的 Shadowrocket 补充规则。

本仓库与上游规则互补，不复制、不替代上游完整规则集，只维护经验证的差异规则。

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
│   └── ClaudeSupplement.list
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

用于补充 blackmatrix7 Claude 规则，覆盖：

- Claude Web 服务域名
- Claude Code 相关服务
- Anthropic 登录/CDN 服务
- 部分缺失的网络出口规则

使用：

```ini
RULE-SET,https://raw.githubusercontent.com/rrxl/shadowrocket-rule-supplement/main/rule/Shadowrocket/Claude/ClaudeSupplement.list,Claude
RULE-SET,https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/Shadowrocket/Claude/Claude.list,Claude
```

## 收录标准

- 上游缺失且有实际命中记录的规则
- 可复现故障补充
- 社区验证待合并规则

不复制完整规则，避免与上游重复维护。

## 注意

IP 规则可能随 CDN 调整失效，请结合抓包和连接日志验证。