# The automation map

[简体中文](AUTOMATION_MAP.zh-CN.md) · [Repository overview](README.md) · [Benchmark](LLM_BENCHMARK.md)

## What this is for

If you are planning a systematic review or a meta-analysis, you have to decide, step by step, what
to buy, what to install, what to hand to a language model, and what to do yourself. This page is
that decision laid out as a table: one row per step of the workflow, with the tools that already do
the job and what they cost, what a web-enabled large language model did when we set it a real task
from that step, and the part that is still a person's job. Every row carries the date the claim was
checked, because prices and feature lists go stale within months. The table is the product; the
prose around it is kept short on purpose.

## How to read the table

- **Existing tools** — every tool that carries a link here had its own page opened on the check
  date. Where we could not open it (a login wall, a bot check, a page that is a JavaScript app with
  no readable text), the row says so and the claim is marked unverified rather than guessed. A few
  tools are named without a link: those come from a catalogue listing or from another tool's
  documentation, and the row marks them **page not opened**.
- **Young repositories** — where a tool is hosted on GitHub and the repository is new or unstarred,
  the row prints its star count and creation date from the GitHub API, read 2026-09-26. This is
  applied to every such row, not only to the ones we happen to doubt.
- **Cost** — what the vendor's own page said on the check date. Treat the date as part of the price.
- **Web-enabled LLM** — what happened in our probe, which covered 9 of these steps. Nine tasks were
  built from real published material, the answer key was settled before the scored run (on one task
  the model then found an item the key had missed, and the key was corrected before scoring), and the
  model (Claude Fable 5.1) was restricted to web search and page fetching with no local file
  access. Method, scoring rules and full results: [LLM_BENCHMARK.md](LLM_BENCHMARK.md). The other
  rows say **not tested** — that means untested, not "cannot be done".
- **Still needs a person** — the specific thing the tools and the model do not do. This is the
  column the map exists for.
- **Checked** — `2026-09-17` for the tool survey, `2026-09-16` for the retraction and correction
  row, `2026-09-26` for rows filled in later. All links re-checked 2026-09-26.

Nothing here is a ranking and nothing here is an endorsement. A tool being free does not make it
adequate, and a tool being paid does not make it better.

## The map

| # | Step | The job | Existing tools (checked 2026-09) | Web-enabled LLM in our tests | Still needs a person | Checked |
|---|---|---|---|---|---|---|
| 1 | Protocol and registration | Write the protocol, register it, keep the record current. | [PROSPERO](https://www.crd.york.ac.uk/prospero/) — free registration. [INPLASY](https://inplasy.com/) — its own site states 9,700+ registered protocols, 100 countries, and a DOI per record; it has a Fees & Services page, amount unverified. | Not tested. | The question, the eligibility criteria and the analysis plan. Registry records are coarse: of 96 reviews registered in PROSPERO, 39% did not explicitly specify a primary outcome ([Tricco 2016](https://pubmed.ncbi.nlm.nih.gov/27079845/)). | 2026-09-26 |
| 2 | Terminology mapping (Chinese ↔ MeSH / Emtree) | Map a Chinese outcome, drug or disease name to a MeSH heading, an Emtree term and the standard English name. | [SinoMed subject search](https://www.sinomed.ac.cn/zh/subjectSearch.html) — its page says a matching heading can be found from a Chinese heading, an English heading or a synonym; running a lookup redirected to a login. [CMeSH](http://cmesh.imicams.ac.cn) — the Chinese translation of MeSH; the front page is a login, and libraries list it as a subscription. Emtree lookup sits inside Embase. [Termonline](https://www.termonline.cn) — free, bilingual, 800,000+ terms, but does not map to MeSH or Emtree. | Not tested. | Confirm the heading exists in the current year's thesaurus, and decide when there is no equivalent at all — common for traditional-medicine outcomes. | 2026-09-17 |
| 3 | Search-strategy building and cross-database translation | Rewrite one strategy for every database: field codes, operators, truncation, and controlled vocabulary. | Polyglot Search Translator, now served at [TERA](https://tera-tools.com) — free, account optional; the [developers' paper](https://pmc.ncbi.nlm.nih.gov/articles/PMC9359691/) lists about ten English-language platforms. Library guides and the tool page both say it adjusts syntax and does **not** choose subject headings. [Embase PubMed-to-Embase translator](https://www.elsevier.support/embase/answer/pubmed-to-embase-translation-tool) — maps MeSH to Emtree automatically; it sits inside Embase, so access implies a subscription. [Research Gold translator](https://researchgold.org/resources/database-search-translator) — free, no sign-up; its own page says controlled vocabulary cannot be directly translated. [Ovid Search Translator](https://tools.ovid.com/translate), [Medline Transpose](https://medlinetranspose.github.io) — free, syntax only. None of them targets CNKI, Wanfang or SinoMed. | **Partial.** Syntax was legal on all three Chinese platforms, no field code was invented, and it correctly refused to treat the source strategy's `*` as truncation on platforms that have no such operator. But its subject-term list was not the published one — three of four terms, missing the fourth — and it said itself that it could not verify one platform's heading form against that platform's thesaurus. None of the three databases could be run: all require a login, and one served a slider CAPTCHA. | Choose the subject headings. Then run the query and record what came back. | 2026-09-17 |
| 4 | Search reporting (PRISMA-S) and a reproducible archive | Record, per database: platform, date, the strategy line by line, and the hit count. Then write the appendix. | [CiteSource](https://cran.r-project.org/web/packages/CiteSource/index.html) — free R package and Shiny app; keeps per-source metadata and shows overlap and unique contribution, but does not log dates or hit counts and does not write a PRISMA-S appendix. [slr-harvester](https://github.com/socresearcher/slr-harvester) — free to use, but the repository declares no recognised licence; logs search strategies and screening decisions, and only for API-accessible sources. 0 stars, repository created 2026-02-14 (GitHub API, 2026-09-26). searchRxiv (CABI) — a moderated, DOI-stamped repository for search strings; deposit is manual, and its journal page returned 403 to us, so details are unverified. Nested Knowledge — commercial; records the query and date you typed at import. | **Matched the key.** All nine source names and hit counts reproduced, no platform or vendor invented for the sources that state none, and it derived the total the article never prints (1,687) and showed that the step down to the 107 records that entered screening rests on one author's judgement and is not reproducible. | Observe and write down the hit count and the date at the moment the search runs. A model cannot re-run a login-gated database. | 2026-09-17 |
| 5 | De-duplication of records | Remove records that describe the same document — including a Chinese record and its English counterpart. | [ASySD](https://github.com/camaradesuk/ASySD) + [Shiny app](https://camarades.shinyapps.io/ASySD/) — free, open source; its [developers report](https://pmc.ncbi.nlm.nih.gov/articles/PMC10483700/) sensitivity 0.95–0.99 and specificity >0.99 across five datasets, all English. [Zotero](https://www.zotero.org/support/duplicate_detection) — free; its page says it uses title, DOI and ISBN, then year (±1) and creators. [Rayyan Systematic Auto-Resolver](https://www.rayyan.ai/systematic-auto-resolver) — configurable field matching, a premium feature. Deduklick — commercial, page not opened. EndNote and NoteExpress — paid, pages not opened, and the incumbent in Chinese labs. | Not tested. | Cross-language pairs. In every tool we checked, a Chinese record pairs with its English counterpart only when both carry the same DOI; the fields being compared — title, author, journal — differ across Chinese script, pinyin and English. | 2026-09-17 |
| 6 | Screening (title/abstract, then full text) | Decide which records and reports are eligible, in duplicate. | [Rayyan](https://www.rayyan.ai/) — free tier, its [pricing page](https://www.rayyan.ai/pricing) shows $0 "free forever" for individuals and small teams, plus paid plans. [ASReview](https://asreview.nl/) — free, open source, coordinated at Utrecht University; active-learning screening with a simulation mode for testing it. [Covidence](https://www.covidence.org/) — paid, $339 per review per year, free for Cochrane reviews. | Not tested. | The eligibility decisions. MECIR and PRISMA both expect two people; a screening tool reorders the queue, it does not decide. | 2026-09-26 |
| 7 | Full-text retrieval | Get the full text of every report that passed screening. | Free programmatic routes, both called live on 2026-09-26: the [Europe PMC REST API](https://www.ebi.ac.uk/europepmc/webservices/rest/search?format=json&query=test) and NCBI E-utilities. Beyond those, publisher paywalls apply, and the Chinese databases are closed: SinoMed help pages redirected to a login, CNKI's advanced search served a slider CAPTCHA, and Wanfang requires a login to run a search. | Not tested. | Institutional access, interlibrary loan, writing to the authors. And a rule that applies to people and models alike: do not work around a CAPTCHA or a licence. | 2026-09-26 |
| 8 | Grouping reports into studies | Collate several reports of one study, and spot duplicate or overlapping-sample publications. | [Covidence](https://support.covidence.org/help/consensus-9831e0af) — merging references into one study is a manual step; paid. [Trials to Publications](https://arrowsmith.psych.uic.edu/) — free; given a ClinicalTrials.gov number it ranks PubMed articles by how likely they report that trial; CT.gov and PubMed only, with a batch mode. [refs2study](https://github.com/L-ENA/refs2study) — research code, no app, no language handling. | **Partial.** Six of seven clusters exactly right, including the Chinese/English same-cohort pair the task was built around. One cluster absorbed two records it should have left out — but it stated its reasons, and it flagged the unresolved sample-size conflict inside that cluster instead of smoothing it over. Every record was given a status; none was silently dropped. | Adjudicate the borderline merges, and find the candidate pairs in the first place. Cochrane MECIR item C42 makes collation mandatory. | 2026-09-17 |
| 9 | Data extraction | Pull characteristics and outcome data from each report into a table. | Platform tools in the SR Toolbox extraction category: Covidence (paid), Rayyan (free tier plus paid), [CADIMA](https://www.cadima.info/) (free), and — named in that catalogue, pages not opened — DistillerSR, EPPI-Reviewer, JBI SUMARI, Nested Knowledge, PICO Portal, Sysrev. AHRQ's SRDR+ has ceased operations; our note took 2025-11-28 from the AHRQ notice, but that notice page returned HTTP 202 with no body on every attempt, so the date is `unverified / 未核实`. | Not tested — our probe tested the comparison step, not extraction itself. | The extraction, in duplicate for outcome data (MECIR C46, mandatory). Measured uptake is low: in a sample of 152 reviews, 9 (6%) reported using Covidence — the most frequently used automation tool — for data extraction ([Büchter 2021](https://doi.org/10.1186/s12874-021-01438-z)), so the spreadsheet is still the real workflow. | 2026-09-17 |
| 10 | Double-extraction comparison | Compare two extractors' sheets cell by cell, aligned on study × arm × outcome × timepoint, separating format differences from value differences. | [Covidence consensus](https://support.covidence.org/help/consensus-9831e0af) — paid; its own page says changes are highlighted "except within data tables", which is exactly where outcome data sits. [Diffchecker Excel Compare](https://www.diffchecker.com/excel-compare/) — free in the browser, aligns by position; export is a Pro feature. [daff](https://github.com/paulfitz/daff) — free, MIT; aligns on a key column with `--id`, but does not read xlsx directly. Microsoft Spreadsheet Compare — only ships with Microsoft 365 Apps for enterprise. | **Matched the key.** All 6 value differences, all 6 format-only differences and both coverage differences, with no false positives — and it adjudicated correctly in the planted case where the *second* extractor was the one who had it right. | Decide the final value, and record who arbitrated and why. | 2026-09-17 |
| 11 | Statistical conversions | Median/IQR/range → mean and SD; effect-size interconversion; recover SD from a CI or a p value; combine arms. | [HKBU median-to-mean calculator](https://www.math.hkbu.edu.hk/~tongt/papers/median2mean.html) — free (Wan 2014, Luo 2018, Shi 2020 and 2023). [ebm-helper.cn](https://ebm-helper.cn/Conv/tomean+sd.html) — free, Chinese interface. [estmeansd](https://github.com/stmcg/estmeansd) + [Shiny app](https://smcgrath.shinyapps.io/estmeansd/) — free. [metaConvert](https://metaconvert.org/) — free, open source; [its paper](https://pmc.ncbi.nlm.nih.gov/articles/PMC12527507/) lists 127 formulas, 74 input combinations and 11 effect sizes (risk difference is not among them). [Meta-Analysis Accelerator](https://ma-accelerator.com) — its [pricing page](https://ma-accelerator.com/pricing) is a JavaScript app that prints no price or limit text to a fetcher; the application's own strings, read 2026-09-26, give 3 free projects per account, 10 studies per free project, $6/year and $24 lifetime — our earlier note recorded the free tier as 3 projects per month and 50 rows per project, which the current build does not say; [its paper](https://pmc.ncbi.nlm.nih.gov/articles/PMC11487830/) lists 21 conversions. [尔云间](https://www.biocloudservice.com/meta/home.html) — free, Chinese, 13 calculators. [RevMan calculator](https://documentation.cochrane.org/revman-kb/calculator-95420946.html) — price not checked. `meta::metacont` does the conversion inside the analysis. | Not tested. | Choose the scenario and the estimand, and check the conversion is admissible — converting an OR to an RR needs a baseline risk, and a small-sample CI needs t, not 1.96. | 2026-09-17 |
| 12 | Figure digitising and Kaplan-Meier reconstruction | Read numbers off a plot; rebuild individual data or a hazard ratio from a survival curve. | [WebPlotDigitizer / automeris.io](https://automeris.io), [v4 app](https://apps.automeris.io/wpd4/) — its pricing page returned 404, so the price of the current version is unverified. [IPDfromKM](https://cran.r-project.org/web/packages/IPDfromKM/index.html) — free R package; the online app is hosted at [trialdesign.org](https://trialdesign.org). [SurvdigitizeR](https://github.com/Pechli-Lab/SurvdigitizeR) + [Shiny app](https://pechlilab.shinyapps.io/SurvdigitizeR/) — free; its README calls it under development, and it does not read numbers-at-risk tables. [ebm-helper.cn KM→HR guide](https://ebm-helper.cn/Conv/KM_HR.html) — free, Chinese. KM-GPT is described in a published paper as a free web app; we could not confirm the app is live, so it is unverified. | Not tested. | Axis calibration, the numbers-at-risk table, and sanity-checking the reconstructed estimate. A wrong-but-plausible HR produces no error message. | 2026-09-17 |
| 13 | Risk of bias | Apply RoB 2, ROBINS-I, ROBINS-E or a scale, per outcome, and draw the figures. | [riskofbias.info](https://www.riskofbias.info/welcome/rob-2-0-tool) — free; RoB 2 (current version dated 22 August 2019, plus cluster-randomised and crossover variants), ROBINS-I, ROBINS-E and ROB-ME, as guidance documents and blank templates. [robvis](https://github.com/mcguinlu/robvis) + [Shiny app](https://mcguinlu.shinyapps.io/robvis/) — free; draws the traffic-light and summary figures. RobotReviewer is listed in the SR Toolbox quality-assessment category; we did not examine it. | Not tested. | The signalling-question judgements. Note the measured limit of this step: the [INSPECT-SR stage-2 study](https://pubmed.ncbi.nlm.nih.gov/40349737/) found that problematic studies "do not appear to be flagged by Risk of Bias assessment". | 2026-09-26 |
| 14 | Trustworthiness screening of included trials | Check baseline tables for impossible means and variances, and check that randomisation, allocation concealment and registration are actually described. | [INSPECT-SR](https://inspect-sr.com) — free guidance site; 21 checks in 4 domains, with editable templates, no built-in calculators and no Chinese translation. [GRIM / GRIMMER / TIDES app](https://errors.shinyapps.io/inspect-sr-means-variances/) — free; values are typed in one row at a time. [scrutiny](https://lhdjung.github.io/scrutiny/) — free R package (GRIM, GRIMMER, DEBIT, repeat-value analysis). [baseline](https://github.com/agbarnett/baseline) + [Shiny app](https://aushsi.shinyapps.io/baseline/) — free; detects over- and under-dispersion in baseline tables and can pull baseline tables from PMC articles, which does not reach Chinese journals. [reappraised](https://cran.r-project.org/web/packages/reappraised/index.html) — free R package. [智论助手](https://github.com/wangzhiguan72-droid/zhilun-assistant) — free, MIT, Chinese; GRIM, GRIMMER, Benford and terminal-digit checks, reads Chinese PDF and docx, but is not an RCT-specific checklist; 1 star, repository created 2026-09-15 (GitHub API, 2026-09-26). | **Matched the key.** All three arithmetically impossible means computed with the arithmetic shown, none of the five reachable cells flagged, the subgroup counts that sum to 38 of 40 (percentages to 95%) found — and no registration number invented for an article that prints none, the error the scoring rule punished hardest. | The accusation itself. INSPECT-SR's own [stage-2 paper](https://pubmed.ncbi.nlm.nih.gov/40349737/) warns that several checks "proved difficult to understand or implement, which may have led to unwarranted skepticism in some instances". GRIM only bites on integer-granular means with a known n, so many baseline cells are simply untestable. | 2026-09-17 |
| 15 | Trial-registration checks of included studies | Does each registration number resolve? Was registration before enrolment? Do registered and published primary outcomes agree? | [RegCheck](https://regcheck.app) ([code](https://github.com/JamieCummins/regcheck), AGPL-3.0) — the public web version is free; compares a registration with a paper dimension by dimension and highlights the source text; takes a ClinicalTrials.gov number or an uploaded PDF/DOCX. No ChiCTR support, no documented batch mode, and no check of registration timing. [cthist](https://cran.r-project.org/web/packages/cthist/index.html) — free R package; downloads ClinicalTrials.gov version history in batch. [TRNscreener](https://github.com/bgcarlisle/TRNscreener) and [ctregistries](https://github.com/maia-sh/ctregistries) — free; detect registration numbers and nothing more. [ChiCTR's own status page](https://www.chictr.org.cn/regstatusprojEN.html) — free; when we read it on 2026-09-17 the counter showed 105,848 prospective and 24,347 retrospective registrations; by 2026-09-26 it read 106,156 and 24,376, so treat it as a live counter, not a fixed figure. | **Matched the key.** 10/10 numbers resolved, including one printed incorrectly in the article — it found the real record and said the printed string was wrong rather than quietly substituting. 10/10 on prospective versus retrospective. 4/4 primary-outcome mismatches, quoting the registry field each time. When one registry page would not open it switched to the WHO ICTRP mirror and said so. | Decide whether a wording difference is an outcome change. The registry date field is itself unreliable: an analysis of ChiCTR found 848 trials reporting the wrong year of trial initiation. | 2026-09-17 |
| 16 | Retraction and correction checks on included studies | Check every included study for a retraction, withdrawal, correction, expression of concern, or a preprint since published in full. | [refcheck](https://github.com/KaizenShogun/refcheck) — free, MIT, runs in the browser, reads RIS/NBIB/BIB, covers all notice types and shows the rows it could not resolve; 0 stars, repository created 2026-09-06 (GitHub API, 2026-09-26). [Europe PMC Article Status Monitor](https://europepmc.org/ArticleStatusMonitor) — free batch checking with CSV export; its page returned 403 to our fetcher, so the feature list is from our earlier survey. Zotero — free, retractions. EndNote 20.2+ — paid, retractions. PubMed filters — free. Those three are named without a link: pages not opened this run. [scite](https://scite.ai/) — Basic $20/month billed yearly. [Covidence](https://www.covidence.org/) — $339 per review per year, free for Cochrane. The free interfaces underneath: [Crossref](https://www.crossref.org/), NCBI E-utilities, Europe PMC, and the [Retraction Watch Database](https://retractionwatch.com/retraction-watch-database-user-guide/). | **Matched the key on both runs.** With web access, 24/24 on a 24-item list twice, zero items wrongly declared clean, and it surfaced a correction the key had missed. **Without web access, the same model and the same prompt scored 14/24 and produced 6 confident false reassurances** — the failure direction that matters, because a false reassurance is silent. | Confirm the record that came back is the one your citation refers to. In a script test — a deterministic script, not the model — of 20 reference strings in five citation styles, a Crossref bibliographic query returned the right record as the top hit 13 times out of 20 (of the three title-only strings, 2 of 3) and had the truth somewhere in the top five 17 times; where structured fields exist, PubMed ECitMatch returned 14 exact PMIDs out of 14 and no wrong PMID in 34 calls. Both figures are optimistic, because the strings were generated from clean database metadata rather than copied from real reference lists. Then judge whether a correction matters. And note the blind spot — of the 179 DOIs on a sample of 200 Chinese-language RCT records, 174 returned 404 from Crossref and 5 were rate-limited, so none resolved; a re-sample of 25 also resolved none. That sample was concentrated in a small number of 2022 journals. | 2026-09-16 |
| 17 | Synthesis / meta-analysis | Fit the model, pool, quantify heterogeneity, run the planned sensitivity analyses. | [R meta](https://cran.r-project.org/web/packages/meta/index.html) — free, GPL; v8.5-0, published 2026-05-25; common and random effects, Hartung-Knapp and Kenward-Roger, prediction intervals, trim-and-fill, meta-regression, cumulative and leave-one-out, and it imports RevMan 5 data. [metafor](https://www.metafor-project.org/doku.php/metafor) — free and open source, GPL-2. [RevMan](https://revman.cochrane.org/) — Cochrane; licensing varies by user type and we did not check the price. Stata `metan` — requires Stata (paid). [metaanalysisonline.com](https://metaanalysisonline.com) — free; forest, funnel and Z-score plots, PNG and PDF. [Meta-Mar](https://www.meta-mar.com) — free tier of 2 analyses per month; passes at €19/€39/€69, student €29. [MetaReview](https://metareview.cc) — free, bilingual. | Not tested — this step was not made into a task of its own. | The model, the estimand, and whether pooling is defensible at all. Software will pool whatever you hand it. | 2026-09-26 |
| 18 | Forest and funnel plots | Draw the figures to the journal's specification. | [forestplotgenerator.com](https://forestplotgenerator.com) — free downloads carry a light site watermark, removed by a one-time Researcher Pass; its theme list includes a Cochrane/RevMan preset and journal presets; PNG or SVG. [Hiplot](https://hiplot.cn) — free and open source; forest and funnel modules, Chinese interface. [sci-draw.com](https://sci-draw.com/zh/forest-plot-generator) — Chinese interface, credit-based, draws pre-computed effects only. [SPSSAU](https://spssau.com/helps/meta/continuous.html) — Chinese; pricing is not on the page. R `meta` / `forestploter` and RevMan both export vector figures. | Not tested. | The journal's figure specification — column widths, minimum font size, file format — and actually looking at the rendered figure. | 2026-09-17 |
| 19 | GRADE and summary-of-findings tables | Rate certainty per outcome, record every downgrade reason, build the table. | [GRADEpro GDT](https://www.gradepro.org) — [pricing](https://www.gradepro.org/pricing): Standard $0 with 3 members and 25 questions and the ability to create GRADE evidence tables; Team $2,400 per active project per year. Its user guide says the application "will force the user to provide explanations in fields, where they are expected/necessary", and an evidence profile converts to a summary-of-findings table. Chinese is a selectable interface language. [MAGICapp](https://help.magicapp.org) — licence fee; its help says there is no specific non-profit discount. | Not tested. | The certainty rating itself: not downgrading the same problem twice in two domains, keeping the imprecision threshold consistent, and getting the absolute-effect arithmetic right. | 2026-09-17 |
| 20 | PRISMA flow arithmetic | Check that the flow diagram adds up, and that it agrees with the abstract, the text and the study tables. | [PRISMA2020 R package](https://cran.r-project.org/web/packages/PRISMA2020/index.html) ([repo](https://github.com/prisma-flowdiagram/PRISMA2020)) — free, MIT; v1.1.5, published 2026-09-05; interactive and static diagrams exported as HTML, PDF, PNG, SVG and more. [researchmethod.net generator](https://researchmethod.net/prisma-flow-diagram/) — free; its page says it "warns you when a count is impossible", for example when the number excluded exceeds the number screened. [PeerReviewAI](https://peerreviewai.org/guides/prisma-checklist) — $49 per manuscript; its page says it verifies the flow diagram is present and internally consistent. Covidence builds the diagram from your screening data, which prevents the error rather than detecting it. No free standalone checker takes an existing diagram plus the manuscript text. | **Matched the key.** Found the one broken subtraction — a 9-record gap between two boxes — left all fourteen balancing checks alone, said explicitly that the printed numbers cannot tell you *which* of three boxes is wrong, and located the article's published correction notice unprompted. | Read the diagram's topology, and decide whether a records / reports / studies difference is legitimate — under PRISMA 2020 it very often is. | 2026-09-17 |
| 21 | PRISMA 2020 checklist mapping | Give each of the 27 items (42 sub-items) a status and a location in the manuscript. | [PeerReviewAI](https://peerreviewai.org/guides/prisma-checklist) — $49 per manuscript; a per-item judgement of adequate, incomplete or missing, with no location stated on the page. SciSpace's PRISMA checker agent — credit-based; its own page was behind a bot check, so unverified. [PRISMA-AI](https://github.com/youkiti/PRISMA-AI-Share) — a research benchmark and leaderboard, MIT, not a hosted tool; in the paper's development cohort the reported accuracy is 45.21% given the manuscript alone, and 78.7–79.7% when the model is also handed the PRISMA 2020 checklist itself serialised in a structured format (Markdown, JSON, XML or plain text) — the blank canonical checklist, not one an author had filled in. Its living leaderboard is a separate measurement: 68.5–86.0% across 27 models on a 10-review cohort, read 2026-09-26. [PRISMA2020 R package](https://cran.r-project.org/web/packages/PRISMA2020/index.html) — free; produces the checklist file, but cannot find your page numbers. | **Matched the key.** All 14 item statuses and locations correct, including the two items genuinely not reported anywhere in the article, and it did not invent a Funding or Data Availability section for a journal layout that has none. | Page numbers only exist once the manuscript is rendered. And the 42 sub-item judgements are fine-grained: a heading with the right label is not the same as the item being reported. | 2026-09-17 |
| 22 | Registration-deviation reporting | Diff the registered protocol against the manuscript, and write the "differences from the protocol" table. | [RegCheck](https://regcheck.app) — free public web app, AGPL-3.0; per dimension it returns Deviation, No Deviation or Insufficient, with the source text highlighted; its clinical preset has 11 dimensions. No native PROSPERO or INPLASY support. [metacheck's reg_check module](https://www.scienceverse.org/metacheck_book/chapters/mod-reg-check.html) — free; calls RegCheck, but needs an API token its own documentation says cannot currently be requested on the site. [PreReg Deviation Template](https://apps.leibniz-psychology.org/prp-dev/) — free Shiny app; you fill in the deviations by hand and it exports a PDF or Word statement. | Not tested. | Decide whether narrowing "mortality" to "30-day all-cause mortality" is a deviation, and fetch the revision of the record that was current when data extraction began, not the latest one. PRISMA 2020 item 24c requires amendments to be described and explained. | 2026-09-17 |
| 23 | Format conversion between RevMan, Stata and R | Move one analysis dataset between packages. | [`meta::read.rm5`](https://search.r-project.org/CRAN/refmans/meta/html/read.rm5.html) and `read.cdir` — free; RevMan → R only, with no write-back. RevMan Web documents a CSV import in four file types and a .rm5 → data-package conversion (documentation page not opened this run). Covidence exports a RIS file and four CSVs for RevMan Web (paid). `metafor::to.wide` / `to.long` reshape inside R. Nothing we found writes RevMan import files from an R, Stata or Excel table, and we found no Stata command that reads a RevMan export. | Not tested. | Check the column mapping against the vendor's own template. Mapping errors — events versus non-events, SE versus SD, subgroup and comparison IDs — raise no error at all. | 2026-09-17 |
| 24 | Bibliographic format conversion (CNKI / Wanfang / SinoMed → RIS) | Turn a Chinese database export into something Zotero, Rayyan or Covidence will accept, in the right encoding. | CNKI and Wanfang export EndNote and NoteExpress formats from the database itself; both are login-walled, so we did not open the export pages. [translators_CN](https://github.com/l0o0/translators_CN) — free community Zotero translators for CNKI, Wanfang, CQVIP and Yiigle; when we read the repository tree there was no SinoMed/CBM translator. [cnki-converter](https://github.com/cmsax/cnki-converter) — free; converts CNKI EndNote files to RefMan, though its web app host did not resolve. [CNKI-PDF-RIS-Helper](https://github.com/Doradx/CNKI-PDF-RIS-Helper) — free userscript, per-article RIS. An EndNote "SinoMed CBM" filter exists but needs EndNote (paid); the download page redirected, so its details are unverified. [Covidence accepts](https://support.covidence.org/help/study-imports) EndNote XML, PubMed text format and RIS, so the EndNote route often lands without any RIS conversion. | Not tested. | Check the encoding and spot-check the field mapping. GBK-versus-UTF-8 faults live in the bytes, which a model reading text cannot see. | 2026-09-17 |
| 25 | Internal consistency of a finished review | Check that the numbers in the abstract, text, tables and forest plots agree, and that the pooled result can be recomputed. | [statcheck](https://statcheck.io/) — free; checks APA-reported t, F, χ², r, Z and Q against their p values, and nothing else. [repro-checker](https://mahmood726-cyber.github.io/repro-checker/) — early; input is pasted JSON, and it recomputes the pooled effect, CI, I²/τ²/Q and k. No licence is stated: there is no LICENSE file and the GitHub API reports none, so the reuse terms are unclear. 0 stars, repository created 2026-06-15, in an account holding 684 public repositories (GitHub API, 2026-09-26). [scrutiny](https://lhdjung.github.io/scrutiny/) — free; single-study granularity only. Nothing we found reads a published PDF, reconciles text, tables and forest plot, and then recomputes under the model the review says it used. | Not tested. | Establish which pooling model and estimator the review actually used before calling a mismatch an error. Otherwise "the model differs" gets reported as "the number is wrong", or the reverse. | 2026-09-17 |
| 26 | Responding to statistical reviewer comments | Turn a reviewer's comment into the analyses that actually need running, plus the reply. | Generic AI response-letter generators exist in Chinese — [aigaixie](https://www.aigaixie.com/x-review-response-generator) and [Scientify](https://acadwrite.cn/learn/journal-reviewer-response) — but neither is meta-analysis specific, neither lists which analyses to run, and pricing is credit-based or unstated. Nothing we found maps a statistical comment to a conditional analysis checklist. | Not tested. | Decide which analyses are defensible, and never describe an analysis that was not run. The failure mode to watch: fluent replies that endorse inappropriate fixes — Egger's test with fewer than ten studies, switching to a fixed-effect model because I² is low, post-hoc subgroups offered as explanations, trim-and-fill presented as a correction. | 2026-09-17 |

### One-line summary of the table

No row in this table is finished. In several steps a free tool already does the mechanical part —
statistical conversions (row 11), figure digitising (12), the pooling itself (17), forest and funnel
plots (18), GRADE tables (19) — and every one of those rows still leaves a judgement in the last
column. Read the free options in the cells rather than the word "free": one of them has an
unverified price, one calls itself under development in its own README, one watermarks its downloads
until you buy a pass, and one caps the free tier at three team members and 25 questions. Most of the
remaining rows have a partial tool with a specific, nameable gap. The steps with no adequate tool at
all are the ones that cross a language boundary, and the ones that ask whether a finished document is
internally consistent.

What the nine measured steps show is narrower than a comparison. The model matched the answer key on
seven of them and was partial on two. On the one task where a purpose-built deterministic script was
also run on the same list, the model scored 24/24 and the script 22/24. No other tool was run on any
of these tasks, so nothing here ranks a model against a tool. We publish a map and a measurement
because that is what we could establish, not because a model beat anything.

## Measured numbers behind the table

Counts re-measured live on 2026-09-26 through the PubMed E-utilities, PMC and Europe PMC APIs.
Percentages from published studies are quoted with their sample size.

**Scale.** PubMed indexed 43,367 records typed as systematic review or meta-analysis for 2024 and
48,220 for 2025 (the 2025 query returned 48,221 on a re-run the same day, so read the last digit as
drift, not precision); 10,839 (25.0%) and 12,602 (26.1%) of those carry a China affiliation. Only 91 of
the 2024 records are in Chinese, so the Chinese-language review literature is essentially not in
PubMed and every count here is a floor for a Chinese audience. PROSPERO's own news page reported
306,444 published records as of 11 December 2024 and about 215 new records per day.

**Search reporting.** Across 100 reviews and 453 database searches, 22 (4.9%) reported all six
PRISMA-S items, 47 (10.4%) could be reproduced within 10% of the original number of results, and
one review gave enough detail to be fully reproducible
([Rethlefsen 2024](https://pubmed.ncbi.nlm.nih.gov/38052277/)). In 137 reviews, 92.7% of search
strategies contained some type of error, and errors affecting recall were the most frequent at 78.1%
([Salvador-Oliván 2019](https://pubmed.ncbi.nlm.nih.gov/31019390/)); the abstract does not state the
denominator of that second figure, and we did not open the full text to settle it. In 607 Chinese-journal
meta-analyses of observational studies, retrieval information was not comprehensive in 85.8%
([Zhang 2015](https://pubmed.ncbi.nlm.nih.gov/26644119/)). In 1,176 Chinese social-science reviews,
18.3% provided a full search strategy ([Guo 2025](https://pmc.ncbi.nlm.nih.gov/articles/PMC11743190/)).
Translating one string by hand took a mean of 45 minutes and produced a mean of 14.6 errors, against
31 minutes and 8.6 errors with tool assistance
([Polyglot trial, 2020](https://pmc.ncbi.nlm.nih.gov/articles/PMC7069833/)).

**Reporting adherence.** In 222 reviews, none adhered completely to PRISMA 2020 and average
adherence was 42.64%. Of the 67 of those reviews (30.18%) that used PRISMA 2020 at all, the
search-strategy item was reported in 8 (11.94%) — one of the four least-followed items
([Ivaldi 2024](https://pubmed.ncbi.nlm.nih.gov/40476264/), Cochrane Evid Synth Methods 2024;2(5):e12074).

**Flow diagrams.** 85.9% of 2024 PMC reviews with "systematic review" or "meta-analysis" in the
title mention a flow diagram or flow chart. In an older sample, 78 of 154 assessed articles
(50.6%) had a flow diagram at all, and the least-reported diagram component was the method of
duplicate removal, at 1%
([Vu-Ngoc 2018](https://pmc.ncbi.nlm.nih.gov/articles/PMC6021048/), PLoS One 13:e0195955; the paper
does not say whether that 1% is of all articles or only of those with a diagram). **Honest gap:** we
found no published study measuring how often a flow diagram's arithmetic fails to reconcile. Every
claim to that effect we could reach was on a tool vendor's blog, so we do not repeat one.

**Extraction errors.** In 27 meta-analyses with two trials sampled from each, 17 (63%) had an error
in at least one of the two trials, and the pooled result could not be replicated within 0.1 in 10 of
them (37%) (Gøtzsche 2007, JAMA 2007;298:430-7). Of 500 primary effect sizes drawn from 33 published
meta-analyses, 224 could not be reproduced from the information reported, with discrepancies in 13 of
the 33 (Maassen 2020, PLOS One 15:e0233107). Those two are error rates. A third study is often
quoted beside them and is a different kind of thing: a methodological review searched for articles
describing errors, and from the 50 articles it included it assembled a bank of 139 distinct error
*types* that can arise
in pairwise meta-analyses — 25 in data extraction or manipulation, 74 in statistical analysis, 40 in
interpretation (Kanukula 2024, J Clin Epidemiol 170:111331). It is a taxonomy of what can go wrong,
not a count of what did.

**Protocol deviations.** In 97 Cochrane and 97 non-Cochrane reviews, more than half of each sample
had changes in PICOS elements, and 95.8% of the changes went unreported in the non-Cochrane reviews
against 42.6% in the Cochrane ones (Siebert 2023, PeerJ 11:e16016). In 96 reviews registered in
PROSPERO, a primary-outcome discrepancy occurred in 32% and 39% did not explicitly specify a primary
outcome (Tricco 2016,
J Clin Epidemiol 79:46-54). Of 75 analysable PROSPERO records, 63 (84.0%) were not up to date
(Rombey 2020, J Clin Epidemiol 117:60-67). Only 4.3% of 2024 open-access reviews contain any
protocol-deviation wording at all — the artefact this step produces is almost never produced.

**Cross-language duplicates.** Of 470 Chinese-sponsored randomised trials published as journal
articles, 55 (11.7%) had 75 duplicates, and 53 of those duplicates (70.7%) crossed a language
boundary. All were identified by hand
([JAMA Netw Open 2020](https://pmc.ncbi.nlm.nih.gov/articles/PMC7716193/)). Secondary publication in
another language is permitted under stated conditions by the ICMJE recommendations, so these are not
all misconduct — which is exactly why a review team has to detect them rather than assume they are
flagged.

**Trustworthiness.** In 95 randomised trials inside 50 Cochrane reviews, assessors had some concerns
about authenticity for 25% and serious concerns for 6%; removing both groups left 22% of the
meta-analyses with no trials at all
([INSPECT-SR stage 2](https://pubmed.ncbi.nlm.nih.gov/40349737/), J Clin Epidemiol
2025;184:111824). The same paper states the limit on that last figure itself: assessment was
restricted to meta-analyses with no more than five RCTs, 54% of which contained only one RCT, "which
will distort the impact on results". The tool is described in a separate
[preprint](https://pubmed.ncbi.nlm.nih.gov/40950444/), which has not been peer reviewed. The
developers measured a median of 45 minutes (IQR 27–74) to assess one trial with the tool. Current
practice is close to zero: 0.27% of 2024 open-access reviews mention GRIM or GRIMMER.

**Chinese-database burden.** 8.7% of 2024 PMC title-restricted reviews mention a Chinese database in
full text, rising to 34.0% among China-affiliated reviews; 93.9% of the reviews that mention one are
China-affiliated. Among those that mention CNKI, 70.4% also mention Wanfang and 44.5% also mention
VIP, so the translation and de-duplication work recurs two to four times per project. Counter-point,
stated plainly: Cochrane's mandatory database set is CENTRAL, MEDLINE and Embase — no Chinese
database — and a [2015 Cochrane Colloquium analysis](https://abstracts.cochrane.org/2015-vienna/searching-chinese-biomedical-databases-current-practice-among-cochrane-reviewers)
found only 243 of the then-published 8,680 Cochrane reviews (less than 3%) searched Chinese
databases, and 118 of those 243 (49%) were on complementary and alternative medicine topics. In one
topic where it mattered, 96.64% of eligible studies could be found only in Chinese databases
([Wu 2013](https://pubmed.ncbi.nlm.nih.gov/24223063/)).

**Publication-bias practice.** In 46 reviews from two top sports-science journals, 47.8% assessed
publication bias when fewer than ten studies were pooled, and 28.3% inspected a funnel plot without
any statistical test ([2024 education review](https://pmc.ncbi.nlm.nih.gov/articles/PMC10933152/)).
The Cochrane Handbook's rule is that funnel-plot asymmetry tests "should be used only when there are
at least 10 studies" ([chapter 13](https://www.cochrane.org/authors/handbooks-and-manuals/handbook/current/chapter-13)).

**Tool interconversion.** 404 reviews in 2024 name both RevMan and Stata in the abstract; 33 name
both RevMan and R. 62% of RevMan mentions and 69% of Stata mentions are China-affiliated. Both
counts are floors, because software names live in the Methods, not the abstract.

## When the evidence changes

A review is a claim about a body of studies at a moment in time. The studies keep moving. One gets
retracted; one gets a correction that changes a number; a preprint you extracted becomes a journal
article with different results. The question a review team then faces is not "was one of my included
studies retracted" — several free tools answer that, as row 16 shows — but the next one: **which of
my pooled results, and which of my conclusions, depended on it.**

The scale of the first problem is measured. Of 1,330 retracted randomised trials, 312 (23.5%)
appeared in 847 reviews covering 4,095 meta-analyses; of the 3,902 that could be recomputed,
removing the retracted trial changed the direction of the effect in 8.4% and statistical
significance in 16.0%, and 68 reviews whose conclusions were distorted were used by 157 guideline
documents, of which 89 (57%) are clinical practice guidelines proper and the rest are consensus and
position statements, practice bulletins and committee opinions
([Xu 2025, BMJ](https://doi.org/10.1136/bmj-2024-082068)). Note what that sentence does not say: it
is 312 trials contaminating 847 reviews, not 1,330.

Almost nothing flows downstream. Where the retraction came after the review was published, 9 of 196
reviews and 2 of 43 guidelines were corrected or retracted (Kataoka 2022, J Clin Epidemiol). And
alerting people does not fix it: a randomised trial emailing the authors of citing papers about
7,958 versus 7,963 retracted papers — 246,749 emails — found no difference in citation rate at one
year (−0.007, 95% CI −0.055 to 0.041), while 80.6% of the authors who replied (12,631 of 15,667)
said they had not known about the retraction (RetractoBot, Peer Review Congress abstract).

The counter-evidence deserves equal billing. A separate study identified 61 reviews containing
retracted studies, extracted data from the 50 of them it could, and recalculated 166 of those
reviews' 173 meta-analyses: 160 (96%) of the recalculated results stayed inside the original
confidence interval, and 18 (11%) changed significance (Graña Possamai 2025, JAMA Intern Med). So
the usual effect of one problem study is small. It is the tail that matters, and you cannot tell
which case you are in without recomputing.

Timing decides what a pre-submission check can achieve. In the same study's 50 extracted reviews, 37
(74%) of the retractions happened *after* the review was published, so a check at submission could
have caught at most 13 of the 50. Requirements point the same way: the ICMJE recommendations make authors responsible for checking
whether their references have been retracted, with PubMed named as an authoritative source; Cochrane
MECIR asks for errata and retractions to be re-checked when a review is updated, by hand; PRISMA
2020 has no item for it at all. In 2024 open-access reviews, 0.48% mention "retracted" anywhere in
the methods.

One small, partial answer to the "which of my conclusions depended on it" step is the author's own
tool, **Evidence OS Inspector** — browser-only, MIT licensed, alpha (repository read 2026-09-26). It
binds claims to the source passages they came from, and flags which claims need re-review when a
source changes. It does not discover that a source changed, does not bind to numbers, and has no
measured effect on anybody's time. It is one modest piece of the step, not a solution to it.
**Conflict of interest:** it is the author's own tool. It is not a row in the table, it was not
benchmarked, and, like every tool named on this page, it is listed rather than endorsed.
[Repository](https://github.com/yinchuan123/evidence-os-inspector) ·
[Demo](https://yinchuan123.github.io/evidence-os-inspector/)

## How this map was made

**The prior-art pass (2026-09-17).** Four agents took five candidate jobs each. For every job: at
least two search strategies in English and in Chinese, plus the GitHub search API, the SR Toolbox
catalogue, and Chinese platforms. A tool was listed with a link only if its own page — website,
repository, CRAN page, documentation or the developers' own paper — was opened; a handful of tools
are named without a link because they reached us through a catalogue listing or another tool's
documentation, and those rows say *page not opened*. Pages that could not be opened were
recorded as such: the SR Toolbox is now a Streamlit app that returned only the word "Streamlit" to a
fetcher; TERA and several vendor sites are JavaScript apps with no readable text; CMeSH, SinoMed
search, Covidence and GRADEpro's application are behind logins we did not attempt; Zhihu returned
403. No sign-up, login, payment or form submission was performed anywhere.

**The need pass (2026-09-26).** Three agents measured, for each job: volume (live counts through the
PubMed E-utilities, PMC and Europe PMC APIs), requirement (the guideline or journal text, opened and
quoted), and published problem rate. Counter-evidence was mandatory — every job has an "against"
paragraph in the working notes, and the strongest of those are in the section above.

**The benchmark pass (2026-09-26, plus 2026-09-16 for the retraction task).** Nine tasks built from
real public material. Each task, its answer key and its scoring rule were produced by a separate
agent before the scored run, with every key item established by a verbatim quote or by arithmetic
written out. The model under test was Claude Fable 5.1 in an agent harness, allowed web search and
page fetching only; the call log was audited afterwards for local file reads and none were found.
Scoring was a separate pass, item by item. Full method, task list and results:
[LLM_BENCHMARK.md](LLM_BENCHMARK.md).

**Measured versus assumed.** Counts, prices and tool capabilities were measured or quoted from the
page. The LLM column is measured for 9 rows and absent for 17 — "not tested" means untested. The
prior-art pass also produced a hypothesis about how a model would fail at each job; those
hypotheses are **not** in this map, because for the nine jobs we did test, two of the predicted
weaknesses did not materialise.

**Limitations, stated plainly.**

- One task per job, run once, one model family — except the retraction task, which was run twice
  with web access and once without. Single runs cannot separate skill from luck.
- 17 of the 26 rows have no LLM measurement.
- The prior-art pass was one reviewer per candidate with no adversarial second pass.
- GPT-6 was not tested; neither was any other vendor's model.
- The benchmark ran inside an agent harness that can fetch API responses directly, so an ordinary
  chat interface may do worse.
- The answer keys were assembled with model assistance from primary sources, so a key can be wrong —
  and once it was: on the retraction task the model found a published correction the key had missed,
  and the key was amended before scoring.
- "Mentions a database in full text" is a proxy for "searched that database", not a determination.
  A mention can sit in a reference title or a limitations sentence.
- The PMC denominator is open-access-skewed, about 59% the size of the PubMed count for the same
  year. `China[affiliation]` matches any affiliation string containing "China" and misses Taiwan and
  some Hong Kong forms.
- Web search in the tooling we used is US-centric, so Chinese forum and BBS demand is
  under-sampled; Chinese figures here are floors.
- One widely repeated figure — "20.6% of search strategies were not appropriately translated for
  other databases" — is **not** in the paper it is usually attributed to. We checked the full text
  and discarded it. Please do not cite it to us.
- Nothing here is a clinical validation of anything, and nothing here says a model output should be
  used without a human check.

## Corrections welcome

This map is wrong in places. Prices change, tools ship the missing feature, and a row written from a
vendor page can misread that page. If you can fix a row, please do:

- **A tool is missing, mispriced or misdescribed** — open an issue or a pull request with the tool's
  own page and the date you opened it. One tool per issue.
- **You built a tool listed here** — corrections about your own tool are welcome and are not treated
  as a conflict of interest. Say it is yours so the row can record who reported the change. An
  accurate limitation will not be removed on request; an inaccurate one will be fixed, and a
  limitation you have since fixed will be gladly recorded as fixed.
- **A benchmark result looks wrong** — the tasks, keys and scoring rules are published so you can
  check them. Say what you reran.

How this works in practice: one maintainer, no service level. A row is re-dated when someone opens
an issue with the tool's own page and the date they opened it; a row nobody re-checks keeps the date
it already carries, which is why the date is printed in every row.

The rules, the evidence bar and the issue form are in [CONTRIBUTING.md](CONTRIBUTING.md). Short
version: the tool's own page, the date you opened it, and `unverified / 未核实` instead of a guess.

## About the author

**Chuan Yin (尹川)** — Shanghai Jiao Tong University
ORCID [0009-0005-2830-8167](https://orcid.org/0009-0005-2830-8167)
Email `yinchuan [at] sjtu.edu.cn`

Contact: [yinchuan@sjtu.edu.cn](mailto:yinchuan@sjtu.edu.cn)

## Licence

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — see [LICENSE](LICENSE).
