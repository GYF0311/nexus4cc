# Nexus4CC — 个人 Fork

> 这是 **[librae8226/nexus4cc](https://github.com/librae8226/nexus4cc)** 的个人定制 fork。
> Nexus 项目本身的所有功劳归原作者 **[@librae8226](https://github.com/librae8226)** 与其他贡献者。
>
> 🇬🇧 [English](README.md)

完整功能介绍、快速开始、架构文档、PWA 配置、演示视频、License —— 请访问上游仓库：

### 👉 https://github.com/librae8226/nexus4cc

---

## 为什么有这个 fork

我用 Nexus 作为日常 Claude Code 的远程终端面板（macOS 宿主 + Tailscale 透传 + iPhone/Android/Mac 多端访问）。使用过程中积累了一系列修复和功能增强，让整个工作流在电脑重启、tmux 清空、Claude CLI 升级后都能自动恢复。这些改动集中在本 fork 里。

## 相对上游的改动

基于上游 `v4.5.0`（`8dddc3a`），本 fork 在其上叠加了 **11 个 commit**。

### Bug 修复

- **tmux 新建 session 瞬间自毁** — shellCmd 引号嵌套导致 tmux 收到截断命令；改用 `execFileSync` + 参数数组彻底避开 shell 解析
- **iOS Safari 相册选择器** — `<input type="file">` 的 `display:none` 样式让 iOS 静默拒绝 `.click()`；改为不可见但可交互
- **Rename 支持中日韩 + `-` 开头** — `测试频道`、`-leading-dash`、emoji 全部能正常保存
- **Android 第三方语音输入** — 恢复按 UA 分支的路由，豆包语音 / 讯飞 / 小米语音等直接把文字写进 xterm
- **新建项目 / 频道对话框** — PROFILE 下拉不再默认选 `configs[0]`，默认是"不使用 profile"
- **错误可见化** — 创建失败时显示 toast，不再被静默吞掉

### 新功能

- **Snapshot 持久化 + `claude --resume`**（主打）
  项目与频道骨架写入 `data/sessions-snapshot.json`，Nexus 启动时自动重建。对每个 claude 窗口，从 `~/.claude/projects/<encoded_cwd>/*.jsonl` 反推当前 `session_id`，重建时用 `claude --resume <id>` 精准接续之前的对话——`tmux kill-server`、PM2 重启、电脑重启后历史对话都能回来
- **QuickSwitcher** — 移动端顶部下拉药丸，一指切换窗口
- **iTerm2 启动按钮**（macOS）— 每个项目一个 ▷ 按钮，一键在本机 iTerm2 打开 `tmux -CC attach`
- **默认暗色主题** — 首次打开直接暗色，不再跟随系统
- **iOS IME 代理** — 隐藏 `<input>` 桥接 xterm 输入，支持 iOS 语音听写 + 拼音候选
- **WebSocket onopen 握手** — session 切换由 WebSocket 就绪触发，消除旧的 100ms 闪烁
- **Path 校验** — 后端先验证 cwd 存在再调 tmux，工作目录不存在时直接返回清晰的 400 错误

## 上游 PR

这些改动的大部分已提交到上游项目：

➜ https://github.com/librae8226/nexus4cc/pull/28

合不合并由原作者决定——本 fork 作为我的个人日常部署独立维护。

## License

与上游一致：[GPL v3 / 商业双许可](LICENSE.md)
