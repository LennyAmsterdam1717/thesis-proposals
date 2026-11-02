# Deep dive: Idea 3 — "When do influence links between stocks switch on, off or flip?"

*Signed lead-lag edge-state forecasting. Prepared 2 November 2026 for Frank's topic proposal. Written to be readable without a finance or graph-ML background.*

**Verification legend** (same as the rest of this repo):
**[FT]** full text (or most of it) opened · **[ABS]** abstract / record page opened · **[SNIP]** only seen as a search-result excerpt, not opened — do not rely on it before checking · **[BG]** older background work (not evidence of what is open or closed in 2025–2026).
Numbers quoted from papers must be re-checked against the PDF before they go into the thesis.

---

## 0. The short version (read this if nothing else)

- **The idea.** Each month, build a map of the stock market in which an arrow A → B means "when A moves, B follows a few days later". Each arrow is **+** (B moves the same way) or **−** (B moves the opposite way). Then predict, for every arrow, whether next month it will **stay, appear, disappear or flip sign**.
- **Is the phenomenon real?** Yes. Finance research shows that lead-lag effects between stocks exist and can be used for trading (Cartea, Cucuringu & Jin 2023; Breitung & Müller, *JFE* 2025; DeltaLag, ICAIF 2025). Several studies also show that these relations **change over time** (Bennett et al. 2022; Cartea et al. 2023; DeltaLag 2025). Nobody has turned that change into a prediction task. That is the gap.
- **Closest work, and an important update.** The closest paper (Krstev et al.) was a *withdrawn* ICLR submission when the scouting run checked it. It has since been **published in the journal *Machine Learning* (July 2026)**, with public code and data. It treats lead-lag as temporal link prediction. It still differs from Idea 3 in four ways:
  - it uses 37 assets;
  - edges come from a fixed 5% threshold with a 1-day lag, with no statistical test;
  - the sign is not predicted as its own outcome;
  - there is no persistence ("nothing changes") baseline.

  **Idea 3 is still open, but the proposal must cite this paper as the starting point.**
- **Methods exist but were never applied to stocks.** 2026 models for *dynamic signed networks* (IDP-DSN, SIGIR 2026, code available; HCIM 2026) predict exactly "positive / negative / no link". They are only tested on Bitcoin trust ratings, Wikipedia votes and Reddit.
- **Data.** Daily prices and a historical S&P 500 member list are enough. Both are free.
  - **New finding:** UU's WRDS list (the finance data hub of the Utrecht School of Economics) shows Compustat and Fama–French but **not CRSP**.
  - So the "ask for CRSP" plan probably fails. Ask instead about **LSEG Workspace or FactSet** (both available at UU via financedata@uu.nl) for prices of delisted firms.
- **Biggest risk.** At *daily* frequency, lead-lag effects between big US stocks are weak and noisy. A careful analysis on 2017–2026 data from a practitioner's GitHub project (not peer-reviewed) found the direction of strong pairs repeats only about 50% of the time year to year.

  That makes forecasting hard. It also makes the question **interesting**: *can anything beat "nothing changes"?* The plan has fallbacks (§7).

---

## 1. The idea in plain words

Stocks don't move independently. Sometimes **one stock moves first and another follows**:

- Oil price up, so **Shell rises today**, so an oil-equipment supplier rises two days later. That is a **positive (+)** link.
- **Shell rises today**, so airline stocks fall two days later, because fuel gets more expensive. That is a **negative (−)** link.

These links are not permanent. They switch **on** in some periods, switch **off** in others, and can **flip** sign. Your supervisor described exactly this in your meeting notes: *"edges in the graph are signed and activated (negatively or positively)."*

**What the thesis does:**

1. **Monthly maps.** Every month, test every pair of stocks on the past ~6 months of daily returns. Keep only links that pass a statistical test. Each link records leader → follower, sign (+/−) and lag (1–5 days).
2. **A film of maps.** Line the monthly maps up in time order.
3. **Predict the next map.** For each pair, predict next month's state: *+, −, or no link*. Score separately how well you predict **births** (link switches on), **deaths** (switches off) and **flips** (+ ↔ −).
4. **The honest bar.** Compare everything against "next month = this month" (persistence), because that rule is right most of the time.

---

## 2. Does the phenomenon exist? (papers that back the idea up)

### 2a. Lead-lag effects between stocks are real and economically meaningful

| Paper | What it shows | Why it matters for you | Check |
|---|---|---|---|
| Cartea, Cucuringu & Jin (2023), *Detecting Lead-Lag Relationships in Stock Returns and Portfolio Strategies*, SSRN 4599565 | Ranks ~500 US stocks from leaders to followers (1963–2022) with cross-correlation and Lévy-area measures. Lead-lag portfolios earn >20% a year. **Leader/follower roles change several times over the decades.** | Lead-lag is real, **and it changes over time**, which is the premise of your thesis | [SNIP] via Oxford seminar page and secondary summaries; also in Qi Jin's 2025 Oxford DPhil thesis [SNIP] |
| Breitung & Müller (2025), *Global Business Networks*, *Journal of Financial Economics* 166, 104007 | Builds time-varying firm networks from LLM-generated business descriptions. Stocks whose linked firms did well last month earn significant alphas (US and global) | Economic links create lead-lag; "related-firm" text features can be inputs for your model | [ABS] (publisher page read) |
| Zhou et al. (2025), *DeltaLag*, ICAIF 2025, arXiv 2511.00390 | Deep model that learns which stock leads which, with a pair-specific, time-varying lag. Beats fixed-lag and precomputed statistical lead-lag graphs | Says explicitly that the classic assumption of **persistent** lead-lag patterns is "often invalid". It uses edges to predict returns, not to forecast the edges themselves | [FT] (repo lit_review) |
| Li & Ferreira (2025), *Follow the Leader: … Network Momentum*, arXiv 2501.07135 | Turns lead-lag between markets into a learned graph ("network momentum") that improves trend-following | Shows a lead-lag *network* is a useful object; edges are recomputed every period | [ABS] |
| Bennett, Cucuringu & Reinert (2022), *Lead-lag detection and network clustering…*, *Machine Learning* (arXiv 2201.08283) [BG] | Directed lead-lag networks of US stocks. Year-to-year cluster similarity is **low**. Cites earlier work finding individual relations last **about a year** | Persistence is limited, so change is the norm and worth forecasting | [SNIP] (PDF excerpt) |
| Curme et al. (2015), *Emergence of statistically validated financial intraday lead-lag relationships*, *Quantitative Finance* [BG] | The method for **statistically validating** each lagged link, with correction for testing all pairs at once | Your edge-construction recipe. Warning: their significant links mostly vanish at horizons above one hour (intraday data) | [SNIP] |
| Souza & Aste (2018/19), *Predicting future stock market structure…*, arXiv 1812.01103 [BG] | Predicts insertion and removal of edges in a stock correlation network. **~40% of edges change within 20 weeks** | Early proof that "predict the next market network" is a legitimate task. Unsigned, undirected, no modern GNNs | [SNIP] |

### 2b. Signs, triangles and "balance" in stock networks (useful for features and for variant B)

| Paper | What it shows | Check |
|---|---|---|
| Qing & Xu (2025/2026), *Finding Core Balanced Modules in Statistically Validated Stock Networks*, arXiv 2508.04970, published in *Expert Systems with Applications* 2026 | Builds **statistically validated signed** stock networks and finds balanced groups ("every triangle is consistent"). Their size jumps in crises and their make-up rotates every year | [ABS] |
| Dahake & Chakraborty (2026), arXiv 2608.12023 | S&P 500 signed networks lose **structural balance** in crises, mainly *between* sectors | [ABS] |
| Bartesaghi et al. (2025), *Global balance and systemic risk…*, *Physica A* 674, 130698 (code on GitHub) | The balance of a signed correlation network works as a systemic-risk indicator | [SNIP] (PDF excerpt) |

**Takeaway:** signs and triangle balance carry information in market networks. So far they are used only *descriptively* and on *same-day* correlations, not as features to **forecast** directed, lagged edges.

---

## 3. Closest work, and exactly how Idea 3 differs

| Paper | What they did | What they did *not* do (= your space) |
|---|---|---|
| **Krstev, Rigoni, Mishkovski, Pasa et al. (2026)**, *A Temporal Graph Learning Framework for Lead-Lag Detection in Financial Markets*, ***Machine Learning* 115, art. 173**, published online 8 July 2026. Code: github.com/ivankrstev7/temporal_graph_lead_lag_detection; data: github.com/ivankrstev7/Temporal_Graphs_Lead_Lag_Data [FT] | Lead-lag as **temporal link prediction**: 37 assets (29 stocks + 8 commodities), daily, June 2019 – June 2024. Edge = both assets move more than 5% in the same direction with a 1-day lag. Temporal GNNs (TGN, TGAT, GraphMixer…) plus LSTM/AR/ARIMA. GraphMixer best (AP ≈ 0.79). One dataset variant has + and − relations | Only 37 assets. Threshold edges with **no significance test**. Fixed 1-day lag. **Sign not predicted as its own class.** **No persistence/EdgeBank baseline.** Births, deaths and flips not scored separately. Authors list larger datasets and interpretability as future work. *(Earlier repo notes call this paper "withdrawn ICLR submission". That status is outdated; update `references.bib`.)* |
| Zhou et al. (2025), DeltaLag, ICAIF 2025 [FT] | Learns dynamic leader → follower pairs and lags to **predict returns** | Edges are a means, not the target. Unsigned. No test of edge stability |
| Fanshawe, Masih & Cameron (2026), arXiv 2601.04602 [FT] | Forecasts 10-day-ahead **signed** correlations for S&P 500 with a Transformer + graph attention model | Undirected, same-day (no leader/follower, no lag). Forecasts a number, not on/off/flip events |
| Hou et al. (2026), **IDP-DSN**, SIGIR 2026, arXiv 2604.18213. Code: github.com/codewhisperer-rs/IDP-DSN [FT] | Predicts **positive / negative / no edge** in dynamic signed networks. Strong gains over SEMBA, PolarDSN, TGN… | Tested only on BitcoinAlpha, BitcoinOTC, Wiki-RfA, Epinions. **No persistence baseline.** Never on market data |
| Regier et al. (2026), **HCIM**, arXiv 2605.26290 [ABS] | Adds temporal memory to a signed graph transformer for dynamic signed link prediction | Bitcoin OTC/Alpha, Reddit, synthetic only. No code mentioned |
| Zhang et al. (2026), *Dual-Channel Feature Fusion for Joint Prediction in Dynamic Signed Weighted Networks*, arXiv 2602.12663 [ABS] | Jointly predicts link, sign and weight, using structural-balance and temporal-difference features | Simulation experiments only (no datasets named). A good *design idea* for your model (balance features) |

**Novelty statement you can use in the proposal:**

> Lead-lag relations between stocks are known to exist and to change over time, and 2026 work treats lead-lag detection as temporal link prediction on a small asset set (Krstev et al., 2026). However, no study forecasts the future **state** of **statistically validated, signed and directed** lead-lag edges, distinguishing births, deaths and sign flips and comparing against persistence, on a realistic equity universe. Dynamic signed-network models (IDP-DSN, HCIM, 2026) that predict exactly such states have only been evaluated on social and trust networks.

---

## 4. Lessons from temporal-graph research that make your evaluation stronger

- **Always report persistence.** A 2026 dynamic-GNN paper (Roy & Dasgupta, *SiST-GNN*, arXiv 2605.25548 [FT]) found that simply copying the last snapshot **beat every learned model** on one benchmark (UCI-Message). The authors recommend reporting persistence alongside every result. The memorisation baseline **EdgeBank** (Poursafaei et al. 2022 [BG]) makes the same point.
- **The same thing happened on fund-holdings data.** Izadifar & Rahmati (2026, arXiv 2607.12067 [FT]) found "same as last quarter" scores 0.889 against 0.913 for the best model, while the other GNNs scored far lower. Your design avoids this trap by scoring **changes** separately.
- **Neither signed-network paper (IDP-DSN, HCIM) includes a persistence baseline.** Adding one is a small but genuine methodological contribution.

---

## 5. Data: what you need and where to get it

| What | Source | Cost / access | Notes |
|---|---|---|---|
| **Daily prices** of US stocks | **Stooq** bulk download (zipped CSVs per region) | Free | Easy for current stocks; coverage of **delisted** firms is incomplete (check) |
| | `yfinance` (unofficial Yahoo API) | Free | Mostly current tickers; terms of use unclear; fine for prototyping |
| | **LSEG Workspace** or **FactSet** via UU (email financedata@uu.nl) | Free for UU students (check eligibility) | Likely the best UU route to **delisted** prices. Ask your supervisor / the finance data team |
| | WRDS at UU | Free for UU accounts | UU's WRDS list shows **Compustat, Fama–French factors, S&P Dow Jones Indices…, but not CRSP** (uufinance.github.io/data/wrds/databases/, opened 2 Nov 2026). The S&P Dow Jones Indices entry *might* give historical index membership |
| | Commercial archive (e.g. marketparquet.com: 16,000+ US stocks incl. 8,500 delisted, daily since 2000) | ~$79 one-off (vendor claim, [SNIP]) | Cheap fallback if UU has no delisted prices |
| **Who was in the S&P 500 on each date** (avoids survivorship bias) | GitHub **fja05680/sp500**: historical members since 1996, updated to Aug 2026 (repo checked) | Free | Built from Wikipedia change lists; spot-check a few dates |
| **Factor returns** (to remove the market/sector effect before testing links) | Kenneth French data library; also on UU's WRDS | Free | Using residual returns stops every link from being "the whole market moves together" |
| **Sector codes / text relations** (optional features) | SIC codes from SEC EDGAR; Hoberg–Phillips industry data (listed on UU's finance data hub) | Free | For "are these two firms related?" features |
| **Ready-made lead-lag dataset** (for replication) | Krstev et al. data repo: 37 assets, daily 2019–2024, with prices, indicators and sentiment | Free (GitHub) | Use to **reproduce their result first**, then show what changes with statistical edges, signs and persistence |
| **Signed temporal benchmark datasets** (for checking your models work) | BitcoinAlpha, BitcoinOTC, Wiki-RfA, Epinions (as used by IDP-DSN) | Free | Sanity check: reproduce IDP-DSN's numbers before applying it to stocks |

**Practical plan:** prototype on free data (Stooq/yfinance plus the historical S&P 500 list). In parallel, ask about LSEG or FactSet access for delisted prices. Report honestly how much survivorship matters, for example by comparing "current members only" with "point-in-time members".

---

## 6. Methods and code you can build on

**Step 1. Build the monthly signed lead-lag maps**
- Returns → remove market/sector factors (Fama–French) → lagged cross-correlation for lags 1–5 days on rolling ~120-day windows.
- **Statistical test per pair plus multiple-testing correction** (false discovery rate). Recipes: Curme et al. 2015 [BG]. For a more advanced approach, Uematsu & Yamagata (2025, *JASA*) on Granger networks with FDR control [SNIP].
- Alternatives to compare: Granger tests (`statsmodels`), transfer entropy, Lévy area (Cartea et al.).
- Output per window: arrow i → j with **sign**, **lag** and strength.

**Step 2. Labels**
- For each pair, next window's state: +, − or none. Classify each change as persist, birth, death or flip.

**Step 3. Models, from simple to advanced**
1. Persistence ("same as now"), and "how often was this link on before" (activation frequency).
2. Triangle-balance rule: predict the sign that makes triangles consistent.
3. Logistic regression / gradient boosting on pair features (sector, size, past states, balance, lag consistency).
4. Temporal GNNs (TGN, DyGFormer…) via the **TGM** library (Chmura et al. 2025, github.com/tgm-team/tgm), which is faster than DyGLib and handles monthly snapshots.
5. Dynamic **signed** GNNs: **IDP-DSN** (public code). Krstev et al.'s code gives a working lead-lag temporal-GNN pipeline to start from.

**Step 4. Evaluation**
- Strict time order, e.g. train 2005–2018, validation 2019–2020, test 2021–2025, with no information from the future in any window.
- Metrics per change type (macro-F1 / AUC for births, deaths, flips), always next to persistence.
- Optional economic check: does using forecast links instead of persistent links improve hedging or portfolio risk?

**Explainability (your XAI angle):** for each predicted flip or birth, show *which* neighbouring links and features drove it, for example with GNN explainers or the interpretable signed transformer used in HCIM. This feeds the dashboard ("influence-edge monitor").

---

## 7. Honest risks and how the plan handles them

| Risk | Evidence | Mitigation / fallback |
|---|---|---|
| **Daily lead-lag between large US stocks is weak** | Curme et al. 2015 [BG]: intraday lead-lag links mostly vanish beyond one hour. A 2026 practitioner analysis (GitHub *portfolio-lab*, PR #17, not peer-reviewed [SNIP]) found strongest-pair directions repeat about 50% of the time year to year and no tradable daily effect | (a) Use factor-residual returns and FDR so the surviving edges are real. (b) Also try **weekly** returns. (c) Fallback: **same-day signed residual-correlation edges**, which are denser, with the same pipeline (cf. Fanshawe 2026, Qing & Xu 2026). (d) Include smaller stocks (S&P 1500): the classic finding is that large stocks lead small ones (Lo & MacKinlay 1990 [BG]), so a broader universe may give more real edges |
| Changes are close to unpredictable | Same as above | Still a clean result: "flips/births are (un)predictable beyond persistence". The **signed temporal stock benchmark** is a contribution in itself |
| Survivorship bias | Free price data misses delisted firms | Point-in-time S&P list plus LSEG/FactSet via UU, or a cheap commercial archive. Report the effect |
| Overlapping windows make labels correlated (look-ahead) | Standard pitfall | Non-overlapping test windows; build labels only from data after the prediction date |
| Many tests → false links | 500 stocks ≈ 250,000 ordered pairs × 5 lags per window | FDR control (Benjamini–Hochberg), as in statistically validated networks |
| Compute | Cheap for statistics; GNNs moderate | Monthly snapshots of ~500 nodes are small. Start with S&P 100 if needed |

---

## 8. What changes compared with Card 3 in `topic_shortlist.md`

1. **Krstev et al. is now a peer-reviewed journal paper** (*Machine Learning*, July 2026) with code and data. Position the thesis as *extending* it: statistical edges, signs as states, realistic universe, persistence baselines, change-specific scoring. Update `references.bib`.
2. **CRSP is probably not available at UU** (not on the UU WRDS list). Ask about LSEG Workspace / FactSet instead.
3. **New supporting literature:** Breitung & Müller (JFE 2025), Li & Ferreira (2025), Qing & Xu (ESWA 2026), Bartesaghi et al. (Physica A 2025), SiST-GNN (2026, persistence lesson), Zhang et al. (2026, joint link/sign/weight).
4. **The risk is now sharper:** evidence that *daily* lead-lag in large caps is weak. That is why the weekly and same-day fallbacks are part of the plan from the start.

---

## 9. Suggested proposal text (short)

**Working title:** *When do influence links switch? Forecasting the state of signed lead-lag edges in stock networks.*

**Research question:** Can the future state (positive, negative, absent) of statistically validated lead-lag edges between stocks be predicted better than persistence, in particular births, deaths and sign flips? Which signals (edge history, triangle balance, sector and text relations) carry that predictability?

**Sub-questions:**
1. How stable are signed lead-lag edges over time, and how often do they appear, disappear and flip?
2. Do dynamic signed graph neural networks (e.g. IDP-DSN) and temporal GNNs beat persistence, activation-frequency and balance baselines on these changes?
3. *(Optional)* Do better edge forecasts improve out-of-sample hedging or portfolio risk?

**Contributions:**
1. The first signed, directed, statistically validated temporal stock-network benchmark with change-specific evaluation.
2. The first test of dynamic signed GNNs on financial data, against persistence.
3. An interpretable "influence-edge monitor" dashboard.

**Questions for the supervisor:**
1. Is "forecasting edge states" what she meant by signed, activated edges, or did she mean learning such edges inside a return-forecasting model (more crowded)?
2. Is a benchmark plus careful negative result acceptable if GNNs don't beat persistence?
3. Is data access for delisted prices possible (LSEG / FactSet via UU)?
4. Daily vs weekly frequency, S&P 500 vs S&P 1500?

---

## Sources (opened or seen in this deep dive, 2 Nov 2026)

- Krstev et al. 2026, *Machine Learning* 115:173: https://link.springer.com/article/10.1007/s10994-026-07113-y [FT]
- Krstev data / code: https://github.com/ivankrstev7/Temporal_Graphs_Lead_Lag_Data · https://github.com/ivankrstev7/temporal_graph_lead_lag_detection (repos checked)
- DeltaLag (ICAIF 2025): https://arxiv.org/abs/2511.00390 [FT, repo lit_review]
- Fanshawe et al. 2026: https://arxiv.org/abs/2601.04602 [FT, repo lit_review]
- IDP-DSN (SIGIR 2026): https://arxiv.org/abs/2604.18213 · code https://github.com/codewhisperer-rs/IDP-DSN [FT]
- HCIM 2026: https://arxiv.org/abs/2605.26290 [ABS]
- Zhang et al. 2026 (dual-channel signed weighted): https://arxiv.org/abs/2602.12663 [ABS]
- SiST-GNN 2026 (persistence lesson): https://arxiv.org/abs/2605.25548 [FT]
- Izadifar & Rahmati 2026: https://arxiv.org/abs/2607.12067 [FT]
- Breitung & Müller 2025, *JFE*: https://www.sciencedirect.com/science/article/pii/S0304405X25000157 [ABS]
- Li & Ferreira 2025: https://arxiv.org/abs/2501.07135 [ABS]
- Cartea, Cucuringu & Jin 2023: https://ssrn.com/abstract=4599565 [SNIP]
- Bennett, Cucuringu & Reinert 2022: https://arxiv.org/abs/2201.08283 [SNIP, BG]
- Curme et al. 2015: https://arxiv.org/abs/1401.0462 [SNIP, BG]
- Souza & Aste 2018: https://arxiv.org/abs/1812.01103 [SNIP, BG]
- Qing & Xu 2025/2026: https://arxiv.org/abs/2508.04970 [ABS]
- Dahake & Chakraborty 2026: https://arxiv.org/abs/2608.12023 [ABS]
- Bartesaghi et al. 2025, *Physica A*: https://doi.org/10.1016/j.physa.2025.130698 [SNIP]
- Uematsu & Yamagata 2025, *JASA*: https://doi.org/10.1080/01621459.2025.2450836 [SNIP]
- TGM library: https://github.com/tgm-team/tgm · paper https://arxiv.org/abs/2510.07586 [ABS]
- Practitioner evidence (not peer-reviewed): https://github.com/nick-fournier/portfolio-lab/pull/17 [SNIP]
- UU finance data hub: https://uufinance.github.io/data/ and https://uufinance.github.io/data/wrds/databases/ (opened)
- S&P 500 historical members: https://github.com/fja05680/sp500 (repo checked; last update Sep 2026)
- Stooq data guide: https://www.quantstart.com/articles/an-introduction-to-stooq-pricing-data [SNIP]
