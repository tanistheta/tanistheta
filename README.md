<div align="center">

[![Portfolio](assets/portfolio.png)](https://tanishkgangwar.vercel.app/)

</div>

<div align="center">

**Tanishk Gangwar**
B.Tech CSE (Data Science) · Manipal University Jaipur · Batch '28

[![Portfolio](https://img.shields.io/badge/portfolio-tanishkgangwar.vercel.app-black?style=flat-square)](https://tanishkgangwar.vercel.app/)
[![LinkedIn](https://img.shields.io/badge/linkedin-connect-0A66C2?style=flat-square&logo=linkedin)](https://www.linkedin.com/in/tanishk-gangwar-809614363/)
[![Email](https://img.shields.io/badge/email-tanishk7531@gmail.com-EA4335?style=flat-square&logo=gmail)](mailto:tanishk7531@gmail.com)

</div>

---

I study how information behaves in code - in commits, in adoption curves, in systems that look chaotic until you measure them properly. Most of what's below started as a clean hypothesis. Some of it ended with the data winning instead of me. I think that's the more interesting outcome to publish.

---

## research

### [Sekivara](https://github.com/tanistheta/sekivara)
`difference-in-differences` `quasi-experimental` `git mining` `403k commits`

**Does GitHub Copilot change how people commit - causally, not just correlationally?**

A difference-in-differences study across **403,646 commits from 9 repositories** (2018–2024), comparing Copilot-adopting repos against a control group before and after GA. HC3 robust standard errors, pre-trend validation, an event study, a dose-response check, and author-cohort decomposition - built to survive the obvious confounds, not just produce a headline number.

**Result:** Copilot adoption causally reduced mean files-per-commit by **28%**, insertions-per-commit by **37%**, and large-commit fraction by **2.4pp** (all p < 0.01). The effect concentrates in existing contributors, not newcomers - people write smaller, more atomic commits once an assistant is doing the typing.

A complete IEEEtran paper is drafted, targeting MSR 2027.

---

### [Ikiru](https://github.com/tanistheta/ikiru)
`staggered-adoption DiD` `sun-abraham estimator` `bigquery` `89 repos`

**Does adding a CODEOWNERS file causally change how fast pull requests get reviewed?**

A staggered-adoption difference-in-differences study (Sun-Abraham estimator) on an **89-repo panel** built from **~21TB of GH Archive data** pulled via Google BigQuery, with repository and calendar-time fixed effects and standard errors clustered at the repo level.

**Result:** a clean null on PR closing time through roughly 18 months post-adoption - CODEOWNERS doesn't move review speed in the window most studies look at. Two coefficients turn significant at months 23–24, but that signal was diagnosed rather than reported at face value: with the panel's data cutoff, only 17 of 28 treated repos can even reach that horizon, and without a later-adopting comparison cohort at the same horizon, fixed effects can't separate genuine treatment effect from cohort maturation - so it's documented as confounded, not claimed as a finding.

A follow-up coverage-heterogeneity check (parsing CODEOWNERS files at treatment-date commits across all 28 treated repos) surfaced a bimodal split - 14 repos at ≤10% file coverage, 8 at ≥90% - and the high-coverage subset alone shows a parallel-trends violation, pointing to coverage as a likely moderator the headline null result was masking.

Mid-analysis, a directly competing paper (Lulla, Kula & Treude, 2025) was found and incorporated rather than ignored - Ikiru's 18–24 month window is a range their fixed RDD design structurally cannot observe at all. Includes a placebo test and a balance check on dropped low-activity controls; full pipeline (BigQuery pull → panel construction → event-study regression → robustness checks) is reproducible end to end.

---

### [Entropic Fingerprint](https://github.com/tanistheta/entropic-fingerprint)
`shannon entropy` `leave-one-repo-out CV` `honest null result`

**Does Shannon entropy in commit histories predict upcoming software releases?**

The hypothesis was clean: release prep should look different from normal development at the information-theoretic level. It didn't hold up.

**Result:** AUC 0.47 under leave-one-repo-out cross-validation - indistinguishable from chance. What the data *does* show: commit volume, not entropy, is the dominant structural signal (Spearman r = 0.817, p = 0.007) - a confound that the entropy hypothesis was actually just re-detecting.

Published as what it is: a negative result with a documented confound, not a quiet repo nobody talks about. The methodology is the part worth reading.

---

### [Bias-Aware ML Pipeline](https://github.com/tanistheta/bias_awareness)
`fairness-aware ml` `demographic parity` `equal opportunity` `random forest`

**Does fixing bias in tabular ML actually cost you accuracy - and does every mitigation strategy work equally well?**

A 12-module, measurement-first pipeline that quantifies and mitigates attribute-level bias in tabular datasets, benchmarking three mitigation strategies - reweighting, feature suppression, and post-processing - across multiple fairness axes on a Random Forest classifier. Built in collaboration with Dr. Chirag Joshi; currently pending arXiv endorsement.

**Result:** 64.5% reduction in Demographic Parity Gap and 47.8% reduction in Equal Opportunity TPR Gap, with accuracy held stable. The sharper finding: naive feature suppression - the most intuitive fix - was the *least* effective strategy of the three, underperforming reweighting on every fairness axis tested.

---

## open source

Contributions to libraries with real production surface area — not toy patches.

- **[pandas](https://github.com/pandas-dev/pandas)** — fixed a negative-slice indexer validation bug in core indexing logic using `slice.indices()` ([PR #66101](https://github.com/pandas-dev/pandas/pull/66101)), with regression tests.
- **[PyDriller](https://github.com/ishepard/pydriller)** — corrected `Commit._stats()` to respect the `skip_whitespaces` flag ([PR #320](https://github.com/ishepard/pydriller/pull/320)); added a `Commit.patch` property exposing full unified diffs, closing a long-standing feature request ([PR #321](https://github.com/ishepard/pydriller/pull/321)).
- **[PyGithub](https://github.com/PyGithub/PyGithub)** — added a configurable `max_rate_limit_wait` cap to `GithubRetry`, with a new `RateLimitExceededExceedsMaxWait` exception, replacing unbounded rate-limit stalls ([PR #3540](https://github.com/PyGithub/PyGithub/pull/3540)).

---

## builds

### [Kansei 感性](https://github.com/tanistheta/kansei) · [kansei.duckdns.org](https://kansei.duckdns.org/)
`clip` `umap` `fastapi` `docker` `gcp`

**Can a machine read taste?**

A 25-round image-comparison quiz that encodes every choice as a CLIP ViT-B/32 embedding, scores it against 16 hand-curated aesthetic centroids by cosine similarity, and visualizes the result in a 3D UMAP projection alongside kNN nearest-image retrieval. You can also upload any photo and have it classified the same way.

The quiz logic was the easy part. The infrastructure wasn't:

- Migrated off a paid host onto a **free-tier GCP VM by choice** - 964MB of RAM, two Dockerized services, on purpose, because the constraint is what makes the engineering real
- Diagnosed and fixed three separate production failures with evidence, not guesses: a browser secure-context restriction silently breaking session tracking, a missing auth token, and a latent null-handling bug in the UMAP code path that only a working classify feature could expose
- Found and worked around a **CPU-bursting ceiling** (GCP documents e2-micro at 25% sustained CPU) by measuring it directly rather than assuming it was a memory problem
- Cut a Docker image from 9.2GB to 1.62GB by fixing a pip dependency-resolution bug, and cached an expensive UMAP fit to disk so it doesn't get recomputed on every restart

Free HTTPS, a real domain, zero ongoing cost. The full writeup - every bug, every measurement, every tradeoff - is in the repo's README.

---

## stack

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![C++](https://img.shields.io/badge/C++-00599C?style=flat-square&logo=cplusplus&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![GCP](https://img.shields.io/badge/Google%20Cloud-4285F4?style=flat-square&logo=googlecloud&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=flat-square&logo=scikit-learn&logoColor=white)
![statsmodels](https://img.shields.io/badge/statsmodels-8C564B?style=flat-square)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=flat-square&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=flat-square&logo=numpy&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=flat-square&logo=git&logoColor=white)

---

## achievements

- 🏆 Top **1,500 of 100,000+** participants - Google *"The Big Code"* competitive programming challenge

---

## stats

<div align="center">

![GitHub Stats](https://github-readme-stats.vercel.app/api?username=tanistheta&show_icons=true&theme=nord&hide_border=true&bg_color=00000000)

[![GitHub Streak](https://streak-stats.demolab.com?user=tanistheta&theme=nord&hide_border=true&background=00000000)](https://git.io/streak-stats)

![Top Langs](https://github-readme-stats.vercel.app/api/top-langs/?username=tanistheta&layout=compact&theme=nord&hide_border=true&bg_color=00000000)

</div>

---

<div align="center">
<sub>CGPA 9.00/10 · Manipal University Jaipur · Batch '28 · tanishk7531@gmail.com</sub>
</div>
