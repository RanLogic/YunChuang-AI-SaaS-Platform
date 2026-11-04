# ADR-003: Dify AI Integration Architecture

- Status: Accepted
- Date: Not specified

## Context

产品需要将企业知识检索、选题与内容生成接入销售业务，同时支持不同租户的 AI 配置和资源边界。AI 应用编排与业务身份、权限及数据管理具有不同职责，需要分别组织。

## Decision

集成 Dify 作为 AI 应用编排与知识库能力组成：Dify Workflow 组织 Topic Generation、Public Content Generation；Dify Dataset 承担企业知识库与 RAG 检索链路，提供检索结果及知识来源引用。

按 `tenant_id` 管理租户级 Dify 配置及 AI 资源，凭据采用引用方式管理。禁止跨租户配置 fallback：不能在某租户配置不可用时，转而使用另一租户的配置或资源。凭据引用的存储、解析及具体故障处理方式未在当前 Showcase 仓库核验。

业务服务负责可信身份、租户状态、权限与业务生成控制；Dify 负责 AI 工作流及知识检索能力，不承担全部业务授权。固定 Prompt 或单次 LLM 调用不被描述为自主 Agent，也不推定工作流内部节点或模型调用路径。

## Alternatives Considered

历史资料未记录其他正式候选方案；不将自建编排系统或其他 AI 平台描述为曾经评估的替代方案。

## Consequences

- 企业知识检索与 AI 生成具有清晰的应用层职责，业务服务可以围绕具体销售流程组织集成。
- 租户配置、凭据引用和禁止跨租户 fallback 的原则共同约束 AI 资源选择，避免以可用性为由突破租户边界。
- AI 编排平台的接入不消除业务服务的鉴权责任；配置缺失或失效时仍需遵守租户边界。具体工作流、资源映射及凭据管理实现未在当前 Showcase 仓库核验。

## References

- [AI / RAG 架构](../architecture/ai-rag-architecture.md)
- [SaaS 多租户架构](../architecture/multi-tenant-architecture.md)
- [数据流与权限边界](../architecture/data-flow-and-security.md)
