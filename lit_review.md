# Literature review — 2025–2026 landscape (stock-market and company networks)

*Topic-scouting run, 1–2 Nov 2026. Supervisor: Dr. I. Karnstedt-Hulpuș (UU). See `topic_shortlist.md` for the candidate topics.*

**Legend:** **[FT]** full text opened · **[ABS]** abstract/record page only · **[UNVERIFIED]** seen but not opened (only in `sweep/T*.md` §B, never relied on) · **[BACKGROUND]** pre-2025 work, context only. Year = first public date (arXiv v1 or venue, whichever is earlier). Every number quoted from a paper must be re-checked against the PDF before it goes into the thesis.

## How this review was made

- Eight parallel theme sweeps (T1–T8), each by a separate subagent under the binding rules in `sweep/RULES.md`, plus early kill-test notes by the lead agent (T0).
- Discovery: WebSearch with Scholar-style queries, arXiv monthly listings (q-fin.ST, q-fin.CP), the OpenReview API (ICLR 2026, NeurIPS 2025) and papers.cool venue pages. Reading: arXiv abstract/HTML/PDF and publisher pages. **Google Scholar, DBLP, Semantic Scholar and ACM DL were blocked** (NOTES.md §0.1).
- A shared WebSearch budget ran out partway through several sweeps (T1, T4, T5, T6, T7). After that the agents continued through arXiv listings, the OpenReview API and reference snowballing. Coverage of AAAI/IJCAI/KDD 2026 proceedings and of journals (ESWA, KBS, Physica A) is therefore **incomplete**; crowding counts are lower bounds.
- Query logs per theme: `sweep/T*.md` §F (summarised in NOTES.md §1).

## Counts

| Theme | Main-list papers | 2026 | 2025 | Older (background) | Full text read |
| --- | ---: | ---: | ---: | ---: | ---: |
| T1 Signed / lead-lag | 16 | 9 | 5 | 0 (+2 bkg in text) | 8 |
| T2 Link prediction | 16 | 4 | 9 | 2 | 8 |
| T3 Anomaly detection | 14 | 8 | 6 | 0 | 9 |
| T4 Heterogeneous / KG | 29 | 12 | 15 | 1 | 8 |
| T5 Time series + graph | 14 | 9 | 5 | 0 | 10 |
| T6 Causal | 14 | 9 | 5 | 0 | 7 |
| T7 Credit → equity | 14 | 9 | 3 | 2 | 10 |
| T8 Wildcards | 38 | 20 | 9 | 1 | 10 |
| T0 Lead agent | 3 | 2 | 0 | 1 | 2 |
| **Total main-list entries** | **158** | **82 (52%)** | **57 (36%)** | **7 (4%)** | **72** |

Some papers appear under several themes, so the BibTeX file (`references.bib`) holds about 142 unique main-list works. The sweep files' §A2 short entries add about 90 further opened papers that count toward the crowding estimates. Counts come from the bibkey year, which is the first-public year. Targets were 60–100 opened papers, ≥ 75% from 2025–2026 and ≥ 40% from 2026; all are met (88% 2025–2026, 52% 2026).


---

## T1. Signed, time-activated influence graphs between stocks

**Synthesis.** Each ingredient of the supervisor's "signed, time-activated influence edges" is active in 2025–2026, but nobody has combined them. (i) Lead-lag learning for stocks: DeltaLag (ICAIF 2025) learns per-pair lags but its edges are unsigned. A withdrawn ICLR 2026 submission recasts lead-lag detection as temporal link prediction, but only on 37 assets with ad-hoc labels. (ii) Structural balance in signed *correlation* networks: at least 5 descriptive papers, all on undirected same-day correlations, and the indices are coincident with crises, not leading (acedo2026triadic). (iii) Dynamic signed link prediction: three 2026 methods with code (hou2026inductive, regier2026dynamic) that predict positive/negative/absent future edges, tested only on Bitcoin trust and Wikipedia networks. There is **no public benchmark of signed, time-varying stock networks**. Verdict: **thin at the intersection** — the most direct hit on the supervisor's capitalised point.

Full per-paper notes (from `sweep/T1.md`; raw query log and unopened leads stay there):

##### dahake2026sectoral — Sectoral inter-dependencies drive the loss of structural balance in signed financial networks
- Citation: Dahake, K., Chakraborty, A., *Sectoral inter-dependencies drive the loss of structural balance in signed financial networks*, arXiv:2608.12023, 2026.
- Link: https://arxiv.org/html/2608.12023 (abs: https://arxiv.org/abs/2608.12023)
- First public: arXiv v1 2026-08-12; Venue: preprint (physics.soc-ph, q-fin.GN, q-fin.RM); Peer-reviewed: no
- Verification: [FT]
- Summary: The authors build signed S&P 500 networks for 56 overlapping 252-day windows (2010–2024, 426 stocks, Yahoo Finance). The steps are: remove the market mode with RMT, keep the "group-mode" correlations, and set a ±1 edge only when the value is beyond μ ± 3σ of the random-matrix bulk. A triad polarization index P = (balanced − frustrated)/(all triads) is split into intra-sector and inter-sector parts. Balance is P_G = 1 in 2010–2015 and drops sharply during COVID-19. The loss of balance comes mainly from inter-sector triads (e.g. Real Estate–Communication Services, Healthcare–Energy). A regression of P_G on supply-chain pressure (GSCPI) and CPI uncertainty gives R² ≈ 0.68.
- Graph type: temporal snapshots (rolling windows); homogeneous (stocks) with sector labels; signed; undirected; contemporaneous (no lags).
- Task: other (descriptive structural analysis; null-model significance; macro regression).
- Data: S&P 500 daily, Yahoo Finance (public); FRED/NY Fed macro series (public).
- Code available? Not stated for the main pipeline. Uses the public `null_model_graphlet` reference implementation (Hao & Kovács 2024).
- What it leaves open: Edges are undirected and contemporaneous, so lead-lag is not studied. Windows are coarse (252 days with 63-day overlap), giving only 56 snapshots. The constituent set and static GICS map suggest survivorship bias. The paper is purely descriptive: it has no prediction of balance changes or of individual edge sign flips.

##### fanshawe2026forecasting — Forecasting Equity Correlations with Hybrid Transformer Graph Neural Network
- Citation: Fanshawe, J., Masih, R., Cameron, A., *Forecasting Equity Correlations with Hybrid Transformer Graph Neural Network*, arXiv:2601.04602, 2026.
- Link: https://arxiv.org/html/2601.04602v1 (abs: https://arxiv.org/abs/2601.04602)
- First public: arXiv v1 2026-01-08; Venue: preprint (q-fin.CP, q-fin.TR); Peer-reviewed: no
- Verification: [FT]
- Summary: The model forecasts 10-day-ahead pairwise stock correlations for S&P 500 constituents in Fisher-z space. A Transformer temporal encoder feeds an edge-aware GAT on a signed, weighted graph per date. The graph uses each stock's top-50 and bottom-50 partners plus 75 random mid-strength partners, and edges are routed to separate negative, neutral and positive expert heads. The model predicts residuals over a rolling-correlation baseline: MAE drops from 0.307 to 0.230 and Pearson r rises from 0.31 to 0.78 (OOS 2019–2024). The forecasts feed SPONGEsym signed clustering for basket trading.
- Graph type: temporal (daily snapshots); signed weighted edges; undirected; edge attributes include sector flags.
- Task: regression (edge-weight / correlation forecasting) → clustering → trading.
- Data: CRSP/Compustat via WRDS (not public) + FRED (public).
- Code available? Not stated.
- What it leaves open: The authors motivate the work with correlations that "flip their sign" but never evaluate sign-flip prediction. The edges are contemporaneous and undirected (no lead-lag). The only baseline is persistence, with no other GNNs, and there are no transaction costs. The text has several internal inconsistencies (window length, dates, Sharpe values). Data is WRDS-only.

##### adhikari2026signed — Signed network models for dimensionality reduction of portfolio optimization
- Citation: Adhikari, B., *Signed network models for dimensionality reduction of portfolio optimization*, arXiv:2602.21362, 2026 (extension of arXiv:2510.05377, 2025).
- Link: https://arxiv.org/html/2602.21362v1 (abs: https://arxiv.org/abs/2602.21362)
- First public: arXiv v1 2026-02-24; Venue: preprint; Peer-reviewed: no
- Verification: [FT]
- Summary: The paper builds one complete signed graph per trading day. The sign of edge (i,j) is the sign of the product of the two assets' return deviations from their means, so every edge is activated with a sign each day. Skewness is linked to balanced triangles and kurtosis to balanced 4-clique (K4^B2) patterns. Maximising the K4^B2 density over a subset is NP-hard. A "combinatorial hedge score" (how often an asset forms negative edges) selects a reduced universe. Backtests (199 S&P 500 assets, 2006–2021, Yahoo/Kaggle, annual train/test) show higher return and higher volatility, with mixed Sharpe ratios.
- Graph type: temporal (daily snapshots); signed; undirected; unweighted.
- Task: other (asset selection / portfolio optimisation).
- Data: public (Yahoo Finance via Kaggle).
- Code available? Not stated.
- What it leaves open: Theorem 3.4 shows every daily graph is balanced by construction, so frustration carries no information under this edge definition. Lagged (lead-lag) signs are not considered. There are no transaction costs, there is survivorship bias (a fixed 199 assets), and K is chosen ad hoc. The author notes that motif-density problems on signed graphs are "largely unexplored".

##### acedo2026triadic — The Triadic Stress Index in Financial Markets
- Citation: Acedo, A., *The Triadic Stress Index in Financial Markets*, arXiv:2608.10788, 2026.
- Link: https://arxiv.org/abs/2608.10788
- First public: arXiv v1 2026-08-11; Venue: preprint (physics.soc-ph, q-fin.RM, q-fin.ST); Peer-reviewed: no
- Verification: [ABS]
- Summary: The paper takes a triad-based index from microbiome co-occurrence networks and applies it unchanged to asset correlation networks. It covers five markets from 2006 to 2026: equities, crypto, commodities, FX and sovereign debt. The index is benchmarked against the Absorption Ratio, effective rank, the Vendi score, Ollivier-Ricci curvature, and the global/local balance indices of signed correlation networks, using crisis labels, a fixed alarm budget, OOS testing, block bootstrap and a Holm correction. The TSI beats the Absorption Ratio but ties with effective rank and Vendi. A lead-lag analysis puts peak cross-correlation at zero lag, so the TSI is a coincident indicator, not a forecaster.
- Graph type: temporal (rolling); undirected correlation network. The abstract does not say whether the TSI keeps edge signs.
- Task: anomaly detection / crisis early-warning (alarm evaluation) + node attribution.
- Data: multi-asset, 2006–2026; sources not stated in abstract.
- Code available? The arXiv comments point to the GitHub repo BiomeMakers/TSI-OmegaS. I could not open it (permission not granted).
- What it leaves open: This is the most rigorous evaluation protocol in this cluster (alarm budget, Holm, bootstrap), and it shows that triad/balance-type indices are coincident with crises, not leading. Whether *directed, lagged* signed structure gives a leading signal is untested.

##### deng2026leadlag — Lead-Lag Relationships in Financial Markets: A Comparison of Multiple Clustering Algorithms
- Citation: Deng, R., Zhang, Y., *Lead-Lag Relationships in Financial Markets: A Comparison of Multiple Clustering Algorithms*, arXiv:2608.24703, 2026.
- Link: https://arxiv.org/abs/2608.24703
- First public: arXiv v1 2026-08-25; Venue: preprint (q-fin.ST); Peer-reviewed: no
- Verification: [ABS]
- Summary: The paper compares DTW-KMedoids (the established lead-lag clustering baseline) with MiniRocket-KMeans, KShape and a KShape+DTW ensemble on synthetic and real stock data, using the same lead-lag trading strategy and silhouette-chosen cluster numbers. MiniRocket-KMeans gives the best Sharpe (0.866) but a −63.9% max drawdown, and the ensemble is the most stable.
- Graph type: none explicit (clustering of time series; implicit leader/lagger groups).
- Task: clustering → trading.
- Data: synthetic + real stock data (market not stated in abstract).
- Code available? Not stated.
- What it leaves open: Lead-lag is treated as a static clustering per period. The paper does not handle sign (inverse followers), edge-level activation over time, or a network formulation. The reported p-values of 0.0 and the −64% drawdown suggest weak evaluation.

##### zhang2026spatialtemporal — Spatial-temporal dynamics for enhanced temporal link prediction in stock market crisis forecasting (CF2TLP)
- Citation: Zhang, R., Wei, W., Wu, F., et al., *Spatial-temporal dynamics for enhanced temporal link prediction in stock market crisis forecasting*, IEEE SMC 2026 (Bellevue, 4–7 Oct 2026), in press.
- Link: https://orca.cardiff.ac.uk/id/eprint/188912/
- First public: repository record 2026-08-11; Venue: IEEE SMC 2026; Peer-reviewed: yes (conference)
- Verification: [ABS]
- Summary: The paper frames stock-market crisis forecasting as temporal link prediction on financial networks, treating contagion as information spreading. It combines a GCN for spatial structure with a Transformer for temporal evolution (CF2TLP). On US technology stocks it reports a rise in predicted "contagion" links before the Silicon Valley Bank collapse, with AUC and precision above historical peaks.
- Graph type: temporal; homogeneous; signedness not stated.
- Task: temporal link prediction (used as crisis signal).
- Data: US technology stocks (source not stated on record page).
- Code available? Not stated.
- What it leaves open: This is the closest paper to "edge activation as a crisis signal". It does not report signed edges, does not report lags or directionality, and is a single-event case study (SVB). Rigorous multi-crisis evaluation (as in acedo2026triadic) is missing.

##### kim2026llm — LLM as a Risk Manager: LLM Semantic Filtering for Lead-Lag Trading in Prediction Markets [prediction markets, transferable]
- Citation: Kim, S., Kim, M., Kwon, J., et al., *LLM as a Risk Manager: LLM Semantic Filtering for Lead-Lag Trading in Prediction Markets*, arXiv:2602.07048, 2026.
- Link: https://arxiv.org/abs/2602.07048
- First public: arXiv v1 2026-02-04 (v2 2026-02-27); Venue: preprint (q-fin.RM, q-fin.ST); Peer-reviewed: no
- Verification: [ABS]
- Summary: The screener has two stages. First, Granger causality proposes leader→follower pairs between prediction-market price series. Second, an LLM reads the event descriptions and re-ranks pairs by whether an economic transmission mechanism is plausible. In rolling evaluation on Kalshi Economics markets, the win rate rises from 51.4% to 54.5% and the average loss falls from 649 to 347 USD. The authors attribute the gain to filtering out fragile links.
- Graph type: directed lead-lag pairs (implicit graph); unsigned in abstract; temporal (rolling).
- Task: structure learning / edge filtering → trading.
- Data: Kalshi (public market data).
- Code available? Not stated.
- What it leaves open: The method has not been applied to stocks, where 10-K text, news and supply-chain data could play the role of event descriptions. Edge sign is not addressed. The paper does not test whether "semantically plausible" edges are more *persistent*, which would be the natural edge-activation question.

##### hou2026inductive — Inductive Dual-Polarity Modeling via Static-Dynamic Disentanglement for Dynamic Signed Networks (IDP-DSN) [general signed-graph method]
- Citation: Hou, Y., Huang, J., Ran, Y., et al., *Inductive Dual-Polarity Modeling via Static-Dynamic Disentanglement for Dynamic Signed Networks*, arXiv:2604.18213, 2026.
- Link: https://arxiv.org/html/2604.18213v1 (abs: https://arxiv.org/abs/2604.18213)
- First public: arXiv v1 2026-04-20; Venue: preprint (cs.SI); Peer-reviewed: no
- Verification: [FT]
- Summary: The task is three-class dynamic signed edge prediction (Positive / Negative / NonEdge for a query (u,v,t)) using only past events. Each node keeps separate positive and negative memories, unseen nodes are handled by history-only neighbourhood inference, and an orthogonality regulariser disentangles the static and dynamic parts of each polarity. Splits are chronological 70/15/15. It is compared with 13 baselines (TGN, TGAT, GraphMixer, SGCN, SDGNN, SEMBA, PolarDSN, …) and reports +17–30% Macro-F1.
- Graph type: temporal event stream; signed; directed.
- Task: temporal signed link prediction (existence + sign).
- Data: BitcoinAlpha, BitcoinOTC, Wiki-RfA, Epinions (public, SNAP).
- Code available? https://github.com/codewhisperer-rs/IDP-DSN (stated in paper)
- What it leaves open: It has never been tested on market-derived signed graphs. The three-class formulation (+ / − / none) maps directly onto "lead-lag edge is active positive, active negative, or inactive". Weights and lags are not modelled.

##### regier2026dynamic — Dynamic Link Prediction with Temporally Enhanced Signed Graph Neural Networks (HCIM) [general signed-graph method]
- Citation: Regier, D., Polyak, A., Dadlani, A., et al., *Dynamic Link Prediction with Temporally Enhanced Signed Graph Neural Networks*, arXiv:2605.26290, 2026.
- Link: https://arxiv.org/html/2605.26290 (abs: https://arxiv.org/abs/2605.26290)
- First public: arXiv v1 2026-05-25; Venue: preprint (cs.LG); Peer-reviewed: no
- Verification: [FT]
- Summary: A plug-in Historical Context Integration Module (learnable recency decay, then LSTM, then multi-head temporal attention, then global or node-adaptive fusion) adds temporal memory to a static signed GNN. It is applied to SE-SGformer (self-explainable signed graph transformer, AAAI 2025). AUC gains are 1.4–14% over snapshots of Bitcoin OTC/Alpha, Reddit and synthetic WS/BA graphs with sign flips, over 30 seeds with paired t-tests.
- Graph type: temporal snapshots; signed; (directed per dataset).
- Task: temporal signed link prediction.
- Data: Bitcoin OTC/Alpha, Reddit hyperlinks (public); synthetic.
- Code available? https://github.com/incoder-mru/Historical-Context-Integration-Module
- What it leaves open: The authors list "market dynamics in financial networks" only as future work and call for standardised temporal signed benchmarks. It keeps a self-explainable backbone, which suits analyst-facing explanations of why an edge is predicted negative. Bitcoin OTC is split into only 6 snapshots (counts for other datasets not stated), so the evaluation is very short in time.

##### zhou2025deltalag — DeltaLag: Learning Dynamic Lead-Lag Patterns in Financial Markets
- Citation: Zhou, W., Wang, S., Cucuringu, M., et al., *DeltaLag: Learning Dynamic Lead-Lag Patterns in Financial Markets*, ICAIF '25 (ACM), 2025. arXiv:2511.00390.
- Link: https://arxiv.org/html/2511.00390v1
- First public: arXiv v1 2025-11-01; Venue: ICAIF 2025 (Singapore, 15–18 Nov 2025), DOI 10.1145/3768292.3770421; Peer-reviewed: yes
- Verification: [FT]
- Summary: This is the first end-to-end model that learns a daily-rebuilt lead-lag graph with pair-specific, time-varying lags. Sparsified (top-k = 2) cross-attention runs over (stock, lag) pairs on LSTM embeddings, and the leaders' lag-aligned features predict the lagger's next-day return under a ranking loss. On S&P 500, NASDAQ and NYSE (train 2010–18, val 2019–21, test 2022–23) it beats fixed-lag and self-lag models, statistical lead-lag graphs (lagged correlation graphs, Bennett et al. meta-flow) and spatio-temporal deep models (IC ≈ 0.02–0.03, long-short SR 2.1–2.9).
- Graph type: temporal (daily); directed (leader→lagger); unsigned (softmax attention weights); learned.
- Task: regression/ranking (return prediction) with structure learning.
- Data: US equities; preprocessing follows QuantBench. Raw source not named (probably public prices, but unverified).
- Code available? Not stated.
- What it leaves open: Edge weights are non-negative softmax scores, so the learned graph cannot express "B moves *opposite* to A after lag ℓ" as an explicit signed edge (a downstream MLP may still learn inverse effects). There is one 2-year test period, no transaction costs, k = 2 only, and no evaluation of whether the learned edges are stable, interpretable or statistically valid.

##### bartesaghi2025local — Local and Global Balance in Financial Correlation Networks: an Application to Investment Decisions
- Citation: Bartesaghi, P., Grassi, R., Uberti, P., *Local and Global Balance in Financial Correlation Networks: an Application to Investment Decisions*, arXiv:2512.10606, 2025.
- Link: https://arxiv.org/html/2512.10606v1 (abs: https://arxiv.org/abs/2512.10606v1)
- First public: arXiv v1 2025-12-11; Venue: preprint (q-fin.PM, q-fin.MF); Peer-reviewed: no
- Verification: [FT]
- Summary: Signed correlation networks (DAX, EuroStoxx, FTSE, NIKKEI; 2005–2020; rolling windows) are analysed with a communicability-based global balance index κ(G) = tr e^C / tr e^|C| and its per-node local version. Global balance correlates negatively with market return (≈ −0.2 to −0.3), so it acts as a systemic-risk proxy. A rule that buys assets whose local balance deviates from global balance when global balance is high beats 1/N in-sample and OOS (NIKKEI only). It builds on Bartesaghi, Diaz-Diaz, Grassi, Uberti, *Global balance and systemic risk in financial correlation networks* (Physica A 2025; arXiv:2407.14272, opened [ABS]).
- Graph type: temporal (rolling); signed weighted; undirected; contemporaneous.
- Task: other (risk indicator; asset selection).
- Data: index constituents' daily returns (source not named).
- Code available? Not stated.
- What it leaves open: Thresholds are dataset-specific, the OOS test covers only NIKKEI, there are no costs, and data ends in 2020. The authors propose extending to "signed networks built from inputs other than return correlations", e.g. lead-lag, which is an open door for T1.

##### qing2025finding — Finding Core Balanced Modules in Statistically Validated Stock Networks
- Citation: Qing, H., Xu, X., *Finding Core Balanced Modules in Statistically Validated Stock Networks*, arXiv:2508.04970, 2025.
- Link: https://arxiv.org/abs/2508.04970
- First public: arXiv v1 2025-08-07 (v2 2026-04-07); Venue: preprint (econ.GN, q-fin.EC); Peer-reviewed: no
- Verification: [ABS]
- Summary: Signed stock networks keep only correlations that pass a t-test (statistically validated networks). The paper defines the largest strong-correlation balanced module (every triad sign-product positive) and gives existence and size results under a random signed graph model, plus a MaxBalanceCore heuristic that scales to 10,000 nodes. On China A-shares 2013–2024, the balanced core grows in stress periods (2015 crash) and rotates across sectors yearly.
- Graph type: temporal (yearly snapshots); signed; undirected.
- Task: other (structure discovery / dense balanced subgraph).
- Data: Chinese stock market (source not stated in abstract).
- Code available? Not stated.
- What it leaves open: The edges are contemporaneous and yearly, which is far from "edges activated in certain periods" at trading frequency. There is no directed or lagged version and no predictive use. Its statistical-validation step is a reusable edge-filtering recipe for lead-lag edges.

##### qiu2025hermes — Hermes: A Multi-Scale Spatial-Temporal Hypergraph Network for Stock Time Series Forecasting
- Citation: Qiu, X., Yang, L., Xu, X., et al., *Hermes: A Multi-Scale Spatial-Temporal Hypergraph Network for Stock Time Series Forecasting* (v1 title: *Multi-Scale Spatial-Temporal Hypergraph Network with Lead-Lag Structures for Stock Time Series Forecasting*), arXiv:2509.23668, 2025.
- Link: https://arxiv.org/html/2509.23668v2 ; OpenReview reviews seen via https://api2.openreview.net/notes/search?term=lead-lag%20stock&limit=25 (forum 08FTG45E9m)
- First public: arXiv Sept 2025 (exact v1 day not shown; OpenReview/DBLP record cdate 2025-09-01); Venue: ICLR 2026 submission, **withdrawn** (reviews rated 6/4/2/2); Peer-reviewed: no (not accepted)
- Verification: [FT]
- Summary: Industries are hyperedges. A "moving aggregation" module uses attention between earlier (leading) and last (lagging) points of sliding windows to inject inter-industry lead-lag, and a multi-scale fusion module combines time scales. It reports the best IC/ICIR/SR on NASDAQ/NYSE (Feng et al. 2019; 2013–2017) and S&P 500 (2016–2022).
- Graph type: hypergraph (static industry hyperedges) with temporal attention; unsigned; lead-lag only implicit.
- Task: regression/ranking (next-day return).
- Data: public benchmark datasets (NASDAQ/NYSE from Feng et al. 2019; S&P 500 from Yahoo via Huynh et al. 2023).
- Code available? Not stated.
- What it leaves open: Reviewers flagged no causal validation of the lead-lag modules, a static industry hypergraph, US-only data and an inconsistent lead-lag description. Lead-lag is neither signed nor edge-level, so it cannot answer "which edge is active, and with which sign".

##### krstev2025temporal — A Temporal Graph Learning Framework for Lead-Lag Detection in Financial Markets
- Citation: Krstev, I., Rigoni, D., Mishkovski, I., Pasa, L., *A Temporal Graph Learning Framework for Lead-Lag Detection in Financial Markets*, ICLR 2026 submission (withdrawn), 2025.
- Link: https://api2.openreview.net/notes/search?term=lead-lag%20stock&limit=25 (forum KsWRLyIAKP; the forum page itself was blocked by a browser check)
- First public: OpenReview reviews dated 2025-10-27 to 2025-11-02; Venue: ICLR 2026, withdrawn; Peer-reviewed: reviewed and withdrawn (ratings 2/2/4/4/4)
- Verification: [ABS] (abstract fragments + 5 reviews read via the OpenReview API)
- Summary: Lead-lag detection is cast as temporal link prediction on a dynamic graph of 37 assets (≈29 stocks + 8 commodities, ≈1,257 daily steps). Features are price, indicators, sentiment and 384-d text embeddings. A link is labelled when an asset's next-day move follows another's (τ = 1, ε = 5%), with "positive and negative" and "positive-only" variants. JODIE, DySAT, TGAT, TGN, APAN and GraphMixer are benchmarked, and GraphMixer wins. It appears to grow out of an MSc thesis (Krstev, Univ. Padova, 2025; see A2) with the same 37-asset dataset and GraphMixer result. I have not verified that the authors are the same.
- Graph type: temporal (daily); directed; labels can be signed (positive/negative co-movement).
- Task: temporal link prediction.
- Data: custom 37-asset dataset (public prices + sentiment); release not verified.
- Code available? Not stated in what I could see.
- What it leaves open: This is **the closest existing work to the supervisor's idea, and it shows the niche is open but non-trivial**. Reviewers criticised the arbitrary fixed τ = 1 and ε, labels driven by single-day co-movement noise, the 37-node scale, no comparison with Granger/transfer entropy/DTW, no backtest, and an ablation where static embeddings perform as well as temporal features. A rigorous large-universe version with statistically validated, signed, multi-lag edges does not exist.

---

##### A1. Background (older; labelled)

#### bennett2022leadlag — [BACKGROUND] Lead-lag detection and network clustering for multivariate time series, with an application to the US equity market
- Citation: Bennett, S., Cucuringu, M., Reinert, G., arXiv:2201.08283, 2022 (published in *Machine Learning*, Springer, per link https://link.springer.com/10.1007/s10994-022-06250-4 seen in search; not opened).
- Link: https://arxiv.org/abs/2201.08283
- First public: arXiv v1 2022-01-20; Verification: [ABS]
- Role: The classic statistical pipeline. It builds a directed lead-lag network from pairwise lagged metrics, then uses directed clustering to find leader/lagger clusters with flow imbalance in US equities. It is the "Meta-flow graph" baseline in DeltaLag. The Cucuringu group still presents this line in 2025–2026 talks (see A2).

#### he2022msgnn — [BACKGROUND] MSGNN: A Spectral Graph Neural Network Based on a Novel Magnetic Signed Laplacian
- Citation: He, Y., Permultter, M., Reinert, G., Cucuringu, M., arXiv:2209.00546, 2022.
- Link: https://arxiv.org/abs/2209.00546
- First public: arXiv 2022-09-01; Verification: [ABS]
- Role: The canonical GNN for signed *and directed* graphs (magnetic signed Laplacian). It introduces real-world signed directed datasets built from lead-lag relationships in financial time series, so a ready-made methodological bridge between signed GNNs and lead-lag stock graphs already exists.

---

##### A2. Other opened items (peripheral; one line each)
| Key / item | Link | First public | Verif. | Why peripheral |
|---|---|---|---|---|
| zhang2026dualchannel — LSWJP: joint prediction of link, sign and weight in dynamic signed weighted networks | https://arxiv.org/html/2602.12663v1 | 2026-02-13 | [FT] | General method (Bitcoin/Wiki/email). Gains overstated vs its own Table III, no code. A useful "existence + sign + weight" multi-task template. |
| huang2026crossstock — Cross-Stock Predictability via LLM-Augmented Semantic Networks | https://arxiv.org/abs/2604.19476 | 2026-04-21 | [ABS] | Builds 10-K-embedding edges, then an LLM filters spurious ones. Cross-stock predictability on S&P 500 2011–2019 with Sharpe 0.74 → 0.82. Unsigned and not lead-lag, but it is the stock analogue of kim2026llm. |
| niu2026gapnet — GAPNet: plug-in jointly learning task-specific graph for dynamic stock relation | https://arxiv.org/abs/2602.00888 | 2026-01-31 | [ABS] | Learns a dynamic graph jointly with forecasting. Signed or lead-lag edges not stated. |
| lee2026regimedependent — Source Exclusion in Regime-Gated Forecasting: a Cross-Market Audit of Equity-Factor Predictability (v1 listed as "Regime-Dependent Predictive Structure Between Equity Factors: Evidence from Granger Causality") | https://arxiv.org/abs/2601.10732 | 2026-01-06 | [ABS] | HMM-gated cross-factor predictability. None of 2,400 endpoints resolves a direction under confidence sequences. A cautionary result: regime-activated predictive edges are statistically fragile. |
| jing2026market — When market boundaries weaken: network reconfiguration and regime-dependent cross-asset spillovers | https://arxiv.org/abs/2605.30442 | 2026-05-28 | [ABS] | Rolling correlation networks + VAR connectedness across crypto, FX and S&P 500 (2017–2024). Integration is episodic and stress-dependent. Signedness not stated. |
| hassan2026information — Information Networks of Stock Prices | https://arxiv.org/abs/2606.07450 | 2026-06-05 | [ABS] | Indonesian market, Pearson vs MI, MST/PMFG. Undirected, unsigned. |
| alridhawi2026stock — Node Transformer + BERT sentiment | https://arxiv.org/abs/2603.05917 | 2026-03-06 | [ABS] | 20 S&P 500 stocks with correlation and supply-chain edges. No signed or lead-lag edges stated (IEEE Access DOI listed). |
| girnyk2026correlation — Correlation Structures and Regime Shifts in Nordic Stock Markets | https://arxiv.org/abs/2601.06090 | 2025-12-31 | [ABS] | Regime-dependent eigenstructure; not a network or signed formulation. |
| adhikari2025signed — Signed network models for portfolio optimization | https://arxiv.org/abs/2510.05377 | 2025 (Oct, from ID) | [ABS] | Predecessor of adhikari2026signed: negative-edge hedge score. |
| bartesaghi2025global — Global balance and systemic risk in financial correlation networks | https://arxiv.org/abs/2407.14272 | v1 2024-07-19; v2 2025-06-03; Physica A 674 (2025) per citation in bartesaghi2025local | [ABS] | Global balance index as a systemic-risk measure. |
| diazdiaz2025signed — Signed Networks: theory, methods, and applications (review) | https://arxiv.org/abs/2511.17247 | 2025-11-21 | [ABS] | General survey covering balance/frustration, sign prediction, null models and evolving networks. Good thesis background. |
| fang2025data — From Data Acquisition to Lag Modeling: A-Share | https://arxiv.org/abs/2506.19255 | 2025-06-24 | [ABS] | Two-stage pipeline: daily coupling, then intraday lead-lag (cross-corr/Granger). Strongly coupled pairs show more lead-lag at finer scales. |
| chen2025dual — DRFN: dual relation fusion (static + dynamic relations) | https://arxiv.org/abs/2510.10695 | 2025-10-12 | [ABS] | Dynamic relations; not signed or lead-lag. |
| sadek2025hybrid — news + LSTM + heterogeneous GraphSAGE | https://arxiv.org/abs/2512.08567 | 2025-12-09 | [ABS] | Off-theme (heterogeneous news graph). |
| krstev2025graphmixer — "GraphMixer in Financial Networks: A Novel Approach to Lead/Lag Pattern Detection" (MSc thesis, Univ. Padova; supervisor L. Pasa) | https://thesis.unipd.it/handle/20.500.12608/81805 | available 2025-02-24 | [ABS] | An MSc thesis framing lead-lag as temporal link prediction (37 assets). Precursor of krstev2025temporal (same first author and supervisor). |
| Cucuringu, "Graph Clustering and Ranking for Time Series with Applications to Lead-Lag Detection in Equity Markets" (talk abstract, ISI WSC 2025, 6 Oct 2025) | https://isi-next.org/abstracts/submission/3184/view | 2025-10-06 | [ABS] | Talk, not a paper. Spectral clustering of signed and directed networks plus lead-lag clusters in US equities. |
| Cucuringu, CodEx seminar abstract "Signed and Directed Graph Clustering in Financial Time Series: … the Market Tug-of-War" | https://www.math.colostate.edu/~king/codex/abstracts/cucuringu2026.html | 2026 (page undated) | [ABS] | Talk. Overnight vs daytime lead-lag "tug-of-war" propagating through directed lead-lag relations. The underlying paper was not identified. |
| BSIC "Trading the Network II: What Should an Edge Mean?" (student investment-club note) | https://bsic.it/wp-content/uploads/2026/10/Trading-the-Network-II-11-10-2026.pdf | Oct 2026 (URL path) | [FT] | Grey literature. Residualised (market + sector removed) directed lead-lag graphs with k = 1 neighbour improve Ridge rank-IC (0.0328 vs 0.0289). Contemporaneous correlation graphs do not help, GCN/GAT underperform, and gains shrink after costs. A useful sanity baseline. |

---


---

## T2. Link prediction in company and stock networks

**Synthesis.** Generic temporal link prediction is crowded (TGB, TGB 2.0, TGB-Seq, DyGLib, TGM, many ICLR 2025/2026 submissions). Its application to **listed-company or stock graphs with public data and temporal evaluation** is thin: about 10 relevant 2025–2026 papers, most with proprietary data (FactSet) or random splits. None of the benchmark suites contains an equity or company dataset. Two findings recur: persistence/EMA heuristics nearly match the best temporal GNNs on financial graphs (izadifar2026institutional: EMA 0.888 vs NAVIS 0.913 NDCG, other TGNNs ≤ 0.70), and degree-corrected negative sampling can reverse claimed gains. Any thesis here must beat persistence honestly. Verdict: **crowded methods layer, thin application layer.**

Full per-paper notes (from `sweep/T2.md`; raw query log and unopened leads stay there):

##### izadifar2026institutional — Institutional Equity Holdings Prediction Using Node Affinities of Dynamic Graphs
- Citation: Izadifar, E., Rahmati, Z., *Institutional Equity Holdings Prediction Using Node Affinities of Dynamic Graphs*, arXiv:2607.12067, 2026.
- Link: https://arxiv.org/abs/2607.12067 (full text: https://arxiv.org/pdf/2607.12067v1, https://arxiv.org/html/2607.12067v1)
- First public: arXiv v1 2026-07-13; Venue: preprint; Peer-reviewed: no
- Verification: [FT]
- Summary: Frames SEC 13F holdings forecasting as TGB-style *dynamic node affinity prediction*: predict each manager's next-quarter portfolio-weight vector on a discrete-time manager→security bipartite graph. Calls itself the first benchmark for this task. Setup: 99 sampled managers × 503 S&P 500 securities, 209,351 temporal edges, 48 quarters (2013–2025). Temporal split: train Q2-2013–Q1-2023, val Q2-2023–Q1-2024, test Q2-2024–Q1-2025. Test NDCG@10: NAVIS 0.9127. The **heuristics are nearly as good**: Persistent Forecast 0.8891, EMA 0.8882. Learned TGNNs score far lower: TGNv2 0.6975, TGN 0.6178, DyGFormer 0.5965, DyRep 0.5804. Node features add less than 1.2%.
- Graph type: temporal (discrete, quarterly); heterogeneous bipartite; directed; weighted; unsigned.
- Task: link-weight / node-affinity ranking (a form of temporal link prediction).
- Data: SEC Form 13F (public), fuzzy-matched to S&P 500 constituents (match score ≥80).
- Code available? https://github.com/e-izdfr/portfolio-holdings-prediction (stated in the paper)
- What it leaves open: Only 99 managers (GPU limits). Long-only quarterly data. The 45-day filing lag is mentioned but its handling is not described. It does not say whether the 503 S&P 500 securities are current or point-in-time constituents, which risks survivorship bias (the authors list survivorship as a general risk). It predicts weights only: new positions and exits are representable (zero weights) but not evaluated separately, no signed increase/decrease edges, and no economic test. Stated future work: 13D/13G/N-PORT filings, multi-step forecasting, hypergraphs.

##### marzi2026bayesian — A Bayesian approach to out-of-sample network reconstruction
- Citation: Marzi, M., Squartini, T., *A Bayesian approach to out-of-sample network reconstruction*, arXiv:2602.21869, 2026 (journal DOI 10.1038/s42005-026-02725-1; the journal name is not shown on the page).
- Link: https://arxiv.org/abs/2602.21869 (full text: https://arxiv.org/html/2602.21869)
- First public: arXiv 2026-02-25 (v3 2026-07-29); Venue: journal with a Nature-portfolio DOI (name not shown); Peer-reviewed: likely yes (journal DOI), unconfirmed
- Verification: [FT]
- Summary: Uses past snapshots as a Bayesian prior to predict *future* snapshots of a network (Bayesian Erdős–Rényi model and Bayesian fitness model; link probability z·s_i·s_j/(1+z·s_i·s_j)). Also has a "self-sustained" mode in which each prediction becomes the prior for the next. On weekly eMID interbank data (1999–2012), the fitness model reaches TPR ≈0.40 and Jaccard ≈0.20. Over 561 weeks the self-sustained Bayesian predictor matches an *in-sample* gravity model (mean TPR 0.2498 vs 0.2494).
- Graph type: temporal (weekly snapshots); homogeneous; undirected binarised in the experiments.
- Task: future link prediction / out-of-sample network reconstruction.
- Data: eMID interbank market (not public).
- Code available? not stated
- What it leaves open: No ML or temporal-GNN baselines. Errors rise around the 2008 crisis. Only one market, which is non-public. **Never tested on company networks** (supply chain, ownership, correlation), where node "strength" (revenue, market cap, AUM) is publicly observable.

##### santucci2026missing — Missing links prediction: comparing machine learning with physics-rooted approaches
- Citation: Santucci, F., Cimini, G., Squartini, T., *Missing links prediction: comparing machine learning with physics-rooted approaches*, arXiv:2601.23061, 2026.
- Link: https://arxiv.org/abs/2601.23061 (full text: https://arxiv.org/html/2601.23061)
- First public: arXiv 2026-01-30 (v2 2026-08-01); Venue: preprint; Peer-reviewed: unknown
- Verification: [FT]
- Summary: Compares maximum-entropy "white-box" models (Configuration Model, fitness variants, CMD) with gradient-boosted trees for *missing*-link prediction on the World Trade Web (1990–2000) and eMID (1999–2014). No method wins overall. ML wins when given exogenous features (GDP, distance). White-box models match or beat ML when given degrees or structure, and are faster and interpretable. Results depend on the sampling scheme, the metric and the aggregation level, and targeted (non-random) edge removal degrades every method.
- Graph type: static snapshots; homogeneous; binarised and symmetrised.
- Task: link prediction (missing links, random removal of 10–30%).
- Data: WTW (Gleditsch trade data, public); eMID (not public).
- Code available? not stated
- What it leaves open: Missing-link, not future-link, evaluation. No firm-level networks (supply chain is mentioned only as prior work). No GNN baselines. This makes it a direct template for a "white-box vs TGNN on company graphs" comparison.

##### huang2026cross — Cross-Stock Predictability via LLM-Augmented Semantic Networks
- Citation: Huang, Y., Fan, Z., Hu, K., Ye, Y., *Cross-Stock Predictability via LLM-Augmented Semantic Networks*, arXiv:2604.19476, 2026.
- Link: https://arxiv.org/abs/2604.19476 (full text: https://arxiv.org/html/2604.19476v2)
- First public: arXiv 2026-04-21 (v2 2026-04-26); Venue: preprint (q-fin.PM); Peer-reviewed: no
- Verification: [FT]
- Summary: Builds candidate firm–firm edges from 10-K risk-factor embeddings (top-K=5 cosine peers, year y−1). DeepSeek-Chat then **classifies each candidate edge** into competitor / supply_chain / complementary / substitute / peer / unrelated, using anonymised Item 1 text. Competitor edges are dropped and substitute edges down-weighted. Pair mean-reversion signals are aggregated per stock. On S&P 500, 2011–2019, the long-short Sharpe rises from 0.742 to 0.820 (SIC graph 0.792, random graph 0.541).
- Graph type: static annual (rolling); typed edges (6 relation labels); undirected.
- Task: edge classification / filtering (LLM as link verifier) feeding return prediction.
- Data: 10-K filings (public, EDGAR) and prices.
- Code available? not stated
- What it leaves open: The LLM's memorisation of 2011–2019 is **not tested** (anonymisation is the only safeguard). No transaction costs (turnover about 22× per year). Static graph. The LLM is a standalone classifier, not a learnable link predictor. It never asks whether the LLM-typed edges *predict future* co-movement or supply links.

##### feldman2025revisiting — Revisiting Node Affinity Prediction in Temporal Graphs (NAVIS)
- Citation: Feldman, O., Mantri, K. S. I., Eliasof, M., et al., *Revisting [sic] Node Affinity Prediction in Temporal Graphs*, ICLR 2026. (The ICLR proceedings list Feldman first; the arXiv summary listed Mantri first.)
- Link: https://proceedings.iclr.cc/paper_files/paper/2026/hash/aa5d22c77b380e2261332bb641b3c2e3-Abstract-Conference.html ; https://arxiv.org/abs/2510.06940
- First public: arXiv 2510.06940 (October 2025; exact date not shown); Venue: ICLR 2026 poster (OpenReview forum 6UvkemEgK3); Peer-reviewed: yes
- Verification: [ABS]
- Summary: Explains why TGNNs lose to Persistent Forecast and Moving Average on TGB node-affinity tasks. Proposes NAVIS (per-node states plus a virtual global state, motivated by the heuristics↔state-space-model link) and a rank-based loss, and beats the heuristics on TGB. Datasets: tgbn-trade, tgbn-genre, tgbn-reddit, tgbn-token, plus Wikipedia, Flights, USLegis and UNVote converted to affinity tasks. The abstract names financial networks as an application.
- Graph type: temporal; homogeneous or bipartite.
- Task: node affinity prediction (future link weights).
- Data: TGB datasets (public). tgbn-trade is UN country-level trade; tgbn-token is crypto. No equity data.
- Code available? https://github.com/orfeld415/NAVIS
- What it leaves open: Not evaluated on equity or company graphs. Izadifar 2026 is the first to apply it to 13F data. The fact that heuristics are strong on smooth, persistent financial graphs is the key methodological warning for this theme.

##### chmura2025tgm — TGM: a Modular and Efficient Library for Machine Learning on Temporal Graphs
- Citation: Chmura, J., Huang, S., Ngo, T. G. B., et al., *TGM: a Modular and Efficient Library for Machine Learning on Temporal Graphs*, ICLR 2026 (arXiv:2510.07586).
- Link: https://arxiv.org/abs/2510.07586 ; OpenReview record via https://api2.openreview.net/notes/search?term=TGM%20Modular%20Efficient%20Library%20Machine%20Learning%20Temporal%20Graphs&limit=5
- First public: arXiv v1 2025-10-08 (OpenReview submission 2025-09-17; earlier ICML 2025 CODEML workshop version); Venue: ICLR 2026 poster; Peer-reviewed: yes
- Verification: [ABS]
- Summary: The first library that unifies continuous-time and discrete-time temporal graph learning. It supports dynamic node features, time-granularity conversion, and link-, node- and graph-level tasks. Reports a 7.8× average speedup over DyGLib and 175× on discretisation.
- Graph type: temporal (CTDG and DTDG).
- Task: infrastructure for link, node and graph property prediction.
- Data: no financial datasets mentioned.
- Code available? https://github.com/tgm-team/tgm
- What it leaves open: Tooling only. It is the natural engine for a finance temporal link prediction thesis, because it handles discretisation (daily → monthly co-movement graphs) out of the box.

##### krstev2025temporal — A Temporal Graph Learning Framework for Lead-Lag Detection in Financial Markets
- Citation: Krstev, I., Rigoni, D., Mishkovski, I., et al., *A Temporal Graph Learning Framework for Lead-Lag Detection in Financial Markets*, ICLR 2026 submission (withdrawn).
- Link: https://api2.openreview.net/notes/search?term=Temporal%20Graph%20Learning%20Framework%20Lead-Lag%20Detection%20Financial%20Markets&limit=10 (OpenReview forum KsWRLyIAKP; reviews included in the record)
- First public: OpenReview submission ~2025-09-18; Venue: ICLR 2026 Withdrawn Submission; Peer-reviewed: reviewed (ratings 2–4) and withdrawn, so not accepted
- Verification: [ABS] (abstract plus the reviews; PDF not opened)
- Summary: The **only paper found that frames stock co-movement / lead-lag as temporal link prediction**. It covers 37 assets (29 stocks, mostly renewable-energy/EV, plus 8 commodities) over ~5 years of daily data. Directed leader→lagger edges use a lag of τ=1 day and a threshold of ε=5%, in two settings: positive+negative edges and positive-only. Models: JODIE, DySAT, TGAT, TGN, APAN, GraphMixer and LSTM; GraphMixer was best. Metrics: AP, AUC, MRR, Recall@k.
- Graph type: temporal; directed; **signed variant** (positive/negative relations); node features (prices, 384-d text embeddings, sentiment).
- Task: temporal link prediction.
- Data: "custom-gathered"; public availability not stated.
- Code available? Supplementary zip on OpenReview; no public repository stated.
- What it leaves open: Per the reviewers, the τ and ε choices are unjustified, single-day co-movement makes noisy labels, the universe is tiny and sector-concentrated, and there is no comparison with Granger/transfer-entropy/correlation baselines. Also no persistence baseline, no trading or economic validation, and no regime analysis. Static embeddings may dominate the result. **Withdrawal leaves the niche open.**

##### sun2025intercorprel — InterCorpRel-LLM: Enhancing Financial Relational Understanding with Graph-Language Models
- Citation: Sun, Q., Zheng, J., Jin, B., et al., *InterCorpRel-LLM: Enhancing Financial Relational Understanding with Graph–Language Models*, arXiv:2510.09735, 2025.
- Link: https://arxiv.org/abs/2510.09735 (full text: https://arxiv.org/html/2510.09735v1)
- First public: arXiv v1 2025-10-10; Venue: preprint; Peer-reviewed: no
- Verification: [FT]
- Summary: Combines a GNN with a 7B LLM, trained on company-graph matching, industry classification and supply-relation prediction. Firm text comes from DeepSeek-V3 10-K summaries embedded with Qwen3-Embedding-8B. Data: 3,211 US firms and 11,635 FactSet supplier→customer links (2023). Supply-link F1 is 0.8543 (inductive) vs 0.2287 for zero-shot GPT-5; GraphSAGE scores 0.0 in the fully inductive setting. Zero-shot competitor detection reaches F1 0.7774.
- Graph type: static; directed (supplier→customer); homogeneous firm graph with text attributes.
- Task: link prediction (supply), pair classification (competitor).
- Data: FactSet Supply Chain Relationships (**proprietary**); 10-K text (public).
- Code available? not stated (no release)
- What it leaves open: Single-year snapshot, random **firm-level** split (no temporal split), small fully-inductive test set (95+95), unmatched baseline inputs, proprietary labels, no code. Can it **predict next year's new supply links from public filings**? Untested.

##### li2025cmag — C-MAG: Cascade Multimodal Attributed Graphs for Supply Chain Link Prediction
- Citation: Li, Y., Tang, Z., Zhuang, J., et al., *C-MAG: Cascade Multimodal Attributed Graphs for Supply Chain Link Prediction*, KDD 2025 Workshop AI4SupplyChain (arXiv:2508.08071).
- Link: https://arxiv.org/abs/2508.08071 (full text: https://arxiv.org/html/2508.08071v1)
- First public: arXiv v1 2025-08-11 (v2 2025-08-13); Venue: KDD 2025 AI4SupplyChain workshop poster (OpenReview mE5n6OJHwO); Peer-reviewed: workshop
- Verification: [FT]
- Summary: Introduces PMGraph: 8,888 manufacturers, 72,789 products, 112,597 manufacturer–product edges, 29,178 images. Proposes a two-stage cascade that fuses text and image attributes into group embeddings and then runs multiscale heterogeneous message passing. Best ROC-AUC is about 75% (HeteroGAT).
- Graph type: static; heterogeneous (manufacturer–product–attribute–image); bipartite core.
- Task: link prediction (manufacturer–product, *not* firm–firm).
- Data: PMGraph, public: https://huggingface.co/datasets/shawntzx/PMGraph (built from the SUDOKN KG and a manufacturer directory).
- Code available? not stated
- What it leaves open: Random 80/10/10 edge split, unspecified 1:1 negatives, no external baselines, no variance across seeds, no temporal evaluation. Firms are private US manufacturers, not listed companies.

##### kosasih2025towards — Towards trustworthy AI for link prediction in supply chain knowledge graph: a neurosymbolic reasoning approach
- Citation: Kosasih, E. E., Brintrup, A., *Towards trustworthy AI for link prediction in supply chain knowledge graph: a neurosymbolic reasoning approach*, International Journal of Production Research 63(6):2268–2290, 2025.
- Link: https://econpapers.repec.org/RePEc:taf:tprsxx:v:63:y:2025:i:6:p:2268-2290 (DOI 10.1080/00207543.2024.2399713)
- First public: journal issue 2025 (the DOI suffix suggests online-first in 2024); Venue: IJPR; Peer-reviewed: yes
- Verification: [ABS]
- Summary: Neurosymbolic link prediction on supply-chain knowledge graphs for "digital supply chain surveillance". Matches black-box state of the art on automotive and energy benchmark datasets while making the reasoning transparent.
- Graph type: static; heterogeneous KG.
- Task: link prediction (hidden supplier relations) with explanations.
- Data: automotive and energy supply-chain KGs (availability not stated in the abstract).
- Code available? not stated
- What it leaves open: No temporal evaluation is visible, the data is not listed firms, and there is no link to market outcomes. The explainable-LP angle has not been carried over to listed-company graphs (this fits Frank's XAI background).

##### wu2025dexposure — DeXposure: A Dataset and Benchmarks for Inter-protocol Credit Exposure in Decentralized Financial Networks [crypto, transferable]
- Citation: Wu, W., Qian, K., Lui, A., et al., *DeXposure: A Dataset and Benchmarks for Inter-protocol Credit Exposure in Decentralized Financial Networks*, arXiv:2511.22314, 2025.
- Link: https://arxiv.org/abs/2511.22314 (full text: https://arxiv.org/pdf/2511.22314v1, Section 5.3)
- First public: arXiv v1 2025-11-27; Venue: preprint; Peer-reviewed: no
- Verification: [FT]
- Summary: About 43.7M entries covering ~4.3k DeFi protocols, 602 chains and 24.3k tokens (2020–2025). Exposure edges are inferred from TVL changes. Three benchmarks: clustering, VAR, and dynamic link prediction with a ROLAND-style GCN+GRU on weekly snapshots, scored by AUPRC. **Link-prediction AUPRC drops around major market shocks (Terra, FTX)**, and the authors propose network predictability as a market-condition indicator.
- Graph type: temporal (weekly snapshots); directed exposure network; weighted.
- Task: dynamic link prediction (plus clustering and VAR).
- Data: public (DefiLlama-derived); https://github.com/dthinkr/DeXposure
- Code available? https://github.com/dthinkr/DeXposure
- What it leaves open: Split and negative sampling unspecified, one model only, no numeric results table. The idea that falling link predictability signals stress is **transferable to equity, ownership or credit networks** and is untested there.

##### yi2025tgbseq — TGB-Seq Benchmark: Challenging Temporal GNNs with Complex Sequential Dynamics
- Citation: Yi, L., Peng, J., Zheng, Y., et al., *TGB-Seq Benchmark: Challenging Temporal GNNs with Complex Sequential Dynamics*, ICLR 2025 (arXiv:2502.02975).
- Link: https://arxiv.org/abs/2502.02975
- First public: arXiv 2025-02-05 (v3 2025-03-15); Venue: ICLR 2025 poster (OpenReview 8e2LirwiJT); Peer-reviewed: yes
- Verification: [ABS]
- Summary: Argues that existing future-link-prediction benchmarks contain too many *repeated edges*, so models score well by memorising. TGB-Seq minimises repeated edges and tests generalisation to unseen edges. GraphMixer, DyGFormer and others degrade sharply. Domains: e-commerce, movie ratings, business reviews, social, citation, web links. **No financial or company dataset.**
- Graph type: temporal (CTDG); bipartite or homogeneous.
- Task: temporal link prediction (benchmark).
- Data: public; https://tgb-seq.github.io/
- Code available? https://tgb-seq.github.io/ (datasets, leaderboards, example code)
- What it leaves open: Financial graphs (13F, co-movement, supply chain) are dominated by repeated edges. A "new edges only" evaluation in the TGB-Seq spirit has not been applied to them.

##### yu2025staged — Staged link prediction in bipartite investment networks based on pseudo-edge generation
- Citation: Yu, J., Lee, Y., *Staged link prediction in bipartite investment networks based on pseudo-edge generation*, Information Technology and Management 26(3):391–405, 2025.
- Link: OpenReview record via https://api2.openreview.net/notes/search?term=Staged%20link%20prediction%20bipartite%20investment%20networks%20pseudo-edge&limit=5 (DOI 10.1007/s10799-024-00421-6)
- First public: issue September 2025 (the DOI suggests online-first in 2024); Venue: Inf. Technol. Manag.; Peer-reviewed: yes
- Verification: [ABS]
- Summary: Predicts second-stage investor→company investments in bipartite VC networks. Generates pseudo-edges for undisclosed early-round investors before link prediction. Best accuracy is 88.98%.
- Graph type: bipartite investor–company; staged (implicitly temporal).
- Task: link prediction.
- Data: not specified in the record.
- Code available? not stated
- What it leaves open: Data unknown. Accuracy (not ranking) metric. No TGNN baselines visible. Supervisor fit: investor–company graphs (her earlier Crunchbase work).

##### gastinger2024tgb — TGB 2.0: A Benchmark for Learning on Temporal Knowledge Graphs and Heterogeneous Graphs [BACKGROUND]
- Citation: Gastinger, J., Huang, S., Galkin, M., et al., *TGB 2.0: A Benchmark for Learning on Temporal Knowledge Graphs and Heterogeneous Graphs*, arXiv:2406.09639, 2024.
- Link: https://arxiv.org/abs/2406.09639 (full text: https://arxiv.org/html/2406.09639v2)
- First public: arXiv 2024-06-14 (v2 2024-10-18); Venue: not shown on the page; Peer-reviewed: unknown from the page
- Verification: [FT] (dataset table read)
- Summary: Eight multi-relational temporal datasets: tkgl-smallpedia, tkgl-polecat, tkgl-icews, tkgl-wikidata, thgl-software, thgl-forum, thgl-github, thgl-myket. **None involves companies, stocks or finance.** POLECAT and ICEWS are political events between actors. Used here as background to answer "is there an equity or company dataset in TGB?". Evidence across TGB 1.0/2.0/TGB-Seq: no equity or company dataset. The closest are tgbn-trade (country trade), tgbn-token and tgbl-coin (crypto). The tgbl-coin evidence comes from the OpenReview record of "Temporal Graph Rewiring with Expander Graphs" and the TGB record describing "transaction and trade networks".
- Graph type: temporal KG / temporal heterogeneous graph.
- Task: benchmark for temporal multi-relational link prediction.
- Data: public.
- Code available? not shown on the fetched page
- What it leaves open: A firm-centric TKG/THG (supplier, owner, competitor, event edges) in tkgl/thgl format does not exist yet.

##### cao2023companykg — CompanyKG: A Large-Scale Heterogeneous Graph for Company Similarity Quantification [BACKGROUND]
- Citation: Cao, L., von Ehrenheim, V., Granroth-Wilding, M., et al., *CompanyKG: A Large-Scale Heterogeneous Graph for Company Similarity Quantification*, arXiv:2306.10649 (2023); KDD 2024 (v2); IEEE Transactions on Big Data 11(1):247–258, 2025.
- Link: OpenReview records via https://api2.openreview.net/notes/search?term=CompanyKG%20edge%20prediction&limit=10
- First public: arXiv 2023. Its 2025 journal version would formally qualify under rule 1, but it is labelled [BACKGROUND] here because the work dates from 2023/2024. Peer-reviewed: yes (KDD 2024; IEEE TBD 2025)
- Verification: [ABS]
- Summary: 1.17M companies, 51.06M weighted edges, 15 inter-company relation types, with description embeddings. Tasks: similarity prediction, competitor retrieval, similarity ranking, and (v2) **edge prediction**. Benchmarks 11 methods. Data comes from "a real-world investment platform" and is released under a permissive licence.
- Graph type: static; heterogeneous edge types; weighted.
- Task: similarity, retrieval, edge prediction.
- Data: open (URL not shown in the record).
- Code available? stated open; URL not shown in the record
- What it leaves open: Static, anonymised/private companies, no temporal edges, no link to listed equities.

##### A2. Also opened, peripheral or out of window (not in BibTeX)
- **niu2026relational**: Niu, Jin, Dolphin, Dong, *Relational Probing: LM-to-Graph Adaptation for Financial Prediction*, arXiv:2604.10212, v1 2026-04-11 [ABS], https://arxiv.org/abs/2604.10212. A relation head builds a stock graph from small-LM hidden states (Qwen3 0.6B–4B), trained jointly with trend prediction, against a co-occurrence baseline. This is structure learning, not evaluated as link prediction.
- **tamura2025predicting**: Tamura, *Predicting Startup-VC Fund Matches with Structural Embeddings and Temporal Investment Data*, arXiv:2511.23364 (Nov 2025) [ABS via OpenReview record]. Fund–startup pair classification (Node2Vec, attention fusion, LSTM over fund history) on Japanese startup data; split not stated.
- **zheng2025analytics**: Zheng & Brintrup, *An Analytics-Driven Approach to Enhancing Supply Chain Visibility with GNNs and Federated Learning*, arXiv:2503.07231, 2025-03-10 [ABS], https://arxiv.org/abs/2503.07231. Federated GCN link prediction on country-level supply-chain KGs; no code.
- **lee2025knowledge**: Lee et al., *Knowledge Graph Construction for Stock Markets with LLM-Based Explainable Reasoning*, arXiv:2601.11528 (the page says submitted 2025-11-16; the ID implies a Jan 2026 listing), CIKM 2025 workshop [ABS], https://arxiv.org/abs/2601.11528. KG schema plus LLM multi-hop QA for Korean listed firms; no link prediction.
- **dai2025griffinnet**: Dai et al., *GrifFinNet: A Graph-Relation Integrated Transformer for Financial Predictions*, arXiv:2510.10387 [ABS]. Industry and institutional-ownership graphs used as *inputs* for return prediction on CSI300/800; not link prediction. Code: https://www.healthinformaticslab.org/supp/
- **hassan2026information**: Hassan & Situngkir, *Information Networks of Stock Prices*, arXiv:2606.07450, 2026-06-05 [ABS]. Pearson and mutual-information networks with MST/PMFG filtering on Indonesian stocks, 2015–2025, 2,328 rolling windows. Builds co-movement networks but does not forecast them.
- **li2026understanding**: Li et al., *Understanding Interfirm AI Talent Flow Networks through Online Professional Profiles*, arXiv:2610.08264, 2026-10-06 [ABS]. Firm→firm talent-flow network (535M employment records, 2010–2022); centrality relates to enterprise value; no link prediction. A new type of firm network.
- **zhang2026link**: Zhang & Zhao, *Link prediction in complex networks via fusing node centrality and local similarity indices: a fair-protocol reassessment*, arXiv:2609.09658, 2026-09-09 (v4 2026-10-12) [ABS]. Not finance. Its negative result matters for evaluation: **the gains vanish and reverse under a degree-corrected benchmark**, and AUC gains do not carry over to top-L hits.
- Out of window (opened, not counted as evidence): Takahashi et al., *Dynamic Link and Flow Prediction in Bank Transfer Networks*, arXiv:2409.08718 (2024). Lampert, Blöcker, Scholtes, *From Link Prediction to Forecasting: Information Loss in Batch-based Temporal Graph Learning* (ICLR 2025 rejected submission, created 2024-09-27).


---

## T3. Anomaly detection in dynamic market and company graphs

**Synthesis.** Generic dynamic-graph anomaly detection is crowded (AAAI 2026 BAG, KDD 2026, a stream of withdrawn ICLR 2026 submissions). Network-based crash or stress indicators on correlation matrices form a moderate body of mostly single-author econophysics preprints. They use 3–17 hand-picked crisis episodes, no held-out tests, survivor-only universes, and indicators that are coincident rather than leading. **Learned anomaly detection on equity/company graphs with public, event-typed labels is thin, and anomalies in signed or directed (lead-lag) edges were not found at all.** Directed lead-lag matrices are not positive semi-definite, so the "sign null" that makes signed triads uninformative on correlation matrices (acedo2026triadic) does not apply. That gives a testable claim.

Full per-paper notes (from `sweep/T3.md`; raw query log and unopened leads stay there):

##### acedo2026triadic: The Triadic Stress Index in Financial Markets
- Citation: Acedo, A., *The Triadic Stress Index in Financial Markets*, arXiv:2608.10788, 2026.
- Link: https://arxiv.org/abs/2608.10788 (full text: https://arxiv.org/html/2608.10788)
- First public: arXiv v1 2026-08-11. Venue: preprint (physics.soc-ph; cross-listed q-fin.RM and q-fin.ST). Peer-reviewed: no.
- Verification: [FT]
- Summary: Takes a network index from soil-microbiome ecology unchanged: (closed-triangle clustering × density / inverse spectral gap) × degree variance. It applies this to rolling 20-day |ρ| correlation networks in five asset classes, 2006–2026. Evaluation is unusually rigorous for this literature: out-of-sample (train 2000–2015, test 2016–2026), a fixed top-decile alarm budget, block bootstrap, Holm correction, and three crisis-label schemes. TSI beats the Absorption Ratio on out-of-sample F1. It ties effective rank and the Vendi score. Its per-node attribution scores 0.97–0.99, against 0.33–0.84 for the Bartesaghi et al. local balance index of signed networks.
- Graph type: temporal sequence of weighted, undirected snapshots (edges = |ρ|). A signed variant was tested. The paper's "sign null": signing the correlations leaves the triangle count unchanged, because positive semi-definiteness bounds triangle frustration.
- Task: graph-level stress/anomaly detection plus node-level attribution.
- Data: yfinance prices; OFR Financial Stress Index as the benchmark. Public: yes.
- Code available? Yes: https://github.com/BiomeMakers/TSI-OmegaS (confirmed via the GitHub API search).
- What it leaves open: TSI is coincident, not leading (cross-correlation peaks at lag 0). On real matrices the attribution reduces to node degree. Only symmetric correlation graphs are studied, so directed or lead-lag graphs, where the PSD "sign null" does not hold, are untested. The abstract's ΔF1 = 0.273 does not match the table's Δ = 0.219.

##### caputi2026financial: Financial Anomaly Detection for the Canadian Market
- Citation: Caputi, L., Meadows, N., *Financial Anomaly Detection for the Canadian Market*, arXiv:2604.02549, 2026.
- Link: https://arxiv.org/abs/2604.02549 (full text: https://arxiv.org/html/2604.02549)
- First public: arXiv v1 2026-04-02. Venue: preprint (q-fin.ST, cs.LG). Peer-reviewed: no.
- Verification: [FT]
- Summary: Builds one weighted graph per day from 25-day Convergent Cross Mapping correlations among 39 TSX-60 stocks (2005–2021). It compares graph-level anomaly detectors on these graphs: TDA (directed flag-complex persistent homology with Mahalanobis or LOF scoring), PCA, and graph neural detectors (GlocalKD with GINE, a one-class GIN(E)). Ground truth is 7 peaks of the Canadian Financial Stress Index. An event counts as detected if an anomalous graph appears within the 50 business days before it. Neural F1 is 0.60–0.68 and TDA F1 is 0.55–0.58, against 0.28–0.45 for PCA. A DJIA replication used OFR FSI peaks.
- Graph type: temporal sequence of weighted snapshots. Edges are CCM-based and asymmetric (the TDA uses directed flag complexes). Negative entries are set to 0, so the graphs are unsigned.
- Task: unsupervised graph-level anomaly detection.
- Data: Yahoo Finance via yfinance. Public: yes.
- Code available? Yes: https://github.com/njmead811/Financial-Anomaly-Detection-for-the-Canadian-Market (confirmed via the GitHub API search).
- What it leaves open: only 7 events; survivorship (only stocks traded over the full period); no train/test split; a fixed 97.5% threshold. Negative dependence is discarded. Alerts are not explained: no node or edge attribution.

##### chen2026velocity: Velocity- and Regime-Aware Detection of Intraday Options Market Manipulation, with Explainable Attribution
- Citation: Chen, A., Hybinette, M., *Velocity- and Regime-Aware Detection of Intraday Options Market Manipulation, with Explainable Attribution*, arXiv:2608.05373, 2026.
- Link: https://arxiv.org/abs/2608.05373 (full text: https://arxiv.org/html/2608.05373)
- First public: arXiv v1 2026-08-05. Venue: preprint (q-fin.TR, cs.LG, q-fin.ST). Peer-reviewed: no.
- Verification: [FT]
- Summary: Uses minute-level "state velocity" features (option-delta velocity or price velocity), autoencoders and HMM-regime variants, with exact SHAP attribution per alert. Thresholds are locked before an out-of-sample year. Labels come from public enforcement documents: a SEBI interim order (BANKNIFTY, 10 identified test days) and the Attachment A of the SEC v. Patel complaint (S.D.N.Y., 20 Apr 2026; 493 alleged ticker-days). The plain autoencoder recovers 10/10 days at about 24% precision. A pump-reversal shape score reaches AUC 0.91 (ARQQ) and 0.81 (ACY). Regime conditioning is reported as a negative result. Unconfirmed alerts share the attribution profile of confirmed days (cosine 0.99), which the authors take to mean that the precision ceiling comes from incomplete enforcement labels.
- Graph type: none. Each instrument is scored independently. The authors list graph-based spot–option decoupling detection as future work.
- Task: anomaly detection (market manipulation), with feature attribution.
- Data: public Kaggle BANKNIFTY minute data; SEC and SEBI documents (public). The source of the US equity minute bars is not named.
- Code available? Not stated.
- What it leaves open: no cross-stock or network structure. Labels are positive-only (closed-world precision is a floor). The equity transfer rests on two tickers. **Main transferable idea:** enforcement-document ticker-days as weak labels.

##### zhu2026anomaly: Anomaly detection in autoregressive networks
- Citation: Zhu, X., *Anomaly detection in autoregressive networks*, arXiv:2608.15047, 2026.
- Link: https://arxiv.org/abs/2608.15047 (full text: https://arxiv.org/html/2608.15047)
- First public: arXiv v1 2026-08-15. Venue: preprint (stat.ME). Peer-reviewed: no.
- Verification: [FT] (HTML read; reference list not visible)
- Summary: Splits each transition of a dynamic network into a **formation** matrix (edge appears) and a **dissolution** matrix (edge disappears). Each is embedded with unfolded adjacency spectral embedding (UASE) and compared with a stationary baseline. This gives vertex-level and network-level anomaly statistics that also say whether an anomaly is formation-driven, dissolution-driven or both. The statistics have high-probability guarantees for autoregressive random dot product graphs. A DC-SBM extension adds statistics for community merges, splits and reassignments. Applications: UN Comtrade trade (a dissolution-driven decline, then formation-driven recovery around COVID) and school contact networks.
- Graph type: temporal, binary, undirected, unsigned (LSE statistics).
- Task: anomaly detection (vertex- and network-level), community-change detection.
- Data: UN Comtrade; school contacts. Not financial-market data.
- Code available? Not stated.
- What it leaves open: no weights, signs or direction. No market application. Vertex p-values are pointwise. Autoregressive "memory" needs burn-in. **Direct template** for edge-activation and sign-flip anomaly detection in signed lead-lag graphs (gap D1).

##### hua2026bag: BAG: Benchmarking Anomaly Detection on Dynamic Graphs
- Citation: Hua, F., Qi, Y., Wei, Z., et al., *BAG: Benchmarking Anomaly Detection on Dynamic Graphs*, Proc. AAAI 40(17):14892–14900, 2026.
- Link: https://api.crossref.org/works/10.1609%2FAAAI.V40I17.38510 (Crossref record opened). The AAAI page https://ojs.aaai.org/index.php/AAAI/article/view/38510 was blocked by a bot wall.
- First public: published 2026-03-14 (AAAI 2026). An earlier arXiv version is unknown. Venue: AAAI 2026. Peer-reviewed: yes.
- Verification: [ABS]
- Summary: Benchmarks 25 models (classical and dynamic GNNs) on 10 real-world dynamic-graph datasets, with synthetic and naturally occurring ("organic") anomalies, at edge and node level. Continuous-time models beat discrete-time models on edge anomalies. All methods struggle with organic anomalies, mainly because of temporal anomalies and heavy class imbalance. Code, datasets and protocols are stated to be open source.
- Graph type: temporal (CTDG and DTDG); edge- and node-level.
- Task: anomaly-detection benchmark.
- Data: 10 real-world datasets. Which ones, and whether any are financial, is unverified.
- Code available? Stated as open source. The GitHub API search found a repo `johannedn/BAG` ("Benchmarks for anomaly detection on dynamic graphs"), but it could not be opened to confirm that it is the official one.
- What it leaves open: the hard case, organic and imbalanced temporal anomalies, is exactly the market-event setting. Nothing in the abstract suggests market or company graphs are included.

##### ito2026identifying: Identifying dynamical network markers of financial market instability
- Citation: Ito, M. I., Hasada, H., Honma, Y., et al., *Identifying dynamical network markers of financial market instability*, arXiv:2604.21297, 2026.
- Link: https://arxiv.org/abs/2604.21297
- First public: arXiv v1 2026-04-23. Venue: preprint (physics.soc-ph, physics.data-an, q-fin.RM). Peer-reviewed: no.
- Verification: [ABS]
- Summary: Applies Dynamical Network Marker (DNM) theory, which looks for critical slowing down in high-dimensional interacting systems, to Tokyo Stock Exchange order placement and execution data. Participants are identified by virtual server IDs, and each participant's trading activity is one interacting element. Early-warning signals of large price moves appear at a daily time scale.
- Graph type: implicit interaction network of market participants (multivariate time series); temporal.
- Task: early warning / anomaly (critical-transition indicators).
- Data: TSE order-level data with server IDs. Public availability is not stated and probably restricted.
- Code available? Not stated.
- What it leaves open: depends on participant-level exchange data. The authors say forecasting horizons need refining and several activity series need combining. It is untested whether DNM-style signals can be computed on public price-based stock graphs.

##### zhang2026temporal: Temporal Attentive Graph Networks for Financial Surveillance: An Incremental Multi-Scale Framework
- Citation: Zhang, W., Shen, Y., Zhou, H., et al., *Temporal Attentive Graph Networks for Financial Surveillance: An Incremental Multi-Scale Framework*, Journal of Sensor and Actuator Networks 15:23, 2026.
- Link: https://mdpi-res.com/d_attachment/jsan/jsan-15-00023/article_deploy/jsan-15-00023.pdf
- First public: published 2026-02-16 (received 2025-11-08). Venue: J. Sens. Actuator Netw. (MDPI). Peer-reviewed: yes.
- Verification: [FT]
- Summary: Builds a GAT+GRU model (TAGN) over a learnable fusion of three edge channels: 20-day price correlation (threshold 0.6), supply-chain links, and 13F institutional co-holdings. Node features include illiquidity, realized volatility, FinBERT sentiment and macro variables. The label is a maximum drawdown above 10% within 5 days. On S&P 500 data (train 2018–2022, val 2023, test 2024) it reports AUC 0.890 and P@50 61.5% against WinGNN and PatchTST. A TAGN risk index led the VIX by 1–2 weeks, including around SVB.
- Graph type: temporal, multi-relational (fused into one weighted adjacency), undirected.
- Task: node-level extreme-event prediction (supervised), presented as surveillance.
- Data: partly public (prices, 13F). The supply-chain source is given as FactSet (commercial), so provenance is inconsistent.
- Code available? Not stated.
- What it leaves open: "anomaly" here is really supervised drawdown prediction. The test covers one year. Relation types are collapsed into one adjacency, so the model does not attribute which channel carried the contagion. The venue is outside finance and ML.

##### iwata2026no: No Data Is Not No Risk: Visibility Aware Graph-Based Inference of Business Conduct Risk
- Citation: Iwata, T., Laurmaa, J., Hisano, R., *No Data Is Not No Risk: Visibility Aware Graph-Based Inference of Business Conduct Risk*, arXiv:2607.26859, 2026.
- Link: https://arxiv.org/abs/2607.26859
- First public: arXiv v1 2026-07-29. Venue: preprint (q-fin.RM, cs.LG). Peer-reviewed: no.
- Verification: [ABS]
- Summary: Treats corporate conduct incidents as positive-unlabeled (PU) node classification on a corporate ownership graph. Firms with recorded incidents are positives. All other firms are unlabeled, not negative, because missing records may reflect low visibility. A relation-type-aware GCNII is trained with non-negative PU loss. It gives the best forward-looking ranking and helps for firms with no prior incidents.
- Graph type: heterogeneous (relation-specific message passing) company ownership graph. The abstract suggests a snapshot setting rather than a dynamic one.
- Task: PU node classification / risk ranking.
- Data: incident records and an ownership graph. Sources and public availability are not stated in the abstract.
- Code available? Not stated.
- What it leaves open: not a market-price or temporal anomaly method. It is the clearest 2026 statement that "no label ≠ normal" in company networks, which carries straight over to enforcement-based market-anomaly labels (gap D2).

##### mungari2025ares: ARES: Anomaly Recognition Model For Edge Streams
- Citation: Mungari, S., Bifet, A., Manco, G., Pfahringer, B., *ARES: Anomaly Recognition Model For Edge Streams*, KDD 2026 (arXiv:2511.22078).
- Link: https://arxiv.org/abs/2511.22078
- First public: arXiv v1 2025-11-27 (v2 2026-07-30). Venue: KDD 2026 (accepted, per arXiv comment). Peer-reviewed: yes.
- Verification: [ABS]
- Summary: An unsupervised anomaly detector for temporal edge streams. GNN embeddings of node and edge properties are scored by Half-Space Trees, catching both sudden spikes and sustained bursts. A light supervised thresholding step picks a threshold from a small labelled set using the dispersion of anomaly scores. It is evaluated on cyber-attack streams.
- Graph type: continuous-time edge stream; temporal.
- Task: streaming edge anomaly detection.
- Data: cyber-security datasets.
- Code available? Not stated in the abstract.
- What it leaves open: not tested on financial edge streams, for example lead-lag edge activations or co-trading events. Few-label thresholding fits the weak-label market setting.

##### li2025explainable: Explainable Heterogeneous Anomaly Detection in Financial Networks via Adaptive Expert Routing
- Citation: Li, Z., Fan, R., *Explainable Heterogeneous Anomaly Detection in Financial Networks via Adaptive Expert Routing*, arXiv:2510.17088 (XAI-FIN'25 per the paper), 2025.
- Link: https://arxiv.org/html/2510.17088v1 (abs: https://arxiv.org/abs/2510.17088)
- First public: arXiv v1 2025-10-20 (v3 2026-08-16; v1 was read). Venue: XAI-FIN'25 workshop (Singapore), as the paper states. Peer-reviewed: unknown (workshop).
- Verification: [FT] (v1)
- Summary: Covers 100 US stocks, 2017–2024. A dynamic graph learned from embedding similarity is blended with a GICS/geography prior through a stress-dependent weight. The model combines a BiLSTM-attention temporal encoder, a GCN, a stress-gated mixture of 4 "mechanism" experts (price shock, liquidity, systemic contagion, momentum reversal) and a Market Pressure Index. Detection is unsupervised via reconstruction error. On 13 documented events it reports 12/13 detected with 3.8-day lead time, against TranAD, GHRN, DOMINANT, CoLA and EvolveGCN. Expert routing weights serve as built-in explanations.
- Graph type: temporal, homogeneous, undirected similarity edges (top-k = 20). "Heterogeneous" refers to anomaly mechanisms, not node or edge types.
- Task: anomaly detection (market-level, with mechanism attribution).
- Data: Yahoo Finance, TAQ and CRSP. Partly licensed.
- Code available? Not stated.
- What it leaves open: the authors list directed or causal edges, multi-edge-type heterogeneous graphs and mechanism-level ground truth as open. v1 has internal inconsistencies (abstract gives +30.8 pp, the table +15.4 pp; 2022 events are scored although the test period is 2023–2024). The 13-event evaluation is very small.

##### jaeger2025needles: Needles in a haystack: using forensic network science to uncover insider trading
- Citation: Jaeger, G., Yeung, W. N., Lambiotte, R., *Needles in a haystack: using forensic network science to uncover insider trading*, arXiv:2512.18918, 2025.
- Link: https://arxiv.org/abs/2512.18918 (full text: https://arxiv.org/html/2512.18918)
- First public: arXiv v1 2025-12-21. Venue: preprint (cs.SI, physics.data-an, q-fin.CP). Peer-reviewed: no.
- Verification: [FT]
- Summary: Uses 2.9M SEC Form 4 trades (2014–2024; 70,941 insiders, 9,426 firms). Insiders of the same firm are linked by a same-week trade-timing similarity kernel, with strict edge filters. Centrality plus OddBall/LOF egonet anomalies flag suspicious insiders and clusters. Results are checked against two null models (calibrated generative; constrained temporal shuffling) and cross-checked against a securities-fraud prosecution database.
- Graph type: static weighted undirected graph aggregated from temporal trade data; within-firm edges only.
- Task: anomalous node and subgraph detection (screening).
- Data: SEC EDGAR Form 4. Public: yes.
- Code available? "Upon official publication" (not yet).
- What it leaves open: no price or profitability data, no cross-firm edges (so shadow trading is missed), no temporal evolution of the network, and only anecdotal validation. Oxford network-science group (Lambiotte).

##### stenfors2025shadow: Shadow trading detection: a graph-based surveillance approach
- Citation: Stenfors, A., Li, B., Guo, T., et al., *Shadow trading detection: a graph-based surveillance approach*, Finance Research Letters 86(D):108524, 2025.
- Link: https://researchportal.port.ac.uk/en/publications/shadow-trading-detection-a-graph-based-surveillance-approach-2/
- First public: online 2025-10-08 (issue 2025-12-01). Venue: Finance Research Letters. Peer-reviewed: yes. Open access (CC BY).
- Verification: [ABS] (the DOI link to the publisher was not permitted in this session)
- Summary: Proposes AMGIN, a spatio-temporal market graph combining industry relations with short- and long-term price co-movement. It aims to detect shadow-trading risk, where non-public information about one firm is used to trade economically linked firms, motivated by SEC v. Panuwat (2024). It reportedly beats traditional statistical methods on US equity data.
- Graph type: temporal, multi-relational (industry + co-movement).
- Task: anomaly / risk detection on linked firm pairs.
- Data: US equity data (source not visible in the abstract).
- Code available? Not stated in the abstract.
- What it leaves open: how ground truth is defined is not visible from the abstract (one enforcement case motivates it). There is no link to insider filings (Form 4) or to 8-K event timing.

##### pang2025market: Market state transitions and crash early warning in the Chinese stock market
- Citation: Pang, W.-Y., Lin, L., *Market state transitions and crash early warning in the Chinese stock market*, Frontiers in Physics 13:1647667, 2025.
- Link: https://www.frontiersin.org/articles/10.3389/fphy.2025.1647667/full
- First public: online 2025-07-07, published 2025-08-07. Venue: Frontiers in Physics. Peer-reviewed: yes.
- Verification: [FT]
- Summary: Computes 3,621 rolling correlation matrices for 285 continuously listed CSI 300 stocks (2010–2024). Matrices are noise-suppressed by power mapping, embedded with MDS and clustered with k-means into 5 persistent market states. Crashes cluster in the high-correlation states. A decision tree on state-transition features gives F1 0.966, against 0.77–0.80 for the absorption ratio, largest eigenvalue and a VIX-like index.
- Graph type: temporal sequence of dense, weighted (signed) correlation matrices, treated as whole-graph objects.
- Task: regime/state detection plus crash classification.
- Data: RESSET (licensed).
- Code available? Not stated.
- What it leaves open: no held-out test set (5-fold CV on time-series data, so leakage is likely), 196 configurations tuned, survivorship, and states are coincident by the authors' own account. The regime method is transparent but not graph-learning based.

##### neela2025systemic: Systemic Risk Radar: A Multi-Layer Graph Framework for Early Market Crash Warning
- Citation: Neela, S., *Systemic Risk Radar: A Multi-Layer Graph Framework for Early Market Crash Warning*, arXiv:2512.17185, 2025.
- Link: https://arxiv.org/abs/2512.17185 (full text: https://arxiv.org/html/2512.17185v1)
- First public: arXiv v1 2025-12-19. Venue: preprint (q-fin.RM, cs.AI, cs.LG). Peer-reviewed: no.
- Verification: [FT]
- Summary: Proposes multi-layer market graphs (correlation, sector/factor, sentiment) for crash warning, but only the correlation layer (7-day Spearman, |ρ| ≥ 0.5; 44 stocks) is evaluated. Snapshot GCN and GCN+GRU models are compared with LR and RF on the dot-com, GFC and COVID periods. AUROC varies wildly (for example, the temporal GNN scores 0.59 on dot-com but 0.23 on GFC, and COVID is undefined). No model dominates.
- Graph type: temporal correlation snapshots (multi-layer proposed, not tested).
- Task: node-level future-drawdown classification.
- Data: yfinance, FRED. Public: yes.
- Code available? Not stated.
- What it leaves open: inconsistent horizons and periods across sections and a tiny universe. Useful mainly as evidence that naive GNN crash prediction on correlation graphs is fragile. The same single author also wrote AIMM (A2).

---

#### A2. Other opened papers (short entries, lower relevance or adjacent)
Each was opened with WebFetch. Format: key: title; link; first public; verification; one-line relevance.

**2026**
- **ozimek2026cross**: Cross-sectional topological anomaly scores and intraday return predictability in the S&P 500 (BallMapper + decoder-conditional VAE + function-on-function regression); https://arxiv.org/abs/2606.08586; v1 2026-06-07, preprint; [ABS]. Stock-level anomaly score that conditions on market topology and peer context. Only 10 stocks; used for return prediction, not alerts.
- **hammond2026geometric**: Geometric Observables for Financial Regime Detection; https://arxiv.org/abs/2605.17117; v1 2026-05-16 (v2 05-27), preprint; [ABS]. 46 baselines, 17 crises, walk-forward nested selection. Its geometric signals are uncorrelated with the Absorption Ratio. Index-level, not a graph method, but a good evaluation template.
- **yuan2026saliency**: A Saliency-Fused Spatio-Temporal GNN for Systemic Risk Early Warning in China's Financial-Real Economy; https://scholars.cityu.edu.hk/en/publications/a-saliency-fused-spatio-temporal-graph-neural-network-for-systemi/; IEEE Access 14:36573–36588, 2026 (online 2026-03-04), peer-reviewed; [ABS]. Node-saliency-weighted spatio-temporal GNN predicting threshold-defined risk events at 7- and 15-day horizons. Saliency rose before the 2015 crash.
- **deleonmiranda2026detecting**: Detecting Network Instability via Multiscale Detrended Cross-Correlations and MST Topology; https://arxiv.org/abs/2602.10174; v1 2026-02-10, preprint; [ABS]. "Elastic DCCR" (scale-elasticity of MST length) rises sharply in stress on a global equity-index network. Kapetanios is a co-author.
- **liu2026modeling**: Modeling Spectral Energy Shifts in Spatio-Temporal Graph Anomaly Detection; https://arxiv.org/abs/2606.00304; v1 2026-05-29 (v2 08-11), preprint; [ABS]. General method: "camouflaged" anomalies reduce spectral-energy variation, and the energy-aware message passing works for static and time-series graphs. Not tested on finance.
- **mukhia2026core**: Core-Periphery Dynamics in Market-Conditioned Financial Networks (conditional p-threshold MI + MST); https://arxiv.org/abs/2601.00395; v1 2026-01-01, preprint; [ABS]. COVID-crash reorganisation in 4 markets (weaker core-periphery, disassortativity). Descriptive.
- **ansari2026recovering**: Recovering Structural Organization in Noisy Correlation Networks Using Financial Systems as a Testbed; https://arxiv.org/abs/2607.10297; v1 2026-07-11, preprint; [ABS]. Marchenko-Pastur denoising before building the network. A relevant preprocessing step, since anomalies in raw correlation graphs can be noise.
- **carvalho2026reconfiguration**: The Reconfiguration Premium: Co-movement Structure as an Unspanned Dimension of the Variance Risk Premium; https://arxiv.org/abs/2608.20020; v1 2026-08-20, preprint; [ABS]. The turning rate of S&P 500 correlation eigenspaces (principal angles) is priced (t = 5.40). Structure change carries economic content beyond correlation level.
- **garciaseuma2026where**: Where does the criticality live? Early-warning signals are event-heterogeneous across seven crypto-perpetual liquidation cascades; https://arxiv.org/abs/2607.27070; v1 2026-07-29, preprint; [ABS]; [crypto, transferable]. Critical-slowing-down signals are absent in exogenous news shocks: a caution for any early-warning claim.
- **zelvyte2026multiplex**: A Multiplex Network Hawkes Model for Systemic Risk Measurement; https://arxiv.org/abs/2606.15755; v1 2026-06-14, preprint; [ABS]. CDS of 99 firms; covariate-driven excitation layers give sparse directed contagion pathways. Overlaps with the credit-contagion theme.
- **castillopereda2026systemic**: Systemic Risk and Default Cascades in Global Equity Markets (Gai-Kapadia on co-movement networks); https://arxiv.org/abs/2604.19796; v1 2026-04-10, preprint; [ABS]. Cascade simulation on thresholded correlation networks, 30 assets. Weak; adjacent theme.
- **wang2026unsupervised**: Unsupervised Graph Modeling for Anomaly Detection in Accounting Subject Relationships; https://arxiv.org/abs/2604.26216; v1 2026-04-29, preprint; [ABS]. Edge-reconstruction anomaly scores on intra-firm accounting graphs. Transferable edge-scoring design.
- **hassan2026information**: Information Networks of Stock Prices (Pearson vs MI, MST/PMFG, 4 community algorithms, Indonesia); https://arxiv.org/html/2606.07450v1; 2026 (exact date not shown), preprint; [FT]. "Residual MI" (non-linear co-movement beyond Gaussian) is offered as a diagnostic, not as anomaly detection.
- **huang2026cross**: Cross-Stock Predictability via LLM-Augmented Semantic Networks; https://arxiv.org/html/2604.19476; v2 2026-04-26 (v1 date not shown), preprint; [FT]. 10-K embedding graph with LLM-labelled relation types (competitor, supply chain, ...). A possible "explain-away" relation graph for gap D6.
- **alridhawi2026adaptive**: Adaptive Regime-Aware Stock Price Prediction Using Autoencoder-Gated Dual Node Transformers with RL Control; https://arxiv.org/abs/2603.19136; 2026-03-19 (v2 04-02), preprint; [ABS]. Autoencoder anomaly gate for routing forecasts. Tangential.
- **wade2026do**: Do Better Volatility Forecasts Lead to Better Portfolios? Evidence from GNNs; https://arxiv.org/abs/2605.19278; 2026-05-19, preprint; [ABS]. Tangential (correlation, sector and Granger graphs for volatility).
- **regier2026dynamic**: Dynamic Link Prediction with Temporally Enhanced Signed GNNs; https://arxiv.org/abs/2605.26290; v1 2026-05-25, preprint; [ABS]. Temporal signed GNN (Bitcoin OTC/Alpha). A signed temporal encoder candidate; belongs to the link-prediction theme.
- **blando2026network**: Network Topology Analysis and ML Techniques for Systemic Risk Prediction in U.S. Equity Markets (MSc thesis, Tor Vergata); https://stefano-blando.github.io/publications/network-crash-prediction/; page dated 2026-01-15 (BibTeX gives 2025); [ABS]. Code: https://github.com/stefano-blando/systemic-risk-prediction. Evidence that this is MSc-thesis-sized territory.

**2025**
- **girnyk2025correlation**: Correlation Structures and Regime Shifts in Nordic Stock Markets; https://arxiv.org/abs/2601.06090; v1 2025-12-31, preprint (q-fin.PM); [ABS]. Eigenvalue-ratio crisis indicator (λ1 up, λ2 down) feeding portfolio construction.
- **guritanu2025topological**: Topological Machine Learning for Financial Crisis Detection: Early Warning Signals from Persistent Homology; https://www.mdpi.com/2073-431X/14/10/408; Computers 14(10):408, 2025-09-24, peer-reviewed; [FT]. Persistence-landscape norm on 4 US indices with a causal alarm rule: F1 about 0.50, about 34-day lead, 2 of 4 crises anticipated. Uses giotto-tda.
- **ying2025spatio**: Spatio-Temporal Decoupled Heterogeneous Graph Network for Systemic Risk Prediction (STDHGN); https://pure.ecnu.edu.cn/en/publications/spatio-temporal-decoupled-heterogeneous-graph-network-forsystemic/; ADMA 2025 (LNCS 16200), peer-reviewed; [ABS]. Heterogeneous spatio-temporal GNN identifying high-risk entities in US and Chinese data (ECNU, Dawei Cheng group).
- **neela2025aimm**: AIMM: An AI-Driven Multimodal Framework for Detecting Social-Media-Influenced Stock Market Manipulation; https://arxiv.org/abs/2512.16103; v1 2025-12-18, preprint; [ABS]. AIMM-GT has 33 labelled ticker-days from SEC actions. Social features are *synthetic* (Reddit API limits). Very small label set.
- **zhang2025systemic**: Systemic Risk and Bank Networks: The Use of a Knowledge Graph with Generative AI; OpenReview record via https://api2.openreview.net/notes/search?term=Systemic%20Risk%20and%20Bank%20Networks%20Knowledge%20Graph&limit=5; NeurIPS 2025 GenAI-in-Finance workshop poster (created 2025-08-31); [ABS]. News-derived knowledge graphs of banks in 2007–2009 show dense links among troubled banks. A supervisor-fit (text → KG) example.
- **deng2025statistical**: Statistical analysis and applications of financial network data in the era of digital intelligence (review); https://www.aimspress.com/aimspress-data/dsfe/2025/4/PDF/DSFE-05-04-021.pdf; DSFE 5(4):536–556, 2025-11-20, peer-reviewed; [FT]. Review of complex, higher-order and GNN systemic-risk networks. Open challenges: causal interpretability, counterfactual evaluation, dynamic higher-order scaling.
- **lin2025detecting**: Detecting Multilevel Manipulation from Limit Order Book via Cascaded Contrastive Representation Learning; https://arxiv.org/abs/2508.17086; v1 2025-08-23 (v2 10-10), preprint; also an ICLR 2026 *withdrawn* submission (OpenReview API); [ABS]. Spoofing detection from LOB data, single-asset, no graph.
- **li2025crisis**: Crisis-Resilient Portfolio Management via Graph-based Spatio-Temporal Learning (CRISP); https://arxiv.org/abs/2510.20868; v1 2025-10-23, preprint; [ABS]. Same authors as li2025explainable. Attention to a defensive cluster rises in crises (an "interpretable regime signal"). Tangential.

**Older background (max 2)**
- **[BACKGROUND] balmaseda2023predicting**: Predicting systemic risk in financial systems using Deep Graph Learning; https://repositorio.comillas.edu/items/76bbe62c-bf11-4d4c-8880-75e1e511fe71/full; Intelligent Systems with Applications 19, 2023; [ABS]. Early GNN-for-systemic-importance reference.
- **[BACKGROUND] chullamonthon2022transformer**: A Transformer Model for Stock Price Manipulation Detection in the Stock Exchange of Thailand; https://kirim.kmutt.ac.th/converis/portal/detail/Publication/986842852?lang=en_GB; ECTI-CON 2022; [ABS]. Pattern for "train on synthetic pump-and-dump, test on regulator-prosecuted cases" (5/6 SEC Thailand cases).

---


---

## T4. Heterogeneous (temporal) graphs and knowledge graphs of companies

**Synthesis.** "LLM extracts a financial knowledge graph, then RAG or a heterogeneous GNN predicts the trend" is crowded (≥ 10 papers in 2025–2026, one ICLR 2026 rejection). **Temporal KG completion on company KGs is unexplored in 2025–2026**, even though FinReflectKG (ICAIF 2025) now ships 17.5M time-stamped triples for 743 S&P 500 firms, 2014–2024, under CC-BY-NC. Also thin: systematic study of extraction choices, rigorous tests of path explanations, and investor–company graphs on public data. The last is the supervisor's earlier Crunchbase line, which still depends on PitchBook. Verdict: **moderate overall, uneven; the thin parts match the supervisor's profile.**

Full per-paper notes (from `sweep/T4.md`; raw query log and unopened leads stay there):

##### ding2026trace — TRACE: Temporal Rule-Anchored Chain-of-Evidence on Knowledge Graphs for Interpretable Stock Movement Prediction
- Citation: Q. Ding, H. Shi, L. Castejón Lozano et al., *TRACE: Temporal Rule-Anchored Chain-of-Evidence on Knowledge Graphs for Interpretable Stock Movement Prediction*, arXiv 2603.12500, 2026.
- Link: https://arxiv.org/abs/2603.12500 ; https://arxiv.org/html/2603.12500v1
- First public: 2026-03-12 (v1); Venue: preprint; Peer-reviewed: no
- Verification: [FT]
- Summary: Builds a temporal company KG with validity intervals per triple (42k nodes: Company, Event, Product, Person, Financial, TextSource; 174k edges; 11 relations such as SELLS, INVESTED_IN, ACQUIRED, SUED, CAUSED_INCREASE/DECLINE). It mines 2–4-hop relation-path rules by support/confidence (≥0.60), explores the KG along rule-admissible paths, grounds the paths in same-period news, and an LLM consolidates them into UP/DOWN verdicts with a readable evidence chain. Reports 55.1% accuracy on S&P 500 (487 stocks).
- Graph type: temporal (as-of filtered), heterogeneous, directed KG; text-grounded.
- Task: classification (next-day direction) + path explanation (rule mining = a form of KG reasoning).
- Data: S&P 500 OHLCV; news/earnings reports/filings, publishers not named. KG built 2022-01→2023-01, test 2023. Public? partly (prices yes, KG not released).
- Code available? not stated.
- What it leaves open: Evaluation is weak (my reading of the FT): one train year and one test year; rule-mining period not stated (possible leakage); defaults to DOWN when no rule fires (~15% of cases); the label is set before the LLM-validation step; agent baselines are self-built "proxies"; no transaction costs. The extractor used to build the KG is not described. Explanations are not evaluated for faithfulness.

##### huang2026crossstock — Cross-Stock Predictability via LLM-Augmented Semantic Networks
- Citation: Y. Huang, Z. Fan, K. Hu et al., *Cross-Stock Predictability via LLM-Augmented Semantic Networks*, arXiv 2604.19476, 2026.
- Link: https://arxiv.org/abs/2604.19476 ; https://arxiv.org/html/2604.19476
- First public: 2026-04-21 (v1; v2 2026-04-26); Venue: preprint; Peer-reviewed: no
- Verification: [FT]
- Summary: Builds a top-K (K=5) cosine-similarity firm graph from 10-K risk-factor embeddings, updated yearly. DeepSeek-Chat (temperature 0, firms anonymised) then labels each candidate edge as competitor / supply_chain / complementary / substitute / peer / unrelated. Competitor edges are removed and substitutes down-weighted before pair-level mean-reversion signals are aggregated. The long-short Sharpe rises from 0.742 to 0.820 (S&P 500, 2011–2019).
- Graph type: temporal (annual snapshots), typed edges (multi-relational after LLM labelling), undirected.
- Task: other (graph refinement → return-signal construction / ranking).
- Data: CRSP (licensed), SEC 10-K text (public). Public? partly.
- Code available? not stated.
- What it leaves open: No human validation or accuracy figures for the LLM edge labels. The effect of the label taxonomy, the LLM choice and K on the result is not studied. LLM look-ahead/memorisation is not tested beyond anonymisation. No transaction costs. Sample ends in 2019.

##### fukada2026snippetdriven — Snippet-Driven Supply Chain Discovery with LLMs: Scaling Visibility in China
- Citation: H. Fukada, T. Mizuno, *Snippet-Driven Supply Chain Discovery with LLMs: Scaling Visibility in China*, arXiv 2605.27845, 2026.
- Link: https://arxiv.org/abs/2605.27845 ; https://arxiv.org/html/2605.27845v1
- First public: 2026-05-27; Venue: DOI 10.1109/CIFEr67845.2026.11692384 (suggests IEEE CIFEr 2026; venue name not stated on page); Peer-reviewed: probably yes (unverified)
- Verification: [FT]
- Summary: Uses Google search snippets (via the Serper API) as cheap evidence. Qwen3-Next-80B extracts SUPPLIER/CUSTOMER/PARTNER/GROUP edges, with 3-stage entity resolution (exact, then Wikidata/Orbis aliases, then Jaro-Winkler). Seeded with 130,685 Chinese firms, it yields 7.2× more listed firms and 9.3× more relations than CSMAR (listed-only: 50,600 nodes, 61,086 edges). Each edge keeps provenance and a source-credibility tier.
- Graph type: static, directed, typed (multi-relational) firm graph with provenance.
- Task: other (graph construction / relation extraction).
- Data: web snippets (paid API), CSMAR and Orbis (licensed). Public? no (no release statement).
- Code available? not stated.
- What it leaves open: No precision/recall and no human audit. Overlap with CSMAR edges is not reported. Edges have no timestamps. Downstream use (contagion, returns) is untested. The authors state that salience ≠ materiality.

##### niu2026relational — Relational Probing: LM-to-Graph Adaptation for Financial Prediction
- Citation: Y. Niu, C. Jin, R. Dolphin, R. Dong, *Relational Probing: LM-to-Graph Adaptation for Financial Prediction*, arXiv 2604.10212, 2026.
- Link: https://arxiv.org/abs/2604.10212
- First public: 2026-04-11; Venue: preprint; Peer-reviewed: no
- Verification: [ABS]
- Summary: Replaces the LM head with a "relation head" that induces an entity graph directly from hidden states. The head is trained end-to-end with a stock-trend predictor, using Qwen3 0.6B/1.7B/4B small LMs on a single 24 GB GPU, and beats a co-occurrence graph baseline.
- Graph type: learned (latent) entity graph from text; homogeneity unclear from abstract.
- Task: structure learning + classification (stock trend).
- Data: not named in abstract. Public? unknown.
- Code available? not stated.
- What it leaves open: The induced graph is task-tuned, not an inspectable KG. It is unknown whether its edges match real economic relations. Its edges are not compared with LLM-extracted triples.

##### wesslund2026llmbased — LLM-based Triplet Extraction from Financial Reports
- Citation: D. Wesslund, V. Stenström, P. Linde et al., *LLM-based Triplet Extraction from Financial Reports*, arXiv 2602.11886, 2026.
- Link: https://arxiv.org/abs/2602.11886
- First public: 2026-02-12; Venue: preprint; Peer-reviewed: no
- Verification: [ABS]
- Summary: Semi-automated subject–predicate–object extraction from annual reports, comparing a hand-built static ontology with automatic document-specific ontology induction across several LLMs. Induced ontology: 100% schema conformance, no ontology drift. Regex + LLM-judge verification cuts measured subject hallucination from 65.2% to 1.6%. Subject and object hallucinations are asymmetric (passive voice in financial prose).
- Graph type: static KG (construction only).
- Task: other (relation extraction / KG construction evaluation).
- Data: two annual reports. Public? reports are public; pipeline not released.
- Code available? not stated.
- What it leaves open: Only two reports and no gold standard. The effect of ontology choice on graph topology or any downstream task is unmeasured.

##### zhang2026llmenhanced — LLM-Enhanced Dynamic Financial Knowledge Graphs for Cross-Entity Signal Propagation and Alpha Discovery
- Citation: L. Zhang, *LLM-Enhanced Dynamic Financial Knowledge Graphs for Cross-Entity Signal Propagation and alpha discovery*, arXiv 2607.10932, 2026.
- Link: https://arxiv.org/abs/2607.10932
- First public: 2026-07-12; Venue: preprint (stat.AP); Peer-reviewed: no
- Verification: [ABS]
- Summary: An LLM turns documents into structured events and explicit and implicit firm–firm links (supplier, customer, competitor, technology partner), forming a dynamic KG. Event signals diffuse more strongly within detected communities. Two signals (Community Information Surprise, Propagated Information Surprise) and econometric tests are proposed. Evidence comes from simulations only, one of them calibrated to the Russell 1000.
- Graph type: temporal, multi-relational firm KG with communities.
- Task: other (signal propagation / return predictability).
- Data: simulated (calibrated to the Russell 1000). Public? n/a.
- Code available? not stated.
- What it leaves open: No real-data test. Whether LLM-extracted implicit links carry spillover information in actual returns is untested.

##### assadihaghi2026dynamic — Dynamic Taxonomy Construction and Thematic Filtering for Financial Knowledge Graphs
- Citation: A. Assadihaghi, R. Amini, D. Schiemert et al., *Dynamic Taxonomy Construction and Thematic Filtering for Financial Knowledge Graphs*, ICLR 2026 Workshop on Advances in Financial AI, 2026.
- Link: https://www.iclr.cc/virtual/2026/10016543 (OpenReview forum 3A6Vo4HsD8)
- First public: 2026 (ICLR 2026 workshop, April–May 2026); Venue: ICLR 2026 workshop (poster); Peer-reviewed: yes (workshop)
- Verification: [ABS]
- Summary: LLM pipeline that builds a financial KG from daily earnings-call transcripts. Extracted entities and relations are mapped to parent categories of an adaptive taxonomy, which is then used for theme-based graph filtering to speed up graph search for investors.
- Graph type: dynamic KG (daily transcripts), heterogeneous.
- Task: other (KG construction, taxonomy induction, retrieval).
- Data: earnings-call transcripts (source not stated). Public? unknown.
- Code available? not stated.
- What it leaves open: No predictive or link-level evaluation is visible. The taxonomy's stability over time and its effect on downstream tasks are unmeasured.

##### capozzi2026agentic — Agentic Graph Retrieval-Augmented Generation for Auditable Commercial Registry Analysis
- Citation: A. Capozzi, D. Helbing, *Agentic Graph Retrieval-Augmented Generation for Auditable Commercial Registry Analysis*, IEEE Access 14:139317–139336, 2026 (arXiv 2605.18770).
- Link: https://arxiv.org/abs/2605.18770
- First public: 2026-04-15 (v1; v3 2026-09-29); Venue: IEEE Access 2026; Peer-reviewed: yes
- Verification: [ABS]
- Summary: Converts Swiss Official Gazette of Commerce publications into a Neo4j KG (>5M nodes, 4.7M relationships). Structured fields are ingested deterministically, an LLM extracts actors from unstructured notices, and a deterministic identity-resolution layer follows. An agent with restricted graph tools answers questions: factual correctness 0.83 vs 0.26 for flat RAG.
- Graph type: temporal (dated notices), heterogeneous company–person registry KG.
- Task: other (KG construction + QA); entity resolution.
- Data: Swiss SOGC (public registry). Public? source yes; KG not stated.
- Code available? not stated.
- What it leaves open: Registry KGs are not linked to market data. Ownership and board-interlock networks from public registries are not used for link prediction, anomaly detection or contagion.

##### nasir2026ontokgeq — OntoKG-EQ: A Provenance-Grounded, Competency-Question-Governed Knowledge Graph for Auditable Analyst Querying
- Citation: F. Nasir, M. A. Saeed, M. Ehsan et al., *OntoKG-EQ: ...*, arXiv 2609.08869, 2026.
- Link: https://arxiv.org/abs/2609.08869
- First public: 2026-09-08; Venue: preprint; Peer-reviewed: no
- Verification: [ABS]
- Summary: A small core ontology tied to five fixed competency questions, SHACL-style shapes, and a provenance-aware KG for emerging-market equities (Pakistan, Malaysia, Indonesia). Explanations trace each answer to observations and sources. Eight open LLMs are compared on transcription consistency (provenance coverage 0.00–1.00). A 17-person user study shows more perceived trust.
- Graph type: static/time-aware KG, heterogeneous, ontology-governed (semantic-web stack).
- Task: other (auditable querying, explanation).
- Data: curated market data for 3 markets. Public? authors say code and data are released (no URL on page).
- Code available? stated as open, URL not given.
- What it leaves open: No learning task. Shows the semantic-web governance angle (ontology + provenance) is almost absent from GNN-style finance work.

##### umeorah2026temporal — A Temporal Multiplex Graph Neural Network for Systemic Risk Transmission in Global Banking
- Citation: N. Umeorah, T. Fadina, *A Temporal Multiplex Graph Neural Network for Systemic Risk Transmission in Global Banking*, arXiv 2608.27295, 2026.
- Link: https://arxiv.org/abs/2608.27295
- First public: 2026-08-27; Venue: preprint; Peer-reviewed: no
- Verification: [ABS]
- Summary: Quarterly panel of bank fundamentals, CDS spreads and macro data as a dynamic multiplex. Layers are financial similarity, liquidity co-movement and country-level macro links. GCN+GRU with a learnable fusion gate over channels forecasts CDS changes. Systemic-importance rankings, country spillovers, and transmission paths via edge perturbation are reported.
- Graph type: temporal, heterogeneous multiplex (bank + country layers).
- Task: regression (CDS changes) + explanation (edge perturbation).
- Data: bank fundamentals, CDS, macro (CDS usually licensed). Public? partly/unknown.
- Code available? not stated.
- What it leaves open: Banks only. Edges are similarity-based, not contractual or ownership links. Non-financial listed firms and credit→equity spillover are not covered.

##### zhang2026gametheoretic — Game-Theoretic Modeling of Heterogeneous Investor Interactions for Stock Price Forecasting
- Citation: Y. Zhang, X. Wu, Y. Jia, C. Sun, *Game-Theoretic Modeling of Heterogeneous Investor Interactions for Stock Price Forecasting*, arXiv 2605.23953, 2026.
- Link: https://arxiv.org/abs/2605.23953
- First public: 2026-05-11; Venue: preprint; Peer-reviewed: no
- Verification: [ABS]
- Summary: Embeds game-theoretic interactions among investor *types* around a target stock into a heterogeneous graph with temporal positional encoding, then forecasts prices with an HGNN on two benchmark datasets.
- Graph type: temporal, heterogeneous (investor-type nodes + stock).
- Task: regression (price forecasting).
- Data: two "real-world benchmark datasets" (unnamed in abstract). Public? unknown.
- Code available? not stated.
- What it leaves open: The investor nodes are abstract types, not real holders (e.g. 13F funds). Another return-prediction paper, so the task is crowded.

##### fanshawe2026forecasting — Forecasting Equity Correlations with Hybrid Transformer Graph Neural Network
- Citation: J. Fanshawe, R. Masih, A. Cameron, *Forecasting Equity Correlations with Hybrid Transformer Graph Neural Network*, arXiv 2601.04602, 2026.
- Link: https://arxiv.org/abs/2601.04602v1
- First public: 2026-01-08; Venue: preprint; Peer-reviewed: no
- Verification: [ABS]
- Summary: A Temporal-Heterogeneous GNN (Transformer + GAT) forecasts 10-day-ahead pairwise S&P 500 correlation deviations from a rolling baseline in Fisher-z space. Inputs include sector structure and macro signals. Out-of-sample 2019–2024, it lowers error, and the forecasts are used for graph-clustered basket trading.
- Graph type: temporal, heterogeneous (stocks + sector structure), weighted.
- Task: regression on edges (edge-weight forecasting ≈ weighted link prediction).
- Data: S&P 500 prices, sectors, macro. Public? mostly (free price data feasible).
- Code available? not stated.
- What it leaves open: Edge-level forecasting of *correlation* only. Semantic (KG) relations are not used as inputs, and there is no signed or typed edge prediction.

##### arun2025finreflectkg — FinReflectKG: Agentic Construction and Evaluation of Financial Knowledge Graphs
- Citation: A. Arun, F. Dimino, T. P. Agarwal et al., *FinReflectKG: Agentic Construction and Evaluation of Financial Knowledge Graphs*, ICAIF 2025 (arXiv 2508.17906).
- Link: https://arxiv.org/abs/2508.17906 ; https://arxiv.org/html/2508.17906v2 ; dataset card https://huggingface.co/datasets/domyn/FinReflectKG/blob/main/README.md
- First public: 2025-08-25 (v1; v2 2025-10-07); Venue: ICAIF '25 (DOI 10.1145/3768292.3770363); Peer-reviewed: yes
- Verification: [FT]
- Summary: Schema-guided extraction of 5-tuples from S&P 100 10-Ks (2024) in three modes (single-pass, multi-pass, reflection-agent) with Qwen2.5-72B, judged by rules plus Qwen3-32B as LLM judge. Reflection gives the best rule compliance (64.8%) and coverage but the lowest entropy (more compact graph). Single-pass gives the highest faithfulness. Entity types include ORG, COMP, PRODUCT, SEGMENT, RISK_FACTOR, ESG_TOPIC. Relations include Has_Stake_In, Supplies, Partners_With, Operates_In, Impacted_By. **Dataset card (opened):** the released KG now covers **743 S&P 500 companies, 10-Ks 2014–2024, 17.5M triplets, each with start_date/end_date** and source chunk; CC-BY-NC-4.0.
- Graph type: (released version) temporal, heterogeneous, directed KG with provenance.
- Task: other (KG construction + evaluation).
- Data: SEC EDGAR 10-K. Public? yes (CC-BY-NC-4.0).
- Code available? framework + S&P-100 data via https://anonymous.4open.science/r/KG-Financial-Datasets-SP-100-529B/README.md (stated in the paper).
- What it leaves open: The authors list temporal KGs as future work, and the 17.5M-triple temporal release has no link-prediction / KG-completion benchmark. Cross-document co-reference is only partial, so the same firm may appear under several surface names. The judge is LLM-based (possible bias). Different extraction modes give *different graphs* (entropy, coverage), and the downstream consequences are unexamined.

##### arun2025finreflectkgmultihop — FinReflectKG-MultiHop: Financial QA Benchmark for Reasoning with Knowledge Graph Evidence
- Citation: A. Arun, B. Sarmah, R. R. Harsh, S. Pasquali, *FinReflectKG - MultiHop: ...*, arXiv 2510.02906, 2025.
- Link: https://arxiv.org/abs/2510.02906v1
- First public: 2025-10-03; Venue: preprint; Peer-reviewed: no
- Verification: [ABS]
- Summary: Mines frequent 2–3-hop subgraph patterns per GICS sector from the temporally indexed FinReflectKG (S&P 100, 2022–2024) and generates analyst-style questions with exact KG evidence. KG-linked retrieval gives about +24% correctness and about 84.5% fewer tokens than page-window retrieval. A curated 555-QA subset is released.
- Graph type: temporal, heterogeneous KG.
- Task: other (multi-hop QA / retrieval).
- Data: SEC 10-K. Public? yes (KG on HF; QA URL not on page).
- Code available? not stated.
- What it leaves open: The KG is used only as retrieval context. Predictive tasks on the KG itself (missing or future triples, anomalies) are not attempted.

##### dimino2025finreflectkg — FinReflectKG-EvalBench: Benchmarking Financial KG with Multi-Dimensional Evaluation
- Citation: F. Dimino, A. Arun, B. Sarmah, S. Pasquali, *FinReflectKG -- EvalBench: ...*, arXiv 2510.05710, 2025.
- Link: https://arxiv.org/abs/2510.05710
- First public: 2025-10-07 (v2 2026-03-19); Venue: preprint; Peer-reviewed: no
- Verification: [ABS]
- Summary: Benchmark and judging protocol (commit-then-justify, with controls for position, leniency, verbosity and world-knowledge bias) for KG extraction from 10-Ks. Each triple gets binary faithfulness, precision and relevance judgments, and each chunk a comprehensiveness grade. Reflection wins on comprehensiveness, precision and relevance; single-pass wins on faithfulness.
- Graph type: static/temporal KG (evaluation of extraction).
- Task: other (extraction evaluation).
- Data: SEC 10-K (S&P 100). Public? source yes.
- Code available? not stated.
- What it leaves open: Quality is judged triple by triple. Whether higher triple quality changes graph-level structure or downstream (predictive/financial) value is not measured.

##### li2025finkario — FinKario: Event-Enhanced Automated Construction of Financial Knowledge Graph
- Citation: X. Li, P. Sun, W. Zhou et al., *FinKario: Event-Enhanced Automated Construction of Financial Knowledge Graph*, ACL 2026 (Long), pp. 9826–9845 (arXiv 2508.00961).
- Link: https://aclanthology.org/2026.acl-long.446.pdf ; https://arxiv.org/abs/2508.00961
- First public: 2025-08-01 (arXiv v1); Venue: ACL 2026 main (July 2026); Peer-reviewed: yes
- Verification: [FT]
- Summary: Builds a KG from Chinese equity research reports (East Money, Aug 2024–Feb 2025): 305,360 entities, 210,328 triples, 19 relation types. The schema uses professional templates and FIBO priors. FinKario-RAG (two-stage graph retrieval) improves LLM stock-trend prediction in backtests.
- Graph type: event-enhanced, heterogeneous KG with periodic updates (temporal in the sense of event timestamps).
- Task: other (KG construction) + classification (trend via RAG).
- Data: East Money reports, Tushare prices, Wind classifications. Public? partly (code yes; Wind licensed).
- Code available? https://github.com/Jackson906E/FinKario (stated in paper; repo not opened).
- What it leaves open: Short window (~6 months) and China only. The paper does not frame the KG for link prediction or completion. The KG's value is measured only through LLM trend accuracy.

##### sun2025intercorprel — InterCorpRel-LLM: Enhancing Financial Relational Understanding with Graph-Language Models
- Citation: Q. Sun, J. Zheng, B. Jin et al., *InterCorpRel-LLM: ...*, arXiv 2510.09735, 2025.
- Link: https://arxiv.org/abs/2510.09735 ; https://arxiv.org/html/2510.09735v1
- First public: 2025-10-10; Venue: preprint; Peer-reviewed: no
- Verification: [FT]
- Summary: Pretrained GraphGPT GNN + Vicuna-7B with one trained alignment layer. Stage 1: graph↔company-name matching. Stage 2: SIC-industry classification + directed supplier→customer link prediction on FactSet 2023 (3,211 US firms, 11,635 edges; node features are LLM summaries of annual reports). Reaches inductive F1 0.854 vs GAT 0.536 and GPT-5 0.229, and transfers zero-shot to competitor identification (F1 0.777).
- Graph type: static (single year), directed, homogeneous firm graph with text features.
- Task: link prediction (supply relation), node classification (industry).
- Data: FactSet Supply Chain (proprietary), commercial competitor data. Public? no.
- Code available? not stated.
- What it leaves open: A single snapshot with a random firm-level 9:1 split. Negative sampling is under-specified, and no temporal (future-edge) evaluation is done. The data is proprietary, so a public-data version (e.g. FinReflectKG `Supplies` edges) is untested.

##### li2025cmag — C-MAG: Cascade Multimodal Attributed Graphs for Supply Chain Link Prediction
- Citation: Y. Li, Z. Tang, J. Zhuang et al., *C-MAG: Cascade Multimodal Attributed Graphs for Supply Chain Link Prediction*, KDD 2025 AI4SupplyChain workshop (arXiv 2508.08071).
- Link: https://arxiv.org/abs/2508.08071
- First public: 2025-08-11; Venue: KDD 2025 workshop; Peer-reviewed: yes (workshop)
- Verification: [ABS]
- Summary: Introduces PMGraph, a public heterogeneous multimodal manufacturer–product benchmark (8,888 manufacturers, 70k+ products, 110k+ edges, 29k images). C-MAG fuses text and image attributes and then runs multiscale hetero message passing for link prediction.
- Graph type: static, heterogeneous bipartite (manufacturer–product), multimodal.
- Task: link prediction.
- Data: PMGraph (stated public). Public? yes (link not on abstract page).
- Code available? not stated.
- What it leaves open: Mostly non-listed manufacturers with no link to financial markets. Static, and no temporal split.

##### wang2025corporate — Corporate Fraud Detection in Rich-yet-Noisy Financial Graph
- Citation: S. Wang, Z. Zhang, L. Fang et al., *Corporate Fraud Detection in Rich-yet-Noisy Financial Graph*, arXiv 2502.19305, 2025.
- Link: https://arxiv.org/abs/2502.19305
- First public: 2025-02-26 (v2 2025-05-29); Venue: preprint; Peer-reviewed: unknown
- Verification: [ABS]
- Summary: 18 years of Chinese listed-company records turned into three labelled company graphs, where non-company nodes greatly outnumber companies. Identifies "information overload" (noisy non-company nodes swamp GCN message passing) and hidden fraud (label noise). KeGCN_R uses KG embeddings plus robust two-stage learning.
- Graph type: heterogeneous company KG (static per dataset).
- Task: anomaly detection / node classification (fraud).
- Data: Chinese financial records (source not stated on abstract page). Public? unknown.
- Code available? not stated.
- What it leaves open: China only. Temporal dynamics are not modelled. Hidden-fraud label noise is a general problem for any US version (e.g. SEC enforcement labels).

##### wang2025figraph — FiGraph: A Dynamic Heterogeneous Graph Dataset for Financial Anomaly Detection
- Citation: X. Wang, C. Wang, H. Liu et al., *FiGraph: A Dynamic Heterogeneous Graph Dataset for Financial Anomaly Detection*, WWW Companion 2025, pp. 813–816.
- Link: https://scholar.xjtu.edu.cn/en/publications/figraph-a-dynamic-heterogeneous-graph-dataset-for-financial-anoma/
- First public: 2025-05-23 (WWW'25 Companion); Venue: WWW 2025 Companion; Peer-reviewed: yes (short/resource)
- Verification: [ABS]
- Summary: Public dynamic heterogeneous graph with 9 yearly snapshots (2014–2022), ~730k nodes, ~1.04M edges, 5 node types, 4 edge types, and multi-relations between node pairs. Target nodes have tabular and text attributes, and labels can change across snapshots. The rest are unlabelled background nodes.
- Graph type: temporal (snapshots), heterogeneous, multi-relational.
- Task: anomaly detection (node level).
- Data: https://github.com/XiaoguangWang23/FiGraph (link stated on record page; README could not be opened, so node semantics are unverified). Public? yes (stated).
- Code available? repo stated.
- What it leaves open: A 4-page resource paper, with baselines and temporal-GAD protocol not visible from the record. A ready dynamic heterogeneous GAD benchmark that, judging from my searches, few have used yet.

##### li2025heterogeneous — Heterogeneous Graph Pre-training Based Model for Secure and Efficient Prediction of Default Risk Propagation among Bond Issuers
- Citation: X. Li, X. Shan, W. Yin, H. Wang, *Heterogeneous Graph Pre-training Based Model for ... Default Risk Propagation among Bond Issuers*, arXiv 2501.03268, 2025.
- Link: https://arxiv.org/abs/2501.03268v1
- First public: 2025-01-04; Venue: preprint; Peer-reviewed: unknown
- Verification: [ABS]
- Summary: Pre-trains a heterogeneous graph masked autoencoder (HGMAE) on a large enterprise KG, then trains a classifier on [pretrained embedding ‖ issuer features] to predict default-risk propagation among bond issuers. Motivated by data scarcity and privacy constraints.
- Graph type: static, heterogeneous enterprise KG.
- Task: classification (default-risk propagation).
- Data: enterprise KG (proprietary, Chinese context implied). Public? no.
- Code available? not stated.
- What it leaves open: Proprietary data and no temporal contagion dynamics. Spillover from credit events to *equity* prices of neighbours is not studied.

##### pei2025gaining — The Gaining Paths to Investment Success: Information-Driven LLM Graph Reasoning for Venture Capital Prediction (MIRAGE-VC)
- Citation: H. Pei, Z. Liu, X. Xiao et al., *The Gaining Paths to Investment Success: ...*, arXiv 2512.23489, 2025.
- Link: https://arxiv.org/abs/2512.23489 ; https://arxiv.org/pdf/2512.23489
- First public: 2025-12-29 (v2 2026-01-03); Venue: preprint; Peer-reviewed: no
- Verification: [FT]
- Summary: Time-stamped heterogeneous investor–company network from PitchBook (2005–Nov 2023; 263,729 startups). An information-gain path retriever compresses the network into reasoning chains, and three LLM analyst agents (peer-company, investor-profile, investment-chain) are gated by company attributes. Task: Series A within 12 months. F1 rises 5.0% and P@5 16.6% relative to the best baselines (GNN baselines SHGMNN and GST; RAG baselines).
- Graph type: temporal, heterogeneous, directed bipartite (investor→company) with edge attributes.
- Task: classification ("off-graph" prediction) + path-based explanation.
- Data: PitchBook (proprietary). Public? no (authors note the open Crunchbase snapshot ends in 2013).
- Code available? https://anonymous.4open.science/r/MIRAGE-VC-323F
- What it leaves open: 70:15:15 class-balanced splits are used only to train the path selector/gate; the final test set is time-based (first rounds Oct 2021–Nov 2023, 2,510 startups). Success is defined differently in two places. The quality of the rationales is not evaluated. Proprietary data. Investor→company link prediction (who invests next) is not addressed.

##### liu2025beyond — Beyond Isolated Investor: Predicting Startup Success via Roleplay-Based Collective Agents (SimVC-CAS)
- Citation: Z. Liu, H. Pei, X. Xiao et al., *Beyond Isolated Investor: ...*, arXiv 2512.22608, 2025.
- Link: https://arxiv.org/abs/2512.22608
- First public: 2025-12-27 (v3 2026-05-03); Venue: preprint; Peer-reviewed: no
- Verification: [ABS]
- Summary: Role-playing LLM investor agents interact over a co-investment network through a GNN-based supervised interaction module. Startup financing is treated as a group decision, giving about +25% relative AP@10. Gains are largest for network-central startups.
- Graph type: co-investment network (investor–investor), likely temporal.
- Task: classification/ranking (financing prediction).
- Data: proprietary + public VC data. Public? partly.
- Code available? not stated.
- What it leaves open: Same group as MIRAGE-VC, again relying on proprietary data. Network effects are shown only for central nodes, and link-level predictions are not made.

##### dukkipati2025predictive — Predictive AI with External Knowledge Infusion: Datasets and Benchmarks for Stock Markets
- Citation: A. Dukkipati, K. Mayilvaghanan, N. K. Pallekonda et al., *Predictive AI with External Knowledge Infusion: Datasets and Benchmarks for Stock Markets*, arXiv 2504.20058, 2025.
- Link: https://arxiv.org/abs/2504.20058v2 ; https://arxiv.org/html/2504.20058v2
- First public: 2025-04-14 (v2 2026-02-11); Venue: preprint; Peer-reviewed: no
- Verification: [FT] (methods and data sections. The results/split section was cut off in the fetched excerpt. The HTML contained instruction-like text, which was ignored.)
- Summary: Builds STOCKnowledge temporal KGs for NASDAQ (4,911 entities, 370,567 relations, 12 entity types, 56 relation types) and NSE (1,049 entities, 298,686 relations), 2003–2022. Sources are Seeking Alpha / Moneycontrol news, financial statements, macro indicators, corporate events and Wikidata inter-stock relations. TA-HKGE models KG relations as heterogeneous Hawkes-process events (HPGE + HEAT encoder) to rank stocks by return.
- Graph type: temporal, heterogeneous KG (event-based, continuous time).
- Task: ranking (stock returns).
- Data: scraped news + Wikidata. Public? sources partly public; release URL not visible.
- Code available? not stated.
- What it leaves open: The TKG is only an input to return ranking. The authors do not test whether the Hawkes TKG can *forecast relations/events* themselves (TKG extrapolation), and the dataset release is unclear.

##### yu2025staged — Staged Link Prediction in Bipartite Investment Networks Based on Pseudo-Edge Generation
- Citation: J. Yu, Y. Lee, *Staged link prediction in bipartite investment networks based on pseudo-edge generation*, Information Technology and Management 26(3):391–405, 2025.
- Link: https://pure.seoultech.ac.kr/en/publications/staged-link-prediction-in-bipartite-investment-networks-based-on-/
- First public: 2025-09 (journal issue); DOI 10.1007/s10799-024-00421-6 suggests online-first 2024 (unverified); Peer-reviewed: yes
- Verification: [ABS]
- Summary: Investor–company bipartite network. Generates pseudo-edges for undisclosed early-stage investors, then predicts second-stage investments with Node2vec-based staged link prediction (88.98% accuracy).
- Graph type: temporal (staged), bipartite.
- Task: link prediction.
- Data: not stated on record page. Public? unknown.
- Code available? not stated.
- What it leaves open: Node2vec only, with no heterogeneous or temporal GNN baselines. The data is unclear. Shows that investor–company link prediction exists but is methodologically thin.

##### lee2025knowledge — Knowledge Graph Construction for Stock Markets with LLM-Based Explainable Reasoning
- Citation: C. Lee, Y. Jeong, J. Shin et al., *Knowledge Graph Construction for Stock Markets with LLM-Based Explainable Reasoning*, CIKM 2025 Workshop on Advances in Financial AI (arXiv 2601.11528).
- Link: https://arxiv.org/abs/2601.11528
- First public: 2025-11-16 (date on abs page; ID 2601); Venue: CIKM 2025 workshop; Peer-reviewed: yes (workshop)
- Verification: [ABS]
- Summary: A stock-market KG schema (companies, sectors, indicators, financial statements, inter-company relations) combined with an LLM for multi-hop relational queries and explainable answers. Case studies on Korean listed companies.
- Graph type: static heterogeneous KG.
- Task: other (QA / explanation).
- Data: Korean listed companies. Public? unknown.
- Code available? not stated.
- What it leaves open: Case-study evaluation only, with no quantitative explanation or prediction benchmark.

##### anon2025mehgtlkg — MEHGT-LKG: Multimodal Edge-enhanced Heterogeneous Graph Transformer with LLM-driven Knowledge Graph for Stock Trend Prediction
- Citation: authors unknown (not visible in the record I opened), *MEHGT-LKG: ...*, ICLR 2026 submission (rejected), 2025.
- Link: https://api2.openreview.net/notes/search?term=financial%20knowledge%20graph&limit=25 (record; forum id N5ggpxl8Os. The forum fetch failed with client_error)
- First public: ICLR 2026 submission cycle (autumn 2025; exact date unknown); Venue: ICLR 2026 (rejected); Peer-reviewed: reviewed, rejected
- Verification: [ABS] (summary from the OpenReview API record only)
- Summary: A fine-tuned LLM extracts financial event KGs, and an edge-enhanced heterogeneous graph transformer uses them for stock trend prediction.
- Graph type: heterogeneous event KG, edge features.
- Task: classification (trend).
- Data: unknown.
- Code available? not stated.
- What it leaves open: A typical "LLM-KG → HGNN → trend" pipeline, and it was rejected. Signals that this pattern alone is no longer enough for a top venue.

##### li2024findkg — [BACKGROUND] FinDKG: Dynamic Knowledge Graphs with Large Language Models for Detecting Global Trends in Financial Markets
- Citation: X. V. Li, F. Sanna Passino, *FinDKG: ...*, ICAIF 2024, pp. 573–581 (arXiv 2407.10909).
- Link: https://arxiv.org/abs/2407.10909v2
- First public: 2024-07-15; Venue: ICAIF '24; Peer-reviewed: yes
- Verification: [ABS]
- Summary: Fine-tuned LLM generator (ICKG) builds a dynamic KG from financial news (FinDKG). KGTransformer does temporal link prediction on it and is applied to thematic investing.
- Graph type: temporal, heterogeneous KG.
- Task: link prediction (temporal KG) + thematic investing.
- Data: news (FinDKG dataset). Public? yes (stated as open source).
- Code available? stated as open source; URL not on abs page.
- What it leaves open: Background only. It is the one financial TKG used for link prediction, and among 2025–2026 papers I found **no follow-up that benchmarks TKG completion on a company KG**.

##### Opened but excluded from the main list
- FinFraudBench (arXiv 2608.15177, 2026-08-15) [ABS]: heterogeneous *transaction/card* fraud graphs (up to 8.99M nodes). Out of scope (customer/transaction fraud, close to AML), but its evaluation protocol for heterogeneous GAD is reusable.
- Knowledge-Augmented LLM Agents for Explainable Financial Decision-Making (arXiv 2512.09440, 2025-12-10) [ABS]: generic retrieval-fusion, no graph structure. Low relevance.
- Blom & Helgesson, *Financial News Event Prediction Using Temporal Knowledge Graphs*, Chalmers MSc thesis 2025 [ABS] (https://odr.chalmers.se/items/48c85f1b-34ca-4241-aa37-76f741ffeff4/full): trend-level TKG forecasting with GCN and Transformer/MLA models. Main finding: defining an "actionable trend" in a TKG is the hard part. Precedent: an MSc thesis on this topic is feasible.
- VenGNN "Bring Me a Good One" (Tongji seminar page, 2023) [ABS]: heterogeneous venture information network with meta-paths. Too old.

---


---

## T5. Time series + graph models (crowding map)

**Synthesis (crowding map only).** Graph/relational stock prediction is **crowded**: roughly 3–6 arXiv preprints a month, and an estimated 150–300 papers in 2025–2026 once journals are counted (a judgement; Scholar was blocked). They use ageing benchmarks (NASDAQ/NYSE 2013–2017, CSI300). Critical work is scarce but striking. QuantBench (2025) finds graphs "do not consistently" help. NGAT (2025) shows stronger architectures mask bad graphs. guo2026nystrom (Sep 2026) finds MASTER's learned attention nearly uniform and anti-correlated with return similarity; no cross-stock module beats a per-stock LSTM at about 3,500 stocks. Kong et al. (2026) find that 1.2% of 164 top-venue finance-LLM papers address survivorship bias. No 2025–2026 survey of GNNs for stock prediction was found. Verdict: **crowded for architectures; thin for evaluation rigour and for auditing what learned graphs mean.**

Full per-paper notes (from `sweep/T5.md`; raw query log and unopened leads stay there):

##### guo2026nystrom — Nyström Attention Matches Full Attention for Cross-Sectional Stock Prediction
- Citation: Kunhan Guo, *Nyström Attention Matches Full Attention for Cross-Sectional Stock Prediction*, arXiv:2609.08106, 2026.
- Link: https://arxiv.org/abs/2609.08106 ; https://arxiv.org/html/2609.08106v1
- First public: arXiv v1 2026-09-08; Venue: preprint (cs.LG, q-fin.ST); Peer-reviewed: no
- Verification: [FT] (html; limitations section and references truncated)
- Summary: Dissects the inter-stock attention module of MASTER (a well-known CSI300 model).
  - The module holds 42.5% of the parameters but gives about 25% of the predictive value.
  - The learned attention is nearly uniform and low-rank, and it **anti-correlates with return similarity**: pooled Spearman ρ = −0.614 over 73,910 pairs, partial ρ = −0.627 after controlling for industry, beta and volatility. Same-industry pairs get *lower* attention.
  - **Every graph-based alternative degrades performance** (correlation or industry graph used as hard mask, soft bias, or GCN replacement). Example: GraphMask+Corr IC 0.045±0.004 vs original 0.057±0.008, and below the "module removed" variant at 0.050±0.001.
  - On the full A-share market (~3,486 stocks), no cross-stock module significantly beats a per-stock LSTM.
- Graph type: implicit dense learned graph (attention), dynamic, undirected-weighted; compared against static industry and correlation graphs.
- Task: ranking (IC / RankIC), with mechanistic analysis of learned structure.
- Data: MASTER's open CSI300/CSI800 data (Alpha158 + market features); full A-share via Qlib (train 2010–2017, test 2019–2020). Public: yes.
- Code available? Not stated (builds on MASTER and Qlib open code).
- Statistics: uses TOST equivalence tests and 4–10 seeds, though many ablation rows are single-seed.
- What it leaves open: Only one host model (MASTER) and Chinese data. The finding that attention is "complementarity-seeking" is labelled preliminary. Nobody has tested whether learned cross-stock weights line up with *real* economic relations (supply chain, competitors, co-holdings) instead of industry or correlation.

##### li2026act — ACT: Anti-Crosstalk Learning for Cross-Sectional Stock Ranking via Temporal Disentanglement and Structural Purification
- Citation: Juntao Li, Liang Zhang, *ACT: Anti-Crosstalk Learning for Cross-Sectional Stock Ranking…*, arXiv:2604.20204, 2026.
- Link: https://arxiv.org/abs/2604.20204 ; https://arxiv.org/html/2604.20204v1
- First public: arXiv v1 2026-04-22; Venue: preprint (cs.LG); Peer-reviewed: no
- Verification: [FT] (html; baseline table, backtest section and appendix truncated)
- Summary: A representative 2026 state-of-the-art model on the CSI300/CSI500 benchmark.
  - Decomposes each stock series into trend, fluctuation and shock branches.
  - "Purifies" structural crosstalk between relation types: static industry and region graphs plus a dynamic cosine-kNN learned graph.
  - Claims SOTA ranking accuracy against 16 baselines and "gains of up to 74.25% on CSI300".
- Graph type: multi-relational (industry, region, learned dynamic); undirected; temporal (dynamic learned graph).
- Task: ranking (IC, ICIR, RankIC, RankICIR) plus portfolio backtest and Fama-French alpha regression.
- Data: CSI300 and CSI500; chronological split (train 2010-04 to 2020-12, valid 2021–2022, test 2023-01 to 2025-09). Public: yes (Chinese A-share, Qlib-style).
- Code available? Not stated.
- What it leaves open: Fixed single split with no rolling retraining. Learned and purified graphs are not inspected. Transaction-cost treatment was not visible. Typical of the area: architecture complexity grows while the graph's value is asserted, not audited.

##### niu2026gapnet — GAPNet: Plug-in Jointly Learning Task-Specific Graph for Dynamic Stock Relation
- Citation: Yingjie Niu, Lanxin Lu, Changhong Jin, Ruihai Dong, *GAPNet…*, arXiv:2602.00888, 2026.
- Link: https://arxiv.org/abs/2602.00888 ; https://arxiv.org/html/2602.00888v1
- First public: arXiv v1 2026-01-31; Venue: preprint (Springer/ECML-style formatting, venue not stated); Peer-reviewed: unknown
- Verification: [FT]
- Summary: A plug-in that learns a task-specific stock graph end to end inside existing graph and hypergraph models (GCN, RSR-I, RT-GCN, HypergraphConv, STHAN-SR, CI-STHPAN). It argues that predefined graphs (news, filings, Wiki) are noisy and asynchronous. The learned graph beats every predefined graph on IC/ICIR (e.g. CI-STHPAN IC 0.0052 vs 0.0015 for the best predefined graph).
- Graph type: learned dynamic pairwise graph or hypergraph, initialised from predefined industry/Wiki/DTW relations.
- Task: ranking / return prediction (IRR, Sharpe, IC, ICIR) with top-5 backtest.
- Data: NASDAQ (1,026 stocks) and NYSE (1,737) from Feng et al. 2019, 2013–2017. Chronological split, **test = 2017 only**. Public: yes.
- Code available? Not stated.
- What it leaves open: Says outright that **"transaction costs are ignored"**. The single-year test period is 9 years old. Learned graphs are **never inspected** or compared with industry or Wiki edges, even though the paper claims they are better.

##### niu2026relational — Relational Probing: LM-to-Graph Adaptation for Financial Prediction
- Citation: Yingjie Niu, Changhong Jin, Rian Dolphin, Ruihai Dong, *Relational Probing…*, arXiv:2604.10212, 2026.
- Link: https://arxiv.org/abs/2604.10212
- First public: arXiv v1 2026-04-11; Venue: preprint (cs.CL); Peer-reviewed: no
- Verification: [ABS]
- Summary: Replaces the language-model head with a "relation head". This builds a stock relation graph directly from small-LM hidden states (Qwen3 0.6B/1.7B/4B) and is trained jointly with a stock-trend predictor. It beats a news co-occurrence graph baseline at competitive inference cost, and defines "SLMs" as models fine-tunable on a single 24 GB GPU for reproducibility.
- Graph type: text-derived relation graph, learned end to end; temporal (news-driven).
- Task: classification (stock trend) plus relation-graph construction.
- Data: not specified in abstract (the same group's earlier work uses SPNews / S&P 500 news).
- Code available? Not stated.
- What it leaves open: LLM look-ahead and memorisation are not mentioned in the abstract. The learned relations are not validated against ground-truth company relations. This is part of the third paper in a row from the UCD group (NGAT, GAPNet, Relational Probing) arguing that graph *construction* is the weak link.

##### huang2026crossstock — Cross-Stock Predictability via LLM-Augmented Semantic Networks
- Citation: Yikuan Huang, Zheqi Fan, Kaiqi Hu, Yifan Ye, *Cross-Stock Predictability via LLM-Augmented Semantic Networks*, arXiv:2604.19476, 2026.
- Link: https://arxiv.org/abs/2604.19476 ; https://arxiv.org/html/2604.19476
- First public: arXiv v1 2026-04-21 (v2 2026-04-26); Venue: preprint (q-fin.PM/GN/ST); Peer-reviewed: no
- Verification: [FT] (limitations section truncated)
- Summary: Builds a sparse candidate firm graph from 10-K embeddings. DeepSeek-Chat (with firms anonymised as "Firm A/B") then classifies each edge into competitor, supply_chain, complementary, substitute, peer or unrelated, and filters accordingly. The refined graph aggregates pairwise mean-reversion signals.
  - Long-short Sharpe: 0.742 unfiltered → 0.820 LLM-filtered (S&P 500, 2011–2019).
  - Note: the plain **2-digit SIC industry graph already reaches Sharpe 0.792**, so the LLM's edge over a trivial industry graph is small.
- Graph type: static per-year, typed (6 relation labels), undirected; text-derived.
- Task: cross-stock return predictability / long-short portfolio.
- Data: CRSP (not public) plus 10-K filings (public via SEC EDGAR).
- Code available? Not stated.
- What it leaves open:
  - Anonymisation is the only leakage safeguard; the LLM was trained after 2019 and no contamination test is run.
  - Whether the S&P 500 list is point-in-time is not stated (survivorship risk).
  - Results are gross of costs.
  - LLM edge labels are not checked against ground truth (e.g. Hoberg–Phillips TNIC, supply-chain data).

##### ding2026trace — TRACE: Temporal Rule-Anchored Chain-of-Evidence on Knowledge Graphs for Interpretable Stock Movement Prediction
- Citation: Qianggang Ding, Haochen Shi, Luis Castejón Lozano et al., *TRACE…*, arXiv:2603.12500, 2026.
- Link: https://arxiv.org/abs/2603.12500
- First public: arXiv v1 2026-03-12; Venue: preprint (cs.CE, cs.AI); Peer-reviewed: no
- Verification: [ABS]
- Summary: An interpretable pipeline that combines symbolic relation-sequence rules, multi-hop knowledge-graph exploration and LLM-guided decisions. It grounds reasoning chains in same-period news and outputs auditable UP/DOWN calls with readable evidence paths. On an S&P 500 benchmark: accuracy 55.1%, F1 60.8%, with gains in recall/F1 over the best graph baseline.
- Graph type: heterogeneous temporal knowledge graph (typed, directed relations).
- Task: classification (movement) with explanation paths.
- Data: S&P 500 benchmark plus news (source not given in abstract).
- Code available? Not stated.
- What it leaves open: Accuracy of about 55% is typical of the area. Explanation faithfulness and LLM look-ahead are not evaluated (per abstract). It shows a growing **KG + LLM + explainability** strand that matches the supervisor's KG background.

##### kong2026evaluating — Evaluating LLMs in Finance Requires Explicit Bias Consideration
- Citation: Yaxuan Kong, Hoyoung Lee, Yoontae Hwang et al. (incl. Lopez-Lira, Zohren), *Evaluating LLMs in Finance Requires Explicit Bias Consideration*, arXiv:2602.14233, 2026.
- Link: https://arxiv.org/abs/2602.14233 ; https://arxiv.org/html/2602.14233v1
- First public: arXiv v1 2026-02-15; Venue: preprint, position paper (ICML-style keywords; acceptance not stated); Peer-reviewed: no (preprint; position paper)
- Verification: [FT]
- Summary: Names five finance-specific evaluation biases: look-ahead, survivorship, narrative, objective and cost. It reviews **164 main-conference papers from 2023–2025** (ICML, ICLR, NeurIPS, KDD, AAAI, IJCAI, CIKM, ACL, EMNLP, NAACL, ICAIF).
  - Only **26.8% address look-ahead bias**.
  - Only **1.2% address survivorship bias**.
  - Proposes a "Structural Validity Framework": temporal sanitation, dynamic universe, rationale robustness, calibration, and realistic costs and latency.
- Graph type: n/a (evaluation methodology).
- Task: other (evaluation / meta-research).
- Data: literature review; checklist in an appendix (not read).
- Code available? https://github.com/Eleanorkong/Awesome-Financial-LLM-Bias-Mitigation (stated)
- What it leaves open: Graph and relational models are not discussed. The same audit has not been done for **graph-based stock prediction**, where relation graphs add extra leakage channels: graphs built from full-sample correlations, today's Wikidata or supply-chain snapshots, and LLM-extracted edges.

##### wade2026do — Do Better Volatility Forecasts Lead to Better Portfolios? Evidence from Graph Neural Networks
- Citation: Rylan Wade, *Do Better Volatility Forecasts Lead to Better Portfolios? Evidence from Graph Neural Networks*, arXiv:2605.19278, 2026.
- Link: https://arxiv.org/abs/2605.19278
- First public: arXiv v1 2026-05-19 (v2 2026-05-20); Venue: preprint (q-fin.PM, cs.LG); Peer-reviewed: no
- Verification: [ABS]
- Summary: Weekly realised volatility for 465 S&P 500 stocks (2015–2025). Compares HAR and LSTM against GraphSAGE on rolling-correlation, sector and Granger-causal graphs, with and without macro-regime features. **Three different models win on MSE, ranking accuracy and portfolio Sharpe.** The author concludes that graph models add value only when the portfolio rule can exploit the cross-sectional structure.
- Graph type: static (sector) and rolling (correlation, Granger-causal, i.e. directed) graphs.
- Task: regression (volatility) plus portfolio evaluation.
- Data: S&P 500 (source and constituent selection not stated in abstract; survivorship risk).
- Code available? Not stated.
- What it leaves open: Graph choice (correlation vs sector vs Granger) versus decision quality is only touched on. Decision-aware evaluation of *which* graph helps *which* portfolio rule is an open niche.

##### liu2026bipartite — A Bipartite Graph Approach to U.S.-China Cross-Market Return Forecasting
- Citation: Jing Liu, Maria Grith, Xiaowen Dong, Mihai Cucuringu, *A Bipartite Graph Approach to U.S.-China Cross-Market Return Forecasting*, arXiv:2603.10559, 2026.
- Link: https://arxiv.org/abs/2603.10559
- First public: arXiv v1 2026-03-11 (v2 2026-04-13); Venue: preprint (cs.LG, q-fin.CP); Peer-reviewed: no
- Verification: [ABS]
- Summary: Builds a **directed, time-ordered bipartite graph** of predictive links from U.S. to Chinese stocks (non-overlapping trading hours). Edges are selected by rolling-window hypothesis tests, and the sparse graph is used as a feature-selection layer for regularised and ensemble models. Finds a strong asymmetry: U.S. close-to-close returns predict Chinese open-to-close returns, but not the reverse.
- Graph type: temporal, directed, bipartite, statistically selected (interpretable) edges.
- Task: regression / return forecasting; lead-lag structure discovery.
- Data: U.S. and Chinese equities (sources not stated in abstract).
- Code available? Not stated.
- What it leaves open: A counter-model to black-box learned graphs, because its edges are tested, directed and interpretable. Signed edges, within-market lead-lag and comparison with learned-attention graphs are untested. This group (Oxford, Cucuringu/Dong) is a credible methodological anchor.

##### wang2025quantbench — QuantBench: Benchmarking AI Methods for Quantitative Investment
- Citation: Saizhuo Wang, Hao Kong, Jiadong Guo et al., *QuantBench: Benchmarking AI Methods for Quantitative Investment*, arXiv:2504.18600, 2025.
- Link: https://arxiv.org/abs/2504.18600 ; https://arxiv.org/html/2504.18600v1
- First public: arXiv v1 2025-04-24; Venue: preprint (q-fin.CP); Peer-reviewed: no
- Verification: [FT]
- Summary: An industry-style benchmark (Chinese and US data, Alpha101/158/191, walk-forward). It benchmarks GCN, GAT, RGCN, RSR, HATS, HATR-I, hypergraph models (ESTIMATE, STHCN, STHAN, STHGCN) and adaptive graphs (THGNN, DTML).
  - **"Incorporation of graph structures does not consistently yield performance improvements."** Example: LSTM IC 3.89% vs GAT 3.90% vs GCN −0.10%; THGNN best at 4.93%.
  - Hypergraph models performed poorly (STHAN IC 0.05%).
  - Wikidata relations "yielded minimal improvements".
  - Rolling retraining every 3 months beats no retraining.
- Graph type: static (Wikidata snapshots, GICS industry), adaptive learned, hypergraphs.
- Task: ranking / portfolio benchmark.
- Data: Chinese A-share and US (to May 2024). Wikidata relations use **temporal snapshots to avoid leakage**, which is rare.
- Code available? "Will open-source" (no URL at v1).
- What it leaves open: Code not yet released (at v1). Transaction-cost settings and split dates were not visible. There is no analysis of *why* graphs fail, e.g. whether the graph is wrong or the aggregation is wrong.

##### niu2025ngat — NGAT: A Node-level Graph Attention Network for Long-term Stock Prediction
- Citation: Yingjie Niu, Mingchuan Zhao, Valerio Poti, Ruihai Dong, *NGAT…*, arXiv:2507.02018, 2025.
- Link: https://arxiv.org/abs/2507.02018 ; https://arxiv.org/html/2507.02018v1
- First public: arXiv v1 2025-07-02; Venue: preprint; Peer-reviewed: unknown
- Verification: [FT]
- Summary: Argues three things: the value of relational data is hidden by how downstream tasks are designed; stock-GNNs are over-complex; and company graphs are built "from intuition" with no sound way to compare them. Compares static, return-correlation (5d/21d) and news co-occurrence (5d/21d) graphs. Key findings:
  - "advanced architectures can obscure inherent graph deficiencies": model capacity and graph quality are **confounded**.
  - Apart from NGAT, **no graph model consistently beats the LSTM baseline** across horizons.
- Graph type: temporal co-occurrence graph (decaying weights), correlation graphs, static graph.
- Task: regression (volatility, return; OOS R², MSE) and classification (ACC, MCC, AUC).
- Data: SPNews (268 S&P 500 firms, news) and ACL2018 tweets (76 firms). Public: stated as public.
- Code available? https://github.com/FreddieNIU/NGAT (stated)
- What it leaves open: Split protocol not stated. No backtest or costs. The core proposal, to **evaluate graphs intrinsically rather than only by downstream accuracy**, has no follow-up metric. It cites Niu et al. 2024 (ICAIF), "Evaluating financial relational graphs: interpretation before prediction" (not opened).

##### tan2025magnet — MaGNet: A Mamba Dual-Hypergraph Network for Stock Prediction via Temporal-Causal and Global Relational Learning
- Citation: Peilin Tan, Chuanqi Shi, Dian Tu, Liang Xie, *MaGNet…*, arXiv:2511.00085, 2025.
- Link: https://arxiv.org/abs/2511.00085 ; https://arxiv.org/html/2511.00085v1
- First public: arXiv v1 2025-10-29; Venue: preprint; Peer-reviewed: no
- Verification: [FT]
- Summary: Combines bidirectional Mamba with gating, sparse MoE and attention ("MAGE block"), 2D feature/stock attention, and a dual hypergraph: a temporal-causal hypergraph plus a global probabilistic hypergraph weighted by Jensen-Shannon divergence. Compared with 17 baselines (incl. MASTER, StockMixer, DTML, ESTIMATE, PatchTST, iTransformer).
- Graph type: learned hypergraphs (temporal-causal = directed in time; global = soft assignments).
- Task: classification (movement) plus backtest (AR, Sharpe, Calmar, MDD) with **0.25% transaction cost**.
- Data: six indices (DJIA, HSI, NASDAQ-100, S&P 100, CSI 300, Nikkei 225), 2020–2024 via yfinance, with Qlib Alpha158/Alpha360 features. Public: yes.
- Code available? https://github.com/PeilinTime/MaGNet (stated: data, code, weights)
- What it leaves open: Split given as 7:1:2 with chronology not stated. Short 5-year window. My reading of its tables: some metrics are near chance (S&P 100 AUC 49.36). Learned hypergraphs are not inspected; the authors list this as future work.

##### qiu2025hermes — Hermes: A Multi-Scale Spatial-Temporal Hypergraph Network for Stock Time Series Forecasting
- Citation: Xiangfei Qiu, Liu Yang, Xiangyu Xu et al. (incl. Bin Yang, Christian S. Jensen), *Hermes…* (v1 title: *Multi-Scale Spatial-Temporal Hypergraph Network with Lead-Lag Structures for Stock Time Series Forecasting*), arXiv:2509.23668, 2025.
- Link: https://arxiv.org/html/2509.23668v2 ; https://arxiv.org/abs/2509.23668
- First public: arXiv 2509.* (Sep 2025, from ID; exact date not shown); Venue: **ICLR 2026 withdrawn submission** (OpenReview API search, forum 08FTG45E9m); Peer-reviewed: no (withdrawn)
- Verification: [FT]
- Summary: An industry-hyperedge network with a sliding-window "moving aggregation" for **inter-industry lead-lag** and multi-scale edge-to-edge fusion. Beats hypergraph (ESTIMATE, STHAN-SR, ASHyper), GNN (RSR, GAT, RGCN), Transformer and MLP baselines on IC/ICIR/SR.
- Graph type: hypergraph (industry hyperedges), temporal lead-lag.
- Task: regression / ranking (IC, ICIR, SR).
- Data: NASDAQ and NYSE (Feng et al. 2019, 2013–2017) and S&P 500 (474 stocks, 2016–2022). Public: yes.
- Code available? Not stated.
- What it leaves open: Runs on **8–13-year-old benchmark data**. Has no trading backtest or costs (SR with risk-free rate 0, construction unspecified). Learned lead-lag weights are not inspected or compared with known lead-lag (e.g. supplier→customer). Withdrawn from ICLR 2026, a sign that top ML venues are sceptical of this genre.

##### li2025structure — Structure Over Signal: A Globalized Approach to Multi-relational GNNs for Stock Prediction (OmniGNN)
- Citation: Amber Li, Aruzhan Abil, Juno Marques Oda, *Structure Over Signal…*, arXiv:2510.10775, 2025.
- Link: https://arxiv.org/abs/2510.10775 ; https://arxiv.org/html/2510.10775v1
- First public: arXiv v1 2025-10-12; Venue: preprint (cs.LG); Peer-reviewed: no
- Verification: [FT]
- Summary: A heterogeneous GNN with a **sector node as global hub** so macro shocks propagate in one hop. Uses GAT for neighbours and a Transformer over time, and claims robustness in the COVID period.
- Graph type: heterogeneous (stock, sector/industry nodes; GICS and co-holding stock–stock edges; market-share and ESG stock–industry edges), dynamic.
- Task: ranking (IC, IR, Precision@K, cumulative return).
- Data: **10 IT stocks**, 2019–2022, from Bloomberg, LSEG and Capital IQ. The abstract says "public datasets", but these sources are proprietary.
- Code available? https://github.com/amberhli/OmniGNN (stated)
- What it leaves open: A textbook case of the field's weaknesses:
  - only two baselines (GAT, Transformer)
  - hyperparameters left as "x"
  - no costs
  - robustness claim rests on one COVID window, where the best configuration has a **negative IC**
  - learned attention not inspected
  
  The *idea* (shock propagation through hub nodes) is still worth testing properly.

---

#### A2. Additional opened papers (short entries; used for crowding count)

**2026**
- **benhenda2026look**: *Look-Ahead-Bench: a Standardized Benchmark of Look-ahead Bias in Point-in-Time LLMs for Finance*, M. Benhenda, arXiv:2601.13770, v1 2026-01-20, preprint. [ABS]
  - Measures LLM look-ahead as alpha decay across regimes. Finds significant bias in Llama 3.1 and DeepSeek 3.2 but not in point-in-time "Pitinf" models.
  - Code: https://github.com/benstaf/lookaheadbench (stated).
- **xiao2026rankglu**: *RankGLU: Residual Gated Score Formation for Cross-Sectional Stock Prediction*, H. Xiao, J. Xu, F. Qu et al., arXiv:2606.08930, v1 2026-06-08, preprint. [ABS]
  - CSI300/CSI800 with multi-seed reporting: IC 0.0654±0.0052 → 0.0727±0.0037.
  - Notes that **"relation-path calibrations" give high single-seed peaks but are unstable across seeds**, which is evidence of seed-cherry-picking risk for graph add-ons.
- **alridhawi2026stock**: *Stock Market Prediction Using Node Transformer Architecture Integrated with BERT Sentiment Analysis*, M. Al Ridhawi, M. Haj Ali, H. Al Osman, arXiv:2603.05917, v1 2026-03-06 (v3 2026-05-14). DOI 10.1109/ACCESS.2026.3691980 suggests IEEE Access. [ABS]
  - Graph of sector, correlation and supply-chain edges. Only **20 S&P 500 stocks**, baselines ARIMA/LSTM, metric MAPE.
- **alridhawi2026adaptive**: *Adaptive Regime-Aware Stock Price Prediction Using Autoencoder-Gated Dual Node Transformers with RL Control*, same authors, arXiv:2603.19136, v1 2026-03-19, preprint. [ABS]
  - Regime routing via autoencoder anomaly score plus a SAC controller. Same 20 stocks, 1982–2025.
- **gregnanin2026statistical**: *The Statistical Significance of the Inclusion of Graph Neural Networks in the Financial Time Series Forecasting Problem*, M. Gregnanin, J. De Smedt, G. Gnecco, M. Parton, arXiv:2605.21192, v1 2026-05-20, preprint. [ABS]
  - A univariate series turned into geometric/graph patterns via GNN ("Time-Geometric model"). Claims statistically significant gains.
  - Not cross-stock relations.
- **wang2026large**: *Large language model-driven time-series forecasting of financial network indicators*, M. H. Wang, Y. Yeung, Frontiers in AI 9:1722121, published 2026-01-29, peer-reviewed. [FT]
  - Fund–stock co-holding bipartite graph (2016–2024) projected to stocks. An LLM+RAG forecasts network centralisation and density spikes.
  - Network is holdings-based, not LLM-built. No costs, no code.
- **shu2026dexposurefm**: *DeXposure-FM: A Time-series, Graph Foundation Model for Credit Exposures and Stability on Decentralized Financial Networks*, A. Shu, W. Wu, G. Ibikunle, F. He, arXiv:2602.03981, v1 2026-02-03 (v2 2026-06-30), preprint. [ABS] **[crypto, transferable]**
  - First "time-series + graph foundation model" for exposure-link forecasting.
  - Code: https://github.com/EVIEHub/DeXposure-FM; HF model.
  - Relevant to the credit-contagion theme.
- **sun2026survey**: *A Survey on Foundation Models for Structured Data: Tabular, Time Series, and Graphs*, Q. Sun, H. Yuan, Y. Huang et al. (incl. P. S. Yu), Preprints.org 202605.1532, posted 2026-05-22. DOI 10.1109/TKDE.2026.3717198 (TKDE). [ABS]
  - General survey (150+ methods), not finance-specific.
- **saidd2026controlled**: *A Controlled Comparison of Deep Learning Architectures for Multi-Horizon Financial Forecasting: Evidence from 918 Experiments*, N. A. Saidd, arXiv:2603.16886, v1 2026-02-27, preprint. [ABS]
  - Not graph. A rigour template: seeded HPO, multi-seed, statistical validation.
  - Directional accuracy ~50% everywhere; architecture explains most variance, seeds negligible.

**2025**
- **dai2025griffinnet**: *GrifFinNet: A Graph-Relation Integrated Transformer for Financial Predictions*, C. Dai, W. Wang, Y. Fan et al. (Jilin Univ.), arXiv:2510.10387 (Oct 2025 from ID; dates not shown on fetched page), preprint. [ABS]
  - Industry and institutional-ownership graphs with adaptive gating plus a Transformer, on CSI300/CSI800.
  - Code and data: https://www.healthinformaticslab.org/supp/ (stated).
- **chen2025stock**: *Stock Prediction via a Dual Relation Fusion Network incorporating Static and Dynamic Relations* (DRFN), L. Chen, H. Bai, M. Wang et al., arXiv:2510.10695, v1 2025-10-12, preprint. [ABS]
  - Fuses static and dynamic relations, including overnight influence.
- **tan2025h3mssmoes**: *H3M-SSMoEs: Hypergraph-based Multimodal Learning with LLM Reasoning and Style-Structured Mixture of Experts*, P. Tan, L. Xie, C. Zhi et al., arXiv:2510.25091, v1 2025-10-29, preprint. [ABS]
  - Local and global hypergraphs, a frozen LLM with adapters, and industry experts.
  - Code: https://github.com/PeilinTime/H3M-SSMoEs (stated).
- **sun2025intercorprel**: *InterCorpRel-LLM: Enhancing Financial Relational Understanding with Graph-Language Models*, Q. Sun, J. Zheng, B. Jin et al., arXiv:2510.09735, v1 2025-10-10, preprint. [ABS]
  - GNN+LLM trained on **proprietary FactSet supply-chain** data. Supply-relation F1 0.854 vs GPT-5 0.229.
  - Shows that real relations can be *predicted*; not a return model.
- **xu2025modeling**: *Modeling Interactions Between Stocks Using LLM-Enhanced Graphs for Volume Prediction* (LED-GNN), Z. Xu, Y. Liu, Y. Wang et al. (PKU and Mizuho), FinNLP@COLING/ACL workshop, Jan 2025, peer-reviewed workshop. [FT] https://aclanthology.org/2025.finnlp-1.14.pdf
  - Mistral-7B extracts relations from Reuters news into a dynamic graph, fused with a Gumbel-learned graph. TOPIX500, 2013–2018.
  - Weaknesses: 4:1 split with **no validation set**, "best result after loss stabilised", LLM leakage not discussed.
- **gao2025detecting**: *Detecting Lookahead Bias in LLM Forecasts*, Z. Gao, W. Jiang, Y. Yan, arXiv:2512.23847, v1 2025-12-29 (v2 2026-06-12), preprint. [ABS]
  - "Lookahead Propensity" (LAP) test: positive inside the training window, near zero after the cutoff. A cheap diagnostic usable on LLM-built graphs.
- **sadek2025hybrid**: *A Hybrid Model for Stock Market Forecasting: Integrating News Sentiment and Time Series Data with GNNs*, N. Sadek, M. Moawad, C. Naguib, M. Elzahaby, arXiv:2512.08567, v1 2025-12-09. DOI 10.34190/icair.5.1.4294 suggests ICAIR proceedings. [ABS]
  - Heterogeneous article–company–industry graph with GraphSAGE. Only 53% direction accuracy vs an LSTM baseline.
- **lynch2025novel**: *A Novel Hybrid Temporal Fusion Transformer Graph Neural Network Model for Stock Market Prediction*, S. Lynch, P. Derakhshan, S. Lynch, Preprints.org 202510.2481 (posted 2025-10-31); AppliedMath 5(4):176 (2025), peer-reviewed. [ABS]
  - TFT-GNN vs SARIMA/ETS/TFT on US equities and ETFs, 2012–2024.
  - Code: https://github.com/SebLynch5/Stock-Market-Forecasting (stated).
- **bao2025datadriven**: *Data-driven stock forecasting models based on neural networks: A review*, W. Bao, Y. Cao, Y. Yang et al., Information Fusion 113:102616 (vol. 2025; available 2024-10), peer-reviewed. [ABS]
  - Covers RNN, CNN, Transformer, **GNN**, GAN and LLM for 2015–2023, plus datasets and metrics. The closest thing to a recent survey that includes GNNs. **It counts as 2025 only by venue date.**
- **hu2025study**: *A Study of Dynamic Stock Relationship Modeling and S&P500 Price Forecasting Based on Differential Graph Transformer*, L. Hu, Q. Wang, arXiv:2506.18717, v1 2025-06-23, preprint. [ABS]
  - Compares Pearson, MI, Spearman and Kendall graphs at "global/local/dual scope"; global Kendall is best.
  - **Look-ahead risk** if the "global" correlation spans the test period (not verified, abstract only).
  - Baselines GRU only.
- **lee2025knowledge**: *Knowledge Graph Construction for Stock Markets with LLM-Based Explainable Reasoning*, C. Lee, Y. Jeong, J. Shin et al., arXiv:2601.11528, v1 2025-11-16, CIKM 2025 workshop (FinAI). [ABS]
  - KG schema (companies, sectors, indicators, statements, inter-company relations) plus LLM multi-hop QA. Korean case studies, no prediction benchmark.
- **fu2025new**: *The New Quant: A Survey of Large Language Models in Financial Prediction and Trading*, W. Fu, arXiv:2510.05533, v1 2025-10-07, preprint. [ABS]
  - LLM survey. The abstract does not cover graph or relational methods. Recommends time-safe, cost-aware evaluation.

**Background (older, ≤2)**
- **das2024integrating** [BACKGROUND]: *Integrating sentiment analysis with graph neural networks for enhanced stock prediction: A comprehensive survey*, N. Das, B. Sadhukhan, R. Chatterjee, S. Chakrabarti, Decision Analytics Journal 10:100417, 2024-03. [ABS]
  - The most recent dedicated GNN-for-stocks survey I could open. **2024, so it shows the survey gap rather than filling it.**

---


---

## T6. From correlation to causation

**Synthesis.** Econometric spillover/connectedness work (TVP-VAR, quantile connectedness) is crowded but mostly not ML. Deep models that call a learned stock graph "causal" are a moderate, growing group whose validation is often circular. The general time-series causal-discovery community is very active in 2026 (ICML SRNGC, ICLR TCD-Arena, regime-dependent methods such as FANTOM). These methods name finance as motivation but are **never tested on market data**. **No 2025–2026 paper validates learned lead-lag or causal stock edges against public economic relations or event studies.** Verdict: **moderate overall, thin for externally validated firm-level causal graphs.**

Full per-paper notes (from `sweep/T6.md`; raw query log and unopened leads stay there):

##### ouyang2026uncovering: Uncovering Sparse Financial Networks with Information Criteria
- Citation: Ouyang, F., Yang, T. T., Yao, W., *Uncovering Sparse Financial Networks with Information Criteria*, arXiv:2601.03598, 2026.
- Link: https://arxiv.org/abs/2601.03598 (full text read via https://arxiv.org/pdf/2601.03598v2)
- First public: arXiv Jan 2026 (v2 dated 2026-01-19); Venue: preprint; Peer-reviewed: no
- Verification: [FT]
- Summary: The paper rewrites FEVD and GFEVD (Diebold-Yilmaz) connectedness as a regression of forecasts on structural shocks, so that a zero spillover equals a zero coefficient. A BIC-type information criterion, with its penalty tuned by pseudo-out-of-sample MSFE, then selects a sparse set of spillover channels, and the authors prove selection consistency. Simulations with a known sparse VAR ground truth report edge-recovery rates (CDR1/CDR0). Applications cover the 19-market Diebold-Yilmaz stock data, residualized S&P 500 sector returns for 2006–2025 (Financials switch from net transmitter to net receiver after 2015), and 24 commodity realized volatilities.
- Graph type: static per (sub)period; homogeneous; directed, weighted; unsigned (variance shares)
- Task: structure learning (sparse spillover network)
- Data: DY 1992–2007 weekly market returns (classic public dataset); S&P 500 sector indices; commodity futures RV. The excerpt names no vendor for the sector or commodity data.
- Code available? not stated
- What it leaves open: There is no rolling or regime-wise estimation (only two fixed subperiods), no inference on individual edges, no firm-level networks, and the edges depend on the identification scheme, so there is no causal reading. It never tests whether the sparse edges match economic links.

##### zhao2026forecasting: Forecasting Global Volatility with Predictive Spillover Networks (PGA-Trans-HAR)
- Citation: Zhao, X., Qiao, H., *Forecasting Global Volatility with Predictive Spillover Networks: A Neuro-Econometric Spatio-Temporal Transformer for Asynchronous Financial Markets*, arXiv:2608.25369, 2026.
- Link: https://arxiv.org/abs/2608.25369 ; full text https://arxiv.org/html/2608.25369
- First public: arXiv 2026-08-26 (v2 2026-09-05); Venue: preprint; Peer-reviewed: no
- Verification: [FT]
- Summary: The model fits a rolling ridge-VAR(2) with a 22-step GFEVD connectedness matrix (252-day window, refreshed every 20 origins) and uses it as a masked attention prior. A per-market learned gate mixes this prior with data-driven attention, and the output is a bounded correction on a frozen HAR forecast. Closed markets are masked as sources so that asynchronous calendars do not leak zero-coded signals. The model beats HAR in almost all of 8 international indices for 2006–2022 (DM tests, Model Confidence Sets).
- Graph type: temporal (rolling); homogeneous; directed, weighted; unsigned
- Task: regression (volatility forecasting) with a learned influence structure
- Data: 8 equity indices (SPX, FTSE, HSI, N225, KS11, ...); source not stated in the excerpt
- Code available? not stated
- What it leaves open: The authors call the prior "predictive connectedness", not causal. The gate is static per market, so they do not test whether reliance on the econometric prior changes across regimes or crises. It is index-level only; no firm networks and no comparison of learned attention with the GFEVD graph appear in the excerpt.

##### rodriguezdominguez2026order: Order-Constrained Spectral Causality in Multivariate Time Series
- Citation: Rodriguez Dominguez, A., *Order-Constrained Spectral Causality in Multivariate Time Series*, arXiv:2601.01216, 2026.
- Link: https://arxiv.org/abs/2601.01216 ; full text https://arxiv.org/pdf/2601.01216
- First public: arXiv Jan 2026 (document dated 2026-01-06); Venue: preprint; Peer-reviewed: no
- Verification: [FT]
- Summary: The paper defines directional influence as how sensitive whitened lagged cross-covariance operators are to order-preserving time shifts of a source. Spectral summaries of these operators give system-level statistics (leading eigenvalue, trace, effective rank), with shift-based randomization for inference. Under linear-Gaussian VAR the criterion equals Granger causality, but it can also detect collective and distributed directional dependence. Applied to 211 global cross-asset daily returns (2015–2022; 252-day windows with a 21-day step), it finds directional organization that is episodic and stress-dependent, causal hubs that rotate over time, and sparse transmission channels with heterogeneous lags.
- Graph type: temporal (rolling); homogeneous; directed; channel edges are non-negative (squared loadings), so effectively unsigned
- Task: causal discovery / structure learning (system-level directional dependence)
- Data: 211 return series across FX, rates, credit, equities, commodities, real estate and volatility; instruments and source not named
- Code available? not stated
- What it leaves open: The author states the results are "descriptive, not structural or interventional". There is no external validation (stress episodes are only said to "coincide" with stress), no quantitative comparison with pairwise Granger or TE on real data, and no firm-level application.

##### kang2026information: Transfer Entropy Networks across Investor Types (Korean equities)
- Citation: Kang, S., *Information Propagation Across Investor Types: Transfer Entropy Networks in the Korean Equity Market*, arXiv:2603.20271, 2026.
- Link: https://arxiv.org/abs/2603.20271 ; full text https://arxiv.org/html/2603.20271v1
- First public: arXiv 2026-03-15; Venue: preprint; Peer-reviewed: no
- Verification: [FT]
- Summary: The paper builds symbolic transfer-entropy networks over 100 KRX stocks for 2020–2025 from flows by investor type (foreign, institutional, individual), using block-permutation surrogates and Benjamini-Hochberg FDR control. The networks are very sparse (density about 0.005–0.008). Interaction information is negative (redundant), conditional TE is about 0, and centrality adds almost nothing in Fama-MacBeth regressions. The author reads the edges as shared reaction to public information, not private-signal cascades.
- Graph type: static (full sample); three layers by investor type (multiplex-like); directed; unsigned
- Task: structure learning plus validation via cross-sectional asset pricing
- Data: KRX daily net flows by investor type (published by the exchange; no vendor named)
- Code available? not stated
- What it leaves open: It is a useful negative result (directed TE edges carry little pricing information), but it has no common-driver control (no news variable). The full text has many internal inconsistencies: edge counts, mean TE and MI differ between tables. The author suggests event-driven and intraday extensions.

##### jing2026when: When market boundaries weaken: regime-dependent cross-asset spillovers
- Citation: Jing, R., Rocha, L. E. C., *When market boundaries weaken: Network reconfiguration and regime-dependent cross-asset spillovers*, arXiv:2605.30442, 2026.
- Link: https://arxiv.org/abs/2605.30442
- First public: arXiv 2026-05-28 (v2 2026-06-17); Venue: preprint; Peer-reviewed: no
- Verification: [ABS]
- Summary: The paper covers 381 assets (S&P 500 equities, cryptocurrencies, fiat currencies) from 2017 to 2024 and combines rolling correlation networks, community detection, Turbulence Indices and VAR connectedness. Integration across asset classes is episodic: under stress, modular boundaries weaken and regime shifts change how shocks propagate more than how large spillovers are. The authors argue that full-sample connectedness understates coupling under stress.
- Graph type: temporal (rolling); multi-asset-class; correlation network (undirected) plus directed VAR connectedness
- Task: other (network reconfiguration and spillover analysis across regimes)
- Data: S&P 500, crypto and FX prices; source not stated in the abstract
- Code available? not stated
- What it leaves open: Regimes are defined by turbulence indices, not learned jointly with the graph. There is no causal discovery and no prediction of the regime-specific edges.

##### kim2026llm: LLM as a Risk Manager: LLM Semantic Filtering for Lead-Lag Trading
- Citation: Kim, S., Kim, M., Kwon, J., et al., *LLM as a Risk Manager: LLM Semantic Filtering for Lead-Lag Trading in Prediction Markets*, arXiv:2602.07048, 2026.
- Link: https://arxiv.org/abs/2602.07048
- First public: arXiv 2026-02-04 (v2 2026-02-27); Venue: preprint; Peer-reviewed: no
- Verification: [ABS]
- Summary: The method runs in two stages. Granger causality on market-implied probabilities proposes leader→follower pairs, then an LLM re-ranks the pairs by whether a plausible economic transmission mechanism exists. Because true causal links are unobserved, evaluation uses a fixed signal-triggered trading protocol. LLM filtering raises the win rate (51.4%→54.5%) and roughly halves the average losing trade, mainly by removing fragile links.
- Graph type: temporal (rolling); homogeneous; directed; unsigned
- Task: causal discovery (edge screening) / ranking
- Data: Kalshi Economics prediction markets (event-contract prices)
- Code available? not stated
- What it leaves open: This is prediction markets, not equities, but the method transfers directly to stock lead-lag graphs. Validation by P&L only; no external ground truth. [non-equity, transferable]

##### huang2026cross: Cross-Stock Predictability via LLM-Augmented Semantic Networks
- Citation: Huang, Y., Fan, Z., Hu, K., et al., *Cross-Stock Predictability via LLM-Augmented Semantic Networks*, arXiv:2604.19476, 2026.
- Link: https://arxiv.org/abs/2604.19476
- First public: arXiv 2026-04-21 (v2 2026-04-26); Venue: preprint; Peer-reviewed: no
- Verification: [ABS]
- Summary: A sparse candidate graph is built from 10-K embedding similarity, and an LLM then classifies and filters edges by economic relationship type. Many text-similarity edges are economically spurious. On S&P 500 constituents (2011–2019), the filtered graph improves a cross-stock mean-reversion long-short strategy (Sharpe 0.742→0.820; smaller max drawdown).
- Graph type: static or slowly updated (annual 10-K); heterogeneous relation types after LLM labelling; undirected similarity edges
- Task: other (graph construction/filtering) → ranking/portfolio
- Data: SEC 10-K filings (public, EDGAR) plus S&P 500 prices
- Code available? not stated
- What it leaves open: Edges come from text and are not tested against return-based lead-lag or causal edges. That leaves room for a study that checks whether return-based causal edges agree with LLM-labelled economic relations.

##### yang2026shapley: Shapley Regularized Neural Granger Causality (SRNGC)
- Citation: Yang, M., Zhu, Z., Tian, Y., et al., *Shapley Regularized Neural Granger Causality*, ICML 2026.
- Link: https://icml.cc/virtual/2026/poster/62714 ; code https://github.com/MerlynYang/SRNGC
- First public: OpenReview submission 2026-01-16; ICML 2026 (published ~2026-04-30; poster 2026-07-09); Venue: ICML 2026; Peer-reviewed: yes
- Verification: [ABS] (ICML page and GitHub README)
- Summary: Neural Granger causality is recast as a global feature-attribution problem. The paper introduces an information-theoretic Shapley value (Info-Shap) and proves that Info-Shap = 0 if and only if Granger non-causality holds. Two model-agnostic regularizers suppress spurious links. The README lists VAR(3) and Lorenz-96 (synthetic) and DREAM3/DREAM4/CausalTime (real or realistic) as evaluation data.
- Graph type: static; homogeneous; directed; unsigned
- Task: causal discovery (Granger structure learning)
- Data: no financial data (per README)
- Code available? yes, https://github.com/MerlynYang/SRNGC (also on PyPI as `shapneurgc`; seen in search results, not opened)
- What it leaves open: It was never tested on non-stationary, heavy-tailed financial returns, it has no regimes, and edge signs are not produced. It is a strong 2026 baseline for a finance transfer study.

##### stein2026tcdarena: TCD-Arena: robustness of time-series causal discovery against assumption violations
- Citation: Stein, G., Penzel, N., Piater, T., Denzler, J., *TCD-Arena: Assessing Robustness of Time Series Causal Discovery Methods Against Assumption Violations*, ICLR 2026 / arXiv:2605.03045.
- Link: https://arxiv.org/abs/2605.03045 ; record via https://api2.openreview.net/notes/search?term=regime%20causal%20discovery%20time%20series&limit=25 (venue "ICLR 2026 Poster")
- First public: OpenReview ICLR 2026 submission (review records dated Oct 2025); arXiv v1 2026-05-04; Venue: ICLR 2026 poster; Peer-reviewed: yes
- Verification: [ABS]
- Summary: The paper presents a modular toolkit and a study of about 30 million causal-discovery runs covering 33 kinds of assumption violation at increasing severity. Methods show distinct robustness profiles, and ensembles of causal-discovery methods can improve robustness.
- Graph type: n/a (benchmark of temporal causal graphs)
- Task: causal discovery evaluation
- Data: synthetic (toolkit)
- Code available? toolkit described; no link on the abstract page
- What it leaves open: The violations are generic. No finance-calibrated violations appear in the abstract (heavy tails, volatility clustering, latent common factors, asynchronous trading). The companion 2026 benchmark CausalCompass (A2) also finds that no method wins in every scenario.

##### zhou2025deltalag: DeltaLag: Learning Dynamic Lead-Lag Patterns in Financial Markets
- Citation: Zhou, W., Wang, S., Cucuringu, M., et al., *DeltaLag: Learning Dynamic Lead-Lag Patterns in Financial Markets*, arXiv:2511.00390, 2025.
- Link: https://arxiv.org/abs/2511.00390 ; full text https://arxiv.org/html/2511.00390v1
- First public: arXiv 2025-11-01; Venue: preprint (listed as CoRR 2025 on OpenReview); Peer-reviewed: no
- Verification: [FT]
- Summary: Sparse cross-attention picks, for each lagging stock and day, the top-k leaders and a pair-specific lag (≤ l_max). The leaders' lag-aligned features then predict the lagger's next-day return, trained with a ranking loss. Training is 2010–2018, validation 2019–2021 and testing 2022–2023 (chronological), on S&P 500, NASDAQ (713 stocks) and NYSE (1,140). It beats statistical lead-lag graphs (meta-flow graph from Bennett et al. 2022, lagged Pearson graphs) and fixed-lag models on IC, annual return and Sharpe ratio.
- Graph type: temporal (daily re-selected); homogeneous; directed; unsigned (softmax weights)
- Task: structure learning (lead-lag) plus ranking (return prediction / portfolio)
- Data: US equities (QuantBench preprocessing; data source not named)
- Code available? not stated
- What it leaves open: Edges are not validated against economic relations or events. There are no signed edges, no Granger or Lévy-area baselines, transaction costs are not discussed, and k = 2 was chosen for efficiency. The attention-selected "leaders" are predictive, not causal.

##### krstev2025temporal: A Temporal Graph Learning Framework for Lead-Lag Detection in Financial Markets
- Citation: Krstev, I., Rigoni, D., Mishkovski, I., Pasa, L., *A Temporal Graph Learning Framework for Lead-Lag Detection in Financial Markets*, ICLR 2026 submission (withdrawn), 2025.
- Link: https://api2.openreview.net/notes/search?term=Temporal%20Graph%20Learning%20Framework%20Lead-Lag%20Detection%20Financial%20Markets&limit=10 (forum id KsWRLyIAKP; the forum page was blocked by a browser check)
- First public: OpenReview 2025-09/10 (ICLR 2026 submission); Venue: none (withdrawn); Peer-reviewed: reviewed (ratings 2–4), not accepted
- Verification: [ABS] (abstract, keywords and the 5 reviews via the OpenReview API)
- Summary: Lead-lag detection is reframed as temporal link prediction on a dynamic graph, with a benchmark of 8 models (LSTM, JODIE, DySAT, TGAT, TGN, APAN, GraphMixer, GraphMixer-TNF) in two settings: positive and negative relationships, and positive only. GraphMixer is best. Reviewers criticize the small scale (37 assets: 29 stocks and 8 commodities, about 5 years of daily data), the unjustified label thresholds (τ = 1 day, ε = 5%), the lack of validation against Granger, TE or DTW, the absence of any economic backtest, and an ablation in which static description embeddings match the temporal features.
- Graph type: temporal (discrete snapshots); homogeneous with node features (sentiment, text embeddings); directed; signed (positive/negative setting)
- Task: link prediction (lead-lag edges as targets)
- Data: custom dataset (renewables/EV-heavy); public status unclear
- Code available? not stated
- What it leaves open: This is the closest prior work to "causal/lead-lag edge prediction as link prediction". Its weaknesses define a gap (see D3): scale, label construction, econometric baselines, and validation.

##### harit2025news: From News to Returns: A Granger-Causal Hypergraph Transformer on the Sphere (CSHT)
- Citation: Harit, A., Sun, Z., Yu, J., *From News to Returns: A Granger-Causal Hypergraph Transformer on the Sphere*, ICAIF 2025 / arXiv:2510.04357.
- Link: https://arxiv.org/abs/2510.04357 ; full text https://arxiv.org/html/2510.04357v1
- First public: arXiv 2025-10-05; Venue: ACM ICAIF 2025; Peer-reviewed: yes
- Verification: [FT]
- Summary: Multivariate Granger tests (max lag 5, FDR p < 0.01) over lagged news, sentiment and returns define directed hyperedges on a daily, sliding-window graph. These hyperedges mask Transformer attention on a hypersphere. The model is trained on 450 S&P 500 stocks (train 2018–2020, validation 2021, test 2022–2023 H1), with Yahoo Finance prices and the public Twitter Financial News Sentiment data scored by FinBERT. It reports better MAE, regime accuracy and NDCG@10.
- Graph type: temporal; heterogeneous (news/sentiment nodes plus asset nodes); directed hypergraph; unsigned
- Task: regression and ranking plus regime classification, with Granger structure learning inside
- Data: public (Yahoo Finance; Twitter Financial News Sentiment)
- Code available? not stated
- What it leaves open: The "causal alignment" metric compares attention with a Granger graph built from the same data, which is circular. Case studies are qualitative. The authors say attention "does not imply interventional causality". The full text also has inconsistencies (tweets versus headlines; baselines listed differently in different sections).

##### rahmani2025flow: FANTOM: flow-based dynamic temporal causal models with regimes
- Citation: Rahmani, A., Frossard, P., *Flow based approach for Dynamic Temporal Causal models with non-Gaussian or Heteroscedastic Noises*, arXiv:2506.17065, 2025.
- Link: https://arxiv.org/abs/2506.17065 ; full text https://arxiv.org/html/2506.17065v2
- First public: arXiv 2025-06-20 (v2 2025-10-23; v1 titled "Flow-Based Non-stationary Temporal Regime Causal Structure Learning"); Venue: preprint; Peer-reviewed: no
- Verification: [FT]
- Summary: A Bayesian EM procedure jointly estimates the number of regimes, the regime boundaries, and one DAG per regime (instantaneous and lagged edges), with normalizing-flow noise that can be heteroscedastic or non-Gaussian. The paper proves identifiability. Baselines include CASTOR, CD-NOD, RPCMCI, PCMCI+, Rhino and DYNOTEARS. The real data are wind-tunnel (Causal Chambers) and EEG seizure recordings.
- Graph type: temporal, regime-switching; homogeneous; directed; unsigned
- Task: causal discovery with regime detection
- Data: no financial data
- Code available? not linked (baseline packages named: causalnex, Tigramite, causica, CASTOR code)
- What it leaves open: Finance is named as a motivation, but no market data are used. Edge signs and weights are not recovered, and the method struggles with short regimes, which are typical of crises. It is a clear transfer candidate for regime-switching stock causal graphs.

##### rabel2025context: Context-Specific Causal Graph Discovery with Unobserved Contexts
- Citation: Rabel, M., Runge, J., *Context-Specific Causal Graph Discovery with Unobserved Contexts: Non-Stationarity, Regimes and Spatio-Temporal Patterns*, arXiv:2511.21537, 2025.
- Link: https://arxiv.org/abs/2511.21537
- First public: arXiv 2025-11-26 (v2 2026-02-13); Venue: preprint; Peer-reviewed: no
- Verification: [ABS]
- Summary: The paper changes constraint-based causal discovery at the independence-test level so that context-specific (regime-dependent, non-stationary) graphs can be found when the contexts are unobserved. It is modular across PC, PC-stable, FCI, PCMCI, PCMCI+ and LPCMCI, and splits the problem into change-point detection, clustering and independence testing. The motivating application is gridded climate data.
- Graph type: temporal, context/regime-specific; homogeneous; directed
- Task: causal discovery
- Data: climate-style examples; no finance
- Code available? yes, https://github.com/martin-rabel/Causal_GLDF
- What it leaves open: It comes from the PCMCI (Runge) group and has code, but it is untested on equities. Applying it to sector or firm return panels, with VIX- or turbulence-defined contexts as a check, is an obvious transfer.

#### A2. Also opened (lower priority; short entries)
- **zan2026beyond**: Zan, Assaad, Devijver, Gaussier, *Beyond Stationarity in Time Series: Discovering Causal Structures and Latent Regimes via Markov Blankets* (RCBNB-MB), arXiv:2609.05150, 2026-09-04, preprint [ABS]. Joint regime segmentation and per-regime causal graphs with guarantees; tested on IT monitoring data, not finance. https://arxiv.org/abs/2609.05150
- **lee2026regimedependent**: Lee, C., *Source Exclusion in Regime-Gated Forecasting: A Cross-Market Audit of Equity-Factor Predictability*, arXiv:2601.10732 (v1 Jan 2026, titled in the listing "Regime-Dependent Predictive Structure Between Equity Factors: Evidence from Granger Causality"; v2 2026-09-26), preprint [ABS]. Shows that "dropping a lag block" is not the same as excluding a source when an HMM regime gate also uses it, and that none of 2,400 endpoints resolves a Granger direction under simultaneous confidence sequences. Relevant as a cautionary tale for regime-switching Granger graphs. https://arxiv.org/abs/2601.10732
- **jung2026common**: Jung, J., Lee, K., *Common Signals, Spurious Cross-Excitation, and Identification in Multivariate Hawkes Processes*, arXiv:2610.19428, 2026-10-14, preprint [ABS]. Latent common signals can mimic cross-excitation, and second-order information cannot separate the mechanisms (third-order cumulants can). Crypto application [crypto, transferable]: a direct theoretical warning against reading learned influence edges causally. https://arxiv.org/abs/2610.19428
- **yi2026causalcompass**: Yi, H., Shen, X., Wu, Y., et al., *CausalCompass: Evaluating the Robustness of Time-Series Causal Discovery in Misspecified Scenarios*, arXiv:2602.07915 (v1 2026-02-08, v2 2026-04-30), preprint [ABS]. Eight violation scenarios; no method wins everywhere; deep-learning methods are most robust; NTS-NOTEARS depends heavily on standardization. Code: https://anonymous.4open.science/r/CausalCompass-anonymous-5B4F/ ; https://arxiv.org/abs/2602.07915
- **redden2026causal**: Redden, B., Wang, H., Li, S., *Causal-INSIGHT: Probing Temporal Models to Extract Causal Structure*, arXiv:2603.25473, 2026-03-26, preprint [ABS]. Post-hoc, intervention-inspired clamping of a trained predictor to extract directed lagged influence graphs, with a ground-truth-free sparsity criterion (Qbic). It explicitly extracts what the predictor relies on, not true causal structure. https://arxiv.org/abs/2603.25473
- **fesanghary2026causalts**: Fesanghary, M. (Bloomberg), *Causal-TS: A Python Library for Causal Discovery in High-Dimensional and Nonstationary Time Series*, arXiv:2607.24673, 2026-07-27 [FT]. CDNOTS/CDNOTS+/CEDAR/GRACE plus Granger/LASSO-VAR wrappers, a regime pipeline (PELT/HMM/CUSUM), GPU CI tests, GPL-3.0; no finance experiments and no accuracy benchmarks. https://github.com/bloomberg/causal-ts ; https://arxiv.org/abs/2607.24673
- **wu2026do**: Wu, F., Liu, A., Chen, J., Li, Y., *Do News and Social Media Tell the Same Story? Constructing and Comparing Sentiment Spillover Networks*, arXiv:2604.26811 (2026-04-29), preprint [ABS]. Transfer-entropy sentiment spillover networks among tech firms; news-driven flow strengthens after COVID. https://arxiv.org/abs/2604.26811
- **tian2026tcgformer**: Tian, S., Li, S., *TCGformer: a temporal causal graph enhanced transformer model for multivariate financial time series forecasting*, PeerJ Computer Science, 2026, doi:10.7717/peerj-cs.3959 [ABS via CiteDrive record https://www.citedrive.com/en/discovery/tcgformer-a-temporal-causal-graph-enhanced-transformer-model-for-multivariate-financial-time-series-forecasting/ ; the PeerJ page was blocked]. Variable-lag Granger causal graph among financial institutions injected into a hierarchical Transformer via a GNN.
- **mallory2026two**: Mallory, M. L., *Two-Step Regularized HARX to Measure Volatility Spillovers in Multi-Dimensional Systems*, arXiv:2601.03146 (v1 2026-01-06, v4 2026-01-22) [ABS]. OLS own-dynamics plus ElasticNet cross-spillovers on 6 futures RVs; a sparse network with equity futures as transmitters. https://arxiv.org/abs/2601.03146
- **tabash2026time**: Tabash, M. I., et al., *Time-Varying Global Financial Stress Contagion in a Decade of Trade Wars and Geopolitical Fractures*, Risks 14(3):70, 2026-03-19 [ABS]. Pure TVP-VAR connectedness with no ML; representative of the crowded econometric spillover literature. https://www.mdpi.com/2227-9091/14/3/70
- **kameyama2026causality**: Kameyama, T., Kato, M., et al., *Causality Elicitation from Large Language Models*, arXiv:2603.04276, 2026-03-04 [ABS]. Elicits candidate causal graphs implicitly encoded by an LLM (econ.EM cross-list); explicitly not real-world causality. https://arxiv.org/abs/2603.04276
- **binish2026evolving**: Binish, G. V., et al., *Evolving Causal Regulatory Networks (ECR-Net)*, arXiv:2605.25211, 2026-05-24 [ABS]. Signed (activating/inhibiting) regulatory graphs evolved under distribution shift: conceptually close to "signed regime-switching causal graphs", but no finance data. https://arxiv.org/abs/2605.25211
- **deng2026lead**: Deng, R., Zhang, Y., *Lead-Lag Relationships in Financial Markets: A Comparison of Multiple Clustering Algorithms*, arXiv:2608.24703, 2026-08-25 [ABS]. Lead-lag clustering (MiniRocket-KMeans, KShape, DTW-KMedoids). https://arxiv.org/abs/2608.24703
- **hassan2026information**: Hassan, M. A., Situngkir, H., *Information Networks of Stock Prices*, arXiv:2606.07450, 2026-06-05 [ABS]. Undirected MI/correlation networks (Indonesia), validated against sector classification. A useful validation template, but not causal. https://arxiv.org/abs/2606.07450
- **zuniga2025fincare**: Zuniga (arXiv PDF lists "Michel"), A., Sarmah, B., Arun, A., Pasquali, S., *FinCARE: Financial Causal Analysis with Reasoning and Evidence*, NeurIPS 2025 Workshop on Generative AI in Finance / arXiv:2510.20221 (Oct 2025) [ABS]. PC/GES/NOTEARS constrained by a 10-K knowledge graph plus LLM priors; F1 gains on synthetic data only (500 firms × 18 variables). https://neurips.cc/virtual/2025/132542 ; https://arxiv.org/abs/2510.20221
- **thumm2025towards**: Thumm, D., Ontaneda Mijares, L., *Towards Causal Market Simulators*, ICAIF 2025 Workshop on Rethinking Financial Time-Series / arXiv:2511.04469 (v1 2025-11-06, v4 2026-01-21) [ABS]. TNCM-VAE generates counterfactual financial series that respect a DAG, which could supply synthetic ground truth for evaluation. https://arxiv.org/abs/2511.04469
- **stein2025causalrivers**: Stein, G., et al., *CausalRivers: Scaling up benchmarking of causal discovery for real-world time-series*, ICLR 2025 / arXiv:2503.17452 (2025-03-21) [ABS]. Largest real-world time-series causal discovery benchmark (river networks). No finance counterpart exists. https://arxiv.org/abs/2503.17452
- **tan2025magnet**: Tan, P., et al., *MaGNet: A Mamba Dual-Hypergraph Network for Stock Prediction via Temporal-Causal and Global Relational Learning*, arXiv:2511.00085 (2025-10-29) [ABS]. "Temporal-causal hypergraph" used as an architectural component; code https://github.com/PeilinTime/MaGNet ; https://arxiv.org/abs/2511.00085
- **hui2025deep**: Hui, Y., Liu, C., Song, X., *Deep learning based doubly robust test for Granger causality*, arXiv:2509.15798 (v1 2025-09-19) [ABS]. Neural, doubly robust Granger test with parametric rate and multiplier bootstrap; price-volume application in US, China and Japan. https://arxiv.org/abs/2509.15798
- **letteri2025framework**: Letteri, I., *A Framework for Predictive Directional Trading Based on Volatility and Causal Inference*, arXiv:2507.09347 (2025-07-12) [ABS]. GMM clusters, then Granger + custom PCMCI + effective TE, then DTW-KNN lags; 9 stocks and a 45-day backtest (weak evaluation). https://arxiv.org/abs/2507.09347
- **khalilian2025mapping**: Khalilian, P., et al., *Mapping Crisis-Driven Market Dynamics: A Transfer Entropy and Kramers-Moyal Approach to Financial Networks*, arXiv:2507.09554 (2025-07-13) [ABS]. Four assets; TE rises in crises. https://arxiv.org/abs/2507.09554
- **ataei2025multiscale**: Ataei, M., *Multiscale Causal Analysis of Market Efficiency via News Uncertainty Networks and the Financial Chaos Index*, arXiv:2505.01543 (2025-05-02) [ABS]. Granger networks of uncertainty indices; predictability holds at daily frequency and vanishes at monthly. https://arxiv.org/abs/2505.01543
- **fang2025from**: Fang, J., Wu, S., Tong, J., *From Data Acquisition to Lag Modeling: Quantitative Exploration of A-Share Market with Low-Coupling System Design*, arXiv:2506.19255 (2025-06-24) [ABS]. Coupling (DTW, correlation), then intraday Granger lead-lag in A-shares. https://arxiv.org/abs/2506.19255
- **hu2025study**: Hu, L., Wang, Q., *A Study of Dynamic Stock Relationship Modeling and S&P500 Price Forecasting Based on Differential Graph Transformer*, arXiv:2506.18717 (2025-06-23) [ABS]. Correlation-prior graph transformer; "causal" only in the sense of masked attention. https://arxiv.org/abs/2506.18717
- **souza2025forward**: Souza, L. A., *Forward-Oriented Causal Observables for Non-Stationary Financial Markets*, arXiv:2512.24621 (2025-12-31) [ABS]. "Causal" means non-anticipative filtering, not causal discovery (EURUSDT). https://arxiv.org/abs/2512.24621
- **[BACKGROUND] sadeghi2023causal**: Sadeghi, A., Gopal, A., Fesanghary, M., *Causal Discovery in Financial Markets: A Framework for Nonstationary Time-Series Data*, arXiv:2312.17375 (v1 2023-12-28; v4 2026-09-16) [ABS]. CDNOTS (lagged CD-NOD) applied to factor investing and market data. It is the finance origin of the Causal-TS library; v1 predates 2025, so it is labelled background. https://arxiv.org/abs/2312.17375
- Opened but irrelevant (not counted): PLOS ONE stockout-risk causal inference (doi 10.1371/journal.pone.0350429); arXiv:2503.20987 (the fetch returned body text without a title; it is about domain generalization, not graphs); Cucuringu 2026 CodEx seminar abstract "Signed and Directed Graph Clustering in Financial Time Series: Statistical Arbitrage, Lead-Lag Structure, and the Market Tug-of-War" (a talk, not a paper, but it confirms that signed and directed lead-lag clustering on US equities is an active line). https://www.math.colostate.edu/~king/codex/abstracts/cucuringu2026.html


---

## T7. Credit default → stock market

**Synthesis.** GNN default or credit scoring on **private** enterprise, tax-invoice or guarantee graphs (China, Korea; e.g. DefaultGNN, CIKM 2026) is crowded and not reproducible for a UU student. **Predicting how a credit event moves connected listed firms' stock prices, on public data, is thin: no 2025–2026 paper does it directly.** The nearest papers each stop one step short. Brogaard & Chen 2026 have credit as the endpoint, industry-level edges and commercial data. Zelvyte 2026 is CDS-only. Lin Zhang 2026 is simulation-only. The bottleneck is a public graph plus public credit-event labels (bankruptcy databases, 8-K Item 1.03, 17g-7 rating histories with a 12-month delay), so part of the contribution would be the dataset. Verdict: **moderate overall; open niche, data-limited.**

Full per-paper notes (from `sweep/T7.md`; raw query log and unopened leads stay there):

##### kim2026defaultgnn: DefaultGNN: A Dual-Perspective GNN Framework for Predicting Corporate Default from Buyer-Seller Transaction Networks
- Citation: Kim, J., Kim, H., Choi, S., et al., *DefaultGNN: A Dual-Perspective GNN Framework for Predicting Corporate Default from Buyer-Seller Transaction Networks*, CIKM 2026 (arXiv:2609.25542), 2026.
- Link: https://arxiv.org/abs/2609.25542 ; https://arxiv.org/html/2609.25542
- First public: arXiv v1 2026-09-22. Venue: CIKM '26 (Rome, Nov 2026), DOI 10.1145/3799682.3840153. Peer-reviewed: yes.
- Verification: [FT]
- Summary: Builds buyer-view and seller-view multiplex graphs from six years of Korean electronic tax invoices, 2018–2023 (~0.8–1.0M firms, ~42M transactions per year). Two GCN encoders are fused per firm by a sigmoid gate, with a view-consistency regulariser, to predict next-year default.
  - It beats R-GCN, CompGCN, DMGI, DMG, MGHC and XGBoost on Accuracy Ratio. Example for 2021: AR 0.583 against ~0.551 for the best baseline.
  - The gain is largest for firms without a default history.
  - In deployment with a rating agency it raised approval rates by 7–11 pp with no rise in defaults.
- Graph type: yearly snapshots (discrete-time); homogeneous nodes (firms); directed, weighted multiplex (two views); unsigned.
- Task: node classification (next-year default). Explanations come from integrated gradients over 2-hop subgraphs.
- Data: Douzone Bizon e-tax invoices, with defaults labelled by Techfin Ratings (Basel II definition). Not public; the authors used a secure, access-controlled environment.
- Code: https://github.com/jhkim611/DefaultGNN
- What it leaves open:
  - The main results use a random stratified 70/10/20 split; out-of-time tests are only a robustness check.
  - Explanations are shown as two case studies with no faithfulness evaluation.
  - The firms are unlisted SMEs, so there are no equity prices and no credit→equity channel.

##### brogaard2026network: The Network Foundations of Credit Counterparty Risk: Theory and Evidence
- Citation: Brogaard, J., Chen, B., *The Network Foundations of Credit Counterparty Risk: Theory and Evidence*, working paper. It was previously circulated as "Attention-Based Graph Neural Networks in Firm CDS Prediction".
- Link: https://skbi.smu.edu.sg/sites/skbi.smu.edu.sg/files/2026-06/Day2_945_Belinda%20Chan.%20(paper).pdf
- First public: unknown. The PDF is hosted on the SMU SKBI conference site under a 2026-06 path, and its references include a 2025 SSRN paper. Venue: conference working paper. Peer-reviewed: no/unknown.
- Verification: [FT]
- Summary: A CES production-network model in which supplier productivity shocks pass through input–output links into customers' cash flows. From there they reach equity value and volatility, then Merton distance-to-default (via Bharath–Shumway), then CDS spreads.
  - Empirically, edges come from industry-level idiosyncratic-volatility spillover networks.
  - A GCN-style GNN with these edges improves rolling monthly out-of-sample CDS-spread prediction against a no-edge benchmark, especially in network-sensitive sectors.
- Graph type: industry-level weighted spillover network (directionality not stated in what I read), re-estimated over rolling windows; homogeneous; unsigned.
- Task: regression (CDS spread prediction) plus a structural theory model.
- Data: partly public.
  - Commercial: Markit 5y CDS (XR14), 2005–2020; CRSP daily returns.
  - Public: Fama–French factors and industries; Welch–Goyal macro predictors.
  - Gu–Kelly–Xiu firm characteristics (source database not named).
- Code: not stated.
- What it leaves open:
  - Edges are at industry level, not firm-to-firm.
  - Equity is an intermediate channel and credit is the endpoint. The reverse question (credit shock → connected firms' stock returns) is not tested.
  - The commercial CDS and CRSP data block replication with public data.

##### zelvyte2026multiplex: A Multiplex Network Hawkes Model for Systemic Risk Measurement
- Citation: Zelvyte, M., Griffin, J. E., *A Multiplex Network Hawkes Model for Systemic Risk Measurement*, arXiv:2606.15755, 2026.
- Link: https://arxiv.org/abs/2606.15755 ; https://arxiv.org/html/2606.15755v1
- First public: arXiv v1 2026-06-14. Venue: preprint. Peer-reviewed: no.
- Verification: [FT]
- Summary: Extends the network Hawkes process (Linderman & Adams) to several excitation layers, with edge weights driven by covariates: asset similarity (sector, industry, revenue region), solvency and profitability. Posterior inference is by MCMC.
  - Events are extreme CDS jumps above each firm's 99th percentile: 1,905 events across 99 NA/EU firms, 2004–2022.
  - The inferred contagion network is sparse and directional, with transmission concentrated in a few outward hubs (AIG, Citigroup, Tyson, Marriott, HeidelbergCement).
  - Industry similarity is the most consistently supported channel.
- Graph type: latent directed weighted multiplex inferred from event times. Time variation is handled only by sub-period refits (GFC, euro crisis, COVID); unsigned.
- Task: structure learning / contagion-pathway inference (temporal point process).
- Data: daily 5y CDS log-returns. The vendor is not named in what I read, so likely commercial; public status unknown.
- Code: not stated.
- What it leaves open:
  - The model is CDS-only, with no equity layer or credit→equity cross-excitation.
  - The event threshold is fixed over the full sample.
  - The authors list time-varying and dynamic link formation as future work.
  - Edges are predictive, not exposures.

##### zhang2026bankruptcy: Bankruptcy Prediction from 10-K Narratives: Evidence from Interpretable Text Scores and Accounting Baselines
- Citation: Zhang, Z., Zheng, M., Zhang, T., et al., *Bankruptcy Prediction from 10-K Narratives: Evidence from Interpretable Text Scores and Accounting Baselines*, arXiv:2606.05623, 2026.
- Link: https://arxiv.org/abs/2606.05623 ; https://arxiv.org/html/2606.05623v1
- First public: arXiv v1 2026-06-04. Venue: preprint. Peer-reviewed: no.
- Verification: [FT] (first ~100k characters)
- Summary: Builds a dictionary-based "PB Stress" score from 10-K Items 1, 1A and 7. It covers liquidity, covenants and refinancing, operating decline, restructuring and legal issues, and business fragility.
  - Adding the score to a five-variable accounting model raises one-year bankruptcy AUC from 0.832 to 0.902.
  - Sample: 40,475 firm-years from 10,778 SEC firms, 2010–2021, with 128 bankruptcies.
  - Labels come from the **Florida-UCLA-LoPucki BRD**.
- Graph type: none (included for its public-data label pipeline and as a node-feature source).
- Task: classification (bankruptcy within 365 days).
- Data: mostly public. Sources are the EDGAR APIs and structured SEC financial-statement data, 10-K text, and BRD labels (accessed 2 May 2026; BRD access terms not verified by me).
- Code: not stated in the part I read.
- What it leaves open:
  - It uses no network, peer or contagion information. Customer/supplier concentration appears only as dictionary words.
  - There are only 128 events.
  - The primary split is a fixed 80/20 holdout that is not stated to be chronological (out-of-time testing is a robustness check).

##### majhi2026modeling: Modeling structure and credit risk of the economy: a multilayer bank-firm network approach
- Citation: Majhi, S., Mancini, A., Cimini, G., *Modeling structure and credit risk of the economy: a multilayer bank-firm network approach*, arXiv:2603.09854, 2026.
- Link: https://arxiv.org/abs/2603.09854
- First public: arXiv v1 2026-03-10. Venue: preprint (physics.soc-ph, cross-listed q-fin.RM). Peer-reviewed: no.
- Verification: [ABS]. The HTML full text was rate-limited twice.
- Summary: Reconstructs a multilayer economy from bank and firm balance sheets alone, because the real networks are privacy-protected. The layers are firm supplier–customer links, bank→firm corporate loans and interbank exposures. It then models shocks spreading from the inter-firm layer into the interbank layer.
  - Applied to Italy, it identifies systemically important firms and industries and vulnerable banks.
  - The authors frame it as a "digital twin" for stress tests.
- Graph type: static multilayer, heterogeneous (firms, banks); directionality and weights not verified at abstract level.
- Task: network reconstruction plus shock-propagation simulation.
- Data: Italian bank and firm balance sheets; source not named on the abstract page.
- Code: not stated.
- What it leaves open:
  - Equity prices are not used, and the reconstructed contagion is not checked against observed market reactions.
  - The lender layer (bank–firm) is the channel most ML papers in this sweep ignore.

##### zhang2026temporal: Temporal Attentive Graph Networks for Financial Surveillance: An Incremental Multi-Scale Framework
- Citation: Zhang, W., Shen, Y., Zhou, H., et al., *Temporal Attentive Graph Networks for Financial Surveillance: An Incremental Multi-Scale Framework*, Journal of Sensor and Actuator Networks 15(1):23, 2026.
- Link: https://www.mdpi.com/2224-2708/15/1/23
- First public: 2026-02-16 (online). Venue: J. Sensor and Actuator Networks (MDPI). Peer-reviewed: yes.
- Verification: [FT]
- Summary: TAGN combines GAT and GRU over a multi-scale graph of high-frequency trading co-movement, supply-chain links and institutional co-holdings (SEC 13F), for S&P 500 firms from 2018 to 2024.
  - It reports AUC 0.890 and Precision@50 of 61.5%, beating WinGNN and PatchTST.
  - A TAGN risk index led the VIX by 1–2 weeks around stress events such as the SVB collapse.
- Graph type: temporal (snapshot plus GRU), multi-relational (co-movement, supply chain, co-ownership); unsigned.
- Task: anomaly / risk-state detection (classification) plus a systemic-risk index.
- Data: partly public. The paper names EDGAR, 13F and Yahoo Finance. However, its Section 3.2 says the supply-chain graph is built from FactSet, which is commercial. This contradicts the paper's statement that all data is public.
- Code: https://gitee.com/Maxwell_Peng_pengjin/temporal-attentive-graph-networks-for-financial-surveillance.git ; data at Zenodo DOI 10.5281/zenodo.17785156.
- What it leaves open:
  - Not specific to credit events.
  - The data-provenance inconsistency needs checking before reuse.
  - The VIX lead-lag claim covers only a few stress episodes.

##### zhang2026llmenhanced: LLM-Enhanced Dynamic Financial Knowledge Graphs for Cross-Entity Signal Propagation and Alpha Discovery
- Citation: Zhang, L., *LLM-Enhanced Dynamic Financial Knowledge Graphs for Cross-Entity Signal Propagation and Alpha Discovery*, arXiv:2607.10932, 2026.
- Link: https://arxiv.org/abs/2607.10932 ; https://arxiv.org/html/2607.10932v1
- First public: arXiv v1 2026-07-12. Venue: preprint (stat.AP). Peer-reviewed: no.
- Verification: [FT]
- Summary: An LLM pulls events and typed relations from filings, calls and trade press. Relation types are Supplier, Customer, Competitor, Technology, CapExExposure and Industry. Edge weights are exponentially decayed evidence counts, and the graph refreshes monthly.
  - Event signals spread by damped K-step diffusion, gated so that propagation inside a Louvain community is stronger than across communities. This yields the CIS and PIS factors.
  - Evaluation is on controlled simulations and a Russell-1000-calibrated simulation only. In the calibrated setting, no variant clears 5 bps costs as a standalone strategy.
- Graph type: dynamic, directed, typed (heterogeneous relations); weights unsigned. The event magnitudes are signed.
- Task: signal propagation for return ranking (rank IC, long–short Sharpe, Fama–MacBeth).
- Data: simulation only. A live Russell 1000 study is proposed.
- Code: replication materials are "accompanying the draft"; no link.
- What it leaves open:
  - There is no live test.
  - The event types exclude defaults, bankruptcies and rating actions.
  - It does not show whether the propagation operator and gating hold for credit shocks.

##### pereda2026systemic: Systemic Risk and Default Cascades in Global Equity Markets: A Network and Tail-Risk Approach Based on the Gai Kapadia Framework
- Citation: Castillo Pereda, A. I., *Systemic Risk and Default Cascades in Global Equity Markets: A Network and Tail-Risk Approach Based on the Gai Kapadia Framework*, arXiv:2604.19796, 2026. Earlier related version: arXiv:2504.01969 (v1 2025-03-20, v2 2025-04-04) [ABS].
- Link: https://arxiv.org/abs/2604.19796 ; https://arxiv.org/html/2604.19796v1 ; https://arxiv.org/abs/2504.01969v2
- First public: arXiv v1 2026-04-10. Venue: preprint. Peer-reviewed: no.
- Verification: [FT] for the 2026 version; [ABS] for the 2025 version.
- Summary: Applies the Gai–Kapadia interbank cascade model to 30 stocks (15 Brazilian, 15 developed-market) from 2015 to April 2026.
  - Exposures are E_ij = ρ_ij·σ_i·P_i, thresholded at 0.3 or 0.5.
  - "Default" means an artificial capital buffer of 0.2·P falls below 0.1·P.
  - Across 1,000 Monte Carlo runs no systemic failures occur. Emerging-market tails are heavier.
- Graph type: static correlation-threshold exposure network (non-negative; the author calls it directed and weighted).
- Task: contagion simulation plus tail analysis.
- Data: public (yfinance).
- Code: not stated.
- What it leaves open:
  - "Defaults" are synthetic, not real credit events.
  - The correlations are full-sample, which brings look-ahead.
  - The network has only 30 nodes.
  - There are internal inconsistencies (clustering numbers, Hill index).
  - It shows how weak current "equity default cascade" work is.

##### mori2026identifiability: Identifiability of Contagion Components amid Environmental Fluctuations in Aggregated Default Counts
- Citation: Mori, S., *Identifiability of Contagion Components amid Environmental Fluctuations in Aggregated Default Counts*, arXiv:2604.18118, 2026.
- Link: https://arxiv.org/abs/2604.18118
- First public: arXiv v1 2026-04-20. Venue: preprint (q-fin.RM, cond-mat.stat-mech). Peer-reviewed: no.
- Verification: [ABS]
- Summary: Asks whether contagion can be told apart from common "environmental" variation in default probabilities when only annual default counts by rating class are observed.
  - It compares a Probit-Normal binomial mixture baseline with Davis–Lo cumulative contagion and Torri threshold contagion.
  - Davis–Lo contagion is identifiable for the ALL and speculative-grade classes.
  - The Torri effect is mostly absorbed by environmental heterogeneity.
- Graph type: none (mean-field contagion in counts).
- Task: statistical identification / model comparison.
- Data: aggregated annual default counts by rating class; source not named on the abstract page.
- Code: not stated.
- What it leaves open: there is no firm-level network. It formalises the main confound that any network-contagion thesis must handle: separating contagion from common shocks.

##### zandi2025multimodal: A Multimodal Approach to SME Credit Scoring Integrating Transaction and Ownership Networks
- Citation: Zandi, S., Korangi, K., Moreno-Paredes, J. C., et al., *A Multimodal Approach to SME Credit Scoring Integrating Transaction and Ownership Networks*, arXiv:2510.09407, 2025.
- Link: https://arxiv.org/abs/2510.09407v1 ; https://arxiv.org/html/2510.09407v1
- First public: arXiv v1 2025-10-10. Venue: preprint. Peer-reviewed: no.
- Verification: [FT]
- Summary: GAT and GIN models over a two-layer temporal network of loans: directed, weighted financial transactions plus common ownership, with six monthly snapshots. They are fused with tabular data through hybrid concatenation and cross-attention.
  - Data: ~218k SMEs and 1.2M loans, with an out-of-time test in 2021.
  - The best bimodal model reaches AUC 0.879 against 0.863 for XGBoost, and the confidence intervals overlap.
  - Being on the receiving end of transactions from defaulted firms carries more risk than sending to them.
- Graph type: temporal (snapshots), multilayer; directed and weighted (transaction layer); unsigned.
- Task: node classification (default).
- Data: proprietary lender; not public.
- Code: not stated.
- What it leaves open:
  - Data comes from a single lender.
  - Gains over XGBoost are within the confidence intervals.
  - The firms are not listed.
  - Directional contagion asymmetry is shown but not tested causally.

##### li2025heterogeneous: Heterogeneous Graph Pre-training Based Model for Secure and Efficient Prediction of Default Risk Propagation among Bond Issuers
- Citation: Li, X., Shan, X., Yin, W., Wang, H., *Heterogeneous Graph Pre-training Based Model for Secure and Efficient Prediction of Default Risk Propagation among Bond Issuers*, arXiv:2501.03268, 2025.
- Link: https://arxiv.org/abs/2501.03268v1 ; https://arxiv.org/html/2501.03268v1
- First public: arXiv v1 2025-01-04. Venue: preprint. Peer-reviewed: unknown.
- Verification: [FT]
- Summary: Pre-trains a heterogeneous masked graph autoencoder (HGMAE, GAT backbone) on a Chinese enterprise KG with more than 20M nodes, seeded with 6,714 bond issuers. Relation types are parent–subsidiary, shareholder, shared manager or legal person, and investment.
  - XGBoost then classifies (source-defaulter, target) pairs: does the target default after the source? There are 3,658 pairs.
  - Micro-F1 is 0.831 against 0.794 for plain XGBoost.
- Graph type: static, heterogeneous, directed relations; unsigned.
- Task: pair-level "default propagation" prediction (effectively edge-conditioned link/activation classification).
- Data: proprietary Chinese EKG; not public.
- Code: not stated.
- What it leaves open:
  - The split is random 80/20 and the only metric is Micro-F1, with no variance reported.
  - The "secure/private" claims are not evaluated.
  - It does not look at market reactions.
  - It is still the closest framing to "which connected firm is hit next".

##### du2025enterprise: Enterprise Bankruptcy Prediction Model Based on Heterogeneous Graph Neural Network for Fusing External Features and Internal Attributes
- Citation: Du, X., Cao, J., Jiang, X., et al., *Enterprise Bankruptcy Prediction Model Based on Heterogeneous Graph Neural Network for Fusing External Features and Internal Attributes*, Mathematics 13(17):2755, 2025.
- Link: https://mdpi.com/2227-7390/13/17/2755
- First public: 2025-08-27. Venue: Mathematics (MDPI), DOI 10.3390/math13172755. Peer-reviewed: yes.
- Verification: [FT] (first 100k characters; the code statement was not reached)
- Summary: HGNN-EBP builds a multi-relational graph from financial data, text and industry information. It uses multi-scale graph convolution, Transformer self-attention to impute missing external features, and an MLP head. On 32,459 Chinese enterprises it beats traditional and graph-based baselines.
- Graph type: static, heterogeneous, multi-relational.
- Task: node classification (bankruptcy).
- Data: Tianyancha "IFO" dataset (commercial). Not public; the authors state the raw data cannot be disclosed.
- Code: not verified.
- What it leaves open: the data is non-reproducible, the firms are not listed, there is no equity channel, and temporal dynamics are not modelled explicitly.

##### agca2022credit: [BACKGROUND] Credit Shock Propagation Along Supply Chains: Evidence from the CDS Market
- Citation: Agca, S., Babich, V., Birge, J. R., Wu, J., *Credit Shock Propagation Along Supply Chains: Evidence from the CDS Market*, Management Science, pp. 6506–6538, 2022.
- Link: https://academicnewsletter.sufe.edu.cn/info/399670 (record page; DOI 10.1287/mnsc.2021.4174)
- First public: 2022 (journal). Venue: Management Science. Peer-reviewed: yes.
- Verification: [ABS]
- Summary: Adverse CDS jumps at customers or suppliers raise a firm's 3-day abnormal CDS change by ~63–74 bp, and favourable jumps lower it by ~36–38 bp.
  - The effects roughly double for natural-disaster shocks.
  - There is no effect in inactive supply chains, and the effect is stronger with shared analyst coverage.
  - Adverse shocks propagate to second and third tiers.
- Graph type: directed supply-chain links, multi-tier; static per period.
- Task: event study / econometrics.
- Data: CDS panel plus supply-chain links (sources not named on the record page; likely commercial).
- Code: not stated.
- What it leaves open: credit→credit only, with no prediction of which neighbours react and no equity endpoint. This is the canonical evidence a credit→equity ML thesis would build on.

##### qiu2024contagious: [BACKGROUND] Contagious Stock Price Crashes along the Supply Chain
- Citation: Qiu, B., Xu, F., Yeung, A., Zeng, C., *Contagious Stock Price Crashes along the Supply Chain*, Production and Operations Management 33(8):1679–1699, 2024.
- Link: https://pure.ulster.ac.uk/en/publications/contagious-stock-price-crashes-along-the-supply-chain/
- First public: 2024-05-02 (online). Venue: POM. DOI 10.1177/10591478241254854. Peer-reviewed: yes.
- Verification: [ABS]
- Summary: Crash risk spreads from major customers to suppliers with a lag of **up to two weeks**, linked to supplier information opacity. The effect is stronger when the customer matters more, and operational incidents hit harder than financial ones.
- Graph type: directed customer→supplier links.
- Task: econometric event analysis.
- Data: not stated on the record page.
- Code: not stated.
- What it leaves open:
  - The multi-day lag implies a forecastable window that no ML paper in this sweep exploits.
  - The effect of credit events, as opposed to crashes in general, is not isolated.

#### A2. Also opened, peripheral (compact entries; all fields checked as above)

- **yang2026relational**: Yang, Y., Vasistha, E., *Relational Graph Modeling for Credit Default Prediction: Heterogeneous GNNs and Hybrid Ensemble Learning*, arXiv:2601.14633 (v1 2026-01-21; preprint) [FT]. https://arxiv.org/abs/2601.14633
  - Setup: a heterogeneous foreign-key graph (31M nodes, 50M edges) on the consumer Home Credit Default Risk data, with a random split.
  - Results: the standalone GNNs (AUC 0.740–0.751) lose to LightGBM (0.769). A hybrid of GNN embeddings plus LightGBM is best (0.782).
  - Code: not stated.
  - Relevance: an evaluation lesson, since GNNs alone rarely beat GBDT. It is consumer credit, not a firm network.
- **karanjai2026causalgraphx**: Karanjai, R., Madhavarao, H., Xu, L., Shi, W., *CausalGraphX: A Counterfactual Graph Neural Network Framework for Explainable Systemic Risk Assessment*, arXiv:2607.14416 (v1 2026-07-15; "Accepted in AAAI'26 Workshop, Agentic AI in Financial Services"; workshop peer review) [ABS]. https://arxiv.org/abs/2607.14416
  - Method: GAT plus adversarial regularisation, with counterfactual explanations such as the minimum capital injection that would have prevented a default.
  - Data: synthetic networks only. Code: not stated.
  - Relevance: an explainable-contagion template that has not been tested on real data.
- **jungbluth2026market**: Jungbluth, A., Lederer, J., Trimborn, S., *Market-Informed Networks for Modeling and Forecast Evaluation of Financial Extremes*, arXiv:2609.11575 (v1 2026-09-10; preprint) [ABS]. https://arxiv.org/abs/2609.11575
  - Method: a time-dependent network Hüsler–Reiss model for joint extremes. The JEAM adjacency improves tail log-scores by ~11–15%.
  - Data: one-minute S&P 100 returns from three sectors. Code: not stated.
  - Relevance: a tail-dependence network as a candidate "crash together" layer.
- **wang2026design**: Wang, L., Zhong, Z., Tian, Z., Chen, K., *Design of a Deep Learning Credit Risk Early Warning System Integrating Multi-source Heterogeneous Data*, arXiv:2609.15744 (v1 2026-09-14; DOI 10.1145/3801438.3804860; venue unknown) [ABS]. https://arxiv.org/abs/2609.15744
  - No dataset is named and no firm graph is described.
  - Listed only as an example of the low-information credit-AI papers that inflate the apparent crowding.
- **zhang2025credit**: Zhang, Z., Shen, Q., Hu, Z., et al., *Credit Risk Analysis for SMEs Using Graph Neural Networks in Supply Chain*, arXiv:2507.07854 (v1 2025-07-10, v2 2025-07-20; preprint) [FT]. https://arxiv.org/abs/2507.07854v2
  - Method: two stages, supply-chain link mining (AUC ~0.99) then default prediction (AUC ~0.70).
  - Data: Alipay/Ant SME graphs (proprietary; the abstract says "Discover and Ant Credit"). Stratified random split.
  - Quality: the text and Table 3 disagree.
  - Code: not stated.
  - Relevance: a "mine the links, then propagate" pipeline.
- **shi2025supply**: Shi, J., Deng, M., Yang, J., Zhang, R., *Supply chain contagion effects of negative CSR events: A stock market reaction perspective*, Global Finance Journal 64, 2025, DOI 10.1016/j.gfj.2025.101080 (peer-reviewed) [ABS]. https://ideas.repec.org/a/eee/glofin/v64y2025ics1044028325000079.html
  - Method: an event study on Chinese A-shares, 2016–2021. Negative CSR events spread along supply chains, and customers are hit harder than suppliers.
  - Relevance: a supply-chain event-study template that transfers directly to credit events.
- **oskarsdottir2025multimodal**: Óskarsdóttir, M., Morthens, M. F., Tavakoli, M., Bravo, C., *A Multi-Modal Framework for Credit Risk Assessment: Capturing Corporate Financial Patterns with Sequential and Graph Data*. Conference talk abstract on the Credit Research Centre (Univ. of Edinburgh) site [ABS]. https://www.crc.business-school.ed.ac.uk/sites/crc/files/2025-11/A-Multi-Modal-Framework-for-Credit-Risk-Assessment_-Capturing-Corporate-Financial-Patterns-with-Sequential-and-Graph-Data-abstract.pdf
  - Date: unknown. The URL path is 2025-11, and Bravo already described the model in a Fields Institute talk on 2024-11-27 (https://gfs.fields.utoronto.ca/talks/Financial-Risk-Contagion-Models-Using-Dynamic-Multimodal-Graph-Learning). The work may therefore be 2024.
  - Method: credit-rating prediction for 318 US firms over 11 years. Quarterly statements go through LSTM/GRU/Transformer, plus a GCN on a TMFG-filtered volatility-correlation graph. Evaluation is both transductive and inductive.
  - Relevance: the closest listed-firm, mostly public-data design I found. No full paper was located.


---

## T8. Wildcards (LLM-built graphs, explainers, shocks, rigour, hypergraphs, Europe)

**Synthesis.** LLM/text-built company graphs are growing fast (about 12 papers in 2025–2026), but each uses one LLM and one prompt; nobody varies construction choices or tests edge-level look-ahead contamination. Temporal-GNN explainers are active in core ML (ICML/ICLR 2026) but have **never been evaluated on financial graphs**. Tariff/geopolitical shock propagation is econ-heavy and uses proprietary one-hop networks. Hypergraph stock models are crowded at the architecture level. **European markets (AEX/Euronext/STOXX 600) are essentially absent** from 2025–2026 stock-graph work. Verdict: **thin at the intersections** relevant to this thesis.

Full per-paper notes (from `sweep/T8.md`; raw query log and unopened leads stay there):

##### A.1 (a) Company graphs built from text with LLMs

##### huang2026cross — Cross-Stock Predictability via LLM-Augmented Semantic Networks
- Citation: Huang, Y., Fan, Z., Hu, K., Ye, Y., *Cross-Stock Predictability via LLM-Augmented Semantic Networks*, arXiv:2604.19476, 2026.
- Link: https://arxiv.org/abs/2604.19476 (FT: https://arxiv.org/html/2604.19476)
- First public: v1 21 Apr 2026 (v2 26 Apr 2026); Venue: preprint (q-fin.PM/GN/ST); Peer-reviewed: no
- Verification: [FT]
- Summary: The candidate graph is built from the top-K (K=5) cosine neighbours of 10-K risk-factor embeddings. DeepSeek-Chat (temperature 0) then classifies each candidate pair from 10-K Item 1, product/segment text and competitor sentences, with firms anonymised as "Firm A"/"Firm B". The taxonomy is competitor / supply_chain / complementary / substitute / peer / unrelated. Competitor edges are removed and substitute edges down-weighted, and the remaining edges feed a pairs-style mean-reversion signal. On S&P 500 constituents (CRSP, 2011–2019 backtest), long-short Sharpe rises from 0.742 to 0.820. Robustness checks cover a random graph (Sharpe 0.541, not significant), a 2-digit SIC graph (0.792) and K ∈ {3, 5, 10}.
- Graph type: static per year (rolling); homogeneous nodes; typed (multi-relational) undirected edges; unsigned in form, though relation types are effectively signed by their weighting.
- Task: ranking / cross-sectional return prediction (trading signal)
- Data: CRSP (licensed) + SEC 10-K (public). Partly public.
- Code available? not stated
- What it leaves open: Only one LLM and one prompt are tested, with no prompt, model or threshold variants. The LLM's knowledge cutoff relative to the 2011–2019 backtest is not stated, and memorisation is not tested; only anonymisation is used. The LLM graph (0.820) beats SIC (0.792) by only a small margin, and no significance test of that difference is reported.

##### yang2026llm — LLM Latent Edge Measurement: Point-in-Time Economic Graphs for Quantitative Investing from Corporate Disclosures
- Citation: Yang, F., Zhang, L., *LLM Latent Edge Measurement: Point-in-Time Economic Graphs for Quantitative Investing from Corporate Disclosures*, arXiv:2607.15640, 2026.
- Link: https://arxiv.org/abs/2607.15640 (FT: https://arxiv.org/html/2607.15640)
- First public: v1 17 Jul 2026; Venue: preprint (stat.AP); Peer-reviewed: no
- Verification: [FT]
- Summary: Treats building a firm adjacency matrix as a *measurement* problem. Seven filing agents and two "knowledge" agents (parametric memory only, one of them prompted to be sceptical) produce evidence records with verbatim quotes. Records are fused by weighted logit voting around a sparse prior. A shared-counterparty layer is weighted by IDF, and edges are kept by relative per-firm quantile thresholding (τ = 0.6, floor 0.04). On 42 Nasdaq-100 firms this gives 149 directed edges. An adversarial web-search audit of 30 edges finds 88% confirmed for weight ≥ 0.1, and all refuted edges are low-weight. Edges fall within-sector 1.9× more often than random. Ablations show IDF filtering and relative thresholding matter.
- Graph type: static snapshot (point-in-time by filing date); directed, weighted; two layers (direct + shared-counterparty)
- Task: other (graph measurement / construction). The authors explicitly make no return claims.
- Data: SEC EDGAR 10-K/10-Q/20-F (public). Public.
- Code available? Stated as "released with the paper" (≈950 evidence records + matrices); no URL in the text.
- What it leaves open: One snapshot of 42 firms, one unnamed "Claude" model, and a 30-edge audit. 28.3% of mentions are category-level ("uranium suppliers") and cannot be resolved. The authors say the knowledge channel cannot be used for historical backtests because of look-ahead bias. There is no dynamic version and no downstream test.

##### kose2026reconstructing — Reconstructing temporal multi-relational firm networks at scale using large language models. The case of the semiconductor industry
- Citation: Köse, S., Diem, C., Dervic, E., et al. (incl. Klimek, P.), *Reconstructing temporal multi-relational firm networks at scale using large language models*, arXiv:2605.15842, 2026.
- Link: https://arxiv.org/abs/2605.15842 (FT: https://arxiv.org/html/2605.15842v1)
- First public: v1 15 May 2026; Venue: preprint (also listed as an INET Oxford working paper 2026-14 in search snippets); Peer-reviewed: no
- Verification: [FT]
- Summary: Uses Common Crawl snapshots (two per year, 2015–2025) of firm websites for ~21k ORBIS semiconductor firms. GPT-4o-mini (≈130 USD) assigns each domain pair one of: supplies, supplied-by, partners, owns, owned-by, none. Precision is 0.884 and F1 0.784 on 100 manually annotated pages. About 22% of identified edges (380 links) also appear in S&P Capital IQ, 2.3× the configuration-model expectation (z = 23.1). The network shows a 9% edge drop during the 2022 chip shortage, rising NVIDIA centrality, and a 2024–25 contraction of US–China links that the authors attribute to export controls ("strategic decoupling").
- Graph type: temporal (yearly snapshots); multi-relational, directed; heterogeneous by country
- Task: other (network reconstruction + descriptive temporal network analysis)
- Data: Common Crawl, OECD ICIO and BACI are public. ORBIS and S&P Capital IQ (validation) are licensed. Partly public.
- Code available? on request only
- What it leaves open: No stock returns are used. Edge timestamps are only crawl-snapshot years. The authors do not say whether the decoupling they observe is priced, or whether it can be predicted (link prediction / anomaly detection). The method is not applied beyond semiconductors.

##### niu2026relational — Relational Probing: LM-to-Graph Adaptation for Financial Prediction
- Citation: Niu, Y., Jin, C., Dolphin, R., Dong, R., *Relational Probing: LM-to-Graph Adaptation for Financial Prediction*, arXiv:2604.10212, 2026.
- Link: https://arxiv.org/abs/2604.10212 (FT: https://arxiv.org/html/2604.10212)
- First public: v1 11 Apr 2026; Venue: preprint (cs.CL); Peer-reviewed: no
- Verification: [FT]
- Summary: Replaces an LM's output head with a "relation head", a bilinear scorer over ticker embeddings attending to hidden states. Small Qwen3 models (0.6B/1.7B/4B) produce daily stock graphs from news (adjacency threshold τ = 0.5). These graphs are trained end-to-end with an LSTM+GAT three-class movement classifier. The method beats a news co-occurrence graph, but absolute gains are small (best MCC 0.056, AUC 0.557).
- Graph type: dynamic (daily); homogeneous; weighted
- Task: classification (3-class movement)
- Data: news dataset from Niu et al. (2024) with ticker labels. Public status not stated.
- Code available? not stated
- What it leaves open: The induced graphs are never inspected for economic meaning. Labels use a full-sample standard deviation (a possible leakage source). Only one model family and one dataset are used, and no variance is reported. No industry/GICS baseline is used in experiments.

##### zhou2025zero — Zero-Shot Extraction of Stock Relationship Graphs with LLMs
- Citation: Zhou, H., Costa Sperb, L. F., Ma, T., *Zero-Shot Extraction of Stock Relationship Graphs with LLMs*, Proc. FinNLP Workshop @ EMNLP 2025.
- Link: https://preview.aclanthology.org/ingest-emnlp/2025.finnlp-2.2.pdf
- First public: 2025 (EMNLP 2025 workshop proceedings); Venue: FinNLP @ EMNLP 2025; Peer-reviewed: yes (workshop)
- Verification: [FT]
- Summary: GPT-4o-mini is prompted for *every* S&P 500 pair, using only its prior knowledge with no text input. It assigns one of 24 S&P Global Business Relationship types plus a confidence score. Rare types are pruned, leaving 13. The resulting graph feeds RGCN/RGAT ranking models. The test period (Oct 2023 – Dec 2024) starts after the model's stated knowledge cutoff. The LLM graph beats Wikidata and GICS graphs (e.g. RGCN top-1 Sharpe 1.176 vs 1.011 for GICS).
- Graph type: static; multi-relational, directed
- Task: ranking (stock return ranking)
- Data: S&P 500 prices (public via standard sources); 452 tickers "with full trading records". Public.
- Code available? not stated
- What it leaves open: The universe is survivor-filtered (only tickers with full trading records). The graph is built from parametric knowledge at the time of querying, so edges may encode post-2023 information even though returns are out of sample. Only one LLM is used, and confidence thresholds are not varied.

##### zhang2026llm — LLM-Enhanced Dynamic Financial Knowledge Graphs for Cross-Entity Signal Propagation and Alpha Discovery
- Citation: Zhang, L., *LLM-Enhanced Dynamic Financial Knowledge Graphs for Cross-Entity Signal Propagation and Alpha Discovery*, arXiv:2607.10932, 2026.
- Link: https://arxiv.org/abs/2607.10932
- First public: v1 12 Jul 2026; Venue: preprint (stat.AP); Peer-reviewed: no
- Verification: [ABS]
- Summary: An LLM turns documents into structured "economic state-change" events and explicit/implicit firm relations, forming a dynamic knowledge graph. Events propagate more strongly within dynamically detected communities than across them. The paper defines two signals, Community Information Surprise and Propagated Information Surprise. Evidence comes from controlled simulations, including one calibrated to the Russell 1000.
- Graph type: dynamic; knowledge graph (heterogeneous: events + firms); communities
- Task: other (signal construction / propagation); community detection
- Data: simulations only (per abstract)
- Code available? not stated
- What it leaves open: There is no real-data evidence in the abstract. It is the companion to yang2026llm, which supplies the real measured graph but no dynamics. A real-data, point-in-time test is missing.

##### li2026finkario — FinKario: Event-Enhanced Automated Construction of Financial Knowledge Graph
- Citation: Li, X., Sun, P., Zhou, W., et al., *FinKario: Event-Enhanced Automated Construction of Financial Knowledge Graph*, ACL 2026 (Long), pp. 9826–9845.
- Link: https://aclanthology.org/2026.acl-long.446.pdf
- First public: ACL 2026 (July 2026); any earlier arXiv version unknown; Venue: ACL 2026; Peer-reviewed: yes
- Verification: [ABS] (first page / abstract of the PDF)
- Summary: Builds a knowledge graph from equity research reports using template-guided LLM prompts: 305k entities, ~210k triples, 19 relation types, with attribute and event subgraphs per stock. A two-stage graph RAG (FinKario-RAG) is used for trend prediction. A long-only backtest on Chinese equities shows ~18% accuracy gains over financial LLMs.
- Graph type: dynamic knowledge graph; heterogeneous
- Task: classification (trend) via retrieval
- Data: Chinese equity research reports. Public status unclear (broker reports usually are not public).
- Code available? https://github.com/Jackson906E/FinKario
- What it leaves open: Chinese market only. No look-ahead or knowledge-cutoff controls are mentioned in the abstract. Graph quality is not separated from RAG/LLM quality.

##### A.2 (b) Explainable temporal / dynamic graph models

##### chen2026sttgexplainer — ST-TGExplainer: Disentangling Stability and Transition Patterns for Temporal GNN Interpretability
- Citation: Chen, H., Zheng, X., Jiao, P., et al. (incl. Pan, S.), *ST-TGExplainer*, arXiv:2605.19822; PMLR 306 (ICML 2026).
- Link: https://arxiv.org/abs/2605.19822 (FT: https://arxiv.org/html/2605.19822); PMLR page seen: https://proceedings.mlr.press/v306/chen26gx.html (not opened)
- First public: v1 19 May 2026; Venue: ICML 2026 (PMLR 306 header in the HTML); Peer-reviewed: yes (per header)
- Verification: [FT]
- Summary: A self-explaining temporal GNN that separates *stability* patterns (repeated past interactions) from *transition* patterns (first-time interactions), trained with a disentangled information-bottleneck objective. Evaluated with ACC-AUC and CoDy-style necessity/sufficiency metrics.
- Graph type: temporal (continuous-time event graph); homogeneous
- Task: link prediction + explanation
- Data: Wikipedia, Reddit, UCI, Enron, USLegis, Can.Parl. **No financial data.**
- Code available? https://github.com/hjchen-hdu/ST-TGExplainer
- What it leaves open: Only one-hop histories are used (stated limitation). The method has never been applied to company/stock event graphs, where the split between stable relations and new ones is economically natural (an existing supplier link vs a new contract).

##### sussek2026explaining — Explaining Temporal Graph Predictions With Shapley Values
- Citation: Sussek, L.-M., Heindorf, S., *Explaining Temporal Graph Predictions With Shapley Values*, arXiv:2604.24078, 2026.
- Link: https://arxiv.org/abs/2604.24078 (FT: https://arxiv.org/html/2604.24078)
- First public: v1 27 Apr 2026; Venue: preprint; Peer-reviewed: no
- Verification: [FT]
- Summary: Two model-agnostic explainers for TGNNs: an event-level KernelSHAP explainer and a feature-level Owen-value decomposition. They beat T-GNNExplainer and TempME. The feature-level explainer *found a bug*: TGAT's handling of non-existent events leaks the absolute timestamp.
- Graph type: temporal event graph
- Task: explanation of link prediction
- Data: MOOC, Wikipedia, Reddit plus a synthetic set. **No financial data.**
- Code available? https://github.com/ds-jrg/TGNNSHAP
- What it leaves open: The feature explainer is slow (~50 s per event) and assumes events are independent. The authors do not frame the timestamp-leak finding as a general audit tool, yet that is exactly the kind of leakage that stock-GNN evaluations suffer from.

##### lu2026training — Training-Free Counterfactual Explanation for Temporal Graph Model Inference (TemGX)
- Citation: Lu, M., Che, H., Fan, Y., et al. (incl. Wu, Y.), *Training-Free Counterfactual Explanation for Temporal Graph Model Inference*, ICLR 2026.
- Link: https://mlanthology.org/iclr/2026/lu2026iclr-trainingfree/ (OpenReview: https://openreview.net/forum?id=NqtYz3A8tQ)
- First public: ICLR 2026 (OpenReview submission date unknown); Venue: ICLR 2026; Peer-reviewed: yes
- Verification: [ABS]
- Summary: A post-hoc, retraining-free explainer that finds the temporal subgraphs (and how they evolve) responsible for an output. It extends influence maximisation with structural influence and time decay, and offers fast algorithms with guarantees.
- Graph type: temporal
- Task: explanation (counterfactual)
- Data: unknown (abstract only)
- Code available? not stated
- What it leaves open: The influence-maximisation-with-decay framing maps directly onto shock-propagation questions in finance, but no financial application is reported in the abstract.

##### ding2026trace — TRACE: Temporal Rule-Anchored Chain-of-Evidence on Knowledge Graphs for Interpretable Stock Movement Prediction
- Citation: Ding, Q., Shi, H., Castejón Lozano, L., et al. (incl. Liu, B.), *TRACE*, arXiv:2603.12500, 2026.
- Link: https://arxiv.org/abs/2603.12500
- First public: v1 12 Mar 2026; Venue: preprint; Peer-reviewed: no
- Verification: [ABS]
- Summary: Rule-constrained multi-hop knowledge-graph search plus LLM decisions produce readable evidence chains linking news and graph paths to UP/DOWN verdicts. On an S&P 500 benchmark it reaches 55.1% accuracy and 60.8% F1, beating the best graph baseline on recall and F1.
- Graph type: temporal knowledge graph; heterogeneous
- Task: classification + explanation
- Data: S&P 500 benchmark (source not specified in abstract)
- Code available? not stated
- What it leaves open: The explanations are produced by the pipeline itself and are not tested for faithfulness, sufficiency or analyst usefulness. That is the "narrative bias" kong2026evaluating warns about.

##### A.3 (c) Geopolitical / tariff / sanctions shocks

##### barbiero2026trade — Trade Wars on the Income Statement: The Effects of Tariffs on Firms
- Citation: Barbiero, O., Sheremirov, V., Silva, A., Stein, H., *Trade Wars on the Income Statement*, Federal Reserve Bank of Boston working paper (update 28 May 2026).
- Link: https://asilvub.github.io/assets/papers/BSSS_2026_update.pdf
- First public: 2026 update dated 28 May 2026 (an earlier 2025 Boston Fed conference version appears in search snippets); Venue: working paper; Peer-reviewed: no
- Verification: [FT]
- Summary: Builds firm-level exposure to the 2018 and 2025 US tariffs via three channels: direct import costs (Panjiva), costs passed on by suppliers through the **FactSet supplier–customer network**, and foreign retaliation. Exposures are validated with **one-day stock returns around 13 tariff announcement dates in 2025**. In 2025, import and supplier exposure each cut sales and costs by ~1 pp, with no margin or capex effect.
- Graph type: static supplier–customer network (one hop)
- Task: other (econometric exposure measurement, local projections, event study)
- Data: Panjiva, Compustat, CRSP, FactSet and NL Analytics are licensed. Tariff rates are public. Not public overall.
- Code available? not stated
- What it leaves open: Propagation is limited to one supplier hop on proprietary FactSet data. Multi-hop propagation, public-data networks and European firms are not covered.

##### mukhia2026structural — Structural Dynamics of G5 Stock Markets During Exogenous Shocks: A Random Matrix Theory-Based Complexity Gap Approach
- Citation: Mukhia, K., Ansari, I., Nurujjaman, M., *Structural Dynamics of G5 Stock Markets During Exogenous Shocks*, arXiv:2604.19107, 2026.
- Link: https://arxiv.org/abs/2604.19107
- First public: v1 21 Apr 2026; Venue: preprint (q-fin.ST); Peer-reviewed: no
- Verification: [ABS]
- Summary: Defines an RMT "complexity gap": normalised largest eigenvalue versus mean pairwise correlation. Around shocks it follows three phases (positive, then near zero, then a non-monotone recovery with a false early recovery). Shocks studied include the **2025 US tariff event** and COVID-19. A low gap predicts higher future portfolio volatility.
- Graph type: correlation-matrix (implicit complete weighted graph); static windows
- Task: other (regime indicator / risk forecasting)
- Data: G5 index-constituent returns (exact sources not in abstract)
- Code available? not stated
- What it leaves open: The analysis is market-level and spectral only. It does not say *which* firms or edges carry the tariff shock, and has no firm-relation (supply-chain/text) layer.

##### beyer2026trade — Trade Policy Shocks and Corporate Valuations: Disentangling Trade and Uncertainty Channels
- Citation: Beyer, R. C. M., Tong, H., Zhou, X., *Trade Policy Shocks and Corporate Valuations*, IMF Working Paper 2026/070.
- Link: https://ideas.repec.org/p/imf/imfwpa/2026-070.html
- First public: 2026; Venue: IMF WP; Peer-reviewed: no
- Verification: [ABS]
- Summary: A firm-level event study of **global** equities around the 2025 US tariff shocks (2 April, the pause, later deals). It separates a trade-exposure channel from an uncertainty-sensitivity channel. Together they explain ~20% of the post-2 April decline and ~10% of the rebound among tradable firms.
- Graph type: none (no network)
- Task: other (event study)
- Data: not specified in abstract
- Code available? not stated
- What it leaves open: Roughly 80% of the decline is unexplained, and network or supply-chain exposure is not modelled. That leaves an obvious place to test whether multi-hop network exposure adds explanatory power.

##### arata2026predicting — Predicting Shock Propagation and Uncovering Heterogeneity with Graph Neural Networks
- Citation: Arata, Y., *Predicting Shock Propagation and Uncovering Heterogeneity with Graph Neural Networks*, RIETI Discussion Paper 26-E-045, 2026.
- Link: https://www.rieti.go.jp/en/publications/summary/26050019.html
- First public: May 2026; Venue: discussion paper; Peer-reviewed: no
- Verification: [ABS] (full PDF blocked)
- Summary: A GNN on an inter-firm transaction network predicts firm growth after a large shock, with the global financial crisis export collapse as the case. Analysing the trained model identifies firms and links that matter for propagation, and network structure significantly improves prediction.
- Graph type: static inter-firm transaction network; directed (presumably)
- Task: regression (firm growth) + post-hoc importance
- Data: "firm-level data" (source not stated in abstract; likely proprietary Japanese data — unverified)
- Code available? not stated
- What it leaves open: The targets are real outcomes (growth), not stock prices. Explanation is ad hoc, and no temporal-GNN explainer is used. It has not been applied to a 2025 tariff or sanctions shock.

##### A.4 (d) Evaluation rigour (leakage, look-ahead, survivorship)

##### kong2026evaluating — Evaluating LLMs in Finance Requires Explicit Bias Consideration
- Citation: Kong, Y., Lee, H., Hwang, Y., et al. (incl. Lopez-Lira, A.; Zohren, S.), *Evaluating LLMs in Finance Requires Explicit Bias Consideration*, arXiv:2602.14233, 2026.
- Link: https://arxiv.org/abs/2602.14233 (FT: https://arxiv.org/html/2602.14233)
- First public: v1 15 Feb 2026; Venue: preprint; Peer-reviewed: no
- Verification: [FT]
- Summary: Names five biases: look-ahead, survivorship, narrative, objective, and cost. A review of 164 main-conference papers (2023–2025; ICML/ICLR/NeurIPS/KDD/AAAI/IJCAI/CIKM/ACL/EMNLP/NAACL/ICAIF) finds that only 26.8% acknowledge look-ahead bias and **1.2% mention survivorship**. The paper proposes a pass/fail "Structural Validity Framework" with five components: temporal sanitation, dynamic universe, rationale robustness, calibration, and realistic costs.
- Graph type: n/a
- Task: other (meta-evaluation)
- Data: literature corpus + user study (n = 50 complete)
- Code available? https://github.com/Eleanorkong/Awesome-Financial-LLM-Bias-Mitigation
- What it leaves open: Graph and relational models are **not discussed**. No equivalent audit exists for stock-graph / GNN papers: point-in-time graphs, survivor-only universes, full-sample correlation edges.

##### gao2025detecting — Detecting Lookahead Bias in LLM Forecasts
- Citation: Gao, Z., Jiang, W., Yan, Y., *Detecting Lookahead Bias in LLM Forecasts*, arXiv:2512.23847.
- Link: https://arxiv.org/abs/2512.23847
- First public: v1 29 Dec 2025 (v2 12 Jun 2026); Venue: preprint; Peer-reviewed: no
- Verification: [ABS]
- Summary: A "Lookahead Propensity" (LAP) score comes from date-only recall prompts per firm-date. A positive LAP × forecast interaction in accuracy regressions signals contamination. It is shown on news-based return prediction and on earnings-call capex prediction, and the effect vanishes after the model's cutoff.
- Graph type: n/a
- Task: other (statistical test)
- Data: news headlines, earnings calls (sources not in abstract)
- Code available? not stated
- What it leaves open: The test applies to *forecasts*. It has not been extended to *LLM-generated edges* (relations) such as those in zhou2025zero or huang2026cross.

##### benhenda2026look — Look-Ahead-Bench: a Standardized Benchmark of Look-ahead Bias in Point-in-Time LLMs for Finance
- Citation: Benhenda, M., *Look-Ahead-Bench*, arXiv:2601.13770, 2026.
- Link: https://arxiv.org/abs/2601.13770
- First public: v1 20 Jan 2026; Venue: preprint; Peer-reviewed: no
- Verification: [ABS]
- Summary: Measures alpha decay across temporally distinct regimes against quantitative baselines to separate memorisation from skill. It compares Llama 3.1 (8B/70B) and DeepSeek 3.2 with "point-in-time" Pitinf models; the standard LLMs show significant look-ahead bias.
- Graph type: n/a
- Task: other (benchmark)
- Data: not detailed in abstract
- Code available? https://github.com/benstaf/lookaheadbench
- What it leaves open: Workflows only, with no relational or graph-building tasks. The single author gives no affiliation; whether he is linked to the Pitinf vendor (PiT-Inference) is [UNVERIFIED].

##### li2025can — Can LLM-based Financial Investing Strategies Outperform the Market in Long Run? (FINSABER)
- Citation: Li, W. W., Kim, H., Cucuringu, M., Ma, T., *Can LLM-based Financial Investing Strategies Outperform the Market in Long Run?*, arXiv:2505.07078; KDD 2026 Datasets & Benchmarks (Oral).
- Link: https://arxiv.org/abs/2505.07078
- First public: v1 11 May 2025 (v6 26 Jun 2026); Venue: KDD 2026 D&B track; Peer-reviewed: yes
- Verification: [ABS]
- Summary: FINSABER backtests LLM timing strategies over two decades and 100+ symbols. The previously reported LLM advantages largely vanish once survivorship and data-snooping biases from short windows and small universes are removed. The strategies are too conservative in bull markets and too aggressive in bear markets.
- Graph type: n/a
- Task: other (benchmark / backtesting framework)
- Data: US equities (sources not in abstract)
- Code available? not linked on the abstract page
- What it leaves open: Single-asset timing strategies only. No relational or graph strategies are re-evaluated, so a "FINSABER for stock-GNNs" does not exist.

##### wade2026do — Do Better Volatility Forecasts Lead to Better Portfolios? Evidence from Graph Neural Networks
- Citation: Wade, R., *Do Better Volatility Forecasts Lead to Better Portfolios? Evidence from Graph Neural Networks*, arXiv:2605.19278, 2026.
- Link: https://arxiv.org/abs/2605.19278
- First public: v1 19 May 2026 (v2 20 May 2026); Venue: preprint; Peer-reviewed: no
- Verification: [ABS]
- Summary: GraphSAGE models are built on correlation, sector and Granger graphs (± macro regime features), with HAR and LSTM baselines, for 465 S&P 500 stocks (2015–2025). The best-RMSE, best-ranking and best-Sharpe models are all different, so graph choice "helps only when the portfolio rule can use the cross-sectional structure".
- Graph type: static/rolling; correlation, sector and Granger-causal (directed) graphs
- Task: regression (realised volatility) + portfolio
- Data: S&P 500 (public prices)
- Code available? not stated
- What it leaves open: The result suggests that graph-choice conclusions depend on the metric. That supports a systematic "graph-construction multiverse" study, which the paper does not run.

##### A.5 (e) Higher-order structure (hypergraphs)

##### park2025dynamic — Dynamic Higher-Order Relations and Event-Driven Temporal Modeling for Stock Price Forecasting
- Citation: Park, K., Hong, S., Jeon, J.-J., *Dynamic Higher-Order Relations and Event-Driven Temporal Modeling for Stock Price Forecasting*, IJCAI 2025.
- Link: https://www.ijcai.org/proceedings/2025/0673.pdf
- First public: IJCAI 2025 proceedings (2025); Venue: IJCAI 2025; Peer-reviewed: yes
- Verification: [FT]
- Summary: A Hawkes-process layer models self-exciting extreme co-movements. A predefined hypergraph (industry + historical correlation) is combined with a dynamic low-rank learned incidence matrix. Evaluated on 463 S&P 500 stocks over 10 rolling phases (24/4/8 months), using the ESTIMATE pipeline.
- Graph type: dynamic hypergraph
- Task: regression / forecasting + portfolio
- Data: S&P 500 OHLCV (public)
- Code available? https://github.com/kijeong22/ijcai2025-spf
- What it leaves open: The learned hyperedges are not compared with real groups (supply-chain clusters, consortia, index or ETF baskets). It does not test whether higher-order structure beats its pairwise clique projection.

##### tan2025magnet — MaGNet: A Mamba Dual-Hypergraph Network for Stock Prediction via Temporal-Causal and Global Relational Learning
- Citation: Tan, P., Shi, C., Tu, D., Xie, L., *MaGNet*, arXiv:2511.00085, 2025.
- Link: https://arxiv.org/abs/2511.00085
- First public: v1 29 Oct 2025; Venue: preprint; Peer-reviewed: no
- Verification: [ABS]
- Summary: Bidirectional Mamba + MoE + 2D spatio-temporal attention, combined with a Temporal-Causal Hypergraph and a Global Probabilistic Hypergraph (soft hyperedges, JSD weighting). It outperforms SOTA on six stock indices.
- Graph type: dynamic learned hypergraphs (two)
- Task: classification (trend) + investment simulation
- Data: six major indices (details not in abstract)
- Code available? https://github.com/PeilinTime/MaGNet
- What it leaves open: Typical architecture-stacking paper: the learned hyperedges are not analysed for meaning, and it is unclear which component matters.

##### qiu2025hermes — Hermes: A Multi-Scale Spatial-Temporal Hypergraph Network for Stock Time Series Forecasting
- Citation: Qiu, X., Yang, L., Xu, X., et al. (incl. Jensen, C. S.; Hu, J.), *Hermes*, arXiv:2509.23668, 2025.
- Link: https://arxiv.org/abs/2509.23668v2 (v1 HTML title: "Multi-Scale Spatial-Temporal Hypergraph Network with Lead-Lag Structures for Stock Time Series Forecasting")
- First public: Sept 2025 (inferred from arXiv ID; the abs fetch did not return the version history); Venue: unknown; Peer-reviewed: unknown
- Verification: [ABS]
- Summary: Industry hyperedges with hyperedge-level moving aggregation capture *inter-industry lead-lag*, and multi-scale edge-to-edge fusion is added. It beats SOTA on several datasets, including NASDAQ/NYSE 2013–2017.
- Graph type: temporal hypergraph (industry-defined hyperedges; directed lead-lag between hyperedges)
- Task: regression (forecasting)
- Data: NASDAQ/NYSE (Feng et al. 2019) and others
- Code available? not stated
- What it leaves open: Hyperedges are fixed industry labels. It uses the survivor-only NASDAQ/NYSE 2013–2017 benchmark, and the lead-lag between hyperedges is not validated economically.

##### harit2025from — From News to Returns: A Granger-Causal Hypergraph Transformer on the Sphere
- Citation: Harit, A., Sun, Z., Yu, J., *From News to Returns: A Granger-Causal Hypergraph Transformer on the Sphere*, arXiv:2510.04357 (prepared for ICAIF 2025).
- Link: https://arxiv.org/abs/2510.04357
- First public: v1 5 Oct 2025; Venue: ICAIF 2025 (per comments); Peer-reviewed: likely (per comments), not verified
- Verification: [ABS]
- Summary: Granger-causal hyperedges from news/sentiment to returns, embedded on a hypersphere, with angular attention masks. S&P 500 2018–2023, including COVID. The authors claim traceable paths from macro events to stock responses.
- Graph type: directed hypergraph (Granger-causal); Riemannian embedding
- Task: regression + regime classification + ranking
- Data: S&P 500 + news (sources not in abstract)
- Code available? not stated
- What it leaves open: The interpretability claim is not tested for faithfulness. The causal hyperedges are not validated against known relations.

##### A.6 (f) Is the learned graph meaningful?

##### niu2026gapnet — GAPNet: Plug-in Jointly Learning Task-Specific Graph for Dynamic Stock Relation
- Citation: Niu, Y., Lu, L., Jin, C., Dong, R., *GAPNet*, arXiv:2602.00888, 2026.
- Link: https://arxiv.org/abs/2602.00888 (HTML opened, truncated at §3.2: https://arxiv.org/html/2602.00888)
- First public: v1 31 Jan 2026; Venue: preprint; Peer-reviewed: no
- Verification: [ABS] (partial HTML read)
- Summary: A plug-in module that learns and rewires the stock graph jointly with the task. Its Spatial Perception Layer handles short-term co-movement and its Temporal Perception Layer handles long-term dependencies. It is attached to RT-GCN (pairwise) and CI-STHPAN (hypergraph) backbones on NASDAQ/NYSE 2013–2017, reaching Sharpe up to 2.20.
- Graph type: dynamic learned graph or hypergraph (initialised from industry/Wiki relations)
- Task: ranking / portfolio
- Data: NASDAQ, NYSE (Feng et al. 2019; public)
- Code available? not stated
- What it leaves open: In the text I could read, there is **no analysis of the learned adjacency** (no visualisation and no comparison with industry, Wiki or supply relations). Claims that it removes "harmful biases" are not evidenced there. The benchmark is survivor-only and from 2013–2017.

##### luo2025ftscommdetector — FTSCommDetector: Discovering Behavioral Communities through Temporal Synchronization
- Citation: Luo, T., Zhang, X., Song, D., *FTSCommDetector*, arXiv:2510.00014.
- Link: https://arxiv.org/abs/2510.00014
- First public: v1 17 Sep 2025 (v3 18 Jan 2026); Venue: preprint; Peer-reviewed: no
- Verification: [ABS]
- Summary: Community detection on multivariate time series via a "Temporal Coherence Architecture" that captures synchronisation and desynchronisation, with Normalised Temporal Profiles for evaluation. Gains of 3.5–11.1% are reported on SP100/500/1000 and Nikkei 225.
- Graph type: learned/dynamic similarity graph; communities (implicit higher-order groups)
- Task: community detection (structure learning)
- Data: S&P indices, Nikkei 225 (public prices)
- Code available? not stated
- What it leaves open: The abstract does not report any comparison of the discovered communities with GICS or with text-derived relations. Whether "behavioural communities" correspond to economic links remains unanswered.

##### niu2024evaluating — Evaluating Financial Relational Graphs: Interpretation Before Prediction [BACKGROUND]
- Citation: Niu, Y., Lu, L., Dolphin, R., Poti, V., Dong, R., *Evaluating Financial Relational Graphs: Interpretation Before Prediction*, arXiv:2410.07216, 2024.
- Link: https://arxiv.org/abs/2410.07216
- First public: v1 28 Sep 2024; Venue: preprint; Peer-reviewed: unknown
- Verification: [ABS]
- Summary: Introduces SPNews, a dataset for dynamic S&P 500 relation graphs. Argues that stock graphs should be evaluated *before* prediction by how well they explain historical financial phenomena, rather than by downstream accuracy.
- Graph type: dynamic news-based graphs
- Task: other (graph evaluation)
- Data: SPNews (public; GitHub stated, URL not on abstract page)
- Code available? stated as public on GitHub (URL not shown)
- What it leaves open: Labelled background (2024). Notably, the **same UCD group's 2026 papers** (niu2026gapnet, niu2026relational) do not apply this interpretation-first evaluation to their own learned graphs. That is a visible, citable gap.

##### A.7 (g) European markets / cross-market transfer

##### liu2026bipartite — A Bipartite Graph Approach to U.S.-China Cross-Market Return Forecasting
- Citation: Liu, J., Grith, M., Dong, X., Cucuringu, M., *A Bipartite Graph Approach to U.S.-China Cross-Market Return Forecasting*, arXiv:2603.10559, 2026.
- Link: https://arxiv.org/abs/2603.10559
- First public: v1 11 Mar 2026 (v2 13 Apr 2026); Venue: preprint; Peer-reviewed: no
- Verification: [ABS]
- Summary: Builds a directed bipartite graph of time-ordered predictive links (rolling hypothesis tests) from US stocks to Chinese stocks, exploiting the non-overlapping trading hours. The sparse graph is used as interpretable feature selection for regularised/ensemble models. US→China predictability is strong and China→US is weak.
- Graph type: directed bipartite, rolling (temporal)
- Task: regression (cross-market return forecasting)
- Data: US and China equities (sources not in abstract)
- Code available? not stated
- What it leaves open: US–China only. Partly overlapping time zones (US↔Europe) and European targets are untested, and the edges are purely statistical (no economic or text links).

##### wang2026crossalpha — CrossAlpha: An Annual-Report Benchmark for Cross-Market Factor Research (with LLM Agents)
- Citation: Wang, Q., Tong, Z., Chen, N., Wu, Z., He, B., *CrossAlpha*, arXiv:2605.29286, 2026.
- Link: https://arxiv.org/abs/2605.29286
- First public: v1 28 May 2026 (v3 12 Jun 2026); Venue: preprint (cs.IR); Peer-reviewed: no
- Verification: [ABS]
- Summary: Standardises annual-report business descriptions into ten categories and builds PCA-whitened *cross-market* firm-pair scores (~19M directed pairs, ~3,600 firms, ~10,700 reports). These are evaluated against 11 years of daily prices with realistic cross-market timing. In US→Japan, disclosure peers beat domestic-text, industry-code and return-correlation peers (ICIR 0.39 vs 0.07–0.18).
- Graph type: directed bipartite cross-market similarity graph (dense scores)
- Task: ranking / factor evaluation
- Data: US, Japan, Taiwan, Korea, Hong Kong filings and prices. **No European market.** Benchmark is stated as open-sourced.
- Code available? stated as open-sourced; no URL on abstract page
- What it leaves open: Europe is absent (no ESEF/Euronext). Pair scores are similarity-based, not typed relations (supplier/competitor). No graph learning is applied.

##### paredesramos2025a3tgcn — A3T-GCN for FTSE100 Components Price Forecasting
- Citation: Paredes Ramos, A. L., *A3T-GCN for FTSE100 Components Price Forecasting*, arXiv:2511.21873.
- Link: https://arxiv.org/abs/2511.21873 (HTML: https://arxiv.org/html/2511.21873v1)
- First public: Nov 2025 (inferred from arXiv ID; no date on page); Venue: unpublished paper; Peer-reviewed: no
- Verification: [ABS]
- Summary: An A3T-GCN on 79 FTSE100 firms (2007–2024), with graphs from sector plus return correlation or fundamental-ratio correlation. Annualised log-return features and short input windows help most.
- Graph type: static correlation/sector graph; temporal model
- Task: regression (price forecasting)
- Data: FTSE100 prices (public)
- Code available? not stated
- What it leaves open: This is the *only* European stock-GNN paper I could open (UK, not Euronext/EU). The fixed set of 79 surviving constituents raises survivorship concerns, and the method is weak. Euronext, AEX and STOXX 600 graph work appears essentially absent in 2025–2026; see C.

##### A.8 Peripheral papers also opened (compact; not used as primary evidence)
| Key | Title | Link | First public / venue | Ver. | Sub-theme / note |
|---|---|---|---|---|---|
| fukada2026snippet | Snippet-Driven Supply Chain Discovery with LLMs: Scaling Visibility in China | https://arxiv.org/abs/2605.27845 | v1 27 May 2026; DOI suggests IEEE CIFEr 2026 | [ABS] | (a) Web-search snippets as an LLM extraction source. Full-text chunks find 19.8× more relations at 251× the tokens, a direct construction-choice trade-off. Graph covers 7.2× more listed firms than CSMAR. |
| sun2025intercorprel | InterCorpRel-LLM: Enhancing Financial Relational Understanding with Graph-Language Models | https://arxiv.org/abs/2510.09735 | v1 10 Oct 2025; preprint | [ABS] | (a) GNN+LLM trained on proprietary FactSet supply chain data. Supply-relation F1 0.854 vs GPT-5 0.229, i.e. zero-shot LLMs are poor at supplier detection. |
| lee2025knowledge | Knowledge Graph Construction for Stock Markets with LLM-Based Explainable Reasoning | https://arxiv.org/abs/2601.11528 | submitted 16 Nov 2025; CIKM 2025 workshop | [ABS] | (a)/(b) KG schema for Korean listed firms; case studies only. |
| arun2025finreflectkg | FinReflectKG: Agentic Construction and Evaluation of Financial Knowledge Graphs | https://arxiv.org/abs/2508.17906 | v1 25 Aug 2025; ACM DOI (ICAIF 2025 per yang2026llm) | [ABS] | (a) Open-source KG from S&P 100 10-Ks. Compares single-pass, multi-pass and reflection-agent extraction (a construction-choice comparison, but no downstream test). |
| li2025grexplainer | GRExplainer: A Universal Explanation Method for Temporal Graph Neural Networks | https://arxiv.org/abs/2512.22772 | v1 28 Dec 2025; preprint | [ABS] | (b) Works for both snapshot and event TGNNs; six non-financial datasets. |
| li2025structure | Structure Over Signal: A Globalized Approach to Multi-relational GNNs for Stock Prediction (OmniGNN) | https://arxiv.org/abs/2510.10775 | v1 12 Oct 2025; preprint | [ABS] | (c) A sector "global hub" node lets macro shocks spread without deep hops; robust in COVID. |
| tan2025h3m | H3M-SSMoEs: Hypergraph-based Multimodal Learning with LLM Reasoning and Style-Structured MoE | https://arxiv.org/abs/2510.25091 | v1 29 Oct 2025; preprint | [ABS] | (e) Local and global multimodal hypergraph; code: github.com/PeilinTime/H3M-SSMoEs |
| duan2025factorgcl | FactorGCL: A Hypergraph-Based Factor Model with Temporal Residual Contrastive Learning | https://mlanthology.org/aaai/2025/duan2025aaai-factorgcl | AAAI 2025 | [ABS] | (e) Hypergraph hidden factors on residuals after prior factors. |
| dukkipati2025predictive | Predictive AI with External Knowledge Infusion: Datasets and Benchmarks for Stock Markets | https://arxiv.org/abs/2504.20058 | v1 14 Apr 2025 (v2 11 Feb 2026); preprint | [ABS] | (a) Temporal KG datasets; relations modelled as Hawkes events. |
| dacosta2026graphnetz | GraphNetz: Statistical Benchmarking of GNNs with Paired Tests and Rank Aggregation | https://arxiv.org/abs/2605.09099 | v1 9 May 2026; preprint | [ABS] | (d) Multi-seed CIs, Holm-corrected paired tests, Nemenyi; generic GNNs. Shows that canonical encoders are statistically tied. |
| hu2025fintsb | FinTSB: A Comprehensive and Practical Benchmark for Financial Time Series Forecasting | https://arxiv.org/abs/2502.18834 | v1 26 Feb 2025 (v3 Jun 2026); DOI → Frontiers of Computer Science 2026 | [ABS] | (d) Standardised protocol with transaction fees; code github.com/TongjiFinLab/FinTSB |
| zhang2026review | A Review of LLMs for Stock Price Forecasting from a Hedge-Fund Perspective | https://arxiv.org/abs/2605.05211 | v1 10 Apr 2026; preprint | [ABS] | (d) Lists leakage, horizon design and illiquidity pitfalls. |
| stoll2025graphbench | GraphBench: Next-generation graph learning benchmarking | https://arxiv.org/abs/2512.04475 | v1 4 Dec 2025 (v5 May 2026) | [ABS] | (d) Generic graph benchmark with OOD splits; no finance. |
| fanshawe2026forecasting | Forecasting Equity Correlations with Hybrid Transformer GNN | https://arxiv.org/abs/2601.04602 | v1 8 Jan 2026; preprint | [ABS] | (f)/(d) Forecasts the correlation graph itself (10-day, Fisher-z), S&P 500 2019–2024. |
| bruck2026graph | Graph structure learning for stable processes | https://arxiv.org/abs/2601.06264 | v1 9 Jan 2026; preprint (stat.ME) | [ABS] | (f) Statistically principled sparse graph of jump dependence in stock returns. A possible "learned graph" baseline with known semantics. |
| wang2025modeling | Modeling Bank Systemic Risk of Emerging Markets under Geopolitical Shocks (BRIDGES) | https://arxiv.org/abs/2512.20515 | v1 23 Dec 2025 (v2 Apr 2026) | [ABS] | (c) DTW bank network, temporal GNN anomaly detection and agent-based geopolitical shocks. Banks, not stocks. |
| ortiz2025geopolitics | Geopolitics, Geoeconomics, and Sovereign Risk: Different Shocks, Different Channels | https://arxiv.org/abs/2510.12416 | v1 14 Oct 2025 (v7 Mar 2026) | [ABS] | (c) Sovereign CDS, 42 countries 2018–2025; Shapley–Taylor channel decomposition; gravity-style decay from conflict zone. |
| bonnet2026throwing | Throwing sand on the chips: unintended effects of U.S. export controls on the semiconductor supply chain | https://publications.jrc.ec.europa.eu/repository/handle/JRC147129 | 22 May 2026; JRC (EU) | [ABS] | (c) After US controls, EU, Japan and Singapore equipment exports rose: shocks *redirect* rather than only harm, i.e. signed (positive) spillovers to competitors. |
| saizarurtueta2026cross | Cross Regional Revenue Exposure and Abnormal Returns Around U.S. Tariff Announcements (MSc dissertation, Católica Lisbon) | https://repositorio.ucp.pt/bitstreams/0e87aa78-7a67-4750-a5c1-bd1f26584f8c/download | Feb 2026; MSc thesis | [FT] | (c)/(g) Event study across 4 events incl. the July 2025 US–EU deal (mean CAR −2.57%). No network methods; the author lists GVC centrality as future work. Shows an MSc-sized gap. |

---


---

## T0. Papers opened by the lead agent during early kill tests

##### izadifar2026institutional — Institutional Equity Holdings Prediction Using Node Affinities of Dynamic Graphs
- Citation: Izadifar, E., Rahmati, Z., *Institutional Equity Holdings Prediction Using Node Affinities of Dynamic Graphs*, arXiv:2607.12067, 2026.
- Link: https://arxiv.org/abs/2607.12067 (HTML read: https://arxiv.org/html/2607.12067)
- First public: arXiv v1 13 Jul 2026; Venue: preprint; Peer-reviewed: no
- Verification: [FT] (HTML; limitations section truncated in extraction)
- Summary: First benchmark of TGB-style temporal graph models on SEC 13F holdings, framed as node-affinity prediction (next-quarter portfolio weights) on a discrete-time bipartite manager–security graph (99 managers × 503 S&P 500 stocks, 48 quarters 2013–2025). NAVIS is best (NDCG 0.913) but an exponential moving average (0.888) beats every other dynamic-graph model.
- Graph type: temporal, bipartite (heterogeneous: manager/security), directed, weighted, unsigned.
- Task: node-affinity prediction (ranking) — a link-weight forecasting task.
- Data: SEC 13F (public). Prices not central.
- Code available? Yes: https://github.com/e-izdfr/portfolio-holdings-prediction
- What it leaves open: no stock–stock links (co-movement, common-ownership projections), no signed edges, small manager sample, strong persistence makes the task easy for heuristics; authors list crowding/herding/price pressure as future work. → The **one-mode projection (stock–stock common-ownership edges) and its link to future co-movement is untouched by ML**.

##### huang2026crossstock — Cross-Stock Predictability via LLM-Augmented Semantic Networks
- Citation: Huang, Y., Fan, Z., Hu, K., Ye, Y., *Cross-Stock Predictability via LLM-Augmented Semantic Networks*, arXiv:2604.19476, 2026.
- Link: https://arxiv.org/html/2604.19476
- First public: arXiv v1 2026-04-21 (v2 2026-04-26); Venue: preprint (q-fin.PM); Peer-reviewed: no
- Verification: [FT] (HTML; limitations truncated)
- Summary: Builds a candidate stock graph from 10-K risk-factor embeddings, then lets an LLM (DeepSeek) label each pair as competitor / supply chain / complementary / substitute / peer / unrelated, and uses the filtered relation graph to aggregate pairs-trading mean-reversion signals on S&P 500 (2011–2019). LLM-filtered network Sharpe 0.82 vs 0.74 baseline and 0.79 SIC.
- Graph type: annually rebuilt (time-varying), typed relations, undirected; trading signal effectively signed per pair.
- Task: relation extraction + cross-sectional signal (portfolio), not link prediction.
- Data: CRSP (paid) + 10-K text (public).
- Code available? not stated.
- What it leaves open: one LLM, no construction-sensitivity analysis, no transaction costs, no validation of LLM labels against ground truth; relation types are fixed-weight, not learned; does not predict when pair relations activate/deactivate.

##### anton2014connected — Connected Stocks [BACKGROUND]
- Citation: Antón, M., Polk, C., *Connected Stocks*, Journal of Finance 69(3), 2014.
- Link: https://personal.lse.ac.uk/polk/research/ConnectedStocks.pdf (seen in search; author PDF snippet)
- Verification: [ABS] (search-result abstract text only) — background only
- Summary: Common active mutual-fund ownership forecasts higher future pairwise abnormal return correlation; causal evidence via the 2003 fund scandal.
- Relevance: the economic premise for "common-ownership graph → future co-movement edges".


---

## Crowding map (Phase 2)

Counts are **2025–2026 papers seen** (opened plus seen in search/reference lists) per sub-zone. They are lower bounds: Scholar/DBLP were blocked and the search budget ran out in five sweeps. Verdicts compare *relative* density; "thin" means fewer than ~5 directly on-target 2025–2026 papers were found after deliberate searching.

| Zone | ≈ 2025–26 papers seen | Dominant venues / groups | Shared benchmarks / data | Typical evaluation weaknesses | Verdict |
| --- | ---: | --- | --- | --- | --- |
| **Graph/relational stock return prediction** (new GNN/transformer/hypergraph/Mamba/LLM architectures) (T5, T8e) | ~60 seen; est. 150–300 in total | arXiv cs.LG; AAAI/IJCAI/KDD/CIKM; IEEE Access/MDPI; Chinese groups (ECNU, Jilin, PKU, HKUST-GZ); UCD Dublin (critical line) | NASDAQ/NYSE 2013–17 (Feng 2019), CSI300/800 + Qlib Alpha158, S&P 500 via yfinance | no costs; single seed; survivor universes; graph leakage (full-sample correlations, present-day relations, post-cutoff LLM edges); learned graphs never audited | **CROWDED** (avoid) |
| Generic temporal link prediction methods/benchmarks (T2) | ~20 | ICLR; TGB group (McGill/Mila), TGM | TGB, TGB 2.0, TGB-Seq — **no equity/company dataset** | negative-sampling sensitivity; repeated-edge inflation | Crowded methods → use as tools |
| **Link prediction on company/stock graphs with public data + temporal splits** (T2, T4) | ~10 | workshops, arXiv, IJPR; FactSet-based | none shared; 13F node-affinity benchmark (Jul 2026) only | random splits; proprietary data; no persistence/EMA baseline | **THIN** |
| **Signed lead-lag / time-activated edges between stocks** (T1, T6) | ~15 on target | Cucuringu network (Oxford/UCLA; DeltaLag ICAIF'25); Milan-Bicocca and IISER (balance); ICLR'26 withdrawn MSc-derived paper | none; signed-GNN field uses Bitcoin-OTC/Wiki-RfA only | tiny universes (37); fixed lags/thresholds; edges unsigned or undirected; no edge-level validation; balance indices coincident | **THIN at intersection** (supervisor's core signal) |
| Structural balance in signed correlation networks (T1) | ~6 | physics.soc-ph/q-fin preprints | S&P 500, CSI | descriptive only; undirected, contemporaneous; balanced by construction | Moderate, descriptive |
| Generic dynamic-graph anomaly detection (T3) | ~25 | AAAI'26 (BAG), KDD'26 (ARES), many withdrawn ICLR'26 | BAG, social/transaction graphs | — | Crowded methods → tools |
| **Network early-warning / stress indicators on market graphs** (T3) | ~15 | single-author q-fin preprints; FRL, Frontiers, MDPI | 3–17 hand-picked crises; OFR FSI, VIX | no held-out tests; coincident not leading; survivorship; negative edges dropped | Moderate, weak protocols |
| **Anomalies in signed/directed edge events; event-typed weak labels** (T3) | 0–2 | — | — | — | **THIN / empty** |
| LLM-built financial KG → RAG/HGNN trend prediction (T4, T8a) | ≥ 10–12 | arXiv, ICAIF'25, ACL'26, finance workshops; one ICLR'26 rejection | FinReflectKG, FinDKG, FinKario | single LLM/prompt; LLM labels unvalidated; look-ahead untested | **CROWDED-ish** |
| **Temporal KG completion / future-relation prediction on company KGs** (T4) | 0 (2025–26); FinDKG 2024 only | — | FinReflectKG (17.5M timestamped triples, CC-BY-NC) unused for this | — | **THIN / empty** |
| **Construction-choice sensitivity & edge-level look-ahead of LLM company graphs** (T4, T8a/d) | 1–2 (yang2026llm partial) | — | — | — | **THIN** |
| Auditing learned stock graphs vs real relations (T5, T8f) | 2–3 (guo2026nystrom, NGAT, QuantBench indirectly) | UCD Dublin; Nyström paper | — | — | **THIN** |
| Econometric spillover / connectedness (TVP-VAR etc.) (T6) | many (journal-heavy) | Elsevier/MDPI econ | DY indices | not ML | Crowded (not ML) |
| **Externally validated firm-level causal/lead-lag graphs** (T6) | 0 found | — | no finance causal ground truth | circular validation; P&L as proxy | **THIN / empty** |
| Regime-dependent causal discovery methods (T6) | ~6 | ICML/ICLR 2026, EPFL, DLR/Runge, Bloomberg | synthetic, CausalRivers | never tested on market data | Active methods → tools |
| GNN default prediction on private enterprise/guarantee graphs (T7) | ~15 | Chinese/Korean groups; CIKM'26 DefaultGNN | private | random splits; gains within CIs | **CROWDED** (and data-blocked) |
| **Credit event → connected firms' equity reaction, public data, ML** (T7) | 0 direct | nearest: Brogaard&Chen 2026 (credit endpoint), Zelvyte 2026 (CDS only) | public: LoPucki BRD, 8-K 1.03, 17g-7 | — | **THIN / empty**, data-limited |
| Temporal-GNN explainers (T8b) | ~6 | ICML/ICLR 2026 | Wikipedia, Reddit, MOOC | never on financial graphs | Active methods, **empty in finance** |
| Tariff / geopolitical shock propagation through firm networks (T8c) | ~8 | Fed Boston, IMF, RIETI | FactSet/proprietary one-hop | no multi-hop, no public graph | Econ-heavy, **ML-thin** |
| European (AEX/Euronext/STOXX 600) stock-graph work (T8g) | ~1 (FTSE100) | — | — | survivor sets | **Very thin** |

**Crowded zones to avoid:** new architectures for return/movement prediction; LLM-KG → trend pipelines; GNN default scoring on private graphs; generic anomaly-detection or link-prediction method papers.

**Thin zones (thesis material):** (1) signed, directed, time-activated lead-lag edges as *prediction targets*; (2) link prediction on public company graphs with honest temporal evaluation and persistence baselines; (3) event-typed weak labels and signed edge-event anomalies; (4) temporal KG completion on public company KGs; (5) auditing learned / LLM-built graphs (meaning, construction sensitivity, look-ahead); (6) credit events → equity reactions of connected firms on public data; (7) explainers on financial temporal graphs; (8) Europe.

**Shared weakness across all zones:** almost nobody compares against *persistence / EMA / last-period* baselines, uses point-in-time (survivor-free) universes, or validates edges against external evidence. Any shortlisted topic adopts these as default protocol.

---

## Comparison table: closest works for the shortlisted directions

| Paper (bibkey) | First public · status | Graph | Task | Data public? | Code | What it leaves open |
| --- | --- | --- | --- | --- | --- | --- |
| krstev2025temporal (ICLR'26 submission, withdrawn) | 2025 · preprint/withdrawn | temporal, directed, unsigned | lead-lag edge detection as temporal LP | yes (37 assets) | not stated | scale, label definition, sign, persistence baselines, validation |
| zhou2025deltalag (DeltaLag) | 2025 · ICAIF 2025 | temporal, directed, per-pair lag, unsigned | return prediction via learned lead-lag | yes (US equities) | stated in paper (unchecked) | sign; edges never validated against economics; 2-year test |
| fanshawe2026forecasting | 2026 · preprint | signed, undirected, contemporaneous | correlation forecasting (GNN) | yes | not stated | sign-flip prediction not evaluated |
| hou2026inductive (IDP-DSN) | 2026 · preprint | dynamic signed | signed LP (+/−/none) | Bitcoin/Wiki | yes | never on market data |
| regier2026dynamic (HCIM) | 2026 · preprint | dynamic signed | signed LP | Bitcoin/Wiki | yes | calls for signed temporal benchmarks |
| dahake2026sectoral | 2026 · preprint | signed, undirected | structural balance (descriptive) | yes (S&P 500) | — | directed/lagged balance; prediction |
| acedo2026triadic | 2026 · preprint | signed correlation | stress index (triads) | yes | — | coincident, not leading; PSD "sign null" |
| izadifar2026institutional | 2026 · preprint | temporal bipartite manager–stock | node affinity (holdings) | yes (13F) | yes | stock–stock projection; co-movement; edge birth/death; signs |
| arun2025finreflectkg (FinReflectKG) | 2025 · ICAIF 2025 | temporal company KG (10-K) | KG construction / QA | yes (CC-BY-NC) | dataset | temporal KG completion never done |
| huang2026crossstock | 2026 · preprint | LLM-typed 10-K firm graph | pairs-trading signal | partly (CRSP) | not stated | label validation; construction sensitivity; costs; look-ahead |
| yang2026llm | 2026 · preprint | LLM latent edges, point-in-time | economic graphs for investing | partly | not stated | full construction multiverse; tiny audit (n = 30) |
| guo2026nystrom | 2026 · preprint | learned attention | cross-sectional prediction | yes | — | learned attention is not relation-like; systematic audit across models |
| wang2025quantbench (QuantBench) | 2025 · benchmark | various | benchmark | yes | yes | graphs don't consistently help |
| brogaard2026network | 2026 · working paper | production network, industry-level | CDS spread prediction | no (Markit, CRSP) | — | firm-level public graph; equity endpoint |
| zelvyte2026multiplex | 2026 · preprint | multiplex Hawkes (99 firms) | credit contagion | no (CDS) | — | equity layer; public marks |
| kim2026defaultgnn | 2026 · CIKM 2026 | buyer–seller transaction graph | default prediction | no (Korean tax invoices) | yes | listed firms; equity reactions; temporal split |
| zhang2026bankruptcy | 2026 · preprint | none | bankruptcy from 10-K text | yes (EDGAR + LoPucki) | — | network diffusion of distress |
| chen2026sttgexplainer / lu2026training / sussek2026explaining | 2026 · ICML/ICLR | temporal GNN explainers | explanation | non-finance | yes | financial graphs; economic plausibility |
| zhang2026spatialtemporal (CF2TLP) | 2026 · journal | temporal stock graph | temporal LP for crisis forecasting | partly | — | signed/directed edges; event labels (to check in kill tests) |
| liu2026bipartite | 2026 · preprint | directed bipartite US→China | cross-market forecasting | yes | — | Europe; overlapping hours |
