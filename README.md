# agent-skills

Reusable agent skills for Claude Code, Cursor, and other agents supported by the `skills` CLI.

## Included Skills

- `liubin-post-to-weibo`: Post text and up to 9 images, or a single video, to Weibo through the real web UI in Chrome. Supports preview mode, publish mode, and private-only visibility.
- `html-report`: 把问题排查/故障定位/诊断分析输出为自包含单文件 HTML 报告。强制带流程图（Mermaid）、动态交互图（ECharts）、时间线、关系图、截图等多种形式，避免纯文字长篇。章节遵循因果逻辑：现象 → 复现 → 证据 → 根因 → 影响 → 修复 → 遗留。

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
