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
└── Claude/
    └── ClaudeSupplement.list
```

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