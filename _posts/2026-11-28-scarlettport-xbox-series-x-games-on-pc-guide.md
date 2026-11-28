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

Xbox games on PC usually means one of three things: an official PC port, cloud streaming, or an emulator that struggles to reach playable speed. **ScarlettPort** is a fourth option, and it is the most interesting one. It is an open-source compatibility layer that takes your own Xbox Series X|S game dump and runs it **natively** on a Windows PC - the game's own code executes directly on your CPU and graphics card, with no emulator in between.

The project appeared on GitHub on 24 November 2026, built mainly by a single developer, and it has already crossed **1,390 stars**. I went through the documentation, the release notes and the reported test results in detail. Here is what it actually is, how well it runs today, and what you need to try it.

## What is ScarlettPort?

ScarlettPort is a bring-your-own-copy compatibility layer for Windows, written in Rust and released under the GPL license. The idea is the same one behind Wine and Proton: instead of pretending to be the original hardware, you translate what the game asks for into something the PC can do directly.

Two details matter here:

- **Nothing of the game is included.** The download contains no game code, no assets and no keys. You supply a dump of a copy you legally own. Without your own files, there is nothing to run.
- **It is built around one specific Xbox Series X|S title**, not the whole Xbox library. This is a focused port layer for a single game, closer to the community recompilation projects for classic consoles than to a general-purpose emulator.

It ships as two parts: a launcher (`ScarlettPort.exe`) where you pick your game folder and press Play, and the runtime that does the actual translation work underneath.

## How ScarlettPort works (and why it is not an emulator)

An emulator imitates the console's hardware instruction by instruction, which is why emulation is usually slow. This project skips the imitation entirely, because the Xbox Series X|S and a gaming PC are more alike than most people realise:

- **CPU side:** the console's processor is x86-64, the same instruction set as a PC CPU. It runs the game's x64 code directly on your processor, patching the parts that expect Xbox system services.
- **GPU side:** the console's graphics code is written for AMD's RDNA2 architecture. It recompiles those shaders on the fly into DirectX 12 shaders your own graphics card understands - whether that card is NVIDIA or AMD.

That native approach is why a project like this can even dream of full speed, and it puts ScarlettPort in the same family as another remarkable one-person engineering project I covered recently, [llm.asm](/posts/llm-asm-qwen3-pure-assembly-guide/) - different field, same spirit of removing every layer between the software and the machine.

## ScarlettPort status: can you actually play yet?

This is the honest part, because it is a work in progress - the project itself rates its progress at about **90%**, and the remaining 10% is mostly performance.

What works today, from the project's own status reports:

- The game **boots, passes the menus, legal screens and saving**, and reaches the game world - in about **178 seconds** on the developer's test setup with the current build.
- You get **full control in the opening area**, with complete rendering: lit rooms, ambient and sky lighting, full textures.
- Recent updates fixed the most common crash (running out of memory) and added proper mouse look.

Performance is the catch. On an RTX 3060 12 GB with a Ryzen 5 5600X, the game world ran at roughly **7-8 FPS** in version 0.5.2. Version 0.5.6 then skipped ray-tracing preparation work that was being done even with ray tracing off, and the same scenes jumped to **12.2-13.6 FPS** - about 60% faster, but still short of the project's **30 FPS playable goal**. Long-session stability is also unproven; no marathon runs have been recorded yet.

The pace of development is unusually fast: the project went from 0.5.0 to 0.5.7 in a matter of days, with community pull requests landing alongside the author's own fixes.

## ScarlettPort requirements

| Requirement | Details |
|---|---|
| Operating system | Windows 10 or 11, 64-bit |
| CPU | x86-64 with AVX2 (most CPUs from 2013 onward; older Xeons and pre-Haswell Intel chips cannot run it) |
| GPU | DirectX 12 with Shader Model 6.6 and mesh shaders - RTX 20-series / Radeon RX 6000 or newer |
| RAM | 32 GB recommended. With 16 GB, set a large fixed page file (32-48 GB) on an SSD - running out of memory is the most common crash |
| Game files | Your own dump of your own legally purchased copy |

## How to install ScarlettPort

1. Download the latest `ScarlettPort-<version>-win64.zip` from the project's official [GitHub Releases](https://github.com/ZigZagBoi1/ScarlettPort/releases) page - nowhere else.
2. Right-click the ZIP and choose **Extract All**, keeping `ScarlettPort.exe` and the `runtime` folder side by side.
3. Run `ScarlettPort.exe`.
4. Point it at your game folder. It prepares and caches the translated shaders once, then press **Play**.

That first shader preparation is a one-time cost; later launches reuse the cache and are faster.

## Is ScarlettPort legal? Is it safe?

**Legality:** the layer itself is just code, distributed free under the GPL, and it deliberately includes nothing from any game. Its model is "bring your own copy" - you run a dump of a game you bought. That is the same approach used by respected compatibility projects, and it is very different from sites that hand out game files, which you should avoid entirely.

**Safety:** the builds are not code-signed yet, and the runtime loads and rewrites game code in memory - behaviour that makes some antivirus heuristics shout "Trojan". The project addresses this directly:

- The official 0.5.5 release scored **0 detections out of 68 engines** on VirusTotal, including Microsoft Defender, according to the project.
- Every release lists **SHA-256 checksums**, and from 0.5.6 onward carries a signed GitHub build attestation, so you can verify your download was built by the project's own CI from public source.
- Copies reposted on random sites, Discord servers or video descriptions can be modified - the project's warning is blunt: only download from the official Releases page.

## Honest limitations

- **One title, not your whole library.** ScarlettPort targets a single Series X|S game. It will not run your other Xbox games.
- **Not yet smooth.** Roughly 12-14 FPS in the world on a mid-range GPU is a proof of concept, not a comfortable playthrough. The 30 FPS goal is still ahead.
- **Slow first load.** Reaching the world took about three minutes in the reported test.
- **Hungry for RAM.** 32 GB is genuinely recommended; memory exhaustion is the top crash.
- **Windows only, and very new.** The project is days old, so expect rough edges, rapid updates and the occasional broken build.

## Key facts

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

## Frequently asked questions

**Is ScarlettPort an emulator?**
No. An emulator imitates the console's hardware. This layer runs the game's own x86-64 code directly on your PC's CPU and translates its RDNA2 shaders into DirectX 12 for your graphics card, the same philosophy as Wine and Proton.

**Which games does ScarlettPort support?**
One specific Xbox Series X|S title. It is a focused port layer for that game, not a general Xbox emulator, and it includes no game content - you must supply your own dump of your own copy.

**Is ScarlettPort free?**
Yes. It is open source under the GPL license. The author accepts voluntary donations, which buy nothing extra - no builds, access or perks.

**Why does my antivirus flag ScarlettPort?**
The releases are not code-signed yet, and the runtime rewrites game code in memory, which resembles malware behaviour to heuristic scanners. Download only from the official GitHub Releases page and verify the SHA-256 checksum or build attestation published with each release.

**Can ScarlettPort run on Linux or Steam Deck?**
Not currently. It is a Windows 10/11 program built around DirectX 12.

## Bottom line

ScarlettPort is not the way to play this game comfortably - not yet. At 12-14 frames per second it is a demonstration, not a replacement for the console. But as a demonstration it is genuinely impressive: a Series X|S title booting on a PC, rendering its world correctly and answering your controller, through a translation layer written mostly by one person in Rust. The project is improving in days, not months - one update already bought 60% more performance. If you follow PC gaming or emulation, ScarlettPort is the project to watch this year, and if you own the game and a capable PC, it is absolutely worth trying for yourself.
