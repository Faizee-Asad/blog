---
layout: post
title: "SuperBot: David Ondrej's open-source Grok Bot alternative you can self-host (2026 guide)"
description: "SuperBot is a free, MIT-licensed Rust project that recreates xAI's Grok Bot as a self-hosted team of AI agents with memory, routines and a shared Linux computer on your VPS. Here's what works today, how to install it, and what to watch out for."
date: 2026-11-18 01:30:00 +0530
author: Asad Faizee

categories:
  - AI Tools
  - Open Source

tags: [SuperBot, Grok Bot alternative, self-hosted AI agents, AI teammates, Rust]

image:
  path: /assets/img/posts/superbot-open-source-ai-team-rust.webp
  alt: "SuperBot web app showing a sidebar of robot AI teammates and a chat with the main bot"

pin: false
comments: true
---

## SuperBot, in one paragraph

SuperBot is a free, open-source project that gives you a team of AI agents with names, jobs, memories and a real computer to work on, self-hosted on your own VPS. It's a Rust-built, MIT-licensed take on xAI's Grok Bot, the subscription-only "always-on AI teammates" product xAI launched in August 2026, and it runs on the coding agent CLIs you already have: Claude Code, Codex or Pi.

David Ondrej, the Czech entrepreneur and AI YouTuber with 424,000 subscribers, built the whole thing in public using coding agents themselves. The repo went live on November 15, 2026 and collected 289 stars and 87 forks in its first days. It's the same self-hosted, inspectable-tools philosophy behind [Atomic Chat](https://blog.asadfaizee.is-a.dev/posts/atomic-chat-offline-ai-chatbot-local-llm/), pointed at a bigger target this time.

## What is SuperBot?

SuperBot is a web app plus a Rust server that turns your agent CLIs into persistent teammates. You message bots like coworkers, hand them real work, and they finish it end to end, asking you only when they need something.

Each bot gets a name, one job, its own chat, and its own memory. You pick from 12 robot shapes and 11 colors when you create one, which sounds cosmetic until you realize how much easier it is to trust "Peanut handles my inbox" than "agent-7f3a."

The key architectural idea: every bot works on one shared Linux computer on your VPS. Files and sign-ins stick around between tasks, so a bot can sign into your tools on Monday and pick up where it left off on Tuesday. That persistence is what separates an agent from a chatbot.

## What is Grok Bot, and why build an alternative?

Grok Bot is xAI's product: a team of always-on AI agents, each with its own cloud computer, launched in beta on August 11, 2026 and built together with Cursor ([xAI announcement](https://x.ai/news/introducing-grok-bot)). Bots sign into your tools, work across apps with no clean API, and keep working while you're away. Access is gated behind SuperGrok and Cursor subscriptions, and the whole thing runs on xAI's infrastructure.

SuperBot replicates the workflow with two big differences: you host it, and it's yours. The README is refreshingly honest about the method. The architecture is a "rough guess" at how Grok Bot works under the hood (one shared Linux VM per account, each bot getting its own little world on it), based on what a Grok Bot said about its own environment. The project states plainly it has no affiliation with xAI, and "Grok" stays a trademark of xAI.

So think of SuperBot as a behavioral reimplementation, and say so when you blog about it. That honesty is part of why the project reads as credible rather than as a clone farm.

## How does SuperBot work?

Four steps, straight from the README:

1. You talk to SuperBot, the main bot, like a chief of staff.
2. It hands work to focused teammates: a Developer bot, a Researcher bot, an Inbox bot.
3. Each bot runs on the agent CLI you assigned to it.
4. They all share one Linux computer on your VPS, with files and sign-ins persisting between tasks.

The clever part is the adapter design. Each agent CLI (Claude Code first, then Codex CLI and Pi Agent) plugs in through one small adapter, and everything else stays identical. Grok Build and Hermes Agent adapters are planned. If a better agent ships next month, SuperBot doesn't care; you swap the adapter.

And yes, the meta-detail is real: David built SuperBot largely with coding agents doing the implementation, 25 commits landing on day one. The project is partly a demo of how far agent-driven development has come.

## What can SuperBot's bots actually do today?

Four things work right now, per the repo's own status notes:

- **Chat with real agents.** Bots run on Claude Code, Codex or Pi and complete multi-step work.
- **Memory that grows.** Bots save your rules and preferences as plain files on disk. Ask "What do you remember about me?" and get a readable answer, stored where you can edit or delete it.
- **Routines.** Bots create their own cron jobs, so a task can run on a schedule without you touching a scheduler.
- **Bot to bot.** Bots message each other for help and pass work along, which is how you get a research bot feeding a writing bot without playing middleman.

In progress: approvals (bots pause before risky actions) and computer use. The repo already has commits for computer use, described as "a screen per Bot, browser, desktop apps, live view," so that layer is landing fast. Slack integration and skills are also on the roadmap.

## How do you install SuperBot?

You need a VPS (or any Linux box), Rust, and an agent CLI account. Then:

    git clone https://github.com/davidondrej/superbot
    cd superbot
    cargo build --release
    cp .env.example .env

Fill in the .env file, source it (`set -a; . ./.env; set +a`), and run `./target/release/botd`. The build produces two binaries: `botd` (the server) and `botctl` (the control CLI). Open the web app, say hi to SuperBot, and make sure the agent CLIs (`claude`, `codex`, `pi`) are installed and logged in as the same user running `botd`.

The docs folder has a backend guide for deploying on a VPS, which is the intended home for this. Since the bots keep files and sign-ins on that machine, a persistent server is the whole point; a laptop that sleeps would break the routines.

Total cost math: SuperBot is free under MIT. You pay for a VPS (a few dollars a month) plus whatever your agent subscriptions or API usage already cost. Compare that with subscription-gated access to a closed product, and the economics explain the 289-star first weekend.

## Is self-hosting SuperBot safe?

Treat it as an early project with real sharp edges, because it is one. The repo's status section says so itself, and a [third-party case study](https://www.ai.joaoqueiros.com/blog/davidondrej-superbot-open-source-grok-bot-rust-vps) published on November 16 makes the same point: approvals and per-bot resource limits aren't built yet, and the project is a hands-on self-hosting experiment rather than an audited production service.

Practical rules if you deploy it:

- Put authentication in front of the web app before it touches the public internet. The build-in-public demo exposed an HTTP deployment briefly before adding a password; learn from that sequence instead of copying it.
- Give the bots a dedicated VPS, one where a mistake costs you a rebuild rather than your main machine.
- The memory is plain files, which is great for inspection and also means whatever the bots can read, they remember. Keep secrets out of that filesystem.
- Wait for the approvals layer before letting bots do anything involving money, email on your behalf, or public posts.

The upside of self-hosting is that every one of these controls is yours to add. The upside of waiting a few weeks is that someone else finds the sharp edges first. Your risk appetite decides.

## What are the honest limitations?

- **Early status.** The README labels it "early" and the feature list tags approvals with "(soon)" for a reason.
- **No xAI code inside.** SuperBot contains zero Grok Bot source; the architecture mapping is David's documented guess at how the original works.
- **Agent subscription costs still apply.** SuperBot orchestrates Claude Code, Codex or Pi; it doesn't include a model.
- **One shared computer.** All bots share a single Linux machine, so one bot's runaway process is every bot's problem until resource limits arrive.
- **Three-day-old project.** Expect the docs, features and architecture to move weekly. The 87 forks suggest plenty of people are already patching things themselves.

## Is SuperBot worth trying in 2026?

Yes, if you want to understand where agent products are heading, because Grok Bot's model (persistent teammates with their own computer, memory and routines) is clearly the shape of the next year, and SuperBot lets you run that shape on hardware you control.

It's also a genuinely good artifact to study: Rust server, adapter pattern for swapping agent CLIs, plain-file memory you can read with `cat`. For developers, the adapter design alone is worth an evening.

Start with a cheap VPS, one Researcher bot, and a routine that summarizes something you actually read daily. Once the approvals layer ships, hand it something riskier. And watch the repo: at this pace, the feature list in my next update will look different.

## FAQ

**Is SuperBot really free?**
Yes. MIT-licensed, self-hosted, no accounts. Your costs are the VPS and the agent CLI subscriptions or API keys you attach to your bots.

**Do I need a Grok subscription to use SuperBot?**
No. SuperBot has no connection to xAI. It runs on Claude Code, Codex or Pi, whichever agent CLIs you install.

**What's the difference between SuperBot and Grok Bot?**
Grok Bot is xAI's closed, subscription-gated product on xAI's cloud. SuperBot is an open-source, self-hosted reimplementation of the same workflow (persistent named bots, shared Linux computer, memory, routines), built entirely from public behavior.

**Can SuperBot bots talk to each other?**
Yes. Bot-to-bot messaging works today: bots ask each other for help and pass work along in threads.

## Sources

- [SuperBot GitHub repository](https://github.com/davidondrej/superbot)
- [xAI, Introducing Grok Bot (August 11, 2026)](https://x.ai/news/introducing-grok-bot)
- [João Queirós, SuperBot case study (November 16, 2026)](https://www.ai.joaoqueiros.com/blog/davidondrej-superbot-open-source-grok-bot-rust-vps)
- [David Ondrej on YouTube](https://www.youtube.com/@DavidOndrej)
