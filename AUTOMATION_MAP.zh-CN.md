# 自动化地图

[English](AUTOMATION_MAP.md) · [仓库说明](README.zh-CN.md) · [实测](LLM_BENCHMARK.zh-CN.md)

## 这份地图解决什么问题

准备做系统评价或 Meta 分析的人，需要一步一步决定：哪一步买工具、哪一步装工具、哪一步交给联网大模型、哪一步只能自己做。
这一页就是把这个决定摊成一张表：流程的每一步一行，写清已经有哪些工具在做这件事、多少钱，我们拿这一步的真题去问联网大模型时
它做到了什么，以及剩下哪一部分仍然必须由人来做。每一行都带核对日期，因为价格和功能清单几个月就会过时。**表是主体**，
周围的文字故意写得很短。

## 怎么读这张表

- **已有工具** —— 凡是在这里带链接的工具，都在核对当天打开过它自己的页面。凡是打不开的（需要登录、有机器人验证、
  页面是纯 JavaScript 应用没有可读文字），行里会写出来，并标为未核实，而不是猜一个。有少数工具只写了名字没有链接：
  它们来自目录收录或别的工具的文档，行里标注**未打开页面**。
- **新建仓库** —— 工具托管在 GitHub 且仓库很新或没有 star 时，行里会写出它的 star 数与创建日期（GitHub API，
  2026-09-26 读取）。这条对所有这类行一律适用，不只用在我们恰好怀疑的那几个上。
- **价格** —— 核对当天厂商自己页面上写的。日期是价格的一部分。
- **联网大模型** —— 我们的实测结果，覆盖其中 9 步。9 道题都用真实公开材料出，标准答案在被打分的那一轮之前就定稿（其中一道题，模型查出了标准答案漏掉的一项，标准答案在打分前被改正），
  受测模型（Claude Fable 5.1）只允许网页搜索与抓取，不能读本机文件。方法、打分规则与完整结果见
  [LLM_BENCHMARK.zh-CN.md](LLM_BENCHMARK.zh-CN.md)。其余行写**未测**——意思是没测过，不是"做不到"。
- **仍需人做** —— 工具和模型都不做的那件具体事。这一列是这张地图存在的理由。
- **核对日期** —— `2026-09-17` 是工具普查，`2026-09-16` 是撤稿与更正那一行，`2026-09-26` 是后来补的行。
  所有链接于 2026-09-26 重新核过一遍。

这里没有排名，也没有推荐。免费不等于够用，收费也不等于更好。

## 地图

| # | 步骤 | 这件事是什么 | 已有工具（2026-09 核对） | 联网大模型实测结果 | 仍需人做 | 核对日期 |
|---|---|---|---|---|---|---|
| 1 | 方案与注册 | 写方案、注册、保持记录是最新的。 | [PROSPERO](https://www.crd.york.ac.uk/prospero/) —— 免费注册。[INPLASY](https://inplasy.com/) —— 其自有页面写明 9,700+ 份已注册方案、100 个国家、每条记录给 DOI；站内有 Fees & Services 页面，具体金额未核实。 | 未测。 | 问题、纳入排除标准、分析计划。注册记录本身很粗：96 篇在 PROSPERO 注册的综述里，39% 没有明确写出主要结局（[Tricco 2016](https://pubmed.ncbi.nlm.nih.gov/27079845/)）。 | 2026-09-26 |
| 2 | 中英文术语对照（中文 ↔ MeSH / Emtree） | 把中文的结局、药物、疾病名对应到 MeSH 主题词、Emtree 首选词和英文规范名。 | [SinoMed 主题检索](https://www.sinomed.ac.cn/zh/subjectSearch.html) —— 页面写明可用中文主题词、英文主题词及同义词查找对应主题词；实际发起查询时跳转登录。[CMeSH](http://cmesh.imicams.ac.cn) —— MeSH 中译本，首页是登录页，图书馆将其列为订阅资源。Emtree 查询只在 Embase 内部。[术语在线](https://www.termonline.cn) —— 免费、中英对照、收录 80 万条以上，但不映射 MeSH 或 Emtree。 | 未测。 | 确认这个主题词在当年的词表里确实存在；并且在根本没有对应词时判"无对应"——中医结局经常如此。 | 2026-09-17 |
| 3 | 检索式构建与跨库转换 | 把一条检索式按每个库重写：字段代码、布尔与截词语法、受控词表。 | Polyglot Search Translator，现由 [TERA](https://tera-tools.com) 提供 —— 免费、账号可选；[开发者论文](https://pmc.ncbi.nlm.nih.gov/articles/PMC9359691/) 列出约十个英文平台。图书馆指南与工具页面都写明：它只调整语法，**不**替你选主题词。[Embase PubMed-to-Embase 转换工具](https://www.elsevier.support/embase/answer/pubmed-to-embase-translation-tool) —— 自动把 MeSH 映射到 Emtree；它在 Embase 内部，因此意味着需要订阅。[Research Gold 检索式转换器](https://researchgold.org/resources/database-search-translator) —— 免费、免注册；其自有页面写明受控词表无法直接转换。[Ovid Search Translator](https://tools.ovid.com/translate)、[Medline Transpose](https://medlinetranspose.github.io) —— 免费，只做语法。以上没有一个覆盖知网、万方或 SinoMed。 | **部分正确。** 三个中文平台的语法都合法，没有编造字段代码，并且正确地拒绝把原检索式里的 `*` 当作没有该操作符的平台上的截词符。但它给出的主题词表不是已发表的那一份——四个词里给对三个，漏掉第四个；它自己也说明无法拿某一平台的词表核实该平台的主题词形式。三个库一个都跑不了：都要登录，其中一个还弹出滑块验证。 | 选主题词。然后真的去跑一遍检索，并记下结果。 | 2026-09-17 |
| 4 | 检索报告（PRISMA-S）与可复现存档 | 逐库记录：平台、日期、逐行检索式、命中数。然后写成附录。 | [CiteSource](https://cran.r-project.org/web/packages/CiteSource/index.html) —— 免费 R 包与 Shiny 应用；保留每个来源的元数据并给出重叠与独有贡献，但不记录日期和命中数，也不生成 PRISMA-S 附录。[slr-harvester](https://github.com/socresearcher/slr-harvester) —— 可免费使用，但仓库没有声明可识别的许可；持久记录检索策略与筛选决定，但只覆盖有 API 的来源。0 个 star，仓库创建于 2026-02-14（GitHub API，2026-09-26）。searchRxiv（CABI）—— 带 DOI 的检索式仓库，需要人工提交；其期刊页面对我们返回 403，因此细节未核实。Nested Knowledge —— 商业产品；只记录你导入时自己填的检索式与日期。 | **达到标准答案。** 九个来源名与命中数全部还原，对不写平台的来源没有编造平台或厂商，并且算出了原文从未印出的总数（1,687），指出从这里降到进入筛选的 107 条这一步只依赖一位作者的主观判断，不可复现。 | 在检索真正运行的那一刻，亲眼看到并记下命中数与日期。需要登录的库，模型没法替你重跑。 | 2026-09-17 |
| 5 | 题录去重 | 删掉指向同一篇文献的重复记录——包括一条中文记录和它的英文对应记录。 | [ASySD](https://github.com/camaradesuk/ASySD) + [Shiny 应用](https://camarades.shinyapps.io/ASySD/) —— 免费开源；[开发者报告](https://pmc.ncbi.nlm.nih.gov/articles/PMC10483700/) 在五个数据集上敏感度 0.95–0.99、特异度 >0.99，五个数据集全是英文。[Zotero](https://www.zotero.org/support/duplicate_detection) —— 免费；其页面写明用题名、DOI、ISBN，再看年份（±1）与作者。[Rayyan Systematic Auto-Resolver](https://www.rayyan.ai/systematic-auto-resolver) —— 可配置字段匹配，属付费功能。Deduklick —— 商业产品，未打开页面。EndNote 与 NoteExpress —— 收费，未打开页面，也是中文实验室的既有选择。 | 未测。 | 跨语言的那一对。我们查过的工具里，一条中文记录只有在和英文记录带同一个 DOI 时才配得上；被比较的字段——题名、作者、期刊——在中文、拼音、英文之间本来就不一样。 | 2026-09-17 |
| 6 | 筛选（题录筛选，再全文筛选） | 双人判定哪些记录与报告合格。 | [Rayyan](https://www.rayyan.ai/) —— 有免费档，其[价格页](https://www.rayyan.ai/pricing)写明个人与小团队 $0「free forever」，另有付费档。[ASReview](https://asreview.nl/) —— 免费开源，由荷兰乌得勒支大学统筹；主动学习式筛选，另带模拟模式用于自测。[Covidence](https://www.covidence.org/) —— 收费，每篇综述每年 $339，Cochrane 综述免费。 | 未测。 | 合格与否的判断本身。MECIR 与 PRISMA 都要求两个人；筛选工具改变的是队列顺序，不是谁来判。 | 2026-09-26 |
| 7 | 全文获取 | 把通过筛选的每一份报告的全文拿到。 | 免费的程序化路径，两条都在 2026-09-26 实际调用过：[Europe PMC REST 接口](https://www.ebi.ac.uk/europepmc/webservices/rest/search?format=json&query=test) 与 NCBI E-utilities。除此之外就是出版商付费墙；中文库是关着的：SinoMed 的帮助页跳转登录，知网高级检索弹出滑块验证，万方要登录才能检索。 | 未测。 | 机构权限、馆际互借、写信问作者。还有一条对人和模型同样适用的规矩：不要绕验证码，不要绕授权。 | 2026-09-26 |
| 8 | 把多篇报告归并成研究 | 把同一项研究的多篇报告合起来，并识别重复发表与样本重叠。 | [Covidence](https://support.covidence.org/help/consensus-9831e0af) —— 把多条题录合并为一项研究是人工步骤；收费。[Trials to Publications](https://arrowsmith.psych.uic.edu/) —— 免费；给一个 ClinicalTrials.gov 编号，它按"报告该试验的可能性"给 PubMed 文章排序；只覆盖 CT.gov 与 PubMed，有批量模式。[refs2study](https://github.com/L-ENA/refs2study) —— 研究代码，没有界面，不处理语言差异。 | **部分正确。** 七组里六组完全正确，包括这道题专门设计的中英文同一队列那一组。有一组多吸收了两条本该排除的记录——但它写明了理由，并且把那一组内部尚未解决的样本量矛盾标了出来，而不是抹平。每条记录都给了状态，没有一条被悄悄丢掉。 | 裁定边界上的合并，以及一开始就把候选对找出来。Cochrane MECIR C42 把归并列为强制。 | 2026-09-17 |
| 9 | 资料提取 | 把每份报告的研究特征与结局数据填进表。 | SR Toolbox 提取类目下的平台工具：Covidence（收费）、Rayyan（免费档＋付费）、[CADIMA](https://www.cadima.info/)（免费）、以及——在该目录里被收录但我们未打开其页面的——DistillerSR、EPPI-Reviewer、JBI SUMARI、Nested Knowledge、PICO Portal、Sysrev。AHRQ 的 SRDR+ 已停止运行；我们的笔记从 AHRQ 公告取到的日期是 2025-11-28，但该公告页每次抓取都只返回 HTTP 202 且无正文，所以这个日期 `unverified / 未核实`。 | 未测——我们测的是比对那一步，不是提取本身。 | 提取本身，结局数据必须双人（MECIR C46，强制）。实测采用率很低：152 篇综述里只有 9 篇（6%）报告用 Covidence（使用最多的那个自动化工具）做提取（[Büchter 2021](https://doi.org/10.1186/s12874-021-01438-z)），所以 Excel 仍然是真实工作流。 | 2026-09-17 |
| 10 | 双人提取表比对 | 按 研究 × 组别 × 结局 × 时点 对齐两位提取者的表，逐格比对，并把格式差异与数值差异分开。 | [Covidence 共识功能](https://support.covidence.org/help/consensus-9831e0af) —— 收费；其自有页面写明修改会高亮，"except within data tables"（数据表内除外），而结局数据恰好在数据表里。[Diffchecker Excel Compare](https://www.diffchecker.com/excel-compare/) —— 浏览器内免费，按位置对齐；导出属 Pro 功能。[daff](https://github.com/paulfitz/daff) —— 免费、MIT；可用 `--id` 指定键列对齐，但不直接读 xlsx。Microsoft Spreadsheet Compare —— 只随 Microsoft 365 Apps for enterprise 提供。 | **达到标准答案。** 6 处数值差、6 处纯格式差、2 处覆盖差全部找到，无假阳性——并且在故意设下的"第二位提取者才是对的"那一格上裁定正确。 | 决定最终值，并记下谁裁定、为什么。 | 2026-09-17 |
| 11 | 统计量换算 | 中位数/四分位距/极差 → 均数与标准差；效应量互换；由 CI 或 P 值反推 SD；多臂合并。 | [香港浸会大学 median-to-mean 计算器](https://www.math.hkbu.edu.hk/~tongt/papers/median2mean.html) —— 免费（Wan 2014、Luo 2018、Shi 2020 与 2023）。[ebm-helper.cn](https://ebm-helper.cn/Conv/tomean+sd.html) —— 免费，中文界面。[estmeansd](https://github.com/stmcg/estmeansd) + [Shiny 应用](https://smcgrath.shinyapps.io/estmeansd/) —— 免费。[metaConvert](https://metaconvert.org/) —— 免费开源；[其论文](https://pmc.ncbi.nlm.nih.gov/articles/PMC12527507/) 列出 127 条公式、74 种输入组合、11 种效应量（其中没有率差）。[Meta-Analysis Accelerator](https://ma-accelerator.com) —— 其[价格页](https://ma-accelerator.com/pricing)是一个 JavaScript 应用，对抓取不输出任何价格或额度文字；2026-09-26 读到的应用自身字串为：每个账号 3 个免费项目、每个免费项目 10 项研究，$6/年，$24 终身——我们更早的笔记记的是“每月 3 个新项目、每项目 50 行”，当前版本并没有这么写；[其论文](https://pmc.ncbi.nlm.nih.gov/articles/PMC11487830/) 列出 21 种换算。[尔云间](https://www.biocloudservice.com/meta/home.html) —— 免费、中文、13 个计算器。[RevMan calculator](https://documentation.cochrane.org/revman-kb/calculator-95420946.html) —— 价格未核。`meta::metacont` 可在分析内部完成换算。 | 未测。 | 选对情形与估量，并检查这个换算是否成立——OR 换 RR 需要基线风险，小样本的 CI 要用 t 而不是 1.96。 | 2026-09-17 |
| 12 | 图中取数与生存曲线重建 | 从图上读数；由 Kaplan-Meier 曲线重建个体数据或估计 HR。 | [WebPlotDigitizer / automeris.io](https://automeris.io)，[v4 应用](https://apps.automeris.io/wpd4/) —— 其定价页返回 404，所以当前版本的价格未核实。[IPDfromKM](https://cran.r-project.org/web/packages/IPDfromKM/index.html) —— 免费 R 包；在线应用托管在 [trialdesign.org](https://trialdesign.org)。[SurvdigitizeR](https://github.com/Pechli-Lab/SurvdigitizeR) + [Shiny 应用](https://pechlilab.shinyapps.io/SurvdigitizeR/) —— 免费；其 README 自称仍在开发中，且不读风险人数表。[ebm-helper.cn 的 KM→HR 指南](https://ebm-helper.cn/Conv/KM_HR.html) —— 免费，中文。KM-GPT 在已发表论文里被描述为免费网页应用，但我们无法确认该应用在线，故未核实。 | 未测。 | 坐标轴标定、风险人数表，以及对重建结果做常识检查。一个"看起来合理但错了"的 HR 不会报错。 | 2026-09-17 |
| 13 | 偏倚风险评价 | 按结局套用 RoB 2、ROBINS-I、ROBINS-E 或量表，并画图。 | [riskofbias.info](https://www.riskofbias.info/welcome/rob-2-0-tool) —— 免费；RoB 2（当前版本日期为 2019 年 8 月 22 日，另有整群随机与交叉设计版本）、ROBINS-I、ROBINS-E、ROB-ME，以指南文档与空白模板形式提供。[robvis](https://github.com/mcguinlu/robvis) + [Shiny 应用](https://mcguinlu.shinyapps.io/robvis/) —— 免费；画红绿灯图与汇总图。RobotReviewer 出现在 SR Toolbox 质量评价类目中，我们未做考察。 | 未测。 | 各信号问题的判断。这一步有一条被实测出来的边界：[INSPECT-SR 第二阶段研究](https://pubmed.ncbi.nlm.nih.gov/40349737/)发现，有问题的研究"do not appear to be flagged by Risk of Bias assessment"（似乎不会被偏倚风险评价标出来）。 | 2026-09-26 |
| 14 | 纳入试验的可信度初筛 | 检查基线表里不可能出现的均数与方差，并检查随机化、分配隐藏、注册是否真的写了。 | [INSPECT-SR](https://inspect-sr.com) —— 免费指南站；4 个域 21 项检查，附可编辑模板，没有内置计算器，也没有中文版。[GRIM / GRIMMER / TIDES 应用](https://errors.shinyapps.io/inspect-sr-means-variances/) —— 免费；数值需逐行手输。[scrutiny](https://lhdjung.github.io/scrutiny/) —— 免费 R 包（GRIM、GRIMMER、DEBIT、重复值分析）。[baseline](https://github.com/agbarnett/baseline) + [Shiny 应用](https://aushsi.shinyapps.io/baseline/) —— 免费；检测基线表过度或不足离散，可从 PMC 论文自动抽取基线表，而这对中文期刊无效。[reappraised](https://cran.r-project.org/web/packages/reappraised/index.html) —— 免费 R 包。[智论助手](https://github.com/wangzhiguan72-droid/zhilun-assistant) —— 免费、MIT、中文；GRIM、GRIMMER、Benford、末位数字检查，能读中文 PDF 与 docx，但不是 RCT 专用清单；1 个 star，仓库创建于 2026-09-15（GitHub API，2026-09-26）。 | **达到标准答案。** 三处算术上不可能的均数全部算出并写出了算式，五个可达的格子一个都没误判，找出了亚组人数加起来只有 40 例中的 38 例（百分比合计 95%）——而且对一篇根本没印注册号的论文没有编造注册号，这是打分规则罚得最重的那种错。 | 指控本身。INSPECT-SR 自己的[第二阶段论文](https://pubmed.ncbi.nlm.nih.gov/40349737/)警告，有几项检查"proved difficult to understand or implement, which may have led to unwarranted skepticism in some instances"（难以理解或实施，可能导致某些情况下不该有的怀疑）。GRIM 只对已知 n 的整数颗粒度均数有效，所以很多基线格子根本无法检验。 | 2026-09-17 |
| 15 | 纳入试验的注册号核查 | 每个注册号能不能查到？注册是否早于入组？注册结局与发表结局是否一致？ | [RegCheck](https://regcheck.app)（[代码](https://github.com/JamieCummins/regcheck)，AGPL-3.0）—— 公共网页版免费；逐维度比对注册记录与论文并高亮原文；可输入 ClinicalTrials.gov 编号或上传 PDF/DOCX。不支持 ChiCTR，文档未写批量模式，也不检查注册时间。[cthist](https://cran.r-project.org/web/packages/cthist/index.html) —— 免费 R 包；批量抓取 ClinicalTrials.gov 历史版本。[TRNscreener](https://github.com/bgcarlisle/TRNscreener) 与 [ctregistries](https://github.com/maia-sh/ctregistries) —— 免费；只识别注册号，别的不做。[ChiCTR 自己的状态页](https://www.chictr.org.cn/regstatusprojEN.html) —— 免费；2026-09-17 我们读到时，计数器显示预注册 105,848、补注册 24,347；到 2026-09-26 已变为 106,156 与 24,376，所以要把它当成实时计数器，不是一个固定数字。 | **达到标准答案。** 10/10 号码全部查到，其中一个在论文里印错——它找到了真实记录并说明印出来的那串是错的，而不是悄悄替换。前瞻/回顾 10/10。4/4 处主要结局不一致，每处都引了注册记录字段。某个注册库页面打不开时，它改用 WHO ICTRP 镜像并说明了这一点。 | 判断措辞不同算不算结局改动。注册库的日期字段本身也不可靠：一项对 ChiCTR 的分析发现 848 项试验报错了研究开始年份。 | 2026-09-17 |
| 16 | 纳入研究的撤稿与更正核查 | 逐条核查纳入研究是否有撤稿、撤回、更正、关注声明，或预印本已正式发表。 | [refcheck](https://github.com/KaizenShogun/refcheck) —— 免费、MIT，浏览器内运行，读 RIS/NBIB/BIB，覆盖全部声明类型，并显示查不到的那些行；0 个 star，仓库创建于 2026-09-06（GitHub API，2026-09-26）。[Europe PMC Article Status Monitor](https://europepmc.org/ArticleStatusMonitor) —— 免费批量核查，可导 CSV；其页面对我们的抓取返回 403，功能清单来自我们更早的普查。Zotero —— 免费，撤稿。EndNote 20.2+ —— 收费，撤稿。PubMed 过滤器 —— 免费。这三个只写名字没给链接：本轮未打开其页面。[scite](https://scite.ai/) —— Basic $20/月，按年计费。[Covidence](https://www.covidence.org/) —— 每篇综述每年 $339，Cochrane 免费。底层的免费接口：[Crossref](https://www.crossref.org/)、NCBI E-utilities、Europe PMC，以及 [Retraction Watch 数据库](https://retractionwatch.com/retraction-watch-database-user-guide/)。 | **两轮都达到标准答案。** 联网状态下，24 条清单两次都是 24/24，没有一条把有问题的判成没问题，还查出了标准答案漏掉的一处更正。**不联网时，同一个模型、同一个提问，得 14/24，并给出 6 条自信的"没问题"**——这是最要紧的错误方向，因为假的安心是沉默的。 | 确认查回来的记录确实对应你的那条引文。在一次脚本测试里——跑的是确定性脚本，不是模型——20 条按五种引文格式生成的参考文献字符串，用 Crossref 题录查询，首位命中正确记录 13/20（其中"只给题名"的 3 条里对 2 条），正确记录落在前五名的 17/20；而在有结构化字段时，PubMed ECitMatch 14 条里 14 条给出准确 PMID，34 次调用里没有返回过错误的 PMID。这两个数都偏乐观，因为字符串是由数据库自己的元数据生成的，不是从真实参考文献表里抄的。然后判断这处更正重不重要。还有一个盲区：200 条中文随机对照试验记录抽样中有 DOI 的 179 条，Crossref 返回 404 的 174 条、限流的 5 条，没有一条解析成功；复抽 25 条同样没有一条成功。该样本集中在 2022 年的少数几家期刊。 | 2026-09-16 |
| 17 | 合成 / Meta 分析 | 拟合模型、合并、量化异质性、跑预先计划的敏感性分析。 | [R meta](https://cran.r-project.org/web/packages/meta/index.html) —— 免费，GPL；v8.5-0，2026-05-25 发布；共同效应（common effect）与随机效应、Hartung-Knapp 与 Kenward-Roger、预测区间、剪补法、Meta 回归、累积与逐一剔除分析，并能直接导入 RevMan 5 数据。[metafor](https://www.metafor-project.org/doku.php/metafor) —— 免费开源，GPL-2。[RevMan](https://revman.cochrane.org/) —— Cochrane；授权按用户类型区分，价格我们没核。Stata `metan` —— 需要 Stata（收费）。[metaanalysisonline.com](https://metaanalysisonline.com) —— 免费；森林图、漏斗图、Z 值图，可导 PNG 与 PDF。[Meta-Mar](https://www.meta-mar.com) —— 免费档每月 2 次分析；通行证 €19/€39/€69，学生 €29。[MetaReview](https://metareview.cc) —— 免费，中英双语。 | 未测——没有把这一步单独出成一道题。 | 模型、估量，以及"到底能不能合并"。软件会把你交给它的任何东西合并起来。 | 2026-09-26 |
| 18 | 森林图与漏斗图 | 按目标期刊的要求把图画出来。 | [forestplotgenerator.com](https://forestplotgenerator.com) —— 免费下载带一层浅水印，一次性 Researcher Pass 可去掉；主题列表里有 Cochrane（RevMan）预设与若干期刊预设；导出 PNG 或 SVG。[Hiplot](https://hiplot.cn) —— 免费开源；有森林图与漏斗图模块，中文界面。[sci-draw.com](https://sci-draw.com/zh/forest-plot-generator) —— 中文界面，按积分计费，只画已算好的效应量。[SPSSAU](https://spssau.com/helps/meta/continuous.html) —— 中文；页面上没写价格。R 的 `meta`/`forestploter` 与 RevMan 都能导出矢量图。 | 未测。 | 期刊的图形规格——栏宽、最小字号、文件格式——以及真的把渲染出来的图看一眼。 | 2026-09-17 |
| 19 | GRADE 与结果总结表 | 逐结局判证据质量、记录每一处降级理由、出表。 | [GRADEpro GDT](https://www.gradepro.org) —— [价格](https://www.gradepro.org/pricing)：Standard $0，3 名成员、25 个问题，可创建 GRADE 证据表；Team 每个活跃项目每年 $2,400。其用户手册写明程序"will force the user to provide explanations in fields, where they are expected/necessary"（在应当填写解释的字段强制填写），证据概要表可转为结果总结表。界面语言可选中文。[MAGICapp](https://help.magicapp.org) —— 需付许可费；其帮助写明没有专门的非营利折扣。 | 未测。 | 证据质量判断本身：同一个问题不要在两个域里降两次，不精确性阈值前后一致，每千人绝对效应算对。 | 2026-09-17 |
| 20 | PRISMA 流程图算术 | 核查流程图自身加减是否成立，以及是否与摘要、正文、研究特征表一致。 | [PRISMA2020 R 包](https://cran.r-project.org/web/packages/PRISMA2020/index.html)（[仓库](https://github.com/prisma-flowdiagram/PRISMA2020)）—— 免费，MIT；v1.1.5，2026-09-05 发布；生成交互式与静态流程图，可导出 HTML、PDF、PNG、SVG 等。[researchmethod.net 生成器](https://researchmethod.net/prisma-flow-diagram/) —— 免费；其页面写明会在计数不可能时给出警告，例如排除数大于筛选数。[PeerReviewAI](https://peerreviewai.org/guides/prisma-checklist) —— 每篇稿件 $49；其页面写明会核查流程图存在且内部自洽。Covidence 用你的筛选数据直接生成流程图，属于从源头避免算错而不是事后核查。没有免费的独立核查器接受"已有的图＋稿件正文"。 | **达到标准答案。** 找出了唯一一处减法不成立——两个框之间差 9 条记录——其余十四项应当平的检查一项都没误报，并明确指出仅凭印出来的数字无法判断三个框中哪一个是错的，还主动定位到该文的已发表更正声明。 | 读图的框与箭头拓扑，并判断 records / reports / studies 之间的差值是否合法——在 PRISMA 2020 里它经常是合法的。 | 2026-09-17 |
| 21 | PRISMA 2020 清单定位 | 给 27 个条目（42 个子条目）逐条判状态并给出稿件中的位置。 | [PeerReviewAI](https://peerreviewai.org/guides/prisma-checklist) —— 每篇稿件 $49；逐条给 adequate / incomplete / missing，页面未写是否给位置。SciSpace 的 PRISMA 核查 agent —— 按积分计费；其自有页面被机器人验证挡住，故未核实。[PRISMA-AI](https://github.com/youkiti/PRISMA-AI-Share) —— 研究基准与排行榜，MIT，不是面向用户的托管工具；其论文在开发队列中报告的准确率为：只给稿件 45.21%；把 PRISMA 2020 清单本身以结构化格式（Markdown、JSON、XML 或纯文本）一起给模型时 78.7–79.7%——给的是空白的规范清单，不是别人已经填好的那一份。其排行榜是另一项测量：在自己的 10 篇综述队列上，27 个模型为 68.5–86.0%（2026-09-26 读取）。[PRISMA2020 R 包](https://cran.r-project.org/web/packages/PRISMA2020/index.html) —— 免费；能出清单文件，但没法替你找页码。 | **达到标准答案。** 14 个条目的状态与位置全对，包括两个在全文里确实没有报告的条目；也没有为一种本来就没有这些小节的期刊版式编造出 Funding 或 Data Availability 小节。 | 页码只有在稿件排版之后才存在。而且 42 个子条目的判据很细：标题贴对了标签，不等于该条目真的报告了。 | 2026-09-17 |
| 22 | 与注册方案偏离的报告 | 把注册记录与稿件逐维度比对，写出"与注册方案的差异"表。 | [RegCheck](https://regcheck.app) —— 免费公共网页版，AGPL-3.0；逐维度给 Deviation / No Deviation / Insufficient 并高亮原文；临床预设有 11 个维度。不原生支持 PROSPERO 或 INPLASY。[metacheck 的 reg_check 模块](https://www.scienceverse.org/metacheck_book/chapters/mod-reg-check.html) —— 免费；调用 RegCheck，但需要 API token，而其文档写明暂时无法在网站申请。[PreReg Deviation Template](https://apps.leibniz-psychology.org/prp-dev/) —— 免费 Shiny 应用；偏离要手填，可导出 PDF 或 Word 说明。 | 未测。 | 判断把"死亡率"收窄为"30 天全因死亡率"算不算偏离；以及取"开始提取数据时"那一版记录，而不是最新那一版。PRISMA 2020 条目 24c 要求描述并解释任何修订。 | 2026-09-17 |
| 23 | RevMan、Stata、R 之间的格式互转 | 把同一份分析数据在软件之间搬。 | [`meta::read.rm5`](https://search.r-project.org/CRAN/refmans/meta/html/read.rm5.html) 与 `read.cdir` —— 免费；只有 RevMan → R，没有写回函数。RevMan Web 文档写明支持四类 CSV 导入，并可把 .rm5 转成数据包（该文档页本轮未打开）。Covidence 可导出一个 RIS 文件加四个 CSV 供 RevMan Web 导入（收费）。`metafor::to.wide` / `to.long` 只在 R 内部做宽长变换。我们没有找到能由 R、Stata 或 Excel 表反向写出 RevMan 导入文件的工具，也没有找到能读 RevMan 导出的 Stata 命令。 | 未测。 | 拿厂商自己的模板核对列映射。映射错了——事件数与非事件数、SE 与 SD、亚组与比较编号——完全不会报错。 | 2026-09-17 |
| 24 | 题录格式转换（知网 / 万方 / SinoMed → RIS） | 把中文库的导出转成 Zotero、Rayyan 或 Covidence 能接受的格式，并且编码正确。 | 知网与万方可直接从库里导出 EndNote 与 NoteExpress 格式；两个库都要登录，其导出页我们没有打开。[translators_CN](https://github.com/l0o0/translators_CN) —— 免费的社区 Zotero 转换器，覆盖知网、万方、维普、Yiigle；我们读仓库文件树时没有 SinoMed/CBM 的转换器。[cnki-converter](https://github.com/cmsax/cnki-converter) —— 免费；把知网的 EndNote 文件转成 RefMan，不过其网页应用域名解析不了。[CNKI-PDF-RIS-Helper](https://github.com/Doradx/CNKI-PDF-RIS-Helper) —— 免费用户脚本，逐篇下载 RIS。EndNote 有一个「SinoMed CBM」过滤器，但需要 EndNote（收费）；下载页发生跳转，细节未核实。[Covidence 接受](https://support.covidence.org/help/study-imports) EndNote XML、PubMed 文本格式与 RIS，所以走 EndNote 这条路往往根本不需要转 RIS。 | 未测。 | 检查编码并抽查字段映射。GBK 与 UTF-8 的问题在字节里，读文本的模型看不到。 | 2026-09-17 |
| 25 | 已完成综述的内部数字自洽 | 核查摘要、正文、表格、森林图里的数字是否一致，合并结果能否复现。 | [statcheck](https://statcheck.io/) —— 免费；只核查按 APA 报告的 t、F、χ²、r、Z、Q 与其 P 值是否一致，别的不做。[repro-checker](https://mahmood726-cyber.github.io/repro-checker/) —— 早期；输入是粘贴的 JSON，重算合并效应、CI、I²/τ²/Q 与 k。它没有声明许可：仓库里没有 LICENSE 文件，GitHub API 也报告无许可，所以再利用的条件不明。0 个 star，仓库创建于 2026-06-15，所属账号有 684 个公开仓库（GitHub API，2026-09-26）。[scrutiny](https://lhdjung.github.io/scrutiny/) —— 免费；只到单个研究的颗粒度。我们没有找到任何工具能读一篇已发表的 PDF，把正文、表格、森林图三处对账，再按该综述声明的模型重算。 | 未测。 | 先弄清这篇综述实际用的是哪种合并模型与估计量，再判断不一致是不是错误。否则"模型不同"会被报成"数字错了"，或者反过来。 | 2026-09-17 |
| 26 | 回复审稿人的统计学意见 | 把一条审稿意见变成真正需要补做的分析清单，加上回复措辞。 | 中文的通用 AI 回复信生成器是有的——[AI 改写](https://www.aigaixie.com/x-review-response-generator) 与 [Scientify](https://acadwrite.cn/learn/journal-reviewer-response)——但都不是 Meta 分析专用，都不给"要补做哪些分析"的清单，价格按积分计或未标明。我们没有找到任何工具把一条统计学意见映射成带条件的分析清单。 | 未测。 | 判断哪些分析站得住，并且绝不把没做过的分析写成做过了。要盯的失败模式：回复写得很顺，却认可了不该做的补救——研究数不足 10 项就做 Egger 检验、因为 I² 低就换成固定效应、把事后亚组当成解释、把剪补法当成"矫正"。 | 2026-09-17 |

### 这张表的一句话结论

这张表里没有哪一行是做完了的。有几步的机械部分已经被免费工具做掉了——统计量换算（第 11 行）、图中取数（12）、
合并本身（17）、森林图与漏斗图（18）、GRADE 表（19）——而这几行在最后一列都仍然留着一个判断。请读格子里的具体说明，
而不是"免费"这两个字：这几个免费选项里，有一个价格未核实，有一个在自己的 README 里自称仍在开发中，有一个的下载带水印
要买通行证才能去掉，还有一个的免费档被限到 3 名成员、25 个问题。其余大多数步骤有一个覆盖一部分的工具，并留下一个能
指名道姓的缺口。完全没有合用工具的步骤，是跨语言的那些，以及"问一份已完成的文档内部是否自洽"的那些。

测过的那九步能说明的事，比"比较"要窄。模型在其中七步与标准答案一致，两步部分正确。只有一道题上另有一个专门写的
确定性脚本跑了同一份清单：模型 24/24，脚本 22/24。这九道题里没有任何其他工具被实际运行过，所以这里没有任何
"模型对工具"的排名。本仓库发布一张地图和一份实测，是因为这是我们能确立的东西，不是因为模型赢了什么。

## 表背后的实测数字

计数在 2026-09-26 通过 PubMed E-utilities、PMC 与 Europe PMC 接口现场重测。引自已发表研究的百分比都带上样本量。

**体量。** PubMed 2024 年标注为系统评价或 Meta 分析的记录 43,367 条，2025 年 48,220 条
（同一天重跑这条检索返回 48,221，所以末位应读作漂移而非精度）；其中带中国机构的分别为
10,839 条（25.0%）与 12,602 条（26.1%）。2024 年这批里只有 91 条是中文，也就是说中文系统评价文献基本不在 PubMed 里，
本页每个计数对中文读者而言都只是下限。PROSPERO 自己的新闻页写明：截至 2024 年 12 月 11 日已发布记录 306,444 条，
约每天 215 条新记录。

**检索报告。** 100 篇综述、453 次数据库检索中，22 次（4.9%）报告了全部六项 PRISMA-S 条目，47 次（10.4%）能把结果数复现到
原始值的 10% 以内，只有 1 篇综述给的细节足以完全复现
（[Rethlefsen 2024](https://pubmed.ncbi.nlm.nih.gov/38052277/)）。137 篇综述中，92.7% 的检索式含某种错误，
而影响查全率的错误是最常见的一类，为 78.1%——摘要没有写出后一个数字的分母，我们也没有去开全文核定
（[Salvador-Oliván 2019](https://pubmed.ncbi.nlm.nih.gov/31019390/)）。607 篇中文期刊的
观察性研究 Meta 分析里，85.8% 的检索信息不全面（[Zhang 2015](https://pubmed.ncbi.nlm.nih.gov/26644119/)）。
1,176 篇中文社会科学综述里，18.3% 提供了完整检索策略
（[Guo 2025](https://pmc.ncbi.nlm.nih.gov/articles/PMC11743190/)）。手工转换一条检索式平均耗时 45 分钟、平均出 14.6 处错，
借助工具为 31 分钟、8.6 处错（[Polyglot 随机对照试验，2020](https://pmc.ncbi.nlm.nih.gov/articles/PMC7069833/)）。

**报告规范依从。** 222 篇综述中没有一篇完全符合 PRISMA 2020，平均依从率 42.64%。而在其中确实使用了 PRISMA 2020 的
67 篇（30.18%）里，检索策略这一条只有 8 篇（11.94%）报告了——是依从最差的四条之一
（[Ivaldi 2024](https://pubmed.ncbi.nlm.nih.gov/40476264/)，Cochrane Evid Synth Methods 2024;2(5):e12074）。

**流程图。** 2024 年 PMC 中题名含"systematic review"或"meta-analysis"的综述，85.9% 提到流程图。一个更早的样本里，
154 篇被评估的文章中有 78 篇（50.6%）有流程图；
而流程图各组成部分中报告得最少的是去重方法，为 1%
（[Vu-Ngoc 2018](https://pmc.ncbi.nlm.nih.gov/articles/PMC6021048/)，PLoS One 13:e0195955；
原文没有写明这个 1% 的分母是全部文章还是只有带流程图的那些）。**如实记下的空缺：** 我们没有找到任何已发表研究测量过
"流程图算术对不上的比例"。我们能查到的所有这类说法都出自工具厂商的博客，因此我们不引用其中任何一个。

**提取错误。** 27 篇 Meta 分析、每篇抽两项试验，17 篇（63%）至少有一项试验存在错误，其中 10 篇（37%）的合并结果无法复现到
0.1 以内（Gøtzsche 2007, JAMA 2007;298:430-7）。从 33 篇已发表 Meta 分析抽出的 500 个原始效应量里，224 个无法依其报告的
信息复现，并在其中 13 篇里造成了差异（Maassen 2020, PLOS One 15:e0233107）。
上面两项是错误发生率。还有一项常被并列引用的研究属于另一类东西：
一项方法学综述去检索"描述错误类型"的文章，从纳入的 50 篇文章里归纳出 139 种成对 Meta 分析中可能出现的错误**类型**——
数据提取或数据处理 25 种、统计分析 74 种、结果解释 40 种（Kanukula 2024, J Clin Epidemiol 170:111331）。
它是"可能出什么错"的分类表，不是"实际出了多少错"的计数。

**方案偏离。** 97 篇 Cochrane 与 97 篇非 Cochrane 综述中，两个子样本各有一半以上在 PICOS 要素上有改动；非 Cochrane 综述中
95.8% 的改动没有被报告，Cochrane 综述为 42.6%（Siebert 2023, PeerJ 11:e16016）。
96 篇在 PROSPERO 注册的综述中，32% 存在主要结局不一致，39% 没有
明确写出主要结局（Tricco 2016, J Clin Epidemiol 79:46-54）。75 条可分析的 PROSPERO 记录中，63 条（84.0%）不是最新状态
（Rombey 2020, J Clin Epidemiol 117:60-67）。而 2024 年的开放获取综述里，只有 4.3% 出现任何"方案偏离"类表述——
这一步要产出的东西，几乎从来没有被产出。

**跨语言重复发表。** 470 项以期刊论文形式发表的中国资助随机对照试验中，55 项（11.7%）有 75 篇重复发表，其中 53 篇（70.7%）
跨越语言边界，全部靠人工识别（[JAMA Netw Open 2020](https://pmc.ncbi.nlm.nih.gov/articles/PMC7716193/)）。ICMJE 建议在
满足特定条件时允许以另一种语言二次发表，所以这些并不全是学术不端——正因如此，综述团队必须自己去查，不能假设它们会被标出来。

**可信度。** 50 篇 Cochrane 综述里的 95 项随机对照试验中，评价者对 25% 的试验有一些顾虑、对 6% 有严重顾虑；把这两类都剔除后，
22% 的 Meta 分析一项试验都不剩（[INSPECT-SR 第二阶段](https://pubmed.ncbi.nlm.nih.gov/40349737/)，
J Clin Epidemiol 2025;184:111824）。这篇论文自己写明了最后那个数字的限制：评价只限于纳入不超过 5 项 RCT 的
Meta 分析，其中 54% 只含 1 项 RCT，"which will distort the impact on results"（这会让对结果的影响被扭曲）。
工具本身另有一篇[预印本](https://pubmed.ncbi.nlm.nih.gov/40950444/)描述，尚未经过同行评议。
开发者实测用该工具评一项试验的中位耗时为 45 分钟（IQR 27–74）。而当前实践接近于零：2024 年开放获取综述里提到 GRIM 或
GRIMMER 的占 0.27%。

**中文库的工作量。** 2024 年 PMC 题名限定的综述中，8.7% 在全文里提到中文数据库；在带中国机构的综述中升到 34.0%；
提到中文库的综述里 93.9% 带中国机构。在提到知网的那些里，70.4% 同时提到万方、44.5% 同时提到维普，所以检索式转换与去重这两件事
每个项目要重复 2–4 次。反面证据也要说清：Cochrane 强制的数据库集合是 CENTRAL、MEDLINE、Embase——不含任何中文库——
[2015 年一项 Cochrane Colloquium 分析](https://abstracts.cochrane.org/2015-vienna/searching-chinese-biomedical-databases-current-practice-among-cochrane-reviewers)
发现当时已发表的 8,680 篇 Cochrane 综述里只有 243 篇（不到 3%）检索了中文库，这 243 篇里有 118 篇（49%）
是补充与替代医学主题。而在确实要紧的那类主题里，96.64% 的合格研究只能从中文库找到（[Wu 2013](https://pubmed.ncbi.nlm.nih.gov/24223063/)）。

**发表偏倚的实际做法。** 两份顶级运动科学期刊的 46 篇综述中，47.8% 在合并研究少于十项时仍评估了发表偏倚，28.3% 只看漏斗图
而不做任何统计检验（[2024 年教育性综述](https://pmc.ncbi.nlm.nih.gov/articles/PMC10933152/)）。Cochrane Handbook 的规则是，
漏斗图不对称检验"should be used only when there are at least 10 studies"（只在至少有 10 项研究时使用）
（[第 13 章](https://www.cochrane.org/authors/handbooks-and-manuals/handbook/current/chapter-13)）。

**软件互转。** 2024 年有 404 篇综述在摘要里同时提到 RevMan 与 Stata，只有 33 篇同时提到 RevMan 与 R。
提到 RevMan 的有 62%、提到 Stata 的有 69% 带中国机构。这两个数都是下限，因为软件名通常写在方法学部分而不是摘要里。

## 证据变了怎么办

一篇综述是对"某一时刻的一批研究"下的判断。而这批研究会继续变：有的被撤稿，有的发了更正而改掉一个数字，你提取过的预印本
变成了正式论文而结果不同。此时综述团队面对的问题不是"我纳入的研究里有没有被撤稿的"——第 16 行里那几个免费工具就能回答——
而是紧接着的下一个问题：**我的哪一个合并结果、哪一条结论是依赖它的。**

前一个问题的规模是被测量过的。1,330 项被撤稿的随机对照试验中，312 项（23.5%）进入了 847 篇综述的 4,095 个 Meta 分析；
在可以重算的 3,902 个里，剔除撤稿试验后效应方向改变 8.4%、统计学显著性改变 16.0%；68 篇结论被扭曲的综述被 157 份指南类文件使用，
其中 89 份（57%）是严格意义上的临床实践指南，其余是共识声明、立场声明、临床公告与委员会意见
（[Xu 2025, BMJ](https://doi.org/10.1136/bmj-2024-082068)）。请注意这句话没有说的部分：是 312 项试验污染了 847 篇综述，
不是 1,330 项。

而几乎没有什么会传到下游。撤稿发生在综述发表之后的情形里，196 篇综述中只有 9 篇、43 份指南中只有 2 份做了更正或撤回
（Kataoka 2022, J Clin Epidemiol）。而且光提醒也不管用：一项随机对照试验向引用了撤稿文献的论文作者发邮件，
7,958 篇对 7,963 篇随机分组，共 246,749 封邮件，一年后引用率没有差异（−0.007，95% CI −0.055 至 0.041），
同时回复的作者中 80.6%（12,631/15,667）表示自己并不知道那篇已被撤稿（RetractoBot，Peer Review Congress 摘要）。

反面证据应当给同样的版面。另一项研究识别出 61 篇含撤稿研究的综述，从其中能提取数据的 50 篇里提取，
并重算了这些综述的 173 个 Meta 分析中的 166 个：重算结果有 160 个（96%）仍落在原来的置信区间内，
18 个（11%）显著性发生改变（Graña Possamai 2025, JAMA Intern Med）。也就是说，单个问题研究的通常影响很小。
要紧的是尾部，而不重算就没法知道自己落在哪一种情况里。

时点决定了"投稿前核查"能起多大作用。在同一项研究提取的这 50 篇综述里，37 篇（74%）的撤稿发生在综述发表**之后**，
所以投稿这道关口最多能拦住 50 篇中的 13 篇。要求也指向同一个方向：ICMJE 建议写明作者有责任检查参考文献是否已被撤稿，并把 PubMed 列为权威来源；
Cochrane MECIR 要求综述更新时重新核查勘误与撤稿，靠人工；PRISMA 2020 完全没有这一条。
2024 年的开放获取综述里，方法学部分出现"retracted"的占 0.48%。

对"我的哪条结论依赖它"这一步，作者自己的工具 **Evidence OS Inspector** 是一个小而局部的答案——纯浏览器端、
MIT 许可、alpha（仓库于 2026-09-26 读取）。它把结论绑定到其出处段落，并在来源发生变化时标出哪些结论需要重新复核。
它不发现来源变了，不绑数字，也没有任何关于省时的实测。它是这一步里很小的一块，不是这一步的解决方案。
**利益冲突：** 这是作者本人的工具。它不是表里的一行，没有参加实测；和本页提到的所有工具一样，它是被列出，不是被推荐。
[仓库](https://github.com/yinchuan123/evidence-os-inspector) ·
[演示](https://yinchuan123.github.io/evidence-os-inspector/)

## 这张地图是怎么做的

**先行工具普查（2026-09-17）。** 四个代理各负责五个候选方向。每个方向都做到：中英文各至少两种检索策略，加上 GitHub 检索 API、
SR Toolbox 目录与中文平台。只有打开过工具自己的页面——官网、仓库、CRAN 页面、文档或开发者本人的论文——才会带链接列入；
有少数工具只写了名字没有链接，因为它们是通过目录收录或别的工具的文档进入视野的，这些行标注*未打开页面*。
打不开的都如实记下：SR Toolbox 现在是一个 Streamlit 应用，对抓取只返回"Streamlit"一个词；TERA 与若干厂商站点是
没有可读文字的 JavaScript 应用；CMeSH、SinoMed 检索、Covidence、GRADEpro 的应用内部都要登录，我们没有尝试；知乎返回 403。
全程没有注册、登录、付费或提交任何表单。

**需求测量（2026-09-26）。** 三个代理对每个方向测量：量（通过 PubMed E-utilities、PMC、Europe PMC 接口现场计数）、
要求（打开并逐字引用指南或期刊原文）、已发表的问题发生率。反面证据是强制项——每个方向在工作笔记里都有一段"反对"，
其中最强的那些已写进上一节。

**真题实测（2026-09-26，撤稿那道题为 2026-09-16）。** 9 道题全部用真实公开材料出。每道题的题目、标准答案与打分规则，
都由另一个代理在被打分的那一轮之前完成，答案里每一项都由逐字引文或写全的算式确立。受测模型是在代理环境中运行的
Claude Fable 5.1，只允许网页搜索与抓取；事后审计了调用记录，没有读取本机文件。打分是另一轮，逐条对照。
完整方法、题目清单与结果见 [LLM_BENCHMARK.zh-CN.md](LLM_BENCHMARK.zh-CN.md)。

**哪些是测出来的、哪些是假设的。** 计数、价格、工具能力都是测出来或从页面上引下来的。大模型那一列，9 行有实测、17 行没有——
"未测"就是没测过。先行普查还为每个方向写了一条"模型会怎么失败"的假设；这些假设**没有**放进本地图，
因为在我们真正测过的九个方向里，其中两条预测的弱点并没有出现。

**局限，直说。**

- 每个方向一道题、跑一次、一个模型家族——撤稿那道题例外，它联网跑了两次、断网跑了一次。跑一次分不清本事和运气。
- 26 行里有 17 行没有大模型实测。
- 先行普查是每个候选一个人查，没有做对抗性的第二轮复核。
- 没有测 GPT-6，也没有测任何其他厂商的模型。
- 实测跑在一个能直接抓取接口返回的代理环境里，所以普通聊天界面可能更差。
- 标准答案是在一手来源上借助模型整理的，所以答案本身可能错——而且确实错过一次：撤稿题上模型查出了标准答案漏掉的一份已发表更正声明，标准答案在打分前被改正。
- "全文里提到某个数据库"是"检索了该数据库"的代理指标，不是判定。一处提及可能只是出现在参考文献题名或局限性的句子里。
- PMC 的分母偏向开放获取，约为同年 PubMed 计数的 59%。`China[affiliation]` 匹配任何含"China"的机构字符串，
  会漏掉台湾和部分香港写法。
- 我们所用工具里的网页搜索偏美国，中文论坛与 BBS 的需求被系统性地低估；本页的中文侧数字都应读作下限。
- 有一个被广泛转引的数字——"20.6% 的检索式没有为其他数据库做适当转换"——**并不在**通常被归属的那篇论文里。
  我们查过全文并弃用了它。请不要把它引到我们这里。
- 这里没有任何东西构成临床验证，也没有任何一句话是说模型的输出可以不经人工核查就使用。

## 欢迎纠错

这张地图有写错的地方。价格会变，工具会补上缺的功能，从厂商页面抄下来的一行也可能读错。如果你能修，请动手：

- **漏了某个工具、价格写错、描述有误** —— 开 issue 或 pull request，附工具自己的页面链接和你打开它的日期。一个 issue 一个工具。
- **你是这里某个工具的作者** —— 关于自己工具的纠正非常欢迎，不视为利益冲突。请说明这是你的工具，以便在行里记下是谁提出的更正。
  准确的局限不会因为请求而删除；不准确的会改；你已经修好的局限，我们很乐意记为"已修复"。
- **某条实测结果看起来不对** —— 题目、标准答案与打分规则都是公开的，你可以自己核。请说明你重跑了什么。

实际是怎么处理的：只有一个维护者，没有任何服务承诺。有人开 issue 并附上工具自己的页面和打开日期时，那一行会被重新
标注日期；没人来重新核的行就保留它已有的日期——这也是为什么每一行都把日期印出来。

规则、证据门槛与 issue 模板都在 [CONTRIBUTING.md](CONTRIBUTING.md)（目前只有英文版，暂无中文翻译；
用中文开 issue 完全可以）。一句话版本：工具自己的页面、你打开它的日期，查不到就写 `unverified / 未核实`，不要猜。

## 关于作者

**Chuan Yin（尹川）** —— 上海交通大学
ORCID [0009-0005-2830-8167](https://orcid.org/0009-0005-2830-8167)
邮箱 `yinchuan [at] sjtu.edu.cn`

联系：[yinchuan@sjtu.edu.cn](mailto:yinchuan@sjtu.edu.cn)

## 许可

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) —— 见 [LICENSE](LICENSE)。
