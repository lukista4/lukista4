# Lukáš Staník

I'm a graduate mathematics student at Charles University in Prague. Alongside my studies I build training,
evaluation and inference pipelines for LLMs, focused on mathematics: can a small open model (Qwen3.5-9B) that runs
locally learn to judge olympiad-level mathematical proofs as well as human experts do, and then help write them?

The work runs on a university Slurm cluster, a PBS cluster, Colab, and an 8 GB AMD card at home. I care about
measuring things properly: pre-registered gates, blind tests, paired bootstrap intervals, and negative results reported
with the same detail as positive ones.

## Repositories

| repo | what it shows |
|---|---|
| [judge-lab-public](https://github.com/lukista4/judge-lab-public) | A fast in-process vLLM judge reader, a reader-equivalence test kit (logits, score distributions, bf16 rounding mechanics), grader calibration against expert marks (ProofBench), judge-reliability controls (planted flaws, swapped-order known answers), input-quality checks and evaluation statistics. 152 CPU tests. |
| [consumer-gpu-llm](https://github.com/lukista4/consumer-gpu-llm) | Running and LoRA-fine-tuning Qwen3.5-9B on an 8 GB RX 7600 (llama.cpp Vulkan): a patch set that makes the Gated DeltaNet projections trainable, a trainer off-by-one fix, memory ceilings, long-context and multi-slot throughput, quantization KL table, GGUF tools. |
| [llm-inference-levers](https://github.com/lukista4/llm-inference-levers) | A gated study of throughput levers on L40S / A100: generation-tail exit (0.337x generation time), MTP speculative decoding (1.80x at low concurrency, 0.81x at 256 active, and it breaks cold/warm reproducibility), fp8 KV cache (rejected: lossy), runtime multi-LoRA (adopted), training levers, LLM-as-judge marking cost. |

## Selected results

All numbers are labelled in the repos as MEASURED, DERIVED or PROJECTED, with their hardware and batch regime.

- **Reader equivalence.** A 20-70x faster vLLM judge reader was not allowed to replace the reference reader until it
  passed logit-level, distribution-level and bf16-mechanics gates. It failed the bf16 test by a hair (pre-cast logit
  drift 0.12818 vs one bf16 step 0.125), so the two readers are never mixed in one comparison.
- **Grader calibration.** A frontier-model grader agrees with human experts overall (QWK 0.66-0.70) but is close to
  random and harsh on strong proofs (QWK 0.02-0.05, bias -1.1 to -1.4 inside the 6-7 band).
- **Consumer GPU.** Three concurrent 30k-token prefills on 8 GB collapse to 9.8 tok/s aggregate decode vs 41.8 for one.
- **Inference.** 48% of GPU time in one experiment family ran with at most 4 sequences still generating; a tail-exit
  rule cut generation time to 0.337x.

## How I work

Goals, constraints and decisions are mine. Most code, runs, analysis and documentation are produced by AI agents that I
direct, and results are checked by tests and by reviews from a different model family than the author's. Model grades
are labelled as model judgements, never as human ground truth. I make no claims about open mathematical problems.

## AI usage disclosure

- **My role:** I set the goals, priorities, constraints and budgets, chose what to pursue or drop, ran it on my own
  hardware and accounts, and made the key decisions.
- **The agents' role:** most of the code, experiment runs, analysis and documentation were produced by AI agents:
  - Anthropic Claude (Claude Code) wrote most code and docs; commits carry a `Co-Authored-By: Claude` trailer;
  - OpenAI GPT models via Codex did code and statistics reviews;
  - Google Gemini did design and code reviews.
- **Models as instruments:** several model families (including Mistral-hosted models) were also used as graders. Those
  grades are model judgements, not human ground truth, and are labelled as such.
- **Checks:** results were checked by automated tests and by reviews from a different model family than the author's.
  An AI review is not an expert human review; where only one agent checked something, it says so.
- **Measurements:** all numbers come from real runs on real hardware. Each is labelled MEASURED, DERIVED or PROJECTED.
