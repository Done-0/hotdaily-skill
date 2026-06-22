# 示例对话：查询历史内容

## 场景：用户想查看过去某天的日报或趋势

---

**用户**: `/hotdaily 给我看 6 月 20 号的日报`

**Agent**:
> 正在获取 2026-06-20 日报...

*[调用 `curl -s https://api.hotdaily.top/v1/digests/2026-06-20`]*

> **HotDaily 每日晚报 · 6月20日**
> 
> AI 替你读过 142 篇，精选 30 篇：
> 
> ### 精读推荐 (7 篇)
> 
> 1. **Cloudflare's new Workers AI platform**
>    - 为什么值得读：边缘 AI 推理新方案，延迟优化显著，对分布式应用架构有启发
>    - 阅读时长：6 分钟
> 
> 2. **React 19 RC released with new features**
>    - 为什么值得读：Server Components 稳定、新 Hooks API，生产环境迁移指南详细
>    - 阅读时长：10 分钟
> 
> *(另外 5 篇精读推荐...)*
> 
> ### 略读推荐 (23 篇)
> - **New npm security features**: 供应链安全改进
> - **Kubernetes 1.30 released**: 常规版本更新
> *(另外 21 篇...)*

---

**用户**: 那天有什么趋势？

**Agent**:
> 正在获取 6 月 20 号的趋势...

*[调用 `curl -s https://api.hotdaily.top/v1/trends?window=today&date=2026-06-20`]*

> **6月20日技术趋势**
> 
> 发现 3 个趋势：
> 
> ### 🔥 边缘 AI 推理方案集中涌现 (上升)
> 
> **趋势摘要**：
> Cloudflare、Vercel、Deno 等边缘计算平台在同一周内发布或更新 AI 推理能力，聚焦低延迟场景。主流方案是在边缘节点部署轻量模型或路由到近端推理集群。
> 
> **为什么重要**：
> 边缘 AI 从概念验证进入生产就绪阶段，对实时应用（聊天、推荐）和隐私敏感场景有实际价值。
> 
> **证据来源**：
> 1. Cloudflare Workers AI launch
> 2. Vercel AI Edge Functions
> 3. Deno Deploy ML inference beta
> 
> ---
> 
> *(另外 2 个趋势...)*

---

**用户**: 给我看上周的周报

**Agent**:
> 计算上周的锚点日期...

*上周的周一是 6 月 17 日*

*[调用 `curl -s "https://api.hotdaily.top/v1/trends?window=week&date=2026-06-17"`]*

> **本周技术趋势 (6月10-17日)**
> 
> 聚合过去 7 天的信号，发现 6 个跨日趋势：
> 
> ### 🔥 AI Agents 框架竞争加剧 (上升)
> 
> **演变轨迹**：
> - 6/11: LangChain 0.2 重大更新
> - 6/13: AutoGPT 发布企业版
> - 6/14: Microsoft Semantic Kernel 集成 Azure
> - 6/16: 新开源框架 CrewAI 获关注
> 
> *(详细描述...)*
> 
> ---
> 
> *(另外 5 个跨日趋势...)*

---

**用户**: 最近一个月的月报呢？

**Agent**:
> 正在获取最新月报...

*月报在每月 1 号生成，最近的是 6 月 1 号（聚合 5/1-5/31）*

*[调用 `curl -s "https://api.hotdaily.top/v1/trends?window=month&date=2026-06-01"`]*

> **5月技术趋势月报**
> 
> 聚合过去 30 天的信号，发现 8 个持续趋势：
> 
> ### 🔥 开源 LLM 性能持续逼近商业模型 (上升 → 持续)
> 
> **月度演变**：
> - Llama 3 发布后持续优化
> - Mistral 推出多个垂直领域变体
> - 社区微调方案成熟度提升
> - 推理效率工具链完善
> 
> **趋势摘要**：
> 整个 5 月，开源模型在质量、效率、易用性三个维度都有显著进展。质量上，70B 参数模型在多项基准上接近 GPT-4 水平；效率上，量化和蒸馏技术使部署成本大幅降低；易用性上，Ollama、LM Studio 等工具降低本地部署门槛。
> 
> **为什么重要**：
> 开源模型从"能用"进入"好用"阶段，对成本敏感或隐私要求高的场景，开源方案已成为可行选择。这可能重塑 AI 应用的成本结构和供应链。
> 
> *(另外 7 个月度趋势...)*

---

## 变体对话

**用户**: 能搜索某个主题的历史文章吗？

**Agent**:
> HotDaily API 目前**不支持全文搜索**，但可以：
> 
> 1. **按日期浏览**：`/hotdaily 给我看 6 月 X 号的日报`
> 2. **查看归档列表**：访问 https://hotdaily.top/archive
> 3. **通过趋势找相关文章**：趋势报告会关联证据条目
> 
> 例如，如果想找"数据库"相关内容：
> - 先看趋势报告，找是否有数据库趋势
> - 再从趋势的证据来源追溯到具体文章
> 
> 或者直接访问网站，用浏览器搜索功能查找。

---

**用户**: 有 API 文档吗？我想自己调用

**Agent**:
> HotDaily 所有 API 端点：
> 
> **日报**：
> - `GET /v1/digests/today` - 今日/最近一期
> - `GET /v1/digests/{date}` - 指定日期（YYYY-MM-DD）
> - `GET /v1/digests` - 归档列表
> - `GET /v1/more?page=N` - 溢出页（达标但未入选的）
> 
> **趋势**：
> - `GET /v1/trends/today` - 今日趋势
> - `GET /v1/trends?window={today|week|month}&date={date}` - 指定窗口和日期
> - `GET /v1/trends/archive` - 趋势归档列表
> 
> **条目**：
> - `GET /v1/items/{id}` - 单篇详情
> 
> 基础 URL：`https://api.hotdaily.top`
> 
> 无需认证，直接 GET 访问。响应格式都是 JSON。
> 
> 详细字段说明见：https://github.com/Done-0/hotdaily-skill/blob/main/hotdaily.md

---

**用户**: 能订阅吗？每天自动推送？

**Agent**:
> HotDaily 目前提供：
> 
> 1. **RSS 订阅**：https://hotdaily.top/rss.xml
>    - 每日更新，包含当天精选
>    - 可用 Feedly、Inoreader 等阅读器订阅
> 
> 2. **网站访问**：https://hotdaily.top
>    - 每天 7:00 / 12:00 / 18:00 (北京时间) 更新
> 
> 3. **API 集成**：如果你想自动化，可以写个脚本定时调用 `/v1/digests/today`
> 
> 暂时没有邮件推送或 Webhook，但 RSS + 脚本基本能覆盖订阅需求。
