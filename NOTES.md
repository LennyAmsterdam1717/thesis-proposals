# NOTES.md — decision log (stock / company network thesis, topic scouting)

Running log for the overnight topic-scouting run started **Sun 1 Nov 2026, 20:45 CET**. Newest phase at the bottom of each section. Legend used throughout the repo: **[FT]** full text opened · **[ABS]** abstract/record page only · **[UNVERIFIED]** could not open; mentioned, not relied on.

---

## 0. Setup and access tests (Phase 0, 1 Nov 2026 ~20:50)

### 0.1 Access tests

| Source | Route tried | Result | Consequence |
| --- | --- | --- | --- |
| **Google Scholar** | WebFetch `scholar.google.com/scholar?q=signed+graph+neural+network+stock&as_ylo=2025` | **Blocked** (`robots_disallowed`) | Per instructions: tried once, not retried, no workaround. "Cited by" checks left to Frank. |
| arXiv listing pages | WebFetch `arxiv.org/list/q-fin.ST/recent` | **Works** (listed Oct 2026 papers, IDs 2610.xxxxx) | Use listings for recent q-fin / cs.LG scans |
| arXiv abstract pages | WebFetch `arxiv.org/abs/2604.01315` | **Works** (title, authors, dates, versions, DOI) | Main route for [ABS]; `arxiv.org/html/<id>` and `/pdf/<id>` for [FT] |
| arXiv search | WebFetch `arxiv.org/search/?query=...` | **Blocked** (`robots_disallowed`) | Use WebSearch to find arXiv IDs instead |
| arXiv API (shell) | `curl export.arxiv.org/api/query` | **Blocked** (proxy 403) | — |
| OpenReview web UI | WebFetch `openreview.net/group?id=ICLR.cc/2026/Conference` | JS-only shell, no content | — |
| **OpenReview API** | WebFetch `api2.openreview.net/notes/search?term=stock%20graph` | **Works** (returned ICLR 2026 submissions, NeurIPS 2024) | Use for ICLR 2026 / NeurIPS 2025 venue checks |
| ACM DL | WebFetch `dl.acm.org/doi/proceedings/...` | **Blocked** (`client_error`) | Use arXiv versions, author pages, papers.cool |
| DBLP | WebFetch `dblp.org/search?q=Hulpus` and `dblp.org/pers/h/Hulpus:Ioana.html` | **Blocked** (robots / Anubis bot wall) | DBLP mirrors appear in WebSearch snippets only |
| DBLP / Semantic Scholar / OpenAlex APIs (shell) | curl | **Blocked** (proxy 403) | — |
| Semantic Scholar web | WebFetch search page | **Blocked** (`robots_disallowed`) | — |
| UU research portal | WebFetch `research-portal.uu.nl/en/persons/ioana-karnstedt-hulpus/publications/` | **Blocked** (`client_error`) | — |
| papers.cool venue pages | WebFetch `papers.cool/venue/AAAI.2026` | **Works** (lists accepted papers; keyword filter param ignored) | Possible venue scan route (AAAI/IJCAI/KDD 2026) |
| GitHub API (shell) | `curl api.github.com/search/repositories?q=...` | **Works** unauthenticated (rate-limited); `gh` token invalid | Use for code-availability checks in Phase 6 |
| PyPI (shell) | `curl pypi.org/simple/torch-geometric/` | Works | Prototype code can install libraries later |
| WebSearch | standard mode | Works | Main discovery route (Scholar-style queries) |

**Decision D0.1:** discovery = WebSearch (Scholar-style phrasing, year terms) + arXiv listings + OpenReview API + papers.cool; reading = WebFetch of `arxiv.org/abs|html|pdf`, publisher pages and GitHub READMEs. No Google Scholar.

### 0.2 Supervisor's own recent work (fit / overlap check)

Sources opened: UU staff page (`uu.nl/medewerkers/IRKarnstedtHulpus/Onderzoek`), papers.cool IJCAI 2026 listing, the TAML PDF on co-author Velegrakis' site, UU AI & Finance Lab news item, UU student-thesis repository record.

- **TAML** — Rico, **Hulpuș**, Leisink (ING), Zhao (ING), Velegrakis. *Translating Latent Representations for Money Laundering Detection.* **IJCAI 2026** [ABS via papers.cool + first page of PDF, https://velgias.github.io/docs/RicoHLZV26.pdf]. Jointly solves AML account classification and laundering sub-network discovery in one latent space via a supervised contrastive "translation" loss. → Her group is active in *financial-crime graphs with ING*; a stock-network thesis does **not** overlap, but "sub-network discovery" and "translation-based embeddings" are methods she knows well.
- **LAS-GNN** — appears as the 2025 UU MSc thesis of Stan Verlaan, *Powerful Graph Neural Networks for Money Laundering Pattern Detection* (https://studenttheses.uu.nl/handle/20.500.12932/50600) [ABS]. The ICAIF 2025 paper version mentioned in the briefing was **not found** by search tonight → [UNVERIFIED] as an ICAIF paper.
- **HERDGNN (CIKM 2026)** — still not found → [UNVERIFIED].
- **UU staff page research description** [FT]: graph-based fraud detection with ING; *earlier work on venture-capital investment graphs (Crunchbase) in an industry-funded project and in BSc/MSc theses she supervised*; knowledge bases, semantic relatedness, topic labelling; argumentation. → **Fit signal:** investor–company (heterogeneous, bipartite, temporal) graphs and text-derived knowledge graphs are both squarely in her comfort zone.
- **UU AI & Finance Lab** (news item, uu.nl) [FT]: lab works with ING on fraud/AML, risk management and large-dataset analysis; Karnstedt-Hulpuș quoted as lab representative; Joras Ferwerda (economist, AML) is coordinator. No public list of student projects on stock networks found → no overlap detected.

**Decision D0.2:** treat (a) text/knowledge-graph construction, (b) heterogeneous investor–company graphs and (c) sub-network/motif discovery as *supervisor-fit bonuses* in scoring. AML itself stays out (supervisor: crowded).

### 0.2b Data-source checks (no downloads; pages opened 1 Nov 2026)

| Source | Status checked tonight | Use |
| --- | --- | --- |
| **SEC Form 13F data sets** (sec.gov/data-research/sec-markets-data/form-13f-data-sets) | Public, quarterly ZIPs, **2013 Q2 → Jun–Aug 2026** (latest 96 MB file). Flattened XML extracts. | Common-ownership graphs (fund → stock bipartite, quarterly), temporal |
| **SEC Financial Statement Data Sets** | Public, quarterly ZIPs **2009 Q1 → 2026 Q3**, include SIC codes | Fundamentals, node features |
| **SEC Financial Statement *and Notes* Data Sets** | Public, monthly since Oct 2025, **text of notes included** (txt.tsv) | Text source for LLM/KG extraction (e.g. concentration-of-customers notes) |
| **EDGAR APIs** (data.sec.gov: submissions, companyfacts, frames; nightly bulk ZIPs) | Public, no key; must follow SEC fair-access policy | Filing timelines (8-K events incl. Item 1.03 bankruptcy), XBRL facts |
| **Credit-rating histories (Rule 17g-7(b))** | Public by rule: each NRSRO must post XBRL rating-action histories on its website, **12-month delay** (24 for investor-paid). S&P, KBRA, Egan-Jones pages found. A 2018 Oxford blog points to an open-source crawler + CSV mirror (ratingshistory.info) — mirror **not opened** (fetch needed approval) → [UNVERIFIED] currency | Rating downgrades/defaults as credit events (T7) |
| **FRED corporate spreads** | **ICE BofA OAS series limited to 3 years of history since April 2026** (FRED note). Moody's Baa−10y Treasury (BAA10Y) still long daily history. | Market-level credit stress only; no firm-level spreads |
| Prices (Stooq, yfinance), Kenneth French, Qlib data | Not checked tonight (fetch approvals); Qlib/benchmark repos on GitHub: GitHub REST *repo* endpoints are restricted to session-attached repos, *search* endpoint works | Check in Phase 6 via WebSearch-surfaced pages |

**Access note D0.5:** WebFetch on URLs that did not appear in a search result or earlier page triggered an approval prompt that nobody can answer tonight (ratingshistory.info, a FRED series page). Decision: fetch only URLs surfaced by WebSearch or by previously opened pages. Subagents work the same way (search first, then fetch).

### 0.3 Other setup decisions
- **D0.3:** Repo layout follows briefing §11. Raw per-theme sweep notes from subagents are kept in `sweep/T*.md` (provenance), and integrated into `lit_review.md`.
- **D0.4:** Paper "year" = first public date (arXiv v1 or venue, whichever earlier). Peer-review status recorded separately.

---

## 1. Search queries per theme
Every query (tool, wording, outcome) is logged in the §F table of each sweep file. Summary:
- **T1**: 23 logged queries — full table in `sweep/T1.md` §F.
- **T2**: 34 logged queries — full table in `sweep/T2.md` §F.
- **T3**: 40 logged queries — full table in `sweep/T3.md` §F.
- **T4**: 25 logged queries — full table in `sweep/T4.md` §F.
- **T5**: 37 logged queries — full table in `sweep/T5.md` §F.
- **T6**: 45 logged queries — full table in `sweep/T6.md` §F.
- **T7**: 28 logged queries — full table in `sweep/T7.md` §F.
- **T8**: 38 logged queries — full table in `sweep/T8.md` §F.
- **T0 (lead agent)**: 6 queries (13F common ownership + GNN; peer reaction to downgrade/default; IPO cold-start relations; earnings information transfer + GNN; LLM major-customer extraction from 10-Ks; Hoberg–Phillips TNIC). Outcomes in `sweep/T0_lead_notes.md`.

## 2. Decisions during the sweep
- **D1.1 (1 Nov, 20:52):** Ran 8 theme subagents in parallel (user's explicit request), all bound to `sweep/RULES.md`.
- **D1.2 (1 Nov, ~21:10):** The **5-hour usage limit was hit**. Three agents (T2, T3, T8) were cut off after writing their files; all eight files are complete (sections A–G present). The session resumed 2 Nov, 09:32. No agent was restarted, because nothing was missing.
- **D1.3:** A shared WebSearch budget ran out partway through the T1 (after 17 queries), T4 (22), T5 (25), T6 (35) and T7 (20) sweeps. Those agents continued via arXiv monthly listings, the OpenReview API and reference snowballing. Consequence: proceedings and journal coverage is incomplete, so crowding counts are lower bounds (stated in lit_review.md).
- **D1.4:** Duplicate keys for the same paper across themes: krstev2025temporal = anonymous2025temporal; huang2026crossstock = huang2026cross; sun2025intercorprelllm = sun2025intercorprel; zhang2026llmenhanced = zhang2026llm; li2025finkario = li2026finkario; harit2025from = harit2025news. Canonical keys are the first in each pair. references.bib keeps one entry per title (merge script dedups by normalised title).
- **D1.5:** The lit review keeps the full per-paper template entries from the sweeps verbatim. The lead agent wrote the per-theme syntheses, crowding map and comparison table (Phase 2).
- **D1.6 (agent-reported oddities, to keep in mind):** the arXiv 2504.20058v2 HTML contained instruction-like text (ignored by the T4 agent). TAGN (MDPI 2026) claims public data but uses FactSet. Several papers have internal inconsistencies (Kang 2026, CSHT, Fanshawe, Li & Fan v1); see the sweep files.

## 3. Raw ideas, kill tests and rejected ideas

## 4. Shortlist decisions

## 5. Independent check — corrections log

## 6. Deepening rounds

## 7. Unverified leads

- LAS-GNN as ICAIF 2025 paper (only thesis found)
- HERDGNN (CIKM 2026)

## 8. Open questions for the supervisor
- Does UU give MSc AI students WRDS access (CRSP / Compustat / Compustat customer segments)?
