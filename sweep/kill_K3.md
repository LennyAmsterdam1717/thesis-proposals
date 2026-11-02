# Kill tests K3: credit/event-propagation cluster (I4, I29, I17, I26, I27, I28, I30, I22)

Run by kill-test subagent K3 on 2026-11-02, following `KILL_RULES.md`.

Process notes:
- I ran about 45 WebSearches and about 50 WebFetches.
- Two endpoints were blocked by the session proxy (403): the OpenReview API (`api2.openreview.net`) and EDGAR full-text-search JSON (`efts.sec.gov`). So OpenReview was not queried, and the EDGAR 8-K Item 1.03 check relies on documentation rather than a live query.
- "Known" papers (already in `ideas_raw.md`) are not re-listed as new unless they matter for a verdict.

**Paper that changes the picture: FinRipple (Xu et al., Findings of ACL 2025).** It already implements the generic "event at firm i → signed impact on KG-connected firms j, scored against abnormal returns" pipeline. Its setup:
- S&P 500 news from 2001 to 2023.
- A time-varying KG with four relation types (supply chain, board, fund co-holding, patent similarity).
- An LLM with KG adapters and a PPO reward on CAPM residuals.

It is not a killer for I4/I29, because the event types, data and evaluation all differ (see below). But every proposal in this cluster must now cite it and position against it.

---

## I4 — Credit events → connected firms' signed equity reactions

- Queries:
  1) "bankruptcy filing customer supplier stock returns contagion 2025" → only Hertzel et al. 2008 (JFE, background); no 2025–26 hit.
  2) "machine learning predict peer firms stock reaction bankruptcy announcement" → nothing relevant (own-firm bankruptcy prediction only).
  3) "credit rating downgrade spillover rivals suppliers equity returns 2026" → Alldredge et al. 2022 (RAF), older event studies.
  4) "Chapter 11 announcement competitors contagion competitive effect stock 2025 study" → Krzeczewska & Pastusiak 2022 (IJFE); older work.
  5) "default contagion equity network listed firms graph neural network 2026 arXiv" → Umeorah & Fadina 2026 (banks, CDS); Özden 2026 (record only); Zandi 2025 (known).
  6) "customer bankruptcy supplier stock price reaction supply chain disclosure 2026" → nothing new.
  7) "ICAIF 2025 predicting stock reactions to related company events knowledge graph" → **FinRipple** (ACL Findings 2025).
  8) "rating downgrade announcement contagion competitive effects rivals machine learning heterogeneity" → only pre-2016 finance event studies.
- Closest work found (new):
  - xu2025finripple — FinRipple: Aligning LLMs with Financial Market for Event Ripple Effect Awareness — https://arxiv.org/abs/2505.23826 (also https://preview.aclanthology.org/display_plenaries/2025.findings-acl.489.pdf) — 2025-05-28 — Findings of ACL 2025 — [FT].
    - Same structure as I4: a news event goes in, and the model outputs a signed score per KG-connected S&P 500 firm, aligned to CAPM residuals.
    - Differences from I4:
      - The events are generic news (organisational adjustments, new initiatives). Credit events and earnings events are not studied.
      - The KG is commercial (Compustat/Capital IQ supply chain, BoardEx, Thomson fund holdings).
      - Evaluation uses residual-on-score R² plus a portfolio. There is no edge-level classification metric.
  - hertzel2008interfirm [BACKGROUND] — Inter-firm linkages and the wealth effects of financial distress along the supply chain — https://academicnewsletter.sufe.edu.cn/info/357307 — JFE 87, 2008 — [ABS]. Suppliers lose value around customers' distress and filings, and the effect is larger when intra-industry contagion is strong. This is an in-sample event study with no per-pair prediction.
  - alldredge2022effect [BACKGROUND] — The Effect of Credit Rating Downgrades Along the Supply Chain — https://scholarsmine.mst.edu/bio_inftec_facwork/441 — RAF 21(1), 2022 — [ABS]. Customer downgrades lower supplier CARs. Cross-sectional determinants: anticipation, link strength, rival reaction, attention.
  - krzeczewska2022does [BACKGROUND] — Does bankruptcy filing always mean contagion? Evidence from industry rivals — https://international.vlex.com/vid/does-bankruptcy-filing-always-1049437335 — IJFE 27(1), 2022 — [ABS]. Polish rivals react slightly positively on average (competitive effect), but the result is not robust by industry. This shows that the sign really varies.
  - umeorah2026temporal — A Temporal Multiplex GNN for Systemic Risk Transmission in Global Banking — https://arxiv.org/pdf/2608.27295 — arXiv 2026 (ID 2608) — [FT]. Covers 29 banks, with CDS as the target. No equity and no credit events, so it is not close.
- Verdict: **SURVIVED (narrow margin).** The closest work (FinRipple) differs in three essential dimensions:
  - (i) Event type: credit events (bankruptcy, downgrade) vs generic news.
  - (ii) Data: public graph (10-K, 13F, TNIC) vs commercial KG.
  - (iii) Evaluation: edge-level, out-of-time prediction of *which* neighbours react and with *which sign* vs R² of residual regression.
  
  The finance literature (Hertzel 2008, Alldredge 2022, Krzeczewska 2022) documents average signs and their in-sample correlates only.
- Sharpened version: For each credit event, rank and classify the event firm's 1–2-hop neighbours on a point-in-time public multiplex. Labels: significant negative CAR, positive CAR, or none. The models have to beat relation-type-only, industry-only and FinRipple-style LLM-scoring baselines, out of time. Report how much the competitor-vs-contagion sign is predictable from relation type and the neighbour's characteristics.
- Public data:
  - **BRD:** The Florida-UCLA-LoPucki BRD moved from UCLA to `lopucki.law.ufl.edu`. Its Cases table is a **free download** that the site says will stay up indefinitely. Coverage:
    - Large public companies (≥ $100M assets in 1980 dollars) with Chapter 7/11 cases from Oct 1979 **through December 2022 only**.
    - The site says no further updates will follow, and Law360 (23 Mar 2026) confirms that LoPucki stopped working on it in 2022. There is no successor database.
  - **8-K Item 1.03 for 2023 onward:**
    - EDGAR full-text search covers 2001 onward and filters by form type (FAQ, [FT]). The FAQ does not document an item-number filter.
    - The documented route is to pull all 8-Ks and filter on the parsed `items` list, which edgartools exposes (e.g. `'Item 1.03' in eight_k.items`) [FT of docs]. A full-text phrase query "Item 1.03" restricted to form 8-K is a workable alternative.
    - Live retrieval was **not tested** (`efts.sec.gov` blocked from this sandbox).
    - Expect only tens of large listed-firm bankruptcies per year, so power is the main risk.
  - **Downgrades:** 17g-7(b) rating histories are published by each NRSRO on its website as XBRL (SEC OCR page, [ABS]). They are usable but messy, with per-agency files and issuer-name matching.
  - **Major-customer links:** see the 10-K major-customer item in the I4/I29 data verification section below.
- Feasibility (30 weeks, GenAI-assisted coding): Feasible if credit events are a *transfer test* on top of a larger event set (see I29). Credit events alone (BRD ≤2022 plus 8-K 1.03) give a few hundred events with heavy class imbalance, which is thin for learned models.

## I29 — Signed information-transfer edges at earnings announcements

- Queries:
  1) "intra-industry information transfer earnings announcements 2025 peer firms" → Frankel et al. 2025 (known); Lee, Byun & Kim 2026 (APJAE); a RAST 2026 paper (page failed to load).
  2) "graph neural network earnings announcement spillover peer stock returns" → only older economic-link momentum work (AAAI 2021) and Frankel.
  3) "event-driven link prediction stock relations earnings surprise arXiv 2026" → huang2026crossstock (known; 10-K semantic network, mean reversion, not event-conditioned); Li et al. 2025 (structured events).
  4) "peer earnings news contagion competition effect sign prediction stock returns 2026" → Koo, Wu & Yeung 2017 (sign depends on attribution; background).
  5) "Avramov dual peer effects cross-stock predictability" → the Cambridge WP version (known).
  6) "deep learning which stocks react to other firm's news cross-firm news impact arXiv 2025" → Inoue & Todo 2024/2026; Murjani 2026.
  7) "which peers respond to earnings news customer supplier competitor sign economic links 2025 working paper" → only pre-2018 accounting work.
  8) "ICAIF 2025 predicting stock reactions to related company events knowledge graph" → FinRipple.
- Closest work found (new):
  - xu2025finripple (see I4) — [FT]. Signed ripple prediction for news events on a commercial KG. Not earnings-specific, and it reports no edge-level sign accuracy.
  - inoue2024stock — Stock Price Responses to Firm-Level News in Supply Chain Networks — https://arxiv.org/abs/2409.06255v5 — v1 2024-09-10, v5 2026-01-14 — preprint — [ABS].
    - Takes FinBERT news sentiment for a firm and relates it to its suppliers' and customers' stock prices, before and after disclosure.
    - Global links come from financial statements; Japanese links come from survey data. It is association-based (regressions), not per-edge prediction.
  - lee2026intraindustry — Intra-industry information transfer: evidence from accumulated confirmatory earnings news by peers — https://scholarworks.sogang.ac.kr/item/98e3524f-4cd4-4e2e-b879-7618be22b008 — 2026 (early access) — APJ Accounting & Economics — [ABS]. A firm's own earnings-announcement reaction is stronger when it confirms the direction of peers' earlier news. This is the reverse direction (peers → focal) and an in-sample regression.
  - koo2017earnings [BACKGROUND] — Earnings Attribution and Information Transfers — https://academicnewsletter.sufe.edu.cn/info/411313 — CAR 2017 — [ABS]. The sign of the transfer depends on what managers attribute the news to (industry-wide → positive; competitive moves → negative). This gives text-based features for sign prediction.
  - li2025structured — Structured Event Representation and Stock Return Predictability — https://arxiv.org/pdf/2512.19484 — 2025-12-22 — preprint — [FT, first 100k chars]. Uses LLM event triplets to predict each stock's own return. Cross-firm effects appear only as descriptive entity co-movement.
  - murjani2026zerocopy — Zero-Copy Semantic Contagion — https://arxiv.org/html/2606.05733 — 2026 — SIGMOD FinDS workshop — [FT]. A neural Hawkes model of news-attention contagion across 47 tickers in a single month (July 2022). It is too small to compete.
- Verdict: **SURVIVED.** It differs from FinRipple in:
  - (i) Event type: scheduled, quantified earnings surprises vs unstructured news.
  - (ii) Task form: event-conditioned temporal signed edge activation (link prediction) vs LLM scoring.
  - (iii) Data: public vs commercial KG.
  - (iv) Evaluation: edge-level AUC/F1 for sign vs R².
  
  The finance work (Frankel 2025, Das 2026, Lee 2026, Avramov 2025/26) is explanatory and in-sample. Frankel 2025 is the main *substantive* threat: co-movement is *lower* on earnings-announcement days. Peer reactions are therefore weak, and the thesis must show predictability at the edge level, not on average.
- Sharpened version: Treat each earnings announcement as a shock. Predict, out of time, the signed and lagged activation (CAR beyond a threshold, with sign) of each 1-hop neighbour in a point-in-time public multiplex graph (10-K customer/competitor, TNIC, 13F overlap).
  - Baselines: relation-type priors, the historical edge-activation rate (persistence), FinRipple-style LLM scoring, and a temporal GNN.
  - Then transfer the trained model zero-shot to credit events (I4).
  - Use Koo et al.-style attribution text from the 8-K Item 2.02 press release as a feature for the sign.
- Public data:
  - Announcement dates come from 8-K Item 2.02 (EDGAR).
  - Surprise without I/B/E/S: a standardised unexpected earnings measure (SUE) from the SEC Financial Statement Data Sets (XBRL EPS, seasonal random walk). It is noisier than consensus-based surprise.
  - Prices from yfinance or Stooq. Free sources carry survivorship risk, so build the point-in-time universe from EDGAR.
- Feasibility: **Good.** There are thousands of events per quarter, the labels are cheap and the pipeline is standard. The main risk is a weak signal (Frankel 2025), so preregister the edge-level metric and the persistence baseline.

## I17 — 2025 US tariff shock: multi-hop signed propagation

- Queries:
  1) "Liberation Day tariffs stock returns supply chain network exposure indirect 2026" → news only.
  2) "April 2025 tariff announcement European firms abnormal returns production network" → Garimella et al. 2025 (FRBSF); Bank of Italy QEF 993; Hu, Wang & Han 2026 (Econ Letters); a CESifo WP (not opened); student theses.
  3) "tariff shock higher-order propagation firm-level input-output stock market reaction NBER 2025" → Huang, Lin, Liu & Tang (2018 trade war); IMF WP 2026/070 (= beyer2026trade, known); BSSS 2026 (= barbiero2026trade, known).
  4) "tariffs 2025 customer supplier links stock returns indirect exposure FactSet Revere event study" → Barbiero et al. (known).
  5) "arXiv 2025 tariff graph neural network stock exposure supply chain knowledge graph" → Yılkı 2026 (not tariffs); TAGN (simulated tariff only).
- Closest work found (new):
  - hu2026tariff — Tariff exposure and Liberation Day reactions: Initial evidence from corporate filings — https://scholarlyworks.adelphi.edu/esploro/outputs/journalArticle/Tariff-exposure-and-liberation-day-reactions/991004547559806266 — 2026-02-01 — Economics Letters 259 — [ABS]. A text-based tariff exposure measure from 2024 10-K business and risk-factor sections explains 2 April 2025 abnormal returns. It covers direct exposure only.
  - benecchi2025us — US tariffs and Italian firms: mapping network exposure and early impact from survey data — https://ideas.repec.org/p/bdi/opques/qef_993_25.html — 2025 — Bank of Italy QEF 993 — [ABS].
    - Measures direct *and indirect* (production-network) exposure for 1.8M Italian firms, using private firm-to-firm and customs data.
    - The outcomes are survey sales declines, not stock returns.
  - garimella2025market — Market Reactions to Tariff Announcements — https://www.frbsf.org/research-and-insights/publications/economic-letter/2025/10/market-reactions-to-tariff-announcements/ — 2025-10-06 — FRBSF Economic Letter — [FT]. A sector-level event study. Indirect channels are interpreted, not measured.
  - huang2019trade [BACKGROUND] — Trade Networks and Firm Value: Evidence from the US-China Trade War — https://conference.nber.org/conf_papers/f130065.pdf — 2018/2019 — [FT]. Uses FactSet Revere 1-hop customer and supplier China exposure for the 2018–19 tariffs. Its figures sketch two layers, but its regressions use direct links only.
  - Known: barbiero2026trade (1-hop, FactSet) and beyer2026trade (direct exposure plus an uncertainty channel, explaining about 20%).
- Verdict: **WEAKENED.**
  - Direct exposure is done three ways for 2025: text (Hu 2026), trade (Beyer 2026) and 1-hop FactSet (Barbiero 2026).
  - Network exposure plus indirect channels is done for 2018 equities (1-hop, Huang et al.) and for 2025 Italian firms (multi-hop, but survey outcomes and private data).
  - Still open: **multi-hop (≥2) and signed (competitor-benefit) propagation** on a **public LLM-extracted** graph, explaining **2025 abnormal returns of US *and* European listed firms** beyond direct text exposure (Hu 2026) and correlation graphs.
- Sharpened version: Take Hu et al.'s 10-K tariff-exposure score as the node shock. Propagate it through a public 10-K/20-F customer–supplier–competitor graph with signed edges (competitors take a positive sign). Test whether 2-hop exposure adds explanatory power for the 2 Apr, 9 Apr and later event-window CARs of STOXX 600 and S&P 500 firms, with placebo-shuffled graphs.
- Public data: EDGAR 10-K/20-F (most European blue chips do not file 10-Ks, so their links must be inferred from their US counterparties' filings or annual reports), tariff dates, and yfinance prices.
- Feasibility: Feasible, but it is essentially **one shock cluster**. It is a cross-sectional event study with modest ML content, so it works better as a case study inside I4/I29 than as a stand-alone AI thesis.

## I26 — Heterogeneous firm–lender–supplier–owner graph: which relation carries credit shocks into equity?

- Queries:
  1) "shared lender contagion borrower stock returns bank distress spillover listed firms 2025" → Asgharian et al. 2024 (conference paper); Brož & Teplý 2025 (NAJEF).
  2) "heterogeneous graph transformer relation types credit risk listed companies supply chain shareholder guarantee 2025" → Chinese enterprise credit-risk heterogeneous-graph papers (private data); SME-HGT (unrelated).
  3) "credit agreements EDGAR exhibit 10 lender borrower network public dataset 2025" → a 2022 data project on payday lenders' credit agreements; raw Exhibit 10 filings.
  4) "multilayer firm network ownership supply chain lending which layer explains stock return spillovers 2026" → majhi2026 (known); Hungarian MNB multilayer firm network (private data); Gofman et al. (2017, supply chain only).
- Closest work found (new):
  - asgharian2024lender — Lender-Borrower Stock Return Dependencies in Credit Networks: Evidence from Syndicated Loan Deals — https://lup.lub.lu.se/record/9d7ac81e-4b4e-4991-aaed-a6b7eecd14e0 — 2024 — IRMC conference paper (unpublished) — [ABS]. Finds return dependence between small banks and risky borrowers. Lender returns follow borrower signals, not the reverse. It covers the lender layer only and uses no ML.
  - broz2025collapse — From collapse to contagion: How bank failures influence stock markets — https://ideas.repec.org/a/eee/ecofin/v79y2025ics1062940825000841.html — 2025 — NAJEF 79 — [ABS]. US bank failures 2001–2024 and the reaction of bank stocks. Bank-to-bank only.
  - xu2025finripple — [FT]. Its heterogeneous KG has supply chain, board, fund co-holding and patent relations, but no lender layer and no attribution by relation type.
- Verdict: **SURVIVED.** The closest works differ in:
  - (i) Task: credit-event → equity response of *non-bank* neighbours, attributed by relation type.
  - (ii) Graph: a four-relation heterogeneous graph including a lender layer, vs the bank–firm layer only (majhi2026, Asgharian) or no lender layer (FinRipple).
  - (iii) Data: public vs syndicated-loan, BoardEx or Capital IQ.
- Sharpened version: Make it the attribution analysis of I4/I29 rather than a separate thesis. Use relation-type ablation, R-GCN/HGT relation weights and edge-type Shapley values to ask whether the lender, supplier, owner or competitor layer carries the signed equity response.
- Public data: This is the **blocker**.
  - The lender layer is not public in structured form. DealScan is commercial. Credit agreements sit in Exhibit 10 filings, which EDGAR does not search directly, and need LLM extraction of lead arrangers per agreement. That is possible but is a sizeable project of its own.
  - Owner layer: 13F (public). Supplier layer: see the 10-K major-customer item in the data verification section.
- Feasibility: Risky for 30 weeks if the lender layer has to be built. Feasible if the lender layer is restricted to the agent banks of the 500 largest firms' credit facilities (one LLM pass over 10-K debt notes).

## I27 — Credit ↔ equity multiplex Hawkes from public marks

- Queries:
  1) "multivariate Hawkes equity jumps credit events contagion network firms arXiv 2025" → zelvyte2026 (known); Pasricha et al. 2025 (OPSEARCH); Hawkes surveys.
  2) "self-exciting point process corporate defaults rating downgrades clustering 2026" → Mori 2026 (two default-count papers, no point process or network); older default-clustering work.
  3) "neural temporal point process stock crash contagion supply chain graph Hawkes" → Murjani 2026; Yang 2023 (China index Hawkes).
- Closest work found (new):
  - murjani2026zerocopy (see I29) — [FT]. A neural Hawkes model with directed excitation between companies from news, evaluated on next-day extreme returns of 47 tickers. It is tiny and has no credit layer.
  - pasricha2025structural — Structural credit risk models with stochastic default barriers and jump clustering using Hawkes jump-diffusion processes — https://link.springer.com/article/10.1007/s12597-024-00830-9 — 2025 — OPSEARCH 62 — [ABS]. A theoretical structural model with no data network.
  - mori2026temporal / mori2026identifiability — https://arxiv.org/html/2606.12446 , https://arxiv.org/html/2604.18118v3 — 2026 — preprints — [FT].
    - These use annual and monthly aggregate default counts.
    - They state that Hawkes-type models need event timing data and network structure, which they do not have.
    - They are peripheral, but they confirm that contagion is weakly identified once common environmental shocks are controlled for.
- Verdict: **WEAKENED.** zelvyte2026multiplex already does a multiplex, covariate-layer network Hawkes model for credit (CDS jumps, 99 firms). Still open: **cross-excitation from credit events into equity jumps** (a bipartite credit→equity kernel), with layers built from public relations, compared against a GNN.
- Sharpened version: Use a two-mark Hawkes process. Mark A: public credit marks (8-K Item 1.03, 17g-7 downgrades, distance-to-default threshold crossings computed from equity and XBRL debt). Mark B: equity jump days. Excitation from A at firm i to B at firm j is parameterised by public layers (supply chain, 13F overlap, TNIC, sector). Test whether any layer has non-zero cross-excitation after a common-factor intensity is included (the Mori-style confound).
- Public data: CDS (zelvyte's input) is commercial, so the credit marks must be bankruptcies, downgrades or distance-to-default, which are sparse. Equity jumps can be computed from free prices.
- Feasibility: Moderate. tick or a custom PyTorch Hawkes is doable, but sparse credit marks make cross-excitation estimates noisy. It works best as a robustness model next to I4/I29, not as the main thesis.

## I28 — Supply-chain rewiring (decoupling) prediction and pricing

- Queries:
  1) "supply chain link prediction graph neural network firm network 2025 temporal" → Xiang et al. 2025 (GDES); Chang et al. AAAI 2025.
  2) "predicting supplier relationship termination decoupling China US firms stock returns" → Bao et al. 2026 (World Economy); a CEPR DP (not opened).
  3) "export controls supply chain reconfiguration firm-level links priced equity 2026 paper" → Crosignani et al. (NY Fed SR 1096, rev. Feb 2025).
- Closest work found (new):
  - xiang2025graph — Graph Neural Poisson Models for Supply Chain Relationship Forecasting — https://arxiv.org/html/2508.12044v1 — arXiv 2025-08 — preprint — [FT].
    - Forecasts the next period's supply edges, both formation and disruption, among 87,969 firms over 17 periods (AUC 93.8%).
    - The data provider is not named. There is no price link.
  - chang2025learning — Learning Production Functions for Supply Chains with Graph Neural Networks — https://cs.stanford.edu/people/jure/pubs/supplychain-gnn-aaai25.pdf — AAAI 2025 — [FT]. Temporal GNN (TGN/GraphMixer extensions) for transaction forecasting on proprietary bill-of-lading data (TradeSparq), plus the open SupplySim simulator.
  - bao2026trade — Trade Tensions and the Reconfiguration of Supply Chain Linkages: Evidence From U.S. Listed Firms — https://www.citedrive.com/en/discovery/trade-tensions-and-the-reconfiguration-of-supply-chain-linkages-evidence-from-us-listed-firms/ — 2026 — The World Economy (DOI 10.1111/twec.70117) — [ABS]. Uses FactSet Revere for 2017–2019. US-listed firms cut direct Chinese suppliers but stay indirectly dependent. Descriptive; no prediction and no pricing.
  - crosignani2025securing — Securing Technological Leadership? The Cost of Export Controls on Firms — https://www.newyorkfed.org/medialibrary/media/research/staff_reports/sr1096.pdf?sc_lang=en — Apr 2024, rev. Feb 2025 — NY Fed Staff Report 1096 — [ABS].
    - Export controls make US suppliers drop Chinese customers without forming new ones, giving a CAR of about −2.5% and roughly $130B in total.
    - Data: FactSet Revere, CRSP and confidential Y-14.
- Verdict: **WEAKENED.**
  - Temporal supply-chain link prediction, including deletions, exists (GDES 2025; Chang et al. AAAI 2025).
  - Cross-bloc rewiring after trade and export-control shocks is documented and partly priced (Bao 2026; Crosignani 2025; kose2026 known).
  - Still open: predicting *cross-bloc edge deletions* out of time on a **public** text-extracted graph, and testing whether *predicted* (not yet realised) rewiring is priced.
- Sharpened version: Use a link-deletion forecaster on a 10-K-extracted US-firm ↔ foreign-counterparty graph, with the BIS Entity List additions as the shock. The asset-pricing test sorts firms on predicted loss of Chinese links before the next 10-K reveals it.
- Public data: This is the blocker. 10-K customer disclosures rarely name Chinese counterparties (they are often anonymised or below threshold). Common Crawl extraction (kose2026) is heavy, and FactSet is commercial. Labels are sparse and annual.
- Feasibility: Low to moderate. Annual snapshots plus sparse cross-bloc edges give few positive deletion labels. A student could deliver the descriptive part, but the predictive-and-pricing claim is fragile.

## I30 — Distress-text diffusion over the supply network

- Queries:
  1) "customer financial distress predicts supplier bankruptcy prediction network features" → older trade-credit contagion work (PLoS ONE 2021; TU Delft).
  2) "graph neural network bankruptcy prediction listed US firms 10-K text supply chain neighbors 2026" → Zhu 2025 (NEEQ); Zhang, Pang & Li 2026 (ISF); Du et al. 2025 (Mathematics); Yılkı 2026.
  3) "economically linked firms textual tone spillover crash risk customer 10-K" → Chen (Macau seminar 2024) on customer forward-looking text → supplier crash risk; Ge et al. (text-based linkages and risk spillovers, older).
- Closest work found (new):
  - du2025enterprise — Enterprise bankruptcy prediction model based on heterogeneous GNN fusing external features and internal attributes — https://eprints.gla.ac.uk/363230 — 2025-08-27 — Mathematics 13(17):2755 — [ABS]. A multi-relational graph plus text plus financials for 32,459 Chinese enterprises. Not US, not listed firms, private relations.
  - zhang2026multiperspective — Multi-Perspective Fusion Graph Model for Financial Distress Prediction of Listed Companies — https://www.springerprofessional.de/en/multi-perspective-fusion-graph-model-for-financial-distress-pred/51996870 — 2026-01-29 — Information Systems Frontiers — [ABS]. Chinese listed firms, 2001–2020. Annual-report text and tabular data feed *KNN similarity* graphs with GraphSAGE, so there are no economic links.
  - zhu2025multichannel — Multi-Channel GNN for Financial Risk Prediction of NEEQ Enterprises — https://arxiv.org/abs/2507.12787 — 2025-07-17 — preprint — [ABS]. Numeric, text and relation channels for 7,731 Chinese NEEQ firms.
  - yilki2026supply — Supply Chain Propagation of Textual Signals: LLM Embeddings and Cross-Sectional Return Predictability — https://www.alphaxiv.org/abs/2606.29290 — 2026-06-28 — arXiv preprint — [ABS].
    - Propagates FinBERT MD&A embeddings over a supply-chain KG to predict returns.
    - The KG is tiny and hand-curated (48 nodes, 51 edges). The target is returns, not distress.
- Verdict: **WEAKENED.**
  - "Graph plus text → distress" is done on Chinese data, with similarity graphs or private relations.
  - "Text propagated over the supply chain → returns" exists in toy form (Yılkı 2026).
  - Own-firm 10-K distress text for US bankruptcy is done (zhang2026bankruptcy, known).
  - Still open: whether **neighbours' 10-K distress scores, diffused over public economic links**, add *out-of-time* predictive power for US listed firms' bankruptcy, downgrade or crash beyond the firm's own text score.
- Sharpened version: Take zhang2026bankruptcy's PB-Stress score as the node signal and diffuse it over 10-K customer/supplier and 13F layers. Test the increment in AUC and PR-AUC on chronological splits, with placebo-rewired graphs. Use crash and delisting labels to fix the 128-event problem.
- Public data: 10-K text from EDGAR, BRD labels (≤2022), 8-K Item 1.03 and delisting codes derivable from EDGAR, and crash labels from free prices.
- Feasibility: **Good** as an engineering task, because it reuses a published dictionary score. Event scarcity is the risk, so crash and downgrade labels are needed.

## I22 — Cross-firm insider / shadow-trading edge anomalies

- Queries:
  1) "shadow trading economically linked firms insiders empirical evidence 2025 Panuwat" → Mehta, Reeb & Zhao 2021 (TAR); Golemac et al. 2025 (Brattle/Westlaw).
  2) "Form 4 insider trades graph anomaly detection machine learning 2026" → jaeger2025needles (known); an older network insider-trading paper (arXiv 1702.05809); an informal replication brief.
  3) "opportunistic insider trading detection heterogeneous temporal graph insiders companies" → Jaeger (known); stenfors2025shadow (known).
- Closest work found (new):
  - mehta2021shadow [BACKGROUND] — Shadow trading — https://iris.unibocconi.it/handle/11565/4040993 — TAR 96(4), 2021 — [ABS]. Uses market-based informed-trading measures to show elevated informed trading in partners and competitors before a firm's private news. The core empirical phenomenon is established.
  - golemac2025shadow — Shadow trading: Economic evidence and potential implications from recent SEC insider trading cases — https://www.brattle.com/wp-content/uploads/2025/11/Shadow-Trading-Economic-Evidence-and-Potential-Implications-from-Recent-SEC-Insider-Trading-Cases.pdf — 2025-11-17 — Westlaw Today (practitioner) — [FT]. Event-study methods from SEC v. Panuwat and SEC v. Bechtolsheim. It warns that economic links are hard to establish and that event studies produce false positives.
  - Known: stenfors2025shadow (graph-based shadow-trading surveillance) and jaeger2025needles (Form 4 co-trading network).
- Verdict: **WEAKENED, close to killed on data grounds.**
  - The phenomenon is documented (Mehta 2021), graph surveillance exists (Stenfors 2025) and Form 4 network forensics exists (Jaeger 2025).
  - Key blocker [UNVERIFIED in this run, regulatory fact to be checked]: Form 4 reports trades only in the insider's *own* issuer's securities. The actual shadow trades in linked firms are therefore **not observable in public data**. Only market-level footprints are (abnormal volume, options, pre-announcement drift).
  - Ground truth is a handful of SEC cases, and named persons raise GDPR issues.
- Sharpened version (if kept): Use firm-level, anonymised *pre-announcement abnormal volume and return footprints in linked firms* as edge anomalies, scored against SEC litigation releases. This drops individuals entirely, so it becomes a subcase of I19/I3 rather than a separate idea.
- Public data: Form 4 (EDGAR), SEC litigation releases, and prices and volume. Options data (the strongest signal in Panuwat) is commercial.
- Feasibility: Low. Labels are tiny, the core signal is unobservable, and there is ethical and GDPR overhead. Recommend dropping it.

---

## Data verification for I4/I29 (requested checks)

| Source | Status (2 Nov 2026) | Verification |
| --- | --- | --- |
| UCLA-LoPucki BRD → **Florida-UCLA-LoPucki BRD** | The UCLA site redirects to `lopucki.law.ufl.edu`. The **Cases table is a free download** and the site says the link stays active indefinitely. Coverage: large public firms (≥ $100M assets in 1980 dollars), Ch. 7/11 cases filed Oct 1979 to **Dec 2022**. No further updates; LoPucki stopped work in 2022 (Law360, 23 Mar 2026). No successor found. | [ABS] of the UF home page, the UCLA redirect page and the Law360 preview. Download terms page not opened. |
| SEC 8-K **Item 1.03** (bankruptcy or receivership) | EDGAR FTS covers 2001 onward with a form-type filter. The FAQ does not document an item-number filter. edgartools exposes `eight_k.items` for filtering (post-parse). A phrase query "Item 1.03" plus the form 8-K filter is the practical route. **Live API call not possible here** (efts.sec.gov returned a proxy 403). Fills the post-2022 BRD gap and smaller filers. | [FT] FAQ, [FT] edgartools docs |
| **10-K major-customer disclosures** (ASC 280 / Reg S-K) | ASC 280 requires disclosing that a customer exceeds 10% of revenue. Customer *identity* is not required by ASC 280; SEC rules can require naming a customer whose loss would be material (Akamah 2016 says identity disclosure is "mandated by the SEC"). Firms often anonymise customers ("Customer A"). The standard derived database is **Compustat Segments customer file (commercial, WRDS)**; Huang et al. note it as an alternative to FactSet. **No 2025–2026 public dataset derived specifically from major-customer disclosures was found.** Nearest public resources are listed below this table. | [UNVERIFIED] for the exact ASC 280 vs Reg S-K wording (the Deloitte DART page did not render); [ABS] Akamah; [FT] Huang et al.; [ABS] the others |
| 17g-7(b) rating histories (I4 downgrades) | Each NRSRO must post XBRL rating histories (RoCR taxonomy) on its website. The files are public but per agency, and need issuer matching. | [ABS] SEC OCR page |

Nearest public resources for 10-K customer and supplier links:
- **FinReflectKG** on Hugging Face (`domyn/FinReflectKG`): 743 S&P 500 firms, 10-Ks from 2014 to 2024, about 17.5M triples, CC-BY-NC-4.0. It has a Supplies relation, plus Partners_With and Depends_On. There is no explicit Customer relation. [ABS of dataset card]
- Jackson et al. 2025 (JORS): RAG mapping of electronics supply chains (4,644 nodes) from 10-Ks and earnings calls. Whether the graph is released is unclear. [ABS]
- SEFD (Bettencourt et al., arXiv 2606.18192, Jun 2026): clean EDGAR markdown, about 152B tokens. It is a good extraction substrate, not a relation dataset. [ABS]
- Practical route: LLM extraction from 10-K Item 1 and the segment note ("customer accounted for X% of revenue"), validated on a small hand-labelled gold set. Anonymised customers will be lost, so coverage is biased toward large, named customers.

---

## BibTeX (new papers only)

```bibtex
@inproceedings{xu2025finripple,
  author = {Xu, Yuanjian and Hao, Jianing and Tang, Kunsheng and Chen, Jingnan and Liu, Anxian and Liu, Peng and Zhang, Guang},
  title = {FinRipple: Aligning Large Language Models with Financial Market for Event Ripple Effect Awareness},
  booktitle = {Findings of the Association for Computational Linguistics: ACL 2025},
  pages = {9377--9398}, year = {2025},
  url = {https://arxiv.org/abs/2505.23826},
  note = {[FT] arXiv v1 2025-05-28; Findings of ACL 2025}
}
@article{hertzel2008interfirm,
  author = {Hertzel, Michael G. and Li, Zhi and Officer, Micah S. and Rodgers, Kimberly J.},
  title = {Inter-firm linkages and the wealth effects of financial distress along the supply chain},
  journal = {Journal of Financial Economics}, volume = {87}, pages = {374--387}, year = {2008},
  url = {https://academicnewsletter.sufe.edu.cn/info/357307},
  note = {[ABS] 2008; JFE; BACKGROUND}
}
@article{alldredge2022effect,
  author = {Alldredge, Dallin M. and Chen, Yinfei and Liu, Steve Yu and Luo, Lan},
  title = {The Effect of Credit Rating Downgrades Along the Supply Chain},
  journal = {Review of Accounting and Finance}, volume = {21}, number = {1}, pages = {1--31}, year = {2022},
  url = {https://scholarsmine.mst.edu/bio_inftec_facwork/441},
  note = {[ABS] 2022; RAF; BACKGROUND}
}
@article{krzeczewska2022does,
  author = {Krzeczewska, Olga and Pastusiak, Rados{\l}aw},
  title = {Does bankruptcy filing always mean contagion? Evidence from industry rivals},
  journal = {International Journal of Finance \& Economics}, volume = {27}, number = {1}, pages = {1357--1366}, year = {2022},
  url = {https://international.vlex.com/vid/does-bankruptcy-filing-always-1049437335},
  note = {[ABS] 2022; IJFE; BACKGROUND}
}
@misc{umeorah2026temporal,
  author = {Umeorah, Nneka and Fadina, Tolulope},
  title = {A Temporal Multiplex Graph Neural Network for Systemic Risk Transmission in Global Banking},
  year = {2026}, eprint = {2608.27295}, archivePrefix = {arXiv},
  url = {https://arxiv.org/pdf/2608.27295},
  note = {[FT] arXiv 2026 (ID 2608); preprint}
}
@misc{inoue2024stock,
  author = {Inoue, Hiroyasu and Todo, Yasuyuki},
  title = {Stock Price Responses to Firm-Level News in Supply Chain Networks},
  year = {2024}, eprint = {2409.06255}, archivePrefix = {arXiv},
  url = {https://arxiv.org/abs/2409.06255v5},
  note = {[ABS] v1 2024-09-10, v5 2026-01-14; preprint}
}
@article{lee2026intraindustry,
  author = {Lee, Dahye and Byun, Sanghyuk and Kim, Min},
  title = {Intra-industry information transfer: evidence from accumulated confirmatory earnings news by peers},
  journal = {Asia-Pacific Journal of Accounting and Economics}, year = {2026},
  url = {https://scholarworks.sogang.ac.kr/item/98e3524f-4cd4-4e2e-b879-7618be22b008},
  note = {[ABS] early access Mar 2026; APJAE}
}
@article{koo2017earnings,
  author = {Koo, David S. and Wu, J. Julie and Yeung, P. Eric},
  title = {Earnings Attribution and Information Transfers},
  journal = {Contemporary Accounting Research}, volume = {34}, number = {3}, pages = {1547--1579}, year = {2017},
  url = {https://academicnewsletter.sufe.edu.cn/info/411313},
  note = {[ABS] 2017; CAR; BACKGROUND}
}
@misc{li2025structured,
  author = {Li, Gang and Qiao, Dandan and Zheng, Mingxuan},
  title = {Structured Event Representation and Stock Return Predictability},
  year = {2025}, eprint = {2512.19484}, archivePrefix = {arXiv},
  url = {https://arxiv.org/pdf/2512.19484},
  note = {[FT] (first 100k chars) 2025-12-22; preprint}
}
@inproceedings{murjani2026zerocopy,
  author = {Murjani, Kabir},
  title = {Zero-Copy Semantic Contagion: An In-Memory Streaming Architecture for Evolving Attention Graphs},
  booktitle = {ACM SIGMOD Workshop on Data Management for the Modern Financial Systems (FinDS)}, year = {2026},
  url = {https://arxiv.org/html/2606.05733},
  note = {[FT] arXiv 2606.05733, 2026; SIGMOD FinDS workshop}
}
@article{hu2026tariff,
  author = {Hu, Wenyao and Wang, Heng Emily and Han, Yue},
  title = {Tariff exposure and liberation day reactions: Initial evidence from corporate filings},
  journal = {Economics Letters}, volume = {259}, pages = {112787}, year = {2026},
  url = {https://scholarlyworks.adelphi.edu/esploro/outputs/journalArticle/Tariff-exposure-and-liberation-day-reactions/991004547559806266},
  note = {[ABS] 2026-02-01; Economics Letters}
}
@techreport{benecchi2025us,
  author = {Benecchi, Andrea and Borin, Alessandro and Conteduca, Francesco Paolo and Leone, Fabrizio and Mancini, Michele and Modugno, Lucia and Mongardini, Matteo and Papini, Giulio and Zoi, Patrick},
  title = {US tariffs and Italian firms: mapping network exposure and early impact from survey data},
  institution = {Bank of Italy}, type = {Questioni di Economia e Finanza}, number = {993}, year = {2025},
  url = {https://ideas.repec.org/p/bdi/opques/qef_993_25.html},
  note = {[ABS] 2025; Bank of Italy occasional paper}
}
@techreport{garimella2025market,
  author = {Garimella, Rohit and Kwan, Simon and Mertens, Thomas},
  title = {Market Reactions to Tariff Announcements},
  institution = {Federal Reserve Bank of San Francisco}, type = {Economic Letter}, number = {2025-23}, year = {2025},
  url = {https://www.frbsf.org/research-and-insights/publications/economic-letter/2025/10/market-reactions-to-tariff-announcements/},
  note = {[FT] 2025-10-06; FRBSF Economic Letter}
}
@techreport{huang2019trade,
  author = {Huang, Yi and Lin, Chen and Liu, Sibo and Tang, Heiwai},
  title = {Trade Networks and Firm Value: Evidence from the US-China Trade War},
  year = {2019}, institution = {NBER conference paper},
  url = {https://conference.nber.org/conf_papers/f130065.pdf},
  note = {[FT] first version 2018-05, this version 2019-09-10; BACKGROUND}
}
@misc{asgharian2024lender,
  author = {Asgharian, Hossein and Goodwin, Dominice and Liu, Lu},
  title = {Lender-Borrower Stock Return Dependencies in Credit Networks: Evidence from Syndicated Loan Deals},
  year = {2024}, howpublished = {International Risk Management Conference 2024},
  url = {https://lup.lub.lu.se/record/9d7ac81e-4b4e-4991-aaed-a6b7eecd14e0},
  note = {[ABS] 2024; conference paper, unpublished}
}
@article{broz2025collapse,
  author = {Bro{\v{z}}, V{\'a}clav and Tepl{\'y}, Petr},
  title = {From collapse to contagion: How bank failures influence stock markets},
  journal = {The North American Journal of Economics and Finance}, volume = {79}, year = {2025},
  url = {https://ideas.repec.org/a/eee/ecofin/v79y2025ics1062940825000841.html},
  note = {[ABS] 2025; NAJEF}
}
@article{pasricha2025structural,
  author = {Pasricha, P. and Selvamuthu, D. and Tardelli, P.},
  title = {Structural credit risk models with stochastic default barriers and jump clustering using Hawkes jump-diffusion processes},
  journal = {OPSEARCH}, volume = {62}, pages = {1061--1081}, year = {2025},
  url = {https://link.springer.com/article/10.1007/s12597-024-00830-9},
  note = {[ABS] 2025; OPSEARCH}
}
@misc{mori2026temporal,
  author = {Mori, Shintaro},
  title = {Temporal Coarse-Graining of Latent Default-Probability Paths Generates Effective Default Correlation},
  year = {2026}, eprint = {2606.12446}, archivePrefix = {arXiv},
  url = {https://arxiv.org/html/2606.12446},
  note = {[FT] arXiv 2026 (ID 2606); preprint; peripheral}
}
@misc{mori2026identifiability,
  author = {Mori, Shintaro},
  title = {Identifiability of Contagion Components amid Environmental Fluctuations in Aggregated Default Counts},
  year = {2026}, eprint = {2604.18118}, archivePrefix = {arXiv},
  url = {https://arxiv.org/html/2604.18118v3},
  note = {[FT] arXiv 2026 (ID 2604, v3); preprint; peripheral}
}
@misc{xiang2025graph,
  author = {Xiang, Ling and Hu, Quan and Zhang, Xiang and Lan, Wei and Liu, Bin},
  title = {Graph Neural Poisson Models for Supply Chain Relationship Forecasting},
  year = {2025}, eprint = {2508.12044}, archivePrefix = {arXiv},
  url = {https://arxiv.org/html/2508.12044v1},
  note = {[FT] arXiv 2025-08; preprint}
}
@inproceedings{chang2025learning,
  author = {Chang, Serina and Lin, Zhiyin and Yan, Benjamin and Bembde, Swapnil and Xiu, Qi and Wong, Chi Heem and Qin, Yu and Kloster, Frank and Luo, Xi and Palleti, Raj and Leskovec, Jure},
  title = {Learning Production Functions for Supply Chains with Graph Neural Networks},
  booktitle = {AAAI}, year = {2025},
  url = {https://cs.stanford.edu/people/jure/pubs/supplychain-gnn-aaai25.pdf},
  note = {[FT] AAAI-25}
}
@article{bao2026trade,
  author = {Bao, Xiaohua and Deng, Jianpeng and Lu, Yue and Wei, Yaning and Zhang, Jie},
  title = {Trade Tensions and the Reconfiguration of Supply Chain Linkages: Evidence From U.S. Listed Firms},
  journal = {The World Economy}, year = {2026}, doi = {10.1111/twec.70117},
  url = {https://www.citedrive.com/en/discovery/trade-tensions-and-the-reconfiguration-of-supply-chain-linkages-evidence-from-us-listed-firms/},
  note = {[ABS] 2026; The World Economy (ISSN 0378-5920)}
}
@techreport{crosignani2025securing,
  author = {Crosignani, Matteo and Han, Lina and Macchiavelli, Marco and Silva, Andr{\'e} F.},
  title = {Securing Technological Leadership? The Cost of Export Controls on Firms},
  institution = {Federal Reserve Bank of New York}, type = {Staff Report}, number = {1096}, year = {2025},
  url = {https://www.newyorkfed.org/medialibrary/media/research/staff_reports/sr1096.pdf?sc_lang=en},
  note = {[ABS] first Apr 2024, revised Feb 2025; NY Fed Staff Report}
}
@article{du2025enterprise,
  author = {Du, Xinke and Cao, Jinfei and Jiang, Xiyuan and Duan, Jianyu and Tian, Zhen and Wang, Xiong},
  title = {Enterprise bankruptcy prediction model based on heterogeneous graph neural network for fusing external features and internal attributes},
  journal = {Mathematics}, volume = {13}, number = {17}, pages = {2755}, year = {2025},
  url = {https://eprints.gla.ac.uk/363230},
  note = {[ABS] 2025-08-27; Mathematics (MDPI)}
}
@article{zhang2026multiperspective,
  author = {Zhang, Zhipeng and Pang, Jiaxin and Li, Gang},
  title = {Multi-Perspective Fusion Graph Model for Financial Distress Prediction of Listed Companies},
  journal = {Information Systems Frontiers}, year = {2026}, doi = {10.1007/s10796-025-10689-w},
  url = {https://www.springerprofessional.de/en/multi-perspective-fusion-graph-model-for-financial-distress-pred/51996870},
  note = {[ABS] online 2026-01-29; ISF}
}
@misc{zhu2025multichannel,
  author = {Zhu, Jianyu},
  title = {Multi-Channel Graph Neural Network for Financial Risk Prediction of NEEQ Enterprises},
  year = {2025}, eprint = {2507.12787}, archivePrefix = {arXiv},
  url = {https://arxiv.org/abs/2507.12787},
  note = {[ABS] 2025-07-17; preprint}
}
@misc{yilki2026supply,
  author = {Y{\i}lk{\i}, Asef},
  title = {Supply Chain Propagation of Textual Signals: LLM Embeddings and Cross-Sectional Return Predictability},
  year = {2026}, eprint = {2606.29290}, archivePrefix = {arXiv},
  url = {https://www.alphaxiv.org/abs/2606.29290},
  note = {[ABS] 2026-06-28; preprint}
}
@article{mehta2021shadow,
  author = {Mehta, Mihir N. and Reeb, David M. and Zhao, Wanli},
  title = {Shadow Trading},
  journal = {The Accounting Review}, volume = {96}, number = {4}, pages = {367--404}, year = {2021},
  url = {https://iris.unibocconi.it/handle/11565/4040993},
  note = {[ABS] 2021; TAR; BACKGROUND}
}
@misc{golemac2025shadow,
  author = {Golemac, Angela and Nguyen, Nguyet and Li, Yingzhen},
  title = {Shadow trading: Economic evidence and potential implications from recent SEC insider trading cases},
  year = {2025}, howpublished = {Westlaw Today (The Brattle Group)},
  url = {https://www.brattle.com/wp-content/uploads/2025/11/Shadow-Trading-Economic-Evidence-and-Potential-Implications-from-Recent-SEC-Insider-Trading-Cases.pdf},
  note = {[FT] 2025-11-17; practitioner article}
}
@article{jackson2025supply,
  author = {Jackson, Ilya and Sa{\'e}nz, Maria Jesus and Ivanov, Dmitry and Ma, Benedict Jun},
  title = {Supply chain mapping through retrieval-augmented generation: applications to the electronics industry},
  journal = {Journal of the Operational Research Society}, year = {2025},
  url = {https://dspace.mit.edu/handle/1721.1/165214},
  note = {[ABS] 2025-12-19; JORS (data source check)}
}
@misc{bettencourt2026stanford,
  author = {Bettencourt, Nick and Ding, Xiaowei and Giesecke, Kay},
  title = {The Stanford EDGAR Filings Dataset: Reconstructing U.S. Corporate and Financial Disclosures into Layout-Faithful and Token-Efficient Pretraining Data},
  year = {2026}, eprint = {2606.18192}, archivePrefix = {arXiv},
  url = {https://papers.cool/arxiv/2606.18192},
  note = {[ABS] 2026-06-16; preprint (data source check)}
}
@phdthesis{akamah2016strategic,
  author = {Akamah, Herita},
  title = {Strategic Non-disclosure of Major Customer Identity},
  school = {University of Oklahoma}, year = {2016},
  url = {https://shareok.org/items/8a538efc-6bf0-4462-bee8-ae2b5995f5b3/full},
  note = {[ABS] 2016; dissertation; BACKGROUND (data source check)}
}
```

Data and documentation pages opened (not papers):
- Florida-UCLA-LoPucki BRD home: https://www.lopucki.law.ufl.edu/index.php [ABS]
- UCLA redirect page: https://lopucki.law.ucla.edu/ [ABS]
- Law360, 23 Mar 2026: https://www.law360.com/articles/2456296 [ABS, paywalled preview]
- EDGAR FTS FAQ: https://www.sec.gov/edgar/search/efts-faq.html [FT]
- edgartools 8-K docs: https://edgartools.readthedocs.io/en/latest/eightk-filings/ [FT]
- SEC OCR 17g-7 page: https://www.sec.gov/about/divisions-offices/office-credit-ratings/disclosure-of-credit-rating-histories [ABS]
- FinReflectKG dataset card: https://huggingface.co/datasets/domyn/FinReflectKG/blob/main/README.md [ABS]
- Mendeley AI&SupplyChain dataset (Chinese data, not relevant): https://data.mendeley.com/datasets/pxrs7cgvhn/1 [ABS]

Opened but not usable:
- Özden 2026 (BMIJ): the record page had no abstract.
- Springer RAST s11142-026-09954-3: the page failed to load.
- Deloitte DART ASC 280: the page did not render.

## Query log

| # | Query | Tool | Outcome |
| --- | --- | --- | --- |
| 1 | bankruptcy filing customer supplier stock returns contagion 2025 | WebSearch | Hertzel 2008 (bkg) |
| 2 | machine learning predict peer firms stock reaction bankruptcy announcement | WebSearch | nothing relevant |
| 3 | credit rating downgrade spillover rivals suppliers equity returns 2026 | WebSearch | Alldredge 2022 |
| 4 | intra-industry information transfer earnings announcements 2025 peer firms | WebSearch | Frankel 2025 (known), Lee 2026, RAST 2026 (failed) |
| 5 | graph neural network earnings announcement spillover peer stock returns | WebSearch | nothing new |
| 6 | event-driven link prediction stock relations earnings surprise arXiv 2026 | WebSearch | huang2026 (known), Li 2025 |
| 7 | peer earnings news contagion competition effect sign prediction stock returns 2026 | WebSearch | Koo 2017 (bkg) |
| 8 | Avramov dual peer effects cross-stock predictability | WebSearch | Cambridge WP (known) |
| 9 | "information transfer" Review of Accounting Studies 2026 earnings announcement peers | WebSearch | Das 2026 (known) |
| 10 | deep learning which stocks react to other firm's news cross-firm news impact arXiv 2025 | WebSearch | Inoue & Todo; Murjani 2026 |
| 11 | temporal graph network firm events abnormal returns neighbors prediction 2025 | WebSearch | nothing relevant |
| 12 | Chapter 11 announcement competitors contagion competitive effect stock 2025 study | WebSearch | Krzeczewska 2022 |
| 13 | default contagion equity network listed firms graph neural network 2026 arXiv | WebSearch | Umeorah 2026; Özden 2026 |
| 14 | customer bankruptcy supplier stock price reaction supply chain disclosure 2026 | WebSearch | nothing new |
| 15 | Florida-UCLA-LoPucki Bankruptcy Research Database download access 2026 | WebSearch | BRD UF site; Law360 2026 |
| 16 | EDGAR full-text search 8-K Item 1.03 bankruptcy or receivership | WebSearch | raw 8-K filings |
| 17 | 10-K major customer disclosures dataset public supply chain network EDGAR 2025 | WebSearch | Jackson 2025 JORS |
| 18 | SEC EDGAR full-text search FAQ coverage since 2001 filter by form type | WebSearch | EFTS FAQ |
| 19 | 8-K items dataset EDGAR item 1.03 Python download items field submissions API | WebSearch | edgartools docs |
| 20 | efts.sec.gov search-index "Item 1.03" forms=8-K | curl | blocked (proxy 403) |
| 21 | open dataset customer-supplier links extracted from 10-K filings LLM release 2026 | WebSearch | SEFD 2026; kose2026 (known); Mendeley |
| 22 | "10 percent" major customer disclosure SFAS 131 customer identity omission study | WebSearch | Akamah 2016; DART |
| 23 | FinReflectKG Hugging Face dataset S&P 100 10-K supplier customer triples release | WebSearch | HF card |
| 24 | Compustat segments customer file alternative free major customers 10-K text extraction | WebSearch | arXiv 2605.23924 (segments only, not customers) |
| 25 | Liberation Day tariffs stock returns supply chain network exposure indirect 2026 | WebSearch | news only |
| 26 | April 2025 tariff announcement European firms abnormal returns production network | WebSearch | FRBSF 2025; BdI QEF 993; Hu 2026 |
| 27 | tariff shock higher-order propagation firm-level input-output stock market reaction NBER 2025 | WebSearch | Huang et al. 2019; IMF 2026 (known) |
| 28 | tariffs 2025 customer supplier links stock returns indirect exposure FactSet Revere event study | WebSearch | Barbiero (known) |
| 29 | arXiv 2025 tariff graph neural network stock exposure supply chain knowledge graph | WebSearch | Yılkı 2026; TAGN (simulated tariff) |
| 30 | shared lender contagion borrower stock returns bank distress spillover listed firms 2025 | WebSearch | Asgharian 2024; Brož 2025 |
| 31 | heterogeneous graph transformer relation types credit risk listed companies supply chain shareholder guarantee 2025 | WebSearch | Chinese private-data papers; SME-HGT |
| 32 | credit agreements EDGAR exhibit 10 lender borrower network public dataset 2025 | WebSearch | raw Exhibit 10; payday-lender data project |
| 33 | multilayer firm network ownership supply chain lending which layer explains stock return spillovers 2026 | WebSearch | majhi2026 (known); Gofman 2017 |
| 34 | multivariate Hawkes equity jumps credit events contagion network firms arXiv 2025 | WebSearch | zelvyte (known); Pasricha 2025 |
| 35 | self-exciting point process corporate defaults rating downgrades clustering 2026 | WebSearch | Mori 2026 ×2 |
| 36 | neural temporal point process stock crash contagion supply chain graph Hawkes | WebSearch | Murjani 2026 |
| 37 | supply chain link prediction graph neural network firm network 2025 temporal | WebSearch | Xiang 2025; Chang AAAI 2025 |
| 38 | predicting supplier relationship termination decoupling China US firms stock returns | WebSearch | Bao 2026 |
| 39 | export controls supply chain reconfiguration firm-level links priced equity 2026 paper | WebSearch | Crosignani 2025 |
| 40 | customer financial distress predicts supplier bankruptcy prediction network features | WebSearch | older trade-credit papers |
| 41 | graph neural network bankruptcy prediction listed US firms 10-K text supply chain neighbors 2026 | WebSearch | Zhu 2025; Zhang ISF 2026; Du 2025 |
| 42 | economically linked firms textual tone spillover crash risk customer 10-K | WebSearch | Macau seminar 2024; Ge et al. (older) |
| 43 | shadow trading economically linked firms insiders empirical evidence 2025 Panuwat | WebSearch | Mehta 2021; Brattle 2025 |
| 44 | Form 4 insider trades graph anomaly detection machine learning 2026 | WebSearch | Jaeger (known) |
| 45 | opportunistic insider trading detection heterogeneous temporal graph insiders companies | WebSearch | Jaeger, Stenfors (known) |
| 46 | OpenReview API: 5 terms (bankruptcy contagion stock; earnings announcement peer firms; credit event equity graph; supply chain link prediction; insider trading graph) | curl | blocked (proxy 403) |
| 47 | ICAIF 2025 predicting stock reactions to related company events knowledge graph | WebSearch | **FinRipple** (ACL Findings 2025) |
| 48 | rating downgrade announcement contagion competitive effects rivals machine learning heterogeneity | WebSearch | pre-2016 event studies only |
| 49 | which peers respond to earnings news customer supplier competitor sign economic links 2025 working paper | WebSearch | pre-2018 accounting work |
| 50 | SEC Rule 17g-7 rating history XBRL files download corporate issuer ratings dataset | WebSearch | SEC OCR page |
