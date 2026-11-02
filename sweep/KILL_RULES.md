# Binding rules for kill-test subagents (Phase 3)

Context: MSc AI thesis topic search (Utrecht University), *network analysis of the stock market and between listed companies*. Today is 2 Nov 2026. Your training knowledge stops around mid-2026: never rely on memory for papers; search, and open what you cite. Read `/home/claude/thesis-proposals/sweep/ideas_raw.md` for the full idea table (IDs I1–I30, with the closest work already known).

## Your job
For each idea assigned to you, **try hard to KILL it**: find 2025–2026 work (or older work if it does exactly the same thing) that already does the core of the idea.

1. Run **at least 3 distinct, targeted searches per idea** (WebSearch, standard mode; 3–8 words). Use *new wording*, not the wording in the table: synonyms, the task name from a neighbouring field, venue names ("KDD 2026", "ICAIF 2025", "arXiv 2026"), dataset names. Where useful, also query the OpenReview API: `https://api2.openreview.net/notes/search?term=<url-encoded>&limit=25`.
2. **Open with WebFetch** every paper that might kill or weaken the idea (arXiv abs/html pages, publisher pages). Only fetch URLs that appeared in a search result or in a page you opened; other URLs trigger an approval prompt nobody can answer, so skip them. Google Scholar, DBLP, Semantic Scholar and ACM DL are blocked: do not try them.
3. Verdict per idea:
   - **KILLED**: a paper already does the core task on comparable (equity/company) data with comparable evaluation. Name it.
   - **WEAKENED**: close work covers a major part. Say exactly which part remains open and propose the sharpened, still-open version.
   - **SURVIVED**: the closest work differs in at least two essential dimensions (e.g. task, sign/direction, data domain, evaluation protocol). Name those dimensions.
4. Also assess quickly: **public-data feasibility** (which public sources; any blocker) and a **one-line feasibility note** for a single MSc student in ~30 weeks who relies on generative AI for coding.
5. Be honest: if searches find nothing, say so and list what you searched. Absence after 3+ good searches is evidence, not proof.
6. **Never reproduce long quotes** (max one quote < 15 words per source); paraphrase.

## Verification legend
[FT] full text read · [ABS] abstract/record page opened · [UNVERIFIED] snippet only (never used as the killer).

## Output
Write ONE Markdown file at the path given in your task. Do not edit other files and do not run git. Structure per idea:

```
## I<n> — <short name>
- Queries: 1) "..." → outcome; 2) ...; 3) ...
- Closest work found (new): bibkey — Title — link — first public date — venue/preprint — [FT]/[ABS] — how close (1–2 sentences)
- Verdict: SURVIVED / WEAKENED / KILLED — reason (name the dimensions or the killer paper)
- Sharpened version (if weakened/survived): 1–2 sentences
- Public data: ...; Feasibility (30 weeks, GenAI-assisted coding): ...
```

At the end, add `## BibTeX (new papers only)` with one entry per newly opened paper (key `lastnameYEARfirstword`, `url`, `note = {[FT]/[ABS] first public date; venue}`), and a `## Query log` table.

## Final reply to the parent (< 250 words)
A one-line verdict per idea, plus any paper that changes the picture substantially.
