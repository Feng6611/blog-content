---
date: '2026-10-18'
title: "【Command Reopen】我做了一个修复cmd tab体验的Mac App"
slug: "command-reopen"
description: "在 Mac 上用 Cmd+W 关掉或 Cmd+M 最小化窗口后，Cmd+Tab 切回去窗口不会出来。我 vibe 了一个零权限的小工具 Command Reopen 来补上这一步，这篇写它的原理、局限和价格。"
keywords: "Command Reopen, Cmd+Tab, macOS, 最小化窗口, 窗口恢复, 零权限, Vibe Coding, 独立开发, AltTab"
topics: [building]
---
## 背景
Mac是一个切换成本很高、但一旦习惯就离不开的设备。2020年大学毕业后我第一次使用Mac，我需要专程去学习《白皮书》、看了Youtube的课程，前人手把手帮我设置三指拖放和四指切屏，才一步步上手。
MacOS有自己的一套设计逻辑，有时候你说不清楚他是讲究还是固执。带着其他系统习惯过来的人，总有几处用着不顺手。

比如当我使用了`cmd w`关掉、或用`cmd m`最小化某个app的窗口之后，系统是这样处理的：
- App并没有退出（`quit`），在`cmd tab`的切换栏中还有该App
- 但窗口已经关闭或最小化，通过`cmd tab`切回这个app，窗口并不会被打开

我去调研如何解决这个问题，得到的答案大概有3种：
- 用`cmd h`隐藏，而不是关闭或者最小化。隐藏的App可以通过`cmd tab`打开窗口（但这根本就不符合我的习惯）
- `cmd tab` 选中 App 后按住 `Option`，再松开 `Cmd`，窗口就会出来（但我从来没有按键成功过）
- 通过程序坞激活（...）

于是忍无可忍，vibe了一个app解决这个问题。
## Command Reopen的实现

在我被折磨的日子里，细细研究过`cmd tab`的行为：切到一个没有窗口的app时，他已经被带到前台了，只是窗口没打开。
所以 cmd reopen 的方案很简单：监听系统的前台应用切换事件，一旦发现app已经到前台但没有可见窗口，就发送一个重新打开（reopen）的指令，把窗口唤醒。
市面上已有的解决方案（以[AltTab](https://alt-tab-macos.netlify.app/)为代表），往往有着复杂的窗口管理行为——比如窗口预览、重排，甚至直接重绘并覆盖原生的`cmd tab`界面，而且通常需要辅助功能甚至屏幕录制权限。
cmd reopen只解决这样一个问题，不改变系统UI，也不做复杂逻辑，只是补充了一个窗口激活行为，对我的日常使用来说，这已经完全足够了。

![[command-reopen-before-after.gif|alt=左：未安装时，cmd tab 切回已关闭窗口的 App，窗口没有出现；右：安装 Command Reopen 后，窗口随即出现]]

## 零权限带来的局限性
因为坚持不申请系统敏感权限，目前的方案有两个客观限制：
### 1. 缺乏窗口级别的控制
精细到窗口级别的管理能力需要申请AX辅助功能权限，因此目前的cmd reopen只能做到App级别的唤醒。比如当你的App有多个最小化窗口，他只能唤起最上层的那个，无法指定激活哪一个窗口。
### 2. 依赖 App 遵循 macOS 原生窗口规范
这个机制需要目标 App 规范实现 macOS 的窗口 API，才能正常响应系统的激活指令。如果某个 App 的窗口管理是“纯手搓”的非标实现，就可能无法兼容。
在测试场景下，有用户发现，通过飞书启动的飞书会议有占用一个单独的dock栏icon，他就完全无法响应cmd reopen的激活行为。此外可能还有一些不规范的App没有被发现。

不过在我日常使用大半年的场景下，绝大多数主流App都基本没有啥问题。
## 价格&展望
- [App Store](https://apps.apple.com/app/command-reopen/id6757333924?pt=128417926&ct=kkuk-blog&mt=8)
- [Website](https://commandreopen.com/zh/)
- Lifetime $9.99
- [Github开源地址](https://github.com/Feng6611/mac-command-reopen)

[Release](https://github.com/Feng6611/mac-command-reopen/releases)提供了direct分发的稍旧版本，可以免费下载。或者可以clone后自己编译，这里有一个[实验性的含AX的版本](https://github.com/Feng6611/mac-command-reopen/tree/codex/direct-ax-advanced-mode)，可以处理上方讲的局限性问题。

在cmd reopen发布后，我想过要不要针对于类似的细节痛点，每个场景都单独做一个小App——毕竟macOS上可以优化的小体验实在太多了。但顺手做了2个之后，我停下了。一方面你不可能下载100个app，另一方面聚合的体验解决型产品才适合解决类似的场景。比如[Raycast](https://www.raycast.com/)和[Supercharge](https://sindresorhus.com/supercharge)。
