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
- **D3.1 (2 Nov, 09:40):** 30 raw ideas written to `sweep/ideas_raw.md`, derived from §D gaps of T1–T8. 20 of 30 are cross-theme or unconventional (target ≥ 1/3).
- **D3.2:** Kill tests were delegated to 4 parallel agents (K1 signed/causal; K2 link prediction/KG; K3 credit/events; K4 LLM graphs/audit/anomaly) under `sweep/KILL_RULES.md`: ≥ 3 new-wording searches per idea, open every potential killer, verdict SURVIVED/WEAKENED/KILLED. About 160 queries in total; logs and BibTeX are in `sweep/kill_K*.md`.
- **D3.3 Outcome:** 0 killed outright, 13 survived, 16 weakened, 1 dropped.
  - Survived: I1, I4, I7, I8, I9, I11, I12, I16, I19, I20, I23, I24, I26, I29.
  - Weakened: I2, I3, I5, I6, I10, I13, I14, I15, I17, I18, I21, I25, I27, I28, I30.
  - Full table at the end of topic_shortlist.md.
- **Rejected ideas and why:**
  - **I22** (insider/shadow trading): dropped. Form 4 does not report trades in *linked* firms (agent's understanding, not source-checked), labels are tiny, and nodes are named natural persons (GDPR). Close to killed by stenfors2025shadow / jaeger2025needles.
  - **I13** (regime signed causal graphs on sectors): incremental after Lee 2026 (arXiv 2601.10732).
  - **I15** (KG priors for causal discovery): prior art exists (GregNets 2021, Zoroddu TMLR 2024); remaining delta small.
  - **I18** (US→Europe lead-lag): no free point-in-time STOXX 600 membership or European intraday data.
  - **I27** (credit–equity Hawkes): zelvyte2026multiplex is close, and public credit marks are sparse.
  - **I28** (decoupling/rewiring): 10-Ks rarely name Chinese counterparties; supply-link forecasting exists (Xiang 2025, Chang AAAI 2025).
  - **I30** (distress-text diffusion): graph + text distress prediction exists (China); a US version would be incremental.
- **D3.4 Paper that changed the picture:** **FinRipple** (xu2025finripple, Findings of ACL 2025, arXiv 2505.23826, [FT]): signed ripple prediction for KG-connected firms after news. Cards built on I4/I29 must position against it (differences: earnings/credit events, public graph, edge-level chronological evaluation).
- **D3.5 Data facts learned in kill tests:**
  - LoPucki BRD now at Univ. of Florida; free cases table ends Dec 2022.
  - EDGAR full-text-search JSON was blocked by the proxy (no live test).
  - edgartools exposes 8-K item lists.
  - No public 2025–26 dataset of 10-K major-customer links was found (Compustat segments is commercial).
  - FinReflectKG is the nearest public source of company relations.
  - The OpenReview API was blocked (403) for K3 this morning, but worked for K1/K2.

## 4. Shortlist decisions
- **D4.1:** Scored survivors and merged clusters with the §9 rubric (max 70). Merges: I7(+I24) → Card 1; I9+I8 → Card 2; I1(+I2, +I25 protocol) → Card 3; I29+I4(+I26) → Card 4; I10+I11+I12 → Card 5; I20+I19 → Card 6.
- **D4.2:** Wrote **six** full cards instead of five, because two directions the supervisor named (anomaly detection, credit default) would otherwise only appear as runners-up. Runners-up R1–R5: I3+I21, I5+I6, I24, I17, I23.
- **D4.3:** The ranking is by score, but the top four are within 6 points, which is inside judgement noise. The morning summary says so and flags Card 3 as the most direct hit on the supervisor's capitalised point.
- **D4.4:** Every card adopts the shared protocol: chronological walk-forward, persistence/EMA baseline, repeated-vs-new edge split, point-in-time universe where possible, seeds and confidence intervals.
- **D4.5:** The kill-test BibTeX (83 new entries) was merged into references.bib (227 unique keys in total).

## 5. Independent check — corrections log
- **Set-up (2 Nov, 09:56):** two fresh agents that wrote no content. Check A covered 25 random keys (seed 20261102, list in `sweep/check_list.md`) plus the closest work of Cards 1–2. Check B covered the closest work of Cards 3–6 plus one new-wording kill search per Card 1–5. Reports: `sweep/check_A.md`, `sweep/check_B.md`.
- **Results:** 61 citations checked (40 + 21). **Every link opened the right paper; no first author or first-public year was wrong.** 33 fully OK, 18 minor issues, 11 real errors. **No fresh kill search changed a verdict.**
- **Real errors fixed:**
  1. krstev2025temporal: authors were missing (stored as `anonymous2025temporal`). They are Krstev, Rigoni, Mishkovski, Pasa. Card 3 wrongly said the paper has "no sign": it includes ± lead-lag relations but predicts link existence. Card 3 also overstated what the reviewers asked for. Card 3 and the morning summary are reworded; the novelty is now edge *state* (+/−/inactive), statistical validation, birth/flip scoring and persistence baselines.
  2. FinRipple (Card 4): it already outputs pos/neg/neutral per connected firm and has a chronological test. Card 4's difference is restated as: realised significant peer reactions as per-edge labels, earnings/credit events, public graph, credit transfer.
  3. ren2026effect is a journal article (JIFMIM 106, 2026); bib and Card 6 updated.
  4. alridhawi2026stock: the "GICS check" claim was wrong. Its learned weights are initialised from sector priors (circular), and there are only sector averages.
  5. benhenda2026look and sun2025intercorprel(llm): keys did not resolve; unified.
  6. gao2025detecting: a fictitious "v1 title" was removed.
  7. A conflict-of-interest claim about benhenda2026look's author is now marked [UNVERIFIED].
  8. izadifar2026institutional: the "current constituents" and "no edge birth" claims were too strong; softened.
  9. pei2025gaining: split description corrected (the final test set is time-based).
  10. lee2026source / lee2026regimedependent: same paper; unified (v2 retitled Sep 2026).
  11. kong2026evaluating: peer-review status made consistent.
- **Minor fixes applied:** zhang2026all title ("…and Mitigation…") and note; sadeghi2023causal dates; shukla2026timedependent and bettencourt2026stanford URLs; DOIs for xu2025finripple, krzeczewska2022does and naik2026finkgr; yilki2026supply support qualified (48-node graph); HalluBench (arXiv 2603.20252) added as a FinReflectKG follow-up in Card 1; huang2026crossstock v1 date; kose2026reconstructing numbers; zhou2025zero tag [FT].
- **Not fixed (known, low impact):**
  - lit_review.md still has duplicate entries for the same paper under different themes (e.g. li2025cmag, sun2025intercorprel, huang2026crossstock), sometimes with different [FT]/[ABS] tags. The higher tag (from the agent that read the full text) applies.
  - The raw `sweep/T*.md` files are provenance and keep their original (uncorrected) text; `lit_review.md`, `topic_shortlist.md` and `references.bib` are the corrected versions.
- **New leads from the checkers** (opened by the checkers, not re-read by the lead agent → treat as [UNVERIFIED] until read): Chiu et al., arXiv 2509.18775 (10-K risk-relation extraction vs same-year return correlation; related to Card 1); *Can Prompts Rewind Time for LLMs?*, arXiv 2510.02340 (related to Card 2); HalluBench, arXiv 2603.20252.

## 6. Deepening rounds
- **D6.1 (2 Nov, 10:02): Phase 6 skipped at the user's request.** Before that instruction arrived, one quick code-availability check had already been run with the GitHub search API (metadata only, nothing executed). Repositories exist for:
  - shenyangHuang/TGB; yule-BUAA/DyGLib; liu-yushan/TLogic; Lee-zix/RE-GCN; Liyyy2122/TiRGN;
  - SherylHYX/pytorch_geometric_signed_directed (signed/directed GNNs, pushed Sep 2026); M1ngX/DeltaLag; SJTU-DMTai/MASTER; PeilinTime/MaGNet;
  - e-izdfr/portfolio-holdings-prediction (13F benchmark); dgunning/edgartools (EDGAR incl. 8-K items, 13F); jakobrunge/tigramite (PCMCI); pygod-team/pygod;
  - riazarbi/sp500-scraper (S&P 500 constituent history, pushed Oct 2026);
  - FinReflectKG community repos (multi-hop QA set; ArangoDB ports).

## 7. Unverified leads

- LAS-GNN as ICAIF 2025 paper (only thesis found)
- HERDGNN (CIKM 2026)

## 8. Open questions for the supervisor
1. Does UU give MSc AI students WRDS access (CRSP / Compustat / Compustat customer segments)? This decides survivor-free, point-in-time US universes for Cards 3–6.
2. Does she prefer a method contribution or an evaluation/benchmark contribution?
3. Which of her signals matters most: signed time-activated edges (Card 3), KG link prediction (Card 1) or credit default (Card 4)?
4. Is an LLM-extracted KG (FinReflectKG, CC-BY-NC) acceptable as data and partial ground truth? What GPU/LLM budget is available?
5. Scope: US only, or with a European/Euronext angle?

## 9. Checks for Frank himself
- Google Scholar "cited by" for: arun2025finreflectkg (2508.17906), krstev2025temporal (OpenReview KsWRLyIAKP), xu2025finripple (2505.23826), yang2026llm (2607.15640), izadifar2026institutional (2607.12067), guo2026nystrom (2609.08106).
- The SJSU project FinKGR (naik2026finkgr, embargoed until 2027): does it use FinReflectKG? Ask the author or SJSU if Card 1 is chosen.
- FinReflectKG licence (CC-BY-NC) vs thesis and internship use.
- WRDS access at UU; UU AI & Finance Lab for overlapping student projects.

## 11. Deep dive on Idea 3 (2 Nov 2026, afternoon)
- **D11.1:** Frank asked for a deep dive on Card 3 (signed lead-lag edge-state forecasting) before pitching it. Output: `deep_dive_idea3.md`.
- **D11.2 (status change):** krstev2025temporal is no longer a withdrawn ICLR submission. It was published in *Machine Learning* 115(7):173 (online 8 Jul 2026, open access) with code and data on GitHub. The core differences from Card 3 hold (37 assets; 5% threshold edges with lag 1 and no significance test; sign not a separate class; no persistence baseline). `references.bib` updated.
- **D11.3 (data):** UU's WRDS list (uufinance.github.io/data/wrds/databases/) shows Compustat, Fama–French and S&P Dow Jones Indices but **no CRSP**. The question for the supervisor becomes: delisted prices via LSEG Workspace / FactSet (financedata@uu.nl)? The free point-in-time S&P 500 list is at github.com/fja05680/sp500 (updated Sep 2026).
- **D11.4 (risk):** evidence that daily lead-lag between large caps is weak (Curme et al. 2015 [BG]; practitioner GitHub analysis 2026, not peer-reviewed). Weekly returns and same-day signed residual-correlation edges are planned fallbacks.
- **Queries (all WebSearch):**
  - lead-lag network stock returns 2026 arXiv
  - dynamic signed network link prediction 2026
  - statistically validated lead-lag networks stocks
  - lead-lag relationship persistence stability over time equities 2025
  - Cucuringu lead-lag 2025 2026
  - signed GNN stock positive negative correlation 2026
  - temporal graph benchmark persistence baseline EdgeBank 2025
  - Jialin Yu computer vision firm networks lead-lag
  - Cartea Cucuringu Jin detecting lead-lag
  - "lead-lag" stocks time-varying regime network 2026 (extended)
  - network momentum lead-lag graph learning 2025
  - structural balance signed stock correlation network 2025 2026
  - predict future stock correlation network edges link prediction 2026 (extended)
  - Granger causality network S&P 500 FDR 2025
  - benchmark dynamic signed networks datasets 2025
  - UU WRDS CRSP
  - S&P 500 historical constituents GitHub
  - Stooq bulk / delisted
  - lead-lag deep learning 2026 q-fin
  - TGM / DyGLib / PyG signed
- **Unverified leads:** a Jialin Yu (HKUST) computer-vision firm-network lead-lag paper (HKUST article links DOI 10.1016/j.jfineco.2023.103716, which may be Yan & Yu 2023 "Cross-stock momentum and factor momentum", JFE 150). Not opened; not cited in the deep dive.
