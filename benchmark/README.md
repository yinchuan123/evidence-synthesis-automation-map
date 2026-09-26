# How to run this benchmark on your own material

The results are in [LLM_BENCHMARK.md](../LLM_BENCHMARK.md) ([简体中文](../LLM_BENCHMARK.zh-CN.md)).
This page is the method, so you can repeat it on chores and material of your own.

No code is needed. The whole method is a discipline about the order you do things in.

## The one rule everything rests on

**Build the answer key from primary sources before the model answers, and never from memory.**

If the key is written after you read the model's output, you are not measuring the model — you are
agreeing with it. If the key is written from what you remember about a paper, you are measuring two
memories against each other. Every item in the key needs either a verbatim quote from a source you
opened, or arithmetic written out in full.

## Ten steps

1. **Pick a chore with a checkable answer.** Does the flow diagram subtract correctly. Do the two
   extraction sheets agree. Is this mean possible for this N. Does this registry record match the
   published outcome. Avoid anything whose answer is a matter of judgement — you will end up
   scoring taste.

2. **Find real public material and open it yourself.** Log every URL and the date you read it.
   Prefer open access, and prefer a rendering you can quote exactly. Record which rendering you
   used, because two published versions of the same article can differ in exactly the numbers you
   are testing.

3. **Transcribe the task text verbatim.** Change nothing: no rounding, no reordering, no tidying of
   punctuation. Do not let the wording hint at the answer — if the answer is "these numbers do not
   reconcile", the task must not contain the word "inconsistent".

4. **Write the answer key, item by item.** Each item carries its evidence: the quote, or the
   calculation. Where you could not establish an item, write that you could not, and leave it out of
   the required set or mark it as a bonus.

5. **Count your false-positive surface.** List the items where the correct answer is "this one is
   fine", and score them. A solver that finds your one real problem *and* invents four others has
   not done the job. In our set one task had one true positive surrounded by fourteen checks that
   balance, two of which required adding fourteen hand-transcribed integers — a classic place to
   produce a confident wrong total.

6. **Plant at least one item whose correct answer is "cannot be determined".** This is the
   calibration test, and it is the one most benchmarks leave out. In our flow-diagram task the
   printed numbers are compatible with three different boxes being the wrong one; naming one of them
   as a fact is over-claiming, and the key says so.

7. **Write the scoring rule before the model answers.** Two parts:
   - **Points per item**, so partial credit is defined in advance.
   - **A separate list of dangerous errors** that fail the task regardless of the point total. Ours
     were: a confident wrong verdict; an invented identifier, date or registry field; and silently
     returning fewer items than were asked for. Weight these as fatal, not as lost points — a
     fabricated registration number is worse than "unknown", and the point total will not show that.

8. **Restrict the model's tools, then audit.** Give it web search and page fetching and nothing
   else. Afterwards, read the call log and confirm it did not open a local file, including the file
   holding your key. Do not skip this because it seems unlikely. Report that you checked.

9. **Score in a separate pass.** Different session from the one that answered, ideally different
   from the one that built the task. Go item by item against the written key. Record dangerous
   errors separately from the score.

10. **Report the run count, and what you did not test.** One run is one run. Say so. Then list the
    things your design cannot speak to: other models, scale, adversarial input, paywalled sources,
    non-English material.

## Suggested files per task

```
task-01-flow-arithmetic/
  task.md       the task text, exactly as given to the model
  key.md        the answer key, every item with its quote or arithmetic, plus the source log
  scoring.md    points per item, and the dangerous-error list
  answer.md     the model's reply, unedited
  score.md      item-by-item scoring, dangerous errors, verdict
```

Keep `answer.md` unedited, including the parts that are wrong. The interesting content of a probe
like this is in the mistakes.

## Public endpoints that make keys cheap to build

All checked 2026-09 and reachable without a key or a login.

| Source | Use |
|---|---|
| [PubMed / PMC E-utilities](https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esearch.fcgi?db=pubmed&term=test&retmode=json) | Counts, metadata, abstracts, JATS XML for structure and headings |
| [Europe PMC REST](https://www.ebi.ac.uk/europepmc/webservices/rest/search?query=test&format=json&pageSize=1) | Full-text XML, licence fields, table cells, full-text search counts |
| [Crossref REST](https://api.crossref.org/works/10.1136/bmj.n71) | DOI metadata, `update-to` / `updated-by` relations for retractions and corrections |
| [ClinicalTrials.gov API v2](https://clinicaltrials.gov/api/v2/studies?query.id=NCT00000102) | Registration dates, registered primary outcomes, record history |
| [WHO ICTRP](https://trialsearch.who.int/) | The mirror to use when a primary registry's own pages need JavaScript |
| [PRISMA 2020 checklist PDF](https://www.prisma-statement.org/s/PRISMA_2020_checklist-ab3g.pdf) | Authoritative item numbers and wording, CC BY 4.0 with an attribution line |
| [PRISMA-S](https://jmla.pitt.edu/ojs/jmla/article/view/962) | The 16 search-reporting items, for search-appendix tasks |
| [Cochrane methods peer-review checklist](https://www.cochrane.org/authors/submit-your-manuscript/methods-peer-review-checklist-review) | Ready-made wording for what a reviewer is supposed to check |
| [EQUATOR Network](https://www.equator-network.org/) | Reporting guidelines for other study types |

Cross-check anything important against two of these. In our runs, a single source missed a
correction that a second source carried.

## Pitfalls we hit, so you do not have to

- **Figures are bitmaps.** PRISMA diagrams in several journals are images; a text fetch returns only
  the caption. Read the numbers off the image at high zoom, in sections, and say that you did.
- **Publisher pages return 403 to scripts.** Several journals and one whole publisher blocked every
  scripted fetch. Anchor the key to a rendering you can actually reach — often PMC — and state in
  the task which rendering it is.
- **Registry pages need JavaScript.** One registry's record pages render client-side. A WHO mirror is
  a workaround, not an equivalent; if you use one, say so in the key.
- **CAPTCHAs.** One database's search pages sat behind a slider CAPTCHA. We did not attempt it, and
  the consequence is worth stating plainly: no hit count from that platform is in the key at all, and
  the scoring rule treats any number offered for it as fabricated. A second site's article pages
  returned a reCAPTCHA, so we anchored the key to a different service that carries the same records —
  a substitute source, not that site's own API. Record the route you took, and record the route you
  could not take.
- **Your key can be wrong.** One of ours was incomplete and the model found the gap. On a second task
  a key had a known limit the model could not reach: it said so and told us to go and check the figure
  ourselves. Budget for amending the key, record the amendment, and say which findings came from the
  model rather than from you.
- **A correction notice is not always complete.** In one task the published correction listed three
  changed values while the two published versions of the figure differ in six. Do not let the key
  depend on a correction notice being exhaustive.
- **Live counts drift.** Database hit counts and index counts change between runs. Never make a live
  count a required item; make it a bonus, or freeze it as a quoted number in the task text.
- **Vendor documentation goes missing.** For some platforms the official syntax manual was
  unreachable and only a mirror on a university library site was readable. If you cannot verify a
  field code, the key must say "unverified" rather than assert it.
- **Two versions of one article.** Check whether the article was corrected after publication. If it
  was, pick one version, take both the figure and the prose from that same version, and say which.

## What not to claim afterwards

- Not "the model can do X". At most: "on this task, once, its output matched a key built this way".
- No rate, no accuracy figure, no confidence interval from single runs.
- No naming of an individual article, author or trial as having a problem on the strength of a model
  answer. If a problem is already on the public record — a published retraction, correction or
  expression of concern — you may cite that record; otherwise describe the class of problem and keep
  the article out of it.
- Nothing about clinical safety or regulatory acceptability. This design cannot reach those.

---

## 中文要点

结果见 [LLM_BENCHMARK.zh-CN.md](../LLM_BENCHMARK.zh-CN.md)。本页是方法，供你用在自己的材料上。不需要写代码。

**唯一的铁律：标准答案必须在模型作答之前、依据一手来源建立，绝不凭记忆。** 每一条要么有来源原文的逐字引文，要么有完整写出的算式。

十步：①选一件有可核对答案的活儿，别选判断题；②自己打开真实公开材料，记下每个 URL 与阅读日期，并记清用的是哪一个版本；③题面逐字转录，不改数字、不透露答案；④逐条写标准答案并附证据，立不住的就写"立不住"；⑤数出假阳性面——正确答案是"这条没问题"的条目也要计分；⑥至少埋一条正确答案是"无法判定"的；⑦作答前写好评分规则，分为逐条计分与"危险错误"两部分，后者（自信的错误判定、编造标识符、静默漏项）一旦出现无论总分一律不通过；⑧限制模型工具（只许搜索与抓取），事后审计调用记录确认它没读本机文件，并把"查过了"写进报告；⑨换一个会话单独打分，逐条对照；⑩报告跑了几次，以及你没测什么。

我们踩过的坑：流程图是位图，需放大分块读图；出版商页面对脚本返回 403，要把标准答案锚定在你真能打开的那份渲染上并写明；注册库页面需 JavaScript，WHO 镜像是替代而非等价；验证码没去过（我们没试滑块，代价是该平台的命中数一个都不在标准答案里；另一个站点改用了载有同批记录的**另一个服务**，不是该站自己的接口）；**你的标准答案可能是错的**（我们有一份不完整，被模型查出来了；另一道题的标准答案有一处已知边界，模型到不了，它自己说了并让我们去核图）；更正声明未必是完整交代；实时命中数会漂移，不要设成必答项；厂商文档可能打不开，核实不了的字段代码必须写"未核实"；同一篇文章可能有两个版本，图与正文必须取自同一版本并写明是哪一版。

事后不要声称："模型能做 X"（最多只能说"在这道题上跑了一次，其输出与这样建立的标准答案一致"）；不要从单次运行给出准确率或置信区间；不要凭模型回答点名任何文章、作者或试验有问题——除非该问题已进入公共记录（已发表的撤稿、更正或关注声明）；不要触及临床安全性或监管可接受性，本设计到不了那里。
