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

I almost scrolled past this one. "Xbox Series X|S game running on PC" - sure, I've heard that before, and it usually ends with a blurry emulator doing 11 frames per second or a cloud stream that falls apart the moment your Wi-Fi blinks.

Then I actually read what **ScarlettPort** does, and I stopped scrolling.

There's no emulator here at all. ScarlettPort takes your own Xbox Series X|S game dump and just... runs it. On your actual CPU. On your actual graphics card. The game's own code, executing natively on a Windows PC, with a translation layer underneath catching the parts that expect an Xbox.

Full honesty before we go further: I haven't run it on my own machine yet. Everything below comes from the project's documentation, its release notes and the test results people have posted. Where a number comes from the developer's own testing, I'll say so. But even on paper, this is one of the most interesting open-source projects I've seen all year - and it appeared, seemingly out of nowhere, on 24 November 2026. Four days later it had over 1,390 stars on GitHub.

## What is ScarlettPort, in plain words?

Think of Wine, or Proton - the tools that let Windows games run on Linux without a Windows install anywhere in sight. ScarlettPort is that idea, pointed at an Xbox.

It's a compatibility layer for Windows, written in Rust, released under the GPL license, and built mostly by one developer who clearly did not sleep much in November. You get a launcher and a runtime. The launcher is the friendly part - pick your game folder, press Play. The runtime is where the actual sorcery happens.

Two things you need to understand about it, because they define the whole project:

**It contains nothing from any game.** No code, no assets, no keys, nothing. You bring a dump of a copy you legally bought, or you have nothing to run. That line matters - it's the difference between a tool and a piracy site.

**It's built for one specific Series X|S title.** Not the Xbox library. Not "your collection". One game, done properly, instead of five hundred games done badly. If that reminds you of the community recompilation projects for old consoles, you're thinking along the right lines.

## Why "not an emulator" is the whole point

Emulators are slow for a simple reason: they pretend to be the original hardware and translate everything, instruction by instruction, forever. It's like reading a novel through someone who whispers a translation of every single word into your ear. You get the story. Slowly.

ScarlettPort skips the pretending, because - and this is the part people forget - an Xbox Series X|S and a gaming PC are basically cousins. Same processor family. Very similar graphics hardware. So:

The game's CPU code is x86-64, which is exactly what your PC processor speaks. ScarlettPort runs it directly and only steps in where the code asks for Xbox-specific system services.

The graphics are trickier. The console's GPU code is written for AMD's RDNA2 architecture, so the layer recompiles those shaders on the fly into DirectX 12 shaders your own card - NVIDIA or AMD - can execute.

No whisper-translator. The book was almost in your language already; someone just had to fix the dialect.

It reminds me of [llm.asm](/posts/llm-asm-qwen3-pure-assembly-guide/), another project I covered recently where most of the magic came from deleting layers instead of adding them. Different world, same instinct.

## Okay, but does it actually run? (the FPS question)

Here's where I stop being impressed and start being honest, because right now ScarlettPort is a brilliant proof of concept - not a way to comfortably play through a game. The project itself puts its progress at around 90%, and most of that missing 10% is performance.

What genuinely works, per the project's status page: the game boots. Menus work. Legal screens, saving, all fine. It reaches the actual game world - took about 178 seconds in the developer's test - and once you're in, you have full control in the opening area. Rooms are lit properly, textures are complete, ambient and sky lighting are there. It looks like the game. That alone is wild.

The frame rate, though. On the developer's test rig - RTX 3060 12 GB, Ryzen 5 5600X - the world ran at about 7 to 8 FPS in version 0.5.2. Painful. Then version 0.5.6 did something clever: it stopped doing ray-tracing preparation work every frame for a feature that was switched off anyway, and the same scenes jumped to 12.2-13.6 FPS. Sixty percent faster from deleting work. Still not the 30 FPS the project calls its playable goal, but the direction is obvious.

Since then, 0.5.7 has gone after the most common crash - the game eating memory until Windows gives up - and mouse look has been fixed up by community contributors. And I do mean *since then* as in *days*. This thing went from 0.5.0 to 0.5.7 faster than most projects ship a hotfix.

## What your PC needs

| Requirement | Details |
|---|---|
| Operating system | Windows 10 or 11, 64-bit |
| CPU | x86-64 with AVX2 (most CPUs from 2013 onward; older Xeons and pre-Haswell Intel chips can't run it) |
| GPU | DirectX 12 with Shader Model 6.6 and mesh shaders - RTX 20-series / Radeon RX 6000 or newer |
| RAM | 32 GB recommended. With 16 GB, set a large fixed page file (32-48 GB) on an SSD - running out of memory is the most common crash |
| Game files | Your own dump of your own legally purchased copy |

That RAM line is the one that stings. 32 GB as a *recommendation* tells you everything about where the project is right now - it works, but it is not yet polite about resources.

## Installing ScarlettPort takes about two minutes

1. Grab the latest `ScarlettPort-<version>-win64.zip` from the official [GitHub Releases](https://github.com/ZigZagBoi1/ScarlettPort/releases) page. Only from there - more on why below.
2. Extract it, and keep `ScarlettPort.exe` and the `runtime` folder next to each other.
3. Run the launcher.
4. Point it at your game folder and press **Play**.

First launch is slow because the layer has to translate and cache the game's shaders. That's a one-time cost - later starts reuse the cache. Don't judge it by the first boot.

## The legal and safety questions, because someone always asks

Is it legal? The layer itself is just code, and it's deliberately empty of anything belonging to a game. Its entire model is "bring your own copy". That's the same footing as every respected compatibility project, and it's a completely different world from sites handing out game files. Avoid those. Always.

Is it safe? This one deserves a straight answer, because your antivirus may scream at you. The builds aren't code-signed yet, and the runtime loads game code and rewrites parts of it in memory - which, to a heuristic scanner, looks a lot like what malware does. The project knows this and does the right things about it: the 0.5.5 release scored 0 detections out of 68 engines on VirusTotal (Defender included, per the project), every release publishes SHA-256 checksums, and from 0.5.6 there's a signed GitHub build attestation proving the file came out of the project's own public CI.

The real risk isn't the official build. It's the "ScarlettPort download" links floating around Discord servers and video descriptions, which can contain literally anything. Official Releases page or nothing.

## What ScarlettPort can't do yet

Let me collect the caveats in one place, because I don't want anyone downloading this with the wrong expectations:

- **One game.** Your other Xbox titles will not work. This is a scalpel, not a Swiss army knife.
- **12-14 FPS is not a playthrough.** It's a milestone. There's a difference, and the project is upfront about it.
- **Three minutes to reach the world** in the current test results. Bring patience, or coffee.
- **Windows only.** No Linux, no Steam Deck build right now.
- **It's days old.** Expect updates that fix things and occasionally break different things. That's the deal with projects moving this fast.

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

## Questions people keep asking

**Is ScarlettPort an emulator?**
No - and this is the hill the project dies on. Nothing is being imitated. The game's own code runs on your hardware; the layer only translates the bits that were written for Xbox services and RDNA2 shaders. Wine for an Xbox game, roughly speaking.

**Which games work with it?**
One. A single Series X|S title that the layer is purpose-built for. And since it ships with zero game content, "work" also means *your* dump of *your* copy.

**Does it cost anything?**
No. It's GPL open source. The author takes voluntary donations and is oddly clear that they buy you nothing - no early builds, no perks. Just goodwill.

**Why did my antivirus flag it?**
Unsigned build plus a program that rewrites code in memory. That combination trips heuristics everywhere. Verify the checksum or the build attestation from the release notes, and only ever download from the official Releases page.

**Steam Deck? Linux?**
Not today. Windows and DirectX 12 are the whole foundation right now.

## My take

If you own the game and you've got a decent PC, try ScarlettPort - not because it's ready, but because watching a Series X|S title boot on your desktop and respond to your controller, with no console involved, is the kind of thing that reminds you why open source is fun. Just go in with your eyes open: you'll get a slideshow with excellent lighting, not a finished port.

But watch the release page. A project that finds 60% more performance by *deleting* work, fixes its biggest crash a week later, and is pulling in community contributions within days of launch - that project is going somewhere. My bet: the 30 FPS goal falls sooner than people expect, and when it does, ScarlettPort stops being a curiosity and starts being a genuinely new way to play a console game on PC.
