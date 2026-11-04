# Public Content Generation & Idempotency

## Background

Stage9 将行业洞察转化为可编辑、可发布、可分享的内容，并将匿名访问数据回流至企业分析。完整业务链路为：

> Industry Insights → Topic Generation → Public Content Generation → Cover Generation → Content Editing → Publishing → Public Page → WeChat Sharing → Anonymous Event Tracking → Enterprise Analytics

## Problem

生成请求可能重复提交，内容还需要经历编辑与发布；因此请求幂等、额度管理、内容保存和公开访问需要具有各自的业务边界。生成成功本身不足以代表分享与统计闭环完成。

## Investigation

项目已确认 Original / Current 双模型、`requestId` 幂等、服务端 quota 管理、内容持久化、发布授权和公开读取边界。现有记录未提供具体重复请求样例、逐次调试过程或额度扣减时序，未核实，不补造历史排查步骤。

源码确认用于说明实现，测试代码用于说明覆盖场景，用户生产验收用于说明实际运行结果；当前 Showcase 不包含商业源码和完整测试报告，不能据此宣称所有测试通过。

## Solution

- **内容模型**：Original / Current 区分原始内容与当前内容，为生成与后续编辑提供清晰的内容表达；不推定具体数据库结构或版本历史机制。
- **请求与额度**：使用 `requestId` 处理重复生成请求，由服务端管理 quota。计量与额度管理不等于预占配额，不将事后计量描述为预占，也不推定扣减、回退或并发事务策略。
- **业务边界**：生成由服务端控制，编辑衔接当前内容及持久化，发布涉及授权；公开页面按公开读取边界提供访问。
- **分享与统计**：微信分享后的匿名事件回流至企业分析，支持 PV / UV 与分享统计；不补写未经确认的分享回调机制。

Topic Generation、正文生成、封面生成、编辑发布与统计分别属于不同业务环节，不将整个链路描述为同一种 AI 调用或每步必经 RAG 检索。

## Result

用户已确认 Stage9 完成生产闭环、朋友圈分享完整回流通过验收，以及生产定时任务持续运行通过验收。这些是已完成的生产验收结果，不是待验证事项；但不代表每个定时任务的职责、调度表达式或执行性能均已公开核验。

没有可引用的调用耗时、性能或成本指标，本记录不作量化效果声明。

## Lessons Learned

- 幂等、额度、内容模型与发布权限需要分别表达，不能由一个生成接口概括全部业务行为。
- 验收需要覆盖公开页面、分享和数据回流，而不止检查 AI 是否返回正文。
- 源码、测试覆盖与生产验收属于不同证据，不互相替代。

相关资料：[项目业务闭环](../../README.md) · [数据流与权限边界](../architecture/data-flow-and-security.md) · [Dify 集成决策](../adr/ADR-003-dify-ai-integration.md)
