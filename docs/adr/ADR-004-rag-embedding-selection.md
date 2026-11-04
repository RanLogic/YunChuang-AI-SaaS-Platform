# ADR-004: RAG & Embedding Technology Selection

- Status: Accepted
- Date: Not specified

## Context

企业知识问答需要知识接入、文本向量化与检索能力。项目已确认曾在 Mac 本地通过 Ollama 部署 Embedding 模型并跑通相关链路，后续迁移线上时保留了这一实现方式。

这一实践构成本地验证到线上迁移的选择依据；没有已确认的候选模型对比、性能测试、准确率或成本数据。

## Decision

企业知识库通过 Dify Dataset 接入，由 Dify 知识库链路承担 RAG 检索，Qwen3-Embedding 0.6B 用于文本向量化。Ollama 是模型运行工具，Embedding 模型本身与运行工具分别承担不同职责。

从 Mac 本地迁移线上时保留已经跑通的 Ollama 部署 Embedding 模型的方式，延续已验证的技术路径，不将这次迁移描述为重新开发检索或向量存储系统。腾讯云服务器部署 Embedding 模型与 Ollama 的事实不用于推定具体容器或网络关系。

DeepSeek 与 Qwen3.8-Flash 是系统使用的生成模型，其中 Qwen3.8-Flash 用于朋友圈内容生成；它们的生成职责与 Embedding 文本向量化职责分开。DashScope 是模型服务相关组成，不据此推定它或 Ollama 与上述生成模型的具体调用关系。

## Alternatives Considered

历史资料未记录其他正式候选方案；不补写模型排名、向量数据库选型比较或未确认的服务替代历史。

## Consequences

- 知识接入、检索、文本向量化及模型运行职责清晰，本地已跑通的实现方式在线上得到延续。
- 保留部署方式有助于维持迁移前后的技术路径一致，但不代表本地与线上环境完全相同，也不证明两者性能相同。
- 未声称自行开发向量数据库；未确认的底层存储、检索参数、性能与成本指标不作为选型结论。具体实现细节未在当前 Showcase 仓库核验。

## References

- [AI / RAG 架构](../architecture/ai-rag-architecture.md)
- [整体技术架构](../architecture/README.md)
