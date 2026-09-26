# The benchmark: can a web-enabled LLM already do the routine chores of a systematic review?

[简体中文](LLM_BENCHMARK.zh-CN.md) · Part of the [Evidence Synthesis Automation Map](README.md)

Run 2026-09. Nine tasks, one model, single runs — the retraction task was run twice with web access
and once without. A small probe, not a validated benchmark.

---

## 1. The question

Several steps of a systematic review are chores. They have a right answer that someone else can
check: does the flow diagram subtract correctly, do the two extractors' spreadsheets agree, is each
included study still un-retracted, does this search string run in that database. Tools exist for
some of them. Some tools are free, some cost money, some were built for one database only.

The question this probe asks is narrow:

> On chores of that kind, does a large language model with web search and page fetching already
> produce output as good as a purpose-built tool?

The honest answer we measured is: on the tasks we tested, mostly yes — and that is a negative
result for anyone thinking of building another small tool. It is **not** a statement that the model
is reliable. The most useful thing in this report is section 6, where the same model, same prompt,
loses its web access and starts producing confident false reassurances.

## 2. What already exists, for comparison

Capabilities, prices and links checked 2026-09. Prices change; treat the check date as part of the
claim. "Free" means free at the time of checking, not free forever.

The right-hand column comes from a single-reviewer tool survey with no adversarial second pass. Read
every "we found no tool" as an invitation to send a correction, not as an established absence.

| Chore | Purpose-built tools that already do some of it | Price as stated | What they leave out |
|---|---|---|---|
| PRISMA flow-diagram arithmetic | [PeerReviewAI](https://peerreviewai.org/guides/prisma-checklist) PRISMA review; diagram builders that warn while you type | Part of a $49 AI peer review; builders free | We found no free standalone checker that takes an existing diagram plus the manuscript and reconciles the boxes against the abstract, text and tables. |
| PRISMA 2020 checklist mapping | [PRISMA's own checklist web app](https://prisma.shinyapps.io/checklist/); [PRISMA-AI](https://github.com/youkiti/PRISMA-AI-Share) (code and benchmark only); PeerReviewAI | Free; free (MIT); $49 tier | The official app records your own answers. The PRISMA-AI benchmark's own figures, measured across ten different models rather than for one tool: 45.2% accuracy when a model is given the manuscript only, and 78.7–79.7% when the blank canonical PRISMA 2020 checklist is supplied alongside it in a structured form, with 70.6–82.8% across models ([paper](https://arxiv.org/abs/2511.16707)). What varies there is whether the model was handed the checklist, not whether it was handed the authors' filled one. Paid reviewers return adequate / incomplete / missing without a location. |
| Two-extractor reconciliation | [Covidence](https://www.covidence.org/) comparison and consensus; [Diffchecker Excel Compare](https://www.diffchecker.com/excel-compare/); [daff](https://github.com/paulfitz/daff) | Covidence [$339 per review per year](https://www.covidence.org/pricing/), free for Cochrane reviews; Diffchecker free to compare, paid export; daff free (MIT) | Covidence [fills the consensus column automatically](https://support.covidence.org/help/comparison-and-consensus) where both reviewers entered the same value and shows "a decision is required" for the rest; it aligns the two forms field by field inside its own template. Whether differences *inside a data table* are surfaced was not established in this run: the "modified by either reviewer will be highlighted, [except within data tables](https://support.covidence.org/help/consensus-9831e0af)" sentence is written about re-publishing a changed template, not about comparing two extractors. Diffchecker aligns by position, not by study × arm × outcome × timepoint. AHRQ's SRDR+ comparison tool was free; AHRQ's closure notice gives a final day of operation of 28 November 2025 (effectivehealthcare.ahrq.gov/news/ceasing-operations returns an empty body to scripted requests, so that date comes from the page's indexed title and snippet, not from a page we opened). |
| Search appendix and hit-count archive | [CiteSource](https://cran.r-project.org/package=CiteSource); [SLR Harvester](https://github.com/socresearcher/slr-harvester); [Nested Knowledge](https://about.nested-knowledge.com/docs/prisma-chart/) | Free (CRAN); free for non-commercial use (source-available, all rights reserved); commercial, price not checked | None of them records platform, search date, line-by-line strategy and per-database hit counts for the mixed English/Chinese database set that a China-based review actually uses. |
| Trial-registration audit | [RegCheck](https://regcheck.app); [TRNscreener](https://github.com/bgcarlisle/TRNscreener); the [ChiCTR registry's own prospective/retrospective label](https://www.chictr.org.cn/regstatusprojEN.html) | Public web app free per [its paper](https://arxiv.org/abs/2601.13330) ("using RegCheck is entirely free for users"; the site itself states no price), code AGPL-3.0; free, no licence stated; free | RegCheck compares one pair at a time and supports ClinicalTrials.gov and OSF. TRNscreener only finds whether a number is reported. ChiCTR is one record at a time and its record pages are rendered by JavaScript. |
| Baseline-table numeric screening | [INSPECT-SR guidance site](https://www.inspect-sr.com); a free [GRIM/GRIMMER/TIDES app](https://errors.shinyapps.io/inspect-sr-means-variances/); [baseline](https://github.com/agbarnett/baseline) | Guidance site and GRIM app free; `baseline` free but CC BY-NC-ND 4.0, so non-commercial and no derivatives | The guidance site has no calculators. The GRIM app needs values typed in row by row. `baseline` reads a template or a PMC paper, so a Chinese-language PDF is out of scope. |
| Search-string translation | [Polyglot Search Translator, now at TERA](https://tera-tools.com/); [Embase's PubMed-to-Embase translator](https://www.elsevier.support/embase/answer/pubmed-to-embase-translation-tool); [QueryStrategist](https://github.com/2025247378/QueryStrategist) | Free (account optional); inside an Embase subscription; free (MIT), needs your own LLM key | A library guide for Polyglot states it ["DOES NOT CHOOSE RELEVANT SUBJECT HEADINGS. It just adjusts the syntax"](https://library.svhm.org.au/literature_searching/polyglot); its target list contains no Chinese-language database. QueryStrategist builds a query from an idea rather than translating an existing string. |
| Collating reports into studies | [Covidence merge studies](https://support.covidence.org/help/merging-and-unmerging-studies); [Trials to Publications](http://arrowsmith.psych.uic.edu/cgi-bin/arrowsmith_uic/TrialPubLinking/trial_pub_link_start.cgi); [refs2study](https://github.com/L-ENA/refs2study) | Paid; free; free research code, no licence stated | Covidence merging is manual. Trials to Publications covers ClinicalTrials.gov and PubMed only. No tool we found proposes cross-language candidate pairs. |
| Retraction / correction check | [refcheck](https://github.com/KaizenShogun/refcheck); [Europe PMC Article Status Monitor](https://europepmc.org/ArticleStatusMonitor); Zotero; EndNote 20.2+; [scite](https://scite.ai/); [Covidence integrity alerts](https://support.covidence.org/help/how-covidence-displays-research-integrity-alerts) | Free to scite Basic, which [displayed](https://scite.ai/pricing) $14/month billed yearly with a 30%-off promotion ending 2026-09-30; a check earlier in the month read $20 | They flag the reference. None of them says which of your own pooled estimates changes once the study is removed. |

## 3. Why these chores and not others

These nine were picked because they come up often and because guidelines ask for them. A sample of
the frequency evidence, each figure traceable. The counts we ran ourselves are listed again after the
list with the exact query behind each one, so you can re-run them rather than take our word.

- **Volume.** PubMed indexed 48,221 records typed as systematic review or meta-analysis for 2025
  and 43,367 for 2024; 12,602 (26.1%) and 10,839 (25.0%) respectively carry a China affiliation
  (PubMed [E-utilities](https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esearch.fcgi?db=pubmed&term=test&retmode=json),
  re-run 2026-09-26; the 2025 count read 48,220 earlier the same day, so treat the last digits as
  drift).
- **Flow diagrams are near-universal.** 21,890 of 25,491 reviews with "systematic review" or
  "meta-analysis" in the title, published 2024 and in PubMed Central, mention a PRISMA flow diagram
  or flow chart (85.9%; counted via E-utilities, 2026-09-26).
- **Searches are rarely reproducible.** Across 100 reviews and 453 database searches, 22 (4.9%)
  reported all six key PRISMA-S items
  ([Rethlefsen 2024, J Clin Epidemiol](https://pubmed.ncbi.nlm.nih.gov/38052277/)).
- **Published search strategies carry errors.** Across 137 systematic reviews, the proportion of
  search strategies containing some type of error was 92.7%, and errors affecting recall were the
  most frequent (78.1%)
  ([Salvador-Oliván 2019, J Med Libr Assoc](https://doi.org/10.5195/jmla.2019.567)).
- **Extracted numbers carry errors.** In 27 meta-analyses with two trials examined in each, 17 (63%)
  had errors in at least one of the two trials, and in 10 (37%) the result could not be replicated
  within 0.1 ([Gøtzsche 2007, JAMA](https://doi.org/10.1001/jama.298.4.430)).
- **Dual extraction is common; the machinery around it is rarely reported.** 4,806 of 15,307
  open-access reviews published 2024 (31.4%) use an explicit dual-independent-extraction phrase
  (Europe PMC, 2026-09-26). In a separate sample of 152 reviews, 52 (34%) reported procedures to
  avoid data errors, and 9 (6%) reported using Covidence to assist data extraction — the most
  frequent automation tool in that sample
  ([Büchter 2021, BMC Med Res Methodol](https://doi.org/10.1186/s12874-021-01438-z)).
- **Duplicate reports cross languages.** Of 470 Chinese-sponsored randomised trials, 55 (11.7%) had
  75 duplicate publications, 53 of the 75 (70.7%) across a language boundary
  ([Jia 2020, JAMA Netw Open](https://doi.org/10.1001/jamanetworkopen.2020.27104)).
- **Registration timing is checkable and often retrospective.** The ChiCTR registry's
  [own counters](https://www.chictr.org.cn/aboutEN.html) read "Complete registration 130930 /
  Prospective registration 106156 / Retrospective registration 24376" when opened on 2026-09-26;
  24,376/130,930 = 18.6% is our division of two of its counters, not a figure the page prints. The
  registry's separate register-status page gave a different split the same day (106,397 prospective,
  81.26%; 24,530 retrospective, 18.74%), so its own counters do not agree with each other to the
  last digits.
- **Baseline-number screening is almost never done.** 42 of 15,307 open-access reviews published
  2024 (0.27%) mention GRIM or GRIMMER (Europe PMC, 2026-09-26) — while interviews with the authors
  of 2,235 of 3,137 reports labelled as randomised in Chinese journals found only 207 that met an
  acceptable randomisation standard. The article reports that as "6.8% (95% confidence interval
  5.9-7.7)", a percentage whose denominator is the apparent-trial sample rather than the 2,235
  authors interviewed; 207/2,235 would be 9.3%
  ([Wu 2009, Trials](https://doi.org/10.1186/1745-6215-10-46)).
- **Trustworthiness checks are being formalised.** The INSPECT-SR tool "contains 21 checks across
  four domains" ([its guidance site](https://www.inspect-sr.com), which is where that count is
  printed; also in the tool preprint, PMC12424918 — a preprint, so treat it as such), including
  registration timing, registry-versus-publication inconsistency, implausible baseline data and
  impossible integer means and variances. The separate Stage 2 feasibility study is Wilkinson et
  al., [J Clin Epidemiol 2025](https://doi.org/10.1016/j.jclinepi.2025.111824).

### The counts we ran ourselves

All re-run 2026-09-26. Paste these and you should get the same numbers, or a later drift; without the
query string a count like this is not checkable, which is the only reason it is printed here.

PubMed, `esearch.fcgi?db=pubmed`:

```
("systematic review"[pt] OR "meta-analysis"[pt]) AND 2025[dp]                    48221
("systematic review"[pt] OR "meta-analysis"[pt]) AND 2024[dp]                    43367
... AND China[ad], for each of those two years                             12602 / 10839
```

PubMed Central, `esearch.fcgi?db=pmc` — denominator, then the same term plus the flow-diagram clause:

```
("systematic review"[title] OR "meta-analysis"[title] OR "meta analysis"[title])
  AND 2024[pdat]                                                                 25491
  ... AND ("PRISMA flow" OR "flow diagram" OR "flow chart" OR flowchart)          21890
```

Europe PMC, `webservices/rest/search` — one denominator, two numerators. The denominator is defined by
Europe PMC's publication-type facet, not by a title match: swapping `PUB_TYPE:` for `TITLE:` gives
23,678 instead of 15,307, which would move the 31.4% share to roughly 20%. These shares are
denominator-sensitive, so the denominator query is the part to check first.

```
(PUB_TYPE:"Systematic Review" OR PUB_TYPE:"Meta-Analysis")
  AND FIRST_PDATE:[2024-01-01 TO 2024-12-31] AND OPEN_ACCESS:Y AND IN_EPMC:Y      15307
  ... AND ("independently extracted" OR "two reviewers independently"
           OR "two authors independently" OR "extracted in duplicate"
           OR "independently by two")                                             4806
  ... AND ("GRIM" OR "GRIMMER")                                                      42
```

## 4. Method

**Task construction.** Each task was built by a separate agent that opened the primary sources
itself in the same session. For every task that agent produced three artefacts before the model
under test saw anything:

1. **The task text** — real material, transcribed verbatim, with nothing altered, rounded or
   reordered, and with no wording that hints at the answer.
2. **The answer key** — item by item, each item established by a verbatim quote from the source or
   by arithmetic written out in full. Nothing from memory. Where the key could not be established,
   it says so instead of guessing.
3. **The scoring rule** — a scheme fixed before the model answered, points per item in some tasks and
   bands over item groups in others, *plus* a separate list of "dangerous errors" that fail the task
   regardless of the point total: a confident wrong verdict, an invented identifier or registry
   field, or silently returning fewer items than were asked for.

Where the chore has a **false-positive surface** — items whose correct answer is "this one is fine" —
the keys score it, and inventing a problem that is not there is on every dangerous-error list. Three
of the eight keys enumerate that surface item by item: the flow-diagram task lists its fourteen
balancing checks one by one, and the baseline-screening and two-extractor tasks award explicit credit
for not misjudging the cells and rows that are correct. The other five catch over-flagging through
the dangerous-error list instead of counting it. Not crying wolf is half the job, and in the tasks
where it is counted it is close to half the score.

**The model.** Claude Fable 5.1, running inside an agent harness. It was allowed web search and page
fetching and nothing else — no local files, no access to the key or the scoring rule. Call logs were
audited afterwards for local-file reads; none were found.

**Scoring.** A separate pass, item by item, against the written key. Scoring was done by a different
session from the one that answered, and by a different one again from the session that built the
task. It was not a blinded panel of humans.

**Runs.** Each of the eight main tasks was run **once**. The earlier retraction task was run twice
with web access and once with web access removed. That is the whole sample. Single runs cannot
separate skill from luck, and we make no claim that they can.

**Dates.** The eight main tasks were built and answered 2026-09-26. The retraction task was built
and answered 2026-09-16. Its 24-item list deliberately included notices published after the model's
training cut-off.

## 5. Results

"Matched the key" means the scoring rule's pass threshold was met with zero dangerous errors.
"Partial" means the pass threshold was missed on content, again with zero dangerous errors. No task
produced a dangerous error.

| # | Task | Result | Notable behaviour |
|---|---|---|---|
| 1 | **PRISMA flow arithmetic and cross-document agreement.** One flow diagram, the abstract, two Results paragraphs and two characteristics tables; find every internal contradiction. | Matched the key | Found the one broken subtraction — a 9-record gap between the eligibility and included boxes — and left all fourteen checks that balance alone, including two that require adding fourteen hand-transcribed integers. Said explicitly that the printed numbers cannot tell you *which* of three boxes is wrong. Also located the article's published correction notice, which the key did not require. |
| 2 | **PRISMA 2020 checklist location mapping.** 14 checklist items against one open-access review with non-standard headings; give status, location and a quoted sentence per item. | Matched the key | All 14 statuses and locations, including the two items that genuinely are not reported anywhere in the article. Did not invent a Funding or Data Availability section for a journal layout that has none, and did not land on the Methods risk-of-bias heading for the Results item. |
| 3 | **Two-extractor spreadsheet comparison.** Two independently filled extraction sheets, aligned by study × arm × outcome × timepoint. | Matched the key | Found all 6 value differences, all 6 format-only differences and both coverage differences, with no false positives, and adjudicated correctly in the planted case where the *second* extractor was the one who had it right. |
| 4 | **Search-reporting appendix against PRISMA-S.** Nine databases, their strategies and their hit counts; produce the appendix and say what is missing. | Matched the key | Reproduced all nine source names and hit counts without inventing a platform or vendor name for the sources that never state one. Derived the total the article never prints (1,687) and showed it cannot be reconciled with the 107 records that entered screening, and that the step between them rests on one author's judgement of apparent relevance. |
| 5 | **Trial-registration audit.** Ten included trials across two registries: does the number resolve, was registration prospective, do registered and published primary outcomes agree. | Matched the key | 10/10 on number resolution — including one number printed incorrectly in the article, for which it located the real record and said the printed string was wrong rather than quietly substituting. 10/10 prospective versus retrospective. 4/4 primary-outcome mismatches, quoting the registry field each time. |
| 6 | **Baseline-table numeric screening.** Eight mean ± SD cells for GRIM/GRIMMER, the subgroup counts, and a nine-item reporting checklist. | Matched the key | Computed all three arithmetically impossible means and showed the arithmetic — one of them unconditionally, the other two only under a rounding convention and an integer-granularity assumption that the key required it to state, and it stated them. Flagged none of the five cells that are reachable, and found the subgroup counts that sum to 38 of 40 with percentages summing to 95%. Invented no registration number for an article that prints none — the single failure the scoring rule punished hardest. |
| 7 | **Search-string translation into three Chinese-language databases.** Convert a published English strategy into runnable CNKI, Wanfang and SinoMed expressions, with field codes and term provenance. | **Partial** | Syntax was legal on all three platforms, no field code was invented, it correctly refused to treat the source strategy's asterisk as truncation on platforms that have no such operator, and it dropped the country concept exactly as the published Chinese strategies do. But its subject-term list was not the published one: of the four published Chinese terms for social anxiety, one matched exactly, a second was reached only because a shorter stem it printed is contained in it, a third it excluded deliberately and said why, and the fourth is absent. The task had asked it to name what it could not verify, and it used that line correctly: it named one platform's subject-heading form as something it could not check against that platform's own thesaurus, instead of returning a vacuous "nothing". |
| 8 | **Cross-language duplicate-report detection.** 34 records down to studies; flag duplicate and overlapping-sample reports and the look-alikes that are *not* the same trial. | **Partial** | 6 of 7 clusters exactly right, including the Chinese/English same-cohort pair that the task was built around. One cluster absorbed two records it should have left out — but it stated its reasons, and it flagged the unresolved sample-size conflict inside that cluster rather than smoothing it over. Every record was given a status; none was silently dropped. |
| 9 | **Retraction, correction and expression-of-concern check** of a 24-item included-study list (built earlier, 2026-09-16). | Matched the key, on both runs | With web access: 24/24 twice, and zero items wrongly declared clean. It surfaced a published correction attached to the journal version of one record that the key had missed; the key was amended before scoring. A purpose-built deterministic script over the same three public APIs scored 22/24 on the same list. **Without web access the same model, same prompt, scored 14/24 and produced 6 confident false reassurances.** |

Tally: 7 of 9 matched the key, 2 partial, 0 poor, 0 dangerous errors — on single runs.

### Cost and time, for scale

The web-enabled runs of the retraction task took about 6 minutes each and made 67 and 75
search-or-fetch calls. The deterministic script took 222 seconds because it was written to query one
record at a time. Neither number is a benchmark of anything; they are here so nobody imagines the
model answers instantly or that the script is slow by nature.

### One place the key was wrong, and one place a correction notice was

The answer key is the weakest part of any exercise like this.

- In the retraction task, the model found a published correction the key had missed. The key was
  amended. That is the one key error in the set, and the model found it.
- In the flow-diagram task, the published correction notice for the source article lists three
  changed values, while comparing the two published versions of the figure shows six values changed.
  Here the incomplete document was the *notice*, not the key: the key had been written to rest only
  on the arithmetic of the numbers as printed, precisely so that it would not inherit the notice's
  gaps. We found the six-value difference by reading the two figure bitmaps while building the task.
  The model got close from the outside: it worked out that the corrected diagram still does not
  reconcile unless two further boxes also changed, said it could not read those two boxes from the
  image, and told the reader to go and look at them. That is the right shape of answer, and it is not
  the same as having found it.

Both are recorded because they cut against the model's score being taken at face value: if a key can
be incomplete, "matched the key" is a weaker statement than it sounds.

## 6. What this does and does not show

**It does not show that LLMs are reliable for this work.** Specifically:

- **Nine tasks, one run each** (two for the retraction task). Single runs cannot distinguish a
  capability from a good roll. Nothing here is a variance estimate.
- **One model, one family.** Other models were not tested. A newer or older version of the same
  model was not tested.
- **The harness matters.** The runs happened in an agent environment that can fetch API responses
  directly — PubMed E-utilities, Crossref, Europe PMC, registry APIs. An ordinary chat interface
  with web search may do considerably worse, and results from one should not be read as results for
  the other.
- **Mostly English, mostly open access.** Except for the two Chinese-language tasks, the material
  was English and reachable without a subscription. Paywalled PDFs, scanned documents and
  Chinese-database-only records were not tested end to end.
- **No scale test.** The largest list was 34 records; the retraction list was 24. Nothing here says
  what happens at 100 or 400 included studies, where silent drops are the obvious risk.
- **No adversarial user.** The task material was internally coherent and the questions were fair.
  Nobody tried to push the model toward a wrong answer, and nobody tested what happens when the
  input itself is contradictory or manipulated.
- **The keys were built by agents too.** They were built from primary sources with verbatim quotes
  and were checked, but that is not the same as an independent second human extractor, and one of
  them turned out to be incomplete.
- **The comparison script is not an independent yardstick.** The 22/24 script and the answer key for
  the retraction task queried the same three public APIs, so the script's score on items those APIs
  expose is partly circular: it can only be right about what they show. Both items it lost were
  items they do not carry — a correction attached to a journal version, and a citation with no
  identifier of any kind. Read "24/24 versus 22/24" with that in mind.
- **Scoring was not blinded.** It was a separate pass against a written key, not an independent
  human panel.
- **No clinical or regulatory validation.** None of this supports using a model's output as a
  finding in a review without a human check.

**The failure mode that matters is documented, and it is not incompetence.** Take the web away and
the same model on the same prompt reported 6 of 24 flagged records as clean, in confident prose,
with no usable notice numbers because everything came from memory. A missed retraction that arrives
as "no issues found" is worse than no check at all, because it is silent and it is reassuring. Every
practical recommendation below follows from that one observation.

## 7. Practical guidance for reviewers

### Chores worth handing to a model — with web access on

Ranked roughly by how well it went here: two-extractor reconciliation; flow-diagram and
cross-document arithmetic; checklist-to-location mapping; assembling a search appendix and naming
what is missing from it; retraction and correction lookup; trial-registration audit; baseline-number
arithmetic; grouping records into studies. The two that came back partial — translating a search
strategy into another database's controlled vocabulary, and the hardest merge decisions — are the
ones to treat as a draft rather than an answer.

### What to demand in the output

Write these into the request. They are what made the failures in this run visible.

1. **One row per item.** Never a summary verdict. A verdict without rows hides everything.
2. **A source per row** — the URL, identifier or registry field it actually opened. Not "according
   to the literature".
3. **The arithmetic written out.** If it says a number is impossible, make it show the calculation.
4. **An explicit "cannot verify", with the step it got stuck at.** This is the single most valuable
   line in any of these answers. Ask for it by name. In this run the task that asked for it got it,
   and got it right: the model named a specific subject-heading form it could not check against the
   platform's own thesaurus. Asking is why the line was there, which is the whole point of putting it
   on this list.
5. **The negative rows printed too.** Items where it found nothing must appear as rows saying so.
   An item that does not appear looks identical to an item that was fine.
6. **The count it processed, stated back to you.** "24 of 24 records checked" makes a silent drop
   visible. Absence of a row is the failure you will not notice.
7. **A refusal to answer from memory.** For anything whose answer can change — retraction status,
   a registry record, a hit count — no web access means no answer, not a remembered one.

### What to still check by hand

- **Anything behind a login, a CAPTCHA or a JavaScript-rendered page.** In this run the model routed
  around one such registry through the WHO mirror and said so — good behaviour, but the mirror is
  not always equivalent, and the alternative route is not always right.
- **Controlled-vocabulary term lists.** One of the two clear content failures here was *completeness*
  of a Chinese subject-term list, not legality of syntax. A strategy that is missing a term still runs,
  returns fewer records, and reports no error. Have a librarian or information specialist check the
  terms.
- **Every merge that changes the pool.** Deciding that two reports are one study deletes a trial
  from your analysis; deciding they are two double-counts a cohort. The model's one content error
  in the duplicate task was exactly this kind. Treat every proposed merge as a proposal.
- **Every number that reaches the manuscript.** Re-derive it from the source, once, by hand.
- **Any claim that something does not exist.** "No registration number is reported", "no data
  availability statement" — a whole-document negative is easy to state and easy to get wrong.
  Search the document yourself.
- **Anything the model was confidently brief about.** Confidence and brevity together were the
  signature of the no-web failures.

## 8. Where these chores sit in a review

For readers mapping this onto their own workflow. The middle column is what a web-enabled model was
tested on here; the right column is what this probe says nothing about.

| Stage | Tested here | Not tested |
|---|---|---|
| Question, protocol, registration | — | Everything. |
| Search design | Translating a strategy into other databases' syntax and vocabulary (partial) | Whether the strategy is any good; sensitivity and precision. |
| Running and archiving the search | Reconstructing a PRISMA-S search appendix from the record; naming what is missing | Actually running searches; live hit counts, which drift. |
| Deduplication and collating reports into studies | Grouping 34 records into studies, cross-language, with look-alikes (partial) | Scale beyond a few dozen records; records held only in Chinese databases. |
| Screening | — | Title/abstract screening at any scale; full-text eligibility judgement. |
| Data extraction | Comparing two completed extraction sheets cell by cell | Extraction itself, from PDFs or figures. |
| Risk of bias and trustworthiness | Registration audit; GRIM/GRIMMER arithmetic; a reporting-item checklist | Risk-of-bias *judgement*; the domains that need reading, not arithmetic. |
| Synthesis and statistics | — | Pooling, heterogeneity, subgroup and sensitivity analysis, figures. |
| Certainty of evidence | — | GRADE ratings and downgrade reasons. |
| Reporting and submission checks | Flow-diagram arithmetic and cross-document agreement; PRISMA 2020 item-to-location mapping | Whether the reporting is *good*, as distinct from present. |
| After publication | Retraction, correction and expression-of-concern lookup | Deciding what a retraction does to your own pooled estimate — and no tool we found does this either. |

Two readings of that table matter. First, everything tested is a **checking or clerical** step with
a verifiable answer; nothing tested is a judgement call, and the judgement calls are where a review's
credibility actually lives. Second, the untested column is not untested because it is easy.

## 9. Data statement

The tasks were built from real, published material: open-access reviews and protocols, trial
registry records, and official reporting guidelines reproduced under CC BY with their attribution
lines carried along. No number in any task was altered, rounded or reordered.

**By deliberate decision, no individual article is named in this report as having a numeric
discrepancy, a registration discrepancy, an unreproducible search, or a duplicate publication.**
This report exists to measure a model, not to audit authors, and a single-run model answer is not a
sound basis for a public allegation about anyone's paper. Named examples appear only where the
problem is already a matter of public record — a published retraction, correction or expression of
concern — and then only as a neutral illustration; the one such illustration in this report is in
section 5, and it is about how completely a correction notice describes itself.

The task material can be sent on request for verification: the task text, the answer key with its
verbatim quotes, the scoring rule, and the model's unedited answer. Not an item-by-item score sheet —
the scoring pass was done item by item against the key, but what was written down per task is the
verdict and the specific misses, and we will not describe that as more than it is.

Two things to know before asking. The material **names** the articles and trials each task was built
from, including the ones this report declines to name, because a key cannot quote a source without
identifying it. So it is sent one task at a time, to people who say which task and why, on the
understanding that a single model run is not a finding about anyone's paper. Requests from the authors
of the articles used are especially welcome. Ask by email: `yinchuan [at] sjtu.edu.cn`.

## 10. Reproducing this

See [benchmark/README.md](benchmark/README.md) for the method in a form you can run on your own
material, with the pitfalls we hit.

## 11. About the author

**Chuan Yin (尹川)** — Shanghai Jiao Tong University
ORCID [0009-0005-2830-8167](https://orcid.org/0009-0005-2830-8167)
Email `yinchuan [at] sjtu.edu.cn`

Public tool by the same author: **Evidence OS Inspector** — browser-only, MIT licensed, alpha. It
binds claims to the passages they came from, and flags which claims need re-review when a source
changes. [Repository](https://github.com/yinchuan123/evidence-os-inspector) ·
[Demo](https://yinchuan123.github.io/evidence-os-inspector/)

Contact: [yinchuan@sjtu.edu.cn](mailto:yinchuan@sjtu.edu.cn)

Corrections are welcome, and corrections to this page are more welcome than agreement with it. If
you think a result here is wrong, ask for the task material.

## Licence

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Quoted material from PRISMA 2020,
PRISMA-S and the published articles used in the tasks remains under its own licence and
attribution.
