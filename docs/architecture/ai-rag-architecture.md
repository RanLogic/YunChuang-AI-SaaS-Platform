# AI & RAG Architecture

## 应用编排与知识检索

**Dify Workflow** 组织 AI 应用流程，已确认用于 Topic Generation 与 Public Content Generation。业务服务负责服务端生成控制，将 AI 能力接入内容生产流程；工作流不替代租户鉴权或业务额度管理。

**Dify Dataset** 承载企业知识库能力，RAG 检索由 Dify 知识库链路承担，并提供检索结果与知识来源引用。Embedding 模型负责文本向量化，不能与检索链路或知识库存储混为一谈。

CloudBase Database / Storage 承担共享业务数据与文件职责；Dify Dataset 承担知识库职责。文件上传、异步处理及 Dify 文件同步具有任务状态管理，与生成请求的 `requestId` 幂等分别处理不同问题。

## 模型与运行服务

| 组成 | 已确认职责 |
| --- | --- |
| DeepSeek | 系统使用的 LLM。 |
| Qwen3.8-Flash | 朋友圈内容生成。 |
| Qwen3-Embedding 0.6B | 文本向量化。 |
| Ollama | 模型运行工具，不是模型本身。 |
| DashScope | 模型服务相关组成，不等同于 LLM 或 Embedding 模型。 |

腾讯云服务器已部署 Docker、Dify、Ollama、Embedding 模型及 Nginx。仅凭此部署事实不能确定各模型的服务接入路径，也不表示所有模型均通过 Ollama 或 DashScope 调用。

## 与业务服务的集成

小程序由 CloudBase 云函数承接业务，Platform Admin 由 Next.js Server / API Routes 承接业务。AI 能力通过服务端集成进入产品，租户级配置、访问权限和生成控制仍属于业务服务的责任边界。

RAG 问答、知识文件同步与公开内容生成是不同业务流程。不能将每次内容生成都描述为必经 Dataset 检索，也不能把文件处理状态管理等同于内容生成幂等。

具体模型调用路径、工作流内部节点、检索参数、容器关系及网络拓扑，具体实现细节未在当前 Showcase 仓库核验。

相关说明：[数据流与权限边界](data-flow-and-security.md) · [整体架构](README.md)
