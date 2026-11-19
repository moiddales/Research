# 研究计划：AI 文本检测工具准不准、稳不稳、能不能被绕过（修订版）

Nov 19, 2026 · @L

## 修订说明

本版在 11 月 18 日版的基础上补了 19 条研究假设，修正了样本数字和统计功效的说法，加了混杂控制和三组新语料，并把"AI 写、人改"换成拼接实验。研究问题和七个子问题不变。

| 改动 | 所在部分 | 原因 |
| --- | --- | --- |
| 新增 19 条研究假设，分核心和探索性两类 | 研究假设 | 原版只有问题，没有可以证伪的预期 |
| 新增"AI 参与程度"0–4 级，事先规定回译、改语法的文本算哪一类 | 研究问题 | 决定这些文本被判成 AI 时算误伤还是算检出 |
| 人写样本定为 330 篇：BAWE 240 + 已发表论文 60 + 同学课堂写作 30 | 数据、实验 | 原版数据表 300 篇和 E1 的 330 篇对不上 |
| 新增托福作文 91 篇（复现 Liang et al.）和 ICNALE 水平分组样本（选做） | 数据、E1 | 区分"工具变好了"和"文本不一样"；弥补 BAWE 只收高分作业 |
| AI 文本增至 240 篇，与 BAWE 按题目一一配对；4 个模型 × 3 种提示；要求英式英语 | 数据 | 控制主题、拼写和生成模型的差异 |
| "AI 写、人改"改为拼接混合；人工修改缩到 20 篇，作为选做 | 数据、E5 | 原方案太耗时；拼接文本有精确到句子的标准答案 |
| 新增 E0 试点和长度对照；E2 改选中间分数，加时间漂移和上下文对照 | 实验 | 原样重测大概率完全一致，测了说明不了问题 |
| 降 AI 方法加测语义保真度；人性化工具改为 2 款；回译加中文 | E4、指标 | 只看检出率会高估这些方法的实用性 |
| 子问题 7 和 E6 改以连续分数为主要结果 | 分析方法 | 误伤接近 0 时，二元比较和逻辑回归都做不了 |
| 写明统计功效：本研究无法证实 0.019% 级别的误伤率 | 分析方法 | 330 篇全部判对时，误伤率上限仍约 0.9% |
| 原版与改动版同批提交、顺序随机 | 实验 | 防止工具中途更新混进比较 |
| 新增数据上传第三方服务的许可核查；新增"局限"一节 | 伦理、局限 | 语料会被上传到多个商业服务 |
| 按新的检测量重算预算 | 工具与预算 | 每款工具的检测从约 1,530 次增至约 2,010 次（核心部分） |

## 研究问题

本研究检验 AI 文本检测工具能不能分清英文学术写作是人写的、AI 写的，还是两者混合的；它们给出的判断和置信度可不可信；以及哪些改动会让判断改变。研究对象是这类工具本身，GPTZero 和 Pangram 是本研究选用的两款主要工具。

**主研究问题（报告标题）**

> To what extent can AI-text detectors correctly identify AI involvement in English academic writing, and how easily can their judgements be changed?

**七个子问题，分四部分**

*第一部分：判得准不准*

1. **阴性和阳性准不准**：人写的论文会不会被误判成 AI？AI 写的论文能不能被认出来？两款工具会不会错在同样的文章上？
2. **置信度准不准**：工具说"98% 是 AI"的时候，是不是真有那么大把握？

*第二部分：稳不稳定*

3. **原样重测**：同一篇原封不动再测一次，结果会不会变？放回原文上下文里测、隔几周再测，又会不会变？
4. **小改动**：换几个标点、故意加几个语法错误、插入空白字符或随机字符——这些不改变内容的改动，会不会改变结果？

*第三部分：能不能被绕过*

5. **降 AI 方法**：AI 同义替换、专门的"人性化"改写工具、翻译成小语种再译回英文并重复多次，能把 AI 判定降低多少？改写后原意还剩多少？工具能不能认出这些改写？

*第四部分：混合文本与系统性误判*

6. **两种混写**：工具能不能区分"AI 生成的内容占一部分"和"人写、AI 润色"？给出的 AI 占比和标出的位置对不对？
7. **系统性误判**：工具会不会系统性地误判某一类文章，例如非英语母语作者写的、英语水平较低的、某些学科或体裁的，或者某种写作风格的？

**范围**：第一阶段只用英文学术写作，包括大学课程作业、已发表的论文和中学生课堂写作；托福和 ICNALE 作文作为对照组。其他语言和体裁作为后续补充。

**AI 参与程度的定义**

主研究问题里的 "AI involvement" 分为五级。每篇文本在检测前定好等级。

| 等级 | 含义 | 本研究中的文本 | 被判成 AI 时算 |
| --- | --- | --- | --- |
| 0 | 纯人写 | BAWE、已发表论文、同学课堂写作、托福作文、ICNALE | 误伤 |
| 1 | 人写，工具辅助 | 人写文本的回译版；AI 只改语法和拼写的版本 | 误伤（主分析） |
| 2 | 人写，AI 润色 | 轻度润色、全面润色、按学术风格重写 | 不算对错，看分数随编辑比例怎么变 |
| 3 | 混合 | 拼接文本、AI 写人改 | 不算对错，看工具给出的占比和标注位置 |
| 4 | AI 写 | AI 原文，以及经过同义替换、人性化、回译的 AI 文本 | 检出（判成人写算漏检） |

等级 1 按"人写"计，因为多数学校允许学生用拼写检查和翻译软件；如果学校规定不同，结论要相应调整。除了判得对不对，还计算工具分数和等级的 Spearman 相关，看分数是否随 AI 参与程度单调上升。

## 研究假设

下面 19 条假设在开始正式检测前写进研究日志并注明日期。★ 为核心假设，正式检验并在报告中重点讨论；其余为探索性分析，只描述、不下强结论。

| # | 子问题 | 假设 | 怎么判定 |
| --- | --- | --- | --- |
| H1 | 1 | 两款工具在 BAWE 人写文本上的误伤率（严格口径）都不超过 1% | 误伤率加 Clopper-Pearson 区间；区间下限高于 1% 即否定 |
| H2 ★ | 1 | 漏检率从低到高：普通提示 < 模仿大学生 < 给范文模仿；四个生成模型之间也有差别 | 按提示和模型分组比较漏检率（Fisher 检验）和分数（Kruskal-Wallis 检验） |
| H3 ★ | 1 | 文本越短，误伤和漏检越多 | 40 篇各截 150 / 300 / 600 词，配对比较三档的分数（Friedman 检验） |
| H4 ★ | 1、7 | 托福作文的误伤率远低于 2023 年报告的 61.3%，但仍高于 BAWE 非母语组 | 比例加区间；Fisher 检验。结果决定是"工具变好了"还是"文本不一样" |
| H5 | 2 | GPTZero 标为高置信度的判断，准确率达到厂商宣称的 98% 以上 | 按置信度类别分组计算准确率和区间 |
| H6 ★ | 2 | 遇到不熟悉的文本（非母语、回译、混写）时，工具判错了仍然很自信 | 判错的文章里高置信度的比例，按文本类型比较 |
| H7 | 3 | 同一时段原样重测结果一致；但同一段落放回原文上下文中提交，分数会变；研究结束时重测，部分判定已经改变 | 一致率；配对 Wilcoxon 检验；时间漂移的翻转率 |
| H8 ★ | 4 | 改动的影响不对称：AI 文本加错误后分数下降，人写文本加错误后分数不升 | 两组分别做配对 Wilcoxon 检验，比较效应量 |
| H9 | 4 | 把破折号等"AI 标志性"标点换掉，会降低 AI 文本的分数 | 配对 Wilcoxon 检验 |
| H10 ★ | 5 | 回译轮数越多，检出率越低，但语义保真度同步下降；没有哪一轮能既躲过检测又保住原意 | 画检出率—保真度曲线；"保住原意"的门槛在试点后、正式检测前定好 |
| H11 | 5 | 人写文本回译后 AI 分数上升；经斯瓦希里语比经中文上升更多 | 配对 Wilcoxon 检验 |
| H12 | 5 | Pangram 4 的人性化分数能认出多数人性化工具的输出；人性化工具让 GPTZero 的检出率降得比 Pangram 多 | 识别率；两款工具检出率下降幅度之差 |
| H13 ★ | 6 | 人写 + AI 润色被判 AI 的概率随编辑比例上升，存在临界点；只改语法时低于临界点 | 拟合逻辑曲线，估计"一半被判 AI"时的编辑比例 |
| H14 | 6 | AI 内容占比相同时，集中成段的 AI 文字（拼接）比分散在句子里的 AI 修改（润色）更容易被判成 AI；做了选做的人工修改组时，再比较"AI 写、人改"和"人写、AI 润色" | 以 AI 占比或编辑比例为横轴，比较各组曲线；回归中加入混写类型 |
| H15 | 2、6 | Pangram 给出的 AI 占比和拼接文本的真实占比接近线性（斜率约为 1）；两款工具标出的位置与真实 AI 句子的重合度高于随机 | 线性回归；词级精确率、召回率、F1 |
| H16 ★ | 7 | 非母语组和格式化体裁（如实验报告、方法描述、案例分析）的平均 AI 分数更高，即使误伤率没有差别 | Mann-Whitney 检验；以分数为因变量、加入学科和体裁的回归 |
| H17 ★ | 7 | 词汇多样性低、困惑度低、连接词多的人写文本 AI 分数更高，这些特征能解释大部分母语差异；困惑度对 GPTZero 分数的解释力大于对 Pangram | 以分数为因变量的回归，比较加入写作特征前后"母语"系数的变化 |
| H18 ★ | 1、7 | 两款工具错在同样的文章上：被一款误判的人写文本，被另一款误判的概率远高于平均 | Cohen's κ；条件误判率；两款都判 AI 才算 AI 时的误伤率 |
| H19 | 7 | （选做）ICNALE 作文的英语水平越低，AI 分数越高；同题目的母语者分数最低 | 水平等级与分数的 Spearman 相关 |

**检验规则**

- 显著性水平 0.05，核心假设用 Holm 法做多重比较校正；探索性假设只报效应量和置信区间。
- 假设、样本和分析方法在正式检测前写进研究日志。

## 为什么值得做

- **检测结果正在影响学生。** Cambridge International 要求教师认证课程作业是学生独立完成的，同时提醒检测工具既会漏检也会误报（[Cambridge](https://www.cambridgeinternational.org/support-and-training-for-schools/artificial-intelligence)）。Turnitin 称，在 AI 内容超过 20% 的文档上，它的误伤率低于 1%（[Turnitin](https://www.turnitin.co.uk/blog/ai-writing-detection-update-from-turnitins-chief-product-officer)）；范德堡大学按 1% 算了一笔账，一年会冤枉约 750 篇论文，于是关掉了这项功能（[Vanderbilt](https://www.vanderbilt.edu/brightspace/2023/08/16/guidance-on-ai-detection-and-why-were-disabling-turnitins-ai-detector/)）。2026 年 1 月，美国 Adelphi 大学一名学生的论文被 Turnitin 判为 100% AI，最后通过诉讼才撤销处分（[判决](https://www.nycourts.gov/reporter/3dseries/2026/2026_26021.htm)）。
- **研究结论正好相反。** 2023 年的研究发现，7 款检测器平均把 61.3% 的非母语托福作文判成 AI（[Liang et al.](https://arxiv.org/abs/2304.02819)）；而 2025–2026 年的新测试中，Pangram 和 GPTZero 在人写文本上几乎零误伤（[Jabarian & Imas](https://bfi.uchicago.edu/wp-content/uploads/2025/09/BFI_WP_2025-116.pdf)、[Epoch AI](https://epoch.ai/data-insights/ai-text-detection-challenges)）。是工具变好了，还是测试用的文本不一样？本研究把 Liang et al. 用过的同一批托福作文拿来重测：如果新工具在这批作文上也几乎不误伤，说明是工具变好了；如果仍大量误伤，而 BAWE 上不会，说明是文本不一样。
- **"降 AI 率"的方法到处流传，却很少被系统地测过。** 哪种真的有效、改写后原意还剩多少、会不会让本来人写的文章反而被判成 AI，都缺少数据。
- **现实中最常见的是人和 AI 一起写的文章。** 工具对这类文章给出的结果到底意味着什么，目前说不清楚。

## 已有研究

| 研究 | 发现 | 对本研究的用处 |
| --- | --- | --- |
| [Liang et al., 2023](https://arxiv.org/abs/2304.02819) | 7 款检测器平均把 61.3% 的非母语托福作文判成 AI；91 篇托福作文已[公开](https://github.com/Weixin-Liang/ChatGPT-Detector-Bias)（MIT 许可） | 子问题 7；本研究直接重测这批作文（H4） |
| [Weber-Wulff et al., 2023](https://arxiv.org/abs/2306.15666) | 14 款工具都不够准，机器翻译和改写让表现更差 | 子问题 5 |
| [Perkins et al., 2024](https://arxiv.org/abs/2403.19148) | 6 款检测器在 AI 文本上准确率 39.5%，简单处理后降到 17.4% | 子问题 5 |
| [RAID, 2024](https://arxiv.org/abs/2405.07940) | 拼写错误、零宽字符、同形字符等 11 种改动会明显干扰检测器 | 子问题 4 的改动类型；"低误伤率下的检出率"这一指标 |
| [Sadasivan et al., 2023](https://arxiv.org/abs/2303.11156) | 反复改写能大幅降低检测准确率 | 子问题 5 的多轮改写 |
| [ESPERANTO, 2024](https://arxiv.org/abs/2409.14285) | 译成其他语言再译回英文，检出率明显下降 | 子问题 5 的回译 |
| [Beemo, 2025](https://arxiv.org/abs/2411.04032) | 专家修改过的 AI 文本能躲过检测 | 子问题 6 的"AI 写、人改" |
| [APT-Eval, 2025](https://arxiv.org/abs/2502.15666) | 轻度 AI 润色也常被判成 AI | 子问题 6 的"人写、AI 润色"（H13） |
| [Jabarian & Imas, 2025](https://bfi.uchicago.edu/wp-content/uploads/2025/09/BFI_WP_2025-116.pdf) | Pangram 在中长文本上误伤和漏检都接近 0；文本很短时所有工具都变差 | 子问题 1；长度对照（H3） |
| [Epoch AI, 2026](https://epoch.ai/data-insights/ai-detectors-false-negatives) | 495 篇人写文本零误伤；模仿真实作者风格的 AI 科学写作约 26% 没被检出 | 子问题 1 的"给范文模仿"提示（H2） |
| [Kobak et al., 2025](https://arxiv.org/abs/2406.07016) | 用"异常高频词"分析 1,500 多万篇 PubMed 摘要，估计 2024 年至少 13.5% 的摘要经过大模型处理 | E6 的"AI 高频词"写作特征（H17） |
| [Int. J. Educational Integrity, 2026](https://link.springer.com/10.1007/s40979-026-00226-w) | GPTZero、Pangram、Copyleaks、Turnitin 在学术论文上误伤很少 | 和本研究最接近，需精读 |

厂商也给出了可以直接检验的说法：Pangram 4 称自己在近 6.3 万篇英文学术写作上的误伤率为 0.019%（[Pangram 4 模型说明](https://www.pangram.com/research/model-card/pangram-4)）；GPTZero 称它的高置信度判断中，人写文章 99.1% 被判为人写、AI 文章 98.4% 被判为 AI（[GPTZero](https://support.gptzero.me/articles/4083713560-how-do-i-turn-the-probabilities-from-your-api-into-outcomes)）。本研究的样本量只能检验后一种说法（H5）；前一种只能在出现误伤时被否定，无法被证实（见"指标与分析方法"）。

**本研究的新东西**

- 七个问题在同一批英文学术文本上一起测，可以直接比较：哪种改动影响最大、哪类文章最容易被误判。
- 在 Liang et al. 的同一批托福作文上重测，直接检验新旧研究结论的矛盾。
- 测置信度、原样重测和上下文效应。多数研究每篇只测一次，只看最终标签。
- 降 AI 方法同时测语义保真度，看"躲过检测"要付出多少原意。
- 用已知 AI 占比的拼接文本，检验 Pangram 给出的占比准不准，并逐句核对工具标出的位置。
- 检验两款工具是否错在同样的文章上，即"两款都查"能不能减少冤枉。
- 用可以计算的写作特征，解释"为什么这篇人写的会被判成 AI"。

## 工具给出的是什么

**GPTZero** 给整篇文章一个标签（HUMAN\_ONLY 人写 / MIXED 混合 / AI\_ONLY AI 写）和三类概率，并标出疑似 AI 的句子。按官方说明，这个概率表示"这个判断正确的把握"，不是"文章里有多少是 AI 写的"（[GPTZero](https://support.gptzero.me/articles/8947054519-how-do-i-use-and-interpret-the-results-from-your-api)）。

**Pangram** 把文章切成小段逐段判断，给出人写、AI 辅助、AI 生成三类文字各占的比例（按字符计），再给整篇标签：人写占比至少 90% 判为 Human，AI 生成占比至少 80% 判为 AI，其余为 Mixed。2026 年 7 月发布的 Pangram 4 还新增了识别"人性化改写"的分数（[Pangram 4](https://www.pangram.com/research/model-card/pangram-4)）。

所以平时说的"AI 率"在两款工具里不是一回事：GPTZero 的是把握，Pangram 的是占比。报告里分开写：GPTZero 的概率用校准曲线检验，Pangram 的占比用已知占比的拼接文本检验。

**判定口径**

- **严格口径**：只算 AI\_ONLY / AI。
- **宽松口径**：把 MIXED / Mixed 也算上。
- **软口径**：整篇判为人写，但有句子被高亮，或 Pangram 标出了 AI 辅助段落。老师实际看到的是高亮，不只是整篇标签，所以人写文本上的软口径比例单独报告，称为"软误伤"。

**每次检测记录的字段**

- GPTZero：整篇标签、三类概率、置信度类别（`confidence_category`）、逐句概率和高亮。
- Pangram：三类占比、整篇标签、逐段标签及其置信度、人性化分数（以 API 实际返回的字段为准，试点时确认）。
- 两者共同：提交日期和时间、接口和模型版本号、文本编号、词数、提交批次，以及原始返回结果（整段 JSON 存档）。

**术语**

| 术语 | 意思 |
| --- | --- |
| 误伤（假阳性） | 人写的被判成 AI |
| 漏检（假阴性） | AI 写的被判成人写 |
| 误伤率 | 人写文本里被判成 AI 的比例 |
| 漏检率 | AI 文本里被判成人写的比例 |
| 软误伤 | 人写文本整篇判为人写，但部分句子或段落被标为 AI |
| 低误伤率下的检出率 | 把门槛调到误伤率只有 1% 时，AI 文本能被检出多少 |
| 校准 | 工具说"90% 把握"的那批判断里，是否真有约 90% 判对 |
| 翻转率 | 改动前后，判定结果变了的文章所占比例 |
| 回译 | 译成另一种语言，再译回英文 |
| 语义保真度 | 改写后的文章和原文意思有多接近，用句向量相似度衡量 |
| 编辑比例 | 改动的词数占原文词数的比例，用来衡量混写文本改了多少 |
| 拼接文本 | 把同题目的人写段落和 AI 段落按句子拼在一起，AI 占比事先已知 |
| 95% 置信区间 | 由样本推出的真实比例的合理范围；样本越多，范围越窄 |

## 数据

人写文本 330 篇，另有托福和 ICNALE 两个对照组；AI 文本 240 篇，与 BAWE 一一配对；混写文本从这两组派生。

| 组 | 来源 | 数量 | 说明 |
| --- | --- | --- | --- |
| 人写：学生课程作业 | BAWE 语料库：英国大学本科到硕士的课程作业，写于 ChatGPT 出现之前 | 240 | 母语、非母语作者各 120；每名学生最多一篇；覆盖四个学科大类和多种体裁 |
| 人写：已发表论文 | 2020 年以前的开放获取论文（如 arXiv、PLOS ONE） | 60 | 专业学术写作，和学生作业对照 |
| 人写：同学课堂写作 | 2026 年同学在课堂、无电子设备条件下手写，由研究者逐字录入 | 30 | 检测器肯定没见过；最接近 Cambridge 课程作业的作者群体 |
| 对照：托福作文 | Liang et al. 公开的非母语托福作文 | 91 | 直接复现 2023 年的研究（H4） |
| 对照：ICNALE（选做） | 亚洲英语学习者作文，200–300 词，带 CEFR 水平分级，有同题目的母语者组 | 80 | 非母语 A2、B1 低、B1 高、B2+ 各 15 篇，母语者 20 篇（H19） |
| AI 写 | 4 个模型 × 3 种提示，按 BAWE 的同一批题目生成 | 240 | 与 BAWE 240 篇一一配对 |
| 混写 A：拼接 | 从配对文本中选 40 对，按句子拼接 | 40 × 3 档 | AI 占比 25%、50%、75% |
| 混写 A′：AI 写、人改（选做） | 从 AI 组选 20 篇，按事先写好的规则人工修改 | 20 | 按规则修改 |
| 混写 B：人写、AI 润色 | 从 BAWE 组选 40 篇，用 AI 润色 | 40 × 4 档 | 只改语法和拼写、轻度润色、全面润色、按学术风格重写 |

另有 20 篇（人写 10、AI 10）只用于 E0 试点，不进入正式分析。

**人写文本**

- BAWE 面向非商业研究者免费提供，需在牛津文本档案库注册并同意使用条件（[Warwick](https://warwick.ac.uk/fac/soc/al/research/collect/bawe/)）。每篇带有学科、年级、体裁和作者信息（[BAWE 介绍](https://varieng.helsinki.fi/CoRD/corpora/BAWE/index.html)、[Nesi & Gardner 2018](https://core.ac.uk/download/228154633.pdf)）。作者母语字段以拿到语料后的元数据为准，先核对再抽样。
- BAWE 只收成绩较好的作业（Warwick 称为 proficient writing；具体门槛是否为 merit 及以上，引用前在 Nesi & Gardner 2018 中核对）。所以 BAWE 代表写得好的学生，较弱的写作由托福和 ICNALE 组补充。
- 同一学生有多篇时只随机取一篇，避免样本不独立。
- 母语和非母语两组在学科和体裁上尽量平衡，否则母语差异会和学科差异混在一起。
- 同学课堂写作的题目从 BAWE 题目中选与中学课程接近的；记录作者年级和是否以英语为母语。
- ICNALE 作文是两个固定话题的议论文，不是学术写作，只用来看英语水平的影响（[ICNALE](https://language.sakura.ne.jp/icnale/)）。

**AI 文本**

- **模型**：4 个模型家族各 60 篇，例如 GPT、Claude、Gemini 各取当前主力版本，再加一个开源模型（如 Qwen 或 Llama）。记录模型名、版本、日期和生成参数。
- **提示**：每个模型下三种提示各 20 篇。
  1. 普通：给出题目、学科、年级、体裁和字数。
  2. 模仿大学生：在普通提示上要求写得像英国大学生。
  3. 给范文模仿：附上 1–2 篇同学科的 BAWE 范文（不是配对的那一篇，也不进入人写样本），要求模仿风格。
- 所有提示都要求：英式英语；只写正文段落，不用标题、列表和加粗。先生成整篇再截取，和人写文本一样是"长文中的一段"。
- **配对**：每篇 AI 文本对应一篇 BAWE 文本，题目、学科、体裁、年级相同，主题因此不再是混杂因素。
- **时代线索**：抽查 AI 文本是否提到 2008 年以后的事件或技术，有则记录并在分析中标注。

**截取与清洗**（所有组用同一个脚本）

- 每篇截取约 300 词的连续正文，从段落开头起、在句末结束。托福和 ICNALE 作文用全文，超过 300 词的同样截取。
- 删除标题、列表、表格、图注和 BAWE 中省略内容的占位标记。
- 括号引用（如 (Smith, 2003)）和编号引用（如 \[12\]）统一删除。AI 文本常常引用较少或编造引用，保留会成为区分人写和 AI 的线索。
- 只统一编码和空白，不改标点和拼写：标点是 E3 的实验变量。
- 已发表论文从 PDF 或 LaTeX 转成纯文本后，删除公式和残留的排版命令。

**混写文本**

- **拼接**：取一对配对文本，用 AI 文本中连续的若干句替换人写文本的对应部分，使 AI 词数占比约为 25%、50%、75%。一半文章的 AI 部分在前，另一半在后。逐句记录来源，作为核对工具标注的标准答案。
- **润色**：四档提示词事先写好，尽量用学生实际会输入的说法（如 "fix my grammar"、"improve my writing"），全部用同一个模型。用词级对比算出编辑比例，并标出哪些词是 AI 改的。
- **人工修改（选做）**：规则事先写好，例如改写开头和结尾句、替换 AI 高频词、拆分或合并长句、加入具体例子。

**记录**：每篇文本一个编号，记录组别、AI 参与等级、来源、学科、体裁、作者母语和词数；AI 文本记录模型、版本、日期和提示词；混写文本记录编辑比例和逐词来源标注。抽样和截取都用固定的随机种子，保证可以重复。

## 实验

全部实验每款工具约检测 2,010 次，选做部分再加约 100 次。

| 实验 | 子问题 / 假设 | 样本 | 做法 | 每款工具检测次数 |
| --- | --- | --- | --- | --- |
| E0 试点 | — | 人写 10 + AI 10（不进入正式分析） | 确认 API 返回字段、分数分布、清洗脚本和实际费用；定下语义保真度门槛等待定参数 | 20 |
| E1 基础判断 | 1、7；H1–H4、H16–H19 | 人写 330 + 托福 91 + AI 240（ICNALE 80 选做） | 每篇测一次，记录全部字段 | 661（+80） |
| E1b 长度对照 | 1；H3 | 人写 20 + AI 20 | 同一篇另截 150 词和 600 词两个版本 | 80 |
| E2 置信度与重测 | 2、3；H5–H7 | 置信度直接用 E1；重测 40 篇；时间漂移 60 篇；上下文 30 篇 | 40 篇分数居中的文章原样重测 3 次；研究结束时把 60 篇再测一次；30 个段落放回原文约 900 词的上下文中提交 | 210 |
| E3 小改动 | 4；H8、H9 | 人写 40 + AI 40 | 四种改动：替换标点（破折号换成逗号或括号、分号换成句号）；加入 3–5 处语法或拼写错误；插入空格和零宽字符（看不见的字符）；插入随机字符 | 320 |
| E4 降 AI 方法 | 5；H10–H12 | AI 40 + 人写 40 | AI 文本 8 个版本：AI 同义替换 1 轮和 3 轮、人性化工具 A 和 B 各 1 次、斯瓦希里语回译 1/3/5 轮、中文回译 1 轮；人写文本 3 个版本：斯瓦希里语回译 1 轮和 3 轮、中文回译 1 轮 | 440 |
| E5 两种混写 | 6；H13–H15 | 拼接 120 + 润色 160（人工修改 20 选做） | 比较分数和判定随 AI 占比或编辑比例的变化；逐句核对工具标出的位置 | 280（+20） |
| E6 系统性误判 | 7；H16–H19 | E1 的人写结果 | 按母语、学科、体裁、来源和英语水平分组比较；计算写作特征，找出和 AI 分数相关的特征 | 不需额外检测 |

**样本怎么选**

- E2 重测：只选分数不在两端的文章（例如 GPTZero 的 AI 概率在 0.2–0.8 之间，或 Pangram 判为 Mixed）。分数极端的文章重测后几乎不会变；另外 API 可能对相同文本有缓存，所以更重要的是时间漂移和上下文两项。
- E3：每组 40 篇中一半随机选、一半选分数居中的。随机部分给出有代表性的翻转率，居中部分看改动在"临界"文章上的作用，两部分分开报告。
- E4：AI 文本从两款工具都判为 AI 的文章中随机选，这样"降了多少"有明确起点；人写文本从 BAWE 中随机选，母语和非母语各半。
- E1b 和 E5 拼接：从配对文本中随机选。

**执行规则**

- 原版和它的所有改动版同一天、同一批提交，批内顺序随机打乱。这样即使工具中途更新，也不会混进改动前后的比较。
- 每组实验集中在一两周内做完，记录每批的日期和工具版本。
- 返回异常（报错、空结果、文本被截断）时重新提交一次并记入日志。
- 改动版都和自己的原版直接比，样本不多也能看清变化。
- 回译以小语种为主（例如斯瓦希里语），轮数越多，原文的痕迹被冲淡得越多，可以画出"轮数—检出率"和"轮数—保真度"两条曲线。加一种中文回译，是因为中文母语学生最可能这样用翻译软件。
- 人写文本也做回译：如果回译后的人写文章被判成 AI，说明用翻译软件辅助写作的学生可能被误伤。现在的机器翻译部分基于大模型，回译文本按"AI 参与程度"表中的规则归类。
- 降 AI 方法的每个版本都计算语义保真度、语法错误数和长度变化。
- E6 用 Python 计算每篇的写作特征：平均句长、句长起伏（人写的句子通常长短变化更大）、词汇多样性（用 MATTR，受长度影响小）、被动语态比例、连接词频率（moreover、furthermore 等）、AI 高频词频率、破折号频率，以及困惑度（用一个小型开源语言模型计算，数值越低说明文字越"在模型意料之中"）。
- 时间允许的话，加入一两款免费检测工具（例如 ZeroGPT、QuillBot 的 AI 检测器）做小样本对照，看结论是不是这两款独有的。

## 指标与分析方法

| 子问题 | 看什么 | 怎么做 |
| --- | --- | --- |
| 1 阴性和阳性 | 误伤率、漏检率、软误伤率；AUROC 和误伤率 1% 时的检出率；两款工具的一致性 | 比例加 Clopper-Pearson 区间；ROC 曲线；Cohen's κ |
| 2 置信度 | 校准曲线、Brier 分数；GPTZero 各置信度类别的准确率；Pangram 占比与真实占比 | 按把握分组比较"宣称"和"实际"，对照厂商数字；拼接文本做线性回归 |
| 3 原样重测 | 一致率、分数标准差；时间漂移和上下文造成的翻转 | 一致率；配对 Wilcoxon 检验 |
| 4 小改动 | 翻转率、分数变化 | McNemar 检验；配对 Wilcoxon 检验加效应量 |
| 5 降 AI 方法 | 检出率、语义保真度、语法错误数 | "轮数—检出率"和"检出率—保真度"曲线 |
| 6 两种混写 | 分数随 AI 占比或编辑比例的变化；标注的精确率、召回率、F1 | 逻辑曲线拟合；词级比对 |
| 7 系统性误判 | 分组的分数分布和误伤率；写作特征 | Mann-Whitney 和 Fisher 检验；以 AI 分数为因变量的回归 |

语义保真度用句向量余弦相似度计算，语法错误数用 LanguageTool 统计。

**统计功效**

- 330 篇人写文本全部判对时，误伤率的 95% 区间上限仍约 0.9%。要证实 Pangram 宣称的 0.019%，需要约 1.6 万篇，所以本研究只能在出现误伤时否定这一说法。
- 母语和非母语各 120 篇，误伤率大约要差 5 个百分点（如 0/120 对 6/120）才显著。所以子问题 7 以连续分数为主要结果，误伤率作补充。
- 误伤很少时无法以"是否被判 AI"做逻辑回归，E6 改以 AI 分数为因变量。

**被冤枉的比例**

被判成 AI 的学生里有多少是冤枉的，取决于班里实际有多少人用 AI，可以用贝叶斯公式算出来。例如误伤率 1%、检出率 95% 时：如果班里 5% 的人用 AI，被判 AI 的学生中约 17% 是冤枉的；如果只有 1% 的人用 AI，这个比例约为 51%。报告会代入实测数值及其区间上下限，并算出一个学生一学年交 20 篇作业时至少被误判一次的概率（误伤率 1% 时约 18%）。

## 工具与预算

| 用途 | 工具 |
| --- | --- |
| 主要检测 | GPTZero、Pangram（已有会员，用 API 批量检测） |
| 对照检测（可选） | 一两款免费检测工具，小样本对照 |
| 生成和改写 | 4 个模型家族；2 款人性化工具；机器翻译（如 Google 翻译）做回译 |
| 处理和分析 | Python：批量调用 API、截取文本、计算编辑比例、写作特征、困惑度、语义保真度，统计和画图；LanguageTool 统计语法错误 |

核心部分每款工具约 2,010 次检测、约 63 万词（按每篇 300 词估算）。Pangram API 每 100 词 0.05 美元，核心部分约 315 美元，加上选做部分约 330 美元（[价格](https://pangram.com/pricing?category=developers)）；个人套餐每月含 10 美元额度，非商业学术研究可以申请免费额度（[API 页](https://pangram.com/solutions/api)）。GPTZero 按套餐的词数额度使用，额度不够就分两个月做。预算紧张时依次压缩：去掉选做部分；E3 和 E4 降到每组 30 篇；长度对照只做 150 词一档。

## 伦理与数据使用

- 同样的改写和扰动手段已在 RAID、ESPERANTO、Beemo 等研究中公开测试过；本研究按方法类别报告结果，目的是让学生和学校看清这些工具的边界。
- BAWE 和 ICNALE 按各自许可使用。研究需要把文本上传到检测、翻译和人性化服务，开始前确认许可允许这样做（ICNALE 条款禁止复制和再分发数据）。
- 同学的文本取得同意，同意书写明会上传到哪些服务，并匿名处理。

## 局限

- BAWE 只有成绩较好的作业，且写于 2004–2007 年；较弱的写作由托福和 ICNALE 组部分补充。
- 公开语料可能在检测器的训练数据里，测出的误伤率可能偏低；同学课堂写作组用来对照。
- 每篇只取约 300 词，和整篇作业的表现可能不同，长度对照只能部分回答。
- 没有测试学校最常用的 Turnitin。
- 工具会更新，结论只对应测试时的版本。
- 美式英语的学生论文语料（如 MICUSP）可作为后续对照。

## 报告结构与字数

报告正文 5,000 词，结果和分析占一半左右；研究日志和参考文献另作两个文件。

| 部分 | 词数 | 内容 |
| --- | --- | --- |
| 引言 | 400 | 背景、研究问题、为什么重要 |
| 文献综述 | 900 | 检测工具的原理、新旧研究的矛盾、已知的降 AI 方法 |
| 方法 | 900 | 数据、实验、假设、分析方法 |
| 结果与分析 | 1,800 | 判得准不准 550；稳不稳定 300；能不能被绕过 450；混写与系统性误判 500 |
| 讨论与评价 | 500 | 结果对学生和学校意味着什么；研究本身的不足 |
| 结论 | 300 | 回答主研究问题 |
| 反思 | 200 | 研究过程中的关键决定和收获 |

和评分标准对应：研究 24 分（文献和数据收集）、分析 20 分（结果与分析）、评价 12 分（讨论与评价）、反思 12 分（反思和研究日志）、表达 12 分（结构、图表和引用）。

- 结果部分每节以对应的子问题开头，以一句结论结尾。
- 核心假设在正文展开，探索性结果简要报告。
- 图表紧跟在分析它的段落旁边。

## 参考文献

以下是待精读的原始文献（整理于 2026 年 11 月 18 日，19 日补充），正式引用前核对原文。

**检测器评估研究**

- [Liang et al. (2023) GPT detectors are biased against non-native English writers](https://arxiv.org/abs/2304.02819)
- [Weber-Wulff et al. (2023) Testing of detection tools for AI-generated text](https://arxiv.org/abs/2306.15666)
- [Perkins et al. (2024) GenAI Detection Tools, Adversarial Techniques and Implications for Inclusivity in Higher Education](https://arxiv.org/abs/2403.19148)
- [Dugan et al. (2024) RAID: A Shared Benchmark for Robust Evaluation of Machine-Generated Text Detectors](https://arxiv.org/abs/2405.07940)
- [Sadasivan et al. (2023) Can AI-Generated Text be Reliably Detected?](https://arxiv.org/abs/2303.11156)
- [Ayoobi et al. (2024) ESPERANTO: back-translation and AI detection](https://arxiv.org/abs/2409.14285)
- [Artemova et al. (2025) Beemo: Benchmark of Expert-edited Machine-generated Outputs](https://arxiv.org/abs/2411.04032)
- [Saha & Feizi (2025) Almost AI, Almost Human: The Challenge of Detecting AI-Polished Writing](https://arxiv.org/abs/2502.15666)
- [Jabarian & Imas (2025) Artificial Writing and Automated Detection](https://bfi.uchicago.edu/wp-content/uploads/2025/09/BFI_WP_2025-116.pdf)
- [Epoch AI (2026) AI detectors rarely flag human writing, but sometimes miss AI text imitating real authors](https://epoch.ai/data-insights/ai-text-detection-challenges)
- [Epoch AI (2026) AI detectors: false negatives](https://epoch.ai/data-insights/ai-detectors-false-negatives)
- [International Journal for Educational Integrity (2026) GPTZero, Pangram, Copyleaks and Turnitin on academic papers](https://link.springer.com/10.1007/s40979-026-00226-w)
- [Kobak et al. (2025) Delving into LLM-assisted writing in biomedical publications through excess vocabulary. Science Advances](https://arxiv.org/abs/2406.07016)

**工具说明（厂商资料）**

- [Pangram 技术报告](https://arxiv.org/abs/2402.14873)
- [Pangram 4 模型说明](https://www.pangram.com/research/model-card/pangram-4)
- [Pangram API 文档](https://docs.pangram.com/api-reference/ai-detection)
- [GPTZero：如何解读 API 结果](https://support.gptzero.me/articles/8947054519-how-do-i-use-and-interpret-the-results-from-your-api)
- [GPTZero：如何把 API 概率转成判定](https://support.gptzero.me/articles/4083713560-how-do-i-turn-the-probabilities-from-your-api-into-outcomes)
- [Turnitin (2023) AI writing detection update from Turnitin's Chief Product Officer](https://www.turnitin.co.uk/blog/ai-writing-detection-update-from-turnitins-chief-product-officer)
- [Originality.ai 翻译与检测研究](https://originality.ai/blog/ai-translation-impact-ai-detection-study)

**语料**

- [BAWE 语料库介绍](https://varieng.helsinki.fi/CoRD/corpora/BAWE/index.html)
- [Nesi & Gardner (2018) The BAWE corpus and genre families classification of assessed student writing](https://core.ac.uk/download/228154633.pdf)
- [Warwick：BAWE 获取方式](https://warwick.ac.uk/fac/soc/al/research/collect/bawe/)
- [Liang et al. 数据与代码（含 91 篇托福作文）](https://github.com/Weixin-Liang/ChatGPT-Detector-Bias)
- [ICNALE：International Corpus Network of Asian Learners of English](https://language.sakura.ne.jp/icnale/)

**政策与案例**

- [Cambridge International：Teaching, learning, assessment and AI](https://www.cambridgeinternational.org/support-and-training-for-schools/artificial-intelligence)
- [Vanderbilt: Why we're disabling Turnitin's AI detector](https://www.vanderbilt.edu/brightspace/2023/08/16/guidance-on-ai-detection-and-why-were-disabling-turnitins-ai-detector/)
- [Matter of Newby v Adelphi Univ. (2026)](https://www.nycourts.gov/reporter/3dseries/2026/2026_26021.htm)

**Cambridge IPQ 要求**

- [9980 评分方案（2026 年 5/6 月）](https://www.cambridgeinternational.org/Images/763937-2026-mark-scheme-01.pdf)
- [9980 考官报告（2026 年 3 月）](https://pastpapers.papacambridge.com/directories/CAIE/CAIE-pastpapers/upload/9980_m26_er.pdf)
