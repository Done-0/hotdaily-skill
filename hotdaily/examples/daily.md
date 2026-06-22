# 示例对话：获取每日精选

## 场景：用户想了解今天有什么值得读的技术文章

---

**用户**: `/hotdaily 今天有什么值得读的？`

**Agent**:
> 正在获取今日日报...

*[调用 `curl -s https://api.hotdaily.top/v1/digests/today`]*

> **HotDaily 每日晚报 · 6月22日**
> 
> AI 替你读过 156 篇，精选 30 篇：
> 
> ### 精读推荐 (6 篇)
> 
> 1. **Anthropic's new interpretability research reveals neuron behaviors**
>    - 为什么值得读：AI 安全性突破，首次实现大规模神经元行为解释，对理解模型决策过程有重大意义
>    - 阅读时长：8 分钟
>    - [阅读原文](https://...)
> 
> 2. **PostgreSQL 17 Beta Released with Major Performance Improvements**
>    - 为什么值得读：性能提升 40%，新增分布式事务支持，生产环境值得关注的重大更新
>    - 阅读时长：5 分钟
>    - [阅读原文](https://...)
> 
> 3. **Inside Meta's new code generation model**
>    - 为什么值得读：技术细节扎实，介绍训练方法和评估基准，对 AI 工程师有实践参考价值
>    - 阅读时长：12 分钟
>    - [阅读原文](https://...)
> 
> *(另外 3 篇精读推荐...)*
> 
> ### 略读推荐 (24 篇)
> 
> - **GitHub Copilot Workspace launches**: 从需求直接生成 PR，适合快速了解新工具
> - **Rust 1.79 release notes**: 常规版本更新，稳定性改进
> - **New JS framework benchmarks**: 性能对比数据，选型时参考
> 
> *(另外 21 篇略读推荐...)*
> 
> 另有 15 篇也达标但未入选前 30，可访问 https://hotdaily.top/more 查看。

---

**用户**: 给我看第一篇的详细信息

**Agent**:
> 正在获取详情...

*[调用 `/v1/items/{id}`]*

> **Anthropic's new interpretability research reveals neuron behaviors**
> 
> **完整摘要**：
> Anthropic 发布最新可解释性研究，通过分析 Claude 3 模型的数百万神经元，首次实现了对大规模语言模型内部机制的系统性理解。研究团队开发了自动化工具来识别和分类神经元功能，发现模型在处理不同类型输入时会激活特定的神经元模式。这项研究不仅有助于提升 AI 安全性，还为模型调试和优化提供了新思路。论文详细介绍了实验方法、发现的神经元类型分布，以及如何利用这些发现改进模型行为。
> 
> **为什么值得读**：
> AI 安全性突破，首次实现大规模神经元行为解释，对理解模型决策过程有重大意义。研究方法可复现，对从事 AI 安全和可解释性研究的团队有直接参考价值。
> 
> **社区反响**：
> - Hacker News: 342 分，78 条讨论
> - 类型：研究论文
> 
> **阅读时长**：8 分钟
> 
> [阅读原文](https://...)

---

## 变体对话

**用户**: 有没有关于数据库的文章？

**Agent**: 
> 在今日精选中找到 2 篇数据库相关：
> 
> 1. **PostgreSQL 17 Beta Released** (精读) - 性能提升 40%...
> 2. **DuckDB 1.0 reaches production readiness** (略读) - 嵌入式分析数据库...

---

**用户**: 都是英文的吗？有中文版吗？

**Agent**:
> HotDaily 的呈现方式：
> - **标题**：保留英文原题（方便搜索原文）
> - **摘要和理由**：提供中文翻译
> - **正文**：部分文章有完整中文摘要，点击详情可查看
> 
> 所有内容都经过 AI 阅读和价值评估，即使不看原文，通过中文摘要和理由也能快速判断是否值得深入阅读。
