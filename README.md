# Evidence Synthesis Automation Map

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.22981564.svg)](https://doi.org/10.5281/zenodo.22981564)

[简体中文](README.zh-CN.md)

A maintained map of which steps of a systematic review or meta-analysis already have working
tools, what those tools cost, and what they leave to the human — plus a measured probe of how
much of the same work a web-enabled large language model already does.

Two kinds of content, kept separate on purpose:

- **The map** describes tools. Each row names a tool, links its own page, and records the date the
  claim was checked.
- **The benchmark** describes a measurement. Real tasks, answer keys built from the primary sources
  by a separate agent before the model under test saw anything, and the scores — including the cases
  where the model did better than the answer key.

**Status: first public version.** This repository is being built in the open. Both main documents
are here: the map covers 26 steps, and the benchmark reports nine tasks. The five rows below are
pulled from the map, and every figure on this page is sourced. Rows will be added and corrected as
tools change.

## Start here

| Document | What it contains |
|---|---|
| [AUTOMATION_MAP.md](AUTOMATION_MAP.md) | The map: 26 steps of the review workflow, the tools that already do each one, cost, and a check date per row. |
| [LLM_BENCHMARK.md](LLM_BENCHMARK.md) | The benchmark: method, scoring rules, results and limits. The task texts and answer keys are not in the repository; they are sent on request. |
| [CONTRIBUTING.md](CONTRIBUTING.md) | How to add a tool or correct a row. |

## Who this is for

- Review authors and methodologists deciding whether to buy, build, or do a step by hand.
- Librarians and information specialists who get asked "is there a tool for this?".
- Tool builders who want to know which gap is real before writing code.
- Editors and peer reviewers who want to know what can be checked automatically.

## Why this exists

Systematic reviews are produced at scale, and the steps that guidelines require are, measurably,
often not done well:

- **Volume.** PubMed indexed 48,220 records typed as systematic review or meta-analysis for 2025
  and 43,367 for 2024 (counted through the PubMed E-utilities API, 2026-09-26; the 2025 query
  returned 48,221 on a re-run the same day, so read the last digit as drift, not precision).
- **Searches are rarely reproducible.** Across 100 reviews and 453 database searches, 22 (4.9%)
  reported all six PRISMA-S items, 47 (10.4%) could be reproduced within 10% of the original
  number of results, and one review had enough detail to be fully reproducible
  ([Rethlefsen 2024, J Clin Epidemiol](https://pubmed.ncbi.nlm.nih.gov/38052277/)).
- **Reporting checklists are not met.** Of 222 reviews sampled, 67 (30.18%) reported using PRISMA
  2020; none adhered completely, and average adherence was 42.64%. The article's item-level figures
  are reported out of those 67, not out of 222 — the four least-adhered items, among them the search
  strategy and the characteristics of excluded studies, ran from 7.46% (5/67) to 11.94% (8/67)
  ([Ivaldi 2024, Cochrane Evid Synth Methods](https://pmc.ncbi.nlm.nih.gov/articles/PMC11795886/)).
- **Extracted numbers carry errors.** In 27 meta-analyses with two trials sampled from each, 17
  (63%) had an error in at least one of the two trials
  ([Gøtzsche 2007, JAMA](https://doi.org/10.1001/jama.298.4.430)).
- **Retracted studies reach pooled results.** 312 of 1,330 retracted randomised trials were used
  quantitatively, in 4,095 meta-analyses across 847 reviews; 3,902 of those could be recomputed.
  Removing the retracted study changed the direction of the effect in an estimated 8.4% of them
  (95% CI 6.8–10.1) and statistical significance in 16.0% (14.2–17.9). These are cluster-adjusted
  model estimates, not raw proportions — do not multiply them back out against 3,902
  ([Xu 2025, BMJ](https://doi.org/10.1136/bmj-2024-082068)).
- **Counter-evidence, stated plainly.** A separate study identified 61 reviews and extracted data
  from 50 of them; of 166 meta-analyses it could recompute, 160 (96%) stayed inside the original
  confidence interval and 18 (11%) changed significance
  ([Graña Possamai 2025, JAMA Intern Med](https://doi.org/10.1001/jamainternmed.2025.0256)).
  So the usual effect of one problem study is small; the tail is what matters.
- **Some duplicates cross a language boundary.** Of 470 Chinese-sponsored randomised trials,
  55 (11.7%) had 75 duplicate publications, and 53 of those duplicates (70.7%) crossed languages.
  All were identified by hand
  ([Jia 2020, JAMA Netw Open](https://doi.org/10.1001/jamanetworkopen.2020.27104)).

A map is useful because these gaps are unevenly served. Some steps are finished work with free
tools. Some have a paid tool and a free approximation. Some are still done by hand.

## Five rows from the map

Costs and capabilities checked 2026-09. Prices change; treat the check date as part of the claim.

| Step | Tools that already do it | Cost | What is still left to the human |
|---|---|---|---|
| Translate one search string into each database's syntax | Polyglot Search Translator, now served at [TERA](https://tera-tools.com) | Free, account optional (checked 2026-09) | Polyglot adjusts syntax; per the [developers' paper](https://pmc.ncbi.nlm.nih.gov/articles/PMC9359691/) and a [library guide](https://library.svhm.org.au/literature_searching/polyglot) it does not choose subject headings, so MeSH/Emtree mapping stays manual. Per those same two sources the target list is ten English-language platforms. TERA's live list is a JavaScript app we could not read (checked 2026-09), so treat the absence of any Chinese-language database as unconfirmed on the current product. |
| Remove duplicate records | [ASySD](https://github.com/camaradesuk/ASySD) | Free, open source (checked 2026-09) | Its [developers report](https://pmc.ncbi.nlm.nih.gov/articles/PMC10483700/) sensitivity 0.95–0.99 and specificity >0.99 across five datasets, all English. Across the dedup tools checked, a Chinese-language record and its English counterpart pair up only when both carry the same DOI — the string fields compared differ across scripts. |
| Collate several reports into one study | [Covidence](https://www.covidence.org/) (merge studies), [refs2study](https://github.com/L-ENA/refs2study) (research code) | Covidence [$339 USD/year per review](https://www.covidence.org/pricing/), and [free for registered Cochrane reviews once the review is linked to a Cochrane account](https://support.covidence.org/help/general-information-for-cochrane-authors); refs2study free (checked 2026-09) | Cochrane [MECIR item C42](https://www.cochrane.org/authors/handbooks-and-manuals/mecir-manual/standards-conduct-new-cochrane-intervention-reviews-c1-c75/performing-review-c24-c75/selecting-studies-include-review-c39-c42) makes collating reports mandatory. Merging in Covidence is a manual step. refs2study predicts which references belong to the same study, but it is research code with no app and no language handling, and [Trials to Publications](https://arrowsmith.psych.uic.edu/) covers ClinicalTrials.gov and PubMed only — we found nothing that proposes cross-language candidate pairs. |
| Convert a median and IQR into a mean and SD | [HKBU median-to-mean calculator](https://math.hkbu.edu.hk/~tongt/papers/median2mean.html), [estmeansd](https://cran.r-project.org/package=estmeansd), `meta::metacont` | Free (checked 2026-09) | Covered. What remains is choosing the scenario and the estimator, which is a methods decision, not a missing tool. |
| Check included references for retractions and corrections | [Europe PMC Article Status Monitor](https://europepmc.org/ArticleStatusMonitor) (batch, CSV export), [Zotero](https://www.zotero.org/blog/retracted-item-notifications/) and [EndNote 20.2+ with sync enabled](https://libguides.rug.nl/umcg/endnote/retraction) (per a university library guide, not the vendor), [scite](https://scite.ai/) | Free to $20/month (scite Basic) (checked 2026-09) | The "corrections" half of this step is mostly unserved by the free tools listed: measured 2026-09, the Europe PMC batch endpoint returned retraction and withdrawal status but no erratum or expression-of-concern status, and Zotero and EndNote flag retractions only. And none of these tells you which pooled estimate in your own review changes once a study is removed. |

The full map covers the workflow end to end, in the same 15 steps the
[issue form](.github/ISSUE_TEMPLATE/add-or-fix-a-tool.yml) uses, so a contribution lands in a row
without re-mapping: question, protocol and registration; building the search strategy; translating a
strategy between databases; running searches and archiving the search record; deduplicating records;
screening; collating multiple reports into one study; data extraction; reconciling two extractors'
data; risk of bias and trustworthiness checks; effect-size preparation and conversion; synthesis,
meta-analysis and figures; certainty of evidence (GRADE) and summary tables; reporting and
submission checks; and maintenance after publication.

## What the benchmark measured

Nine tasks — eight built for this probe, plus an earlier 24-item retraction and correction check —
were set from real, public material: published reviews and trial registry records. Each key was
built from those primary sources, item by item, before the model under test saw anything. One key
did not survive the run: on the retraction task the model found a published correction the key had
missed, that key was corrected, and the 24/24 reported below is the score against the corrected key.
The model (Claude Fable 5.1) was restricted to web search and web fetch, with no access to local
files; the call log was audited afterwards for local-file reads and none were found. Scoring was done
against the key, item by item, by a session that had not answered the task.

Result on the eight new tasks: six outputs were adequate, two were partly right, and none was poor.
On one of them the model reported something the key had not asked for: a published correction
notice attached to the journal version of a preprint. On the registration task, the article printed
a registration number in a form that does not exist; finding the real record was a required item of
the key, and the model found it.

The ninth task, the 24-item retraction and correction check, is the one worth reading twice. With
web access the model scored 24/24 on two independent runs, against the corrected key. Without web
access, same prompt, it scored 14/24 and reported 6 flagged items as clean — the failure direction
that matters, because a false reassurance is silent.

**What this does not show.** One item per task and a small n. One model family. The keys were built
by agents as well, from the primary sources with verbatim quotes — which is not the same as an
independent second human extractor. A key can therefore be wrong, and in these runs one was. The
web-enabled runs happened inside an agent harness that can fetch API responses directly, so an
ordinary chat interface may do worse. The tool survey behind the map was one reviewer per candidate
with no adversarial second pass. None of this is a clinical validation of anything, and none of it
says a model should be trusted without a human check.

The honest headline of the probe is negative: on the tasks we tested, a web-enabled model was
already adequate. We publish that result instead of a tool.

## About the author

**Chuan Yin (尹川)** — Shanghai Jiao Tong University
ORCID [0009-0005-2830-8167](https://orcid.org/0009-0005-2830-8167)
Email `yinchuan [at] sjtu.edu.cn`

Public tool by the same author: **Evidence OS Inspector** — browser-only, MIT licensed, alpha. It
binds claims to the passages they came from and flags which claims need re-review when a source
changes. [Repository](https://github.com/yinchuan123/evidence-os-inspector) ·
[Demo](https://yinchuan123.github.io/evidence-os-inspector/)

Contact: [yinchuan@sjtu.edu.cn](mailto:yinchuan@sjtu.edu.cn)

## How to contribute

One tool per issue or pull request, with a link and a note of what you checked and when — see
[CONTRIBUTING.md](CONTRIBUTING.md). Corrections about your own tool are welcome.

If the map is useful to you, watching or starring the repository helps other people find it.

## Licence

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). [LICENSE](LICENSE) carries the licence
text as Creative Commons publishes it; [NOTICE](NOTICE) says how to attribute reuse.

## How to cite

Yin, C. (2026). *Evidence synthesis automation map, and a measured probe of what a web-enabled LLM already does*. Zenodo. https://doi.org/10.5281/zenodo.22981564

Citation metadata is in [CITATION.cff](CITATION.cff); GitHub renders it as a ready-made citation
from the "Cite this repository" link on the repository page.
