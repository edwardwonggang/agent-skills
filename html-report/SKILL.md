---
name: html-report
description: 生成内容丰富、可视化的问题定位/诊断 HTML 报告。用于把排查问题时的"现象/影响/复现/根因/证据/修复/遗留"输出成自包含单文件 HTML，强制带流程图（Mermaid）、动态交互图（ECharts）、时间线、关系图、截图/图片等多种形式，避免纯文字长篇。触发词：html 报告、问题定位报告、诊断报告、故障报告、排查报告、issue 报告、用 html 出报告、富文本报告、动态图报告。
metadata:
  short-description: 问题定位/诊断的富可视化 HTML 报告
---

# HTML Report（问题定位/诊断富可视化报告）

用本 skill 把一次问题排查 / 故障定位 / 诊断分析，输出为**自包含单文件 HTML**，做到"结论优先、图文并茂、动态可交互"，避免纯文字长文或满屏静态图。

## 何时使用 / 何时不用

- **使用**：用户要求"用 html 输出问题定位/诊断/排查/故障报告"，或要求"报告里要有流程图/动态图/多种形式"。
- **不用**：用户只是要普通 Markdown 小结、或明确要静态 PDF 排版（PDF 场景请另走静态 SVG 方案，本 skill 允许 JS 动态图，不保证打印一致性）。

## 输出形态总原则

1. **单文件自包含 HTML**：所有 CSS/JS 尽量内联或用 CDN script；不依赖本地额外文件（截图可用 img dataURI 或相对路径并说明）。
2. **结论优先**：页面顶部第 01 章必有"核心结论"卡片，一眼看懂"是什么问题、影响多大、根因是什么、建议动作"。
3. **图族强制 triage**：动笔前必须逐项评估下面 5 个图族，能用的必须嵌入 >=1 个（通常 2-4 个），每个记一句 use/skip 理由（可写进 HTML 注释）。禁止"整篇只有文字"。
4. **动态优先于静态**：数据类图优先用 ECharts（可缩放/悬停/动画），流程类用 Mermaid（可缩放/折叠）；截图/证据类用 img。
5. **颜色语义化**：颜色只表达含义（正常/告警/危险/主色），不为了好看乱配色；每个语义色都配文字标签。
6. **可折叠章节**：用 details/summary 收纳长证据、日志、原始输出，正文保持可扫读。

## 图族 triage（每个报告都要跑，禁止静默跳过）

逐项走一遍并记录 use/skip + 一句理由：

| 图族 | 用途 | 实现 | 默认判定 |
|------|------|------|----------|
| 1 流程/时序/状态图 | 调用链、请求链路、状态机、流程、根因传播路径 | Mermaid flowchart/sequence/stateDiagram | 有流程/链路/状态变更 必用 |
| 2 数据动态图 | 时序曲线、占比、分布、性能指标、多组对比 | ECharts line/bar/pie/scatter（交互动态） | 有数值/时间序列 必用 |
| 3 关系/影响面图 | 模块依赖、服务拓扑、影响范围、关联方 | Mermaid graph | 有依赖/拓扑/影响扩散 用 |
| 4 时间线 | 事件发生顺序、排查过程、故障演进 | 结构化时间线列表 + ECharts timeline | 有先后顺序事件 用 |
| 5 图片/截图证据 | 错误面板、日志截图、复现画面 | img（可 dataURI 内联） | 有可视化证据 用 |

每个图族都必须在回复或 HTML 注释里给出一句 verdict，例如：
```html
<!-- triage: 1flow=use(调用链); 2dataviz=use(错误率时序); 3graph=use(服务拓扑影响面); 4timeline=use(故障演进); 5img=use(报错截图) -->
```

## 问题定位报告的推荐章节结构

按内容取舍（不需要的删掉，编号随之连续）：

顺序遵循因果逻辑：**现象 → 复现 → 证据 → 根因 → 影响**——先摆证据才能据此分析根因，分析出根因才能推断影响范围。禁止把证据放在根因之后或影响放在根因之前。

1. 核心结论（编号 01）：问题一句话、影响面、根因、建议动作（顶部卡片，与正文章节同样带序号 01）
2. 现象描述：症状、报错、用户影响（配截图/错误原文）
3. 复现步骤：逐步可复现（配流程图）
4. 证据链：日志、调用链、指标曲线（配 ECharts + 折叠原文）——先呈现客观证据
5. 根因分析：据证据定位直接原因 + 深层原因（配时序/流程）——证据在前，分析在后
6. 影响范围：涉及模块/服务/用户/时间窗口（配关系图或数据图）——根因确定后评估波及面
7. 修复方案：方案 + 步骤 + 验证方式（配流程图）
8. 遗留问题 / 下一步：未解决项、待确认、后续建议

## 对齐规范（图类居中 / 文本数据类左对齐 · 硬性）

对齐不是随意定的，原则是：**"图"为了美观要水平居中，"文本/数据"为了阅读要左对齐**。动笔前按下面清单逐类判定，禁止漏判：

**应水平居中（图类组件）：**
- Mermaid 图（flowchart/sequence/stateDiagram/graph）：`pre.mermaid{text-align:center}` + `svg{margin:0 auto}`
- PlantUML 图（若用到）：同样 `pre.plantuml` 居中
- ECharts 图表容器：`.chart{margin:0 auto}`，canvas 在容器内天然居中
- 截图 / 图片 `<img>`：`display:block;margin:0 auto`（骨架已含）
- 图注 `.fig-caption`：`text-align:center`

**保持左对齐（文本/数据类组件，禁止居中）：**
- 时间线 `.timeline`：纵向时间轴列表流，居中会破坏时间线可读性
- 表格 `table`：宽度 100%、单元格左对齐
- 代码块 / 日志 `.chain`、`details pre`：代码左对齐，居中会导致错位
- 提示块 `.note` / `.risk-block` / `.ok-block`：带左边框的文本块，左对齐

> 骨架模板已内置这些规则；手写 HTML 时务必保留，不要删掉 `pre.mermaid` / `pre.plantuml` / `img` 的居中规则。

## 技术规范


- Mermaid：用 CDN script（cdn.jsdelivr.net/npm/mermaid），pre class="mermaid" 写图；启用 securityLevel loose 便于交互。**流程图必须水平居中**（骨架已含 pre.mermaid{text-align:center} + svg margin:0 auto，复制骨架即可；手写时务必保留该规则，Mermaid 默认左对齐）。
- ECharts：用 CDN script（cdn.jsdelivr.net/npm/echarts），每个图一个 div id="chartX"，底部统一 initCharts()。
- 折叠：长日志/原始输出放 details>summary + pre。
- 截图：能内联就 data:image/png;base64；不能则相对路径并标注来源。
- 自包含：优先 CDN（有网即可开）；若用户要完全离线，把 mermaid/echarts 的 min.js 内联进 script。
- 配色：统一用 --navy #294066（主）、--ok #276749（正常）、--warn #8a5a12（告警）、--risk #9b342a（危险）、--muted #5c6672（次要）、--ink #1b2733（正文）、--line #e4e2db（边框）、--bg #f6f7f9（底）。

## 校验（收尾必跑）

1. HTML 用真实浏览器渲染验证：用 Playwright 或 open 打开 HTML，确认图都渲染出来（Mermaid/ECharts 不白屏）、折叠可用、截图显示、无明显排版错乱。
2. 确认 triage 注释存在且 >=1 个图族 use。
3. 确认顶部有编号为 01 的"核心结论"卡片。
4. 确认无未替换的占位符 {{...}}。

## 产出物

- 报告 HTML 写到用户指定路径；未指定则写到当前项目 docs/reports/<日期>-<主题>.html 并告知路径。
- 完成后用 open <path> 或截图展示给用户看效果。
