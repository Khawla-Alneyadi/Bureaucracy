# Bureaucracy Analyzer

A lightweight Gradio app that computes a **Bureaucracy Index (BI)** from 15 sliders (1–10) and, optionally, calls an LLM (Google **Gemini** or **OpenAI**) to return a **concrete, step‑by‑step plan** to reduce bureaucracy (quick wins + 30/60/90 roadmap + KPIs).


## ✨ Features

* **BI score (0–100%)** from a weighted sum of 15 dimensions (approval layers, redundancy, wait time, automation level, etc.).
* **Clear categories**: Low, Moderate, High, Extreme.
* **LLM advice** (optional): send the sliders + score to Gemini or OpenAI to generate tailored actions and metrics.
* **Gradio UI** with accessible sliders and Markdown output.


## 🧮 How the score is calculated

Each slider is 1–10 (1 = best, 10 = worst). The app uses these weights (sum = **0.98**):

| Key | Weight | Description                                |
| --- | -----: | ------------------------------------------ |
| A   |   0.08 | Approval Layers                            |
| PC  |   0.08 | Process Complexity                         |
| TC  |   0.08 | Time Per Step                              |
| T   |   0.12 | Total Process Time                         |
| WA  |   0.05 | Waiting for Approvals                      |
| DL  |   0.05 | Documentation Load                         |
| RF  |   0.08 | Redundancy                                 |
| EE  |   0.05 | Employee Effort                            |
| AW  |   0.05 | Administrative Workload                    |
| MR  |   0.05 | Meetings Required                          |
| CWT |   0.08 | Customer Wait Time                         |
| ES  |   0.05 | Employee Frustration                       |
| CI  |   0.05 | Customer Impact                            |
| AL  |   0.05 | Automation Level (1 automated → 10 manual) |
| PRR |   0.06 | Rejection/Resubmission Rate                |

**Normalization**: `BI = clamp( (Σ(value×weight) / (10×Σweights)) × 100, 0, 100 )` → `10×Σweights = 9.8`.

**Bands**

* ≤30 → Low
* 30–50 → Moderate
* 50–70 → High
* > 70 → Extreme

> The weights are course/project defaults. Adjust them in `WEIGHTS` to match your organization.


**Gemini (REST)**

* Endpoint: `https://generativelanguage.googleapis.com/v1beta/models/{model}:generateContent`
* Headers: `Content-Type: application/json`, `x-goog-api-key: $GEMINI_API_KEY`
* Body shape: `{ "contents": [{ "role": "user", "parts": [{"text": "..."}]}] }`

## 📚 References

* **Gradio Blocks**: [https://www.gradio.app/4.44.1/docs/gradio/blocks](https://www.gradio.app/4.44.1/docs/gradio/blocks)
* **Hugging Face Spaces – README YAML config**: [https://huggingface.co/docs/hub/en/spaces-config-reference](https://huggingface.co/docs/hub/en/spaces-config-reference)
* **Hugging Face Spaces – manage secrets/variables**: [https://huggingface.co/docs/huggingface\_hub/en/guides/manage-spaces](https://huggingface.co/docs/huggingface_hub/en/guides/manage-spaces)
* **Spaces overview (env vars exposed to apps)**: [https://huggingface.co/docs/hub/en/spaces-overview](https://huggingface.co/docs/hub/en/spaces-overview)
* **Gemini API – generateContent (REST)**: [https://ai.google.dev/api/generate-content](https://ai.google.dev/api/generate-content)
* **Gemini API – docs hub**: [https://ai.google.dev/gemini-api/docs](https://ai.google.dev/gemini-api/docs)
