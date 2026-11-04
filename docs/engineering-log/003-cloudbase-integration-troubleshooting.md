# CloudBase Integration & Authentication Troubleshooting

## Background

Platform Admin 通过 Next.js 服务端接入共享 CloudBase 业务环境。该记录描述项目历史上一次 Server SDK 接入故障，不是 CloudBase 所有项目的通用故障说明。

## Problem

最初采用以下初始化方式，在默认 Endpoint 模式下出现 `401 INVALID_CREDENTIALS`：

```javascript
cloudbase.init({
  env,
  secretId,
  secretKey
})
```

## Investigation

根据已确认的项目排查事实，SecretId、SecretKey、CAM 权限和 CloudBase 环境均正确，排除了这些配置错误作为本次故障原因的可能。上述事实不表示其他项目遇到同一报错时也可跳过这些检查。

历史资料未记录四项检查的先后顺序、具体命令或底层认证诊断过程，未核实，不补造日志分析、网络抓包或权限变更操作。

## Solution

显式设置 `endPointMode: "CLOUD_API"` 后，接入问题解决：

```javascript
cloudbase.init({
  env,
  secretId,
  secretKey,
  endPointMode: "CLOUD_API"
})
```

该设置明确选择 SDK 的云 API Endpoint 模式。示例仅保留变量名，不包含真实凭据、环境 ID 或生产地址。

## Result

本项目在确认凭据、权限与环境正确后，通过显式选择 Endpoint 模式解决了当时的服务端接入故障。底层认证差异与 SDK 版本未在当前 Showcase 仓库核验；不将此结果泛化为默认模式普遍失效。

## Lessons Learned

- 认证报错应结合凭据、权限、目标环境和 SDK 接入模式排查，不能仅凭错误名称认定密钥错误。
- 记录已经排除的原因与最终生效的配置，有助于区分事实和底层原因推测。
- SDK 接入成功与业务授权是不同问题，不能替代租户与资源权限校验。

相关资料：[CloudBase 接入决策](../adr/ADR-002-cloudbase-server-integration.md) · [整体技术架构](../architecture/README.md)
