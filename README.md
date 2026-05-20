# Hy3
Comparrison of current top AI Models

root@Agent-Cody:~/hy3# more HY3_ANALYSIS.md
# Hy3 Preview Analysis: Why It's Good and Cheap

## Overview

Hy3 Preview is Tencent's 295B parameter Mixture-of-Experts (MoE) model with 21B active parameters, released April 22-24, 2026. It achieves frontier-class performance at 75-100x lower cost t
han competing models.

---

## Why Hy3 is "So Good" (Capabilities)

### 1. Architecture Advantage — MoE Efficiency
- **295B total parameters, only 21B active** per token (Mixture-of-Experts)
- Delivers reasoning power of a ~300B model at inference cost of a ~21B model
- **256K context window** with solid long-context performance
- Architecture allows scaling total parameters without proportional cost increase

### 2. Rebuilt Infrastructure
- Tencent rebuilt pretraining + RL stack from scratch
- Led by **Yao Shunyu (former OpenAI researcher)**
- Strong focus on agentic workflows and coding tasks

### 3. Benchmark Performance

| Benchmark | Hy3 Preview | GPT-5.5 | Opus 4.7 | DeepSeek V4-Pro |
|-----------|-------------|----------|-----------|-----------------|
| SWE-Bench Verified | **74.4%** | 88.7% | 87.6% | 83.7% |
| Terminal-Bench 2.0 | **54.4%** | n/p | n/p | n/p |
| GPQA Diamond | **87.2** | 93.6 | 94.2 | 89.1 |
| MMLU-Pro | **65.8** | 83.2 | ~82 | 78.9 |
| LiveCodeBench v6 | **34.9** | ~78 | ~75 | ~68 |

### 4. Agent-Ready Features
- Configurable reasoning modes (high/low/disabled)
- Native function/tool calling
- Strong on search agent benchmarks (BrowseComp, WideSearch)
- **LMSYS Arena Elo: 1417** (86.2 percentile)
- Ranking: #104 of 333 coding models on LM Market Cap

### 5. Accessibility
- Weights available on HuggingFace, GitHub, ModelScope, GitCode
- License: Hy Community License (not OSI-open, but freely accessible for research/use)

---

## Why Hy3 is "So Cheap"

### 1. MoE Inference Economics
- Cost scales with **active parameters (21B)**, not total (295B)
- ~14x cheaper to serve than a dense 295B model
- Only 7% of parameters active per token

### 2. Two-Tier Pricing Structure

| Provider | Input (per 1M) | Output (per 1M) | Blended (70/30) |
|----------|-----------------|------------------|------------------|
| **OpenRouter** | $0.066 | $0.26 | **~$0.124** |
| **Tencent Cloud** | ~$0.17 (RMB 1.2) | ~$0.55 (RMB 4) | **~$0.284** |

- OpenRouter price is ~3x cheaper because Chinese inference providers run on **subsidized GPUs** and eat margin to gain volume
- Tencent direct price reflects actual infrastructure costs
- Free tier exists but rate-limits hard (sampling only, not production)

### 3. Strategic Positioning
- Marketed as a **cost-efficient workhorse**, not premium flagship
- Thin margins to capture market share in coding/agent space
- Tencent's scale allows operating at lower margins than specialized AI labs

---

## Cost Comparison to ALL Top Models

Blended cost per million tokens (70% input / 30% output ratio):

| Model | Blended $/1M | SWE-Bench | Cost per SWE-Bench point (¢) | Multiple vs Hy3 |
|-------|---------------|-----------|-------------------------------|-----------------|
| **Hy3 Preview (OpenRouter)** | **$0.124** | 74.4% | **0.17¢** | **1x (baseline)** |
| Hy3 Preview (Tencent Direct) | $0.284 | 74.4% | 0.38¢ | 2.3x |
| Kimi K2.6 | $1.87 | 76.8% | 2.4¢ | 15x |
| DeepSeek V4-Pro | $2.26 | 83.7% | 2.7¢ | 18x |
| Gemini 3.1 Pro (<200K) | ~$5.00 | ~85%* | ~5.9¢ | 40x |
| Claude Opus 4.7 | $11.00 | 87.6% | 12.6¢ | **89x** |
| GPT-5.5 | $12.50 | 88.7% | 14.1¢ | **101x** |

*Gemini 3.1 Pro SWE-Bench score estimated from general capability tier.

---

## Key Takeaways

### The Math
Hy3 OpenRouter gives you **~75x more coding quality per dollar** than Opus 4.7:
- Absolute quality gap: ~14 points on SWE-Bench
- Cost gap: **75-100x**
- Cost per SWE-Bench point: 0.17¢ (Hy3) vs 12.6¢ (Opus 4.7)

### When Hy3 Makes Sense
1. **Workflows with cheap verification**: tests, lint, compile checks catch the 26% of failures
2. **High-volume applications**: where 75x cost savings outweigh 14-point quality gap
3. **Multi-attempt strategies**: run 3-4 attempts for same cost as 1 Opus attempt
4. **Agentic loops**: where per-step cost matters more than per-step accuracy

### When Hy3 Does NOT Make Sense
1. **Single-shot critical tasks**: where 74% success rate is unacceptable
2. **Multi-step agent workflows**: 74% per step collapses to ~30% on 5-step plans vs ~53% for 88% models
3. **Premium applications**: where absolute quality matters more than cost

---

## Sources
- [GitHub Repository](https://github.com/Tencent-Hunyuan/Hy3-preview)
- [HuggingFace Model](https://huggingface.co/tencent/Hy3-preview)
- [TokenCost Pricing Analysis](https://tokencost.app/blog/hunyuan-hy3-preview-pricing)
- [LM Market Cap](https://lmmarketcap.com/model/hy3-preview-free)
- [OpenRouter Rankings](https://openrouter.ai/rankings)
- Tencent Announcement (May 2026)

---

*Analysis generated: 2026-05-20*
*Model: Hy3 Preview (295B A21B MoE)*
root@Agent-Cody:~/hy3#
