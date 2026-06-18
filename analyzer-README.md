# Resume Analyzer Auto

A single-file AI-powered resume and CV analysis tool that runs entirely in the browser. Drop in a PDF or Word document, configure your target role, and get a comprehensive audit across 15 analysis steps — ending in a fully rewritten, job-targeted document ready to download. No backend, no account, no data leaves your browser except the API calls you authorize.

Built to work with both the **Anthropic Claude API** (cloud) and **LM Studio** (local model, fully private).

---

## What It Does

### 15 Analysis Steps

Each step is a separate AI call producing a focused analysis or rewrite. Steps run sequentially so each can build on prior results.

| # | Step | What it produces |
|---|------|-----------------|
| 1 | **Contact & Header** | ATS-safe header rewrite, PII risk check, GitHub/portfolio URL audit |
| 2 | **Core Competencies & Skills** | Keyword gap analysis, credential/cert check, skills section rewrite |
| 3 | **Role Relevance Triage** | Full role history evaluated — Full Write / Condensed / Early Career Group / Drop |
| 4 | **Transferable Skills** | Cross-domain value surfaced with specific reframe language |
| 5 | **Professional Summary** | Summary rewritten using skills gaps, triage decisions, and transferable skills |
| 6 | **Experience & Accomplishments** | Role-by-role rewrite per triage decisions, 3-tier achievement quality audit |
| 7 | **Supporting Sections** | Education, certs, publications (APA/MLA/Bluebook), patents, language proficiency |
| 8 | **Career Narrative & Progression** | Trajectory, gaps, pivots, short tenures + red flag pre-emption |
| 9 | **Format, Structure & ATS** | Section ordering, ATS danger zones, page length, date consistency |
| 10 | **Tailoring & Keyword Alignment** | JD match score, missing keywords, language substitutions, anti-stuffing enforcement |
| 11 | **Company Research** | Live web search on target employer, resume alignment tips, cover letter hooks |
| 12 | **Level & Compensation Signals** | Seniority projection, scope signals, comp positioning guidance |
| 13 | **Consistency & Voice Audit** | Cross-section tone and persona consistency check on all rewritten content |
| 14 | **Fix Reconciliation** | All recommended changes compiled into a numbered checklist |
| 15 | **Build Document** | Checklist applied to produce the final Resume or CV |

Steps 10 and 11 are **optional** — skipped automatically if no job description or company name is provided. Steps 14 and 15 run automatically as system steps after the analysis is complete.

---

### Review, Revision & Approval

After the document is built it opens in a live editor. You can:

- **Edit directly** — type changes anywhere in the document
- **Apply feedback** — describe what to change in plain language; the AI revises only what you request, preserves everything else, and never alters the factual context of any role
- **Version history** — each revision is tracked; prior versions are kept for undo on failure
- **Approve** — locks the document and unlocks five post-approval generators

---

### Post-Approval Generators

Once you approve the document, five generators unlock — each drawing on the approved document and prior analysis context.

| Generator | Output |
|-----------|--------|
| **LinkedIn Profile (v2)** | 6 sections: Headline (3 variations), About, Experience (full career history), Skills, Featured recommendations, Profile Settings & Recommendations. Experience section uses the full role history from triage — not just the roles that survived resume trimming. |
| **Cover Letter** | Personalized 300–400 word letter, company-aware when research was run. Four-paragraph structure with opening hook, evidence, company reference, and confident close. |
| **Interview Prep & STAR Stories** | 12–15 likely questions by category, 5–6 STAR stories from resume achievements, salary negotiation + offer evaluation framework. |
| **Email Templates** | Same-day thank-you note, 7–10 day follow-up email, LinkedIn connection request to interviewer. |
| **1-Page Resume Variant** | Aggressively trimmed single-page version (resume mode only — CVs are not eligible). Opens in a second editor tab alongside the full document. |

---

## Modes

### Claude (default)
Uses the Anthropic Messages API. Requires an API key configured in the environment. Each step is a standard `claude-sonnet-4-6` call. Company Research uses the web search tool.

### LM Studio
Uses a locally running LM Studio server at `http://localhost:1234` (configurable). All processing stays on your machine — nothing leaves your network. Recommended: a model with a 16K+ context window and 13B+ parameters.

**LM Studio notes:**
- Company Research (Step 11) requires a web search plugin active in LM Studio server settings. Skips gracefully without one.
- Fix Reconciliation (Step 14) generates the change checklist directly in JavaScript for LM Studio — no API call needed — to avoid context window overhead.
- Compile (Step 15) applies LM Studio-aware truncation on analysis step inputs to stay within context limits while preserving rewrite content at full quality.

---

## Resume vs CV

The tool handles both formats. The choice affects every step.

**Resume** — 1–2 pages, role-targeted, ruthlessly focused on relevance. Standard for most private sector positions in the US, Canada, and UK.

**CV (Curriculum Vitae)** — No page limit, comprehensive record of the full career. Standard in academia, research, medicine, and international hiring. Every role is preserved. Publications, patents, and credentials get full treatment.

A "Not sure which to choose?" toggle below the selector explains both formats and when to use each.

---

## Guardrails

Several explicit constraints are enforced across all prompts:

**Authenticity** — The AI will never change the factual context of where or how a skill was used. A retail purchasing role stays retail. A candidate must be able to stand behind every word in an interview.

**No metric invention** — Quantified metrics are only added when the original resume already supports them. Missing metrics trigger targeted questions to help the candidate find the data themselves.

**Anti-stuffing** — JD keywords are only recommended where the candidate can authentically back them up in an interview. Genuine gaps are flagged, not papered over.

**Role vs industry priority** — The target role is always the primary evaluation lens. The industry is environmental context only. A Purchasing Agent in Manufacturing needs procurement skills, not manufacturing operations skills.

**Context preservation across rewrites** — Each step that rewrites content is audited by the Voice step, which reads all rewritten sections holistically to ensure consistent persona and tone throughout.

---

## Files

```
resume-analyzer.html    Single self-contained file. No build step, no dependencies to install.
resume-architect.html   LinkedIn Profile Architect (standalone, step-by-step tool)
resume-builder.jsx      LinkedIn Profile Architect (React/JSX version for Claude artifacts)
```

---

## Usage

### Quick start
1. Open `resume-analyzer.html` in any modern browser
2. Select **Claude** or **LM Studio** mode
3. Choose **Resume** or **Full CV**
4. Enter your industry, target role, and optionally a job description and company name
5. Upload your PDF or Word document
6. Click **Analyze Resume**

### API key (Claude mode)
The tool calls `https://api.anthropic.com/v1/messages` directly from the browser. Anthropic's API supports browser-side requests with proper CORS headers. Your API key is never stored — it is used only for the duration of the session.

### LM Studio setup
1. Download and install [LM Studio](https://lmstudio.ai)
2. Load a model (13B+ parameters recommended; Gemma, Llama, Mistral, Qwen all work)
3. Go to **Server** → **Start Server** in LM Studio
4. Set context length to 16K or higher in server settings
5. Toggle **LM Studio** in the tool and confirm the server URL

---

## Technical Notes

- **No backend** — all processing is client-side JavaScript. The only external calls are to the Anthropic API or your local LM Studio server.
- **PDF extraction** — uses PDF.js loaded from cdnjs. Text-based PDFs extract cleanly. Scanned or image-based PDFs may produce poor extraction — use the preview toggle before running.
- **DOCX extraction** — uses Mammoth.js loaded from cdnjs.
- **No data storage** — nothing is persisted between sessions. Copy your outputs before closing the tab.
- **Token budgets** — standard steps use 4,000 max tokens; LinkedIn and interview prep use 6,000; all calls include null guards to prevent `content: undefined` errors in LM Studio.

---

## Acknowledgments

Built iteratively with [Claude](https://claude.ai) (Anthropic). PDF extraction via [PDF.js](https://mozilla.github.io/pdf.js/). DOCX extraction via [Mammoth.js](https://github.com/mwilliamson/mammoth.js).
