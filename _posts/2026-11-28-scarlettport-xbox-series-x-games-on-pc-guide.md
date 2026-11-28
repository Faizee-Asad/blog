---
layout: post
title: "ScarlettPort: Run Xbox Series X|S Games on PC Without an Emulator (2026 Guide)"
description: "ScarlettPort is an open-source compatibility layer that runs an Xbox Series X|S game natively on a Windows PC - no emulator, no streaming. How ScarlettPort works, its current FPS, requirements and install steps."
author: Asad Faizee
date: 2026-11-28 13:20:00 +0530
categories: [Gaming, Open Source]
tags: [scarlettport, xbox series x, pc gaming, compatibility layer, game port]
image:
  path: /assets/img/posts/ScarlettPort_cover.webp
  alt: ScarlettPort - run Xbox Series X|S games on PC without an emulator
---

I was about to skip past this one. "Xbox Series X|S game running on PC" - of course, this is nothing new, and most of the time the end result involves an emulator that manages to hit 11 fps or a cloud stream that immediately dies as soon as there's even a flicker in the Wi-Fi signal.

But then I actually read how ScarlettPort works, and my fingers froze from scrolling.

No emulation here at all. ScarlettPort loads your very own Xbox Series X|S game dump and runs it. Right there on your very own CPU and GPU. Your game, running directly and natively on a Windows PC, through a layer of translation for the code that expects an Xbox.

In all honesty, before I dive deeper, I must tell you I have not tested this on my own computer. Everything that follows is taken from the project's documentation, release notes and the testing results shared by other users. Whenever a number is based on the developer's own testing, I will mention it. However, if we look at it from a purely theoretical angle, this is arguably one of the most fascinating open-source projects I've seen throughout the year - and it showed up, seemingly out of nowhere, on 24 November 2026. Four days later, it had already gained over 1,390 stars on GitHub.

## What is ScarlettPort, in simple terms?

Think of Wine, or Proton - the tools which allow you to play Windows games on Linux, without having Windows installed at all. ScarlettPort is the same idea, but aimed at Xbox games.

ScarlettPort is a compatibility layer for Windows, created using Rust language, licensed under GPL and developed mainly by one developer who clearly did not get much sleep in November. You get a launcher and a runtime. The launcher is the convenient part - point your game folder and press Play. The real magic takes place during the runtime.

Two things you need to understand about it, because they define the entire project:

**It contains nothing from any game.** No code, no assets, no keys, nothing at all. All you need is a dump of a copy you legally purchased, otherwise you have nothing to run. That line matters - it makes all the difference between a tool and a piracy site.

**It's designed for one specific Series X|S game.** Not the whole Xbox library. Not "your entire collection". Just one game, done correctly, instead of five hundred games done poorly. If you are thinking of those community recompilation projects for retro consoles, you are on the right track.

## Why "not an emulator" matters so much

Emulators are slow for a simple reason: they pretend to be the original hardware and translate everything, instruction by instruction, over and over again. It's like trying to read a novel with someone whispering the translation of every single word into your ear. You get the story. But slowly.

ScarlettPort skips all that pretense because — and this is a fact people often forget — an Xbox Series X|S and a gaming PC are practically cousins. Same processor family. Very similar graphics hardware. So:

The game's CPU code is x86-64, which is exactly the language your PC processor speaks. ScarlettPort runs it directly and only steps in when the code asks for Xbox-specific system services.

The graphics are a little more complicated. The console's GPU code is written for AMD's RDNA2 architecture, so the layer recompiles those shaders in real time into DirectX 12 shaders that your own card — NVIDIA or AMD — can run.

No whisper-translator. The book was already written in your native language anyway; someone just had to fix the dialect.

It reminds me of [llm.asm](/posts/llm-asm-qwen3-pure-assembly-guide/), another project I covered recently where most of the magic came from deleting layers instead of adding them. Different world, same instinct.

## Okay, but does it even run? (FPS question)

This is where I stop saying 'cool' and start being blunt, as what we have here is a brilliant proof of concept - not a way of enjoying a game. In terms of the project, its progress stands at about 90%, while most of that 10% left is concerned with performance.

The things that actually work according to the project's development page: the game launches. Menus work fine. Legal screens, saves, everything is okay. It goes into the actual game world (it took about 178 seconds in the developer's test), and once it does, the user has total control in the introductory area. Proper room lighting, full textures, ambient and sky lighting are available. It's exactly like the game.

Frame rate, however. On the developer's test machine, RTX 3060 12 GB + Ryzen 5 5600X, the game had an average of 7-8 frames per second in version 0.5.2. Brutal. The 0.5.6 version, however, managed to do one cool trick and cut off preparation for ray-tracing for a feature which was off in the first place, resulting in 12.2-13.6 FPS in the same scenes. 60% improvement from skipping unnecessary work, though still far from 30 FPS target set by the project, is definitely a step in the right direction.

Since then, version 0.5.7 fixed the most popular cause for crashes – running out of memory in the game until Windows decides to close it – and mouse look functionality was improved by community members. I mean it literally since the thing went from 0.5.0 to 0.5.7 in a matter of days.

## What your PC needs

| Requirement | Details |
|---|---|
| Operating system | Windows 10 or 11, 64-bit |
| CPU | x86-64 with AVX2 (most CPUs from 2013 onward; older Xeons and pre-Haswell Intel chips can't run it) |
| GPU | DirectX 12 with Shader Model 6.6 and mesh shaders - RTX 20-series / Radeon RX 6000 or newer |
| RAM | 32 GB recommended. With 16 GB, set a large fixed page file (32-48 GB) on an SSD - running out of memory is the most common crash |
| Game files | Your own dump of your own legally purchased copy |

## Setting up ScarlettPort takes just two minutes

1. Grab the newest ScarlettPort zip from the official GitHub Releases page. Only from there - I'll explain more on why below.
2. Unzip it and place ScarlettPort.exe and the runtime folder side by side.
3. Run the launcher.
4. Point it to your game folder and press Play.

The initial startup will be slow because the system needs to translate and store the game's shaders in cached information. That's a one-time thing - the cached information is reused on subsequent launches. Don't judge it by the first start-up.

## Legal and safety concerns, since somebody will eventually ask

Is it legal? This particular layer is simply code and has no assets of any kind from the game included in it. It has a "bring your own copy" approach and that puts it on the same ground as all respectable compatibility projects and far away from the download sites of game files. Don't use them. Ever.

Is it safe? This is a question that requires an honest answer, since your antivirus might raise red flags. The builds haven't been code-signed and the runtime loads game code and modifies it in memory, which, from a heuristic point of view, is very similar to what malware does.

The project is aware of this and takes the right measures: the 0.5.5 version was detected by 0 of 68 engines on VirusTotal, Defender included, per the project, SHA-256 checksum is provided for each release, starting with 0.5.6 there is a signed GitHub build attestation confirming that the file was produced via the project's public CI.

The actual risk does not come from the official build. It comes from "ScarlettPort download" links you can find on Discord servers and YouTube videos, and those links may literally contain anything. Official Releases page only.

## What ScarlettPort still can't do

Since I want nobody downloading this package with the wrong set of assumptions, here they are, in one place:

- One game. Other games on your Xbox won't run with this. ScarlettPort is a scalpel, not a Swiss Army Knife.
- 12-14 FPS isn't a gameplay. It's a milestone. There's a difference, and this project doesn't try to hide it.
- Three minutes to get into the world in the current test run. Bring patience, or coffee.
- Only Windows. No Linux, no Steam Deck port at the moment.
- It's a few days old. Expect new versions with bug fixes and sometimes breaking something else. Such is life when you have such a fast-moving project.

## ScarlettPort at a glance

| Item | Details |
|---|---|
| Project | ScarlettPort by ZigZagBoi1 |
| Type | Compatibility layer (not an emulator) |
| Platform | Windows 10/11 PC, running an Xbox Series X|S title |
| Language | Rust |
| License | GPL |
| Status | Boots, playable control in opening area, ~12-14 FPS (RTX 3060 test rig) |
| Price | Free and open source (voluntary donations) |
| Repository | [github.com/ZigZagBoi1/ScarlettPort](https://github.com/ZigZagBoi1/ScarlettPort) |

## Repeated questions

Is ScarlettPort an emulator? Nope - and that's the hill ScarlettPort dies on. Nothing is emulated here. It's the code from the game itself that is executed on your hardware, while the layer converts the stuff that was made specifically for Xbox services and RDNA2 shaders. Sort of like wine for an Xbox game.

What games are supported? One. Just one Series X|S game, designed specifically for this layer. And because the layer comes empty of any game data, "support" also includes your own dump of your game.

Is it free to use? Yes. GPL open-source software. Author asks for voluntary donation but strangely makes it clear that there is nothing you get in return except good will; no access to pre-release builds or anything else.

Why did my antivirus mark it? Because it is an unsigned build and because there is a program which rewrites the code at runtime. Do a simple checksum/verify release attestation from release notes and never download from anywhere else but Releases page.

Steam Deck? Linux? Nope, not yet. Windows/DirectX 12 is the sole platform at the moment.

## My view

If you own the game and have a decent PC, give ScarlettPort a try – not because it is ready, but because watching a Series X|S game start up on your desktop and respond to your controller, without involving a console, is one of those things that reminds you why open source is so much fun. Just go into it with eyes open: you will see a slideshow with excellent lighting, but not a fully finished port.

But keep an eye on the release page. A project that gets 60% more performance by simply removing work, fixes its biggest crash a week later, and starts receiving community contributions just days after launch – that project is going somewhere. My prediction is that the 30 FPS goal will be achieved sooner than people think, and when it does, ScarlettPort stops being an oddity and becomes a genuinely new way to play a console game on PC.
