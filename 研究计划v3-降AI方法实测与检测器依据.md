# 研究计划 v3：降 AI 方法实测与检测器依据

Nov 19, 2026 · @L

## 方向调整

实测中 GPTZero 和 Pangram 在未改动的人写和 AI 文本上几乎零误伤、零漏检，"判得准不准"已经有了答案。研究重心转向两件事：网上流传的降 AI 方法到底有没有用，以及检测器靠什么做出判断。

- **保留**：已完成的实测作为基线，写进报告结果的第一部分。
- **新增**：主流降 AI 方法实测；控制变量找检测器依据；多个 LLM 对比。
- **降级或去掉**：置信度校准、原样重测、系统性误判。零误伤时这些问题没有可分析的误差。

**新的主研究问题（报告标题）**

> What do AI-text detectors rely on to identify AI-generated academic writing, and can the "humanising" methods widely shared online evade them?

## 研究问题与假设

**三个子问题**

1. **降 AI 方法有没有用**：网上流行的降 AI 方法能把检测结果降多少？降下来后文章还能不能用？检测器能不能认出这些改写？
2. **检测器看什么**：每次只改变文本的一个特征、其余不变，哪些特征会让分数明显变化？
3. **不同 LLM 有没有差别**：ChatGPT、Gemini、Grok、Claude、DeepSeek 等模型写的文本，被检测和被"降 AI"的难易程度是否不同？（同理，可以研究不同模型进行回译/同义替换之后的效果

**假设**

| # | 子问题 | 假设 |
| --- | --- | --- |
| H1 | 1 | 只靠提示词让 LLM "写得像人"，绝大多数文本仍被判为 AI。包括只说“写的像人”本身的提示词和“网络上流行的复杂完整提示词” |
| H2 | 1 | 人性化工具对两款检测器的效果差别很大 |
| H3 | 1 | 回译轮数越多，检出率越低，但语义保真度同步下降 |
| H4 | 1 | 零宽字符、同形字符等字符技巧对两款检测器基本无效 |
| H5 | 1 | 人工改写比自动方法更能降低检出率，改动比例越高越有效（人类手动改写很花时间，并且我的经验已经证明了只要花足够多的时间改写是大概率可以实现零AI率的，但是用Ai来写论的人本来就不愿意在这方面花太多精力，因此无限制增加人类改写轮数不一定符合现实情况 |
| H6 | 2 | 检测器主要依据用词和句式，而不是标点、格式、拼写等表层特征 |
| H7 | 2 | 打乱句子顺序后检出率基本不变，即检测器看的是句子内部，不是篇章结构 |
| H8 | 2 | 往人写文本里只加入一种"AI 特征"（如 AI 偏好词、破折号、总结句），不足以让它被判为 AI |
| H9 | 3 | 各模型原文的检出率都接近 100%，但被降 AI 方法降下来的难易程度不同 |
| H10 | 3 | 同一个降 AI 提示词在不同模型上效果不同 |

## 资料：网上流行的降 AI 方法

网上流传的方法可以归为七类。宣称的效果大多来自推广文章，没有可核查的前后检测数据；只有人性化工具有较系统的研究，而且厂商之间的结论互相矛盾。

| 类别 | 网上流传的做法 | 宣称的效果 | 已有证据 |
| --- | --- | --- | --- |
| 1 提示词 | 要求"提高困惑度和突发性"；长短句交替；避开 leverage、robust 等套话；指定阅读难度；加入个人观点；用"学术表达优化顾问"一类系统提示词；每次只改 100–300 字 | 有人称输出"至少 90% 像人写"，未验证 | Russell et al. 用自建的提示词式人性化改写 60 篇文章：GPTZero 检出率 46.7%，Pangram 90.0%，Pangram 人性化专用模型 96.7% |
| 2 回译 | 中英互译；经小语种多轮往返；用 Google、DeepL、百度翻译 | 常被提到，但可读的中文教程都没有给出具体做法和数据 | ESPERANTO（2024）发现回译明显降低研究型检测器的检出率；有用户反映中译英后 ZeroGPT 仍显示 60–80% |
| 3 改写工具 | QuillBot、Grammarly 的改写功能 | — | Pangram 自称对两者的输出准确率均为 100% |
| 4 人性化工具 | Undetectable AI、StealthGPT、WriteHuman、HIX Bypass、Bypass GPT、Humbot、Phrasly、TwainGPT、GPTinf；中文有笔灵 AI 等 | 测评称绕过 GPTZero 82–87%；中文推广文称"AI 率 78% → 0%" | DAMAGE（Pangram 团队，19 款工具）：学术文本上 GPTZero 在 5% 误伤率下的检出率从 99.7% 降到 60.0%。Pangram 自称对 20 款工具准确率 90.3–100%；GPTZero 自称在 12 种以上改写器上检出 93.5%，而 Pangram v3 只有 50.2% |
| 5 人工改写 | 拆长句；替换"首先、其次、最后"等模板连接词；每段加一句批判性观点；加入数据和范围限定；插入括号和破折号；AI 初稿后逐段重写 | 推广文称 69% → 15%（截图无法核实） | Beemo（2025）发现专家修改过的 AI 文本能躲过部分检测器 |
| 6 字符技巧 | 同形字符（如西里尔字母替换拉丁字母）、零宽字符、逗号前加空格 | 部分人性化工具内部就在用 | 2024 年研究显示同形字符能骗过多个研究型检测器，逗号前加空格当时能骗过 GPTZero；对当前版本没有数据 |
| 7 多模型接力与混写 | 一个模型写、另一个模型改；自己写部分段落（自己写部分段落可以删去，太花时间没办法大规模测试 | — | 未找到系统测试 |

**代表性提示词**（实验一直接使用，原文存入研究日志）

- 困惑度与突发性："When it comes to writing content, two factors are crucial, 'perplexity' and 'burstiness.' … I need it to have a good amount of perplexity and burstiness."（[GitHub gist](https://gist.github.com/fmachado/324f351bba11ced334a3bfc00067bf0c)）
- 系统提示词"学术表达优化顾问"：先问领域、目标、风格等五个问题，再按"Alternate between long and short sentences to create natural rhythm"等策略改写（[linux.do](https://linux.do/t/topic/511132)，作者注明未验证）
- 中文教程常见写法："请改写以下句子，使其更加符合学术写作风格，并降低 AIGC 率，同时保证原意不变"

中文人工改写技巧主要来自 [CSDN 实测文](https://damodev.csdn.net/6a424721662f9a54cb85b79e.html) 和 [CSDN 经验总结](https://gitcode.csdn.net/69df4ead54b52172bc69eea7.html)，两者都带推广性质。B 站上的几篇教程无法自动读取，需要你手动补充。人性化工具的研究见 [DAMAGE](https://arxiv.org/abs/2501.03437)、[Pangram 博客](https://www.pangram.com/blog/humanizers-aug-25)、[GPTZero 博客](https://gptzero.me/news/detecting-ai-humanized-text-how-gptzero-stays-ahead/) 和 [Russell et al.](https://arxiv.org/abs/2501.15654)；字符技巧见[同形字符研究](https://arxiv.org/abs/2406.11239)。

## 资料：检测器看什么

厂商只公开了大致原理。独立研究发现，破折号、连接词、格式等表层特征单独就能大致分开人写和 AI 文本，但主流检测器在去掉这些差异后仍有判断力。具体靠哪些特征，没有研究在 GPTZero 和 Pangram 上做过逐项拆解，这正是实验二要补的空白。先去研究这两个工具和其他工具宣传的原理

| 来源 | 说法或发现 |
| --- | --- |
| GPTZero | 早期以困惑度（用词是否在模型意料之中）和突发性（句长、句式的起伏）为核心，现在是多个组件组成的深度学习模型 |
| [Pangram 技术报告](https://arxiv.org/abs/2402.14873) | 直接训练的分类器；训练时让 AI 按同一题目写一篇与人写文本配对，并反复加入最难分辨的例子 |
| [Russell et al., 2025](https://arxiv.org/abs/2501.15654) | 常用 ChatGPT 的人靠 AI 偏好词（如 testament、crucial）、公式化结构、原创性、正式程度来判断；他们多数投票在人性化文本上检出 100%，高于 GPTZero 和 Pangram |
| [SV-Detect, 2026](https://arxiv.org/abs/2606.07313) | 只用破折号、弯引号、省略号、格式、正式连接词、模板痕迹等规则特征的简单分类器，AUROC 约为 0.75–0.9；权重最高的是破折号、正式连接词和提示词泄露模板 |
| [Dutta Chowdhury et al., 2026](https://aclanthology.org/2026.findings-emnlp.1051/) | 表层格式在三个基准上都能强烈区分人写和 AI 文本，各基准学到的模式差别很大；但检测器在此之外仍保留信号 |
| [Kobak et al., 2025](https://arxiv.org/abs/2406.07016) | 用"异常高频词"估计 2024 年至少 13.5% 的 PubMed 摘要经过大模型处理，是实验二"AI 偏好词"词表的来源 |
| [MASH, 2026](https://arxiv.org/abs/2601.08564) | 用检测器分数反馈训练的"风格人性化"模型，对研究型和两款商业检测器的绕过率平均约 90%；需要训练模型，不是学生能用的方法，只作背景 |

对实验设计的启示：表层特征单独有区分力，所以实验二按字符、用词、句式、篇章分层检验；各模型的"标志"不同，所以实验二的原料取自实验三的多个模型。

## 实验一：降 AI 方法实测

把上面七类方法落实为 14 个具体版本，用在实验三生成的 AI 文本上，每个版本都和自己的原文配对比较。

| # | 类别 | 版本 | 样本 |
| --- | --- | --- | --- |
| M1 | 提示词 | 困惑度与突发性提示词，由生成该文的模型自己改写 | 全部 100 篇 |
| M2 | 提示词 | "学术表达优化顾问"系统提示词 | 全部 100 篇 |
| M3 | 提示词 | 中文常见指令（译成英文）：符合学术风格、降低 AI 率、保持原意 | 全部 100 篇 |
| M4 | 回译 | 英 → 中 → 英，1 轮 | 40 篇 |
| M5 | 回译 | 英 → 斯瓦希里语 → 英，3 轮 | 40 篇 |
| M6 | 改写工具 | QuillBot 改写 | 40 篇 |
| M7 | 人性化工具 | Undetectable AI | 40 篇 |
| M8 | 人性化工具 | StealthGPT 或 WriteHuman | 40 篇 |
| M9 | 人工改写 | 按第 5 类技巧清单改写，每篇限时 10 分钟 | 40 篇 |
| M10 | 字符技巧 | 同形字符替换 | 40 篇 |
| M11 | 字符技巧 | 插入零宽字符 | 40 篇 |
| M12 | 字符技巧 | 逗号前加空格 | 40 篇 |
| M13 | 多模型接力 | 另一个模型用 M3 指令改写 | 40 篇 |
| M14 | 组合 | M7 之后再做 M9 | 40 篇 |

- 40 篇从实验三的 100 篇中按模型均匀抽取（每个模型 8 篇）。
- **能不能降**：每个版本记录两款工具的标签和分数，计算仍判 AI、降到 Mixed、降到人写的比例。
- **代价是什么**：语义保真度（句向量相似度）、语法错误数、长度变化；一个版本即使降下来，如果原意已经丢失，实际也没法用。
- **能不能被认出**：记录 Pangram 4 的人性化分数，以及两款工具是否将其标为 AI 辅助或改写。
- **人写对照（选做）**：20 篇人写文本也做 M4、M6、M7，看这些工具会不会反过来让人写文本被判成 AI。

## 实验二：检测器看重什么

每个变体只改变一个特征，其余保持不变，比较改动前后的分数。分三个方向做：

- **去掉**：从 AI 文本里去掉一个"AI 特征"，看分数降不降。
- **加入**：往人写文本里加入同一个特征，看分数升不升。
- **破坏**：打乱某一层信息（句序、词序、用词），看检测器还能不能认出来。

| 层级 | 变体 | AI 文本（去掉） | 人写文本（加入） | 做法 |
| --- | --- | --- | --- | --- |
| 字符 | 破折号、弯引号 | 换成逗号、直引号 | 加入破折号 | 脚本 |
| 字符 | 英式 / 美式拼写 | 互换 | 互换 | 脚本 |
| 用词 | AI 偏好词（delve、crucial、pivotal、underscore 等） | 换成普通同义词 | 替换进去 | 脚本 + 词表 |
| 用词 | 句首连接词（Moreover、Furthermore、Overall） | 删除 | 加入 | 脚本 |
| 用词 | 随机替换实词 10% / 30% / 50% | 是 | — | 脚本，作为上一行的对照 |
| 句式 | 句长起伏 | 拆分、合并句子，拉大长短差异 | 把句长调得均匀 | LLM 限定只改这一项，用对比核对 |
| 句式 | 被动 / 主动语态 | 被动改主动 | 主动改被动 | LLM 限定 + 核对 |
| 篇章 | 开头主题句和结尾总结句 | 删除 | 加入一句 AI 式总结 | 手工 |
| 内容 | 具体细节（数字、例子、个人经历） | 加入 | — | 手工 |
| 内容 | 拼写和语法错误 2 / 5 / 10 处 | 加入 | — | 脚本 |
| 破坏 | 打乱句子顺序 | 是 | 是 | 脚本 |
| 破坏 | 打乱句内词序 | 是 | 是 | 脚本 |
| 长度 | 截取前 50 / 100 / 150 / 200 词 | 是 | 是 | 脚本 |
| 混合 | 人写文本中 1 / 2 / 4 / 8 句换成 AI 句 | — | 是 | 脚本，用同题目的 AI 文本 |

- 每个变体都记录编辑比例。"AI 偏好词替换"和"随机实词替换"在编辑比例相同时对比，就能分清是"改了什么"起作用，还是"改了多少"起作用。
- 原文分数几乎都是 0 或 100，单一特征可能推不动。所以主要看连续分数（GPTZero 的 AI 概率、Pangram 的 AI 占比和逐段结果）；如果仍然不动，就在实验一中被降到 Mixed 的"临界"文本上重做一遍。
- "单一特征推不动分数"本身就是结论：说明检测器综合了多种信号，而不是靠某个"开关"。

## 实验三：不同 LLM 对比

五个模型用同样的 20 个题目、同样的提示词各生成 20 篇，共 100 篇；这批文本同时作为实验一和实验二的原料。

| 模型 | 厂商 | 选它的原因 |
| --- | --- | --- |
| ChatGPT | OpenAI | 学生用得最多 |
| Gemini | Google | Epoch AI 的测试里，Gemini 模仿真实作者风格的科学写作是最常躲过检测的一类 |
| Grok | xAI | 独立测试数据很少 |
| Claude | Anthropic | 主流模型之一 |
| DeepSeek | 深度求索 | 中文学生常用 |
| 开源模型（如 Qwen 或 Llama，选做） | — | 看结论能不能推广到较少见的模型 |

- **生成**：用各家当前的主力模型和默认设置，记录模型名、版本和日期。题目取自 BAWE，每篇生成整篇后截取约 300 词，和第一阶段的处理方式一致。
- **原文对比**：同一题目下各模型配对比较检出率和分数。
- **降 AI 对比**：每个模型用同一套降 AI 提示词改写自己的文章（H10）；再加一种"多模型接力"：一个模型写、另一个模型改。
- **结论的形式**：模型 × 降 AI 方法的检出率表，看是否某个模型或某种组合特别容易躲过检测。

## 指标、样本量与预算

| 实验 | 主要指标 | 分析 | 每款工具检测次数 |
| --- | --- | --- | --- |
| 三：多模型基线 | 各模型的检出率和分数 | 模型间比较（Fisher / Kruskal-Wallis 检验） | 100 |
| 一：降 AI 方法 | 仍判 AI / Mixed / 人写的比例；分数变化；语义保真度、语法错误、长度变化；人性化识别率 | 配对比较（McNemar、Wilcoxon 检验）；检出率—保真度散点图；模型 × 方法的逻辑回归 | 740（人写对照 +60） |
| 二：控制变量 | 每个特征变体的分数变化和效应量；替换比例、错误数、长度、AI 句数的剂量曲线 | 配对 Wilcoxon 检验；按效应量给特征排序 | 760（AI 20 篇 × 21 个变体 + 人写 20 篇 × 17 个变体） |
| 合计 |  |  | 约 1,600 |

- 基线接近 100%，比例用 Clopper-Pearson 区间；主要看配对的分数变化，而不只是标签是否翻转。
- **检测费用**：约 45 万词，Pangram 按每 100 词 0.05 美元约 225 美元；GPTZero 按套餐词数额度使用。
- **其他费用**：两款人性化工具各订阅一个月（测评中报价约每月 10–25 美元）；QuillBot 免费版可能有字数限制；五个模型的生成和改写费用很低。
- 预算紧张时优先缩小实验二：去掉剂量曲线中间档，可省约 200 次。

## 需要你确认的事项

- [ ] 模型清单：ChatGPT、Gemini、Grok、Claude、DeepSeek 五个是否合适，是否加一个开源模型。合适。
- [ ] 生成方式：用 API 默认参数（版本可控、省时），还是用网页或 App（更接近学生的实际用法）。待定。
- [ ] 人性化工具选哪两款，是否愿意各付费订阅一个月。待定。
- [ ] 人工改写（M9）由谁来做，每篇 10 分钟是否可行。人工改写不现实。
- [ ] 题目继续用 BAWE 的，还是换成你们课程的作业题目。题目已改变，我有备用资源。
- [ ] 第一阶段的实测结果作为报告里的"基线"一节
- [ ] 你的 API 返回里是否有 Pangram 的人性化分数、GPTZero 的改写类别
- [ ] B 站的降 AI 教程无法自动读取，如果要补充，请提供链接或截图

## 参考资料

**降 AI 方法与人性化工具**

- [Masrour, Emi & Spero (2025) DAMAGE: Detecting Adversarially Modified AI Generated Text](https://arxiv.org/abs/2501.03437)
- [Pangram (2025) How well does Pangram perform on humanizers?](https://www.pangram.com/blog/humanizers-aug-25)
- [GPTZero (2026) Detecting AI-Humanized Text: How GPTZero Stays Ahead](https://gptzero.me/news/detecting-ai-humanized-text-how-gptzero-stays-ahead/)
- [Russell et al. (2025) People who frequently use ChatGPT for writing tasks are accurate and robust detectors of AI-generated text](https://arxiv.org/abs/2501.15654)
- [Ayoobi et al. (2024) ESPERANTO: back-translation and AI detection](https://arxiv.org/abs/2409.14285)
- [Artemova et al. (2025) Beemo: Benchmark of Expert-edited Machine-generated Outputs](https://arxiv.org/abs/2411.04032)
- [Creo & Pudasaini (2024) Evading AI-Generated Content Detectors using Homoglyphs](https://arxiv.org/abs/2406.11239)
- [Gu, Li & Hu (2026) MASH: Evading Black-Box AI-Generated Text Detectors via Style Humanization](https://arxiv.org/abs/2601.08564)
- [prompt-write-like-a-human（GitHub gist）](https://gist.github.com/fmachado/324f351bba11ced334a3bfc00067bf0c)
- [AI 润色论文，降低 AIGC 率提示词（linux.do）](https://linux.do/t/topic/511132)
- [实测！论文 AI 率 78%→0% 的破解秘籍（CSDN，推广文）](https://damodev.csdn.net/6a424721662f9a54cb85b79e.html)
- [如何有效降低论文 AIGC 率？（CSDN）](https://gitcode.csdn.net/69df4ead54b52172bc69eea7.html)

**检测器原理与特征**

- [Pangram 技术报告](https://arxiv.org/abs/2402.14873)
- [Pangram 4 模型说明](https://www.pangram.com/research/model-card/pangram-4)
- [Vishnyakov & Gaintseva (2026) SV-Detect: AI-generated Text Detection with Steering Vectors](https://arxiv.org/abs/2606.07313)
- [Dutta Chowdhury et al. (2026) Formatting and Surface Feature Confounds in AI Text Detection: A Clever Hans Effect](https://aclanthology.org/2026.findings-emnlp.1051/)
- [Kobak et al. (2025) Delving into LLM-assisted writing in biomedical publications through excess vocabulary](https://arxiv.org/abs/2406.07016)

**多模型与基线**

- [Epoch AI (2026) AI detectors: false negatives](https://epoch.ai/data-insights/ai-detectors-false-negatives)
- [Jabarian & Imas (2025) Artificial Writing and Automated Detection](https://bfi.uchicago.edu/wp-content/uploads/2025/09/BFI_WP_2025-116.pdf)
- [第二版研究计划](https://claude.ai/artifact/7esRHubRzHrT6wZhSu4ocd)
