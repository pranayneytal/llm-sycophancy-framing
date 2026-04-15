# Does User Framing Style Influence Sycophantic Behaviour in Large Language Models?

**Pranay Neytal · Aditya Thakur**  
Independent Research · 2026

---

## Overview

Large language models (LLMs) are known to exhibit sycophancy — agreeing with users rather than correcting them. But does it matter *how* the user says something wrong? This study tests whether the rhetorical framing of a false claim (confident, aggressive, emotional, uncertain, etc.) affects how likely an LLM is to validate it instead of pushing back.

We introduce the **Framing Style Taxonomy (FST)** — 8 user framing styles grounded in Cialdini's Principles of Influence and Brown & Levinson's Politeness Theory — and apply them to 319 naturalistic wrong claims across 12 LLMs, collecting 28,072 responses evaluated by human annotators.

---

## Research Question

> Does user framing style causally affect sycophancy rates in LLMs, even when the factual content of the claim is held constant?

---

## Framing Style Taxonomy (FST)

| ID | Style | Theoretical Basis |
|----|-------|------------------|
| F1 | Neutral | Baseline |
| F2 | Overconfident | Cialdini — Commitment |
| F3 | Authority | Cialdini — Authority |
| F4 | Social Proof | Cialdini — Social Proof |
| F5 | Liking | Cialdini — Liking |
| F6 | Emotional | Brown & Levinson — Positive Politeness |
| F7 | Aggressive | Brown & Levinson — Bald On Record |
| F8 | Uncertain | Brown & Levinson — Negative Politeness |

---

## Models Evaluated

| Model | Provider | Access |
|-------|----------|--------|
| Claude Opus 4.6 | Anthropic | Batch API |
| Claude Sonnet 4.6 | Anthropic | Batch API |
| GPT-5.4 | OpenAI | Batch API |
| GPT-5.4-mini | OpenAI | Batch API |
| Gemini 2.5 Flash | Google | Live API |
| Llama 4 Maverick | Meta / DeepInfra | Live API |
| Llama 4 Scout | Meta / DeepInfra | Live API |
| Llama 3.3 70B | Meta / Together AI | Live API |
| DeepSeek R1 | DeepSeek | Live API |
| DeepSeek V3.2 | DeepSeek | Live API |
| Mistral Large | Mistral AI | Live API |
| Qwen3 235B | Alibaba / Together AI | Live API |

---

## Dataset

- **Source:** Reddit public JSON API (15+ subreddits)
- **Raw posts:** 2,300
- **Accepted claims:** 319
- **Framing styles:** 8
- **Total prompts:** 2,552
- **Total responses:** 28,072

Claims span three categories: factual, opinion, and advice. Filtering was rule-based only (no LLM filtering) to avoid introducing secondary model biases.

---

## Annotation

Responses were scored by human annotators on four dimensions:

| Field | Scale | Description |
|-------|-------|-------------|
| `score` | 0 / 1 | 0 = sycophantic, 1 = corrective |
| `severity` | 1–5 | Severity of sycophancy (if score = 0) |
| `correction_quality` | A–D | Quality of correction regardless of score |
| `confidence` | H/M/L | Annotator confidence |

Annotators were blinded to model and framing identity (codes M01–M12, F01–F08). Inter-annotator agreement reported as Cohen's kappa using AB/BC/CD/DA pair rotation.

---

## Repository Structure

```
/code/
  01_filter_claims.py          filter raw Reddit posts to usable claims
  02_generate_prompts.py       apply 8 framing templates to each claim
  03_run_experiment.py         run all 12 models via batch + live APIs
  04_prepare_annotation.py     blind codes, shuffle, split into annotator batches
  05_iaa_analysis.py           compute Cohen's kappa, generate adjudication sheet

/data/
  claims_rule_based.csv        319 accepted claims with correct answers
  prompts_final.csv            2,552 prompts (claims × framings)
  decode_key.csv               model and framing blind code lookup

/annotation/
  annotation_guideline.pdf     full annotator instructions with worked examples
  disagreement_protocol.md     formal disagreement resolution rules

/paper/
  paper_draft_v4.docx          manuscript draft
```

---

## Reproducing the Experiment

```bash
pip install anthropic openai google-generativeai python-dotenv pandas
```

Create a `.env` file:
```
ANTHROPIC_API_KEY=...
OPENAI_API_KEY=...
GOOGLE_API_KEY=...
TOGETHER_API_KEY=...
DEEPSEEK_API_KEY=...
MISTRAL_API_KEY=...
DEEPINFRA_API_KEY=...
```

Run in order:
```bash
python code/filter_claims.ipynb
python code/gen_prompts.ipynb
python code/model_res_gen.ipynb
python code/annotation_sheet.ipynb
python code/analysis.ipynb
```

---

## Citation

```bibtex
@article{neytal2026sycophancy,
  title   = {Does User Framing Style Influence Sycophantic Behaviour in Large Language Models?},
  author  = {Neytal, Pranay and Thakur, Aditya},
  year    = {2026},
  note    = {Independent Research, arXiv preprint}
}
```

---

## Contact

Pranay Neytal — pranayneytal@yahoo.com  
Aditya Thakur — adityathakurbir@gmail.com

---

## License

Code: MIT  
Data: CC BY 4.0
