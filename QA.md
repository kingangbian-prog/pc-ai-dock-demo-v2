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

补充实际页面检查：Workbuddy 在完成「发布渠道建议」后，同时显示此任务的已打开文档及「竞品交互研究」的 Computer Use；仍为 AI Dock。1280px 计算样式：外层 overflow visible、连接区 33px；390px：连接区 82px、列表内部 overflow auto。无控制台错误。证据：`screenshots-v2/combined-task-context.png`。这些为点击操作和布局证据，不替代真实鼠标路径测试。

## 用户补充修订：单行 Dock、Goal 目录树、独立文档卡

- Dock 始终 `flex-wrap:nowrap`，增加应用时横向加长，达到屏幕宽度后内部横向滚动；左右按钮和键盘可操作，预览采用外层 Dock 定位，未被内部滚动裁切。
- Bot 左侧目录树目标名称右侧显示实际任务归属 Agent，1–2 个并列，3–4 个扇形堆叠，完整名称通过 title 和 aria-label 提供。通过真实发送到新品发布 Goal 的任务依次验证 1、2、3、4 个 Agent。
- 文档由原先嵌套的小条目改成独立窗口卡，已打开文档排在卡组前面；Computer Use 独立成卡，任务进度仍独立保留。
- 实际浏览器打开千问两份文档：独立文档卡数 2；聚焦并关闭第一份后 1；关闭最后一份后 0；整个过程中任务进度卡数始终 6，AI Dock 始终存在。
- 1280px 启动七个传统应用：全部 14 个 Dock 应用按钮 y 坐标均为 616，未换行。390px：按钮均在 y=758，滚动区可见宽304、内容宽967，点击右滚按钮可访问其余图标。预览窗口实际显示在滚动条上方，无裁剪。
- 新增 Goal 参与 Agent 去重与 Goal 隔离测试，总计 44 项测试通过。控制台没有应用错误。
- 真实无按键鼠标移动仍未实测：本次工具没有公开 move/hover API，未用 DOM 事件伪造。所有交互验证为实际点击/键盘，截图显示其最终可见状态。

证据：`screenshots-v2/goal-fan-three.png`、`screenshots-v2/independent-document-cards.png`、`screenshots-v2/single-row-mobile.png`。
