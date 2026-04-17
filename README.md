# Nexus4CC — Personal Fork

> A personal fork of **[librae8226/nexus4cc](https://github.com/librae8226/nexus4cc)**.
> All credit for the original project goes to **[@librae8226](https://github.com/librae8226)** and the upstream contributors.
>
> 🇨🇳 [中文版](README_CN.md)

For the full project — feature overview, Quick Start, architecture documentation, PWA setup, showcase video, and licensing — please refer to the upstream repository:

### 👉 https://github.com/librae8226/nexus4cc

---

## Why this fork

I run Nexus as my daily-driver control panel for Claude Code over Tailscale Funnel — macOS host, iPhone + Android + Mac clients. Along the way I accumulated a handful of fixes and features that made the workflow robust enough to survive reboots, laptop-close-reopen cycles, and claude CLI upgrades. This fork is where those changes live.

## What's different from upstream

Based on upstream `v4.5.0` (commit `8dddc3a`), this fork adds **11 commits** of fixes and features.

### Bug fixes

- **tmux `new-session` silently loses sessions** — shellCmd quote-nesting caused truncated tmux commands; now uses `execFileSync` with an argv array
- **iOS Safari photo picker** — `<input type="file">` with `display:none` silently no-op'd `.click()`; now off-screen but interactive
- **Rename supports Unicode + leading `-`** — `测试频道`, `-leading-dash`, emoji all work now
- **Android third-party voice IMEs** — restored per-UA routing so `豆包语音` / `讯飞` / MIUI voice etc. land text directly into xterm
- **New-project / channel dialogs** — PROFILE dropdown no longer auto-selects `configs[0]`; default is now "no profile"
- **Error visibility** — create-session / create-window failures now surface as toasts instead of being silently swallowed

### New features

- **Snapshot persistence + `claude --resume`** (headline feature)
  Project and channel skeletons are written to `data/sessions-snapshot.json` and auto-rebuilt on Nexus startup. For each claude window, the active `session_id` is inferred from `~/.claude/projects/<encoded_cwd>/*.jsonl` and replayed via `claude --resume <id>` — so historical conversations come back verbatim after `tmux kill-server`, PM2 restart, or a full reboot.
- **QuickSwitcher** — mobile top-of-screen pill for fast window switching
- **iTerm2 launcher** (macOS only) — per-project ▷ button opens the session locally in iTerm2 via `tmux -CC attach`
- **Default dark theme** — no longer follows system preference
- **iOS IME proxy** — hidden `<input>` bridges xterm input for iOS voice dictation + Pinyin candidate strip
- **WebSocket onopen handshake** — session switches wait for socket readiness, eliminating the previous 100ms flicker
- **Path validation** — backend rejects non-existent `cwd` before invoking tmux, surfaces a clear 400 error

## Upstream PR

Most of these changes have been submitted back to the upstream project:

➜ https://github.com/librae8226/nexus4cc/pull/28

Whether the upstream author chooses to merge any subset is entirely up to them — this fork stands on its own as a personal daily-driver build.

## License

Same as upstream: [GPL v3 / Commercial dual license](LICENSE.md).
