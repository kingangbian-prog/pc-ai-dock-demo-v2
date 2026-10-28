# 2.0 验证记录

日期：2026-10-28。公开站：https://kingangbian-prog.github.io/pc-ai-dock-demo-v2/

## 自动验证

- `npm run check`：7 个运行模块语法通过。
- `npm test`：43 项通过，0 项失败。
- 新增测试明确使用虚拟时间验证 Bot 研究→写作的来源继承；多文档草稿互不覆盖；打开草稿不改变 AI Dock；旧存储键不被写入。

## 实际浏览器

使用 CUA 内置浏览器，1280×720 和 390×844。浏览器页面标题与 2.0 URL 一致，无空白页、错误覆盖层和应用控制台警告/错误。

| 检查 | 结果 |
|---|---|
| Goal 多 Agent 图标 | 通过，Goal 下显示 Workbuddy、千问、豆包、小艺 |
| Bot 来源与手动演示卡区别 | 通过，简报/研究带 Bot，手动演示卡不带 |
| 文档由任务卡打开 | 通过，桌面独立窗口，AI Dock 不切换 |
| 编辑内容聚合预览 | 通过，千问预览直接显示文档最新文字 |
| 多输出、多文档、重复打开 | 通过，两份输出分别打开，重复打开数量仍为 2 |
| 草稿刷新保留 | 通过，本地刷新后重新打开保留独立修改 |
| Computer 两个入口 | 通过，同一会话 cu-session-001，无重复窗口 |
| 接管→保存→交还 | 通过，等待双方 ACK，上下文从 v1 更新到 v2 |
| Word 多窗口生命周期 | 通过，关闭其中一个仍保留图标，最小化保留，关闭最后窗口移除 |
| 7 个传统应用同时运行 | 通过，桌面 Dock 内显示全部运行图标 |
| 窄屏 | 通过，390px 下任务卡横向展示，文档与 Dock 可操作 |
| 公开站交互 | 通过，在线点击千问→输出→聚合预览，Bot 标记 1、文档窗口 1、AI Dock 1 |
| 线上翻面 | 通过，切换到普通 Dock |
| 原仓库未改 | 远端 main 仍为 5a4be2e3b7ec22a04800b0ec94391b30ef528e0d |

Agent 与普通应用保留 CSS hover 和 focus-within 预览机制，实际浏览器以点击固定和键盘进入窗口预览验证可操作性；没有独立注入鼠标移动事件。窗口为原型内容预览，并非真实 OS 窗口缩略图。没有进行真实 Agent、真实应用、跨浏览器或 390px 以下屏幕测试。

## 截图

- `screenshots-v2/task-document.png`：Agent 任务 + Bot 来源 + 已打开文档预览。
- `screenshots-v2/computer-task.png`：Computer Use 与任务归属。
- `screenshots-v2/mobile-task.png`：390×844 任务卡。
- `screenshots-v2/published-task.png`：公开站实际任务文档聚合。

`screenshots/` 是继承的 1.0 历史截图，请以 `screenshots-v2/` 作为 2.0 证据。

## Hover follow-up

工具公开 API 只有点击、拖动、滚动和键盘；没有鼠标移动或 locator.hover。工具清单与浏览器文档未发现其他可用 hover 接口。未用脚本事件伪造 hover，因此真实鼠标从 Dock 移入预览的路径仍未实测。

代码复查发现外层 `overflow:auto` 会裁掉悬停桥接伪元素。已改为外层 `overflow:visible`、内部任务列表滚动，并将窄屏桥接扩至 82px 覆盖 76px 间隙。普通 Word 的桥接不受该裁剪影响。点击固定、键盘进入、文档保持 AI 模式、Computer 同一会话和 Word 多窗口生命周期已实际验证。
