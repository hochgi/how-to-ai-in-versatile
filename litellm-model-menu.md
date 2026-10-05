# LiteLLM stg — model menu for the `vn-*` aliases

Scraped from `litellm.internal.stg.versatile.ai` → Models + Endpoints, view
"All Available Models", 59 entries, LiteLLM v1.96.2.

Prices are **USD per 1M tokens, in / out**, as the gateway reports them. They do
not reflect promo credits, negotiated rates, or platform-side discounts — apply
those yourself when picking.

Pick a **model group name** from the left column of any table; that string is what
goes in the alias's "Target Model Group" field.

---

## Anthropic (credential: `anthropic-versatile-central`)

| Model group | Underlying | In | Out |
|---|---|---:|---:|
| `claude-haiku-4.5` | `anthropic/claude-haiku-4-5-20251001` | 1.00 | 5.00 |
| `claude-sonnet-5` | `anthropic/claude-sonnet-5` | 2.00 | 10.00 |
| `claude-sonnet-4` ⚠️ | `anthropic/claude-sonnet-5` | 2.00 | 10.00 |
| `claude-opus-4.8` | `anthropic/claude-opus-4-8` | 5.00 | 25.00 |
| `claude-opus-5` | `anthropic/claude-opus-5` | 5.00 | 25.00 |
| `claude-fable-5` | `anthropic/claude-fable-5` | 10.00 | 50.00 |

⚠️ `claude-sonnet-4` points at **sonnet-5**. Anyone selecting it gets a different
model than the name says. Worth fixing or deleting independently of the aliases.

---

## Valar-hosted open models (credential: `valar-versatile-central`)

Every family is published in tiers. Cheapest tier per family is bolded.
Blank = that tier does not exist for that family.

| Family | batch | flex | standard | priority | bare |
|---|---:|---:|---:|---:|---:|
| `gpt-oss-120b` | | **0.02 / 0.15** | 0.04 / 0.30 | | 0.06 / 0.40 |
| `gemma-4-26B-A4B-it` | | **0.04 / 0.11** | 0.05 / 0.16 | | 0.09 / 0.27 |
| `DeepSeek-V4-Flash` | **0.03 / 0.06** | 0.04 / 0.07 | 0.05 / 0.10 | 0.07 / 0.14 | 0.09 / 0.19 |
| `gemma-4-31B-it` | | **0.14 / 0.24** | 0.18 / 0.30 | | 0.36 / 0.60 |
| `Qwen3.5-397B-A17B` | | **0.17 / 0.53** | 0.25 / 0.75 | | 0.45 / 1.35 |
| `Qwen3.6-35B-A3B` | | **0.09 / 0.37** | 0.13 / 0.52 | | 0.23 / 0.90 |
| `Qwen3.5-27B` | **0.08 / 0.66** | 0.09 / 0.77 | 0.14 / 1.10 | | 0.27 / 2.20 |
| `DeepSeek-V4-Pro` | **0.34 / 0.68** | 0.40 / 0.79 | 0.57 / 1.13 | 0.85 / 1.70 | 1.13 / 2.26 |
| `NVIDIA-Nemotron-3-Ultra-550B` | | **0.17 / 0.77** | 0.25 / 1.10 | | 0.50 / 2.20 |
| `MiniMax-M3` | | **0.07 / 0.80** | 0.10 / 1.00 | | 0.20 / 1.20 |
| `MiniMax-M2.7` | | **0.10 / 0.42** | 0.15 / 0.60 | | 0.27 / 1.08 |
| `GLM-5.2` | | **0.34 / 1.08** | 0.49 / 1.54 | | 0.98 / 3.40 |
| `Kimi-K2.7` | | **0.26 / 1.23** | 0.38 / 1.75 | | 0.75 / 3.50 |
| `GLM-5.2-fast` | | **0.59 / 1.85** | 0.84 / 2.64 | | 1.68 / 5.28 |
| `Kimi-K3` | **0.90 / 4.50** | 1.05 / 5.25 | 1.50 / 7.50 | | 3.00 / 15.00 |

Model group names follow `valar-<family>[-tier]`, lowercased, e.g.
`valar-deepseek-v4-pro-flex`, `valar-glm-5.2`, `valar-gpt-oss-120b-standard`.
The bare name (no tier suffix) is always the most expensive tier.

---

## Wildcard pass-throughs

| Model group | Credential |
|---|---|
| `bedrock/*` | Bedrock |
| `bedrock_mantle/*` | Bedrock mantle |

No price data attached, so spend on these will not be attributed by the gateway's
cost columns.

---

## The five aliases to define

Models + Endpoints → **Model Group Alias** tab. Two fields, one button. Currently
empty.

| Alias | Role | What the phase actually does |
|---|---|---|
| `vn-spec` | phase 1 · spec-author | Reads the codebase, writes Gherkin/EARS. Reasoning-heavy, runs once per slice. |
| `vn-test` | phase 2 · test-author | Translates SPEC → failing tests. Scope discipline matters more than raw power — over-broad tests generate slop downstream. |
| `vn-coding` | phase 3 · coder | The grind loop to green. Tests are the oracle, so there is no judgment call here by design. |
| `vn-review` | phase 4 · reviewer | Correctness + regression review, read-only. Judgment-heavy. |
| `vn-verify` | phase 5 · verifier | Runs CRAP + mutation, audits every `// Stryker disable` by hand. Read-only. |

Re-pointing any of these is one edit here and **zero repo changes** — that is the
whole reason the repos say `litellm/vn-coding` instead of a model name. The UI
renders the equivalent `router_settings.model_group_alias` YAML underneath the
table, which is what you would paste into
`vnatures/devops-k8s` → `helm-charts/LiteLLM/stg/values.yaml` if you later want
the mapping reviewable in git rather than click-ops.
