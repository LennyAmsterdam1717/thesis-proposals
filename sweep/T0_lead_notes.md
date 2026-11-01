# T0 — lead agent's own notes (early kill-test searches during Phase 1)

Papers opened by the lead agent (not by a theme subagent). Same template as the sweep files.

## A. Papers (main list)

### izadifar2026institutional — Institutional Equity Holdings Prediction Using Node Affinities of Dynamic Graphs
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

### huang2026crossstock — Cross-Stock Predictability via LLM-Augmented Semantic Networks
- Citation: Huang, Y., Fan, Z., Hu, K., Ye, Y., *Cross-Stock Predictability via LLM-Augmented Semantic Networks*, arXiv:2604.19476, 2026.
- Link: https://arxiv.org/html/2604.19476
- First public: arXiv 2026 (v2 26 Apr 2026; v1 date not shown in extraction); Venue: preprint (q-fin.PM); Peer-reviewed: no
- Verification: [FT] (HTML; limitations truncated)
- Summary: Builds a candidate stock graph from 10-K risk-factor embeddings, then lets an LLM (DeepSeek) label each pair as competitor / supply chain / complementary / substitute / peer / unrelated, and uses the filtered relation graph to aggregate pairs-trading mean-reversion signals on S&P 500 (2011–2019). LLM-filtered network Sharpe 0.82 vs 0.74 baseline and 0.79 SIC.
- Graph type: annually rebuilt (time-varying), typed relations, undirected; trading signal effectively signed per pair.
- Task: relation extraction + cross-sectional signal (portfolio), not link prediction.
- Data: CRSP (paid) + 10-K text (public).
- Code available? not stated.
- What it leaves open: one LLM, no construction-sensitivity analysis, no transaction costs, no validation of LLM labels against ground truth; relation types are fixed-weight, not learned; does not predict when pair relations activate/deactivate.

### anton2014connected — Connected Stocks [BACKGROUND]
- Citation: Antón, M., Polk, C., *Connected Stocks*, Journal of Finance 69(3), 2014.
- Link: https://personal.lse.ac.uk/polk/research/ConnectedStocks.pdf (seen in search; author PDF snippet)
- Verification: [ABS] (search-result abstract text only) — background only
- Summary: Common active mutual-fund ownership forecasts higher future pairwise abnormal return correlation; causal evidence via the 2003 fund scandal.
- Relevance: the economic premise for "common-ownership graph → future co-movement edges".

## B. Seen but not opened [UNVERIFIED]
- DefaultGNN: corporate default prediction from buyer–seller transaction networks — arXiv 2609.25542 (Sep 2026) — seen in search (T7 agent should cover; private transaction data).
- Zandi et al., *A Multimodal Approach to SME Credit Scoring Integrating Transaction and Ownership Networks*, arXiv 2510.09407 (Oct 2025) — SME data, private.
- Li et al., *Heterogeneous Graph Pre-training ... Default Risk Propagation among Bond Issuers*, arXiv 2501.03268 (Jan 2025) — Chinese enterprise KG.
- GAPNet (arXiv 2602.00888, Jan 2026), DRFN (arXiv 2510.10695, Oct 2025), Relational Probing (arXiv 2604.10212, 2026), KG construction for stock markets with LLM reasoning (arXiv 2601.11528, first posted Nov 2025) — T5/T8 agents.

## E. Public data noticed
- Hoberg–Phillips TNIC-3/TNIC-2/TNIC-All, VTNIC, ETNIC (doc2vec; preliminary, 2025): firm-pair-year similarity networks from 10-K text, 1989–2023, free download; **keyed on Compustat gvkey only, no CIK/ticker link provided** → needs WRDS or a crosswalk.
- SEC 13F data sets 2013Q2–Aug 2026; Financial Statement (and Notes) data sets; EDGAR APIs; 17g-7(b) rating histories (12-month delay). See NOTES.md §0.2b.

## G. BibTeX
```bibtex
@misc{izadifar2026institutional,
  title  = {Institutional Equity Holdings Prediction Using Node Affinities of Dynamic Graphs},
  author = {Izadifar, Emad and Rahmati, Zahed},
  year   = {2026},
  eprint = {2607.12067},
  archivePrefix = {arXiv},
  url    = {https://arxiv.org/abs/2607.12067},
  note   = {[FT] arXiv v1 2026-07-13; preprint; code github.com/e-izdfr/portfolio-holdings-prediction}
}
@misc{huang2026crossstock,
  title  = {Cross-Stock Predictability via {LLM}-Augmented Semantic Networks},
  author = {Huang, Yikuan and Fan, Zheqi and Hu, Kaiqi and Ye, Yifan},
  year   = {2026},
  eprint = {2604.19476},
  archivePrefix = {arXiv},
  url    = {https://arxiv.org/abs/2604.19476},
  note   = {[FT] arXiv 2026 (v2 2026-04-26); preprint}
}
@article{anton2014connected,
  title   = {Connected Stocks},
  author  = {Ant{\'o}n, Miguel and Polk, Christopher},
  journal = {The Journal of Finance},
  volume  = {69},
  number  = {3},
  pages   = {1099--1127},
  year    = {2014},
  doi     = {10.1111/jofi.12149},
  note    = {[ABS] BACKGROUND (pre-2025). Abstract via search snippet of author PDF.}
}
@misc{hobergphillips_tnic,
  title  = {Hoberg-Phillips Data Library: Text-based Network Industry Classifications ({TNIC})},
  author = {Hoberg, Gerard and Phillips, Gordon},
  year   = {2025},
  url    = {https://hobergphillips.tuck.dartmouth.edu/tnic_basedata.html},
  note   = {[FT] data page + TNIC-3 readme opened 2026-11-01; coverage 1989--2023; gvkey identifiers only. Data resource.}
}
```
