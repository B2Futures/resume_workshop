# Resume Builder

A single-file AI-powered resume and CV builder that works job by job, industry by industry. No existing resume required. Add your roles, let the AI generate strong industry-specific bullet points for each one, then build either a complete work history or a date-trimmed version. The output is a high-quality starting point — run it through **Resume Analyzer** to target it for a specific job before submitting anywhere.

Works with both the **Anthropic Claude API** (cloud) and **LM Studio** (local model, fully private).

---

## The Concept

Most resume builders are form-fillers. They ask for your job title and a few bullet points, then format it. The output is only as good as what you typed in — which is the problem the builder is designed to solve.

Resume Builder approaches each role like a career coach would: it reads the job title and industry, generates targeted questions about what you actually did, and uses your answers to write strong, metric-backed, action-verb-led bullets in proper resume format. Your answers inform the content but never appear verbatim. The result is a document that sounds like a professional wrote it — because the AI had enough information to actually do so.

The industry tag travels with the role, not the document. A role in retail purchasing gets retail purchasing bullets. A role in manufacturing operations gets manufacturing operations bullets. Someone who spent ten years in one field before moving to another gets accurate, industry-appropriate content for every stage of their career.

---

## How It Works

### Step 1 — Personal Information
Name, contact details, LinkedIn URL, optional portfolio or GitHub link. No target role, no job description — the builder isn't about targeting. That's the analyzer's job.

### Step 2 — Work History

Add each role with:
- Company name, job title, **industry for this role**, start/end dates
- Optional description (anything you remember — format doesn't matter)

**Expert Interview** activates automatically when a role description is under 40 words — roughly two short sentences. That threshold exists because the AI can't produce specific, metric-backed bullets without something to work from. The interview fills that gap.

**What the expert interview asks:**

Questions are generated fresh per role based on the job title and industry — not a fixed template. A fast food crew member gets questions about volume, speed, customer flow, and coverage. A restaurant manager gets questions about labor cost percentage, food cost, scheduling, and revenue per shift. A manufacturing supervisor gets questions about throughput, downtime, safety incidents, and team size. A software engineer gets questions about system scale, deployment frequency, and technical debt reduction.

**Question count varies by seniority:**
- Entry-level roles (crew, assistant, associate, intern): 3–4 questions
- Mid-level roles: 5–6 questions
- Senior/executive roles (director, VP, C-suite): 6–8 questions

The interview is conversational — one question at a time. Answers are collected but never copied into the document. The AI uses them to write bullets, then discards them.

**After generation:**
- Review the bullets in the card
- Use the feedback field to request specific changes: *"add the cost savings I mentioned"*, *"make bullet 2 more specific"*, *"the second role should mention the team I managed"*
- Click Approve when the bullets are right — the sidebar dot turns green
- Unapprove anytime to make more changes before the final build

### Step 3 — Supporting Sections

- **Education** — school, degree, field, year, optional GPA
- **Certifications & Licenses** — name, issuing body, year
- **Skills** — rough list; the AI organizes into categories (Technical Skills, Domain Knowledge, Tools & Platforms, Soft Skills) and adds industry-standard keywords based on your role history
- **CV only:** Publications (with citation format: APA / MLA / Bluebook / Business), Languages (standard proficiency levels: Native through Basic), Awards

### Step 4 — Build Document

Two options:

**Master Document**
Every role included, fully written, no cuts. Your complete professional record. Good for long-term record keeping, feeding the analyzer, or building a CV. If you're in CV mode, use this.

**Recent History**
Same bullets, same quality — trimmed by date only. No AI judgment about which roles are relevant; just a hard cutoff.

- **10 Years** — Standard for most resume formats. Recommended if earlier roles are in a different industry or less relevant to where your career is now.
- **15 Years** — Better for senior roles, technical careers, or fields where depth of experience matters. Recommended if work from 10–15 years ago is still directly relevant.

Roles with no parseable end date are always included. Current roles are always included.

**After building:**
The document opens in a review editor. Edit directly or describe changes in plain language. Each revision is versioned. Copy the text or save as PDF when ready.

---

## Important: This Output Is Not Application-Ready

The document produced by the builder is a high-quality starting point — not something to send to a recruiter. It has not been:

- Tailored to a specific job opening
- Checked for ATS compatibility
- Aligned to a job description's keywords and language
- Evaluated for career narrative and progression
- Audited for seniority signals and compensation positioning

**Before submitting anywhere, run the output through Resume Analyzer.** The analyzer will tailor the document to a specific role, fix ATS issues, align keywords to a job description, audit the career narrative, and produce a polished final version ready to submit.

---

## Resume vs CV

**Resume** — Industry-specific bullet points, standard formatting, US/UK private sector standard. 3–5 bullets per role. Skills section limited to most relevant keywords. Good starting point for targeted applications through the analyzer.

**CV (Curriculum Vitae)** — Same bullets plus publications, languages with standard proficiency levels (Native / Fluent C2 / Advanced C1 / Upper-Intermediate B2 / Intermediate B1), and additional academic sections. 4–7 bullets per role with fuller context. No page limit. Standard in academia, medicine, research, and international hiring.

Switch the toggle before starting — it affects what fields appear and how content is generated per role.

---

## Modes

### Claude (default)
Uses the Anthropic Messages API with `claude-sonnet-4-6`. Question generation, bullet generation, bullet refinement, assembly, and feedback revisions each use a separate focused API call.

### LM Studio
Uses a locally running LM Studio server at `http://localhost:1234` (configurable). All processing stays on your machine. Recommended: 13B+ parameter model, 16K+ context window.

---

## Files

```
resume-builder.html    Single self-contained file. No build step, no dependencies to install.
resume-analyzer.html   Resume Analyzer — for targeting, ATS checking, and job-specific tailoring
```

---

## Usage

### Quick start
1. Open `resume-builder.html` in any modern browser
2. Select **Claude** or **LM Studio** mode
3. Choose **Resume** or **Full CV**
4. Click **Personal Information** and fill in your details
5. Click **Work History**, then **+ Add a Role** for each job
6. Fill in title, company, industry, and dates — add a description if you have one
7. Click **Generate Bullets** — the expert interview activates automatically if needed
8. Review, refine, and approve each role
9. Fill in **Supporting Sections**
10. Go to **Build Document**, choose Master or Recent History, click Build

### API key (Claude mode)
The tool calls `https://api.anthropic.com/v1/messages` directly from the browser. Your API key is never stored — it is used only for the duration of the session.

### LM Studio setup
1. Download and install [LM Studio](https://lmstudio.ai)
2. Load a model (13B+ parameters recommended)
3. Go to **Server** → **Start Server**
4. Set context length to 16K or higher
5. Toggle **LM Studio** in the tool and confirm the server URL

---

## Technical Notes

- **No backend** — all processing is client-side JavaScript. The only external calls are to the Anthropic API or your local LM Studio server.
- **No data storage** — nothing is persisted between sessions. Copy your output or save as PDF before closing the tab.
- **No file upload required** — unlike the analyzer, the builder starts from scratch. There is no file extraction step.
- **Per-role generation** — bullets are generated independently per role, not in a single large call. This keeps each call focused, produces better quality per role, and allows individual roles to be regenerated without rebuilding the entire document.
- **Assembly call** — the final Build step makes one additional call to generate the Professional Summary from the full picture of all roles, organize skills into categories, format credentials, and stitch everything into a coherent document.
- **Token budgets** — question generation and bullet generation use 4,000 max tokens. The assembly call uses 6,000 max tokens to handle long work histories.
- **Authenticity enforced** — the AI will never invent metrics. Where numbers are missing, it writes `[add your number]` as a placeholder. It will never change the factual context of any role.

---

## Acknowledgments

Built iteratively with [Claude](https://claude.ai) (Anthropic).
