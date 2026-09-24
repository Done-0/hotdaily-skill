---
name: hotdaily
description: Use when the user asks about tech trends, startup news, daily tech articles, startup opportunities, or wants to access HotDaily's AI-curated content, trend analysis, and opportunity mining
---

当用户询问科技趋势、创业动态、产品与创业机会、或想了解最新技术文章时，使用本 skill 从 HotDaily API 获取数据。

# 核心能力

HotDaily 是一个由 AI 驱动的科技与创业精选日报，每天从 Hacker News、Lobsters 等社区筛选内容，经过 AI 阅读、价值评估后呈现给读者；并从每天读过的真实语料里挖掘产品与创业机会。

## 可用端点

### 日报相关

**今日日报**
`GET https://api.hotdaily.top/v1/digests/today`

返回今日最新一期日报（早报/午报/晚报），包含 AI 替读者读过的所有文章、价值判断（精读/略读）、中文摘要、阅读时长。

**指定日期日报**
`GET https://api.hotdaily.top/v1/digests/{date}`

获取历史某天的日报，日期格式：`YYYY-MM-DD`，例如 `2026-09-22`。

**日报归档列表**
`GET https://api.hotdaily.top/v1/digests`

返回所有历史日报的日期列表，按倒序排列。

**更多精选（溢出页）**
`GET https://api.hotdaily.top/v1/more?page={N}`

当日达标但未入选前 30 的好文，分页获取。`page` 默认 1。

### 趋势相关

**今日趋势**
`GET https://api.hotdaily.top/v1/trends/today`

返回当日技术趋势信号、强度曲线、证据条目。每个趋势包含：标题、摘要、为什么重要、状态（new/heating/sustained/cooling）、置信度、强度分数、证据来源。

**周报/月报**
`GET https://api.hotdaily.top/v1/trends?window={window}&date={date}`

- `window`: `today` | `week` | `month`
- `date`: 锚点日期（YYYY-MM-DD），可选，默认最新

周报每周一生成（聚合过去 7 天信号），月报每月 1 号生成（聚合过去 30 天信号）。

**趋势归档列表**
`GET https://api.hotdaily.top/v1/trends/archive`

返回所有趋势报告（日报/周报/月报）的日期和窗口列表。

### 机会相关

**机会时间线（雷达）**
`GET https://api.hotdaily.top/v1/opportunities/radar?window={window}&limit={N}`

返回最近 N 条机会卡片，按日期倒序。每张卡：问题标题、问题定义、空白分析、置信度、证据数与证据列表。

- `window`: `today` | `week` | `month`，可选，缺省全部窗口
- `limit`: 返回条数，可选

**指定日期机会研究**
`GET https://api.hotdaily.top/v1/opportunities?window={window}&date={date}`

返回某天某窗口的完整机会研究卡片。`window` 必填（`today` | `week` | `month`）；`date` 可选（锚点日期，默认该窗口最新一期）。

**单条机会详情**
`GET https://api.hotdaily.top/v1/opportunities/{id}`

按 id 获取单条机会的完整研究：问题、现有方案、空白、业务流程、验收标准、技术要求、验证计划、风险、证据。

**生成机制**：AI 每天从已读语料里挖机会，有真机会就发、没有就不发（宁缺毋滥）——没挖到的日期没有卡片，属正常现象。日→周→月逐级生成，历史日期由回填自动补齐。

### 搜索

**全文搜索**
`GET https://api.hotdaily.top/v1/search?q={query}&page={N}&size={N}`

搜索已读语料（标题 + 正文摘录），返回命中条目列表。`q` 必填；`page` 默认 1；`size` 默认 10、上限 30。

### 条目详情

**单篇文章详情**
`GET https://api.hotdaily.top/v1/items/{id}`

获取单篇文章的完整信息：标题、URL、价值理由、完整摘要、审核信息、阅读时长、正文（如果可渲染）。

# 常见返回字段

## 日报接口常见字段

```json
{
  "date": "2026-09-22",
  "edition": "晚报",
  "serverDate": "2026-09-22",
  "readCount": 156,
  "selectedCount": 30,
  "qualifiedCount": 45,
  "items": [
    {
      "id": "0a831d51-33eb-4963-931c-240d82ceba84",
      "title": "Article Title in English",
      "url": "https://example.com/article",
      "valueLight": "green",
      "reason": "为什么值得读的中文理由",
      "valueVerdict": "一句中文判词",
      "summaryZh": "中文摘要",
      "readingMinutes": 5,
      "source": "hacker-news",
      "signals": {
        "points": 342,
        "numComments": 78
      },
      "typeTag": "research"
    }
  ]
}
```

**字段说明**：
- `valueLight`: `green` = 精读（深度价值），`yellow` = 略读（快速扫一眼）
- `readCount`: AI 替读者读过的文章总数
- `selectedCount`: 日报精选数量
- `qualifiedCount`: 达标总数（包含溢出到 /more 的）
- `reason`: 中文价值理由
- `valueVerdict`: 面向读者的一句中文判词，可能为 `null`
- `summaryZh`: 一句话中文摘要，可能为 `null`

## 趋势接口常见字段

```json
{
  "window": "today",
  "anchorDate": "2026-09-22",
  "generatedAt": 1782072932123,
  "trends": [
    {
      "id": "trend:today:abc",
      "title": "AI 模型路由成为基础设施新热点",
      "summary": "多家公司推出模型路由解决方案，自动选择最优模型处理请求",
      "whyItMatters": "降低成本同时提升响应质量，成为 AI 应用的关键中间层",
      "status": "heating",
      "score": 91,
      "confidence": "high",
      "sparkline": [3, 5, 7, 9],
      "evidence": [
        {
          "itemId": "...",
          "title": "OpenRouter launches new routing algorithm",
          "source": "hacker-news",
          "rank": 1
        }
      ]
    }
  ]
}
```

**字段说明**：
- `status`: `new`（新出现）/ `heating`（升温）/ `sustained`（持续）/ `cooling`（降温）
- `confidence`: `high` / `medium` / `low`
- `score`: 趋势强度分数
- `sparkline`: 强度曲线数值数组，用于可视化趋势强度变化

## 机会接口常见字段

```json
{
  "opportunities": [
    {
      "id": "3581c161-5efc-4355-9a2d-6a903d7e8e7d",
      "window": "today",
      "anchorDate": "2026-09-23",
      "problem": "独立开发者做模型选型时算不清实际成本，只能按单价估",
      "problemDefinition": "新模型几乎每月发布一批，选型的人每次都要重算一遍。",
      "gapAnalysis": "现有评测只报标准化基准数据，没人按你的真实任务样本重放。",
      "riskNotes": "评测站点加官方重放功能后空白消失",
      "existingSolutions": [
        {
          "name": "Artificial Analysis",
          "url": "https://artificialanalysis.ai",
          "pros": "指标全、更新快",
          "cons": "只有标准化基准，不测真实任务"
        }
      ],
      "businessProcess": ["选 10 条真实任务", "逐模型重放", "记录消耗与通过率", "汇总成对比表"],
      "acceptanceCriteria": [
        { "criteria": "对比表可用", "metric": "模型数", "target": "≥5 个主流模型" }
      ],
      "techRequirements": [
        { "capability": "多模型并发调用", "option": "OpenAI 兼容网关", "rationale": "一次跑完所有候选模型" }
      ],
      "validationPlan": [
        { "milestone": "手工对比表", "metric": "耗时", "success": "2 天内出第一版" }
      ],
      "confidence": "high",
      "evidenceCount": 4,
      "evidence": [
        {
          "itemId": "...",
          "title": "GPT-5 pricing makes small models attractive",
          "source": "hacker-news",
          "url": "https://news.ycombinator.com/item?id=...",
          "points": 156,
          "valueLight": "green",
          "valueVerdict": "成本结构变化的一手讨论",
          "summaryZh": "讨论各模型实际成本差异"
        }
      ],
      "generatedAt": 1790132000000
    }
  ]
}
```

**字段说明**：
- `problem`: 一句话问题标题（15-30 个中文字符）
- `problemDefinition`: 问题定义，2-3 句（谁疼、多久疼一次、现在怎么忍）
- `existingSolutions`: 现有方案（最多 3 个），每个含 `name`/`url`/`pros`/`cons`——`cons` 是空白分析的依据
- `gapAnalysis`: 空白——现有方案共同没覆盖的那个具体位置（1-2 句）
- `businessProcess`: 一步步怎么做（3-5 步，每步 8-12 个字）
- `acceptanceCriteria`: 怎么算做成了（2-3 条，指标能算出来）
- `techRequirements`: 落地需要的技术能力（2-3 项）
- `validationPlan`: 先做最小验证（2 个里程碑）
- `riskNotes`: 这个机会可能为什么不成立（1-2 句）
- `confidence`: `high`（高把握，绿）/ `medium`（中把握，琥珀）/ `low`（低把握，红）——模型对自身研究的置信度
- `evidenceCount` + `evidence`: 证据条数与证据列表（链回 `/item/{itemId}` 看原文）
- `anchorDate`: 机会锚定的日期；`window`: `today` | `week` | `month`

## 条目详情接口常见字段

```json
{
  "id": "...",
  "title": "Article Title",
  "url": "https://...",
  "valueLight": "green",
  "valueReason": "价值理由",
  "summaryTriageZh": "简短摘要",
  "summaryDetailZh": "详细摘要",
  "readingMinutes": 5,
  "bodyKind": "full",
  "content": {
    "markdown": "# 正文\n\n...",
    "charCount": 5234
  },
  "review": {
    "quality": "ok",
    "compliance": "pass",
    "note": ""
  }
}
```

**`bodyKind` 说明**：
- `full`: 完整正文可渲染
- `native`: 原生内容（论坛帖子等）
- `pdf`: PDF 文档（已多模态读取）
- `abstract`: 学术论文摘要
- `summary`: 仅摘要
- `none`: 无正文

用户侧理解方式：

- 列表页主要看：`title`、`valueLight`、`reason`、`valueVerdict`、`summaryZh`
- 趋势页主要看：`title`、`summary`、`whyItMatters`、`status`、`evidence`
- 机会页主要看：`problem`、`problemDefinition`、`gapAnalysis`、`confidence`、`evidence`
- 详情页主要看：`valueReason`、`summaryTriageZh`、`summaryDetailZh`、`content`

# 使用模式

## 1. 用户问"今天有什么值得读的"

```bash
curl -s https://api.hotdaily.top/v1/digests/today
```

**呈现建议**：
- 按 `valueLight` 分组（精读 vs 略读）
- 每篇给出：标题、理由、阅读时长、链接
- 提示"AI 替你读过 {readCount} 篇，精选 {selectedCount} 篇"

## 2. 用户问"最近技术趋势"

```bash
curl -s https://api.hotdaily.top/v1/trends/today
```

**呈现建议**：
- 每个趋势给出：标题、摘要、为什么重要
- 标注状态（🆕 新出现 / 🔥 升温 / 🔄 持续 / ❄️ 降温）
- 列出主要证据来源（2-3 个代表性条目）

## 3. 用户问"最近有什么产品/创业机会"

```bash
curl -s "https://api.hotdaily.top/v1/opportunities/radar?limit=10"
```

**呈现建议**：
- 每条机会给出：问题（`problem`）、问题定义（`problemDefinition`）、空白（`gapAnalysis`）
- 标注置信度（`confidence`）：`high=高把握` / `medium=中把握` / `low=低把握`
- 列出证据来源（2-3 个代表性条目）
- 提示生成机制：有真机会就发、没有就不发——某天没卡片是正常的

## 4. 用户要深挖某条机会

```bash
curl -s "https://api.hotdaily.top/v1/opportunities/{id}"
```

**呈现建议**：
- 按 `problem` → `problemDefinition` → `existingSolutions`（pros/cons）→ `gapAnalysis` → `businessProcess` → `validationPlan` → `riskNotes` 顺序讲
- 验收标准（`acceptanceCriteria`）与技术要求（`techRequirements`）单独列出
- 证据链回原文：每条证据有 `itemId`，可调 `/v1/items/{itemId}` 看完整上下文

## 5. 用户问"AI 基础设施最近怎么样"

```bash
curl -s "https://api.hotdaily.top/v1/trends?window=week"
```

按标题、摘要、`whyItMatters` 与证据条目判断哪些趋势属于 AI / 基础设施方向，再总结演变方向。

## 6. 用户要搜某个话题

```bash
curl -s "https://api.hotdaily.top/v1/search?q=模型路由"
```

返回命中条目（标题 + 中文摘要 + 价值灯），按 `total` 与分页翻页。

## 7. 用户要某天的日报

```bash
curl -s https://api.hotdaily.top/v1/digests/2026-09-20
```

## 8. 用户要某篇文章的详情

```bash
curl -s https://api.hotdaily.top/v1/items/{id}
```

展示完整摘要、价值分析、原文链接；如果 `bodyKind` 是 `full` 或 `native`，可以展示正文摘录。

# 调用示例（给使用接口的人）

如果用户或代理要直接调公开接口，可以这样读：

```bash
# 获取今日日报
curl -s https://api.hotdaily.top/v1/digests/today | jq '.items[] | {title, valueLight, reason}'

# 获取今日趋势
curl -s https://api.hotdaily.top/v1/trends/today | jq '.trends[] | {title, summary, status}'

# 获取最近 10 条机会卡片
curl -s "https://api.hotdaily.top/v1/opportunities/radar?limit=10" | jq '.opportunities[] | {problem, gapAnalysis, confidence}'

# 获取周报
curl -s "https://api.hotdaily.top/v1/trends?window=week" | jq '.trends[0]'

# 搜索
curl -s "https://api.hotdaily.top/v1/search?q=模型路由" | jq '.items[] | {title, summaryZh}'
```

# 使用注意

- **API 无需认证**，直接 GET 访问
- **今日日报和趋势不缓存**，实时更新
- **历史日报、机会、搜索有边缘缓存**，响应快
- **解析 JSON 后按用户意图组织呈现**，不要直接转发原始 JSON
- **日报发布时机**：每天 08:00 / 12:00 / 18:00（北京时间）三次出版（早报/午报/晚报），两次出版之间报头数字恒定
- **趋势生成时机**：每日自动，周一生成周报，每月 1 号生成月报
- **机会生成机制**：AI 每天自动从已读语料里挖，有真机会就发、没有就不发——某天没卡片是正常现象；置信度 `confidence` 反映 AI 对该机会研究的把握程度
- **如果 API 返回 404**，说明该日期尚未出报、趋势或机会尚未生成
- **内容语言**：标题为英文原题，摘要、理由、趋势分析、机会研究均为中文

# 典型用法

**用户**: 今天有什么值得读的？

**助手行为**：
1. 调用 `/v1/digests/today`
2. 按价值灯分类（精读 / 略读）
3. 每篇给出：标题（英文）、理由（中文）、时长、链接
4. 报头总结："AI 替你读过 156 篇，精选 30 篇"

---

**用户**: 最近 AI 领域有什么趋势？

**助手行为**：
1. 调用 `/v1/trends/today`
2. 按标题、摘要、证据判断是否属于 AI 领域
3. 每个趋势给出：标题、摘要、为什么重要、状态（🔥/🔄/❄️）
   如需贴中文标签，建议映射为：`new=新出现`、`heating=升温`、`sustained=持续`、`cooling=降温`
4. 可选：列出 2-3 个代表性证据条目

---

**用户**: 最近有什么值得做的产品/创业机会？

**助手行为**：
1. 调用 `/v1/opportunities/radar?limit=10`
2. 每条机会给出：问题（`problem`）、空白（`gapAnalysis`）、置信度（`confidence` → 高把握/中把握/低把握）
3. 挑 2-3 条展开：问题定义 → 现有方案的 pros/cons → 怎么做（`businessProcess`）→ 怎么算成（`acceptanceCriteria`）
4. 提示：AI 每天从已读语料里挖，有就发、没有就不发

---

**用户**: 给我看本周技术趋势报告

**助手行为**：
1. 调用 `/v1/trends?window=week`
2. 展示所有趋势，按接口返回顺序讲解（通常已按强度排序）
3. 每个趋势注明演变方向（new/heating/sustained/cooling）

---

**用户**: 详细讲讲这篇文章 [提供 ID]

**助手行为**：
1. 调用 `/v1/items/{id}`
2. 展示完整摘要（`summaryDetailZh`）
3. 价值理由（`valueReason`）
4. 如果有正文（`bodyKind` 是 `full`），可摘录关键段落
5. 给出原文链接

# 呈现原则

- 优先给用户中文摘要和中文价值理由，不要直接甩原始 JSON
- 标题通常保留英文原题，便于用户搜索原文
- 用户要"最近有什么趋势"，优先先看 `today`
- 用户要"本周/本月"，再切到 `week` / `month`
- 用户要"最近的机会"，用 `/v1/opportunities/radar`；要深挖某条，再调 `/v1/opportunities/{id}`
- 用户要搜某个话题，用 `/v1/search`；要深挖某篇，再调 `/v1/items/{id}`
