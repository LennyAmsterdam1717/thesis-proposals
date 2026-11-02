# Kill tests K1 — ideas I1, I2, I3, I12, I13, I14, I15, I18

Agent K1, 2 Nov 2026. Rules: `sweep/KILL_RULES.md`. All "new" papers below were opened in this session (WebFetch); papers already in `ideas_raw.md` are marked *(known)* and not re-counted as new. The Bing-style search tool sometimes returns old papers for "2025/2026" queries; absence claims are scoped to the queries listed.

**Requested item, found and opened:** *The Network Factor of Equity Pricing: A Signed Graph Laplacian Approach*, by Uddin, Tao and Yu (NJIT), Journal of Financial Econometrics 22(5):1616–1655, 2024, DOI 10.1093/jjfinec/nbae010. I read the record page [ABS] at https://digitalcommons.njit.edu/fac_pubs/795. The NJIT repository has no PDF. The paper builds a signed graph Laplacian over firm networks and summarises how the market's network structure changes into a *dynamic network index* (DNI). It then shows that DNI is a priced factor: stocks with higher DNI beta earn lower expected returns. This is asset pricing with a network-derived factor. It does **not** do edge-level forecasting, link prediction, or lead-lag edges. Use it as a 2024 background citation for "signed stock networks carry pricing information" (relevant to I1, I2, I3). It kills nothing.

---

## I1 — Signed lead-lag edge-state forecasting (temporal signed link prediction)
- Queries: 1) "predicting lead-lag relationships stocks link prediction 2026": only DeltaLag *(known)* and huang2026crossstock *(known)*; opened 2604.19476, which is not edge forecasting. 2) "signed temporal link prediction correlation network stocks forecasting": led to Castilho et al. 2021 (opened). 3) "cryptocurrency lead-lag network edge prediction graph neural network": Inzirillo 2022 (lead-lag graph fusion for return prediction, not edge forecasting). 4) "forecasting correlation network edges dynamic graph stock arXiv 2025": fanshawe2026forecasting *(known)*, re-opened. 5) "signed dynamic graph benchmark financial market stocks positive negative edges": regier2026dynamic and adhikari2026signed *(known)*, no stock benchmark. 6) "forecast future lead-lag network structure foreign exchange commodities": only statistically validated FX lead-lag networks from 2016/2019, which detect but do not forecast edges. 7) "\"lead-lag\" graph \"link prediction\" returns 2026 arXiv": nothing new. 8) OpenReview "lead-lag stock graph": krstev2025temporal (withdrawn, 37 assets) *(known)*, Hermes, CausalStock, DeltaLag. Nothing new that forecasts edges. 9) OpenReview "signed temporal link prediction": 9 records, none with stock, FX or crypto-price data. 10) "dynamic link prediction stock co-movement network temporal GNN 2025": only an IJOC 2022 inductive co-movement paper that predicts returns. 11) "predicting sign changes of stock correlations machine learning": nothing relevant. 12) "cryptocurrency correlation network link prediction temporal 2025": Shukla et al. 2026 (crypto Granger networks, descriptive) and Jing et al. 2025 (crypto correlation-cluster portfolios), both opened, neither forecasts edges. 13) "persistence of lead-lag relationships stock pairs out-of-sample": Fang et al. 2025 (A-share lead-lag screening, in-sample only), opened.
- Closest work found (new):
  - castilho2021forecasting: *Forecasting Financial Market Structure from Network Features using Machine Learning*, https://arxiv.org/abs/2110.11751, Oct 2021 arXiv, [FT via ar5iv]. Explicitly frames future edge existence in filtered **co-movement** networks (6 indices incl. DAX, EUROSTOXX50) as **link prediction**, uses XGBoost with a time-invariant (persistence) benchmark, and evaluates by AUC. This is the closest *task* precedent. It is old ([bkg]), **unsigned, undirected, contemporaneous**, and uses survivor-only constituents.
  - fanshawe2026forecasting *(known, re-opened [FT])*: forecasts 10-day-ahead **signed** pairwise correlations for S&P 500 with a THGNN against a rolling-correlation persistence baseline. This is **edge-weight regression** on **undirected, contemporaneous** edges. There is no lead-lag, no direction, no edge-state classification, and no ML baselines.
  - Crypto, FX, commodities: **no** 2025–2026 paper found that forecasts lead-lag or correlation edges or their signs as link prediction. Shukla et al. 2026 (crypto Granger networks) and Jing et al. 2025 (crypto correlation clusters) are descriptive or portfolio papers. The only signed temporal link-prediction "finance" data is Bitcoin-OTC/Alpha **trust** ratings (regier2026dynamic *(known)*), which are not price-derived.
  - **Signed temporal stock-network benchmark:** none found. TGB/TGB-Seq and the signed-GNN literature use social or trust graphs. adhikari2026signed *(known)* builds daily signed S&P 500 graphs but does not predict them.
- Verdict: **SURVIVED.** The closest works differ in at least two essential dimensions. (a) **Edge semantics:** directed, lagged lead-lag edges with a lag attribute, versus undirected contemporaneous correlation (Castilho, Fanshawe). (b) **Task form:** a 3-state signed edge-state classification (+/−/inactive, plus lag) as temporal signed link prediction with a persistence and triad baseline ladder, versus binary unsigned existence (Castilho) or weight regression (Fanshawe). (c) **Evaluation:** a point-in-time universe and signed dynamic GNNs (none of the found works run these on market data). krstev2025temporal *(known)* is the only direct overlap, and it is withdrawn and tiny (37 assets).
- Sharpened version: "A public, point-in-time *signed directed lead-lag* temporal-graph benchmark (S&P 500, optionally STOXX 600) with statistically validated edges, plus the question *how much of next-window edge state is predictable beyond persistence and balance heuristics*." The negative result ("near persistence") counts as a contribution. Explicitly cite Castilho 2021 and Fanshawe 2026 as the unsigned and undirected predecessors.
- Risk: daily lead-lag edges among large caps are weak and unstable. Expect low base rates and AUC close to persistence. Pre-define edge validation (FDR, shuffled nulls) so the labels are not noise.
- Public data: daily prices via Yahoo/Stooq (survivorship issue). Point-in-time S&P 500 membership can be rebuilt from Wikipedia change tables or public GitHub lists. STOXX 600 point-in-time membership is **not** freely available, which is a blocker for Europe, so start with the US only. WRDS/CRSP via the university would be best if Utrecht has access. Feasibility: **good**. Rolling lead-lag estimation, edge labelling and persistence and triad baselines are simple numpy/pandas. Signed and temporal GNNs exist in PyG / PyTorch Geometric Signed Directed / DyGLib. Roughly 30 weeks is realistic.

## I2 — Structural balance of directed, lagged signed graphs (sign multiplicativity + lag additivity)
- Queries: 1) "structural balance directed signed network lead-lag stocks triads": dahake2026sectoral *(known)*, Ferreira et al. 2021 "Loss of structural balance in stock markets" [bkg]. 2) "lag consistency transitivity lead-lag triangles cycle consistency synchronization": only computer-vision cycle-consistency and oscillator holonomy work, nothing financial. 3) "frustration signed Granger causality network financial markets 2026": dahake2026 *(known)* and Chakrabarti et al. 2019 PRE (frustration precedes crises, undirected correlation), opened. 4) "lead-lag synchronization ranking pairwise lags recover global ordering stocks": Deng & Zhang 2026 (2608.24703, opened) and a Cucuringu 2026 talk abstract (opened). 5) "lagged cross-correlation time delays triangle additivity consistency network test": nothing financial. 6) "balance theory directed weighted signed stock networks motifs 2025": Diaz-Diaz et al. 2025 review and Gallo 2025 PhD thesis (both opened).
- Closest work found (new):
  - deng2026leadlag: *Lead-Lag Relationships in Financial Markets: A Comparison of Multiple Clustering Algorithms*, https://arxiv.org/pdf/2608.24703, 25 Aug 2026, arXiv, [FT]. Builds an antisymmetric DTW lead-lag matrix on CRSP stocks and ranks leaders by row sums. It does **not** test triad or cycle consistency and lists the DTW triangle-inequality issue as future work.
  - gallo2025rethinking: PhD thesis (IMT Lucca, Oct 2025), [ABS]. Signed ERGM null models for balance, including **directed** signed networks and reciprocity–frustration, and including economic networks. This is the right null-model toolkit, but it has no lags and no lead-lag graphs.
  - chakrabarti2019emergence: PRE 2019, [ABS], [bkg]. Loss of balance (frustration) in NYSE correlation networks precedes crises.
  - diazdiaz2025signed: *Signed Networks: theory, methods, and applications* (review, arXiv 2511.17247, v2 Mar 2026), [ABS]. Has no lagged or financial-directed focus.
- Verdict: **WEAKENED.** Balance and frustration of *undirected contemporaneous* stock correlation networks is crowded (Ferreira 2021, Chakrabarti 2019, dahake2026 *(known)*), and signed directed null models exist (Gallo 2025). Still open, with no hit in 6 searches: the **joint** test of sign multiplicativity **and lag additivity** (ℓ_ik ≈ ℓ_ij + ℓ_jk) on directed lagged stock graphs, and using cycle inconsistency to filter spurious lead-lag edges.
- Sharpened version: "Lag-and-sign cycle consistency as a validity filter for lead-lag edges." Show on synthetic lagged factor models (where truth is known) and on S&P 500 data that cycle-inconsistent edges are less persistent out of sample. This works best as a **module inside I1** (balance-triad baseline plus edge filter), not as a standalone thesis.
- Public data: same as I1. Feasibility: **good but thin alone.** Mostly descriptive statistics plus null models. Combine with I1.

## I3 — Edge-event anomaly detection in signed directed lead-lag graphs as a leading stress signal
- Queries: 1) "early warning financial crisis lead-lag network topology changes 2025": Neela 2025 (Systemic Risk Radar, opened), Zhao et al. 2025 IREF (Laplacian-energy-like measure, opened), and older motif-transition work. 2) "anomaly detection dynamic signed graph edge events financial stress indicator": DGraph and TGN-for-fraud (transaction graphs, not markets). 3) "network change point detection stock correlation graph crisis leading indicator arXiv 2026": De Leon Miranda et al. 2026 (DCCA+MST instability, opened) and older change-point work. 4) "edge turnover rewiring rate stock network predicts market stress volatility": Sözen 2025 AIMS Math (multiscale spillover network, opened). 5) "temporal graph anomaly detection stock market network GNN 2026 crash": caputi2026financial *(known, re-opened [FT])*.
- Closest work found (new):
  - neela2025systemic: *Systemic Risk Radar: A Multi-Layer Graph Framework for Early Market Crash Warning*, https://arxiv.org/abs/2512.17185, 19 Dec 2025, arXiv, [ABS]. Correlation-graph features plus snapshot GNNs give earlier crash warnings across dot-com, GFC and COVID. Unsigned, undirected, graph-level features.
  - zhao2025can: *Can we better predict financial crisis? The role of Laplacian-energy-like measure*, IREF 103 (2025), [ABS]. Network-topology measure fed to ML for crisis early warning (China A-shares, US check).
  - deleonmiranda2026detecting: arXiv 2602.10174, 10 Feb 2026, [ABS]. Multiscale MST instability rises in stress (global indices). Coincident or leading status unclear.
  - caputi2026financial *(known)*: graph-level anomaly detection (GlocalKD, TDA) on **directed CCM** graphs of TSX-60. Negative weights are zeroed, so the graph is **unsigned**. Uses a **leading** evaluation window (a hit if an anomaly falls ≤50 business days before a stress-index peak). This is the closest; it already uses a leading-window protocol.
- Verdict: **WEAKENED.** "Network-structure anomalies give early stress warnings" is crowded (Neela 2025, Zhao 2025, Caputi 2026, De Leon Miranda 2026, plus older motif and frustration work). Still open: **signed** and **directed-lagged** edges with **edge-event channels** (formation, dissolution, *sign flip*) and directed signed-triad frustration, tested with a **pre-registered lead-vs-coincident** comparison against standard indicators (OFR FSI/STLFSI, absorption ratio). Caputi's loose 50-day window does not separate lead from coincident.
- Sharpened version: "Do sign flips and frustration in directed lead-lag graphs *lead* stress indices beyond absorption ratio and correlation-graph features?" Use a strict out-of-sample, pre-registered event-study and Granger-type comparison. The detector is a by-product of the I1 pipeline.
- Public data: prices as in I1. Stress indices: OFR FSI and St. Louis Fed STLFSI (FRED, free). VIX from FRED. Feasibility: **good** as an I1 extension. Few crisis events (≈5–8), so statistical power is the main risk. Use many smaller stress episodes.

## I12 — External validation suite for lead-lag / causal stock edges
- Queries: 1) "validate Granger causality stock networks against supply chain customer links": only older tail-Granger and spillover papers, nothing that validates against links. 2) "ground truth evaluation causal discovery financial returns economic links precision": Sadeghi et al. (CD-NOTS, opened) and TimeGraph (opened). 3) "economically linked firms lead-lag return predictability graph learned edges compared 2026": huang2026crossstock *(known)*, Karpman et al. 2022 (opened), Baaijens et al. 2025 (opened). 4) "real-world benchmark time series causal discovery stock market no ground truth proxy validation 2026": CausalTime, CausalDynamics, Sadeghi. 5) "PCMCI transfer entropy equity network compared industry supply chain links": nothing relevant.
- Closest work found (new):
  - sadeghi2023causal: *Causal Discovery in Financial Markets: A Framework for Nonstationary Time-Series Data* (CD-NOTS, Bloomberg), https://arxiv.org/html/2312.17375v3, [FT]. On real data, validation is only plausibility and assumption checks. The paper notes that no ground truth exists. It is a 2023 preprint and has no external economic-link validation.
  - karpman2022learning: *Learning Financial Networks with High-frequency Trade Data*, arXiv 2208.03568, [FT]. Validates edges statistically (bootstrap + BH-FDR) and narratively (Lehman/AIG), **not** against supply-chain or industry links.
  - ferdous2025timegraph: *TimeGraph* (KDD 2025), [FT]. Synthetic ground truth only.
- Verdict: **SURVIVED** (as an evaluation-protocol contribution). No found work scores Granger, PCMCI+, TE or neural-Granger stock edges by precision@k against **independent public economic links** (10-K customer and competitor links, 13F co-ownership, earnings-event information transfer). The dimensions that differ from the closest work are: validation target (external economic links versus synthetic or plausibility checks) and scope (a multi-method comparison on a point-in-time equity universe). Classic finance (customer momentum, Cohen–Frazzini 2008) shows *returns* lead along links. It does not *score discovery algorithms*, so it is background only. kim2026llm and huang2026crossstock *(known)* still need checking against this by the lead agent.
- Sharpened version: "Which time-series causal-discovery method recovers economically documented directed links between firms?" Score precision@k and AUC against 10-K-derived customer→supplier links (direction known), with a degree-matched null. Use earnings announcements as quasi-interventions to test the direction.
- Public data: 10-K customer disclosures (SFAS 131 major customers) via EDGAR full-text or LLM extraction. Compustat Segments is better but needs WRDS. 13F via SEC bulk data. Earnings dates via EDGAR 8-K Item 2.02. Feasibility: **good**. Tigramite (PCMCI+), statsmodels and existing neural-Granger repos make it mostly orchestration. The main work is building link ground truth, which an LLM can help extract.

## I13 — Regime-switching signed causal graphs among sector portfolios
- Queries: 1) "regime-dependent causal discovery sector returns calm stress periods": Lee 2026 (opened) and Luwang et al. 2026 (opened). 2) "Markov switching causal graph time series financial markets 2025 arXiv": Rabel & Runge high-order Markov-switching causal discovery (known as rabel2025context family) and 2512.24621 (EURUSDT, not relevant). 3) "regime-specific spillover network industry portfolios Diebold Yilmaz causal discovery comparison": regime-specific US sector spillovers (Energy Econ 2022, [UNVERIFIED], snippet only) and ASEAN regime-dependent volatility network 2511.19824 [UNVERIFIED]. 4) "Order-Constrained Spectral Causality": Rodriguez Dominguez 2026 (opened).
- Closest work found (new):
  - lee2026regimedependent: *Regime-Dependent Predictive Structure Between Equity Factors: Evidence from Granger Causality*, https://arxiv.org/html/2601.10732v1, Jan 2026, arXiv, [FT]. Uses an HMM with 3 regimes and per-regime pairwise Granger on 6 Fama-French **factors**. Finds HML→SMB only in crises. The graph is unsigned, the regime labels use the full sample (look-ahead), and the units are factors, not sectors.
  - rodriguezdominguez2026order: *Order-Constrained Spectral Causality in Multivariate Time Series*, arXiv 2601.01216, 3 Jan 2026, [ABS]. Reports that directional structure in a multi-asset return panel is episodic and stress-dependent.
  - luwang2026regime: arXiv 2601.08571, Jan 2026, [ABS]. Regime discovery in index returns. Not a causal graph.
- Verdict: **WEAKENED.** Regime-dependent Granger and spillover structure in equities is established (Markov-switching causality in econometrics, regime-specific sector spillovers, Lee 2026 for factors). Still open: **signed** context-specific causal discovery (FANTOM / regime-PCMCI with latent regimes) on **sector or industry portfolios**, with **out-of-sample regime labels** and a head-to-head against rolling Diebold–Yilmaz. This is incremental: a method transfer rather than a new problem.
- Sharpened version: "Does regime-aware causal discovery beat rolling DY spillovers at *forecasting* next-period sector spillovers under stress, with real-time regime filtering?" A leakage-free design is the novelty hook.
- Public data: Kenneth French 49-industry and 12-industry portfolios (free, daily). FRED stress indices. Feasibility: **good technically** (Tigramite has RPCMCI; FANTOM code exists). Scientific novelty is modest and supervisor fit is weaker (less "network-creative").

## I14 — Finance-calibrated synthetic benchmark for time-series causal discovery
- Queries: 1) "synthetic financial time series causal discovery benchmark heavy tails GARCH ground truth DAG": FinStressTS 2026 (opened) and TimeGraph. 2) "causal discovery benchmark finance simulated returns factor model lagged graph 2026": FinCARE *(known)* re-opened and CD-NOTS. 3) "FinCausal benchmark market simulator agent-based causal structure evaluation Granger PCMCI": Thumm & Ontaneda 2025 *Towards Causal Market Simulators* (opened) and an ABM-validation paper from 2024. 4) "causal discovery methods fail on stock returns simulation study non-synchronous trading volatility clustering": only an older "Trading with TS causal discovery" study. 5) OpenReview "causal discovery benchmark financial time series": TCD-Arena (ICLR 2026) *(known)*, CausalRivers, CausalTime, DAG-TFRC (uses S&P 500 as real data). No finance-calibrated benchmark.
- Closest work found (new):
  - sun2026finstressts: *FinStressTS: A Parametric Synthetic Benchmark for Time-Series Forecasting in Finance*, arXiv 2606.03184, Jun 2026, [ABS]. 30 environments with exactly the finance mechanisms I14 wants (volatility clustering, heavy tails, regime switching, self-exciting jumps), but for **forecasting** with no causal graph.
  - thumm2025towards: *Towards Causal Market Simulators* (ICAIF 2025, arXiv 2511.04469), [FT]. Counterfactual generation with 2 Gaussian AR processes. No discovery benchmark.
  - stein2026tcdarena *(known)*: generic assumption-violation stress testing for TCD.
- Verdict: **WEAKENED.** TCD-Arena covers "robustness to assumption violations" generically, and FinStressTS covers finance-calibrated mechanisms for forecasting. The **intersection** is still open: a known lagged, signed, sparse DAG embedded in a market-factor + GARCH + heavy-tail + non-synchronous-close + regime generator, with parameters calibrated to real S&P 500 or sector data. This combination was not found.
- Sharpened version: "FinStressTS-style mechanisms × TCD-Arena-style scoring." Calibrate the generator to real data, then report which causal-discovery methods survive each finance stylised fact. This is most useful as the **synthetic-validation chapter** of I1 or I12 rather than a standalone thesis.
- Public data: none needed beyond calibration prices (Yahoo, French library). Feasibility: **very good** (pure simulation plus Tigramite and existing baselines), but supervisor fit is weaker (benchmark engineering, not network analysis).

## I15 — KG/LLM priors for causal discovery on stock returns
- Queries: 1) "LLM prior knowledge causal discovery stock returns company relations Granger": FinCARE *(known; NeurIPS 2025 workshop, synthetic only)*. 2) "knowledge graph guided causal graph learning financial time series supply chain prior 2026": GregNets (KDD-MLF 2021, opened), TCGformer (PeerJ CS 2026, opened), Zoroddu et al. TMLR 2024 (opened). 3) "language model constrained PCMCI time series causal discovery background knowledge": generic LLM-constrained causal-discovery papers (2024), not financial.
- Closest work found (new):
  - ibrahim2021knowledge: *Knowledge Graph Guided Simultaneous Forecasting and Network Learning for Multivariate Financial Time Series* (GregNets), KDD-MLF workshop 2021, [FT], [bkg]. Uses an EDGAR **co-search** KG as hard or soft masks on a sparse **partial-correlation** network for S&P 500/1500 volatility residuals. This is "KG prior for stock network learning", but for undirected, non-lagged, non-causal networks, with an older KG type.
  - zoroddu2024learning: *Learning Network Granger Causality Using Graph Prior Knowledge*, TMLR 2024, [ABS], [bkg]. A generic method: a noisy undirected graph as prior for Granger graphs. Real datasets are not named on the record page.
  - tian2026tcgformer: *TCGformer*, PeerJ CS 2026, [ABS]. Variable-lag Granger graph among institutions feeds a forecaster. No KG prior.
- Verdict: **WEAKENED.** "KG as prior or mask for a stock network" exists (GregNets 2021). "Graph prior for Granger" exists as a generic method (Zoroddu 2024). "KG+LLM-augmented causal discovery in finance" exists on synthetic data (FinCARE 2025). Still open: an **LLM-built, point-in-time company KG** (10-K relations) as soft prior for **lagged** causal discovery (PCMCI+ / neural Granger) on **real** returns, evaluated by **out-of-sample edge stability and predictive value**, with a check that the KG does not leak future knowledge (links to I9).
- Sharpened version: "Do text-derived relation priors make lagged causal stock graphs more stable and predictive out of sample, or do they only import the LLM's hindsight?" Compare a point-in-time KG against an LLM parametric KG.
- Public data: EDGAR 10-K text, prices. Feasibility: **moderate**. LLM extraction cost and entity resolution are the bottlenecks, and the evaluation design is subtle.

## I18 — US→Europe cross-market lead-lag bipartite graph
- Queries: 1) "US stocks lead European stocks next day firm-level cross-market predictability network": de Gier 2017 EUR MSc thesis (opened), Grüner & Finke 2015 (opened), liu2026bipartite *(known)*. 2) "cross-market lead-lag graph neural network stock prediction overlapping trading hours 2026": liu2026bipartite *(known, re-opened [ABS])* and DeltaLag. 3) "transatlantic return spillover individual stocks S&P 500 STOXX 600 overnight": Jiao 2025 *Foreign Signal Radar* (opened). 4) "international economic links US firms predict foreign stock returns cross-border customer peers": CrossAlpha *(known)*, Pyun & Sulaeman 2021 (opened), Albuquerque et al. (trade credit, [UNVERIFIED]).
- Closest work found (new):
  - jiao2025foreign: *Foreign Signal Radar*, https://arxiv.org/pdf/2504.07855, version 20 Mar 2025, [FT]. Uses lagged returns of foreign markets and stocks across 47 markets to predict individual **US** (S&P 500) stock returns, with stock-specific, time-varying links learned by >100k ML models. The **direction is opposite** (foreign→US), with no explicit graph and no Europe focus.
  - pyun2021crossborder: *Cross-Border Trade Competition and International Stock Return Correlations*, NUS RMI WP 2021-07, [FT], [bkg]. Delayed reaction of foreign *indices* (incl. EU) to US shocks via trade-competition links. Index level, not stocks.
  - liu2026bipartite *(known)*: US→China directed bipartite stock graph with rolling-test edges and **non-overlapping** hours.
  - gruner2015leading, degier2017europe: index-level US leadership [bkg].
- Verdict: **WEAKENED.** The method (rolling-tested directed bipartite cross-market stock graph) is done for US→China (Liu et al. 2026), and stock-level foreign↔US predictability is done in the reverse direction (Jiao 2025). Still open: **US→Europe at stock level with partially overlapping trading hours**. The lead-lag must be defined on intraday-aligned windows (Europe's post-US-open session versus the US close), and a **zero-shot transfer** of a US text-derived relation graph to European firms.
- Sharpened version: "Does the overlap window absorb US→EU information so that the next-day lead-lag edges seen for China vanish for Europe? Do text-matched US–EU peer edges explain which survive?" A clean natural-experiment contrast to Liu et al.
- Public data: **blocker**. Free intraday European stock data and point-in-time STOXX 600 constituents are not available. Daily open and close from Yahoo is possible but coarse for the overlap question. The AEX/EURO STOXX 50 subset is feasible. Feasibility: **moderate-to-risky** (data alignment across exchanges, holidays, FX conversion).

---

## Summary table

| Idea | Verdict | Key reason |
| --- | --- | --- |
| I1 | SURVIVED | Only predecessors are unsigned or undirected (Castilho 2021 link prediction; Fanshawe 2026 signed-correlation regression). No signed directed lead-lag edge-state forecasting or signed temporal stock benchmark found, including crypto, FX and commodities. |
| I2 | WEAKENED | Balance on correlation nets is crowded; joint sign × lag-additivity cycle test on lead-lag graphs not found. Best as an I1 module. |
| I3 | WEAKENED | Network early-warning is crowded (Neela 2025, Zhao 2025, Caputi 2026). Signed-directed edge-event channels plus a strict lead-vs-coincident test remain open. |
| I12 | SURVIVED | No external-link (10-K/13F/earnings) precision@k validation of causal-discovery stock edges found. |
| I13 | WEAKENED | Regime-dependent Granger in equities is established (Lee 2026 factors). Signed context-specific discovery on sectors with real-time regimes is incremental. |
| I14 | WEAKENED | TCD-Arena (generic) and FinStressTS (finance, forecasting only). The intersection is open but works best as a validation chapter. |
| I15 | WEAKENED | GregNets 2021 (KG mask, stock networks), Zoroddu 2024 (graph-prior Granger), FinCARE (synthetic). Real-data, point-in-time LLM-KG prior for lagged discovery is open. |
| I18 | WEAKENED | Liu et al. 2026 (US→China bipartite) and Jiao 2025 (foreign→US stock-level). US→EU with overlapping hours is open but data-blocked. |

## BibTeX (new papers only)

```bibtex
@article{uddin2024network,
  title={The Network Factor of Equity Pricing: A Signed Graph Laplacian Approach},
  author={Uddin, Ajim and Tao, Xinyuan and Yu, Dantong},
  journal={Journal of Financial Econometrics}, volume={22}, number={5}, pages={1616--1655}, year={2024},
  url={https://digitalcommons.njit.edu/fac_pubs/795},
  note={[ABS] 2024 (repository online 29 Apr 2025); J. Financial Econometrics; DOI 10.1093/jjfinec/nbae010}
}
@misc{castilho2021forecasting,
  title={Forecasting Financial Market Structure from Network Features using Machine Learning},
  author={Castilho, Douglas and Souza, Th{\'a}rsis T. P. and Kang, Soong Moon and Gama, Jo{\~a}o and de Carvalho, Andr{\'e} C. P. L. F.},
  year={2021}, url={https://arxiv.org/abs/2110.11751},
  note={[FT] Oct 2021; arXiv preprint; background}
}
@misc{jing2025optimising,
  title={Optimising cryptocurrency portfolios through stable clustering of price correlation networks},
  author={Jing, Ruixue and Kobayashi, Ryota and Rocha, Luis Enrique Correa},
  year={2025}, url={https://arxiv.org/abs/2505.24831},
  note={[ABS] 30 May 2025 (v2 29 Apr 2026); arXiv}
}
@misc{shukla2026timedependent,
  title={Time-dependent weighted directed networks of cryptocurrency interaction from high-frequency returns},
  author={Shukla, Shubhangam and Peyyala, Mahesh and Chakraborty, Abhijit},
  year={2026}, url={https://ideas.repec.org/p/arx/papers/2606.25466.html},
  note={[ABS] Jun 2026 (rev. Aug 2026); arXiv 2606.25466}
}
@misc{fang2025from,
  title={From Data Acquisition to Lag Modeling: Quantitative Exploration of A-Share Market with Low-Coupling System Design},
  author={Fang, Jianyong and Wu, Sitong and Tong, Junfan},
  year={2025}, url={https://arxiv.org/html/2506.19255v1},
  note={[FT] 29 May 2025; arXiv}
}
@misc{inzirillo2022deep,
  title={Deep Fusion of Lead-lag Graphs: Application to Cryptocurrencies},
  author={Inzirillo, Hugo and others},
  year={2022}, url={https://arxiv.org/abs/2201.02040v2},
  note={[ABS] 5 Jan 2022; arXiv; background}
}
@misc{deng2026leadlag,
  title={Lead-Lag Relationships in Financial Markets: A Comparison of Multiple Clustering Algorithms},
  author={Deng, Ruichen and Zhang, Yichi},
  year={2026}, url={https://arxiv.org/pdf/2608.24703},
  note={[FT] 25 Aug 2026; arXiv}
}
@phdthesis{gallo2025rethinking,
  title={Rethinking balance theory: a probabilistic approach to the study of signed networks},
  author={Gallo, Anna}, school={IMT School for Advanced Studies Lucca}, year={2025},
  url={https://iris.imtlucca.it/handle/20.500.11771/37238},
  note={[ABS] defended 20 Oct 2025; PhD thesis}
}
@misc{diazdiaz2025signed,
  title={Signed Networks: theory, methods, and applications},
  author={Diaz-Diaz, Fernando and Candellone, Elena and Gonzalez-Casado, Miguel A. and Fraxanet, Emma and Vendeville, Antoine and Ferri, Irene and Teixeira, Andreia Sofia},
  year={2025}, url={https://arxiv.org/abs/2511.17247v1},
  note={[ABS] 21 Nov 2025 (v2 13 Mar 2026); arXiv review}
}
@article{chakrabarti2019emergence,
  title={Emergence of frustration signals systemic risk},
  author={Chakrabarti, Anindya S. and Kuyyamudi, Chandrashekar and Sinha, Sitabhra},
  journal={Physical Review E}, volume={99}, pages={052306}, year={2019},
  url={https://arxivlens.com/PaperView/Details/emergence-of-frustration-signals-systemic-risk-5405-2357682},
  note={[ABS] 29 May 2019; PRE; arXiv 1807.02923; background}
}
@misc{cucuringu2026signed,
  title={Signed and Directed Graph Clustering in Financial Time Series: Statistical Arbitrage, Lead-Lag Structure, and the Market Tug-of-War (seminar abstract)},
  author={Cucuringu, Mihai}, year={2026},
  url={https://www.math.colostate.edu/~king/codex/abstracts/cucuringu2026.html},
  note={[ABS] talk abstract, 2026 (date not stated); CodEx seminar}
}
@misc{neela2025systemic,
  title={Systemic Risk Radar: A Multi-Layer Graph Framework for Early Market Crash Warning},
  author={Neela, Sandeep}, year={2025}, url={https://arxiv.org/abs/2512.17185},
  note={[ABS] 19 Dec 2025; arXiv}
}
@article{zhao2025can,
  title={Can we better predict financial crisis? The role of Laplacian-energy-like measure},
  author={Zhao, Xian and Huang, Chuangxia and Yang, Xiaoguang and Cao, Jie and Yang, Xin},
  journal={International Review of Economics \& Finance}, volume={103}, year={2025},
  url={https://ideas.repec.org/a/eee/reveco/v103y2025ics1059056025005593.html},
  note={[ABS] 2025; IREF}
}
@misc{deleonmiranda2026detecting,
  title={Detecting Network Instability via Multiscale Detrended Cross-Correlations and MST Topology},
  author={De Leon Miranda, Jose and Dolfin, Marina and Kapetanios, George and Leonida, Leone},
  year={2026}, url={https://arxiv.org/abs/2602.10174},
  note={[ABS] 10 Feb 2026; arXiv}
}
@article{sozen2025stressaware,
  title={Stress-aware multiscale spillover networks: Cross-market transmission via coherence--entropy centrality},
  author={S{\"o}zen, {\c{C}}a{\u{g}}lar}, journal={AIMS Mathematics}, volume={10}, number={11}, pages={26313--26333}, year={2025},
  url={https://aimspress.com/article/doi/10.3934/math.20251157},
  note={[ABS] 14 Nov 2025; AIMS Mathematics}
}
@misc{karpman2022learning,
  title={Learning Financial Networks with High-frequency Trade Data},
  author={Karpman, Kara and Basu, Sumanta and Easley, David},
  year={2022}, url={https://arxiv.org/pdf/2208.03568},
  note={[FT] Aug 2022; arXiv; background}
}
@inproceedings{baaijens2025harnessing,
  title={Harnessing firm similarities with graph neural networks for superior stock predictions},
  author={Baaijens, Tein and G{\"u}ven, {\c{C}}i{\c{c}}ek and N{\'a}poles, Gonzalo},
  year={2025}, url={https://research.tilburguniversity.edu/en/publications/harnessing-firm-similarities-with-graph-neural-networks-for-super/},
  note={[ABS] 11 Apr 2025; Complex Networks 2024 proceedings}
}
@misc{sadeghi2023causal,
  title={Causal Discovery in Financial Markets: A Framework for Nonstationary Time-Series Data},
  author={Sadeghi, Agathe and Gopal, Achintya and Fesanghary, Mohammad},
  year={2023}, url={https://arxiv.org/html/2312.17375v3},
  note={[FT] Dec 2023 (v3 later); arXiv; background}
}
@inproceedings{ferdous2025timegraph,
  title={TimeGraph: Synthetic Benchmark Datasets for Robust Time-Series Causal Discovery},
  author={Ferdous, Muhammad Hasan and Hossain, Emam and Gani, Md Osman},
  booktitle={KDD}, year={2025}, url={https://arxiv.org/html/2506.01361v1},
  note={[FT] Jun 2025; KDD 2025}
}
@misc{lee2026regimedependent,
  title={Regime-Dependent Predictive Structure Between Equity Factors: Evidence from Granger Causality},
  author={Lee, Chorok}, year={2026}, url={https://arxiv.org/html/2601.10732v1},
  note={[FT] Jan 2026; arXiv}
}
@misc{luwang2026regime,
  title={Regime Discovery and Intra-Regime Return Dynamics in Global Equity Markets},
  author={Luwang, Salam Rabindrajit and Sharma, Buddha Nath and Mukhia, Kundan and Nurujjaman, Md. and Rai, Anish and Petroni, Filippo and Rocha, Luis E. C.},
  year={2026}, url={https://arxiv.org/abs/2601.08571v1},
  note={[ABS] 13 Jan 2026; arXiv}
}
@misc{rodriguezdominguez2026order,
  title={Order-Constrained Spectral Causality in Multivariate Time Series},
  author={Rodriguez Dominguez, Alejandro}, year={2026}, url={https://arxiv.org/abs/2601.01216v1},
  note={[ABS] 3 Jan 2026; arXiv}
}
@misc{sun2026finstressts,
  title={FinStressTS: A Parametric Synthetic Benchmark for Time-Series Forecasting in Finance},
  author={Sun, Jiaze and Koa, Kelvin J. L. and Ni, Ruiyang and Liu, Yize and Chen, Haonan and Huang, Ke-Wei},
  year={2026}, url={https://ideas.repec.org/p/arx/papers/2606.03184.html},
  note={[ABS] Jun 2026; arXiv 2606.03184}
}
@inproceedings{thumm2025towards,
  title={Towards Causal Market Simulators},
  author={Thumm, Dennis and Ontaneda Mijares, Luis},
  booktitle={ICAIF}, year={2025}, url={https://arxiv.org/pdf/2511.04469v4},
  note={[FT] Nov 2025; ICAIF 2025}
}
@inproceedings{ibrahim2021knowledge,
  title={Knowledge Graph Guided Simultaneous Forecasting and Network Learning for Multivariate Financial Time Series},
  author={Ibrahim, Shibal and Chen, Wenyu and Zhu, Yada and Chen, Pin-Yu and Zhang, Yang and Mazumder, Rahul},
  booktitle={KDD Workshop on Machine Learning in Finance}, year={2021},
  url={https://www.mit.edu/~rahulmaz/Knowledge%20Graph%20Guided%20Simultaneous%20Forecasting%20and%20Network%20Learning.pdf},
  note={[FT] Aug 2021; KDD-MLF workshop; background}
}
@article{zoroddu2024learning,
  title={Learning Network Granger Causality Using Graph Prior Knowledge},
  author={Zoroddu, Lucas and Humbert, Pierre and Oudre, Laurent},
  journal={Transactions on Machine Learning Research}, year={2024},
  url={https://mlanthology.org/tmlr/2024/zoroddu2024tmlr-learning},
  note={[ABS] 2024; TMLR; background}
}
@article{tian2026tcgformer,
  title={TCGformer: a temporal causal graph enhanced transformer model for multivariate financial time series forecasting},
  author={Tian, Sihua and Li, Shaofang}, journal={PeerJ Computer Science}, year={2026},
  url={https://www.citedrive.com/en/discovery/tcgformer-a-temporal-causal-graph-enhanced-transformer-model-for-multivariate-financial-time-series-forecasting/},
  note={[ABS] 2026; PeerJ CS e3959}
}
@misc{jiao2025foreign,
  title={Foreign Signal Radar}, author={Jiao, Wei}, year={2025},
  url={https://arxiv.org/pdf/2504.07855},
  note={[FT] version 20 Mar 2025; arXiv}
}
@techreport{pyun2021crossborder,
  title={Cross-Border Trade Competition and International Stock Return Correlations},
  author={Pyun, Sungjune and Sulaeman, Johan}, institution={NUS RMI}, number={2021-07}, year={2021},
  url={https://rmi.nus.edu.sg/wp-content/uploads/2021/04/RMI-WPS-2021-07.pdf},
  note={[FT] Apr 2021; working paper; background}
}
@techreport{gruner2015leading,
  title={The Leading Role of the United States and U.S. Stock Market Components},
  author={Gr{\"u}ner, Andreas and Finke, Christian}, year={2015},
  url={https://www.alexandria.unisg.ch/handle/20.500.14171/107161},
  note={[ABS] 2015; working paper; background}
}
@mastersthesis{degier2017europe,
  title={Europe's stock return predictability}, author={de Gier, Jason}, school={Erasmus University Rotterdam}, year={2017},
  url={https://thesis.eur.nl/pub/38531/},
  note={[ABS] 2017; MSc thesis; background}
}
```

## Query log

| # | Idea | Query / source | Outcome |
| --- | --- | --- | --- |
| 1 | req. | "Network Factor of Equity Pricing Signed Graph Laplacian" | Found and opened NJIT record (uddin2024network) |
| 2 | I1 | "predicting lead-lag relationships stocks link prediction 2026" | DeltaLag (known), huang2026crossstock (known, opened: not LP) |
| 3 | I1 | "signed temporal link prediction correlation network stocks forecasting" | Castilho 2021 (opened) |
| 4 | I1 | "cryptocurrency lead-lag network edge prediction graph neural network" | Inzirillo 2022 (opened), no edge forecasting |
| 5 | I1 | "forecasting correlation network edges dynamic graph stock arXiv 2025" | fanshawe (known, re-opened) |
| 6 | I1 | "signed dynamic graph benchmark financial market stocks positive negative edges" | regier, adhikari (known, re-opened); no stock benchmark |
| 7 | I1 | "forecast future lead-lag network structure foreign exchange commodities" | only 2016/2019 FX validated-network detection |
| 8 | I1 | "\"lead-lag\" graph \"link prediction\" returns 2026 arXiv" | nothing new |
| 9 | I1 | OpenReview "lead-lag stock graph" | krstev (known), Hermes, CausalStock, DeltaLag |
| 10 | I1 | OpenReview "signed temporal link prediction" | 9 records, no finance data |
| 11 | I1 | "dynamic link prediction stock co-movement network temporal graph neural network 2025" | IJOC 2022 return prediction only |
| 12 | I1 | "predicting sign changes of stock correlations machine learning" | irrelevant |
| 13 | I1 | "cryptocurrency correlation network link prediction temporal 2025" | Shukla 2026, Jing 2025 (opened; descriptive) |
| 14 | I1 | "persistence of lead-lag relationships stock pairs out-of-sample predictability 2025" | Fang 2025 (opened; in-sample) |
| 15 | I2 | "structural balance directed signed network lead-lag stocks triads" | dahake2026 (known), Ferreira 2021 [bkg] |
| 16 | I2 | "lag consistency transitivity lead-lag triangles cycle consistency synchronization" | non-financial only |
| 17 | I2 | "frustration signed Granger causality network financial markets 2026" | Chakrabarti 2019 (opened) |
| 18 | I2 | "lead-lag synchronization ranking pairwise lags recover global ordering stocks" | Deng & Zhang 2026, Cucuringu talk (opened) |
| 19 | I2 | "lagged cross-correlation time delays triangle additivity consistency network test" | nothing financial |
| 20 | I2 | "balance theory directed weighted signed stock networks motifs 2025" | Diaz-Diaz review, Gallo thesis (opened) |
| 21 | I3 | "early warning financial crisis lead-lag network topology changes 2025" | Neela 2025, Zhao 2025 (opened) |
| 22 | I3 | "anomaly detection dynamic signed graph edge events financial stress indicator" | transaction-fraud graphs only |
| 23 | I3 | "network change point detection stock correlation graph crisis leading indicator arXiv 2026" | De Leon Miranda 2026 (opened) |
| 24 | I3 | "edge turnover rewiring rate stock network predicts market stress volatility" | Sözen 2025 (opened) |
| 25 | I3 | "temporal graph anomaly detection stock market network GNN 2026 crash" | caputi2026 (known, re-opened) |
| 26 | I12 | "validate Granger causality stock networks against supply chain customer links" | none |
| 27 | I12 | "ground truth evaluation causal discovery financial returns economic links precision" | CD-NOTS, TimeGraph (opened) |
| 28 | I12 | "economically linked firms lead-lag return predictability graph learned edges compared 2026" | Karpman 2022, Baaijens 2025 (opened) |
| 29 | I12 | "real-world benchmark time series causal discovery stock market no ground truth proxy validation 2026" | CausalTime/CausalDynamics (generic) |
| 30 | I12 | "PCMCI transfer entropy equity network compared industry supply chain links" | none |
| 31 | I13 | "regime-dependent causal discovery sector returns calm stress periods" | Lee 2026, Luwang 2026 (opened) |
| 32 | I13 | "Markov switching causal graph time series financial markets 2025 arXiv" | Rabel & Runge (known family); 2512.24621 irrelevant |
| 33 | I13 | "regime-specific spillover network industry portfolios Diebold Yilmaz causal discovery comparison" | Energy Econ 2022 sectors [UNVERIFIED] |
| 34 | I13 | "\"Order-Constrained Spectral Causality\" multivariate time series" | Rodriguez Dominguez 2026 (opened) |
| 35 | I14 | "synthetic financial time series causal discovery benchmark heavy tails GARCH ground truth DAG" | FinStressTS 2026 (opened) |
| 36 | I14 | "causal discovery benchmark finance simulated returns factor model lagged graph 2026" | FinCARE (known, re-opened) |
| 37 | I14 | "FinCausal benchmark market simulator agent-based causal structure evaluation Granger PCMCI" | Thumm 2025 (opened) |
| 38 | I14 | "causal discovery methods fail on stock returns simulation study non-synchronous trading volatility clustering" | nothing new |
| 39 | I14 | OpenReview "causal discovery benchmark financial time series" | TCD-Arena (known), CausalRivers, CausalTime; no finance benchmark |
| 40 | I15 | "LLM prior knowledge causal discovery stock returns company relations Granger" | FinCARE only |
| 41 | I15 | "knowledge graph guided causal graph learning financial time series supply chain prior 2026" | GregNets 2021, TCGformer 2026, Zoroddu 2024 (opened) |
| 42 | I15 | "language model constrained PCMCI time series causal discovery background knowledge" | generic, non-financial |
| 43 | I18 | "US stocks lead European stocks next day firm-level cross-market predictability network" | de Gier 2017, Grüner & Finke 2015 (opened) |
| 44 | I18 | "cross-market lead-lag graph neural network stock prediction overlapping trading hours 2026" | liu2026bipartite (known, re-opened) |
| 45 | I18 | "transatlantic return spillover individual stocks S&P 500 STOXX 600 overnight" | Jiao 2025 Foreign Signal Radar (opened) |
| 46 | I18 | "international economic links US firms predict foreign stock returns cross-border customer peers" | Pyun & Sulaeman 2021 (opened), CrossAlpha (known) |
