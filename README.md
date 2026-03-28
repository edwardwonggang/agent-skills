# agent-skills

Reusable agent skills for Claude Code, Cursor, and other agents supported by the `skills` CLI.

## Included Skills

- `liubin-post-to-weibo`: Post text and up to 9 images, or a single video, to Weibo through the real web UI in Chrome. Supports preview mode, publish mode, and private-only visibility.

## Install

List skills in this repository:

```bash
npx -y skills add https://github.com/edwardwonggang/agent-skills --list
```

Install the Weibo skill:

```bash
npx -y skills add https://github.com/edwardwonggang/agent-skills --skill liubin-post-to-weibo
```

Install globally for all supported agents:

```bash
npx -y skills add https://github.com/edwardwonggang/agent-skills --skill liubin-post-to-weibo -g -a '*' -y
```
