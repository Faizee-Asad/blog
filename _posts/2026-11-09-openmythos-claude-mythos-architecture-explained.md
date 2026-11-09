---
layout: post

title: "OpenMythos Explained: Reconstructing Claude Mythos Architecture in PyTorch"
description: "Discover OpenMythos, an open-source theoretical reconstruction of Claude Mythos. Explore recurrent-depth transformers, latent reasoning, Mixture of Experts, installation, and its limitations."

date: 2026-11-09 17:48:00 +0530
author: Asad Faizee

categories:
  - Artificial Intelligence
  - Open Source

tags:
  - OpenMythos
  - Claude Mythos
  - Claude Mythos architecture
  - Anthropic
  - recurrent-depth transformer
  - looped transformer
  - latent reasoning
  - Mixture of Experts
  - PyTorch
  - open source AI
  - AI research
  - large language models
  - LLM architecture
  - transformer architecture
  - Parcae
  - Multi-Latent Attention
  - Grouped Query Attention
  - AI reasoning
  - machine learning
  - Python

image:
  path: /assets/img/posts/openmythos-claude-mythos-architecture.webp
  alt: "OpenMythos theoretical Claude Mythos architecture reconstruction using recurrent-depth transformers and PyTorch"

pin: false
comments: true
---

# OpenMythos explained: an open source guess at how Claude Mythos works

Anthropic hasn't published how Claude Mythos is built. So a developer named Kye Gomez wrote a guess in PyTorch and put it on GitHub. That repo, [OpenMythos](https://github.com/kyegomez/OpenMythos), has 14.9k stars and 3.3k forks.

The guess is specific: Mythos is a recurrent-depth transformer, which reuses the same layers many times per forward pass instead of stacking hundreds of unique ones. The repo ships the code, 7 model sizes from 1B to 1T parameters, and a long README full of citations.

It's a hypothesis with no trained weights. Here's what it claims, how to run it, and where it stops being evidence.

---

## What is OpenMythos?

OpenMythos is an open source, theoretical implementation of the architecture its author thinks sits behind Claude Mythos. It's MIT-licensed and installs from PyPI as `open-mythos`.

The README describes a Recurrent-Depth Transformer (RDT) with 3 stages: a Prelude of ordinary transformer blocks, a looped Recurrent Block that repeats up to `max_loop_iters` times, and a final Coda. Attention switches between Multi-Latent Attention (MLA) and Grouped Query Attention (GQA). The feed-forward layers use a sparse Mixture of Experts (MoE) with routed and shared experts.

[Decrypt's coverage](https://decrypt.co/366745/openmythos-claude-mythos-architecture-open-source-reconstruction) calls it structured speculation in code. [A DEV Community teardown](https://dev.to/clintjosy/openmythos-teardown-dissecting-the-open-source-reconstruction-of-claude-mythos-9e5) dates the first publication to April 21, 2026.

---

## Is OpenMythos the real Claude Mythos architecture?

Nobody outside Anthropic can say. The repo carries its own disclaimer: it's an independent, community-driven reconstruction based on public research and speculation, with no affiliation to Anthropic.

Here's the public status. Claude Mythos Preview isn't available to the general public. Anthropic describes a small number of trusted organizations using it as part of [Project Glasswing](https://www.anthropic.com/glasswing). No architecture paper exists.

Decrypt makes the same point about the README's wording. It says the file keeps saying "likely," "suspected," and "almost certainly," and that the real model may not be a looped transformer at all.

Read OpenMythos as a well-sourced bet. It collects published ideas into one codebase and says "this is what a Mythos-class model could look like."

---

## What is a recurrent-depth transformer?

A recurrent-depth transformer, also called a looped transformer, runs a small block of layers several times in a row. A standard transformer passes data once through a tall stack of different layers.

If forward passes and attention heads are new to you, this [plain-English guide to how LLMs work](https://blog.asadfaizee.is-a.dev/posts/how-do-llm-actually-work/) covers the basics first.

OpenMythos splits the model like this:

```
Input
  ↓
[Prelude]          standard transformer layers, run once
  ↓
[Recurrent Block]  looped T times, same weights each time
  ↓
[Coda]             standard transformer layers, run once
  ↓
Output
```

Each loop updates a hidden state with this rule from the README:

```
h_{t+1} = A·h_t + B·e + Transformer(h_t, e)
```

Here `h_t` is the hidden state after loop `t`, and `e` is the encoded input from the Prelude. `A` and `B` are learned injection parameters.

### Why re-inject the input every loop?

Re-injecting `e` keeps the original prompt alive across all the loops. The README says this stops the hidden state from drifting away from the input during deep recurrence.

The reasoning happens inside one forward pass, in continuous latent space, with no intermediate tokens printed. More loops means more compute spent on the same question.

---

## Why would looping help reasoning?

The README gives 4 reasons, each tied to published research. Treat them as arguments for looped models in general, since none of them proves anything about Mythos in particular.

1. **Systematic generalization.** The README says looped transformers combine known pieces in new ways, and that this ability arrives abruptly after a memorization stage and an in-distribution stage.
2. **Depth extrapolation.** Train on 5-hop reasoning chains, test on 10-hop, and a looped model can cope by running more loops at inference.
3. **Latent thoughts.** Each loop works like one step of chain-of-thought, but in latent space. The README cites [Saunshi et al.](https://arxiv.org/abs/2502.17416) for a formal result on this, and adds that latent states can carry several candidate next steps at once.
4. **Parameter efficiency.** A model with `k` layers run `L` times can match a `kL`-layer model with only `k` layers of parameters.

On the last point, the README cites Parcae: a 770M-parameter looped model reached the downstream quality of a 1.3B fixed-depth transformer on the same data. That's roughly half the parameters for the same quality.

---

## How does OpenMythos keep the loops stable?

It constrains the injection matrix so the loop can't blow up. Looped models are notoriously unstable to train, and the README names 2 failure modes: residual explosion and loss spikes.

The README models the loop as a linear time-invariant system and says stability depends on the spectral radius of `A`. If `ρ(A) < 1`, the loop converges. If `ρ(A) ≥ 1`, it diverges. The README claims every divergent run it saw had `ρ(A) ≥ 1`.

The fix comes from [Parcae (Prairie et al., 2026)](https://arxiv.org/abs/2604.12946). It parameterizes `A` as a negative diagonal matrix, discretizes it, and enforces `A := Diag(-exp(log_A))`. That guarantees `ρ(A) < 1` by construction, whatever the learning rate.

The usage snippet in the repo prints the spectral radius so you can check it yourself.

---

## What else is inside the model?

OpenMythos stacks 5 more ideas on top of the loop. All are borrowed from published work.

- **Mixture of Experts.** Fine-grained routed experts plus always-on shared experts. The README argues MoE supplies breadth while looping supplies depth.
- **MLA or GQA attention.** MLA caches a compressed key-value latent, and GQA uses fewer KV heads to shrink the cache. GQA can use Flash Attention 2 when installed.
- **Depth-wise LoRA.** A small adapter per loop lets each iteration behave a little differently while sharing most weights.
- **Adaptive halting.** Inspired by the [Universal Transformer](https://arxiv.org/pdf/1807.03819) and its Adaptive Computation Time, easy tokens stop looping early. The README warns about "overthinking," where too many loops degrade predictions.
- **Loop-index embedding.** The README floats the idea of a RoPE-like signal that tells the block which loop it's on. It labels this a hypothesis.

---

## What model sizes does OpenMythos define?

The repo ships 7 preset configs from 1B to 1T parameters. Here's the table from the README:

| Variant | `dim` | Experts | Loop iters | Context | Max output |
|---|---|---|---|---|---|
| `mythos_1b` | 2048 | 64 | 16 | 4k | 4k |
| `mythos_3b` | 3072 | 64 | 16 | 4k | 4k |
| `mythos_10b` | 4096 | 128 | 24 | 8k | 4k |
| `mythos_50b` | 6144 | 256 | 32 | 8k | 4k |
| `mythos_100b` | 8192 | 256 | 32 | 1M | 128k |
| `mythos_500b` | 12288 | 512 | 48 | 1M | 128k |
| `mythos_1t` | 16384 | 512 | 64 | 1M | 128k |

These are configs, and nothing in the repo is a trained checkpoint. Building a big one means training it yourself.

---

## How do you install and run OpenMythos?

Install from PyPI, then build a small model and run a forward pass:

```bash
pip install open-mythos
```

For Flash Attention 2 in the GQA path, which needs CUDA and build tools:

```bash
pip install open-mythos[flash]
```

Here's a tiny GQA example based on the README's usage block:

```python
import torch
from open_mythos.main import OpenMythos, MythosConfig

cfg = MythosConfig(
    vocab_size=1000, dim=256, n_heads=8, max_seq_len=128,
    max_loop_iters=4, prelude_layers=1, coda_layers=1,
    n_experts=8, n_shared_experts=1, n_experts_per_tok=2,
    expert_dim=64, lora_rank=8, attn_type="gqa", n_kv_heads=2,
)

model = OpenMythos(cfg)
ids = torch.randint(0, cfg.vocab_size, (2, 16))

logits = model(ids, n_loops=4)
out = model.generate(ids, max_new_tokens=8, n_loops=8)

A = model.recurrent.injection.get_A()
print(torch.linalg.eigvals(A).abs().max().item())  # must be < 1
```

Notice that `n_loops` is a runtime argument. You can run the same weights for 4 loops or 8, which is the whole point of the design.

A small catch in the README: the preset example imports `mythos_1b` through `mythos_1t`, then calls `mythos_7b()`, which isn't in the import list or the variant table. Use one of the listed presets, like `mythos_1b()`.

The training script for the 3B model on FineWeb-Edu lives at `training/3b_fine_web_edu.py`. It uses AdamW, PyTorch DDP through `torchrun`, bfloat16 on H100 and A100 cards, and a target of 30B tokens.

---

## What are OpenMythos's limits?

It's a research scaffold with no trained weights, and the evidence stops at the architecture. Check these before you build on it:

- **No weights.** Decrypt's summary is that the repo gives you a technique and no trained model to run.
- **The Mythos link is a guess.** The central claim can't be checked without Anthropic's design, which is unpublished.
- **Behavior claims are unverified.** The README says Mythos has uneven factual recall and strong novel reasoning. It offers no evaluation to back that up.
- **Training costs are real.** Decrypt estimates training a serious version at hundreds of thousands of dollars on H100s.
- **The repo is young.** It has 38 commits, 19 open issues, and 42 open pull requests.
- **Citations vary in strength.** Some references are papers, and others are X threads.

---

## Who should read or use OpenMythos?

ML researchers who want a compact codebase for looped transformers and MoE. Students who learn better from code than from papers. Writers and builders who want to understand what "latent reasoning" claims actually rest on.

If you want a beginner-friendly walkthrough first, start with [this simple guide to OpenMythos](https://blog.asadfaizee.is-a.dev/posts/Claude-Mythos) and then come back here for the details.

Skip it if you want a model to chat with. There's nothing to download.

---

## Frequently asked questions

### Is OpenMythos affiliated with Anthropic?

No. The README says it's independent and not endorsed by Anthropic or connected to its systems.

### Can I run Claude Mythos with OpenMythos?

No. OpenMythos contains code and configs only, with no trained weights. It can't reproduce Claude Mythos.

### What is a looped transformer?

A looped transformer reuses the same block of layers several times per forward pass. More loops means more compute without more parameters.

### What is Parcae?

Parcae is a 2026 paper on stable looped language models. OpenMythos borrows its constraint on the injection matrix to keep training stable.

### Is OpenMythos free?

Yes. It uses the MIT license.

---

## Key facts at a glance

- **Repo:** [github.com/kyegomez/OpenMythos](https://github.com/kyegomez/OpenMythos), MIT license
- **Stats:** 14.9k stars, 3.3k forks, 38 commits, 19 open issues, 42 open PRs
- **First published:** April 21, 2026, per a third-party teardown
- **Install:** `pip install open-mythos`
- **Architecture:** Prelude, looped Recurrent Block, and Coda with a sparse MoE feed-forward and MLA or GQA attention
- **Update rule:** `h_{t+1} = A·h_t + B·e + Transformer(h_t, e)`
- **Stability rule:** spectral radius of `A` must stay below 1
- **Parcae claim:** a 770M looped model matched a 1.3B fixed-depth transformer on quality
- **Presets:** 7 configs from 1B to 1T parameters
- **Training script:** 3B model on FineWeb-Edu, 30B-token target
- **Weights:** none released
- **Status of the claim:** a hypothesis, since Anthropic hasn't published Mythos's architecture
- **Beginner guide:** [What is OpenMythos? A simple guide](https://blog.asadfaizee.is-a.dev/posts/Claude-Mythos)
