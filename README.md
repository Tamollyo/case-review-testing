# Claude Prompt Test Harness

A browser-based tool for running the same prompt multiple times in parallel against the Claude API, comparing outputs side by side, and scoring consistency. Built for legal teams evaluating prompt reliability across case document workflows.

---

## Live tool

**[YOUR-USERNAME.github.io/prompt-test-harness](https://tamollyo.github.io/case-review-testing/)**

> Replace the link above with your actual GitHub Pages URL after deploying.

---

## What it does

When you're building prompts for consistent, structured outputs — like personal injury case summaries, red flag identification, or fact classification — you need to know whether Claude gives the same answer every time. This tool runs your prompt N times simultaneously, shows you all outputs side by side, and lets you score each one.

**Key features:**

- **Parallel runs** — fires 2–8 API calls at the same time, no waiting for each one to finish
- **File upload** — attach PDFs and images to send alongside your prompt on every run
- **Pre-loaded PI skill** — system prompt is pre-filled with the personal injury case summary instructions; edit it directly in the browser
- **Scoring** — rate each run Good / Ok / Bad; summary stats update in real time
- **Highlight diff** — marks words that appear in one run but not others, making inconsistencies immediately visible
- **Session history** — last 10 prompts saved to browser storage; click any to reload
- **Export** — download results as JSON or Markdown for sharing or record-keeping

---

## Setup

### Option 1 — Use the hosted version (recommended)

Open the live link above. Paste your Anthropic API key in the top-right field and start testing.

### Option 2 — Run locally

1. Download `index.html` from this repo
2. Open it in any modern browser (Chrome, Firefox, Safari)
3. Paste your API key in the header and go

No server, no install, no dependencies beyond a browser.

---

## Getting an API key

1. Go to [console.anthropic.com](https://console.anthropic.com)
2. Sign in and navigate to **API Keys**
3. Click **Create Key**, name it (e.g. `test-harness`), and copy it immediately — it's only shown once
4. Paste it into the header field of the harness

**Each team member should use their own key.** Keys are entered locally in your browser and sent directly to Anthropic — they are never stored on any server or visible to anyone else.

---

## How to use it

### 1. Set your system prompt
The system prompt defines Claude's role and rules. It's pre-filled with the PI case summary skill instructions. Edit it to test different prompt versions. This stays fixed across all runs in a session.

### 2. Write your user prompt
The user prompt is the specific task — what you're asking Claude to do with the documents. Change this to test different task framings (e.g. "summarize liability facts" vs "identify red flags").

### 3. Attach documents
Drag and drop or click to browse. Supports PDF, PNG, JPG, and WEBP up to 20 MB each. Files are read into memory and sent with every run automatically.

### 4. Configure settings

| Setting | What it does | Recommended for testing |
|---|---|---|
| Model | Sonnet 4 (more capable) or Haiku 4.5 (faster, cheaper) | Sonnet 4 for quality evals; Haiku for high-volume runs |
| Runs | How many parallel calls to fire (2–8) | 3–5 is usually enough to spot variance |
| Temperature | 0 = deterministic, 1.0 = most variable | 0.7 for general testing; 0 to confirm consistency |
| Max tokens | Cap on response length | 1000–2000 for full case summaries |

### 5. Run and score
Click **Run tests**. Results appear as cards. Score each one Good / Ok / Bad. Use **Highlight diff** to see where outputs diverge. Add notes to any card to record why it passed or failed.

### 6. Export
Download results as JSON (for programmatic use) or Markdown (for sharing with the team or pasting into docs).

---

## Testing strategy for PI prompts

**Testing system prompt wording** — keep the user prompt identical, change only the system prompt between sessions, compare results.

**Testing task framing** — keep the system prompt fixed, vary the user prompt across sessions (e.g. "list all red flags" vs "identify inconsistencies in the record").

**Testing consistency** — keep both fixed, set runs to 5, temperature to 0.7, and look at the diff view to see where the model diverges.

**Things to watch for in PI outputs:**
- Does `[UNDISPUTED]` / `[CONTESTED]` / `[UNCLEAR]` classification stay consistent across runs?
- Are the same red flags identified in every run, or do some get dropped?
- Does the tone stay neutral, or does editorializing creep in at higher temperatures?
- Do source citations appear consistently and correctly?

---

## Prompts used by this team

| Prompt | Purpose |
|---|---|
| Summarize liability facts | Tests fact classification consistency |
| Identify red flags and inconsistencies | Tests completeness and neutrality |
| List all diagnoses and ICD-10 codes | Tests structured extraction accuracy |
| Summarize damages | Tests numeric accuracy and table structure |

---

## Cost

Each run makes one API call. Approximate costs at standard pricing:

| Model | Per run (1000 output tokens) | 3-run session |
|---|---|---|
| Claude Sonnet 4 | ~$0.015 | ~$0.045 |
| Claude Haiku 4.5 | ~$0.001 | ~$0.003 |

Costs are billed to whichever API key is used. Check [anthropic.com/pricing](https://www.anthropic.com/pricing) for current rates.

---

## Updating the tool

To deploy a new version:

**Via GitHub UI:**
1. Click `index.html` in the repo
2. Click the pencil (edit) icon
3. Paste the updated code and commit
4. GitHub Pages redeploys automatically within ~60 seconds

**Via git:**
```bash
git clone https://github.com/YOUR-USERNAME/prompt-test-harness
# replace index.html with updated version
git add index.html
git commit -m "update harness: [describe change]"
git push
```

---

## Security notes

- API keys are entered locally and sent directly from your browser to Anthropic — they never touch this repo or any intermediate server
- Files are processed entirely in-browser memory and never uploaded anywhere other than Anthropic's API
- Session history is stored in your browser's `localStorage` — it does not sync across devices or users
- This tool makes direct browser-to-API calls using Anthropic's `anthropic-dangerous-direct-browser-access` header, which is intended for trusted internal tooling of this kind

---

## Questions

Contact the HR or legal ops team for access questions, or open an issue in this repo for bug reports.
