# Topic shortlist — stock-market and company-network thesis

*Overnight topic-scouting run, 1–2 Nov 2026. Supervisor: Dr. I. Karnstedt-Hulpuș (UU). Sources: `lit_review.md` (158 opened entries, 52% from 2026), kill tests in `sweep/kill_K1–K4.md`, decisions in `NOTES.md`.*
*Legend: [FT] full text read · [ABS] abstract/record only · [UNVERIFIED] not opened. All numbers quoted from papers must be re-checked against the PDF before thesis use.*

---

## Morning summary

**What was found.** The landscape splits cleanly. **Crowded:** new GNN/transformer/hypergraph architectures for stock return prediction (an estimated 150–300 papers in 2025–26 on ageing benchmarks), "LLM builds a financial KG → trend prediction" pipelines, and GNN default scoring on private Chinese/Korean enterprise graphs. **Thin or empty, where your supervisor's signals point:**
- signed, directed, time-activated lead-lag edges *as prediction targets* (one withdrawn ICLR 2026 attempt on 37 assets);
- link prediction on *public* company graphs with honest temporal evaluation; none of the TGB benchmark suites has a company dataset;
- temporal KG completion on public company KGs (the free FinReflectKG, with 17.5M time-stamped 10-K triples, has never been used for it);
- credit or earnings events → *which connected stocks react, and with which sign* (public-data version absent; FinRipple, ACL Findings 2025, is the closest and uses commercial data and generic news);
- auditing LLM-built and model-learned stock graphs: construction sensitivity, look-ahead leakage, and whether they match real economic links.

A recurring lesson: on financial graphs, **persistence/EMA baselines nearly match the best temporal GNNs** (13F benchmark, July 2026). Every topic below therefore makes "beat persistence honestly, on chronological splits" part of the design.

All 30 raw ideas survived or were weakened. None was killed outright, but one (insider/shadow trading) was dropped for data and GDPR reasons.

**The six best topics** (scores out of 70, rubric in briefing §9; the top four are within noise of each other):

1. **Next year's company relations: temporal KG completion on a public 10-K knowledge graph (60).** Predict which company–company relations (supplies, partners, competes, holds a stake) appear next year in FinReflectKG, then test whether predicted *new* edges carry co-movement or return information. Strongest supervisor fit (knowledge networks, link prediction, heterogeneous/temporal), empty niche, existing TKG libraries. Main risk: the "ground truth" is itself LLM-extracted.
2. **Does the LLM know the future? Look-ahead contamination and construction robustness of LLM-built company graphs (59).** Measure, edge by edge, how often LLMs "know" company relations that only started after a given date, and how much the graph and downstream results change across construction choices (model, prompt, anonymisation, thresholds). Best fit with your LLM-validation and AI Act background and very relevant to Accenture. Weaker on the supervisor's explicit graph signals.
3. **When do influence edges switch on, off or flip? Signed lead-lag edge-state forecasting (58).** The most direct hit on the supervisor's capitalised point. Build statistically validated, signed, directed lead-lag graphs on a point-in-time universe and predict each pair's next state (+ / − / inactive) as temporal signed link prediction, against persistence and structural-balance baselines. Main risks: weak signal, and point-in-time constituents/delisted prices without CRSP.
4. **Which peers move when a firm surprises or defaults? Event-conditioned signed edge activation (54).** Use thousands of earnings announcements per quarter as frequent public shocks, learn which connected firms react and with which sign, then transfer to rare credit events (bankruptcies, downgrades). Hits the supervisor's credit-default problem, signed edges and link prediction together. Main risk: rare credit events and FinRipple as close prior work.
5. **Do learned and discovered stock graphs reflect real economic links? (54).** Score the graphs that stock GNNs learn, and the edges that causal-discovery methods find, against independent public relations (10-K links, 13F co-ownership, earnings-event information transfer), and test whether learned edges *predict future* real relations. An evaluation-rigour thesis that the literature lacks; cheap compute. Risk: negative results (which are still publishable).
6. **Unexplained co-movement: anomaly detection relative to a company knowledge graph (52).** An edge is anomalous when stocks co-move strongly but neither factor exposures nor any known relation explains it. Detect such edges, then check them against event-typed weak labels (SEC litigation, trading halts, index changes). Hits anomaly detection and KG; strong analyst artefact. Risk: weak labels.

**Five questions to bring to the supervisor.**
1. Do UU MSc students get **WRDS (CRSP/Compustat) access**? It decides whether topics 3–6 can use survivor-free, point-in-time US universes, or must fall back on free data (yfinance/Stooq + Wikipedia constituent history) or Chinese Qlib data.
2. Does she prefer a **method contribution** (a new model) or an **evaluation/benchmark contribution** (a protocol, dataset or audit)? Topics 1–3 can lean either way; 2 and 5 are evaluation-first.
3. Of her signals, which matters most to her: **signed time-activated edges (topic 3)**, **link prediction on knowledge graphs (topic 1)**, or **credit default (topic 4)**? Several topics can absorb another as a sub-question.
4. Is an **LLM-extracted KG (FinReflectKG, CC-BY-NC)** acceptable as data and partial ground truth? Is there a **GPU/LLM budget** (UU cluster, open-weight models)?
5. **Scope:** US only (best data), or with a European / Euronext angle (Europe is nearly absent from the literature, but free point-in-time data is poor)?

**What to check yourself.** Google Scholar "cited by" for the closest papers of your favourite topic. In particular, check:
- FinReflectKG (arXiv 2508.17906)
- the withdrawn lead-lag submission krstev2025temporal (OpenReview forum KsWRLyIAKP)
- FinRipple (2505.23826)
- yang2026llm (2607.15640)
- izadifar2026institutional (2607.12067)
- guo2026nystrom (2609.08106)

Also check the following:
- Whether the SJSU MSc project **FinKGR** (embargoed until 2027) uses FinReflectKG.
- WRDS access at UU.
- Whether FinReflectKG's licence allows thesis use.
- The UU AI & Finance Lab for any current student project on these topics.

---

## Scoring overview

Rubric (briefing §9): Novelty ×3, Supervisor fit ×2, Data ×2, Feasibility ×2, Evaluation clarity ×1, AI relevance ×1, Artefact ×1, Industry/Accenture ×1, Fit with Frank ×1. Max 70. All six pass the gates: public data; not plain classification or return prediction; concrete 2025–26 closest work; feasible in 30 weeks.

| # | Topic | Nov×3 | Fit×2 | Data×2 | Feas×2 | Eval | AI | Artef. | Ind. | Frank | **Total** | Directions hit |
| --- | --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | --- |
| 1 | Temporal KG completion on 10-K company KG | 5 | 5 | 4 | 4 | 4 | 5 | 3 | 4 | 3 | **60** | link pred., heterogeneous, temporal, text/KG |
| 2 | Look-ahead & construction robustness of LLM company graphs | 5 | 3 | 4 | 4 | 4 | 5 | 3 | 5 | 5 | **59** | text/KG, evaluation rigour, (link pred. downstream) |
| 3 | Signed lead-lag edge-state forecasting | 4 | 5 | 4 | 4 | 5 | 4 | 4 | 3 | 4 | **58** | **signed + time-activated**, link pred., temporal, TS+graph |
| 4 | Event-conditioned signed edge activation (earnings → credit) | 4 | 4 | 3 | 3 | 4 | 4 | 4 | 5 | 5 | **54** | **credit default**, signed, link pred., heterogeneous |
| 5 | Audit of learned / discovered stock graphs vs real links | 4 | 4 | 4 | 4 | 4 | 4 | 3 | 3 | 4 | **54** | TS+graph, causal, link pred. |
| 6 | Unexplained co-movement vs company KG (anomalies) | 4 | 4 | 3 | 3 | 3 | 4 | 5 | 4 | 4 | **52** | **anomaly detection**, KG, signed |

Scores are judgements. A difference of ≤ 3 points is not meaningful. Choose by interest and by the supervisor's answer to question 3.

---

## Card 1 — Next year's company relations: temporal KG completion on a public 10-K knowledge graph

**1. Pitch.** Predict which company–company relations (supplies, partners with, competes with, holds a stake in) will appear in next year's 10-K filings, using a free, time-stamped knowledge graph of S&P 500 firms. Then ask whether the predicted *new* edges already carry market information.

**2. Research question.** *To what extent can future inter-company relations disclosed in 10-K filings be predicted from the past temporal company knowledge graph, and do predicted new relations anticipate co-movement between the firms involved?*
- RQ variant A (method): Do temporal KG reasoning models (rule-based, recurrent GNN, LLM) beat recurrence/persistence baselines on year-ahead company-relation completion under strict chronological splits?
- RQ variant B (economics of edges): Do firm pairs with a high predicted probability of a *new* relation show higher return co-movement or lead-lag strength before the relation is disclosed?
- RQ variant C (cold start): How well can relations of firms entering the KG (IPOs, index additions) be predicted inductively from their first filing's text?
- Sub-questions: Which relation types are most predictable? How fast do mined rules decay year to year? How sensitive are results to entity resolution?

**3. Supervisor directions.** Link prediction (core) · heterogeneous graph (typed relations, firm/product/sector nodes) · temporal graph · text and knowledge networks (her expertise) · optional time series + graph in RQ B.

**4. Closest 2025–2026 work and the difference.**
- **arun2025finreflectkg**: *FinReflectKG*, ICAIF 2025 (arXiv 2508.17906) [FT; dataset card opened]. Builds the KG: 743 S&P 500 firms, 10-Ks 2014–2024, about 17.5M triples with start/end dates, CC-BY-NC. Used only for QA (FinReflectKG-MultiHop) and extraction evaluation (EvalBench). *Difference:* we use it for **temporal link prediction** and an **economic test of predicted edges**, which no paper has done.
- **sun2025intercorprelllm**: *InterCorpRel-LLM* (arXiv 2510.09735) [FT/ABS]. Supplier-link prediction with GNN + LLM, but on proprietary FactSet, a single year and a random split. *Difference:* public data, year-ahead temporal split, several relation types.
- **dukkipati2025predictive** (arXiv 2504.20058, v2 2026) [ABS]. Stock-market TKGs whose relations are modelled as Hawkes events *to rank returns*; KG completion is not the task.
- **naik2026finkgr** (*FinKGR*, SJSU MSc project, Jul 2026) [ABS; PDF embargoed to 2027]. Temporal link prediction on an unnamed financial TKG; results not verifiable. **Must re-check before go/no-go.**
- **li2024findkg** [BACKGROUND] (*FinDKG*, 2024). The only earlier financial TKG link-prediction study, built from news.
- Supporting: **yilki2026supply** (arXiv 2606.29290) [ABS] shows 10-K signals propagated over supply links predict returns (supports RQ B); **huang2026crossstock** (arXiv 2604.19476) [FT] shows LLM-typed 10-K edges carry trading information.

**5. Kill test.** Searches (kill_K2, I7): "FinReflectKG link prediction knowledge graph completion"; "temporal knowledge graph forecasting company relations 10-K filings 2026"; "FinReflectKG temporal reasoning benchmark 2026 arXiv"; "supply chain link prediction public firms SEC filings knowledge graph embedding 2025"; OpenReview API "financial temporal knowledge graph completion". Outcome: **SURVIVED**. No use of FinReflectKG, or of any public time-stamped 10-K company KG, for link prediction was found. The only near-hit (FinKGR) is unverifiable.

**6. Data and access risk.**
- FinReflectKG (Hugging Face `domyn/FinReflectKG`, CC-BY-NC). **Low risk**, but confirm that the non-commercial licence suits thesis use and the Accenture context (research only).
- Prices for RQ B: yfinance/Stooq (free; delisted firms missing) or CRSP via WRDS (**ask**). The universe is S&P 500, so survivorship matters less if membership history is taken from Wikipedia's S&P 500 change list or a public GitHub mirror of it [UNVERIFIED].
- Optional validation: Hoberg–Phillips TNIC/VTNIC (free, but gvkey-keyed; needs a CIK link: **ask about WRDS**).
- Caveat: FinReflectKG relations are LLM-extracted, not gold. Plan a 100–200-edge manual check.

**7. Method sketch and existing code.**
1. Extract the company–company subgraph per year and fix entity resolution (CIK/ticker).
2. Baselines: recurrence/EdgeBank-style and frequency heuristics; TLogic-style temporal rules; RE-GCN / TiRGN-type recurrent TKG models (public PyTorch repos; TGB 2.0 tooling for heterogeneous temporal graphs, gastinger2024tgb [BACKGROUND]); a static R-GCN on PyTorch Geometric; an LLM baseline restricted to post-cutoff test years.
3. RQ B: event-study-style comparison of co-movement and lead-lag for predicted-new vs matched non-predicted pairs, before disclosure.
Mostly library glue plus one clean evaluation harness, which suits GenAI-assisted coding.

**8. Baselines and evaluation.** Train on years ≤ t, test on t+1 with walk-forward over 2019–2024. Time-aware filtered MRR/Hits@k, scored separately on **repeated vs new** edges (the new-edge score is the headline metric). Degree-corrected negatives. Report variance over seeds. For RQ B: pre-disclosure windows only, no look-ahead (use filing dates, not fiscal-year ends).

**9. Usable artefact.** A "relation radar" notebook/dashboard that shows, for a chosen firm, its current KG neighbourhood, predicted new relations with rule-path explanations, and their co-movement history.

**10. 30-week feasibility, risks, fallback.**
- Weeks 1–8: literature review + proposal; data access check; go/no-go.
- Weeks 9–12: KG slicing, entity resolution, manual check.
- Weeks 13–20: baselines + TKG models.
- Weeks 21–24: RQ B economic test.
- Weeks 25–27: artefact.
- Weeks 28–30: writing.

Main risks: LLM-extraction noise makes "new" edges partly extraction artefacts; FinKGR turns out to overlap. Fallback: restrict to high-precision relation types (Has_Stake_In, Supplies) and turn the noise issue into a sub-result (how stable are extracted relations year to year?).

**11. Ethics.** Company-level nodes only. FinReflectKG may include person entities (executives): drop them. Respect CC-BY-NC. Disclose GenAI use.

**12. Scores.** Novelty 5, Fit 5, Data 4, Feasibility 4, Evaluation 4, AI relevance 5, Artefact 3, Industry 4, Frank 3 → **60/70**.

**13. Questions for the supervisor.** Is an LLM-extracted KG acceptable as (noisy) ground truth? Should the thesis lean towards KG reasoning (rules, explainable paths) or towards the economic test? Any Crunchbase-era code or students to reuse?

---

## Card 2 — Does the LLM know the future? Look-ahead contamination and construction robustness of LLM-built company graphs

**1. Pitch.** LLM-built company graphs are the fastest-growing input to stock models, but nobody has checked edge by edge whether the LLM leaks future knowledge, or how much the graph changes with harmless-looking construction choices. This thesis builds the diagnostic.

**2. Research question.** *How much future information leaks into LLM-generated company-relation graphs, and how robust are graph structure and downstream conclusions to construction choices?*
- RQ variant A (leakage-first): What share of relations an LLM asserts "as of date t" provably began after t, per model and training cutoff, and how much downstream "graph alpha" disappears when only point-in-time, filing-grounded edges are used?
- RQ variant B (multiverse-first): Across a grid of construction choices (LLM, prompt, anonymisation, ontology, entity-resolution threshold, top-K), how much do the resulting graphs differ, and how wide is the spread of a fixed downstream result?
- RQ variant C (link prediction): Can an LLM's parametric relation predictions be separated into genuine inference and memorisation, using relations first disclosed after its cutoff as a clean test set?

**3. Supervisor directions.** Text analysis and knowledge networks (her expertise) · link prediction (variant C) · temporal (point-in-time graphs). Less direct on signed/anomaly/credit.

**4. Closest 2025–2026 work and the difference.**
- **yang2026llm**: *LLM Latent Edge Measurement: Point-in-Time Economic Graphs…* (arXiv 2607.15640) [FT]. Separates a filing channel from a parametric "knowledge" channel and warns that the latter causes look-ahead bias, but **measures no contamination**, uses one LLM and has no return test (the K4 agent estimates it covers about 20% of this idea).
- **zhang2026all**: *All Leaks Count, Some Count More* (arXiv 2602.17234) [FT/ABS]. Claim-level temporal-leakage detection in LLM *forecast rationales*, not in relations or graphs.
- **gao2025detecting** (LAP test, arXiv 2512.23847) and **benhenda2026look** (*Look-Ahead-Bench*, arXiv 2601.13770) [ABS]. Look-ahead in LLM *forecasts*, not edges.
- **zhou2025zero** (*Zero-shot extraction of stock relationship graphs*, FinNLP 2025) [ABS], **huang2026crossstock** [FT]. Each uses one LLM and one prompt; neither tests leakage or construction variance.
- Method sources outside finance: **baumann2025large** (*LLM Hacking*, arXiv 2509.08825) and **crainic2026prompt** (*specification curve for LLM-based social science*, ICML 2026) [ABS]. The multiverse method exists for annotation, not for company graphs.
- **kong2026evaluating** (arXiv 2602.14233) [FT]. Only 26.8% of 164 finance-LLM papers address look-ahead; graphs are not covered.

**5. Kill test.** Searches (kill_K4, I8/I9) covered look-ahead/memorisation tests of LLM-extracted relations or KGs, "specification curve"/"multiverse" for LLM KG construction in any domain, and point-in-time LLM graphs. Outcome: **SURVIVED** (I9 strengthened). yang2026llm frames the problem but does not measure it; the multiverse method exists only outside finance.

**6. Data and access risk.**
- SEC EDGAR 10-K text: public; filing dates provide edge start dates.
- FinReflectKG as a ready-made point-in-time reference graph (CC-BY-NC).
- Open-weight LLMs with documented cutoffs (Qwen, Llama families) via Hugging Face. **GPU access or a small API budget is needed (ask).**
- Prices for the downstream test: yfinance/Stooq or CRSP.

Risk: **low to medium**. Cost is mostly LLM inference.

**7. Method sketch.**
1. Build a ground-truth set of relations with verifiable start dates (first 10-K mention; FinReflectKG start dates).
2. Query several LLMs "as of t" (named vs anonymised firms) and estimate a contamination rate per model and cutoff, with a pre/post-cutoff difference-in-differences.
3. Run a grid of 20–50 construction variants and measure edge Jaccard, spectral distance, community NMI and centrality-rank stability.
4. Downstream: a fixed simple model (network momentum or an R-GCN ranker), comparing the spread of results across variants and point-in-time vs parametric graphs.

Code: Hugging Face transformers/vLLM, NetworkX, PyG.

**8. Baselines and evaluation.** Contamination: post-cutoff relations as a clean control; random-pair and shuffled-date placebos. Robustness: specification-curve plots with confidence bands. Downstream: chronological walk-forward, net-of-cost portfolio metrics. Validate extraction against a 150–300-edge human gold set.

**9. Usable artefact.** A reusable "graph leakage checker" (Python package + report) that scores any LLM-built company graph for look-ahead risk and construction sensitivity. This is a governance tool that fits AI Act / model-risk practice.

**10. 30-week feasibility, risks, fallback.**
- Weeks 1–8: review + proposal.
- Weeks 9–13: ground-truth set + gold annotation.
- Weeks 14–20: contamination experiments.
- Weeks 21–24: multiverse grid.
- Weeks 25–27: downstream test + tool.
- Weeks 28–30: writing.

Risks: LLM inference budget; contamination may be small (still a clean, publishable finding). Fallback: drop the downstream test and focus on contamination plus graph divergence.

**11. Ethics.** Company-level only; no personal data. Respect LLM licences. Disclose GenAI use (the topic is about GenAI, so be explicit about which models did what).

**12. Scores.** Novelty 5, Fit 3, Data 4, Feasibility 4, Evaluation 4, AI relevance 5, Artefact 3, Industry 5, Frank 5 → **59/70**.

**13. Questions for the supervisor.** Is a mostly evaluation/diagnostic thesis acceptable? Is the link-prediction variant (C) needed to keep it within her network-analysis scope? Which LLM resources are available at UU?

---

## Card 3 — When do influence edges switch on, off or flip? Signed lead-lag edge-state forecasting

**1. Pitch.** Turn the supervisor's "signed, time-activated influence edges between stocks" into a precise prediction target. For every stock pair and window: is there a validated lead-lag edge, is it positive or negative, and will it switch on, off or flip next window?

**2. Research question.** *Can the future activation state (positive, negative, inactive) of statistically validated lead-lag edges between stocks be predicted beyond persistence, and which structural signals (signed balance, lag consistency, sector and text relations) carry that predictability?*
- RQ variant A (forecasting): Do dynamic signed GNNs and temporal GNNs beat persistence / activation-frequency / triad-balance baselines on next-window edge state, especially on *births and sign flips*?
- RQ variant B (structure): Do directed lead-lag triads obey sign multiplicativity **and lag additivity**, and does cycle inconsistency identify spurious edges that do not persist? (weakened idea I2, used as a module)
- RQ variant C (economics): Do correctly forecast edges improve out-of-sample hedging or minimum-variance risk compared with persistence?

**3. Supervisor directions.** **Signed, time-activated edges + time series (her capitalised point)** · link prediction · temporal graphs · optional anomaly detection (sign-flip bursts; runner-up R1).

**4. Closest 2025–2026 work and the difference.**
- **krstev2025temporal**: *A Temporal Graph Learning Framework for Lead-Lag Detection in Financial Markets*, ICLR 2026 submission, **withdrawn** (OpenReview forum KsWRLyIAKP) [ABS + reviews via API]. Lead-lag as temporal link prediction, but 37 assets, fixed one-day lag, ad-hoc 5% threshold, no sign and no statistical validation; reviewers asked for exactly these fixes. *Difference:* signed, statistically validated edges on a realistic universe, with persistence baselines.
- **zhou2025deltalag**: *DeltaLag*, ICAIF 2025 (arXiv 2511.00390) [FT]. Learns per-pair dynamic lags for return prediction; edges are unsigned and never validated or evaluated as edges.
- **fanshawe2026forecasting** (arXiv 2601.04602) [FT]. Forecasts *signed* pairwise correlations, but undirected and contemporaneous; motivates sign flips without evaluating them.
- **hou2026inductive** (IDP-DSN, arXiv 2604.18213) and **regier2026dynamic** (HCIM, arXiv 2605.26290) [FT/ABS]. Dynamic signed link prediction (+/−/none) with code, tested only on Bitcoin-OTC/Alpha and Wikipedia; regier2026 calls for signed temporal benchmarks.
- **dahake2026sectoral** (arXiv 2608.12023) and **acedo2026triadic** (arXiv 2608.10788) [FT/ABS]. Structural balance on *undirected, same-day* correlation networks; descriptive or coincident.
- Background: **castilho2021forecasting** (arXiv 2110.11751) [FT, BACKGROUND]. Link prediction of future co-movement edges vs persistence, unsigned and undirected.

**5. Kill test.** 13 searches + 2 OpenReview queries (kill_K1, I1) covered lead-lag link prediction, signed temporal stock benchmarks, correlation-edge/sign forecasting, and crypto/FX/commodity analogues. Outcome: **SURVIVED**. No 2025–2026 work forecasts signed lead-lag edge states on a realistic equity universe, and no signed temporal stock-network benchmark exists. The NJIT "signed graph Laplacian" paper (J. Fin. Econometrics 2024) builds a pricing factor and does not forecast edges [ABS, BACKGROUND].

**6. Data and access risk.**
- Daily prices for S&P 500 (or S&P 1500) via yfinance/Stooq (free; delisted firms are the problem), or CRSP via WRDS (**ask**). Point-in-time constituents from public change histories [UNVERIFIED mirror]. Fama–French factors (free) for residualising.
- European extension (STOXX 600) is weaker: free point-in-time membership is unavailable.
- Risk: **medium**, because survivorship needs CRSP or an honest survivor-bias analysis.

**7. Method sketch and existing code.**
1. Rolling windows (e.g. 120 trading days, step 20). Factor-residualised lagged cross-correlation or Granger tests over lags 1–5 with FDR control produce edges i→j with sign and lag, as in statistically validated networks.
2. Labels: next-window state per pair.
3. Models: persistence; activation frequency; logistic/GBDT on pair features (sector, size, past states, triad balance, lag-cycle consistency); dynamic signed GNNs (IDP-DSN / HCIM code); TGN/DyGFormer via TGM/DyGLib (chmura2025tgm).

Python: statsmodels, PyG, TGM.

**8. Baselines and evaluation.** Strict chronological walk-forward (e.g. 2005–2018 train, 2019–2020 validation, 2021–2025 test), no full-sample normalisation. Metrics: macro-F1 / AUC per state, scored separately for **persistent edges, births, deaths and sign flips**; calibration. Report everything against persistence (the honest bar, cf. izadifar2026institutional). Economic check: hedging error / min-variance risk with forecast vs persistence graphs, net of costs.

**9. Usable artefact.** An "influence-edge monitor": an interactive signed, directed network of the market with edges coloured by sign, lag on hover, and next-window flip/activation probabilities; a filter for sector or stock.

**10. 30-week feasibility, risks, fallback.**
- Weeks 1–8: review + proposal.
- Weeks 9–13: data + edge-validation pipeline.
- Weeks 14–16: label statistics and baselines.
- Weeks 17–22: GNN models.
- Weeks 23–25: structure analysis (RQ B) + economic test.
- Weeks 26–27: dashboard.
- Weeks 28–30: writing.

Risks: lead-lag edges are sparse and noisy, so births/flips may be close to unpredictable. That is still a clear finding and is publishable as a benchmark (the signed temporal stock benchmark itself is a contribution). Fallback: use contemporaneous signed residual-correlation edges (denser) with the same pipeline.

**11. Ethics.** Company-level market data only; no personal data. Disclose GenAI use.

**12. Scores.** Novelty 4, Fit 5, Data 4, Feasibility 4, Evaluation 5, AI relevance 4, Artefact 4, Industry 3, Frank 4 → **58/70**.

**13. Questions for the supervisor.** Is "edge-state forecasting" what she meant by signed, activated edges, or did she mean a model that *learns* signed edges while forecasting returns (more crowded)? Is a benchmark + baseline study enough novelty if GNNs do not beat persistence? WRDS?

---

## Card 4 — Which peers move when a firm surprises or defaults? Event-conditioned signed edge activation

**1. Pitch.** When one firm reports an earnings surprise or a credit event, some connected firms' stocks move with it (contagion), some against it (competitors gain) and most not at all. Learn to predict which edges activate and with which sign, using frequent earnings events, then transfer to rare credit events.

**2. Research question.** *Can we predict which connected listed firms show a significant abnormal stock reaction to a focal firm's event, and with which sign, from a public heterogeneous company graph, and does a model trained on earnings announcements transfer to credit events?*
- RQ variant A (earnings): For earnings announcements, which relation types (customer/supplier, competitor, common owner, sector) predict a peer's signed reaction, and does a heterogeneous temporal GNN beat sector-peer and economic-link baselines?
- RQ variant B (credit transfer): Does the earnings-trained model rank the stocks that react to bankruptcies and downgrades better than a credit-only event study?
- RQ variant C (attribution): Which relation type carries credit shocks into equity (relation-level attribution; idea I26)?

**3. Supervisor directions.** **Credit default → stock market** · signed, time-activated edges (activation conditioned on events) · link prediction · heterogeneous graphs.

**4. Closest 2025–2026 work and the difference.**
- **xu2025finripple**: *FinRipple*, Findings of ACL 2025 (arXiv 2505.23826) [FT]. Predicts a signed "ripple" score for KG-connected S&P 500 firms after news, aligned to residual returns. *Difference:* earnings and credit events instead of generic news; a **public** graph instead of Compustat/BoardEx/Thomson; edge-level classification with chronological splits; a credit-transfer question.
- **brogaard2026network** (working paper, 2026) [FT]. A production-network theory plus GNN for **CDS spreads**, with industry-level edges and commercial data.
- **zelvyte2026multiplex** (arXiv 2606.15755) [FT/ABS]. A multiplex Hawkes model of contagion among 99 firms' credit spreads; CDS only, no equity layer.
- **kim2026defaultgnn** (*DefaultGNN*, CIKM 2026) [FT/ABS]. Default prediction on private Korean buyer–seller networks; unlisted firms, no equity reactions.
- Finance background: Hertzel et al. 2008 (JFE), Alldredge et al. 2022 (RAF), Frankel et al. 2025 (JAR) [ABS]. In-sample average effects. Frankel 2025 finds *lower* peer co-movement on earnings days, a substantive risk for variant A.

**5. Kill test.** About 15 searches (kill_K3, I4 and I29) covered supply-chain bankruptcy contagion, rating-downgrade spillovers, intra-industry information transfer, ML peer-reaction prediction, and event-driven KG link prediction. Outcome: **SURVIVED** (I4 narrowly, I29 clearly). FinRipple is the paper to position against.

**6. Data and access risk.**
- Earnings dates from 8-K Item 2.02 filing timestamps (EDGAR, public).
- Surprise: either price-based (announcement-window abnormal return, no analyst data needed) or SUE from XBRL quarterly EPS (SEC Financial Statement Data Sets). I/B/E/S consensus is paid.
- Credit events:
  - UCLA-LoPucki BRD, now hosted by the University of Florida; free cases table, **ends Dec 2022**;
  - 8-K Item 1.03 via edgartools item lists;
  - 17g-7(b) rating histories (12-month delay).
- Graph: FinReflectKG relations, 13F co-ownership, SIC/TNIC.

Risk: **medium–high**. Credit events are rare (a few hundred), and building the event set takes time.

**7. Method sketch.**
1. Event study per (focal, peer) pair gives labels: significant positive, negative or none (CAR with factor model, FDR control).
2. Features and graph: relation types, path length, past co-movement, sizes.
3. Models: logistic/GBDT; R-GCN/HGT link classifier conditioned on the event embedding; temporal models via TGM.
4. Transfer by fine-tuning or zero-shot on credit events.

Python: PyG heterogeneous modules, statsmodels for CARs.

**8. Baselines and evaluation.** Sector peers; Cohen–Frazzini-style customer links; persistence of past reactions; FinRipple-style LLM scoring as an optional baseline. Chronological splits by event date. Macro-F1/AUC per sign, precision@k per event, calibration. Placebo: matched unlinked firms, to separate contagion from common shocks.

**9. Usable artefact.** A "contagion explorer": pick a firm and an event type and see predicted reacting peers with signs, paths and historical analogues. This is a natural risk-management demo for Accenture.

**10. 30-week feasibility, risks, fallback.**
- Weeks 1–8: review + proposal.
- Weeks 9–14: event sets + graph.
- Weeks 15–20: earnings models.
- Weeks 21–24: credit transfer + attribution.
- Weeks 25–27: explorer.
- Weeks 28–30: writing.

Risks: weak earnings-day spillover signal (Frankel 2025); too few credit events. Fallback: earnings-only thesis with credit as a qualitative case study; or downgrades only (more frequent than bankruptcies).

**11. Ethics.** Company-level only. Rating data under agency terms (attribution, no redistribution). Disclose GenAI use.

**12. Scores.** Novelty 4, Fit 4, Data 3, Feasibility 3, Evaluation 4, AI relevance 4, Artefact 4, Industry 5, Frank 5 → **54/70**.

**13. Questions for the supervisor.** Is the earnings-first, credit-transfer design acceptable for "credit default into stock markets"? Is a price-based surprise measure fine, or should we seek I/B/E/S via WRDS? How should the work be positioned against FinRipple?

---

## Card 5 — Do learned and discovered stock graphs reflect real economic links?

**1. Pitch.** Dozens of stock GNNs learn a graph, and many causal-discovery methods claim influence edges, but almost nobody checks these graphs against reality. Score them against independent public economic links and test whether they *anticipate* new real relations.

**2. Research question.** *Do the stock-to-stock graphs learned by graph-based stock models, and the edges found by time-series causal discovery, recover independently documented economic relations, and do they predict relations that emerge later?*
- RQ variant A (learned graphs): Do the learned adjacencies or attention of 3–4 open-source stock GNNs align with sector, 10-K, 13F and validated lead-lag relations, and does replacing them with random graphs of equal density hurt performance?
- RQ variant B (causal discovery): Which method (Granger, PCMCI+, transfer entropy, neural Granger) recovers documented directed customer→supplier links best (precision@k), and do earnings events behave like interventions along those edges?
- RQ variant C (forward-looking): Do learned or discovered edges at t predict relations that first appear at t+1…t+4 quarters (new 13F co-holding overlap, new 10-K mentions, M&A, index co-inclusion)?

**3. Supervisor directions.** Time series + graph (critically) · link prediction (variant C) · causal (variant B) · knowledge networks as ground truth.

**4. Closest 2025–2026 work and the difference.**
- **guo2026nystrom** (arXiv 2609.08106) [FT]. Audits **one** model (MASTER) on Chinese A-shares against industry and return similarity: attention is near-uniform and anti-correlated with return similarity. *Difference:* several models with explicit learned adjacency, a US universe, 10-K/13F/lead-lag relation layers, and a forward-looking test.
- **niu2025ngat** (arXiv 2507.02018) [FT] and **wang2025quantbench** (arXiv 2504.18600) [FT]. Graph value is fragile; graph quality is masked by model capacity. No relation-level audit.
- **zhou2025deltalag** [FT]. Learned lead-lag edges never validated.
- Kill-test finds: **sadeghi2023causal** (CD-NOTS, Bloomberg) [FT, BACKGROUND] notes there is no ground truth; **alridhawi2026stock** (arXiv 2603.05917) [FT] checks learned weights only qualitatively against GICS.

**5. Kill test.** Searches (kill_K4 I10; kill_K2 I11; kill_K1 I12): I10 **WEAKENED** by guo2026nystrom (single model and market); I11 and I12 **SURVIVED**. No work scores causal or learned stock edges against independent economic links, or tests them as predictors of future relations.

**6. Data and access risk.** Prices (yfinance/Stooq or CRSP); FinReflectKG relations; 13F data sets (2013–2026, public); SIC from the SEC data sets; M&A from 8-K Item 2.01 or public lists; S&P index changes. Open-source model code (MASTER, MaGNet; the repos are named in the papers, unchecked). Risk: **low to medium**.

**7. Method sketch.** Retrain 3–4 models on a common point-in-time universe; extract their graphs over time; build relation layers; compute alignment AUC and precision@k with degree-preserving null models; run causal discovery with tigramite (PCMCI+) and neural Granger code (SRNGC, ICML 2026); for variant C, run a temporal evaluation against relations that appear later.

**8. Baselines and evaluation.** Raw correlation and partial correlation; industry graph; random rewiring nulls. Chronological windows only. Report seeds and confidence intervals.

**9. Usable artefact.** A "graph audit report" generator: for any learned stock graph, a one-page profile of what it resembles (sector, supply chain, ownership, noise).

**10. 30-week feasibility, risks, fallback.**
- Weeks 1–8: review + proposal.
- Weeks 9–13: relation layers.
- Weeks 14–19: model retraining and graph extraction.
- Weeks 20–23: causal discovery.
- Weeks 24–26: forward test.
- Weeks 27–30: writing.

Risks: open-source code that does not run (pick models with working repos at the go/no-go); negative results. Fallback: causal-discovery part only (variant B), which needs no third-party models.

**11. Ethics.** Company-level only; disclose GenAI use.

**12. Scores.** Novelty 4, Fit 4, Data 4, Feasibility 4, Evaluation 4, AI relevance 4, Artefact 3, Industry 3, Frank 4 → **54/70**.

**13. Questions for the supervisor.** Is an audit/evaluation thesis acceptable as the main contribution? Should B (causal) or A (learned graphs) be the core?

---

## Card 6 — Unexplained co-movement: anomaly detection relative to a company knowledge graph

**1. Pitch.** Most strong co-movement between stocks is explained by factors or known relations: same sector, supplier, common owner. Flag the edges that are *not* explained, and test whether they coincide with coordination, meme episodes, manipulation cases or unannounced events.

**2. Research question.** *Can anomalous co-movement edges, strong but explained neither by factor exposures nor by any relation in a company knowledge graph, be detected reliably, and which kinds of public events do they coincide with?*
- RQ variant A (detection): Does a KG-conditioned edge-anomaly score (residual co-movement minus what a relation-aware model expects) separate event-linked edges from normal ones better than plain correlation-change or dynamic-graph anomaly detectors?
- RQ variant B (labels): Using event-typed weak labels (SEC litigation releases, trading halts, S&P index changes, 8-K events), which event types are graph-detectable at all, under positive-unlabelled evaluation? (ideas I19 + I20)
- RQ variant C (signed): Are sudden sign flips or new negative edges between related firms more often linked to events than new positive edges?

**3. Supervisor directions.** **Anomaly detection** · knowledge networks · signed and temporal edges · heterogeneous graph as context. Adjacent to her financial-crime work without being AML.

**4. Closest 2025–2026 work and the difference.**
- **ren2026effect** (arXiv 2605.08726) [ABS]. Explains *average* excess co-movement through co-attention and co-ownership (China and US); flags no individual edges.
- **caputi2026financial** (arXiv 2604.02549) [FT]. Graph-level anomaly detection on directed but unsigned TSX-60 graphs with 7 stress episodes.
- **chen2026velocity** (arXiv 2608.05373) [FT]. Manipulation detection with positive-only labels from SEC complaints; not graph-based.
- **iwata2026no** (arXiv 2607.26859) [ABS]. Positive-unlabelled inference on a company graph ("no record ≠ no risk").
- **huang2026crossstock** [FT]. A source of typed relations explaining co-movement; no anomaly angle.

**5. Kill test.** Searches (kill_K4 I19, I20): I20 **SURVIVED** (no edge-level "unexplained co-movement" detector found); I19 **SURVIVED narrowly** (graph anomaly benchmarks use few macro events or opaque labels).

**6. Data and access risk.** Prices + Fama–French factors; FinReflectKG + SIC + 13F as the explaining graph; labels from SEC litigation releases (public, need ticker mapping), trading-halt lists [availability to check], S&P index changes, 8-K items. Risk: **medium**, because building the labels is the main work.

**7. Method sketch.** Rolling residual correlations (signed); a relation-aware expectation model (GBDT or R-GCN predicting co-movement from KG features); anomaly score = observed − expected (+ temporal novelty). Compare with dynamic graph anomaly detectors (BAG-benchmark methods; PyGOD) and correlation-change baselines; PU metrics per event type.

**8. Baselines and evaluation.** Chronological evaluation; alarm budgets; precision@k per event type; placebo windows; report which events are *explained* anomalies (index rebalancing) versus unexplained ones.

**9. Usable artefact.** An analyst "surprise feed": a weekly list of the most unexplained co-moving pairs with context (relations checked, factors removed, nearby filings). This has the highest dashboard value of the six.

**10. 30-week feasibility, risks, fallback.**
- Weeks 1–8: review + proposal.
- Weeks 9–14: labels + graph.
- Weeks 15–20: detectors.
- Weeks 21–24: evaluation by event type.
- Weeks 25–27: feed/dashboard.
- Weeks 28–30: writing.

Risks: weak and sparse labels; most anomalies may be explained by unobserved news. Fallback: S&P index additions/deletions as a clean, explainable shock to validate the detector, plus a descriptive study of unexplained edges.

**11. Ethics.** Present as **screening, not accusation**. Do not name individuals; litigation releases name persons, so keep only firm/ticker-level labels. Disclose GenAI use.

**12. Scores.** Novelty 4, Fit 4, Data 3, Feasibility 3, Evaluation 3, AI relevance 4, Artefact 5, Industry 4, Frank 4 → **52/70**.

**13. Questions for the supervisor.** Do ING/SPIN contacts have views on market-abuse surveillance as a use case? Is a weak-label benchmark acceptable as a contribution?

---

## Runner-up cards (shorter)

### R1 — Sign flips as early warning: edge-event anomalies in signed, directed lead-lag graphs (ideas I3 + I21) — score ≈ 50
- **RQ:** Do bursts of lead-lag sign flips, directed signed-triad frustration, or a rise in link-forecast surprise *lead* market stress, beyond the absorption ratio, TSI and VIX, in a pre-registered lead-vs-coincident test across 2008, 2020, 2022 and 2023?
- **Closest:**
  - caputi2026financial: unsigned, 50-day loose window.
  - neela2025systemic (arXiv 2512.17185): unsigned graph features.
  - acedo2026triadic: coincident, with a sign null on PSD correlation matrices.
  - zhang2026spatialtemporal (CF2TLP, IEEE SMC 2026, record only): predicted-link intensity before SVB.
- **Kill test:** WEAKENED (early warning from network structure is crowded); signed directed edge events and forecast surprise remain open.
- **Data:** prices + OFR FSI/STLFSI (public).
- **Why runner-up:** few crisis events make evaluation fragile. Best used as a chapter of Card 3, which shares its graphs.

### R2 — Common ownership → future co-movement edges (I5 + I6) — score ≈ 49
- **RQ:** Does a temporal heterogeneous 13F manager–stock graph predict *new or strengthening* co-movement edges beyond the Antón–Polk regression, persistence and price-only correlation forecasters?
- **Closest:**
  - izadifar2026institutional (13F node affinity, code available);
  - fanshawe2026forecasting (no holdings);
  - dai2025griffinnet (holdings only as input);
  - Antón & Polk 2014 [BACKGROUND].
- **Kill test:** WEAKENED (the economics is known); the ML link-prediction version is open.
- **Data:** SEC 13F data sets 2013–2026 (public) + prices.
- **Why runner-up:** novelty is mostly the method transfer; the finance result is known.

### R3 — Cold-start relations for new listings (I24) — score ≈ 47
- **RQ:** From an S-1 / first 10-K and industry codes, predict which incumbent firms a new listing will co-move with (and the sign) in its first 6–12 months.
- **Closest:**
  - wang2023metastock [BACKGROUND]: price direction only.
  - TransFIR (ICLR 2026, record only): generic emerging entities.
- **Kill test:** SURVIVED. Risk: 100–300 usable IPOs per year.
- **Fit:** text + link prediction (inductive). Good sub-question for Card 1 (RQ C).

### R4 — The 2025 tariff shock through a public firm network (I17) — score ≈ 46
- **RQ:** Does multi-hop, signed exposure through an LLM-extracted supplier/customer/competitor graph explain abnormal returns on the 2025 tariff announcement dates beyond direct exposure, GICS and correlation graphs, for US and European listed firms?
- **Closest:**
  - Hu et al. (Econ Letters 2026): 10-K exposure text;
  - beyer2026trade;
  - barbiero2026trade: one-hop, FactSet.
- **Kill test:** WEAKENED; multi-hop signed propagation on a public graph is open.
- **Why runner-up:** effectively one shock, so a case study rather than a thesis. Strong fit with your politics interest; could be the application chapter of Card 1 or 4.

### R5 — Higher-order relations from text: hyperedges vs clique projections (I23) — score ≈ 46
- **RQ:** Do LLM-extracted n-ary relations (shared suppliers, consortia, joint ventures) as hyperedges predict group co-movement or crash contagion better than their clique projection and industry hyperedges?
- **Closest:** park2025dynamic, qiu2025hermes, tan2025magnet (learned or industry hyperedges); Qian et al. 2026 (IRF; shared suppliers raise co-movement) [ABS].
- **Kill test:** SURVIVED.
- **Why runner-up:** hypergraph models are a crowded architecture zone, and the extraction is costly.

---

## All raw ideas and kill-test outcomes

Kill tests: 2–13 searches per idea, by four kill-test agents (files `sweep/kill_K1–K4.md`, with query logs and BibTeX). "Responsible paper" = the paper that weakened the idea, or the closest work for survivors.

| ID | Idea | Outcome | Responsible / closest paper | Where it went |
| --- | --- | --- | --- | --- |
| I1 | Signed lead-lag edge-state forecasting | **Survived** | krstev2025temporal (withdrawn, 37 assets); castilho2021forecasting [bkg] | **Card 3** |
| I2 | Directed lagged structural balance (sign + lag cycle consistency) | Weakened | dahake2026sectoral; gallo2025rethinking (signed directed ERGM nulls) | Module in Card 3 (RQ B) |
| I3 | Edge-event anomalies in signed lead-lag graphs (early warning) | Weakened | caputi2026financial; neela2025systemic; zhao2025can | R1 |
| I4 | Credit events → connected firms' signed equity reactions | Survived (narrowly) | xu2025finripple; Hertzel 2008 [bkg] | **Card 4** |
| I5 | 13F common ownership → future co-movement edges | Weakened | Antón & Polk 2014 [bkg]; fanshawe2026forecasting; dai2025griffinnet | R2 |
| I6 | 13F edge birth/death and signed changes | Weakened | Thomaz & Mauá 2024 (N-PORT new positions); izadifar2026institutional | R2 / evaluation backbone |
| I7 | Temporal KG completion on FinReflectKG | **Survived** | naik2026finkgr (embargoed, unverifiable); arun2025finreflectkg | **Card 1** |
| I8 | Extraction multiverse for LLM company graphs | Survived | baumann2025large; crainic2026prompt (non-finance method sources) | **Card 2** (variant B) |
| I9 | Edge-level look-ahead contamination of LLM stock graphs | **Survived (strengthened)** | yang2026llm (frames but does not measure); zhang2026all | **Card 2** |
| I10 | Audit learned stock graphs vs real relations | Weakened | guo2026nystrom (one model, A-shares) | **Card 5** (variant A) |
| I11 | Learned edges as predictors of future real relations | Survived | alridhawi2026stock (qualitative only) | **Card 5** (variant C) |
| I12 | External validation suite for causal/lead-lag edges | Survived | sadeghi2023causal [bkg]; karpman2022learning [bkg] | **Card 5** (variant B) |
| I13 | Regime-switching signed causal graphs (sector portfolios) | Weakened | Lee 2026 (arXiv 2601.10732, regime-dependent Granger on factors) | Not shortlisted (incremental) |
| I14 | Finance-calibrated synthetic causal-discovery benchmark | Weakened | stein2026tcdarena; FinStressTS (2026) | Possible validation chapter of Card 5 |
| I15 | KG/LLM priors for causal discovery on stock returns | Weakened | GregNets (KDD workshop 2021) [bkg]; Zoroddu et al. TMLR 2024 [bkg]; FinCARE | Not shortlisted |
| I16 | Temporal-GNN explainers on financial temporal graphs | Survived | chen2026sttgexplainer; sussek2026explaining (non-finance data) | Explanation module for Cards 1/3/4 |
| I17 | 2025 tariff shock multi-hop signed propagation | Weakened | Hu et al. 2026 (Econ Letters); barbiero2026trade | R4 |
| I18 | US→Europe cross-market lead-lag bipartite graph | Weakened | liu2026bipartite (US→China); Jiao 2025 (arXiv 2504.07855) | Not shortlisted (European data) |
| I19 | Event-typed weak-label benchmark for market-graph anomalies | Survived (narrowly) | chen2026velocity; caputi2026financial; wang2025figraph | **Card 6** (variant B) |
| I20 | Unexplained co-movement vs company KG | Survived | ren2026effect (averages only) | **Card 6** |
| I21 | Link-forecast surprise as early warning | Weakened | zhang2026spatialtemporal (CF2TLP); wu2025dexposure (crypto) | R1 |
| I22 | Cross-firm insider / shadow-trading anomalies | **Dropped** (close to killed) | stenfors2025shadow; jaeger2025needles; Form 4 data limits + GDPR (named persons) | Rejected |
| I23 | Text-derived hyperedges vs clique projection | Survived | park2025dynamic; qiu2025hermes; Qian et al. 2026 | R5 |
| I24 | Cold-start signed relations for IPOs | Survived | wang2023metastock [bkg]; TransFIR (record only) | R3 / Card 1 RQ C |
| I25 | "How much of financial temporal LP is persistence?" | Weakened | cornell2025power; hayes2025what; izadifar2026institutional | Evaluation protocol in Cards 1, 3, R2 |
| I26 | Firm–lender–supplier–owner heterogeneous graph | Survived (no public lender layer) | majhi2026modeling; zandi2025multimodal | Card 4 (RQ C) |
| I27 | Credit ↔ equity multiplex Hawkes from public marks | Weakened | zelvyte2026multiplex | Not shortlisted (sparse events) |
| I28 | Supply-chain rewiring (decoupling) prediction and pricing | Weakened | Xiang 2025; Chang (AAAI 2025); Bao 2026; Crosignani 2025 | Not shortlisted (10-Ks rarely name Chinese counterparties) |
| I29 | Earnings-surprise signed information-transfer edges | **Survived** | xu2025finripple; Frankel et al. 2025 (JAR) | **Card 4** |
| I30 | Distress-text diffusion over the supply network | Weakened | Chinese graph+text distress models (see kill_K3); zhang2026bankruptcy | Not shortlisted |
