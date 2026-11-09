---
layout: post

title: "Matt Pocock's Skills: 27 AI Agent Skills for Claude Code, Codex & Copilot"
description: "Explore Matt Pocock's 27 open-source agent skills for Claude Code, Codex, and Copilot. Learn what each skill does, how to install them, and how to improve AI coding workflows."

date: 2026-11-09 17:48:00 +0530
author: Asad Faizee

categories:
  - AI Tools
  - Developer Tools

tags:
  - Matt Pocock
  - Matt Pocock skills
  - Claude Code skills
  - AI agent skills
  - coding agents
  - Codex
  - GitHub Copilot
  - Cursor AI
  - Gemini CLI
  - AI coding tools
  - open source AI
  - developer productivity
  - grill-me skill
  - grill-with-docs
  - TDD
  - code review
  - AI development workflow
  - agentic coding
  - GitHub

image:
  path: /assets/img/posts/matt-pocock-skills-claude-code-codex.webp
  alt: "Matt Pocock's open-source collection of 27 AI agent skills for Claude Code, Codex, and Copilot"

pin: false
comments: true
---

# Matt Pocock's skills repo: 27 agent skills for Claude Code and Codex, explained

The most popular skill in Matt Pocock's repo is about 10 lines long, and its whole job is to interview you. You type `/grill-me`, and your coding agent asks questions about your plan until every open decision is settled. That one habit helped push [mattpocock/skills](https://github.com/mattpocock/skills) past 307,000 GitHub stars.

The repo holds 27 small, MIT-licensed skills for Claude Code, Codex, Copilot, Cursor, and other agents. They aim at 4 failure modes Pocock kept hitting with coding agents.

Here's what each skill does, how to install them, and what to watch for.

---

## What is the mattpocock/skills repo?

It's a public collection of agent skills, which are reusable Markdown instruction files that tell a coding agent how to do a specific job. Pocock, a TypeScript educator, publishes them straight from his own `.agents` directory.

The repo sits at [github.com/mattpocock/skills](https://github.com/mattpocock/skills). The README calls the project "Skills for Real Engineers" and describes the skills as small, easy to adapt, and composable. They work with any model.

Current stats from the repo page: 307.2k stars, 25.8k forks, 551 commits, MIT license, 171 open issues, and 21 open pull requests. Only collaborators can open PRs.

---

## Why did the repo take off?

It gives you control. The README names 3 process frameworks (GSD, BMAD, and Spec-Kit) and argues that they own the whole workflow, which makes bugs in that workflow hard to fix. These skills do the opposite. You can read them, edit them, and swap them.

The repo also has an audience. The README says about 60,000 developers subscribe to Pocock's newsletter for updates. [The skills page on AI Hero](https://aihero.dev/skills) lists a changelog that moves fast: v1 on June 18, 2026, v1.1 on July 8, v1.2 on August 5, and a v1.3 that added `/implement-spec`, `/pr`, and `/retro`.

Third-party directories show the same pull. [One skills directory](https://llmbase.ai/skills/mattpocock/grill-me/) lists `grill-me` at 1.4M installs and number 2 of 111 skills. Treat directory numbers as rough, since each site counts differently.

---

## What problems do these skills solve?

The README organizes everything around 4 failure modes. Each one comes with a fix, and each fix ships as one or more skills.

### The agent didn't build what you wanted

The problem is a mismatch between you and the agent. You think it understands. Then you see the result and realize it didn't.

The fix is a grilling session. `/grill-me` works for any plan, and `/grill-with-docs` does the same job for code projects and adds documentation. The README says to run one every time you want to make a change.

### The agent is way too verbose

Agents land in a project and have to guess the jargon. The README says they end up using 20 words where 1 will do.

The fix is a shared language, kept in a `GLOSSARY.md` file. The README shows a before-and-after: a long sentence about a lesson becoming "real" in the file system shrinks to a short phrase about the "materialization cascade." Consistent names also make a codebase easier for an agent to navigate and cut the tokens it spends on thinking.

`/grill-with-docs` builds this glossary as you talk, and it records hard-to-explain decisions in ADRs (architecture decision records). Pocock calls it possibly the coolest technique in the repo.

### The code doesn't work

An agent with no feedback flies blind. The README points to the usual loops: static types, browser access, and automated tests.

`/tdd` pushes the agent through red-green-refactor, where it writes a failing test first and then fixes it. `/diagnosing-bugs` wraps debugging into a loop gated phase by phase: build a feedback loop that fails on the bug, minimize it, form a hypothesis, instrument, fix, and add a regression test.

### The codebase turns into a ball of mud

Agents write code fast, and the README says that speeds up software entropy too. The fix is to care about design. `/to-spec` quizzes you about which modules a change touches before it writes a spec.

`/improve-codebase-architecture` surveys your code for deepening opportunities, meaning spots where a lot of behavior could sit behind a small interface. The README suggests running it every few days. It also says plainly that the skill is a survey and not a rescue.

---

## What's the difference between user-invoked and model-invoked skills?

User-invoked skills run only when you type them. Model-invoked skills can also be picked up by the agent on its own when the task fits.

The README explains the split. User-invoked skills orchestrate, like `/grill-me`. Model-invoked skills hold reusable discipline, like `/tdd`. A user-invoked skill may call model-invoked ones, but never another user-invoked one.

That rule keeps the design clean. It also means some skills fire without you typing anything, so read the model-invoked ones before you trust them in a shared repo.

---

## Which skills does the repo include?

The README lists 27 skills across 2 groups: 20 engineering skills and 7 productivity skills.

| Group | Type | Skills |
|---|---|---|
| Engineering | User-invoked (11) | ask-matt, grill-with-docs, triage, improve-codebase-architecture, setup-matt-pocock-skills, to-spec, to-tickets, implement, implement-spec, wayfinder, retro |
| Engineering | Model-invoked (9) | prototype, diagnosing-bugs, research, tdd, domain-modeling, codebase-design, code-review, pr, wizard |
| Productivity | User-invoked (5) | grill-me, handoff, teach, to-questionnaire, wait-what |
| Productivity | Model-invoked (2) | grilling, writing-for-agents |

A few worth knowing about:

- **`/ask-matt`** routes you to the right skill for your situation.
- **`/handoff`** compacts a long session into a document so another agent can pick up the work.
- **`/wait-what`** re-pitches a confusing message in plain English, using your glossary terms.
- **`/code-review`** reviews a diff on 2 axes, repo standards and fidelity to the spec, using parallel sub-agents so neither pollutes the other.
- **`/wayfinder`** plans work too big for one session as a map of decision tickets on your issue tracker.

---

## How do you install Matt Pocock's skills?

Pick one install route per agent. The README warns that installing both the plugin and the editable copy gives you every skill twice.

### Claude Code

```bash
claude plugin install mattpocock-skills@claude-plugins-official
```

The plugin updates itself by default.

### Codex

```bash
codex plugin marketplace add mattpocock/skills
codex plugin add mattpocock-skills@mattpocock
```

It updates at startup.

### GitHub Copilot (CLI)

```bash
copilot plugin marketplace add mattpocock/skills
copilot plugin install mattpocock-skills@mattpocock
```

Then add the `extraKnownMarketplaces` block from the README to `~/.copilot/settings.json`. In VS Code, run Chat: Install Plugin From Source and enter the repo URL.

### Any other agent, or editable files

```bash
npx skills@latest add mattpocock/skills -a <agent>
```

The `-a` flag accepts agents like cursor, opencode, devin, windsurf, amp, and pi. When the installer asks which skills to take, include `setup-matt-pocock-skills`. Update later with `npx skills@latest update`.

Gemini CLI has its own manual commands in the README, and you re-run them to update.

The tradeoff is simple. A plugin updates itself. The `npx` route copies plain files into your project that you can edit, and you pull updates by hand.

---

## What does /setup-matt-pocock-skills do?

It configures a repo for the engineering skills. You run it once per repo, before anything else.

It asks 3 things: which issue tracker you use (GitHub, GitLab, local files, or something you describe), which labels you apply when triaging tickets, and where to save the docs it creates. Skills like `/triage` and `/to-tickets` depend on those answers.

---

## What does a workflow with these skills look like?

The README doesn't prescribe one fixed order, but the skill descriptions fit together into a natural pipeline. Here's one way to chain them:

1. Run `/grill-with-docs` to settle the plan and update your glossary and ADRs.
2. Run `/to-spec` to turn the conversation into a spec on your issue tracker.
3. Run `/to-tickets` to split the spec into tracer-bullet tickets with declared blockers.
4. Run `/implement`, which drives `/tdd` and closes with `/code-review` before committing.
5. Use `/pr` for a pull request body with a summary, before-and-after evidence, and a merge-risk call.

For bigger work, `/implement-spec` builds a whole spec on one integration branch and runs sub-agents across the tickets that are ready. Start with the smaller flow and add pieces as you need them.

---

## What are the limits of the skills repo?

These skills reflect one engineer's workflow, and the README doesn't publish benchmarks. Check these before you adopt them:

- **Opinions come built in.** The skills lean on TDD, domain-driven design, and deep modules. If your team works differently, edit them.
- **Plugin installs can't be edited.** Third-party coverage describes the Claude Code plugin as managed and read-only. Use the `npx` route if you want to change files.
- **The architecture skill is a survey.** On a genuinely old codebase it finds real candidates, but it won't untangle the mess for you.
- **Open issues.** The repo has 171, so check which ones touch your agent.
- **Counts differ by source.** Some third-party articles quote different skill totals. The README lists 27, so I used that number.

---

## Who should use Matt Pocock's skills?

Developers who already use Claude Code, Codex, or Copilot and keep getting wrong or bloated output. Teams that want a shared vocabulary between people and agents. Anyone who prefers small editable files over a framework that owns the process.

Skip them if you want a single one-click workflow with no opinions. The repo expects you to read, adapt, and sometimes rewrite what you install.

---

## Frequently asked questions

### Are Matt Pocock's skills free?

Yes. The repo uses the MIT license, so you can use and modify the skills freely.

### Do they only work with Claude Code?

No. The README lists install routes for Claude Code, Codex, GitHub Copilot, and Gemini CLI, and the `npx` installer covers Cursor, opencode, Devin, Windsurf, Amp, and Pi.

### What should I install first?

Install the set, run `/setup-matt-pocock-skills` once, then try `/grill-me` on your next plan. The README calls the grilling skills the most popular ones.

### Can I edit the skills?

Yes, if you install through `npx skills`. That copies plain files into your repo. The plugin route stays managed.

---

## Key facts at a glance

- **Repo:** [github.com/mattpocock/skills](https://github.com/mattpocock/skills), MIT license
- **Stats:** 307.2k stars, 25.8k forks, 551 commits, 171 open issues
- **Skills:** 27 total, 20 engineering and 7 productivity
- **Types:** user-invoked skills run when you type them, model-invoked skills can also trigger automatically
- **Four problems solved:** wrong output, verbosity, broken code, and design decay
- **Top skills:** `/grill-me` for any plan, `/grill-with-docs` for code projects
- **Shared language:** `GLOSSARY.md` plus ADRs, built by `/grill-with-docs`
- **Install:** Claude Code plugin, Codex, Copilot, Gemini CLI, or `npx skills@latest add mattpocock/skills`
- **First step:** run `/setup-matt-pocock-skills` once per repo
- **Newsletter:** about 60,000 subscribers, per the README
- **Changelog:** v1 on June 18, 2026, v1.3 added `/implement-spec`, `/pr`, and `/retro`
- **Course:** the free 25-lesson series is on [AI Hero](https://www.aihero.dev/skills-catalog)
