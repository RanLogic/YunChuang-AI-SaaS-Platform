# ADR-002: CloudBase Server-side Integration

- Status: Accepted
- Date: Not specified

## Context

Platform Admin 需要在 Next.js 服务端访问共享 CloudBase 业务环境。根据已确认的项目排查记录，最初使用以下 Server SDK 初始化方式，在默认 Endpoint 模式下出现 `401 INVALID_CREDENTIALS`：

```javascript
cloudbase.init({
  env,
  secretId,
  secretKey
})
```

## Decision

显式指定 `endPointMode: "CLOUD_API"`，采用以下初始化方式，解决了该项目当时的接入问题：

```javascript
cloudbase.init({
  env,
  secretId,
  secretKey,
  endPointMode: "CLOUD_API"
})
```

`CLOUD_API` 在此处的作用是显式选择 SDK 的云 API Endpoint 模式，而非沿用默认模式。项目历史确认了默认模式报错与显式切换后接入成功的结果；未记录更底层的认证失败原因，不将其归因于密钥内容错误或其他未经确认因素。

示例仅包含变量名，不包含真实凭据、环境 ID 或服务地址。

## Alternatives Considered

实际尝试过未指定 `endPointMode` 的默认初始化方式，出现上述错误后未继续采用。历史资料未记录其他正式候选方案。

## Consequences

- Platform Admin 服务端通过明确的 SDK 接入模式访问共享 CloudBase 环境，解决当时阻塞服务端集成的认证报错。
- 该配置解决的是此项目的特定接入问题，不代表 CloudBase 默认模式或其他认证模式普遍不可用。
- 接入成功不替代业务权限校验，也不改变管理员 SQLite 认证与 CloudBase 业务数据之间的独立边界。SDK 版本及底层 Endpoint 认证差异未在当前 Showcase 仓库核验。

## References

- [整体技术架构](../architecture/README.md)
- [数据流与权限边界](../architecture/data-flow-and-security.md)
