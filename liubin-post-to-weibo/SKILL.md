---
name: liubin-post-to-weibo
description: Post content to Weibo. Supports text posts with up to 9 images or a single video, using real Chrome with CDP automation.
---

# Post to Weibo

Post content to Weibo through the real web UI in Chrome.

## Script Directory

All scripts are under `scripts/`.

Agent execution steps:
1. Resolve this file's directory as `SKILL_DIR`
2. Use `${SKILL_DIR}/scripts/<script-name>.ts` for script paths
3. Replace `${SKILL_DIR}` with the actual absolute path at runtime

Script reference:
| Script | Purpose |
|--------|---------|
| `scripts/weibo-browser.ts` | Compose and publish text, image, or video posts |
| `scripts/copy-to-clipboard.ts` | Clipboard helper kept for compatibility |
| `scripts/paste-from-clipboard.ts` | Paste helper kept for compatibility |

## Prerequisites

- Google Chrome or Chromium installed
- `bun` installed
- First run requires manual login in the opened Weibo browser window

## Regular Posts

Supports:
- text only
- text + up to 9 images
- text + 1 video

```bash
# Preview mode
npx -y bun ${SKILL_DIR}/scripts/weibo-browser.ts "Hello from Claude!"

# One image
npx -y bun ${SKILL_DIR}/scripts/weibo-browser.ts "Check this out" --image ./photo.png

# Multiple images
npx -y bun ${SKILL_DIR}/scripts/weibo-browser.ts "Photos" --image a.png --image b.png --image c.png

# One video
npx -y bun ${SKILL_DIR}/scripts/weibo-browser.ts "Video post" --video ./clip.mp4

# Private-only video post
npx -y bun ${SKILL_DIR}/scripts/weibo-browser.ts "Private video" --video ./clip.mp4 --private

# Actually post
npx -y bun ${SKILL_DIR}/scripts/weibo-browser.ts "Publish now" --image ./photo.png --submit
```

## Parameters

| Parameter | Description |
|-----------|-------------|
| `<text>` | Post content |
| `--image <path>` | Image path, repeatable, max 9 |
| `--video <path>` | Video path, single file only |
| `--submit` | Actually publish instead of preview |
| `--private` | Set visibility to private only |
| `--profile <dir>` | Custom Chrome profile directory |

## Notes

- Always preview before `--submit` when possible
- Do not mix `--image` and `--video` in one post
- Browser closes automatically after the run
- Supports macOS, Linux, and Windows

## Extension Support

Optional overrides may be provided in:
1. `.liubin-skills/liubin-post-to-weibo/EXTEND.md`
2. `~/.liubin-skills/liubin-post-to-weibo/EXTEND.md`

If present, load the extension content before following the workflow.
