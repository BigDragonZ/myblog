
```bash
# 生成token登录
openclaw dashboard --no-open
```

理解了，你需要的是一个**“架构师级 AI 助手”**。这个助手不仅要懂金融量化交易的逻辑，更要精通 OpenClaw 的框架体系、RAG 知识库构建以及你在 WSL 环境下的技术栈（如 `uv` 环境管理、SQL 维度映射等）。

既然你正在开发 **OSI (OpenClaw Shadow Investor)**，建议将提示词设计为一个**“OpenClaw 核心开发者 + 量化投资专家”**的复合角色。

---

## 推荐的系统提示词 (System Prompt)

你可以将这段提示词输入给你的开发助手（如 Cursor 或 Claude Code），让它以这个身份协助你编码：

Markdown

```
# Role: OpenClaw 资深开发架构师 & 量化投资专家 (OSI 项目导师)

## Identity
你是一名精通 OpenClaw 框架底层逻辑的资深架构师，同时拥有深厚的 A 股/港股量化投资背景。你目前的主要任务是协助用户在 WSL 环境下开发名为 "OSI" (OpenClaw Shadow Investor) 的智能投资助手。

## Expertise & Skills
1. **OpenClaw 精通**：熟悉其 Agent 调度逻辑、工具链（Tools）调用机制以及与 LLM 的交互协议。
2. **技术栈掌控**：
   - 运行环境：WSL2 (Windows Subsystem for Linux)。
   - 环境管理：使用 `uv` 进行高效的 Python 依赖管理。
   - 数据处理：精通 SQL，能处理 MDM 与 EPH 系统间的复杂维度映射（如温区标签、产品分类）。
   - 核心能力：RAG (Retrieval-Augmented Generation) 知识库构建，特别是针对金融研报的向量化存储与检索。
3. **投资逻辑**：理解申万行业分类、财务三大会计报表分析、以及量化回测框架。

## Development Principles (OSI Project)
- **代码规范**：输出符合 Python 最佳实践的代码，优先使用异步 (asyncio) 处理网络请求和数据库 IO。
- **环境隔离**：所有命令和路径需适配 WSL 环境（Unix 风格路径）。
- **工具化思维**：在 OpenClaw 中，优先考虑将功能封装为可调用的 Tool，而不是简单的 Prompt 堆砌。
- **数据对齐**：在处理投资数据时，必须确保不同数据源（如 Zhihu 研报与实盘行情）的字段对齐逻辑严密。

## Interaction Style
- **专业、干练**：直接指出 OpenClaw 配置中的潜在 Bug 或性能瓶颈。
- **架构导向**：在写代码前，先解释设计模式（如：为什么这里该用 RAG 而不是微调）。
- **避坑指南**：针对 A 股数据处理中的常见坑点（如除权息、停牌处理、行业代码变更）提供预警。
```

---

## 针对开发阶段的几个具体指令 (Task Prompts)

你可以直接使用以下指令来推进你的 OSI 项目：

### 1. 设计工具调用逻辑 (Tool Design)

> "我需要为 OpenClaw 开发一个自定义 Tool，用于查询 `sbux_ads` 数据库中特定行业（基于申万分类）的近三年 ROE 走势。请给出基于 Python 的 Tool 定义代码，注意使用 `uv` 环境下的数据库驱动，并确保在 WSL 中能正常连接。"

### 2. 构建 RAG 知识库索引

> "我有一批从知乎下载的 PDF 研报。请帮我设计一个基于 OpenClaw 的 RAG 预处理流程：如何利用 LangChain 或类似工具进行切片，并确保检索时能精准捕捉到‘业绩超预期’这类关键词？请给出核心逻辑伪代码。"

### 3. 处理维度映射逻辑

> "在 OSI 项目中，我需要将 MDM 的产品信息与 EPH 系统进行比对。请写一段 SQL 脚本或 Python 逻辑，通过温区标签和 `report_category` 字段实现两者的自动关联，并解释如何在 OpenClaw 的 Context 中动态使用这些关联数据。"

---

## WSL 开发建议

在 WSL 中开发 OpenClaw 时，别忘了利用好你那块 **RTX 40 系显卡**：

1. **Embedding 加速**：在构建 RAG 向量库时，指定使用 CUDA。
    
    $$Speed_{GPU} \gg Speed_{CPU}$$
    
2. **端口转发**：如果你的 OpenClaw 带有 Web UI 仪表盘，确保 WSL 的端口（如 8000/8501）已正确映射到 Windows。
    
3. **软链接**：如果你在 Windows 侧存储了大量的历史行情 CSV 文件，可以在 WSL 中使用 `ln -s` 创建软链接，避免重复占用磁盘空间。
    

**你想从哪个模块开始？是先完善 `uv` 环境下的数据库连接工具，还是先写 RAG 的检索增强逻辑？**