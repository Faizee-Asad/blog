---
layout: post
title: "llm.asm: a Working LLM Written in Pure Assembly - 14 KB, Zero Dependencies (2026 Guide)"
description: "llm.asm runs Qwen3-0.6B, boots as its own operating system and even trains a Mini-GPT - all in hand-written x86-64 assembly with no C, no libc and no dependencies. What it does, how fast it is, and how to run it."
author: Asad Faizee
date: 2026-11-28 03:05:00 +0530
categories: [AI Tools, Open Source]
tags: [llm.asm, nasm, assembly, qwen3, local llm]
image:
  path: /assets/img/posts/llm.webp
  alt: llm.asm - a large language model written in pure x86-64 assembly with zero dependencies
---

Most local LLM tools are big. llama.cpp is hundreds of thousands of lines of C++. Python runtimes drag in gigabytes of libraries before a single token is generated. So when I first saw a project claiming to run Qwen3 in **pure assembly language** - no C, no libc, no runtime at all - I assumed it was a toy demo that prints a few tokens and stops.

It is not a toy. llm.asm is a small collection of complete, working LLM projects written in hand-written x86-64 NASM assembly, and the main inference binary is **14,712 bytes**. For comparison, that is smaller than most of the images on this blog post's page. I spent time reading the code and the documentation, and I liked the project enough to contribute a small documentation fix to it myself (more on that below).

## What is llm.asm?

[llm.asm](https://github.com/alesha-pro/llm.asm) is an open-source project by GitHub user **alesha-pro**, released under the MIT license. It appeared on 25 November 2026 and crossed **2,100 stars in about three days**, which tells you how unusual it is.

Everything in the first three projects is assembly: the math functions (`exp`, `sin`/`cos`, softmax, RMSNorm, RoPE, SiLU), the AVX2 matrix engine, the BPE tokenizer that turns text into tokens, and even the thread pool built directly on Linux `clone()` and `futex()` syscalls. There is no C library underneath doing the heavy lifting - the code talks to the CPU and the kernel directly. Across the three original projects that is roughly **17,252 lines of NASM**.

## The projects inside

### 01 - Qwen3 inference in a 14 KB binary

The headline project is a static Linux program that runs **Qwen3-0.6B** (Q8_0 quantization) with a built-in tokenizer, grouped-query attention, RoPE and a multi-turn `--chat` mode that keeps a KV cache between turns.

The numbers below are from the project's own benchmark, run on an AMD EPYC server CPU against llama.cpp using the same model and the same Q8_0 quantization, CPU-only:

| Setup | Speed |
|---|---|
| llm.asm, 1 thread | 20.7 tokens/second |
| llm.asm, 40 threads | 126.8 tokens/second |
| vs llama.cpp (same model, CPU) | **18-36% faster** across 1-40 threads |

The project also reports a 0.99965 cosine similarity against PyTorch FP32 outputs and 64 out of 64 exact greedy tokens in its correctness test. Treat these as the author's own measurements on server hardware - your laptop will be slower - but even with that caution, an assembly binary beating a mature C++ engine at all is remarkable.

One honest clarification: the **binary** is 14 KB, but the model weights are not. The Qwen3-0.6B weights file is about 614 MB, converted from GGUF into the project's own `.asmbin` format with a script the repo provides.

### 02 - An LLM that is its own operating system

The second project removes Linux from the picture entirely. It is a **24 KB bootable operating system** that loads Qwen3-0.6B on bare metal: it boots in roughly 74-94 milliseconds, brings up multiple CPU cores by itself, and gives you a working multi-turn chat on a plain VGA screen with a PS/2 keyboard. In the author's KVM tests it reached about 134.9 tokens/second. There is no kernel, no drivers folder, no userspace - just the model and the machine.

### 03 - A GPT trainer, also in assembly

Inference is only half of machine learning, so the third project trains. It is a **33 KB Mini-GPT trainer** with a full hand-written backward pass and AdamW optimizer, training at roughly 27,662 tokens/second in the author's tests and taking TinyShakespeare from a validation loss of 4.19 down to 1.51 in about four minutes - while drawing a live training graph in the terminal using Braille characters. Its gradients were checked against PyTorch and matched to a cosine of 1.0000000000 across all 21 gradient tensors.

### 04 - The newest addition: Ring -1 Kernel Surgeon

A fourth project has just landed and it is the most experimental of the set: a small hypervisor that sits *underneath* a running Linux kernel, watches for fatal crashes, and uses an embedded 2B model to diagnose the fault and propose a validated patch - without rebooting. Unlike the first three, this one mixes some freestanding C in with the assembly, and it is very much a research prototype, but it shows where the author is taking the idea.

## How can assembly do all this with no libraries?

Three things make it possible:

1. **Direct syscalls.** On Linux, a program does not actually need libc to read files, allocate memory or print text - libc is mostly a wrapper around kernel syscalls. llm.asm calls the kernel itself.
2. **Hand-written math.** Every function a transformer needs - exponentials for softmax, sine/cosine for RoPE, square roots for normalization - is implemented from scratch in assembly.
3. **Vector instructions.** The heavy matrix work uses AVX2, FMA and F16C instructions, processing many numbers per instruction. This is also why the speed is competitive: at this level there is almost nothing between the algorithm and the CPU.

If the internals of transformers are new to you, my beginner-friendly explainer on [how LLMs actually work](/posts/how-do-llm-actually-work/) covers attention, tokens and KV cache in plain language - llm.asm implements exactly those ideas, just at the lowest possible level.

## How to run it

You need a 64-bit Linux machine (or VM) with an x86-64 CPU that supports AVX2, FMA and F16C - most Intel and AMD processors from the last decade qualify. Then:

```bash
sudo apt install nasm binutils git
git clone https://github.com/alesha-pro/llm.asm
cd llm.asm/01-qwen3-inference
make
```

After building, convert the Qwen3-0.6B GGUF weights to the project's `.asmbin` format using the conversion script in the repo, and start the binary from the `build/` folder. Add the `--chat` flag for the multi-turn chat mode. The whole build is just `nasm` plus `ld` - there is nothing else to install, which still feels strange to type.

If running models locally is your thing, you may also like my guide to [running Kimi K3 GGUF on a normal PC](/posts/kimi-k3-gguf-how-to-run-the-worlds-largest-open-weight-ai-model-locally/) - the opposite end of the size spectrum.

## My small contribution to the project

While reading the chat-mode documentation I found a behaviour that was not written down anywhere: in `--chat` mode, the model's `<think>` blocks stay in the conversation history, so they keep influencing later replies. I opened [pull request #21](https://github.com/alesha-pro/llm.asm/pull/21) to document exactly that in the inference project's README. It is a tiny change, but it is real, it came from actually using the project, and it is the kind of detail that saves the next user an hour of confusion.

## Honest limitations

- **One model family for now.** Projects 01 and 02 are built around Qwen3-0.6B. This is not a general replacement for llama.cpp, which runs thousands of models - including huge ones like the ones in my [DeepSeek-V4 Flash GGUF guide](/posts/deepseek-v4-flash-0731-gguf-run-284b-ai-model-locally/).
- **x86-64 Linux only.** No ARM Macs, no Windows build for the inference project.
- **Benchmarks are the author's own**, measured on a many-core EPYC server. Expect lower numbers on consumer hardware.
- **The project is days old and moving fast.** File sizes and features have already changed once since release, so check the repo for current numbers.
- **Assembly is hard to audit.** A 14 KB binary is easy to download and hard to fully verify by reading. Run it like you would any young open-source project: in a VM first, if you are cautious.

## Key facts

| Item | Details |
|---|---|
| Project | llm.asm by alesha-pro |
| Language | 100% x86-64 NASM assembly (projects 01-03) |
| Dependencies | None - no C, no libc, no runtime |
| Inference binary | 14,712 bytes, runs Qwen3-0.6B (Q8_0) |
| Speed (author's benchmark) | 126.8 tok/s at 40 threads; 18-36% faster than llama.cpp CPU |
| Bare-metal OS | 24 KB bootable image, ~74-94 ms boot |
| Trainer | 33 KB Mini-GPT trainer with live Braille TUI |
| License | MIT |
| Repository | [github.com/alesha-pro/llm.asm](https://github.com/alesha-pro/llm.asm) |

## Frequently asked questions

**Is llm.asm really faster than llama.cpp?**
In the author's own head-to-head test on an AMD EPYC server, with the same Qwen3-0.6B Q8_0 model on CPU only, yes - by about 18-36% depending on thread count. Independent benchmarks on consumer hardware are still rare because the project is new.

**Do I need a GPU?**
No. Everything runs on the CPU, using AVX2 vector instructions. There is no GPU code at all.

**Can it run other models like Llama or DeepSeek?**
Not today. The inference and OS projects target Qwen3-0.6B's architecture specifically. General model support is llama.cpp's strength, not this project's.

**Is it safe to run?**
It is MIT-licensed open source and the code is public, but it is also only a few days old and written in assembly, which few people can audit line by line. Trying it inside a virtual machine first is a sensible approach.

## Bottom line

llm.asm is not going to replace llama.cpp or PyTorch, and it does not try to. What it proves is more interesting: a modern transformer model does not actually need a mountain of software underneath it. Seventeen thousand lines of assembly, a 14 KB binary and zero dependencies are enough to run, boot and even train a language model - and occasionally outrun the tools everyone assumed were required. As a piece of engineering, it is one of the most impressive open-source releases I have seen this year.
