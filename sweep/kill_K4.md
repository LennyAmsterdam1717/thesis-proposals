# Kill tests K4: I8, I9, I10, I16, I19, I20

Kill-test agent K4, 2 Nov 2026. Rules: `sweep/KILL_RULES.md`. "Known" means the paper is already listed in `ideas_raw.md`. Only papers opened in this pass appear as "new". Verification legend: [FT] full text read, [ABS] abstract or record page opened, [UNVERIFIED] snippet only.

---

## I8: Extraction multiverse for LLM-built company graphs
- **Queries:**
  1. "multiverse analysis LLM knowledge graph construction robustness": GraphRAG robustness and KG-enrichment papers only. Nothing varies the extraction pipeline and measures graph divergence.
  2. "specification curve LLM annotation choices downstream conclusions": found the general-method precedents baumann2025large and crainic2026prompt.
  3. "sensitivity of extracted knowledge graph to LLM choice prompt variance network structure": found murin2026measuring (clinical, not graphs).
  4. "financial knowledge graph extraction comparing LLMs graph divergence 10-K 2026": found dimino2025finreflectkg (EvalBench).
  5. "LLM-extracted firm network reliability downstream asset pricing measurement error": found grieser LLM measurement-error seminar (finance, not graphs), plus yang2026llm and kose2026reconstructing.
- **Closest work found (new):**
  - baumann2025large, *Large Language Model Hacking: Quantifying the Hidden Risks of Using LLMs for Text Annotation*. https://arxiv.org/abs/2509.08825v1. 2025-09-10, preprint, [ABS]. Covers 18 LLMs, 37 annotation tasks and 2,361 hypotheses. About 31% of conclusions flip with configuration choices for state-of-the-art models. This is the methodological template for a multiverse, but the domain is social-science labels: no graphs and no finance.
  - crainic2026prompt, *The Prompt Is the Analytic Choice: Specification Curve Analysis for LLM-Based Social Science*. https://icml.cc/virtual/2026/68298. ICML 2026, [ABS]. Builds a 2,592-cell multiverse over model, persona, framing and temperature for synthetic survey respondents, with a variance decomposition (η²). Same method, different object (survey answers, not relation graphs).
  - murin2026measuring, *Measuring the sensitivity of LLM-based structured extraction to prompt, model, and schema choices in clinical discharge summaries*. https://arxiv.org/pdf/2606.05970. June 2026, preprint, [FT]. Measures cross-prompt kappa on MIMIC-IV flags. It does not extract graphs and does not measure downstream conclusions.
  - dimino2025finreflectkg, *FinReflectKG – EvalBench*. https://arxiv.org/html/2510.05710v2. v2 2026-03-19, preprint, [ABS]. Compares single-pass, multi-pass and reflection extraction modes by triple-level LLM-judge scores. It has no structural graph comparison, no downstream financial test and no human gold labels; the authors name these as open.
  - yang2026llm (known; see I9) uses one LLM (Claude) and one prompt setup. It has no cross-model or prompt sensitivity analysis and hand-sets its fusion hyperparameters.
  - kose2026reconstructing (known; https://arxiv.org/pdf/2605.15842, [FT]) uses GPT-4o-mini only, with no prompt or model ablation, and validates on 100 human-annotated pages.
  - grieser LLM measurement-error seminar. https://bankfin.unipi.gr/seminar/william-grieser-oklahoma-state-university-llms-and-systematic-measurement-error. [ABS]. Argues that LLM-generated finance measures carry non-classical measurement error (shown on Item 1A risk text). No networks.
- **Verdict: SURVIVED.** The method (multiverse or specification-curve analysis of LLM measurement choices) is established in social-science annotation (2025–2026). Finance work on LLM measurement error exists for text scores. No paper varies LLM, prompt, ontology or entity-resolution choices for a company relation graph. None measures structural divergence (edge Jaccard, centrality rank, communities) or the spread of downstream equity results. Two essential dimensions differ:
  - Object: a relational graph, where errors propagate through topology, instead of independent labels.
  - Evaluation: graph-structural divergence plus downstream asset-pricing or link-prediction variance.
- **Sharpened version:** Frame the thesis explicitly as "LLM hacking for financial graphs". Run a pre-registered multiverse on one fixed corpus of 10-Ks with roughly six factors:
  - model
  - prompt
  - ontology (closed vs induced)
  - entity-resolution threshold
  - anonymisation
  - filing-only vs parametric channel

  Report η² per factor for graph metrics and for a fixed downstream test (e.g. huang2026-style cross-stock predictability). Anchor it with a 200–300-edge human gold set. Cite baumann2025large and crainic2026prompt as the method source.
- **Public data:** SEC EDGAR 10-K full text, plus open-weight LLMs (Qwen, Llama) and one API model. FinReflectKG is an open baseline graph. CRSP is not needed if prices come from public sources (yfinance or Stooq), but survivorship needs care. No blocker.
- **Feasibility (30 weeks, GenAI-assisted coding):** Good. The pipeline is engineering-heavy but standard. The main costs are LLM inference (cap at ~100–200 firms × 3–5 years) and annotating the gold set (about 2–3 weeks).

## I9: Edge-level look-ahead contamination of LLM stock relation graphs
- **Queries:**
  1. "LLM Latent Edge Measurement Point-in-Time Economic Graphs": found yang2026llm (opened) and zhang2026llmenhanced.
  2. "LLM memorization company relationships look-ahead bias supply chain knowledge graph": found gao2025detecting (known), yilki2026supply and Lopez-Lira's memorization paper. No edge-level test.
  3. "temporal knowledge leakage LLM extracted relations 'as of' date cutoff test 2026": found zhang2026all.
  4. "LLM-generated stock relationship graph leakage training cutoff backtest": found jeon2026blindfolded and Profit Mirage (2510.07920, not opened; agent-level leakage).
  5. "point-in-time knowledge LLM temporal knowledge probing corporate events after cutoff": generic knowledge-cutoff and evolving-knowledge probing (evolveQA, LLMLagBench). Nothing on firm relations.
  6. "chronologically consistent language model peer firms network look-ahead": ChronoGPT (2502.21206) and instruction-tuned chronological models (2510.11677). These are tools to use, not killers.
  7. "LLM recall supplier customer relationships evaluation without documents parametric knowledge firms": found FirmCORe (du2026firmcore) and kose2026reconstructing. Neither tests leakage.
- **Closest work found (new):**
  - yang2026llm, *LLM Latent Edge Measurement: Point-in-Time Economic Graphs for Quantitative Investing from Corporate Disclosures* (Yang and Zhang, VinePeak Research). https://arxiv.org/pdf/2607.15640. arXiv July 2026 (2607.x; exact date not printed), preprint, [FT].
    - Design: separates a filing channel (point-in-time by construction) from a "knowledge" channel (parametric memory, no documents) for 42 Nasdaq-100 firms, giving 149 edges.
    - It states that the knowledge channel inherits the training cutoff and would bias backtests, and recommends dropping it (109 vs 149 edges) or using cutoff-controlled models.
    - It does **not** measure contamination. It has no test of whether models output relations that began after t, no per-model contamination rate, no return comparison of point-in-time vs parametric graphs, and a single LLM.
    - Overlap with I9 is therefore conceptual only: it names the problem and offers the filing-vs-knowledge split as a design, which I9 can reuse as its two arms. I estimate it covers about 20% of I9 (the framing and two-channel design), not the measurement.
  - zhang2026llmenhanced (Lin Zhang, Cotality), *LLM-Enhanced Dynamic Financial Knowledge Graphs for Cross-Entity Signal Propagation and Alpha Discovery*. https://arxiv.org/pdf/2607.10932. July 2026 working paper, [FT].
    - Lists post-cutoff placebo queries and document-only prompt guards as a leakage-control plan, but tests nothing. All results are simulated.
  - zhang2026all, *All Leaks Count, Some Count More: Interpretable Temporal Contamination Detection in LLM Backtesting* (Zhang, Chen and Stadie). https://arxiv.org/html/2602.17234v1. 2026-02-19, preprint, [FT].
    - Detects leakage at claim level: it decomposes rationales into atomic claims, verifies them against the reference date, and Shapley-weights them.
    - Its objects are prediction rationales (Supreme Court cases, NBA salaries, stock-return ranking). It has no relations, graphs or edges.
    - Closest methodologically, since "claim" maps to "edge".
  - jeon2026blindfolded, *Can Blindfolded LLMs Still Trade? An Anonymization-First Framework…*. https://arxiv.org/html/2603.17692. 2026-03-18, preprint, [ABS]. Anonymises tickers and builds LLM-embedding semantic edges. Its leakage audit shuffles predictions and does not test edges.
  - yilki2026supply, *Supply Chain Propagation of Textual Signals: LLM Embeddings and Cross-Sectional Return Predictability*. https://arxiv.org/pdf/2606.29290. 2026, [FT]. Uses a hand-built 48-node supply graph with FinBERT embeddings. It argues look-ahead away rather than testing it, and runs no memorization test on relations.
  - du2026firmcore, *FirmCORe: benchmark for inter-firm collaboration reasoning*. https://arxiv.org/pdf/2609.17128. 2026, [FT]. Does not address memorization.
- **Verdict: SURVIVED.** It is *strengthened* by yang2026llm, which explicitly flags the unmeasured parametric-channel look-ahead in stock graphs as a problem. The look-ahead literature (gao2025detecting, benhenda2026look, zhang2026all, kong2026evaluating) works at the level of forecasts or rationales. Stock-graph papers (yang2026llm, zhang2026llmenhanced, kose2026reconstructing, huang2026crossstock) either ignore look-ahead or only recommend mitigations. Two essential dimensions differ:
  - Unit of test: edge or relation with an EDGAR-dated start, versus forecast or claim.
  - Evaluation: per-model contamination rate tied to known cutoffs, plus the downstream return gap between point-in-time and parametric graphs.
- **Sharpened version:**
  - Ground truth: relations dated by first appearance in 10-K, 8-K or press filings (EDGAR).
  - Probes: query 4–6 LLMs with known cutoffs, including a chronologically consistent ChronoGPT-style model as a negative control, for "as-of-t" relations.
  - Measure how often a model asserts relations that start in (t, cutoff], versus after its cutoff (placebo).
  - Downstream test: rerun yang2026llm's filing-vs-knowledge two-channel design and a huang2026-style return test on both graphs. Report the alpha gap.
- **Public data:** SEC EDGAR (10-K customer disclosures under ASC 280 >10% customers, 8-K material agreements, Item 1.01) for dating edges, plus open-weight LLMs with documented cutoffs and public price data. Blocker: dating the *start* of a relation precisely is noisy, so use "first filing mention" as the conservative lower bound.
- **Feasibility (30 weeks, GenAI-assisted coding):** Good. The core experiment is prompting plus matching. The hardest part is entity resolution and edge dating (about 4–6 weeks). A clean, publishable evaluation contribution.

## I10: Auditing learned stock graphs against real relations
- **Queries:**
  1. "Nyström Attention Matches Full Attention Cross-Sectional Stock Prediction": no hit.
  2. "arXiv 2609.08106 Nyström attention stock MASTER Guo": hit, guo2026nystrom opened.
  3. "learned stock graph interpretability alignment industry classification attention weights audit": model papers (GAPNet, OmniGNN, etc.) only. No audit.
  4. "does the graph matter stock prediction GNN random graph ablation 2026": found wade2026better (fixed graphs, not learned).
  5. "learned graph structure recovers economic links stocks supply chain ground truth comparison": production-network reconstruction literature (firm growth dynamics, 2302.09906; not stock-model audits).
  6. "graph structure learning stock relations evaluation 'learned graph' sector overlap analysis 2025": niu2024evaluating (known background) only.
- **Closest work found (new to this pass):**
  - guo2026nystrom, *Nyström Attention Matches Full Attention for Cross-Sectional Stock Prediction* (K. Guo, HKU). https://arxiv.org/pdf/2609.08106. 2026-09-08, preprint, [FT].
    - Systematically audits **one** model (MASTER, mostly seed 0) on **Chinese A-shares** (CSI300/800/all).
    - Attention is near-uniform and *anti*-correlated with return correlation (ρ ≈ −0.61, robust to industry, beta and volatility controls). Same-industry pairs get less attention, with no block structure.
    - The low-rank deviation is also present in untrained models.
    - Graph-necessity ablations (GraphMask, GraphBias, GCN, oracle/uniform attention) all hurt, so explicit graphs do not help.
    - Not examined: supply-chain, ownership and lead-lag relations, other architectures, other markets. The author says the results need verification on other architectures and markets.
  - wade2026better, *Do Better Volatility Forecasts Lead to Better Portfolios? Evidence from GNNs*. https://arxiv.org/html/2605.19278. 2026-05-20, [ABS]. Swaps fixed graphs (correlation, GICS, Granger) under a fixed GraphSAGE backbone. It does not audit learned graphs.
- **Verdict: WEAKENED.**
  - guo2026nystrom already performs the core audit for one model: alignment with industry and return similarity, plus graph-necessity ablations. It also delivers the headline finding that the learned structure is not economic.
  - Still open:
    - multiple architectures (MASTER vs explicit graph learners such as GAPNet or MaGNet hyperedges vs a graph-structure-learning GNN)
    - a US/EU point-in-time universe
    - relation layers beyond industry: 10-K customer/competitor links, 13F co-holdings, validated lead-lag edges
    - temporal analysis: do learned edges anticipate *new* real relations?
- **Sharpened version:** "Beyond attention: a multi-model, multi-relation audit of learned stock graphs." Test whether guo2026nystrom's anti-economic finding generalises:
  - across 3 model families with *explicit* learned adjacency or hyperedges, not just attention
  - on S&P 500 / STOXX 600 point-in-time
  - against 4 public relation layers, with degree-preserving null models

  Merging with I11 (learned edges as link predictors of future real relations) would add a forward-looking test that guo2026 lacks.
- **Public data:** Qlib-compatible price data, which can be built from public sources. Also SEC 10-K text (competitors/customers), SEC 13F, and SIC codes from EDGAR (GICS is licensed; use SIC/NAICS or Fama-French 49 instead). Open-source MASTER, GAPNet and MaGNet repos.
- **Feasibility (30 weeks, GenAI-assisted coding):** Feasible but riskier. Re-running 3 research codebases on a new universe is fiddly, so budget about 6–8 weeks. The relation layers overlap with I8/I9 infrastructure.

## I16: Temporal-GNN explainers on financial temporal graphs
- **Queries:**
  1. "explaining temporal graph neural network stock prediction explainer faithfulness": general TGNN explainers only (TempME, ST-TGExplainer [known], liu2026towards ICML 2026, dileo2025 TMLR evaluation). No finance.
  2. "post-hoc explanation dynamic GNN financial transaction graph TGNNExplainer evaluation 2026": found nekka2026amortised (UCI-Messages only) and lu2026training (known).
  3. "explainable GNN stock relations subgraph explanation economic plausibility supply chain": generic explainers (SubgraphX, GNNExplainer, GraphEXT 2507.17848, which has no finance data).
  4. "GNNExplainer stock price prediction interpretable inter-stock influence arXiv 2025": stock GNN papers (2506.18717, 2502.15813, DRFN, OmniGNN). They use visual inspection or no explainer.
- **Closest work found (new):**
  - liu2026towards, *Towards the Explainability of Temporal Graph Networks via Memory Backtracking and Topological Attribution* (Liu, Zhang, Xie, Xiong). https://icml.cc/virtual/2026/poster/61239. ICML 2026, [ABS]. An LRP-style TGN explainer evaluated on 9 unnamed temporal datasets. Finance is mentioned only as an application.
  - nekka2026amortised, *Amortised Post-Hoc Explanation with Exact Preservation for Dynamic Graph Anomaly Detectors*. https://arxiv.org/pdf/2608.15559. 2026, [FT].
    - Explains a dynamic-graph anomaly detector on UCI-Messages only. It states that ground-truth culprit benchmarks are missing.
    - Notably, it finds target-link **leakage** in released StrGNN code by code audit, not by the explainer. This supports the "explainer as leakage auditor" angle.
  - hu2025study, *Dynamic Stock Relationship Modeling … Differential Graph Transformer*. https://web3.arxiv.org/pdf/2506.18717. 2025, [FT]. S&P 500 data. Its interpretability rests on plots only, with no explainer and no metrics.
  - sonani2025stock, *Stock Price Prediction Using a Hybrid LSTM-GNN Model*. https://arxiv.org/abs/2502.15813v1. 2025, [ABS]. Names explainability as an open challenge and implements none.
- **Verdict: SURVIVED.** Known TGNN-explainer papers (ST-TGExplainer, TemGX, lu2026training, GRExplainer, liu2026towards) evaluate on non-financial benchmarks (Wikipedia, Reddit, UCI, etc.). Stock-GNN papers either use no formal explainer or use static attention. Two essential dimensions differ:
  - Data domain: financial temporal graphs.
  - Evaluation: economic-plausibility ground truth from 10-K relations, plus a leakage-audit use case, versus fidelity/sparsity on synthetic or social data.

  Caveat: sussek2026explaining and ding2026trace (known) were not re-opened here. The lead should confirm they do not already apply TGNN explainers to stock graphs.
- **Sharpened version:**
  - Train a TGN/DyGFormer on a temporal company graph: co-movement events, or an 8-K event graph plus 10-K links.
  - Run 3–4 TGNN explainers.
  - Score (a) fidelity and stability, and (b) economic plausibility, i.e. the precision of explanation edges against dated 10-K relations.
  - Planted-leakage experiments: inject future-dated edges or features and test which explainers flag them.
- **Public data:** SEC EDGAR, public prices, and open-source explainer code (TempME, T-GNNExplainer, ST-TGExplainer if released). No blocker.
- **Feasibility (30 weeks, GenAI-assisted coding):** Good. Explainer code exists, and the work is integration and evaluation design. Risk: explainers may be unstable on noisy financial graphs, which is itself a reportable result.

## I19: Event-typed weak-label benchmark for market-graph anomalies
- **Queries:**
  1. "stock market anomaly detection benchmark labeled events trading halts SEC enforcement": found chen2026velocity (known, opened again) and yang2025robust (synthetic injections).
  2. "financial time series anomaly detection benchmark real labels positive-unlabeled evaluation 2026": generic TSAD benchmarks (TAB 2506.18046, TimeSeriesBench). Not finance event-typed.
  3. "market manipulation detection benchmark dataset public equities graph anomaly 2025": found FiGraph, DGraph (2022, loan fraud) and AIMM (neela2025aimm, known).
  4. "FiGraph dynamic heterogeneous graph dataset financial anomaly detection listed companies": FiGraph record opened.
  5. "which events do anomaly detectors detect stock correlation network event study validation": found li2025explainable (2510.17088, opened; already listed in the I3 row of ideas_raw) and caputi2026financial (known, opened).
- **Closest work found (new to this pass):**
  - wang2025figraph, *FiGraph: A Dynamic Heterogeneous Graph Dataset for Financial Anomaly Detection*. WWW 2025 Companion, pp. 813–816. https://scholar.xjtu.edu.cn/en/publications/figraph-a-dynamic-heterogeneous-graph-dataset-for-financial-anoma/. [ABS].
    - Real dynamic heterogeneous graph (2014–2022, 9 snapshots, ~730k nodes, 4 edge types including investment and supply chain) with per-snapshot node labels.
    - Label semantics and market are not stated on the record (likely Chinese listed firms, [UNVERIFIED]). It is a single anomaly label, not event-typed.
  - li2025explainable, *Explainable Heterogeneous Anomaly Detection in Financial Networks via Adaptive Expert Routing*. https://arxiv.org/pdf/2510.17088. 2025, [FT]. Covers 100 S&P 500 stocks. It evaluates on 6 hand-picked macro events plus proxy mechanism labels (price shock, liquidity, contagion, reversal), with a mechanism breakdown for 2 case studies.
  - caputi2026financial (known, [FT] this pass). https://arxiv.org/pdf/2604.02549. TSX-60 correlation graphs, labelled by 7 stress-index peaks.
  - chen2026velocity (known, [FT] this pass). https://arxiv.org/pdf/2608.05373. Uses real SEBI/SEC enforcement labels, positive-only, with precision reported as a floor. It has an event-type breakdown (pump-and-reversal vs marking-the-close) but is **not graph-based** and covers a single case each.
- **Verdict: SURVIVED** (narrowly; parts are borrowed). Prior work uses either a handful of macro stress events (caputi2026, li2025explainable), synthetic injections, or a single opaque anomaly label (FiGraph). chen2026velocity already uses positive-only real labels with type breakdowns, but non-graph and n≈2 cases. Two essential dimensions differ:
  - Labels: a large-N, firm/edge-level, multi-type event set from public sources (litigation, halts, index changes, earnings surprises, bankruptcies, 8-K items).
  - Evaluation: per-event-type positive-unlabelled recall/precision floor for graph detectors.
- **Sharpened version:** "What do market-graph anomaly detectors actually see?"
  - Build a public, point-in-time, event-typed label set for S&P 1500 (2005–2025).
  - Run 5–6 graph/temporal-graph detectors (ROLAND, GDN, correlation-graph TDA, snapshot GAD) and non-graph baselines.
  - Report per-type detection under PU metrics, borrowing chen2026velocity's precision-floor convention. Pre-register a lead vs coincident analysis.
- **Public data:**
  - SEC litigation releases and AAER
  - Nasdaq/NYSE halt archives (historical depth to check)
  - S&P index change announcements (press releases; Wikipedia lists [UNVERIFIED completeness])
  - 8-K items via EDGAR full-text search
  - earnings surprises: I/B/E/S is licensed, so a free proxy is the 8-K Item 2.02 return reaction
  - bankruptcies: 8-K Item 1.03 and the UCLA-LoPucki BRD (public)
- **Feasibility (30 weeks, GenAI-assisted coding):** Good. The label assembly is tedious but scriptable. Risk: the benchmark contribution needs careful framing to count as "creative" for the supervisor. It pairs naturally with I3 or I20 as the evaluation layer.

## I20: "Unexplained co-movement" relative to a company KG
- **Queries:**
  1. "excess comovement unexplained by fundamentals links retail coordination meme stocks network": found ren2026effect and schmerling (meme stocks).
  2. "residual correlation network after factor model firm linkages economic relations explain comovement 2026": older finance (Dallas Fed 2019 WP and others) and huang2026crossstock (known). Nothing flags unexplained edges.
  3. "anomalous stock pairs co-movement without economic link knowledge graph detection manipulation": coupled-behaviour manipulation (2012, background), shadow-trading graph (jaeger2025, known).
  4. "how much stock return correlation is explained by economic linkages supply chain common ownership decomposition": classic finance papers (2012–2019, not opened; background). No 2025–2026 hit.
- **Closest work found (new):**
  - ren2026effect, *The Effect of Investor-Driven Information Diffusion on Excess Comovement: Evidence from Retail and Institutional Investors in China and the United States*. https://arxiv.org/pdf/2605.08726. 2026, [FT].
    - Excess comovement is defined as the pairwise correlation of FF5 residuals.
    - It explains this with retail co-attention (Guba/StockTwits) and common institutional ownership over about 140M US pairs.
    - It estimates average effects only. It does not flag anomalous pairs and has no knowledge graph.
    - Closest in spirit: it supplies the "explained by" regressors I20 would use, and shows that co-attention explains part of the residual.
  - schmerling, *A Century of Meme Stocks and the Price of Coordination* (Quantpedia summary, 2026-10-05). https://quantpedia.com/a-century-of-meme-stocks-and-the-price-of-coordination/. [ABS] (summary only). A firm-level memeness classifier, not pairwise or network.
- **Verdict: SURVIVED.** Finance work explains average excess comovement with ownership and attention links (ren2026effect). huang2026crossstock uses an LLM to *remove* spurious edges, the opposite direction. Nothing in the searches flags individual edges whose residual co-movement or lead-lag is unexplained by factors *and* by a KG of firm relations, or validates those edges against coordination or manipulation episodes. Two essential dimensions differ:
  - Task: edge-level anomaly detection (residual of a relational explanation model), versus average-effect regression or prediction.
  - Explanatory set: a multi-relation company KG (supply, competitor, co-ownership, co-attention), versus factors or a single link type.
- **Sharpened version:**
  - Fit an edge-level model of residual co-movement on KG relations plus factors (with ren2026effect's co-attention and 13F overlap as controls).
  - Flag large positive residual edges per window.
  - Validate against meme/retail episodes (Roundhill MEME holdings, StockTwits spikes), SEC manipulation releases and I19 labels.
  - Main risk: "unexplained" may mostly reflect KG incompleteness, so quantify the effect of KG coverage. This links I20 to I8.
- **Public data:** Public prices; Ken French factors; SEC 10-K text and 13F; StockTwits (API access now restricted, so availability needs checking); FinReflectKG; SEC litigation releases. Blocker: retail-attention data access.
- **Feasibility (30 weeks, GenAI-assisted coding):** Moderate-good. Statistically simple. Validation labels are the weak point, so combining it with I19 is recommended.

---

## Cross-idea note
I8, I9 and I10 share infrastructure: an EDGAR-dated company relation graph, open LLMs and a public price panel. I9 (contamination) and I8 (multiverse) could be one thesis with I9 as the lead experiment. yang2026llm is the strongest new anchor: it explicitly leaves look-ahead measurement, cross-model robustness and return tests to future work.

## BibTeX (new papers only)
```bibtex
@misc{yang2026llm,
  author = {Yang, Fan and Zhang, Lin},
  title = {{LLM} Latent Edge Measurement: Point-in-Time Economic Graphs for Quantitative Investing from Corporate Disclosures},
  year = {2026}, eprint = {2607.15640}, archivePrefix = {arXiv},
  url = {https://arxiv.org/abs/2607.15640},
  note = {[FT] arXiv July 2026 (2607.x); preprint}
}
@misc{zhang2026llmenhanced,
  author = {Zhang, Lin},
  title = {{LLM}-Enhanced Dynamic Financial Knowledge Graphs for Cross-Entity Signal Propagation and Alpha Discovery},
  year = {2026}, eprint = {2607.10932}, archivePrefix = {arXiv},
  url = {https://arxiv.org/abs/2607.10932},
  note = {[FT] July 2026; working paper, simulation only}
}
@misc{zhang2026all,
  author = {Zhang, Zeyu and Chen, Ryan and Stadie, Bradly C.},
  title = {All Leaks Count, Some Count More: Interpretable Temporal Contamination Detection in {LLM} Backtesting},
  year = {2026}, eprint = {2602.17234}, archivePrefix = {arXiv},
  url = {https://arxiv.org/abs/2602.17234},
  note = {[FT] 2026-02-19; preprint}
}
@misc{jeon2026can,
  author = {Jeon, Joohyoung and Lee, Hongchul},
  title = {Can Blindfolded {LLMs} Still Trade? An Anonymization-First Framework for Portfolio Optimization},
  year = {2026}, eprint = {2603.17692}, archivePrefix = {arXiv},
  url = {https://arxiv.org/html/2603.17692},
  note = {[ABS] 2026-03-18; preprint}
}
@misc{yilki2026supply,
  author = {Y{\i}lk{\i}, Asef},
  title = {Supply Chain Propagation of Textual Signals: {LLM} Embeddings and Cross-Sectional Return Predictability},
  year = {2026}, eprint = {2606.29290}, archivePrefix = {arXiv},
  url = {https://arxiv.org/abs/2606.29290},
  note = {[FT] June 2026 (2606.x); preprint}
}
@misc{du2026firmcore,
  author = {Du, Tian and Wu, Tiantong and Wang, Yafei and Liu, Mengyu and Chen, Xingyan and Wang, Mu},
  title = {{FirmCORe}: A Benchmark for Structured Reasoning about Inter-Firm Collaboration Opportunities},
  year = {2026}, eprint = {2609.17128}, archivePrefix = {arXiv},
  url = {https://arxiv.org/abs/2609.17128},
  note = {[FT] Sept 2026 (2609.x); preprint}
}
@misc{baumann2025large,
  author = {Baumann, Joachim and R{\"o}ttger, Paul and Urman, Aleksandra and Wendsj{\"o}, Albert and Plaza-del-Arco, Flor Miriam and Gruber, Johannes B. and Hovy, Dirk},
  title = {Large Language Model Hacking: Quantifying the Hidden Risks of Using {LLMs} for Text Annotation},
  year = {2025}, eprint = {2509.08825}, archivePrefix = {arXiv},
  url = {https://arxiv.org/abs/2509.08825},
  note = {[ABS] 2025-09-10; preprint}
}
@inproceedings{crainic2026prompt,
  author = {Crainic, Jacob and Yee, Brandon and Koh, Pairie},
  title = {The Prompt Is the Analytic Choice: Specification Curve Analysis for {LLM}-Based Social Science},
  booktitle = {ICML},
  year = {2026},
  url = {https://icml.cc/virtual/2026/68298},
  note = {[ABS] ICML 2026 virtual page}
}
@misc{murin2026measuring,
  author = {Murin, Martin},
  title = {Measuring the sensitivity of {LLM}-based structured extraction to prompt, model, and schema choices in clinical discharge summaries},
  year = {2026}, eprint = {2606.05970}, archivePrefix = {arXiv},
  url = {https://arxiv.org/abs/2606.05970},
  note = {[FT] June 2026; preprint}
}
@misc{dimino2025finreflectkg,
  author = {Dimino, Fabrizio and Arun, Abhinav and Sarmah, Bhaskarjit and Pasquali, Stefano},
  title = {{FinReflectKG} -- {EvalBench}: Benchmarking Financial {KG} with Multi-Dimensional Evaluation},
  year = {2025}, eprint = {2510.05710}, archivePrefix = {arXiv},
  url = {https://arxiv.org/abs/2510.05710},
  note = {[ABS] v1 Oct 2025, v2 2026-03-19; preprint}
}
@misc{grieser2026llms,
  author = {Grieser, William},
  title = {{LLMs} and Systematic Measurement Error},
  year = {2026},
  url = {https://bankfin.unipi.gr/seminar/william-grieser-oklahoma-state-university-llms-and-systematic-measurement-error},
  note = {[ABS] seminar abstract only; working paper; year and coauthors unverified}
}
@misc{guo2026nystrom,
  author = {Guo, Kunhan},
  title = {Nystr{\"o}m Attention Matches Full Attention for Cross-Sectional Stock Prediction},
  year = {2026}, eprint = {2609.08106}, archivePrefix = {arXiv},
  url = {https://arxiv.org/abs/2609.08106},
  note = {[FT] 2026-09-08; preprint (already in sweep; re-opened)}
}
@misc{wade2026better,
  author = {Wade, Rylan},
  title = {Do Better Volatility Forecasts Lead to Better Portfolios? Evidence from Graph Neural Networks},
  year = {2026}, eprint = {2605.19278}, archivePrefix = {arXiv},
  url = {https://arxiv.org/abs/2605.19278},
  note = {[ABS] v2 2026-05-20; preprint}
}
@inproceedings{liu2026towards,
  author = {Liu, Yazheng and Zhang, Xi and Xie, Sihong and Xiong, Hui},
  title = {Towards the Explainability of Temporal Graph Networks via Memory Backtracking and Topological Attribution},
  booktitle = {ICML},
  year = {2026},
  url = {https://icml.cc/virtual/2026/poster/61239},
  note = {[ABS] ICML 2026 poster page}
}
@misc{nekka2026amortised,
  author = {Nekka, Iyad Assaad and Seba, Hamida and Hidouci, Khaled-Walid and Amrouche, Karima},
  title = {Amortised Post-Hoc Explanation with Exact Preservation for Dynamic Graph Anomaly Detectors},
  year = {2026}, eprint = {2608.15559}, archivePrefix = {arXiv},
  url = {https://arxiv.org/abs/2608.15559},
  note = {[FT] Aug 2026 (2608.x); preprint}
}
@misc{hu2025study,
  author = {Hu, Linyue and Wang, Qi},
  title = {A Study of Dynamic Stock Relationship Modeling and {S\&P500} Price Forecasting Based on Differential Graph Transformer},
  year = {2025}, eprint = {2506.18717}, archivePrefix = {arXiv},
  url = {https://arxiv.org/abs/2506.18717},
  note = {[FT] June 2025 (2506.x); preprint}
}
@misc{sonani2025stock,
  author = {Sonani, Satishbhai and Badii, Atta and Moin, Armin},
  title = {Stock Price Prediction Using a Hybrid {LSTM-GNN} Model: Integrating Time-Series and Graph-Based Analysis},
  year = {2025}, eprint = {2502.15813}, archivePrefix = {arXiv},
  url = {https://arxiv.org/abs/2502.15813},
  note = {[ABS] Feb 2025; preprint}
}
@inproceedings{wang2025figraph,
  author = {Wang, Xiaoguang and Wang, Chenxu and Liu, Huanlong and Wang, Mengqin and Qin, Tao and Wang, Pinghui},
  title = {{FiGraph}: A Dynamic Heterogeneous Graph Dataset for Financial Anomaly Detection},
  booktitle = {Companion Proceedings of the ACM Web Conference 2025},
  pages = {813--816}, year = {2025},
  url = {https://scholar.xjtu.edu.cn/en/publications/figraph-a-dynamic-heterogeneous-graph-dataset-for-financial-anoma/},
  note = {[ABS] WWW 2025 Companion}
}
@misc{li2025explainable,
  author = {Li, Zan and Fan, Rui},
  title = {Explainable Heterogeneous Anomaly Detection in Financial Networks via Adaptive Expert Routing},
  year = {2025}, eprint = {2510.17088}, archivePrefix = {arXiv},
  url = {https://arxiv.org/abs/2510.17088},
  note = {[FT] Oct 2025 (2510.x); preprint (listed in ideas_raw I3; re-opened)}
}
@misc{yang2025robust,
  author = {Yang, Jian and Liu, Lili},
  title = {Robust Anomaly Detection in Financial Markets Using {LSTM} Autoencoders and Generative Adversarial Networks},
  year = {2025},
  url = {https://www.preprints.org/manuscript/202506.1907/v1},
  note = {[ABS] Preprints.org 2025-06-25; synthetic injected anomalies}
}
@misc{ren2026effect,
  author = {Ren, Fei and Yi, Miao-Miao and Chen, Zhang-Hangjian and Gao, Xiang},
  title = {The Effect of Investor-Driven Information Diffusion on Excess Comovement: Evidence from Retail and Institutional Investors in China and the United States},
  year = {2026}, eprint = {2605.08726}, archivePrefix = {arXiv},
  url = {https://arxiv.org/abs/2605.08726},
  note = {[FT] May 2026 (2605.x); preprint}
}
@misc{schmerling2026century,
  author = {Schmerling, Chad},
  title = {A Century of Meme Stocks and the Price of Coordination},
  year = {2026},
  url = {https://quantpedia.com/a-century-of-meme-stocks-and-the-price-of-coordination/},
  note = {[ABS] Quantpedia summary 2026-10-05 only; original paper not opened}
}
```

## Query log
| # | Idea | Query | Useful hits |
| --- | --- | --- | --- |
| 1 | I9 | LLM Latent Edge Measurement Point-in-Time Economic Graphs | yang2026llm, zhang2026llmenhanced |
| 2 | I9 | LLM memorization company relationships look-ahead bias supply chain knowledge graph | gao2025 (known), yilki2026supply |
| 3 | I9 | temporal knowledge leakage LLM extracted relations "as of" date cutoff test 2026 | zhang2026all |
| 4 | I9 | LLM-generated stock relationship graph leakage training cutoff backtest | jeon2026can, Profit Mirage (not opened) |
| 5 | I9 | point-in-time knowledge LLM temporal knowledge probing corporate events after cutoff | generic cutoff probing; none relational |
| 6 | I9 | chronologically consistent language model peer firms network look-ahead | ChronoGPT family (tools) |
| 7 | I9/I8 | LLM recall supplier customer relationships evaluation without documents parametric knowledge firms | du2026firmcore, kose2026 (known) |
| 8 | I8 | multiverse analysis LLM knowledge graph construction robustness | none relevant |
| 9 | I8 | specification curve LLM annotation choices downstream conclusions | baumann2025large, crainic2026prompt |
| 10 | I8 | sensitivity of extracted knowledge graph to LLM choice prompt variance network structure | murin2026measuring |
| 11 | I8 | financial knowledge graph extraction comparing LLMs graph divergence 10-K 2026 | dimino2025finreflectkg |
| 12 | I8 | LLM-extracted firm network reliability downstream asset pricing measurement error | grieser2026llms |
| 13 | I10 | Nyström Attention Matches Full Attention Cross-Sectional Stock Prediction | no hit |
| 14 | I10 | arXiv 2609.08106 Nyström attention stock MASTER Guo | guo2026nystrom |
| 15 | I10 | learned stock graph interpretability alignment industry classification attention weights audit | none (model papers) |
| 16 | I10 | does the graph matter stock prediction GNN random graph ablation 2026 | wade2026better |
| 17 | I10 | learned graph structure recovers economic links stocks supply chain ground truth comparison | production-network reconstruction (bkg) |
| 18 | I10 | graph structure learning stock relations evaluation "learned graph" sector overlap analysis 2025 | niu2024evaluating (known) |
| 19 | I16 | explaining temporal graph neural network stock prediction explainer faithfulness | liu2026towards, ST-TGExplainer (known) |
| 20 | I16 | post-hoc explanation dynamic GNN financial transaction graph TGNNExplainer evaluation 2026 | nekka2026amortised, lu2026 (known) |
| 21 | I16 | explainable GNN stock relations subgraph explanation economic plausibility supply chain | none financial |
| 22 | I16 | GNNExplainer stock price prediction interpretable inter-stock influence arXiv 2025 | hu2025study, sonani2025stock (no explainers) |
| 23 | I19 | stock market anomaly detection benchmark labeled events trading halts SEC enforcement | chen2026velocity (known), yang2025robust |
| 24 | I19 | financial time series anomaly detection benchmark real labels positive-unlabeled evaluation 2026 | generic TSAD benchmarks |
| 25 | I19 | market manipulation detection benchmark dataset public equities graph anomaly 2025 | FiGraph, DGraph (2022), AIMM (known) |
| 26 | I19 | FiGraph dynamic heterogeneous graph dataset financial anomaly detection listed companies | wang2025figraph |
| 27 | I19 | which events do anomaly detectors detect stock correlation network event study validation | li2025explainable, caputi2026 (known) |
| 28 | I20 | excess comovement unexplained by fundamentals links retail coordination meme stocks network | ren2026effect, schmerling2026century |
| 29 | I20 | residual correlation network after factor model firm linkages economic relations explain comovement 2026 | none new |
| 30 | I20 | anomalous stock pairs co-movement without economic link knowledge graph detection manipulation | older/background only |
| 31 | I20 | how much stock return correlation is explained by economic linkages supply chain common ownership decomposition | older finance (not opened) |
