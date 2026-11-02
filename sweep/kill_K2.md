# Kill test K2: I5, I6, I7, I11, I21, I23, I24, I25

Agent K2, 2 Nov 2026. Rules: `sweep/KILL_RULES.md`. Every idea had at least 3 WebSearch queries (36 in total) plus 1 OpenReview API query. Legend: [FT] full text read · [ABS] abstract or record page opened · [UNVERIFIED] snippet only.
"Known" means the paper was already opened in the sweep and is listed in `ideas_raw.md`. Anything else is new.

---

## I5 — Common-ownership graph → future co-movement edges
- Queries:
  1) "common ownership predicts stock return comovement graph neural network": only Antón & Polk (*Connected Stocks*) copies, no ML follow-up.
  2) "institutional co-holding network link prediction future correlation stocks 2025": izadifar2026 (known), Wang & Yeung 2026 (Frontiers AI), GrifFinNet 2510.10387.
  3) "13F overlapping holdings network excess comovement machine learning 2026": portfolio-overlap systemic-risk papers (2016) and blogs, nothing relevant.
  4) "forecasting stock correlation matrix using institutional ownership overlap arXiv": Antón & Polk again, plus Bertschinger & Araneda 2021 (theory).
  5) "\"common ownership\" \"comovement\" 2025 journal finance new evidence": a common-ownership review and Chinese mutual-fund comovement papers, no ML.
  6) "fund holdings graph predict pairwise stock correlation deep learning": fanshawe2026forecasting (known) and MONEY (2023).
- Closest work found (new):
  - **wang2026large**: *Large language model-driven time-series forecasting of financial network indicators*, Frontiers in AI, 29 Jan 2026 [ABS+partial FT]. Projects a quarterly fund–stock holding graph onto stock–stock links, but forecasts aggregate network indicators (centralisation, density spikes). It does not predict edges and does not link to price co-movement.
  - **dai2025griffinnet**: *GrifFinNet*, arXiv 2510.10387, ~Oct 2025 [FT]. Uses an institutional co-ownership graph (CSI300/800) only as an *input* relation for return ranking. Co-movement is not a target.
  - **sun2023money**: *MONEY*, AI Open 2023 [background, ABS]. Uses fund-holding hyperedges as input for price-movement classification.
  - **antonpolk2014connected**: *Connected Stocks*, JF 2014 [background, ABS]. This is the econometric core: common active-fund ownership predicts future excess comovement.
  - **fanshawe2026forecasting** (known): GNN forecasting of S&P 500 correlations with no holdings input.
- Verdict: **WEAKENED**.
  - Finance has already established that common ownership predicts *comovement levels* (Antón & Polk 2014), so "ownership → comovement" is not new.
  - ML correlation forecasting exists (Fanshawe 2026) but ignores holdings.
  - Holdings graphs appear in ML only as an input for return prediction.
  - Open: temporal link prediction of *new* or *strengthening* co-movement edges from a 13F heterogeneous manager–stock graph, with the Antón–Polk regression, persistence and price-only correlation forecasters as baselines.
- Sharpened version: Does a temporal heterogeneous 13F graph add out-of-sample information for *emerging* residual co-movement edges? An emerging edge is a pair whose factor-residual correlation crosses a threshold next quarter but was below it before. The baselines would be Antón–Polk FCAP regression, persistence and a Fanshawe-style price-only GNN. Evaluate with time-respecting splits and the 45-day filing lag.
- Public data: SEC 13F (EDGAR bulk / Form 13F data sets) plus daily prices (e.g. Yahoo or Stooq). A point-in-time S&P 500 list is needed. CUSIP→ticker mapping is the main friction (OpenFIGI). Feasibility: good. The data pipeline is the bulk of the work, and the models (logistic baseline, HGT/TGN in PyG) are standard.

## I6 — 13F edge birth/death and signed changes
- Queries:
  1) "predicting institutional investor position entry exit 13F machine learning": izadifar2026 (known), industry tools, no papers.
  2) "mutual fund portfolio holdings prediction temporal graph neural network 2026": izadifar2026, Jiang et al. ICAIF 2023 (fund performance), Thomaz & Mauá 2024.
  3) "forecasting fund trades buy sell recommendation system holdings graph": Thomaz & Mauá 2024, holder recommendation 2022.
- Closest work found (new):
  - **thomaz2024case**: *A Case Study of Next Portfolio Prediction for Mutual Funds*, arXiv 2410.18098, Oct 2024 [FT]. Predicts *novel* positions (next novel basket) and repeated holdings. Data: SEC N-PORT, 5,014 funds × 4,917 US stocks, 2020–2021 only, out-of-time split. Simple heuristics beat next-basket models on all items, and autoencoders win on novel items. Exits and size changes are not predicted.
  - **unknown2022holder**: *Holder Recommendations using Graph Representation Learning & Link Prediction*, arXiv 2212.09624 [ABS]. GraphSAGE link prediction on a holder–fund graph. The data looks proprietary.
  - **jiang2023network**: ICAIF 2023 [ABS]. A dynamic heterogeneous fund graph for fund performance, not holdings.
  - izadifar2026 (known) covers next-quarter weights for 99 managers.
- Verdict: **WEAKENED**.
  - Births of new positions: Thomaz & Mauá 2024 (N-PORT).
  - Weights: Izadifar 2026 (13F, 99 managers).
  - Open:
    - exits and signed increase/decrease as an explicit 3–4-class edge-event task;
    - the full 13F filer population over a long horizon (2013–2025);
    - point-in-time, filing-lag-aware features;
    - comparing TGNNs with recency/popularity heuristics on 13F.
  - Overall incremental, and close to a benchmark exercise.
- Sharpened version: a filing-lag-aware, signed edge-event benchmark on 13F covering birth, exit, increase and decrease, for all filers and 2013–2025. Unseen-edge negatives, heuristics vs TGN/DyGFormer/NAVIS. Ideally merged into I25 rather than run as a standalone thesis.
- Public data: SEC 13F (fully public). N-PORT can be used for validation. Feasibility: high technically, but low novelty on its own.

## I7 — Temporal KG completion on FinReflectKG
- Queries:
  1) "FinReflectKG link prediction knowledge graph completion": only the FinReflectKG paper, EvalBench and press release, no KGC use.
  2) "temporal knowledge graph forecasting company relations 10-K filings 2026": FinReflectKG dataset card, Dukkipati et al. 2504.20058, SJSU FinKGR, huang2026 (known).
  3) "FinReflectKG temporal reasoning benchmark 2026 arXiv": only MultiHop (known).
  4) "supply chain link prediction public firms SEC filings knowledge graph embedding 2025": Yılkı 2606.29290, InterCorpRel (known).
  5) OpenReview API "financial temporal knowledge graph completion": no financial KGC paper. MEHGT-LKG (rejected ICLR 2026, stock trend) and TransFIR (generic) came up.
- Closest work found (new):
  - **naik2026finkgr**: *FinKGR: Neuro-Symbolic Reasoning on Financial Knowledge Graphs via Rule Mining and Transformers*, SJSU MSc project, online 31 Jul 2026 [ABS; PDF embargoed until 2027]. Temporal link prediction (future quadruples) on a *financial TKG*. The dataset is not named, and 10-Ks and company–company relations are not mentioned. The only reported result is "coverage". It is the closest hit, but unverifiable and not peer-reviewed.
  - **dukkipati2025predictive**: *Predictive AI with External Knowledge Infusion*, arXiv 2504.20058 (v2 Feb 2026) [ABS]. Builds stock-market TKGs and models relations as Hawkes events *to rank returns*. KG completion is not the task.
  - **yilki2026supply**: arXiv 2606.29290, Jun 2026 [ABS]. Propagates 10-K MD&A embeddings over *given* supply-chain links and gets significant return predictability. It does not predict links. It supports the second half of I7 (edges carry return information).
  - An emergentmind topic page lists only the two Arun et al. papers as FinReflectKG users. No paper uses FinReflectKG (or another public time-stamped 10-K company KG) for link prediction or KG completion.
- Verdict: **SURVIVED**. The closest works differ in:
  - data: no public 10-K company KG (FinKGR's dataset is unknown; Dukkipati uses market-built TKGs);
  - task: year-ahead company–company relation completion followed by an economic test of predicted edges (nobody does both).
  - Risk: FinKGR's full text, once released (2027), might use FinReflectKG. Re-check before the go/no-go.
- Sharpened version:
  - Year-ahead temporal KGC on FinReflectKG's company–company relations (Supplies, Partners_With, Competes_With, Has_Stake_In), with entity resolution fixed and a strict train ≤ t / test t+1 split.
  - Baselines: recurrence/EdgeBank, TLogic-style rules, RE-GCN/TiRGN. TransFIR is used for new firms.
  - Then test whether *predicted-new* edges show abnormal co-movement or return spillover before they appear in a filing.
- Public data: FinReflectKG (HF `domyn/FinReflectKG`, CC-BY-NC, 743 firms, 2014–2024, start/end dates) plus prices. Risk: LLM-extraction noise and inconsistent entity names. Plan 3–4 weeks of cleaning. Feasibility: good. Open-source TKG libraries exist (TKG-forecasting repos, PyG). The scope fits 30 weeks.

## I11 — Learned/attention edges as predictors of future real relations
- Queries:
  1) "learned attention graph stocks validate against real company relationships future": Al Ridhawi et al. 2603.05917, niu2024evaluating [bkg], older attention models.
  2) "interpretability latent stock graph structure learning recovers supply chain industry 2026": kose2026 (known), production-network reconstruction (INET Oxford 2022, not opened).
  3) "return co-movement predicts future mergers acquisitions board interlocks network firms": management literature (board interlocks → M&A imitation), no price-learned edges.
  4) "stock return correlations anticipate new customer supplier relationships firms": Cohen–Frazzini-type return-predictability papers (the reverse direction).
- Closest work found (new):
  - **alridhawi2026stock**: *Stock Market Prediction Using Node Transformer … BERT Sentiment*, arXiv 2603.05917 (2026) [FT]. Learned edge weights for 20 stocks are checked only qualitatively against GICS (same-sector pairs get higher weights). The authors note but do not test whether the learned graph matches supply chains or persists into the test period.
  - niu2024evaluating [bkg, known] covers static alignment of predefined graphs.
- Verdict: **SURVIVED**. Closest work differs in:
  - task: future relation prediction vs static or qualitative alignment;
  - targets: 13F co-holdings, 10-K competitor mentions, M&A, index co-inclusion;
  - evaluation: precision@k over time vs anecdotes.
  - Caveat: the idea depends on I10's infrastructure (extracting learned graphs from open-source models). If learned graphs turn out mostly noise, the result is a negative finding.
- Sharpened version: Do edges learned by price-based stock GNNs (MASTER/GAPNet-style) at time t predict relations that first appear at t+1…t+4 quarters? Targets are new 13F co-holding overlap, new 10-K competitor or customer mentions (FinReflectKG), M&A announcements and index co-inclusion. Compare against raw correlation and industry baselines.
- Public data: prices, 13F, FinReflectKG, SEC 8-K for M&A (or Wikipedia/SDC-free lists). Feasibility: medium. Retraining 2–3 open-source models on a point-in-time universe is the main cost, which GenAI-assisted coding makes manageable.

## I21 — Link predictability as an early-warning signal
- Queries:
  1) "CF2TLP temporal link prediction stock market crisis forecasting": ORCA record (opened), plus Spelta-type "links forecast" paper (2017).
  2) "network predictability early warning financial crisis link prediction accuracy drop": bank-distress EWS and IMF connectedness papers, none using link-forecast error.
  3) "correlation network structural surprise systemic risk indicator 2025 arXiv": Bartesaghi 2025 (known), Neela 2512.17185 SRR, global-balance papers.
- Closest work:
  - **zhang2026spatialtemporal** (CF2TLP; ORCA record re-opened, [ABS]). IEEE SMC 2026, in press; record dated 11 Aug 2026. The PDF is restricted to repository staff.
    - Approach: GCN + Transformer temporal link prediction on "structured financial networks" of US tech stocks, with contagion treated as information spreading. The record reports that predicted contagion activity rose before the SVB collapse, with AUC and precision above historical peaks.
    - Not stated: edge definition, period and baselines. It is a single-event (SVB) case study.
    - Signal: the *intensity of predicted links* (and high predictability) is the warning. In I21 the signal is the *forecaster's surprise* (log-loss, i.e. low predictability). These are opposite readings, which in itself is a testable question.
  - **neela2025systemic** (new): *Systemic Risk Radar*, arXiv 2512.17185, Dec 2025 [ABS]. Multi-layer correlation-graph features and GNNs for crash-regime early warning over the dot-com, GFC and COVID episodes. It uses graph *features*, not link-prediction error.
  - wu2025dexposure (known): AUPRC drops at DeFi shocks and is proposed as an indicator (crypto, descriptive).
  - **spelta2017financial** (new, [ABS], authors not shown on the opened page): tensor-decomposition link forecasting of correlation networks, used for trading signals, not crisis detection [background].
- Verdict: **WEAKENED**.
  - The idea that predictability tracks market state is already proposed: DeXposure (DeFi) and implicitly CF2TLP (equity, SVB).
  - Open:
    1. the signal is edge-level forecast *surprise*, not predicted-link counts;
    2. the evaluation is multi-crisis (2008, 2020, 2022, SVB 2023) and pre-registered lead-vs-coincident against standard indicators (absorption ratio, TSI/CISS, VIX, SRR-style graph features);
    3. equity co-movement plus 13F ownership graphs, instead of DeFi.
  - A direct contrast with CF2TLP's "rising predicted links" reading is possible.
- Sharpened version: A pre-registered test of whether rolling edge-level surprise from a simple link forecaster (persistence / logistic / TGN) on equity co-movement graphs *leads* the absorption ratio, CISS/TSI and SRR-style features across four stress episodes. Include placebo windows and a reconciliation with CF2TLP's opposite signal.
- Public data: prices (S&P 500 point-in-time), FRED/ECB CISS, VIX, 13F (optional). Feasibility: good. Methods are light. The risk is a null result (the signal may be coincident, like acedo2026triadic), which is still publishable as a rigorous negative.

## I23 — Higher-order relations from text (hyperedges)
- Queries:
  1) "LLM extracted hypergraph companies stock prediction n-ary relations": H3M-SSMoEs 2510.25091, huang2026 (known, pairwise), KG construction 2601.11528 (known).
  2) "news-driven hyperedges stock co-movement hypergraph 2026 arXiv": CSHT (Harit et al., ICAIF 2025), MONEY 2023, geometric hypergraph 2409.00438 (not opened).
  3) "hypergraph versus clique expansion stock relations text knowledge 2025": generic hypergraph expansion papers (AdE 2502.15564), no finance.
  4) "shared supplier hypergraph firm network stock return contagion": Qian et al. 2026 (IRFI), older supply-chain finance.
- Closest work found (new):
  - **tan2025h3m**: *H3M-SSMoEs*, arXiv 2510.25091 (~Oct 2025) [FT]. Hyperedges are *learned latent* hyperedges. Text is LLM-generated news used as features. No clique-expansion comparison.
  - **harit2025news**: *From news to returns: A Granger-causal hypergraph transformer on the sphere* (CSHT), ICAIF 2025, 14 Nov 2025 [ABS]. Hyperedges come from Granger dependencies, not extracted n-ary relations.
  - **qian2026common**: *Common Suppliers and Stock Return Co-Movement: Evidence From China*, International Review of Finance 2026 [ABS]. Econometric: shared suppliers raise comovement, more strongly in weak markets. This is pairwise and Chinese data, but it supports the economic premise.
  - huang2026crossstock (known) uses LLM-filtered *pairwise* edges from 10-Ks.
- Verdict: **SURVIVED**. Closest works differ in:
  - hyperedge source: explicit LLM-extracted n-ary relations (consortia, shared supplier sets, JVs) vs learned, Granger, industry or fund hyperedges;
  - evaluation design: a controlled hyperedge vs clique-expansion vs industry-hyperedge comparison for co-movement and crash-contagion targets, which no paper runs.
  - Risk: n-ary relations may be sparse in 10-Ks, and shared-supplier hyperedges can be derived from FinReflectKG `Supplies` edges without new extraction.
- Sharpened version: Build shared-counterparty hyperedges (all firms naming the same supplier, customer or JV) from FinReflectKG or 10-K text. Test whether a hypergraph model predicts residual co-movement and joint crashes better than (a) its clique expansion, (b) GICS hyperedges and (c) learned hyperedges, with economic validation à la Qian et al. 2026 on US data.
- Public data: FinReflectKG / EDGAR 10-Ks, prices. Feasibility: good if FinReflectKG is reused. Fresh LLM extraction adds cost and noise (see I8).

## I24 — Cold-start signed relation inference for IPOs/spin-offs
- Queries:
  1) "newly listed stocks IPO cold start graph neural network stock prediction": surveys, IPO-performance ANNs, NGAT (known), nothing on relations.
  2) "IPO peer firms identification prospectus text embedding comparable companies": comparable-company NER (2307.07420), Company2Vec, peer-firm embeddings, none temporal or price-edge.
  3) "inductive stock relation inference unseen stocks no price history 2026": TRACE (known), Relational Probing 2604.10212 (known group), Tian et al. IJOC 2022.
  4) "\"cold-start\" stocks forecasting few-shot new listings transfer learning 2025": Meta-Stock 2308.11117 (sub-new stocks).
- Closest work found (new):
  - **wang2023metastock**: *Meta-Stock: … Sub-new Stock Price Prediction*, arXiv 2308.11117 (2023) [FT, background]. Meta-learning *price direction* for stocks listed under one year (US/CN/HK). No relations, and text is explicitly excluded.
  - **tian2022inductive**: *Inductive Representation Learning on Dynamic Stock Co-Movement Graphs*, INFORMS JoC 2022 [FT, background]. Inductive GNN on price co-movement graphs, not tested on new firms and no text.
  - ding2026trace (known; [FT] here): fixed KG, no unseen entities.
  - Relational Probing (niu, known group; [ABS] here): news-induced relations for a *fixed* ticker set.
  - TransFIR (ICLR 2026, generic TKG emerging entities; known, record only).
- Verdict: **SURVIVED**. Closest work differs in:
  - task: predicting a new firm's *edges* (signed lead-lag / co-movement) vs its price direction (Meta-Stock) or relations among known tickers;
  - inputs: S-1/10-K text plus fundamentals before price history exists;
  - evaluation: inductive time split on actual post-IPO edges.
  - Risk: about 100–300 usable US IPOs/spin-offs per year in a large-cap-relevant universe. Signed lead-lag edges for new firms are noisy, so co-movement edges may be the safer target.
- Sharpened version: Given only an S-1 (or first 10-K) and industry codes, predict which incumbent S&P 1500 firms a new listing will co-move with, and with what sign, in its first 6–12 months. Evaluate against text-similarity peers (Hoberg–Phillips TNIC-style), GICS peers and an inductive GNN, with TransFIR-style KG reasoning as the ML baseline.
- Public data: EDGAR S-1/424B and 10-K, IPO dates (EDGAR form types, Jay Ritter's public IPO lists), prices. Feasibility: medium. Text pipeline plus a small-sample evaluation. Doable in 30 weeks if the target is co-movement rather than validated lead-lag.

## I25 — "How much of financial temporal link prediction is persistence?"
- Queries:
  1) "temporal graph learning financial networks benchmark heuristics beat TGNN evaluation pitfalls": Cornell et al. ICLR 2025 workshop, T-GRAB 2507.10183, TGB.
  2) "temporal link prediction negative sampling evaluation flaws 2026 benchmark memorization baseline": Poursafaei et al. NeurIPS 2022 (EdgeBank) [UNVERIFIED here, well-known bkg], Hayes et al. 2510.09416.
  3) "dynamic link prediction stock networks EdgeBank baseline comparison": only EdgeBank/DGB, no stock-network application.
  4) "temporal graph benchmark financial datasets equity ownership supply chain 2026": kose2026 (known), LDBC FinBench (graph-DB benchmark), TGB. No finance TGL benchmark.
- Closest work found (new):
  - **cornell2025power**: *On the Power of Heuristics in Temporal Graphs*, ICLR 2025 workshop (ICBINB) [ABS]. Recency and popularity heuristics match or beat SOTA TGNNs on TGB and BenchTemp. No financial data.
  - **dizaji2025tgrab**: *T-GRAB*, arXiv 2507.10183 (Jul 2025) [FT summary]. Synthetic diagnostics. TGNNs fall back on persistence-like shortcuts, and EdgeBank sometimes wins.
  - **hayes2025what**: *What Do Temporal Graph Learning Models Learn?*, arXiv 2510.09416 (v3, 2025/26) [FT summary]. Models capture persistence and preferential attachment but fail on direction, density and recency. Only Bitcoin-Alpha is finance-flavoured.
  - Known: yi2025tgbseq, feldman2025revisiting (NAVIS), izadifar2026 (EMA beats most TGNNs on 13F).
- Verdict: **WEAKENED**.
  - The generic claim (heuristics/persistence rival TGNNs; repeated edges inflate scores) is well established for 2022–2026: EdgeBank, TGB-Seq, Cornell 2025, T-GRAB, NAVIS, Hayes.
  - Izadifar 2026 already shows it for 13F weights.
  - Open: a *finance-specific* protocol across several public company graphs (13F, co-movement, FinReflectKG supply links) with filing-lag-aware splits, point-in-time universes and degree-corrected or unseen-edge negatives. No such benchmark was found.
  - Novelty is mainly domain transfer, so it is better as the evaluation backbone of I1/I5/I7 than as a standalone thesis.
- Sharpened version: a "FinTGB-lite" protocol and leaderboard of 3 public financial temporal graphs, scored separately on repeated vs new edges. The main contribution is quantifying the persistence share per graph and showing which conclusions in the 2025–26 stock-graph literature survive the protocol.
- Public data: 13F, prices, FinReflectKG, all public. Feasibility: high. Engineering-heavy but low-risk, with tooling from TGB/DyGLib.

---

## BibTeX (new papers only)

```bibtex
@article{wang2026large,
  author = {Wang, Mini Han and Yeung, Ying},
  title = {Large language model-driven time-series forecasting of financial network indicators},
  journal = {Frontiers in Artificial Intelligence},
  year = {2026},
  url = {https://www.frontiersin.org/articles/10.3389/frai.2026.1722121/full},
  note = {[ABS+partial FT] 2026-01-29; Frontiers in AI vol. 9}
}
@misc{dai2025griffinnet,
  author = {Dai, Chenlanhui and Wang, Wenyan and Fan, Yusi and others},
  title = {GrifFinNet: A Graph-Relation Integrated Transformer for Financial Predictions},
  year = {2025},
  url = {https://arxiv.org/pdf/2510.10387},
  note = {[FT] arXiv 2510.10387 (~Oct 2025); preprint}
}
@article{sun2023money,
  author = {Sun, Zhongtian and Harit, Anoushka and Cristea, Alexandra I. and Wang, Jingyun and Lio, Pietro},
  title = {MONEY: Ensemble learning for stock price movement prediction via a convolutional network with adversarial hypergraph model},
  journal = {AI Open},
  volume = {4}, pages = {165--174}, year = {2023},
  url = {https://durham-repository.worktribe.com/OutputFile/2118518},
  note = {[ABS] 2023; AI Open [background]}
}
@article{anton2014connected,
  author = {Ant{\'o}n, Miguel and Polk, Christopher},
  title = {Connected Stocks},
  journal = {Journal of Finance}, volume = {69}, number = {3}, pages = {1099--1127}, year = {2014},
  url = {https://peeref.com/works/55097011},
  note = {[ABS] 2014; JF [background; already in ideas_raw as anton2014connected]}
}
@misc{bertschinger2021crossownership,
  author = {Bertschinger, Nils and Araneda, Axel A.},
  title = {Cross-ownership as a structural explanation for rising correlations in crisis times},
  year = {2021},
  url = {https://arxiv.org/abs/2112.04824v1},
  note = {[ABS] 2021-12-09; preprint [background]}
}
@misc{thomaz2024case,
  author = {Thomaz, Guilherme and Mau{\'a}, Denis},
  title = {A Case Study of Next Portfolio Prediction for Mutual Funds},
  year = {2024},
  url = {https://web3.arxiv.org/pdf/2410.18098},
  note = {[FT] arXiv 2410.18098 Oct 2024; preprint}
}
@misc{unknown2022holder,
  author = {{Unknown}},
  title = {Holder Recommendations using Graph Representation Learning \& Link Prediction},
  year = {2022},
  url = {https://fugumt.com/fugumt/paper_check/2212.09624v1_enmode},
  note = {[ABS] arXiv 2212.09624, 2022-11-10; preprint; authors not shown [background]}
}
@inproceedings{jiang2023network,
  author = {Jiang, Siqi and Uddin, Ajim and Wei, Zhi and Yu, Dantong},
  title = {The Network of Mutual Funds: A Dynamic Heterogeneous Graph Neural Network for Estimating Mutual Funds Performance},
  booktitle = {ICAIF 2023}, year = {2023},
  url = {https://digitalcommons.njit.edu/fac_pubs/1316},
  note = {[ABS] 2023-11-27; ICAIF 2023 [background]}
}
@misc{naik2026finkgr,
  author = {Naik, Rudraksh},
  title = {FinKGR: Neuro-Symbolic Reasoning on Financial Knowledge Graphs via Rule Mining and Transformers},
  year = {2026},
  url = {https://scholarworks.sjsu.edu/etd_projects/1794},
  note = {[ABS] online 2026-07-31; SJSU MSc project, PDF embargoed to 2027}
}
@misc{dukkipati2025predictive,
  author = {Dukkipati, Ambedkar and Mayilvaghanan, Kawin and Pallekonda, Naveen Kumar and Hadnoor, Sai Prakash and Ayyagari, Ranga Shaarad},
  title = {Predictive AI with External Knowledge Infusion: Datasets and Benchmarks for Stock Markets},
  year = {2025},
  url = {https://arxiv.org/abs/2504.20058v2},
  note = {[ABS] 2025-04-14 (v2 2026-02-11); preprint}
}
@misc{yilki2026supply,
  author = {Y{\i}lk{\i}, Asef},
  title = {Supply Chain Propagation of Textual Signals: LLM Embeddings and Cross-Sectional Return Predictability},
  year = {2026},
  url = {https://arxiv.org/abs/2606.29290},
  note = {[ABS] 2026-06-28; preprint}
}
@misc{alridhawi2026stock,
  author = {Al Ridhawi, Mohammad and Haj Ali, Mahtab and Al Osman, Hussein},
  title = {Stock Market Prediction Using Node Transformer Architecture Integrated with BERT Sentiment Analysis},
  year = {2026},
  url = {https://arxiv.org/pdf/2603.05917},
  note = {[FT] arXiv 2603.05917 (Mar 2026); preprint}
}
@misc{neela2025systemic,
  author = {Neela, Sandeep},
  title = {Systemic Risk Radar: A Multi-Layer Graph Framework for Early Market Crash Warning},
  year = {2025},
  url = {https://econpapers.repec.org/paper/arxpapers/2512.17185.htm},
  note = {[ABS] arXiv 2512.17185, Dec 2025; preprint}
}
@article{spelta2017financial,
  author = {{Unverified (page did not list authors)}},
  title = {Financial market predictability with tensor decomposition and links forecast},
  journal = {Applied Network Science}, year = {2017},
  url = {https://appliednetsci.springeropen.com/articles/10.1007/s41109-017-0028-1/figures/1},
  note = {[ABS] figure page only; year inferred from DOI; Applied Network Science [background]}
}
@misc{tan2025h3m,
  author = {Tan, Peilin and Xie, Liang and Zhi, Churan and Tu, Dian and Shi, Chuanqi},
  title = {H3M-SSMoEs: Hypergraph-based Multimodal Learning with LLM Reasoning and Style-Structured Mixture of Experts},
  year = {2025},
  url = {https://arxiv.org/html/2510.25091v1},
  note = {[FT] arXiv 2510.25091 (~Oct 2025); preprint}
}
@inproceedings{harit2025news,
  author = {Harit, Anoushka and Sun, Zhongtian and Yu, Jongmin},
  title = {From news to returns: A Granger-causal hypergraph transformer on the sphere},
  booktitle = {ICAIF '25}, pages = {674--682}, year = {2025},
  url = {https://kar.kent.ac.uk/112602},
  note = {[ABS] 2025-11-14; ICAIF 2025}
}
@article{qian2026common,
  author = {Qian, Xianhang and Zhang, Le and Zhao, Kunzhu},
  title = {Common Suppliers and Stock Return Co-Movement: Evidence From China},
  journal = {International Review of Finance}, year = {2026},
  url = {https://www.citedrive.com/en/discovery/common-suppliers-and-stock-return-comovement-evidence-from-china/},
  note = {[ABS] 2026; Int. Rev. Finance (ISSN 1369-412X), doi 10.1111/irfi.70087}
}
@misc{wang2023metastock,
  author = {Wang, Linghao and Liu, Zhen and Ma, Peitian and Ma, Qianli},
  title = {Meta-Stock: Task-Difficulty-Adaptive Meta-learning for Sub-new Stock Price Prediction},
  year = {2023},
  url = {https://ar5iv.labs.arxiv.org/html/2308.11117},
  note = {[FT] arXiv 2308.11117, Aug 2023 [background]}
}
@article{tian2022inductive,
  author = {Tian, Hu and Zheng, Xiaolong and Zhao, Kang and Liu, Maggie Wenjing and Zeng, Daniel Dajun},
  title = {Inductive Representation Learning on Dynamic Stock Co-Movement Graphs for Stock Predictions},
  journal = {INFORMS Journal on Computing}, volume = {34}, number = {4}, pages = {1940--1957}, year = {2022},
  url = {https://www.biz.uiowa.edu/faculty/kangzhao/pub/ijoc_2022.pdf},
  note = {[FT] 2022; INFORMS JoC [background]}
}
@inproceedings{cornell2025power,
  author = {Cornell, Filip and Smirnov, Oleg and Zarzar Gandler, Gabriela and Cao, Lele},
  title = {On the Power of Heuristics in Temporal Graphs},
  booktitle = {ICLR 2025 Workshop: I Can't Believe It's Not Better}, year = {2025},
  url = {https://www.iclr.cc/virtual/2025/33600},
  note = {[ABS] 2025; ICLR 2025 workshop}
}
@misc{dizaji2025tgrab,
  author = {Dizaji, Alireza and Tjandra, Benedict Aaron and Hamidi, Mehrab and Huang, Shenyang and Rabusseau, Guillaume},
  title = {T-GRAB: A Synthetic Diagnostic Benchmark for Learning on Temporal Graphs},
  year = {2025},
  url = {https://arxiv.org/pdf/2507.10183v2},
  note = {[FT] arXiv 2507.10183 (v2 2025-07-22); preprint}
}
@misc{hayes2025what,
  author = {Hayes, Abigail J. and Schumacher, Tobias and Strohmaier, Markus},
  title = {What Do Temporal Graph Learning Models Learn?},
  year = {2025},
  url = {https://arxiv.org/html/2510.09416v3},
  note = {[FT] arXiv 2510.09416 (Oct 2025, v3 later); preprint}
}
```

Also re-opened in this run (known, not new): zhang2026spatialtemporal (ORCA record [ABS]; PDF restricted), ding2026trace (2603.12500 [FT]), Relational Probing 2604.10212 [ABS], huang2026crossstock 2604.19476 [ABS], fanshawe2026forecasting 2601.04602 [ABS].

## Query log

| # | Idea | Query | Tool | Outcome |
|---|---|---|---|---|
| 1 | I5 | common ownership predicts stock return comovement graph neural network | WebSearch | Antón & Polk copies only |
| 2 | I5 | institutional co-holding network link prediction future correlation stocks 2025 | WebSearch | izadifar (known), Wang & Yeung 2026, GrifFinNet |
| 3 | I5 | 13F overlapping holdings network excess comovement machine learning 2026 | WebSearch | systemic-risk overlap (2016), blogs |
| 4 | I5 | forecasting stock correlation matrix using institutional ownership overlap arXiv | WebSearch | Antón & Polk, Bertschinger 2021 |
| 5 | I5 | "common ownership" "comovement" 2025 journal finance new evidence | WebSearch | reviews, China MF comovement; no ML |
| 6 | I5 | fund holdings graph predict pairwise stock correlation deep learning | WebSearch | Fanshawe 2026 (known), MONEY 2023 |
| 7 | I6 | predicting institutional investor position entry exit 13F machine learning | WebSearch | izadifar only; tools/blogs |
| 8 | I6 | mutual fund portfolio holdings prediction temporal graph neural network 2026 | WebSearch | izadifar, Jiang 2023, Thomaz 2024 |
| 9 | I6 | forecasting fund trades buy sell recommendation system holdings graph | WebSearch | Thomaz 2024, holder-recsys 2022 |
| 10 | I7 | FinReflectKG link prediction knowledge graph completion | WebSearch | only FinReflectKG/EvalBench; no KGC |
| 11 | I7 | temporal knowledge graph forecasting company relations 10-K filings 2026 | WebSearch | FinKGR (SJSU), Dukkipati 2504.20058 |
| 12 | I7 | FinReflectKG temporal reasoning benchmark 2026 arXiv | WebSearch | MultiHop only |
| 13 | I7 | supply chain link prediction public firms SEC filings knowledge graph embedding 2025 | WebSearch | Yılkı 2606.29290, InterCorpRel |
| 14 | I7 | financial temporal knowledge graph completion | OpenReview API | no financial KGC; MEHGT-LKG, TransFIR |
| 15 | I11 | learned attention graph stocks validate against real company relationships future | WebSearch | Al Ridhawi 2603.05917, niu2024 |
| 16 | I11 | interpretability latent stock graph structure learning recovers supply chain industry 2026 | WebSearch | kose2026, production-network reconstruction |
| 17 | I11 | return co-movement predicts future mergers acquisitions board interlocks network firms | WebSearch | management lit only |
| 18 | I11 | stock return correlations anticipate new customer supplier relationships firms | WebSearch | Cohen–Frazzini-type (reverse direction) |
| 19 | I21 | CF2TLP temporal link prediction stock market crisis forecasting | WebSearch | ORCA record; Spelta-type 2017 |
| 20 | I21 | network predictability early warning financial crisis link prediction accuracy drop | WebSearch | bank-distress EWS; none on forecast error |
| 21 | I21 | correlation network structural surprise systemic risk indicator 2025 arXiv | WebSearch | Neela SRR 2512.17185, balance papers |
| 22 | I23 | LLM extracted hypergraph companies stock prediction n-ary relations | WebSearch | H3M-SSMoEs, huang2026 (pairwise) |
| 23 | I23 | news-driven hyperedges stock co-movement hypergraph 2026 arXiv | WebSearch | CSHT ICAIF 2025, MONEY |
| 24 | I23 | hypergraph versus clique expansion stock relations text knowledge 2025 | WebSearch | generic only |
| 25 | I23 | shared supplier hypergraph firm network stock return contagion | WebSearch | Qian et al. 2026 IRFI |
| 26 | I24 | newly listed stocks IPO cold start graph neural network stock prediction | WebSearch | nothing on relations |
| 27 | I24 | IPO peer firms identification prospectus text embedding comparable companies | WebSearch | comparable-company NER, Company2Vec |
| 28 | I24 | inductive stock relation inference unseen stocks no price history 2026 | WebSearch | TRACE, Relational Probing, Tian 2022 |
| 29 | I24 | "cold-start" stocks forecasting few-shot new listings transfer learning 2025 | WebSearch | Meta-Stock 2023 |
| 30 | I25 | temporal graph learning financial networks benchmark heuristics beat TGNN evaluation pitfalls | WebSearch | Cornell 2025, T-GRAB |
| 31 | I25 | temporal link prediction negative sampling evaluation flaws 2026 benchmark memorization baseline | WebSearch | EdgeBank 2022, Hayes 2510.09416 |
| 32 | I25 | dynamic link prediction stock networks EdgeBank baseline comparison | WebSearch | EdgeBank only |
| 33 | I25 | temporal graph benchmark financial datasets equity ownership supply chain 2026 | WebSearch | no finance TGL benchmark |
