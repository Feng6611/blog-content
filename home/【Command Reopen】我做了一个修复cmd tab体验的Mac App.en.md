---
slug: command-reopen
language: en
translationOf: home/【Command Reopen】我做了一个修复cmd tab体验的Mac App.md
date: '2026-10-18'
title: "Command Reopen: I Built a Mac App to Fix Cmd+Tab"
description: "After Cmd+W or Cmd+M, Cmd+Tab won't bring the window back. I vibe-coded Command Reopen — a zero-permission Mac app that fills that gap."
keywords: "Command Reopen, Cmd+Tab, macOS, minimize, window restore, zero permission, vibe coding, indie dev, AltTab"
---

## Background

A Mac is expensive to switch to, but once you're used to it, you can't leave. After graduating in 2020 I used a Mac for the first time. I had to study a Mac "white paper" guide, watch YouTube courses, and have people ahead of me walk me through three-finger drag and four-finger swipe between desktops before I got the hang of it.

macOS has its own design logic; sometimes you can't tell if it's careful or just stubborn. If you're coming over with muscle memory from other operating systems, a few things are bound to trip you up.

Take what happens when you close an app's window with Cmd+W or minimize it with Cmd+M:
- The app doesn't quit; it still shows up in the Cmd+Tab app switcher.
- But because the window was closed or minimized, switching back to that app with Cmd+Tab won't bring it up.

I went looking for ways to deal with this, and found roughly three answers:
- Hide apps with Cmd+H instead of closing or minimizing them. Hidden apps do reopen windows via Cmd+Tab (which completely goes against my habits).
- Select the app in Cmd+Tab, hold Option, then release Cmd, and the window appears (I've literally never managed to pull this key combo off).
- Activate it from the Dock (…)

Eventually I'd had enough and vibe-coded an app to fix it.

## How Command Reopen Works

During the days this was driving me crazy, I took a closer look at how Cmd+Tab actually behaves: when you switch to an app with no open windows, macOS actually brings it to the foreground—it just doesn't open a window.

So the approach behind Command Reopen is pretty straightforward: listen for system foreground app switches, and as soon as an app arrives in the foreground without any visible window, send a "reopen" command to wake it up.

Existing solutions (notably [AltTab](https://alt-tab-macos.netlify.app/)) often do complex window management — previews, rearranging, even redrawing and covering the native Cmd+Tab UI — and usually need Accessibility permission or even Screen Recording permission.

Command Reopen only fixes this one thing. It doesn't change system UI or run complex logic — it just adds a window-activation step. For daily use, that's enough.

## Limitations of Going Zero-Permission

Because it refuses sensitive system permissions, the current approach has two hard limits:

### 1. Lack of Window-Level Control

Window-level control needs Accessibility (AX) permission, so Command Reopen can only wake apps, not individual windows. If an app has multiple minimized windows, it will only bring up the topmost one—you can't pick a specific window to activate.

### 2. Reliant on Apps Following Native macOS Window Standards

The mechanism needs the target app to implement macOS window APIs properly so it can answer system activation. If an app's window management is a non-standard, hand-rolled setup, it may not work.

In testing, a user found that Feishu Meetings (launched from Feishu / Lark) takes its own Dock icon and completely ignores Command Reopen's activation. There may be other non-standard apps we haven't found yet.

Still, after half a year of daily use, most mainstream apps are mostly fine.

## Pricing & What's Next

- [App Store](https://apps.apple.com/app/command-reopen/id6757333924?pt=128417926&ct=kkuk-blog&mt=8)
- [Website](https://commandreopen.com/zh/)
- Yearly $5.99, Lifetime $9.99
- [GitHub (open source)](https://github.com/Feng6611/mac-command-reopen)

The [GitHub Releases](https://github.com/Feng6611/mac-command-reopen/releases) page offers slightly older direct-distribution builds you can download for free. Alternatively, you can clone the repo and build it yourself. There's also an [experimental build with Accessibility permission](https://github.com/Feng6611/mac-command-reopen/tree/codex/direct-ax-advanced-mode) that addresses the limitations mentioned above.

After shipping Command Reopen, I wondered whether to make a tiny app for every similar papercut — macOS has so many small things you could polish. I knocked out two more and stopped. You won't install a hundred apps; bundled tools that fix many papercuts fit this better — like [Raycast](https://www.raycast.com/) and [Supercharge](https://sindresorhus.com/supercharge).
