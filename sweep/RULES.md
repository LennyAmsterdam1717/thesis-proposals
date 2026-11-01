# Binding rules for the theme sweep subagents (Phase 1)

You are mapping the **2025–2026 research literature** for one theme of an MSc AI thesis topic search: *network analysis of the stock market and between listed companies* (Utrecht University; supervisor works on network analysis, text analysis, knowledge graphs, graph-based financial-crime detection). Today is **1 November 2026**. Your training knowledge stops around mid-2026, so you MUST search; never cite a paper from memory.

## 1. Recency rule
- **Primary:** first public in **2026** (Jan–Oct 2026). **Secondary:** **2025**.
- A paper counts as 2025/2026 if it appeared in a 2025/2026 venue *or* was first posted (arXiv v1) in 2025/2026. Record both dates if they differ, and whether it is peer-reviewed or a preprint.
- Older work: only as clearly labelled **[BACKGROUND]** (original dataset, classic method, definition). At most 2 per theme. Never use old work as evidence that a niche is open or closed.
- If a theme has few 2025–2026 papers, say so explicitly: that is a finding.

## 2. Only cite what you opened
- Every paper in your file must have been **opened by you with WebFetch** (abstract page at minimum).
- Legend: **[FT]** you read full text (arXiv HTML/PDF or publisher full text) · **[ABS]** abstract or record page only · **[UNVERIFIED]** seen in a search snippet but not opened — list these ONLY in the separate "seen but not opened" list, never in the main entries.
- Do not invent authors, venues, numbers or dates. If a field is unknown, write "unknown".
- Prefer [FT] for the most relevant ~4–6 papers (use `https://arxiv.org/html/<id>` or `/pdf/<id>`), and read their related-work sections to snowball.

## 3. Access map (tested tonight)
- **Works:** WebSearch; WebFetch on `arxiv.org/abs/<id>`, `arxiv.org/html/<id>`, `arxiv.org/pdf/<id>`, `arxiv.org/list/<cat>/recent` and monthly listings like `arxiv.org/list/q-fin.ST/2026-03`; OpenReview API `https://api2.openreview.net/notes/search?term=<url-encoded terms>&limit=25` (and `https://api2.openreview.net/notes?forum=<id>`); `papers.cool/venue/<VENUE.YEAR>` (e.g. AAAI.2026, IJCAI.2026, KDD.2026 — lists accepted papers); publisher pages (sciencedirect, springer, mdpi, ieee abstract pages often work); GitHub pages; emergentmind.com paper pages.
- **Blocked (do not retry, do not work around):** Google Scholar, arXiv search page, DBLP, Semantic Scholar, ACM DL, OpenAlex.
- Shell (Bash) reaches only GitHub; you do not need the shell. **No data downloads.**

## 4. Query style
Short Scholar-style queries (3–8 words) with year terms ("2026", "2025"), venue terms ("KDD 2026", "ICAIF 2025", "arXiv"), and synonyms. Run **at least 12 distinct queries** for your theme; log every query with a one-phrase outcome. Snowball from the reference lists / related-work sections of the best 2026 papers.

## 5. Targets for your theme
- **10–14 opened papers** in the main list, of which ≥75% from 2025–2026 and as many from 2026 as exist (aim ≥40% 2026).
- If fewer than 6 relevant 2025–2026 papers exist, say so and explain what you searched.
- Crypto-only papers are out of scope unless the method clearly transfers to stock/company networks (label them "[crypto, transferable]").
- AML / money-laundering papers are out of scope (supervisor considers it crowded) unless the method is a clear transfer candidate.

## 6. Output file
Write **one Markdown file** at the path given in your task (e.g. `/home/claude/thesis-proposals/sweep/T1.md`). Do not edit any other file. Do not run git. Structure:

```
# T<n> sweep — <theme name>
Run by subagent, <date>. Papers opened: N (2026: a, 2025: b, older background: c). [FT]: x, [ABS]: y.

## A. Papers (main list)
### <bibkey> — <Title>
- Citation: Authors (first 3 + et al.), *Title*, Venue/arXiv, year.
- Link: <URL you opened>
- First public: <arXiv v1 date or venue date>; Venue: <venue or "preprint">; Peer-reviewed: yes/no/unknown
- Verification: [FT] or [ABS]
- Summary: 1–3 sentences.
- Graph type: static/temporal; homogeneous/heterogeneous; signed? directed? hypergraph?
- Task: classification / regression / ranking / link prediction / anomaly detection / causal discovery / structure learning / other
- Data: markets/datasets; public? (yes/no/partly; which source)
- Code available? (link if stated, else "not stated")
- What it leaves open: 1–2 sentences (limitations, gaps, untested settings)

## B. Seen but not opened [UNVERIFIED]
- Title — where seen (query) — why not opened

## C. Crowding estimate
- Approx. number of 2025–2026 papers on this theme you saw (opened + seen): N
- Dominant venues / groups / countries:
- Shared benchmarks and datasets:
- Typical evaluation weaknesses observed (look-ahead bias, random splits, survivorship, weak baselines, no transaction costs, ...):
- Verdict: crowded / moderate / thin — with one-paragraph justification.

## D. Open gaps and niche ideas (≥4)
Concrete thesis-sized gaps you noticed, each with the paper(s) that reveal the gap. Be creative: combinations with other themes (signed/temporal edges, link prediction, anomaly detection, heterogeneous graphs, credit-default contagion, LLM-built company graphs, explainability for analysts, geopolitical shocks, evaluation rigour).

## E. Public data and code noticed
Datasets, repos, libraries usable with public data.

## F. Query log
| # | Query | Tool | Outcome |
```

Then append a **BibTeX block** at the end of the same file under `## G. BibTeX`, one entry per main-list paper. Key format: `lastnameYEARfirstword` in lowercase (e.g. `zhang2026signed`). Include `url` and a `note` field like `note = {[FT] arXiv v1 2026-03-04; preprint}`.

## 7. Final reply to the parent
Keep it **under 350 words**: counts (opened, by year, FT/ABS), the 5 most important papers (bibkey + one line), crowding verdict, the 4 best gaps, and anything surprising. The file holds the details.
