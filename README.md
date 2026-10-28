# agent-skills

Reusable agent skills for Claude Code, Cursor, and other agents supported by the `skills` CLI.

## Included Skills

- `liubin-post-to-weibo`: Post text and up to 9 images, or a single video, to Weibo through the real web UI in Chrome. Supports preview mode, publish mode, and private-only visibility.
- `html-report`: 富可视化自包含单文件 HTML 报告，**双模式**：
  - **故障定位（incident）**：现象 → 复现 → 证据 → 根因 → 影响 → 修复 → 遗留（骨架 `assets/skeleton.html`）
  - **知识学习（learn）**：讲清一个原理/技术/概念，先给结论、再讲原理、配交互演示与场景自测（骨架 `assets/skeleton-learn.html`）
  - 共用：Mermaid 流程图、ECharts 动态图、callout 语义块、limits 指标条、kv 参数卡、移动端适配

## Install

List skills in this repository:

```bash
npx -y skills add https://github.com/edwardwonggang/agent-skills --list
```

Install the Weibo skill:

```bash
npx -y skills add https://github.com/edwardwonggang/agent-skills --skill liubin-post-to-weibo
```

Install the HTML report skill:

```bash
npx -y skills add https://github.com/edwardwonggang/agent-skills --skill html-report
```

Install globally for all supported agents:

```bash
npx -y skills add https://github.com/edwardwonggang/agent-skills --skill liubin-post-to-weibo -g -a '*' -y
```
