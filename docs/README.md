# Engineering Documentation

本目录用于存放项目工程文档，按以下类别组织：

| 类别 | 用途 | 当前状态 |
| --- | --- | --- |
| [Architecture](architecture/README.md) | 系统架构文档 | Step 2 已完成 |
| ADR | 关键技术决策记录 | Step 3 已完成 |
| [Engineering Log](engineering-log/) | 真实工程实践记录 | Step 4 已完成 |

项目概览见[根目录 README](../README.md)；现有整体架构图见[系统架构 SVG](../architecture/system-architecture.svg)。

## Architecture

- [整体技术架构](architecture/README.md)
- [SaaS 多租户架构](architecture/multi-tenant-architecture.md)
- [AI / RAG 架构](architecture/ai-rag-architecture.md)
- [数据流与权限边界](architecture/data-flow-and-security.md)

## Architecture Decision Records

- [ADR-001: Multi-Tenant Architecture](adr/ADR-001-multi-tenant-architecture.md)
- [ADR-002: CloudBase Server-side Integration](adr/ADR-002-cloudbase-server-integration.md)
- [ADR-003: Dify AI Integration Architecture](adr/ADR-003-dify-ai-integration.md)
- [ADR-004: RAG & Embedding Technology Selection](adr/ADR-004-rag-embedding-selection.md)

## Engineering Logs

- [001: Asynchronous File Processing & State Management](engineering-log/001-asynchronous-file-processing.md)
- [002: Public Content Generation & Idempotency](engineering-log/002-public-content-generation.md)
- [003: CloudBase Integration & Authentication Troubleshooting](engineering-log/003-cloudbase-integration-troubleshooting.md)
