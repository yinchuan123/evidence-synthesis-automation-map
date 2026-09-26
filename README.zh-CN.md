# 证据合成自动化地图

[English](README.md)

一份持续维护的地图：系统评价／Meta 分析的每一个步骤，哪些已经有能用的工具、工具收多少钱、还有哪些留给人做；
另外附一份实测，量一量联网的大语言模型现在已经能替代其中多少工作。

两类内容，刻意分开放：

- **地图**讲工具。每一行点名一个工具、给出它自己的页面链接，并记下这条说法的核对日期。
- **实测**讲测量。用真实题目、由另一个代理在被测模型看到任何东西之前从一手材料里建立的标准答案、逐项打分
  ——包括模型比标准答案做得更全的那几次。

**状态：首个公开版本。** 这个仓库是公开搭建的。两份主文档都已落地：地图覆盖 26 个步骤，实测报告九道题。
下面这五行取自地图，本页每个数字都有出处。工具会变，行也会持续增补与修正。

## 从这里开始

| 文档 | 里面是什么 |
|---|---|
| [AUTOMATION_MAP.zh-CN.md](AUTOMATION_MAP.zh-CN.md) | 地图：评价流程的 26 个步骤，每一步已经有哪些工具、价格，以及每行各自的核对日期。（[English](AUTOMATION_MAP.md)） |
| [LLM_BENCHMARK.zh-CN.md](LLM_BENCHMARK.zh-CN.md) | 实测：方法、打分规则、结果与局限。题目原文与标准答案不放在仓库里，按需索取。（[English](LLM_BENCHMARK.md)） |
| [CONTRIBUTING.md](CONTRIBUTING.md) | 怎么补一个工具、怎么改一行。 |

## 写给谁看

- 正在决定某一步是买、是自己写、还是手工做的评价作者与方法学研究者。
- 经常被问「这事有工具吗」的图书馆员与信息检索专员。
- 想在动手写代码之前先弄清哪个缺口是真缺口的工具开发者。
- 想知道哪些环节可以自动核查的编辑与同行评议人。

## 为什么要做这件事

系统评价的产量很大，而指南要求的那些步骤，实测常常做得不好：

- **体量。** PubMed 上 2025 年被标为系统评价或 Meta 分析的记录 48,220 条，2024 年 43,367 条
  （2026-09-26 通过 PubMed E-utilities 接口实测计数；同一天重跑 2025 年这条检索返回 48,221，
  末位应读作漂移而非精度）。
- **检索很少能复现。** 100 篇综述、453 次数据库检索中，22 次（4.9%）报告了 PRISMA-S 全部六项，
  47 次（10.4%）能复现到与原命中数相差 10% 以内，只有 1 篇综述给出了足以完全复现的细节
  （[Rethlefsen 2024, J Clin Epidemiol](https://pubmed.ncbi.nlm.nih.gov/38052277/)）。
- **报告规范达不到。** 抽样的 222 篇综述里，67 篇（30.18%）声明使用了 PRISMA 2020；这 67 篇没有一篇完全
  符合，平均符合率 42.64%。该文的逐条符合率是以这 67 篇为分母、不是以 222 篇为分母——符合率最低的四条
  （其中包括检索策略、被排除研究的特征）在 7.46%（5/67）到 11.94%（8/67）之间
  （[Ivaldi 2024, Cochrane Evid Synth Methods](https://pmc.ncbi.nlm.nih.gov/articles/PMC11795886/)）。
- **提取出来的数字带错。** 27 个 Meta 分析、每个抽查两项试验，17 个（63%）至少有一项试验的数据有错
  （[Gøtzsche 2007, JAMA](https://doi.org/10.1001/jama.298.4.430)）。
- **撤稿研究会进到合并结果里。** 1,330 项被撤稿的随机试验中有 312 项被定量合并，进了 847 篇综述里共
  4,095 个 Meta 分析，其中 3,902 个可以重算。剔除撤稿研究后，效应方向改变的估计值是 8.4%
  （95% CI 6.8–10.1）、统计学显著性改变 16.0%（14.2–17.9）。这两个是经簇校正的模型估计值、不是原始比例，
  不要拿它们去乘回 3,902（[Xu 2025, BMJ](https://doi.org/10.1136/bmj-2024-082068)）。
- **反面证据，照直写。** 另一项研究识别出 61 篇综述、从其中 50 篇提取了数据；在可重算的 166 个 Meta 分析里，
  160 个（96%）仍落在原置信区间内，18 个（11%）显著性改变
  （[Graña Possamai 2025, JAMA Intern Med](https://doi.org/10.1001/jamainternmed.2025.0256)）。
  也就是说，单一问题研究的典型影响不大；要紧的是尾部。
- **有些重复发表跨语种。** 470 项中国资助的随机试验中，55 项（11.7%）存在共 75 篇重复发表，其中
  53 篇（70.7%）跨语种。这些全部是人工识别出来的
  （[Jia 2020, JAMA Netw Open](https://doi.org/10.1001/jamanetworkopen.2020.27104)）。

做地图有用，是因为这些缺口被满足的程度很不均匀。有些步骤已经有免费工具彻底做完了，有些是一个付费工具加一个
免费近似方案，还有些仍然靠手工。

## 地图里的五行

价格与能力于 2026-09 核对。价格会变，核对日期是这条说法的一部分。

| 步骤 | 已经能做这件事的工具 | 价格 | 还留给人做的部分 |
|---|---|---|---|
| 把一条检索式转成各库的语法 | Polyglot Search Translator，现由 [TERA](https://tera-tools.com) 提供 | 免费，账号可选（2026-09 核对） | Polyglot 只改语法；按[开发者论文](https://pmc.ncbi.nlm.nih.gov/articles/PMC9359691/)与[图书馆指南](https://library.svhm.org.au/literature_searching/polyglot)，它不替你选主题词，MeSH／Emtree 的映射仍要手工做。按同样这两个来源，它的目标平台是十个英文平台。TERA 的线上清单是一个 JavaScript 应用，我们读不到（2026-09 核对），所以「清单里没有任何中文数据库」这一点在现版产品上属未经证实。 |
| 题录去重 | [ASySD](https://github.com/camaradesuk/ASySD) | 免费、开源（2026-09 核对） | [开发者论文](https://pmc.ncbi.nlm.nih.gov/articles/PMC10483700/)在五个数据集上报告灵敏度 0.95–0.99、特异度 >0.99，这五个全是英文的。在所核对的去重工具里，一条中文题录与它的英文同篇题录只有在双方都带同一个 DOI 时才配得上——被比对的那几个字符串字段跳不过文字体系。 |
| 把同一研究的多篇报告归并成一个研究 | [Covidence](https://www.covidence.org/)（merge studies）、[refs2study](https://github.com/L-ENA/refs2study)（研究代码） | Covidence [每篇综述每年 339 美元](https://www.covidence.org/pricing/)，[已注册的 Cochrane 综述在把综述关联到 Cochrane 账号之后免费](https://support.covidence.org/help/general-information-for-cochrane-authors)；refs2study 免费（2026-09 核对） | Cochrane [MECIR 第 C42 条](https://www.cochrane.org/authors/handbooks-and-manuals/mecir-manual/standards-conduct-new-cochrane-intervention-reviews-c1-c75/performing-review-c24-c75/selecting-studies-include-review-c39-c42)把归并多篇报告定为强制项。Covidence 里的合并是手工步骤。refs2study 确实会预测哪些题录属于同一研究，但它是研究代码、没有应用界面、不处理语种；[Trials to Publications](https://arrowsmith.psych.uic.edu/) 只覆盖 ClinicalTrials.gov 与 PubMed——我们没有找到任何工具替你提出跨语种的候选配对。 |
| 由中位数与四分位间距换算均数与标准差 | [香港浸会大学 median-to-mean 计算器](https://math.hkbu.edu.hk/~tongt/papers/median2mean.html)、[estmeansd](https://cran.r-project.org/package=estmeansd)、`meta::metacont` | 免费（2026-09 核对） | 已被覆盖。剩下的是选哪个情景、用哪个估计量，这是方法学判断，不是工具缺口。 |
| 核查纳入文献有无撤稿与更正 | [Europe PMC Article Status Monitor](https://europepmc.org/ArticleStatusMonitor)（批量、可导 CSV）、[Zotero](https://www.zotero.org/blog/retracted-item-notifications/) 与 [EndNote 20.2 以上且已开启同步](https://libguides.rug.nl/umcg/endnote/retraction)（依据高校图书馆指南，不是厂商页面）、[scite](https://scite.ai/) | 免费到每月 20 美元（scite Basic）（2026-09 核对） | 这一步里「更正」那一半，上列免费工具基本没覆盖：2026-09 实测，Europe PMC 的批量接口返回撤稿与撤回状态，但不返回勘误或关注声明状态；Zotero 与 EndNote 只标撤稿。而且没有一个会告诉你：剔除这项研究之后，你自己综述里的哪个合并估计会变。 |

完整地图覆盖整条流程，并且用的就是[提交表单](.github/ISSUE_TEMPLATE/add-or-fix-a-tool.yml)里的同样 15 个
步骤，这样别人提上来的工具能直接落到某一行、不必再做一次映射：问题、方案与注册；检索式构建；跨库检索式转换；
执行检索与检索记录存档；题录去重；筛选；把多篇报告归并为一个研究；资料提取；两人提取结果的核对；
偏倚风险与可信度核查；效应量准备与换算；合成、Meta 分析与作图；证据确定性（GRADE）与结果总结表；
报告与投稿前核查；发表之后的维护。

## 实测量了什么

九道题——八道为本次探查新出，加上一道更早的 24 条撤稿与更正核查——都取自真实的公开材料：已发表的综述与
试验注册记录。每道题的标准答案都由这些一手材料逐条建立，并且是在被测模型看到任何东西之前建立的。有一份
标准答案没能扛过这次运行：在撤稿题上，模型查出了一份标准答案漏掉的已发表更正声明，那份标准答案被改正，
下面报的 24/24 是对照改正后的标准答案打出来的。被测模型（Claude Fable 5.1）只允许网页搜索与抓取，
不允许读本机文件；事后审计了调用记录，没有发现读取本机文件。打分由另一个没有答题的会话对照标准答案逐项完成。

新出的八道题的结果：6 道输出够用，2 道部分正确，没有一道差。其中一道题模型报出了标准答案并未要求的东西
——预印本正式发表后那一版另有一份已发表的更正声明。另外在注册核查题里，文章印的注册号是一种并不存在的编号格式，找出真实记录本来就是标准答案要求的一项，模型找到了。

第九道题，那份 24 条的撤稿与更正核查，值得看两遍。联网时两次独立运行都是 24/24（对照改正后的标准答案）。
不联网时同一条提问，得 14/24，并把 6 条有问题的文献报成了没问题——这正是要紧的出错方向，因为「说没事」是沉默的。

**这不能说明什么。** 每道题只有一个样本，n 很小。只测了一个模型家族。标准答案同样是由代理建立的——依据
一手材料、带逐字引文，但这和「一位独立的第二位人工提取者」不是一回事。所以标准答案也会错，在这几次运行里
就错了一份。联网那几次是在一个能直接抓取接口响应的代理环境里跑的，普通聊天界面可能更差。地图背后的工具
调查每个候选只有一位复核者，没有做对抗性的第二遍。这些都不构成任何临床验证，也都不说明模型可以不经人工
核查就被信任。

这次探查的诚实结论是负面的：在我们测的这些题上，联网模型已经够用了。我们把这个结果公开，而不是发一个工具。

## 关于作者

**尹川（Chuan Yin）** — 上海交通大学
ORCID [0009-0005-2830-8167](https://orcid.org/0009-0005-2830-8167)
邮箱 `yinchuan [at] sjtu.edu.cn`

同一作者的公开工具：**Evidence OS Inspector** — 纯浏览器端、MIT 许可、alpha 阶段。它把论断绑定到其来源段落，
并在来源发生变化时标出哪些论断需要重新审阅。
[代码仓库](https://github.com/yinchuan123/evidence-os-inspector) ·
[演示](https://yinchuan123.github.io/evidence-os-inspector/)

联系方式：[yinchuan@sjtu.edu.cn](mailto:yinchuan@sjtu.edu.cn)

## 怎么参与

一个 issue 或 pull request 只提一个工具，附链接，并写明你核对了什么、什么时候核对的——见
[CONTRIBUTING.md](CONTRIBUTING.md)。关于你自己工具的纠正，我们欢迎。

如果这份地图对你有用，watch 或 star 一下能帮别人找到它。

## 许可

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)。[LICENSE](LICENSE) 收录 Creative Commons
官方发布的许可正文；[NOTICE](NOTICE) 说明转用时怎么署名。

## 如何引用

引用元数据在 [CITATION.cff](CITATION.cff)；GitHub 会在仓库页面的 "Cite this repository" 处直接渲染成
可复制的引文。
