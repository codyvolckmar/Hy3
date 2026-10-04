# Open-Weights Agentic Models — Benchmarks & Inference Cost

**A price/performance comparison of open-weights models for agentic and coding workloads, focused on seriously cheap inference.**

> **Last updated: 2026-10-04.** Data was pulled from the HuggingFace API, the OpenRouter model API, Tencent Cloud's published price list, and five independent SWE-bench leaderboards on this date. Prices move constantly — re-verify before you commit.

---

## TL;DR — what to pick

| If you want… | Use | Why |
|---|---|---|
| Best quality-per-cent at frontier capability | **GLM-5.3 Flash** | 92.0% SWE-bench Verified at **$0.26** blended/1M. **MIT** license, 1M context, multimodal. ~45× cheaper per SWE-bench point than Claude Opus 4.8. |
| Best absolute open-weights quality | **DeepSeek V4 Pro 0813** | **96.4%** SWE-bench Verified — #2 globally, ahead of Claude Opus 5 (97.0%)'s cheaper siblings and Opus 4.8 (88.6%). MIT. Costs ~8× more than GLM-5.3 Flash. |
| Tencent's current flagship | **Hy4 preview** | 770B/49B-active, **1M context**, Apache-2.0, **$1.20** blended. Best-in-class on SWE-bench Multilingual (82.9). Text-only, 64K max output. |
| Cheapest model that's still respectable | **DeepSeek V4 Flash** | 88.8% SWE-bench Verified at **$0.40** blended. 284B/13B-active, 1M context. |
| Cheapest credible agentic tier | **Qwen3-Coder-30B-A3B** | $0.07 in / $0.28 out, Apache-2.0, 30.5B total. Good router/sub-agent, not a primary solver. |
| Cheapest Tencent model, period | **Hy3** | $0.0825 in / $0.33 out → **$0.16** blended. 295B/21B-active. Superb value when 256K context is enough. |

The headline finding versus our May 2026 analysis: **the "open weights are 75–100× cheaper but much worse" trade-off no longer exists.** DeepSeek V4 Pro 0813 beats Claude Opus 4.8 on SWE-bench Verified (96.4% vs 88.6%) at roughly a fifth of the blended cost. Open weights are now competitive on quality, and cheap inference is a much less interesting reason to choose them than it was in April.

---

## Correction log — what was wrong in the previous revision

This README previously analysed **Hy3 preview** in May 2026. Four and a half months later, several of its claims are wrong or stale. Recording them rather than quietly deleting them:

| Old claim | Reality | Impact |
|---|---|---|
| "License: Hy Community License (not OSI-open, but freely accessible)" | **Apache-2.0.** Both `tencent/Hy3` and `tencent/Hy4-preview` carry `license: apache-2.0` on HuggingFace. | **Material.** We understated how permissive this is. This is one of the most permissively-licensed models at this capability level. |
| Hy3 preview on OpenRouter is **$0.066 in / $0.26 out**, blended **$0.124** | It was **$0.18 / $0.60**, blended **$0.306** | **Material.** The central cost claim understated real cost by ~2.5×. |
| "74.4% SWE-bench Verified, 87.2 GPQA, 65.8 MMLU-Pro, 34.9 LiveCodeBench" for Hy3 preview | **Could not be corroborated.** Tencent publishes its benchmark tables as images only (`assets/benchmark.png`); no machine-readable numbers are published. | **Unverified.** Numbers below are marked with provenance. |
| "LMSYS Arena Elo: 1417 (86.2 percentile)", "#104 of 333 coding models" | **No corroboration found.** | **Removed** as unverifiable pseudo-precision. |
| "Gemini 3.1 Pro ~85%*" (footnote: *"estimated from general capability tier"*) | An estimate presented in the same table as measured figures. | **Removed.** Estimates do not belong in a benchmark table. |
| "GPT-5.5 / Opus 4.7 are the frontier" | Now **Opus 5.5 / GPT-6.1 Sol / GPT-6 Luna**. | Stale reference frame. |
| "Claude Opus 4.7 is 89× the cost of Hy3" | Opus 5.5 blended is **$8.80**; Hy4 preview is **$1.20** → **~7.3×**, not 89×. Versus GLM-5.3 Flash it's ~34×. | **Overstated by an order of magnitude.** |
| "295B total / 21B active", "256K context", "April 2026 preview" | ✅ All correct for **Hy3**. | Kept. |
| "Yao Shunyu (former OpenAI researcher)" | ✅ Correct but understated — he is now **Chief AI Scientist at Tencent**, ex-Research Scientist at OpenAI, PhD Princeton (Narasimhan), and authored ReAct, Tree-of-Thoughts and SWE-bench. | Corrected for completeness. |

**Naming trap:** `Hy3 preview` (Apr 2026) → `Hy3` (Jul 2026) → `Hy4 preview` (Aug 2026) are three distinct releases. Hy3 is the successor to Hy3 preview, not the same thing.

---

## The current open-weights landscape

| Model | Total / Active params | Context | License | Released | Max output |
|---|---|---|---|---|---|
| **Hy4 preview** | 770B / 49B (+10B MTP) | 1M | Apache-2.0 | 2026-08-28 | 64K |
| Hy3 | 295B / 21B (+3.8B MTP) | 256K | Apache-2.0 | 2026-07-06 | — |
| GLM-5.3 Flash | MoE, 288 experts top-8 | 1M | **MIT** | 2026-08-26 | 128K |
| GLM-5.3 | MoE (proprietary license) | 1M | Other | 2026-08-18 | — |
| DeepSeek V4 Pro 0813 | 1.6T / 49B, 384 experts top-6 | 1M | **MIT** | 2026-08-13 | 384K |
| DeepSeek V4 Flash | 284B / 13B | 1M | MIT | 2026-04-24 | — |
| DeepSeek V4.1 Flash | CED arch., 8B in / 16B out | 1M | MIT | 2026-09-10 | — |
| Kimi K3 | 2.8T, 896 experts | 1M | Kimi license | 2026-07-16 | 1M |
| Kimi K2.7 Code | MoE, multimodal | 256K | Kimi license | 2026-06-12 | — |
| Qwen3-Coder-30B-A3B | 30.5B / ~3B, 128 experts top-8 | 256K | Apache-2.0 | 2025-07 | — |
| Qwen3.6-35B-A3B | 35B / 3B | 256K | Apache-2.0 | 2026-04-27 | — |

**Hy4 preview architecture, in detail** (from `config.json` and the official model card):

- 78 layers — layer 1 dense FFN, layers 2–78 MoE; 256 routed experts + 1 shared, top-8 activated
- Attention: **Gated DeepSeek Sparse Attention** with IndexCache for cross-layer sparse index reuse; 64 heads, head_dim 64, query compression 2048, KV compression 512
- Residuals: **iHC (identity Hyper-Connections)**, 4 residual streams
- 1 native MTP layer (10B total, 0.7B active) for speculative decoding
- Text-only. **No vision input** — GLM-5.3 Flash, Kimi K3 and DeepSeek V4.1 Flash are the multimodal options.

---

## Benchmarks

### SWE-bench Verified (third-party leaderboards, ~2026-10-02/03)

Five independent leaderboards (vals.ai, Benchmark Atlas, Ridge, BenchLeader, AnotherWrapper) agree on this ordering. **No Hy model has a published SWE-bench Verified entry** — that is the single biggest gap in evaluating Tencent's models on the standard agentic metric.

| # | Model | SWE-bench Verified | Type | OpenRouter blended $/1M |
|---|---|---|---|---|
| 1 | Claude Opus 5 | 97.0% | closed | $12.00 |
| 2 | **DeepSeek V4 Pro 0813** | **96.4%** | **open (MIT)** | $2.09 |
| 3 | GPT-5.6 Sol | 96.2% | closed | $4.30 |
| 4 | Grok 4.6 | 95.6% | closed | — |
| 5 | **GLM-5.3** | **95.4%** | open | $2.30 |
| 7 | Claude Fable 5 | 95.0% | closed | — |
| 8 | **Kimi K3** | **93.4%** | open | $4.40 |
| 10 | **GLM-5.3 Flash** | **92.0%** | **open (MIT)** | **$0.26** |
| 12 | **DeepSeek V4 Flash** | **88.8%** | open | $0.40 |
| 14 | Claude Opus 4.8 | 88.6% | closed | $11.00 |
| — | Qwen3.8-27B | 86.0% | open | — |

⚠️ **Caveat that matters:** these are vendor-reported scores aggregated by third parties, with inconsistent scaffolding (agent harness, turn limits, retry budget differ per submission). The gap between 96.4% and 88.6% is large enough to trust; the gap between 95.4% and 96.4% is not. Treat anything within ~2 points as a tie.

### Agentic & research benchmarks (llm-stats aggregation)

| Benchmark | Hy4 preview | GLM-5.3 | Kimi K3 |
|---|---|---|---|
| GPQA | **92.3** | — | — |
| Terminal-Bench 2.1 | 85.4% | **88.2%** | — |
| CyberGym | — | **84.5%** | — |
| FrontierSWE | — | **78.1%** | — |
| Toolathlon | — | **73.0%** | — |
| DeepSWE 1.1 | — | **66.9%** | — |
| WideSearch | **83.9%** | — | — |
| MCP Atlas | **83.7%** | — | — |
| SWE-bench Multilingual | **82.9** | 81.3 | 80.8 |
| SWE-bench Pro | **65.7** | 64.6 | 63.3 |
| LLM Stats composite | 50.4 | **52.0** | 52.4 |

Hy4 preview wins the benchmarks where it has an entry, but on composite indexes **GLM-5.3 and Kimi K3 are ahead** — GLM 5.3 leads on 8 of 10 shared benchmarks, Kimi K3 on 8 of 12. Hy4's genuine edge is **multilingual** SWE-bench and MCP/tooling breadth.

### Vendor-reported internal evals (unverified — Tencent's own tasks, not public benchmarks)

- **Hy4 preview**: 163 Tencent engineers, 203 real internal engineering tasks. Hy4 **2.99**/4 vs GLM-5.3 2.92 and Kimi K3 2.94. Win rates 46.8%/12.8%/40.4% vs GLM 5.3; 51.2%/7.9%/40.9% vs Kimi K3. Tencent also claims lowest inference cost **82% below** GLM 5.3 and Kimi K3. *The margins are 0.05–0.07 on a 4-point scale — well inside noise for 163 raters. Treat as "not worse," not "better."*
- **Hy3**: 270 experts on internal tasks — Hy3 2.67/4 vs GLM-5.1 2.51. Hallucination rate 12.5% → 5.4%; commonsense error 25.4% → 12.7%; multi-turn issue rate 17.4% → 7.9%. SWE-bench Verified variance across CodeBuddy/Cline/KiloCode scaffolds within 4%.

**Known Hy4 limitations** (self-reported by Tencent): reasons for longer than necessary on complex tasks, and a tendency to over-verify its own output. Both directly cost you money if you're optimizing for cheap inference.

---

## Pricing

### OpenRouter (USD per 1M tokens, 2026-10-04)

Blended = 70% input / 30% output, which suits chat and tool-calling agent loops.

| Model | Input | Output | Blended 70/30 | SWE-bench Verified | Cost per Verified point |
|---|---|---|---|---|---|
| **GLM-5.3 Flash** | $0.15 | $0.50 | **$0.26** | 92.0% | **0.277¢** |
| Qwen3-Coder-30B-A3B | $0.07 | $0.28 | **$0.13** | — | — |
| **Hy3** | $0.0825 | $0.33 | **$0.16** | — | — |
| DeepSeek V4 Flash | $0.0224 | $1.28 | $0.40 | 88.8% | 0.450¢ |
| **Hy4 preview** | $0.7506 | $2.2509 | **$1.20** | — | — |
| DeepSeek V4 Pro 0813 | $0.85 | $5.00 | $2.09 | 96.4% | 2.173¢ |
| GLM-5.3 | $1.40 | $4.40 | $2.30 | 95.4% | 2.411¢ |
| Kimi K3 | $0.72 | $13.00 | $4.40 | 93.4% | 4.715¢ |
| Claude Opus 4.8 | $5.00 | $25.00 | $11.00 | 88.6% | 12.415¢ |
| Claude Opus 5.5 | $4.00 | $20.00 | $8.80 | — | — |
| GPT-5.5 | $5.00 | $30.00 | $12.50 | — | — |

Note DeepSeek's pricing is unusually **output-heavy**: V4 Pro 0813 costs more per output token than per input token, while GLM-5.3 Flash is the reverse. If your workload is long-context-in/short-output, DeepSeek is the better deal; if it's generation-heavy, GLM Flash wins.

### Tencent Cloud (RMB per 1M tokens, Guangzhou region, listed price)

| Model | Input | Output | Cache hit |
|---|---|---|---|
| Hy4 preview | 6 | 18 | 0.3 |
| GLM-5.3 Flash | 0.8 | 2.8 | 0.23 |
| Kimi K3 | 20 | 100 | 2 |
| DeepSeek V4 Pro 0813 (off-peak) | 4.5 | 13.5 | 0.15 |
| DeepSeek V4 Pro 0813 (peak) | 9 | 27 | 0.3 |

At ~7.1 RMB/USD these cross-check against OpenRouter (Hy4 preview ≈ $0.85/$2.54 vs $0.75/$2.25; GLM-5.3 Flash ≈ $0.11/$0.39 vs $0.15/$0.50).

**Promotions that have expired:** Hy4 preview had a Token Plan credit discount (input/output/cache multipliers cut from 120/360/6 to 100/200/4) running 2026-09-01 to 2026-09-30. GLM-5.3 Flash's 50% discount ran only until 2026-09-10. Both are gone as of this update.

---

## The three levers that actually cut your bill

1. **Prompt caching is the biggest lever, bigger than model choice.** Hy4 preview's cache-hit price is 0.3 RMB vs 6 RMB input — a **20× discount**. Put stable system prompts and tool definitions in a cacheable prefix. Assuming an 80% cache hit rate on input, a 1M-in/200k-out task costs **5.04 RMB** on Hy4 preview instead of 9.6 RMB, and **0.75 RMB** on GLM-5.3 Flash instead of 1.36 RMB.
2. **Output price dominates, not input.** Output is typically 1–6× the input rate, and agent loops emit a lot of tokens. Compressing output length buys more than compressing input length. This is also why "disable reasoning on easy turns" beats "pick a cheaper model."
3. **Route, don't replace.** Run GLM-5.3 Flash (or Qwen3-Coder-30B at $0.13 blended) for planning, extraction, and summarisation; escalate only hard steps to DeepSeek V4 Pro 0813 or GLM-5.3. A 90/10 routing split lands your effective cost near $0.40 while keeping frontier-tier results on the steps that matter.

Note the peak/off-peak split on DeepSeek V4 Pro 0813: peak is Mon–Sun 09:00–12:00 and 14:00–18:00 server-side. Batch jobs scheduled overnight get the 4.5/13.5 rate; online services realistically cannot avoid peak.

---

## When *not* to use open weights

- **You need vision.** Hy4 preview, GLM-5.3 and DeepSeek V4 Pro are text-only. GLM-5.3 Flash, Kimi K3 and DeepSeek V4.1 Flash accept image/video.
- **You need very long outputs.** Hy4 preview caps at **64K max output** — the most conservative in its class (GLM-5.3 Flash 128K, DeepSeek V4 Pro 384K, Kimi K3 1M). Design your streaming and timeout strategy around that up front.
- **You need a permissive license and GLM-5.3 is your target.** `zai-org/GLM-5.3` is licensed "other," not MIT. `GLM-5.3-Flash` *is* MIT.
- **Absolute top-end quality on unreviewable code.** Opus 5 at 97.0% and DeepSeek V4 Pro at 96.4% are within noise of each other, and both are behind neither each other nor Opus 5 by much — but nobody open-weights has closed on Opus 5.
- **Your traffic is bursty on Tencent Cloud.** Tencent currently flags Hy4 preview as high resource load with peak-hour rate limiting on TokenHub.

---

## Serving it yourself

Tencent ships official weights and recipes:

| | Hy4 preview | Hy3 |
|---|---|---|
| Weights | [HF `tencent/Hy4-preview`](https://huggingface.co/tencent/Hy4-preview) · [FP8](https://huggingface.co/tencent/Hy4-preview-FP8) | [HF `tencent/Hy3`](https://huggingface.co/tencent/Hy3) · [FP8](https://huggingface.co/tencent/Hy3-FP8) |
| Mirrors | ModelScope, GitCode, CNB | ModelScope, GitCode, CNB |
| Community quants | `AngelSlim/Hy4-preview-GGUF` (289K dl), `Oxmiq/Hy4-preview-NVFP4-W4A16` | `AngelSlim/Hy3-GPTQ-Int4`, `satgeze/Hy3-1M-GGUF` |
| vLLM | `vllm/vllm-openai:hy4-preview`, TP=8, `FLASHMLA_SPARSE`, MTP speculative (3 tokens) | vLLM + MTP (2 tokens), **H20-3e or larger-memory GPUs for TP=8** |
| SGLang | `lmsysorg/sglang:hy4-preview` | recipe available |

Recommended sampling for both: `temperature=0.9`, `top_p=1.0`. Reasoning control differs — **Hy4 defaults to `reasoning_effort: "high"`**, Hy3 defaults to `"no_think"`. Pass `{"chat_template_kwargs": {"reasoning_effort": "no_think"}}` on Hy4 for direct responses.

**Fine-tuning:** GRPO RL training via `verl` on Megatron-LM with vLLM rollout is supported for both. Quantization via Tencent's [AngelSlim](https://github.com/tencent/AngelSlim).

---

## How to reproduce this data

```bash
# Pricing (authoritative, live)
curl -s https://openrouter.ai/api/v1/models | jq '.data[] | select(.id|test("hy4|hy3|glm-5|deepseek-v4|kimi-k3"))'

# License + downloads (authoritative, live)
curl -s "https://huggingface.co/api/models/tencent/Hy4-preview" | jq '{license: .cardData.license, downloads}'

# Architecture (authoritative, live)
curl -s https://huggingface.co/tencent/Hy4-preview/raw/main/config.json
```

**Provenance rules used in this document:** anything from a Tencent model card or announcement is labelled *vendor-reported*. Anything from a leaderboard aggregator is labelled with the aggregator. Benchmarks marked *unverified* were not corroborated by a second source and should not be quoted. Tencent publishes its benchmark tables as **images only** — there is no machine-readable benchmark data for either Hy model, which is why Hy3/Hy4 have so few comparable public numbers.

---

## Sources

- [tencent/Hy4-preview model card](https://huggingface.co/tencent/Hy4-preview) · [config.json](https://huggingface.co/tencent/Hy4-preview/blob/main/config.json) · [GitHub](https://github.com/Tencent-Hunyuan/Hy4-preview)
- [tencent/Hy3 model card](https://huggingface.co/tencent/Hy3) · [GitHub](https://github.com/Tencent-Hunyuan/Hy3)
- [Tencent Cloud: Hy4 preview vs GLM-5.3-Flash / Kimi K3 / DeepSeek-V4-Pro](https://developer.cloud.tencent.com/article/2736612) — official RMB price list, context limits, engineering comparison
- OpenRouter model API — pricing and context windows, retrieved 2026-10-04
- HuggingFace API — licenses, parameter counts, release dates, retrieved 2026-10-04
- [SWE-bench Verified — vals.ai](https://www.vals.ai/benchmarks/swebench) · [Benchmark Atlas](https://atlas.kevinhu.io/benchmarks/swe-bench-verified) · [Ridge](https://ridgebench.com/benchmarks/swe-bench) · [BenchLeader](https://www.benchleader.com/benchmarks/vals_swebench) · [AnotherWrapper](https://anotherwrapper.com/tools/llm-pricing/evals/swe-bench-verified)
- [llm-stats: Hy4 preview vs Kimi K3](https://llm-stats.com/models/compare/hy4-preview-vs-kimi-k3) · [GLM-5.3 vs Hy4 preview](https://llm-stats.com/models/compare/glm-5.3-vs-hy4-preview)
- [Shunyu Yao — Wikipedia](https://en.wikipedia.org/wiki/Shunyu_Yao) · [personal site](https://ysymyth.github.io/)
- License statuses: `zai-org/GLM-5.3-Flash` (MIT), `zai-org/GLM-5.3` (other), `deepseek-ai/DeepSeek-V4-Pro-0813` (MIT), `Qwen/Qwen3-Coder-30B-A3B-Instruct` (Apache-2.0), `tencent/Hy3` and `tencent/Hy4-preview` (Apache-2.0)

---

*Analysis updated: 2026-10-04 · Supersedes the 2026-05-20 Hy3 preview revision*
