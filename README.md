# HotDaily Skill

让 AI Agent 直接读取 [HotDaily](https://hotdaily.top) 的科技与创业精选日报和趋势分析。

## 功能

- 📰 获取每日精选日报（AI 替你读过并价值评估）
- 📈 分析技术趋势（日报/周报/月报）
- 🔍 查询历史内容
- 📄 获取单篇文章详情

## 安装

```bash
npx skills add https://github.com/Done-0/hotdaily-skill --skill hotdaily
```

## 使用

在对话中直接使用 `/hotdaily` 命令：

```
/hotdaily 今天有什么值得读的？
/hotdaily 最近 AI 趋势
/hotdaily 给我看 6 月 20 号的日报
/hotdaily 分析本周基础设施趋势
```

## 示例对话

### 获取今日精选

**你**: `/hotdaily 今天有什么值得读的？`

**AI**: 
> AI 替你读过 156 篇，精选 30 篇：
> 
> **精读推荐 (5 篇)**
> - [Anthropic's new interpretability research](https://...): AI 安全性突破，首次实现大规模神经元行为解释
> - [PostgreSQL 17 released](https://...): 性能提升 40%，新增分布式事务支持
> 
> **略读推荐 (25 篇)**
> - [GitHub Copilot Workspace](https://...): 从需求直接生成 PR，适合快速了解新工具
> ...

### 查看技术趋势

**你**: `/hotdaily 最近 AI 领域有什么趋势？`

**Agent**:
> 今日 AI 领域 3 个趋势：
> 
> 🔥 **AI 模型路由成为基础设施新热点** (上升)
> 多家公司推出模型路由解决方案，自动选择最优模型处理请求。降低成本同时提升响应质量，成为 AI 应用的关键中间层。
> 证据: OpenRouter 新算法、Martian 发布、LangChain 集成
> 
> 🔄 **多模态模型持续改进** (持续)
> ...

### 查看本周报告

**你**: `/hotdaily 给我看本周技术趋势报告`

**Agent**:
> 本周(6月15-22日)聚合趋势 5 个：
> 
> **高影响趋势**
> - AI 推理成本下降加速 (上升 → 持续)
> - 边缘计算框架成熟度提升 (持续)
> ...

[查看更多示例 →](examples/)

## API 说明

HotDaily Skill 调用以下公开 API：

| 端点 | 说明 |
|------|------|
| `/v1/digests/today` | 今日日报 |
| `/v1/digests/{date}` | 历史日报 |
| `/v1/trends/today` | 今日趋势 |
| `/v1/trends?window=week` | 周报 |
| `/v1/trends?window=month` | 月报 |
| `/v1/items/{id}` | 条目详情 |

完整 API 文档：https://api.hotdaily.top

## 关于 HotDaily

HotDaily 是一个由 AI 驱动的科技与创业精选日报：
- 每天从 Hacker News、Lobsters 等社区筛选内容
- AI 替你读过并评估价值（精读/略读）
- 提供中文摘要和价值理由
- 自动生成技术趋势分析（日报/周报/月报）

访问：https://hotdaily.top

## License

MIT

## 贡献

欢迎提交 Issue 和 PR：
- 改进 skill 提示词
- 添加更多使用示例
- 优化响应格式

---

Made with ❤️ for the developer community
