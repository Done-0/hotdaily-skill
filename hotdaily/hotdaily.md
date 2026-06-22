---
name: hotdaily
description: Use when the user asks about tech trends, startup news, daily tech articles, or wants to access HotDaily's AI-curated content and trend analysis
---

当用户询问科技趋势、创业动态、或想了解最新技术文章时，使用本 skill 从 HotDaily API 获取数据。

# 核心能力

HotDaily 是一个由 AI 驱动的科技与创业精选日报，每天从 Hacker News、Lobsters 等社区筛选内容，经过 AI 阅读、价值评估后呈现给读者。

## 可用端点

### 日报相关

**今日日报**  
`GET https://api.hotdaily.top/v1/digests/today`

返回今日最新一期日报（早报/午报/晚报），包含 AI 替读者读过的所有文章、价值判断（精读/略读）、中文摘要、阅读时长。

**指定日期日报**  
`GET https://api.hotdaily.top/v1/digests/{date}`

获取历史某天的日报，日期格式：`YYYY-MM-DD`，例如 `2026-06-22`。

**日报归档列表**  
`GET https://api.hotdaily.top/v1/digests`

返回所有历史日报的日期列表，按倒序排列。

**更多精选（溢出页）**  
`GET https://api.hotdaily.top/v1/more?page={N}`

当日达标但未入选前 30 的好文，分页获取。`page` 默认 1。

### 趋势相关

**今日趋势**  
`GET https://api.hotdaily.top/v1/trends/today`

返回当日技术趋势信号、强度曲线、影响分析、证据条目。每个趋势包含：标题、摘要、为什么重要、状态（rising/sustained/cooling）、置信度、影响力、证据来源。

**周报/月报**  
`GET https://api.hotdaily.top/v1/trends?window={window}&date={date}`

- `window`: `today` | `week` | `month`
- `date`: 锚点日期（YYYY-MM-DD），可选，默认最新

周报每周一生成（聚合过去 7 天信号），月报每月 1 号生成（聚合过去 30 天信号）。

**趋势归档列表**  
`GET https://api.hotdaily.top/v1/trends/archive`

返回所有趋势报告（日报/周报/月报）的日期和窗口列表。

### 条目详情

**单篇文章详情**  
`GET https://api.hotdaily.top/v1/items/{id}`

获取单篇文章的完整信息：标题、URL、价值理由、完整摘要、审核信息、阅读时长、正文（如果可渲染）。

# 响应格式说明

## DigestVO（日报）

```json
{
  "date": "2026-06-22",
  "edition": "晚报",
  "dateLabel": "6 月 22 日",
  "readCount": 156,
  "selectedCount": 30,
  "qualifiedCount": 45,
  "stories": [
    {
      "id": "0a831d51-33eb-4963-931c-240d82ceba84",
      "title": "Article Title in English",
      "url": "https://example.com/article",
      "valueLight": "green",
      "valueReason": "为什么值得读的中文理由",
      "summaryTriageZh": "中文摘要",
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
- `selectedCount`: 日报精选数量（默认 30 篇）
- `qualifiedCount`: 达标总数（包含溢出到 /more 的）
- `typeTag`: `research`（研究论文）/ `launch`（产品发布）/ `news`（新闻）/ `blog`（技术博客）/ `discussion`（讨论）

## TrendSnapshotVO（趋势）

```json
{
  "window": "today",
  "date": "2026-06-22",
  "generatedAt": "2026-06-22T04:15:32.123Z",
  "trends": [
    {
      "title": "AI 模型路由成为基础设施新热点",
      "summary": "多家公司推出模型路由解决方案，自动选择最优模型处理请求",
      "whyItMatters": "降低成本同时提升响应质量，成为 AI 应用的关键中间层",
      "category": "infrastructure",
      "status": "rising",
      "confidence": "high",
      "impact": "high",
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
- `category`: `ai`（AI/ML）/ `infrastructure`（基础设施）/ `product`（产品）/ `platform`（平台）/ `policy`（政策法规）
- `status`: `rising`（上升）/ `sustained`（持续）/ `cooling`（降温）
- `confidence`: `high` / `medium` / `low`
- `impact`: `high` / `medium` / `low`
- `sparkline`: 强度曲线数值数组，用于可视化趋势强度变化

## ItemDetailVO（条目详情）

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
    "quality": "high",
    "compliance": "safe"
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
- 标注状态（🔥 上升 / 🔄 持续 / ❄️ 降温）
- 列出主要证据来源（2-3 个代表性条目）

## 3. 用户问"AI 基础设施最近怎么样"

```bash
curl -s "https://api.hotdaily.top/v1/trends?window=week"
```

筛选 `category` 包含 `infrastructure` 或 `ai` 的趋势，总结演变方向。

## 4. 用户问"给我看 6 月 20 号的日报"

```bash
curl -s https://api.hotdaily.top/v1/digests/2026-06-20
```

## 5. 用户要某篇文章的详情

```bash
curl -s https://api.hotdaily.top/v1/items/{id}
```

展示完整摘要、价值分析、原文链接；如果 `bodyKind` 是 `full` 或 `native`，可以展示正文摘录。

# 调用示例

使用 `Bash` 工具调用 API：

```bash
# 获取今日日报
curl -s https://api.hotdaily.top/v1/digests/today | jq '.stories[] | {title, valueLight, valueReason}'

# 获取今日趋势
curl -s https://api.hotdaily.top/v1/trends/today | jq '.trends[] | {title, summary, status}'

# 获取周报
curl -s "https://api.hotdaily.top/v1/trends?window=week" | jq '.trends[0]'
```

# 注意事项

- **API 无需认证**，直接 GET 访问
- **今日日报和趋势不缓存**，实时更新
- **历史日报有边缘缓存**，响应快
- **解析 JSON 后按用户意图组织呈现**，不要直接转发原始 JSON
- **趋势生成时机**：每日自动，周一生成周报，每月 1 号生成月报
- **如果 API 返回 404**，说明该日期尚未出报或趋势尚未生成
- **内容语言**：标题为英文原题，摘要、理由、趋势分析均为中文

# 典型对话模式

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
2. 筛选 `category` 包含 `ai` 的趋势
3. 每个趋势给出：标题、摘要、为什么重要、状态（🔥/🔄/❄️）
4. 可选：列出 2-3 个代表性证据条目

---

**用户**: 给我看本周技术趋势报告

**助手行为**：
1. 调用 `/v1/trends?window=week`
2. 展示所有趋势，按 `impact` 排序
3. 每个趋势注明演变方向（rising/sustained/cooling）

---

**用户**: 详细讲讲这篇文章 [提供 ID]

**助手行为**：
1. 调用 `/v1/items/{id}`
2. 展示完整摘要（`summaryDetailZh`）
3. 价值理由（`valueReason`）
4. 如果有正文（`bodyKind` 是 `full`），可摘录关键段落
5. 给出原文链接

# API 稳定性

- 所有端点均已生产验证
- 响应格式稳定，字段名不会变更
- 新增字段向后兼容，旧字段不会删除
- 缓存策略：实时面不缓存，历史面边缘缓存 1-6 小时
