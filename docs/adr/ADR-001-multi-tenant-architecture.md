# ADR-001: Multi-Tenant Architecture

- Status: Accepted
- Date: Not specified

## Context

产品面向多个企业销售团队，需要在同一 SaaS 产品中支持各企业的知识、内容与 AI 配置。两个业务入口使用共享 CloudBase 环境，因此共享业务基础设施的同时，需要明确企业之间的资源与权限边界。

## Decision

采用多租户 SaaS 架构，以 Tenant 表达企业业务边界，以 `tenant_id` 作为业务数据隔离维度，将资源访问与租户范围关联，支持共享环境下的企业级业务组织。

服务端结合可信用户身份、租户成员关系、Tenant 状态与权限校验决定访问范围。客户端传入的租户标识只能作为请求输入，不能证明用户属于该租户或有权访问其资源。微信身份由服务端解析；Platform Admin 的独立管理员认证不与微信身份混为一套体系。

租户级 AI 配置与知识、内容等业务资源遵循同一隔离原则：配置选择与资源使用必须处于获授权的租户范围内。此决策不意味着每租户独立数据库、模型部署或 Dify 实例。

## Alternatives Considered

历史资料未记录其他正式候选方案；不将单租户部署或其他隔离模型描述为曾经完成评估的方案。

## Consequences

- 共享产品与业务环境可服务多个企业，租户级配置支持企业之间的使用差异。
- 租户边界需要贯穿服务端业务处理、数据访问及 AI 资源选择，不能只由界面或客户端参数维持。
- 共享环境本身不自动保证隔离，新增业务访问也需要遵守同一授权边界。具体成员存储、角色与查询约束未在当前 Showcase 仓库核验。

## References

- [SaaS 多租户架构](../architecture/multi-tenant-architecture.md)
- [数据流与权限边界](../architecture/data-flow-and-security.md)
