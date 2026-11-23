---
layout: post
title: "Pinky: let AI agents use your iPhone or Android phone, with a human approval gate (2026 guide)"
description: "Pinky is a free, open-source app that lets Claude Code, Codex, Cursor or any MCP agent operate a real iPhone or Android phone while you watch, approve risky taps and take over. Here's how it works, how to set it up, and where its limits are."
date: 2026-11-16 22:30:00 +0530
author: Asad Faizee

categories:
  - AI Tools
  - Open Source

tags:
  - Pinky
  - DIYgod
  - AI phone agent
  - MCP
  - Model Context Protocol
  - Claude Code
  - Claude Desktop
  - Codex
  - Cursor
  - Android automation
  - iPhone automation
  - human in the loop
  - agent safety
  - open source
  - Go
  - Ollama
  - local AI
  - developer tools
  - AI agents

image:
  path: /assets/img/posts/pinky-ai-phone-agent-mcp.webp
  alt: "Pinky desktop app mirroring an Android phone while an AI agent works, with an approval dialog holding a tap"

pin: false
comments: true
---

## Pinky, in one paragraph

Pinky is a free, open-source desktop app that lets an AI agent use your real iPhone or Android phone while you watch every move. You connect Claude Code, Codex, Cursor or any MCP-capable agent, hand it a task like "text Sam I'm running late," and Pinky mirrors the phone while holding the risky taps (payments, transfers, deletions) until you click Allow.

It comes from DIYgod, the developer behind RSSHub, a project with 47,060 GitHub stars. Pinky itself pulled 2,287 stars in its first 10 days. And it shares the local-first DNA I keep coming back to in this series (see [Atomic Chat](https://blog.asadfaizee.is-a.dev/posts/atomic-chat-offline-ai-chatbot-local-llm/)): records stay on your computer, and there's no account anywhere.

## What is Pinky?

Pinky is a bridge between AI agents and your phone, built on the Model Context Protocol, the open standard Anthropic released in November 2024 ([Anthropic](https://www.anthropic.com/news/model-context-protocol)). OpenAI adopted the same standard in March 2025 ([TechCrunch](https://techcrunch.com/2025/03/26/openai-adopts-rival-anthropics-standard-for-connecting-ai-models-to-data/)), which is why one small app can serve nearly every agent you already use.

The app itself is written in Go, licensed Apache-2.0, and it's tiny: a 16 to 22 MB download drawn on the GPU with no Electron and no webview, idling at about 70 MB of memory and 0% CPU on a Mac. That's a refreshing change from the 300 MB Electron wrappers we usually get.

The repo hit #1 in developer-tool momentum in its first week, gaining 2,039 stars in 7 days ([LavX News](https://news.lavx.hu/article/diygod-pinky-lets-ai-agents-operate-phones-under-user-control-2026-11-13)). As I write, it has 2,287 stars, 245 forks and 1,408 commits, 10 days after the first commit.

## How does Pinky let an AI agent control your phone?

Through MCP. Pinky's gateway serves one MCP server, and agents connect over stdio or HTTP. Each agent gets a set of `phone_` tools, from connecting and tapping to steering a running task, and every action returns the new screen so the agent sees the result of its own tap.

The interesting part is where the rules live. Approvals are enforced by Pinky's gateway, a separate layer the agent can't reach, rather than by instructions in the agent's prompt. That distinction matters because screen text can try to talk an agent into tapping something stupid (prompt injection is a real attack). With Pinky, no amount of convincing text on the screen lets the agent approve itself.

Setup is one click for the big agents. Pinky's Settings finds Claude Code, Claude Desktop, Codex, Cursor, VS Code, Copilot CLI, Windsurf and Gemini CLI on your computer, adds itself to the agent's MCP config, and installs a SKILL.md guide the agent reads so it knows how to drive a phone politely.

## What stops an AI agent from wrecking your phone?

Four layers, and each one is a real design decision rather than a checkbox.

**The approval gate.** Payments, trades and transfers wait for your click by default. In Settings you can extend that to sends, orders, deletions, calls, installs, external links and settings changes, or to every single step. Pinky recognizes these risky buttons in 10 languages.

**Off-limits by default.** Banking, wallet and password-manager apps are invisible to agents. Agents can't type into password or one-time-code fields. When a task genuinely needs a banking app, Pinky asks, and you can allow it for that task only.

**A hidden second screen.** On Android 13 and later, the agent works on a virtual second screen, so your actual screen stays yours, or off and locked. On iPhones and older Androids it uses the main screen.

**Take over any time.** Click the mirror and the agent pauses until you hand control back. Touching the phone yourself pauses it too on Android. And "Stop all" is one shortcut (⌥⌘. on Mac, Ctrl+Alt+. elsewhere) from anywhere.

Every run is also on the record: each step in plain words, screenshots and a screen recording with a mark per step, exportable as an HTML report. It all stays on your computer.

## Which phones and agents does Pinky support?

Nearly everything you'd realistically own:

- **Computers:** macOS 13+ (Apple silicon and Intel), Windows 10/11 (x64), Linux (x86-64, glibc 2.29+).
- **Android phones:** Android 10 through 17, over USB or Wi-Fi pairing. Pinky ships its own adb built from source, so there's nothing else to install.
- **iPhones:** iOS 17.4 or later. On iOS 27 and later, nothing installs on the phone at all. On iOS 17.4 to 26 you add Pinky's on-phone app, which Pinky prepares itself.
- **Agents:** anything that speaks MCP. Claude Code, Claude Desktop, Codex, Cursor, VS Code, Copilot CLI, Windsurf and Gemini CLI get one-click setup; a script works too.
- **Simulators:** an iOS Simulator or Android emulator works in place of a real phone, which is how I'd test it first.

One clever detail: over Wi-Fi, Pinky marks each phone it sets up and verifies the mark on every connection, so another device on your network can't pretend to be your phone. With Tailscale, you can reach your phone from anywhere.

## How do you install and set up Pinky?

Three steps, about 10 minutes.

**1. Install the desktop app.** On Mac: `brew install --cask diygod/tap/pinky`, or grab the dmg from the releases page. On Windows: run the installer (it's unsigned, so SmartScreen will complain; choose More info, then Run anyway, and judge that trade-off yourself). On Linux: install the .deb or unpack the tarball.

**2. Connect your phone.** Open Pinky, choose Add phone, and follow the brand-specific steps (it has tailored guides for Xiaomi, OPPO, vivo, HONOR, HUAWEI, Samsung and Pixel). For Android that means enabling USB debugging and plugging in; for iPhone, a USB cable for setup and Wi-Fi afterwards. Each phone has a Checkup that lists what works and what needs fixing.

**3. Add Pinky to your agent.** In Settings, click Connect next to your agent. For Claude Code, the manual route is:

    claude mcp add --scope user pinky -- pinkyctl mcp

(Note the command is `pinkyctl`, because a different program called `pinky` already exists on many systems.)

Want to try it with no phone attached? Run `pinky mcp --fake` and the agent drives a simulated device. That's a genuinely good on-ramp.

## Can Pinky do tasks without a computer?

Yes. Pinky's app runs right on the phone, on Android and iPhone, and does tasks by itself with a model you pick: DeepSeek, Qwen, Kimi, GLM, Claude, GPT, Gemini, or local models in Ollama or LM Studio. Type or speak a task, or share content to it from another app.

On Android you get a floating button to ask about whatever's on screen, and the agent can work on a hidden background screen while you keep using the phone normally. Scheduled tasks work on both the computer and Android: run something at a set time, or get notified when a condition holds.

After a task, Pinky may suggest a note to remember (a preference you mentioned, for example) and keeps it only if you say yes. Small thing, correct instinct.

One more trick: if your phone is locked, you can open it from your computer, with each go-ahead confirmed by Touch ID, Windows Hello or your keyring password. The agent never sees the PIN.

## Is Pinky safe to use?

Safer than handing an agent raw adb access, and still early. The approval gateway is enforced outside the agent's reach, risky app categories are hidden by default, and every run leaves a reviewable record. That's a stronger safety posture than most agent-phone demos I've seen, where the model is simply asked nicely to behave.

But read the labels. The README says pre-alpha in plain text. Releases roll out every few hours (151 releases in the first 9 days), which means fast fixes and fast breakage. The Windows installer isn't signed yet. And the honest reviewer's rule applies: put the approval gate on a phone you can afford to have tapped wrong, and keep banking apps off-limits unless a task truly needs them.

The design lesson here is bigger than one app: put the safety control where the agent can't touch it. I'd love to see other agent tools steal this pattern.

## What are the honest limitations?

- **Pre-alpha.** Expect rough edges and breaking changes; the docs point to an ACCEPTANCE.md that says exactly what works today.
- **Older iPhones get a reduced surface.** On iOS 17.4 to 26 without the on-phone app, Pinky can only read the screen and open apps: no taps, swipes or typing.
- **Windows driver setup.** Some phones need a manual USB driver on Windows, and Linux needs udev rules.
- **No model included.** On the phone, you bring your own model, which is a feature for cost control and a chore for beginners.
- **Screen-recognition dependence.** Like every vision-driven agent, it can misread a screen; that's why the approval defaults lean strict.

## Is Pinky worth trying in 2026?

Yes, if you've wanted to hand your phone chores to an agent without handing over the keys. The combination of MCP compatibility, a gateway-enforced approval layer and a fully local record of every run makes it the most trustworthy take on phone agents I've seen so far.

It's also just a nice piece of engineering: 20 MB, no Electron, no account, Apache-2.0.

Start with an emulator and `pinky mcp --fake`, graduate to an old Android phone, and only then let it near the phone you actually like. By the time it hits a stable 1.0, you'll already know your way around.

## FAQ

**Is Pinky free?**
Yes. Apache-2.0 open source, no account, no API key from Pinky itself. You pay only for whatever model your chosen agent or on-phone model uses.

**Which AI agents work with Pinky?**
Anything that speaks MCP: Claude Code, Claude Desktop, Codex, Cursor, VS Code, Copilot CLI, Windsurf, Gemini CLI, or a plain script. Pinky connects them over stdio or HTTP.

**Does Pinky work on iPhones?**
Yes, iOS 17.4 and later. On iOS 27+ nothing installs on the phone; on iOS 17.4 to 26 you add Pinky's on-phone app, which the desktop app prepares for you.

**Is Pinky safe for banking apps?**
Banking, wallet and password-manager apps are invisible to agents by default, and agents can't type into password or one-time-code fields. You can allow a specific app for a specific task, but the project is pre-alpha, so I'd keep money apps out of scope for now.

## Sources

- [Pinky GitHub repository](https://github.com/DIYgod/pinky)
- [Pinky MCP reference and tool docs](https://github.com/DIYgod/pinky/blob/main/docs/mcp.md)
- [Pinky releases](https://github.com/DIYgod/pinky/releases)
- [Anthropic, Introducing the Model Context Protocol](https://www.anthropic.com/news/model-context-protocol)
- [TechCrunch, OpenAI adopts Anthropic's MCP standard](https://techcrunch.com/2025/03/26/openai-adopts-rival-anthropics-standard-for-connecting-ai-models-to-data/)
- [LavX News, DIYgod/pinky momentum report](https://news.lavx.hu/article/diygod-pinky-lets-ai-agents-operate-phones-under-user-control-2026-11-13)
- [RSSHub GitHub repository](https://github.com/DIYgod/RSSHub)
