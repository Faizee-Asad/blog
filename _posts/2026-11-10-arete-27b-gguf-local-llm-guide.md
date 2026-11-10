---
layout: post
title: "Arete-27B-GGUF: run Nous Research's 27B value-tuned model as a local LLM (2026 guide)"
description: "Arete-27B-GGUF brings Nous Research's Qwen3.8-27B fine-tune to llama.cpp runtimes in quants from 13.6 GB to 28.6 GB. See what the fine-tune changed, which quant fits your RAM, and how to run Arete 27B offline."
date: 2026-11-10 18:35:00 +0530
author: Asad Faizee
categories:
  - AI Tools
  - Local LLMs
tags:
  - Arete-27B
  - Arete-27B-GGUF
  - Nous Research
  - AtomicChat
  - local LLM
  - GGUF
  - quantization
  - Qwen3.8
  - fine-tuning
  - value alignment
  - offline AI
  - llama.cpp
  - open source AI
  - Hugging Face
  - run LLM locally
  - AI models
image:
  path: /assets/img/posts/arete-27b-gguf-local-llm.webp
  alt: "Arete-27B-GGUF quantization sizes for running the Arete 27B local LLM offline on your own hardware"
pin: false
comments: true
---

A new 27B model just made the "whose values does my AI ship with" debate practical. Arete-27B is Nous Research's fine-tune of Alibaba's Qwen3.8-27B, post-trained to change how it answers politically sensitive topics: fact scores rose from 2.32 to 2.98 out of 5 on Nous's own eval, and answers adopting Chinese state framing dropped from 22% to 0.9%. The [AtomicChat GGUF release](https://huggingface.co/AtomicChat/Arete-27B-GGUF) now runs it on your own hardware, in files from 13.6 GB to 28.6 GB.

If you followed my guide on [running a local LLM offline with Atomic Chat](https://blog.asadfaizee.is-a.dev/posts/atomic-chat-offline-ai-chatbot-local-llm/), this model is exactly what that pipeline is for: open weights, measured quants, one-click download. This guide covers what Arete-27B is, what the fine-tuning actually changed, which GGUF file fits your RAM, and how to run it today.

## What is Arete-27B?

Arete-27B is a 27-billion-parameter model from Nous Research, built by supervised fine-tuning of Alibaba's Qwen3.8-27B. It's Apache-2.0 licensed, and the trifecta is public: the [weights](https://huggingface.co/NousResearch/Arete-27B), the training data (arete-data-pairs) and the evaluation (Propaganda_Eval) ([Nous Research](https://nousresearch.com/arete)).

The name is a Greek pun. Arete (ἀρετή) is the ancient concept of excellence and virtue; an arête is also the sharp rock ridge between two glacial valleys, which is what the blog's cover image shows. A model that sits between two worldviews, more or less.

The training itself is refreshingly boring: 52.8 million tokens at 64k context, 2 epochs, 1,202 steps, supervised fine-tuning only. No reinforcement learning, no preference optimization. The teacher was Inkling, an open model Nous judged best on the target behaviors, which also sidesteps the "illicit distillation" terms-of-service problems a closed teacher would create.

One technical detail matters for local users: Qwen3.8-27B is a vision-language model, and Nous froze the vision encoder during training (the data was text-only). Arete-27B still accepts images, and the GGUF release ships the matching vision projector file.

## Why fine-tune a Chinese open model at all?

Nous's argument, stated in their blog: open-weight Chinese models are among the strongest available, but they ship with state-aligned behavior baked into the weights. The US government has restricted Chinese AI models, DeepSeek especially, from federal use while issuing broader warnings. And studies cited by Nous found that geopolitical context in a prompt can affect code quality, with Chinese models producing more vulnerable code when they believe they are acting on behalf of an American entity.

Open weights cut both ways, and that's the engineering point here. Any lab with the resources can take a strong base and reshape its defaults. Nous trained against seven value categories: freedom of expression, state ideology, state versus free market, education, data privacy and surveillance, democracy, and environmental policy.

A neutral writer's note: this is one lab's chosen answer to what a model should sound like, published openly so anyone can inspect, rerun or disagree with it. That openness is the story as much as the model is.

## What did the fine-tuning actually change?

The eval, in short: 803 prompts across the seven categories, each prompt run five times, graded by a blind LLM judge (Grok 4.7) against a fixed rubric with 2-4 required facts per prompt ([Nous Research](https://nousresearch.com/arete)).

The headline numbers:

- **Fact score: 2.32 → 2.98 out of 5** (Qwen3.8-27B base vs Arete-27B, thinking off)
- **State-framed answers: 22% → 0.9%**
- For comparison on the same eval: GLM 5.3 scored 15%, DeepSeek V4.1 Flash 16%, Mistral Large 4 landed at 3.65 with 0.3%, and the teacher Inkling topped the table at 3.96.

Two honest observations from that table. First, 2.98 out of 5 keeps Arete mid-pack on factual recall, below Mistral Large 4 and its own teacher; the dramatic change is the framing collapse, from 22% down to under 1%. Second, Nous reports no meaningful regression on general-capability and agentic benchmarks, but the eval and the judge are theirs. The saving grace: everything is published, so the claims are checkable rather than a press release.

## What is the AtomicChat Arete-27B-GGUF release?

It's the GGUF conversion of Arete-27B, published by the AtomicChat organization on Hugging Face, with every file measured for speed and quality. The repo went live in early November 2026 and passed roughly 1,200 downloads in its first few days ([AtomicChat on Hugging Face](https://huggingface.co/AtomicChat)).

The org's habit is worth knowing: it converts popular open models to GGUF, measures each quant, and publishes the numbers, across 130 models and 56 collections so far. If you have ever downloaded a quant and wondered whether Q4_K_M actually cost you anything, this is the pipeline that answers that.

The Arete files on offer:

| File | Size | Comfortable RAM |
| --- | --- | --- |
| Arete-27B-AD-IQ3_S.gguf | 13.6 GB | 16 GB (tight) |
| Arete-27B-AD-IQ4_XS.gguf | 16.2 GB | 24 GB |
| Arete-27B-AD-Q4_K_M.gguf | 16.8 GB | 24-32 GB |
| Arete-27B-AD-Q5_K_M.gguf | 19.9 GB | 32 GB |
| Arete-27B-AD-Q6_K.gguf | 24.6 GB | 32 GB+ |
| Arete-27B-Q8_0.gguf | 28.6 GB | 48 GB |

Add the vision projector (mmproj-Arete-27B, about 0.9 GB in BF16 or F16) if you want image input. The projector is small; the language model is what eats your RAM.

## Which GGUF quant should you pick?

Start with Q4_K_M. It's the consensus sweet spot for quality-per-gigabyte across the llama.cpp world, and at 16.8 GB it fits a 24 GB machine with room for context. IQ4_XS saves 600 MB if you're cramped, and IQ3_S exists for 16 GB laptops, but you will pay for it in coherence on harder prompts.

The simple rule: file size, plus 10-20% runtime overhead, plus KV cache for the context you actually use. A 16 GB machine starts at IQ3_S with short contexts. 24 to 32 GB is the comfortable tier, and Q8_0 is for people with 48 GB who distrust compression on principle. At 64k context the KV cache is a real cost, and compression like the TurboQuant setup I covered in the [Atomic Chat guide](https://blog.asadfaizee.is-a.dev/posts/atomic-chat-offline-ai-chatbot-local-llm/) is what keeps long sessions inside your RAM.

## How do you run Arete-27B locally?

Atomic Chat users have the shortest path: open the model browser, search "Arete", pick a quant, and it lands in your library ready to chat. From there it behaves like any other loaded model, including the OpenAI-compatible local API at `localhost:1337/v1` that agents and IDE tools can point at.

Prefer a different runtime? The GGUF files work anywhere llama.cpp does:

- **Ollama** and **LM Studio** import GGUF quants directly.
- **llama.cpp** itself, if you like your models with a CLI.
- **Jan**, the same base that Atomic Chat grew from.

For a 27B model, realistic hardware expectations: a 24 GB GPU or a 24-32 GB Apple Silicon Mac runs Q4_K_M comfortably; a 16 GB laptop will manage IQ3_S and patience; a phone will not (the smallest file alone is 13.6 GB, and that's before the model even starts thinking).

## Is Arete-27B good for daily work?

As a daily assistant, yes, with the right expectations. It inherits the Qwen3.8-27B base's general abilities, has a thinking mode, handles 64k context, and accepts images. Nous reports agentic capability held steady through training, which fits how conservative the recipe was: 2 epochs of SFT on 52.8M tokens is a coat of paint, not a rebuild.

Use it where a strong mid-size local model fits: daily chat, document work, coding help, summaries, running private at zero per-token cost. Use it with the awareness that the special sauce is the topic-framing change, and that a 27B model won't match frontier cloud models on the hardest reasoning.

## What are the honest caveats?

- **The eval is the lab's own.** Blind and fully published, but the judge, the rubric and the categories all come from Nous. Independent replication is the real test.
- **SFT changes surfaces.** Nous says so themselves: they addressed a root cause they measured, not every behavior the base model carries. Expect surprises outside the seven trained categories.
- **Mid-pack factual recall.** 2.98 out of 5 beats the base's 2.32, and loses to Mistral Large 4's 3.65 on the same eval.
- **No small quants exist.** This is a 27B model; 13.6 GB is the floor. Small-RAM and phone users should pick a smaller model, not a smaller quant here.
- **Values are a choice someone made.** If a lab's chosen framing doesn't match yours, the Apache-2.0 license and the published data mean you can build your own variant. That's the genuinely unusual part of this release: the disagreement is operational, not rhetorical.

## Verdict: should you download Arete-27B-GGUF?

Yes, if you run local models on 24 GB or more and you want a strong 27B with a documented, published post-training story. The model is free, the license is permissive, the quants are measured, and the entire value-tuning pipeline is inspectable end to end. That last part is rarer than it should be in 2026.

Skip it if your hardware tops out at 16 GB (pick a 9B-14B model instead), or if you needed frontier reasoning and were never a local-model person anyway.

My take: the interesting artifact here isn't just the weights, it's the precedent. A lab took a strong open model, reshaped its defaults in a direction it cared about, published every prompt and score, and let anyone with a 24 GB machine run the result. However you feel about the direction, the method is how open AI should work.

## FAQ

### What is Arete-27B-GGUF?

The GGUF-format release of Arete-27B, Nous Research's fine-tune of Qwen3.8-27B, converted by the AtomicChat org with per-file measurements. It runs in llama.cpp-based tools like Atomic Chat, Ollama, LM Studio and Jan.

### How much RAM does Arete-27B need?

Plan for the quant size plus overhead plus KV cache. The 16.8 GB Q4_K_M wants 24-32 GB of RAM to be comfortable; the 13.6 GB IQ3_S is the floor for 16 GB machines; Q8_0 at 28.6 GB wants 48 GB.

### Is Arete-27B open source?

The weights are Apache-2.0, and the training data (arete-data-pairs) and evaluation set (Propaganda_Eval) are published openly on Hugging Face.

### Can Arete-27B accept images?

Yes. The Qwen3.8-27B vision encoder survived fine-tuning untouched, and the GGUF release includes a matching ~0.9 GB vision projector file for image input.

---

**Sources:** [AtomicChat/Arete-27B-GGUF on Hugging Face](https://huggingface.co/AtomicChat/Arete-27B-GGUF) · [AtomicChat organization](https://huggingface.co/AtomicChat) · [Nous Research's Arete announcement](https://nousresearch.com/arete) · [NousResearch/Arete-27B model card](https://huggingface.co/NousResearch/Arete-27B) · [my Atomic Chat local LLM guide](https://blog.asadfaizee.is-a.dev/posts/atomic-chat-offline-ai-chatbot-local-llm/)
