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
3. **不同 LLM 有没有差别**：ChatGPT、Gemini、Grok、Claude、DeepSeek 等模型写的文本，被检测和被"降 AI"的难易程度是否不同？用不同模型做回译和同义替换，效果是否不同？

**假设**

| # | 子问题 | 假设 |
| --- | --- | --- |
| H1 | 1 | 只靠提示词让 LLM "写得像人"，绝大多数文本仍被判为 AI，无论是只说"写得像人"的简单提示词，还是网上流行的复杂提示词 |
| H2 | 1 | 人性化工具对两款检测器的效果差别很大 |
| H3 | 1 | 回译轮数越多，检出率越低，但语义保真度同步下降 |
| H4 | 1 | 零宽字符、同形字符等字符技巧对两款检测器基本无效 |
| H5 | 1 | 网上流传的人工改写技巧中可以机械执行的部分（替换模板连接词、拆分长句、加入括号和破折号），单独使用时基本降不了检出率 |
| H6 | 2 | 检测器主要依据用词和句式，而不是标点、格式、拼写等表层特征 |
| H7 | 2 | 打乱句子顺序后检出率基本不变，即检测器看的是句子内部，不是篇章结构 |
| H8 | 2 | 往人写文本里只加入一种"AI 特征"（如 AI 偏好词、破折号、总结句），不足以让它被判为 AI |
| H9 | 3 | 各模型原文的检出率都接近 100%，但被降 AI 方法降下来的难易程度不同 |
| H10 | 3 | 同一个降 AI 提示词，或同一种回译、同义替换，由不同模型执行时效果不同 |

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
| 7 多模型接力 | 一个模型写、另一个模型改 | — | 未找到系统测试 |

**代表性提示词**（实验一直接使用，原文存入研究日志）

- 困惑度与突发性："When it comes to writing content, two factors are crucial, 'perplexity' and 'burstiness.' … I need it to have a good amount of perplexity and burstiness."（[GitHub gist](https://gist.github.com/fmachado/324f351bba11ced334a3bfc00067bf0c)）
- 系统提示词"学术表达优化顾问"：先问领域、目标、风格等五个问题，再按"Alternate between long and short sentences to create natural rhythm"等策略改写（[linux.do](https://linux.do/t/topic/511132)，作者注明未验证）
- 中文教程常见写法："请改写以下句子，使其更加符合学术写作风格，并降低 AIGC 率，同时保证原意不变"

中文人工改写技巧主要来自 [CSDN 实测文](https://damodev.csdn.net/6a424721662f9a54cb85b79e.html) 和 [CSDN 经验总结](https://gitcode.csdn.net/69df4ead54b52172bc69eea7.html)，两者都带推广性质。B 站上的几篇教程无法自动读取，需要你手动补充。人性化工具的研究见 [DAMAGE](https://arxiv.org/abs/2501.03437)、[Pangram 博客](https://www.pangram.com/blog/humanizers-aug-25)、[GPTZero 博客](https://gptzero.me/news/detecting-ai-humanized-text-how-gptzero-stays-ahead/) 和 [Russell et al.](https://arxiv.org/abs/2501.15654)；字符技巧见[同形字符研究](https://arxiv.org/abs/2406.11239)。

## 资料：检测器看什么

各家公开的程度差别很大：Pangram 公开最多，Turnitin、Copyleaks、知网几乎不公开。两款主要工具都已经不是"困惑度 + 突发性"这类简单统计，而是训练出来的深度模型，并且都专门针对改写和人性化做了训练。下表全是厂商自己的说法。

**厂商宣传的原理**

| 工具 | 厂商的说法 | 公开程度 |
| --- | --- | --- |
| [GPTZero](https://arxiv.org/abs/2602.13042) | 2023 年秋起改为深度学习模型，不再以困惑度和突发性为判断依据，两者只是解释结果的"七个指标"之一；2026 年技术报告称 GPTZero 5.0 是"分层、多任务"架构，并用自动化红队测试提高对改写的抵抗力；逐句给出概率 | 中 |
| [Pangram](https://www.pangram.com/research/model-card/pangram-4) | 2024 年报告：Transformer 分类器，用"合成镜像"（让 AI 按同一题目写一篇与人写配对）和难例挖掘训练。Pangram 4（2026 年 7 月）：以开源权重的混合专家语言模型为主干，四个输出头（片段 15 类、逐词 3 类、是否混合作者、人性化 4 类）；按 512 token 的重叠窗口判断，再连成片段，最短片段约两句；人性化判定阈值 0.91；输入至少 50 词；称不用用户提交的文本训练 | 高 |
| [Turnitin](https://www.turnitin.ca/blog/ai-paraphrasing-detection-strengthening-the-integrity-of-academic-writing) | 2024 年 7 月起加入"AI 改写检测"，识别被改写工具处理过的 AI 文本，用不同颜色标出"AI 写"和"AI 改写"片段；架构白皮书无法读取 | 低 |
| [Originality.ai](https://originality.ai/blog/how-does-ai-content-detection-work) | 自研、类似 ELECTRA 的模型（160 GB 文本预训练），再用数百万样本微调；整篇分析加单独的逐句高亮模型；训练用的 AI 文本按不同温度和采样方式生成 | 中 |
| [ZeroGPT](https://www.zerogpt.com/deep-analyse) | "DeepAnalyse" 多阶段方法：整体 → 句子 → 细节 → 综合；逐句高亮；训练数据包括网络文本、教育数据和多种模型生成的合成数据 | 低 |
| Copyleaks | 官网无法读取；第三方描述为学习人写文本的模式，偏离时报警 | 低 |
| [知网 AIGC 检测](https://chn.oversea.cnki.net/nindex/second/viewmore/AIGC.html) | "对比语言模式和语义逻辑区别"，结合"知识增强大模型技术"和知网文献资源 | 低 |

**独立研究的发现**

表层特征（破折号、连接词、格式）单独就能大致分开人写和 AI 文本，但主流检测器在去掉这些差异后仍有判断力。没有研究在 GPTZero 和 Pangram 上做过逐项拆解，这正是实验二要补的空白。

| 来源 | 发现 |
| --- | --- |
| [Russell et al., 2025](https://arxiv.org/abs/2501.15654) | 常用 ChatGPT 的人靠 AI 偏好词（如 testament、crucial）、公式化结构、原创性、正式程度来判断；他们多数投票在人性化文本上检出 100%，高于 GPTZero 和 Pangram |
| [SV-Detect, 2026](https://arxiv.org/abs/2606.07313) | 只用破折号、弯引号、省略号、格式、正式连接词、模板痕迹等规则特征的简单分类器，AUROC 约为 0.75–0.9；权重最高的是破折号、正式连接词和提示词泄露模板 |
| [Dutta Chowdhury et al., 2026](https://aclanthology.org/2026.findings-emnlp.1051/) | 表层格式在三个基准上都能强烈区分人写和 AI 文本，各基准学到的模式差别很大；但检测器在此之外仍保留信号 |
| [Kobak et al., 2025](https://arxiv.org/abs/2406.07016) | 用"异常高频词"估计 2024 年至少 13.5% 的 PubMed 摘要经过大模型处理，是实验二"AI 偏好词"词表的来源 |
| [MASH, 2026](https://arxiv.org/abs/2601.08564) | 用检测器分数反馈训练的"风格人性化"模型，对研究型和两款商业检测器的绕过率平均约 90%；需要训练模型，不是学生能用的方法，只作背景 |

**厂商说法里可以直接检验的点**

| 厂商说法 | 用哪个实验检验 |
| --- | --- |
| GPTZero：困惑度和突发性只是辅助指标 | 实验二"句长起伏"变体：如果几乎不改变 GPTZero 的分数，就印证了这一点（H6） |
| Pangram：最短片段约两句 | 实验二"混合"变体（人写文本中换入 1 / 2 / 4 / 8 句 AI 句）：1 句是否被漏掉，2 句以上是否被标出 |
| Pangram：输入至少 50 词，按 512 token 窗口判断 | 实验二长度变体（50 / 100 / 150 / 200 词） |
| Pangram 4 的人性化检测、GPTZero 的改写红队测试、Turnitin 的 AI 改写检测 | 实验一 M8–M11 的"能不能被认出" |
| GPTZero 和 ZeroGPT 都逐句高亮 | 实验二记录逐句结果，看被高亮的句子是否集中在含 AI 偏好词、连接词的句子 |

对实验设计的启示：表层特征单独有区分力，所以实验二按字符、用词、句式、篇章分层检验；各模型的"标志"不同，所以实验二的原料取自实验三的多个模型。

## 实验一：降 AI 方法实测

把上面的方法落实为 15 个具体版本，用在实验三生成的 AI 文本上，每个版本都和自己的原文配对比较。

| # | 类别 | 版本 | 样本 |
| --- | --- | --- | --- |
| M1 | 提示词 | 简单提示词：只要求"改写得像人写的" | 全部 100 篇 |
| M2 | 提示词 | 困惑度与突发性提示词 | 全部 100 篇 |
| M3 | 提示词 | "学术表达优化顾问"系统提示词 | 全部 100 篇 |
| M4 | 提示词 | 中文常见指令（译成英文）：符合学术风格、降低 AI 率、保持原意 | 全部 100 篇 |
| M5 | 回译（机器翻译） | Google 翻译，英 → 中 → 英，1 轮 | 40 篇 |
| M6 | 回译（机器翻译） | Google 翻译，英 → 斯瓦希里语 → 英，3 轮 | 40 篇 |
| M7 | 回译（LLM） | 五个模型分别做英 → 中 → 英 | 20 篇 × 5 个模型 |
| M8 | 同义替换（LLM） | 五个模型分别做同义替换，要求不改句式和意思 | 20 篇 × 5 个模型 |
| M9 | 改写工具 | QuillBot 改写 | 40 篇 |
| M10 | 人性化工具 | Undetectable AI | 40 篇 |
| M11 | 人性化工具 | StealthGPT 或 WriteHuman | 40 篇 |
| M12 | 机械改写 | 用脚本执行第 5 类中可机械操作的技巧：替换模板连接词、拆分长句、加入括号和破折号 | 40 篇 |
| M13 | 字符技巧 | 同形字符替换 | 40 篇 |
| M14 | 字符技巧 | 插入零宽字符 | 40 篇 |
| M15 | 字符技巧 | 逗号前加空格 | 40 篇 |

- 40 篇从实验三的 100 篇中按模型均匀抽取（每个模型 8 篇）。M1–M4 由生成该文的模型自己改写。
- M7、M8 用其中 20 篇（每个模型 4 篇），每篇由五个模型分别处理，得到"原文模型 × 改写模型"的 5 × 5 矩阵：对角线是模型改自己的文章，其余格子就是多模型接力。和 M5、M6 对比，还能看出 LLM 回译和机器翻译回译的差别。
- 不测完整的人工改写。研究者的经验是，只要花足够时间，人工改写大概率能做到零 AI 率；但用 AI 写作的学生通常不愿意在这上面花太多精力，无限增加人工改写的轮数不符合实际。所以只用 M12 测试网上人工技巧中可以机械执行的部分。
- **能不能降**：每个版本记录两款工具的标签和分数，计算仍判 AI、降到 Mixed、降到人写的比例。
- **代价是什么**：语义保真度（句向量相似度）、语法错误数、长度变化；一个版本即使降下来，如果原意已经丢失，实际也没法用。
- **能不能被认出**：记录 Pangram 4 的人性化分数，以及两款工具是否将其标为 AI 辅助或改写。
- **人写对照（选做）**：20 篇人写文本也做 M5、M9、M10，看这些工具会不会反过来让人写文本被判成 AI。

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
| 篇章 | 开头主题句和结尾总结句 | 删除 | 加入一句 AI 式总结 | LLM 限定只改这一项 + 核对 |
| 内容 | 具体细节（数字、例子、个人经历） | 加入 | — | LLM 限定只改这一项 + 核对 |
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

- **生成**：用各家当前的主力模型和默认设置，记录模型名、版本和日期。题目使用新准备的题目集，每篇生成整篇后截取约 300 词，和第一阶段的处理方式一致。
- **原文对比**：同一题目下各模型配对比较检出率和分数。
- **降 AI 对比**：每个模型用同一套降 AI 提示词改写自己的文章（H10）；回译和同义替换也由五个模型分别执行，得到原文模型 × 改写模型矩阵（实验一 M7、M8）。
- **结论的形式**：模型 × 降 AI 方法的检出率表，看是否某个模型或某种组合特别容易躲过检测。

## 指标、样本量与预算

| 实验 | 主要指标 | 分析 | 每款工具检测次数 |
| --- | --- | --- | --- |
| 三：多模型基线 | 各模型的检出率和分数 | 模型间比较（Fisher / Kruskal-Wallis 检验） | 100 |
| 一：降 AI 方法 | 仍判 AI / Mixed / 人写的比例；分数变化；语义保真度、语法错误、长度变化；人性化识别率 | 配对比较（McNemar、Wilcoxon 检验）；检出率—保真度散点图；模型 × 方法的逻辑回归；原文模型 × 改写模型矩阵 | 960（人写对照 +60） |
| 二：控制变量 | 每个特征变体的分数变化和效应量；替换比例、错误数、长度、AI 句数的剂量曲线 | 配对 Wilcoxon 检验；按效应量给特征排序 | 760（AI 20 篇 × 21 个变体 + 人写 20 篇 × 17 个变体） |
| 合计 |  |  | 约 1,820 |

- 基线接近 100%，比例用 Clopper-Pearson 区间；主要看配对的分数变化，而不只是标签是否翻转。
- **检测费用**：约 52 万词，Pangram 按每 100 词 0.05 美元约 260 美元；GPTZero 按套餐词数额度使用。
- **其他费用**：两款人性化工具各订阅一个月（测评中报价约每月 10–25 美元）；QuillBot 免费版可能有字数限制；五个模型的生成和改写费用很低。
- 预算紧张时优先缩小实验二：去掉剂量曲线中间档，可省约 200 次。

## 需要你确认的事项

- [ ] 模型清单：ChatGPT、Gemini、Grok、Claude、DeepSeek 五个是否合适，是否加一个开源模型。合适。
- [ ] 生成方式：用 API 默认参数（版本可控、省时），还是用网页或 App（更接近学生的实际用法）。待定。
- [ ] 人性化工具选哪两款，是否愿意各付费订阅一个月。待定。
- [ ] 人工改写由谁来做，每篇 10 分钟是否可行。人工改写不现实。
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

- [Adam et al. (2026) GPTZero: Robust Detection of LLM-Generated Texts](https://arxiv.org/abs/2602.13042)
- [GPTZero: Perplexity and burstiness, what is it?](https://gptzero.me/news/perplexity-and-burstiness-what-is-it/)
- [GPTZero: How do I interpret burstiness or perplexity?](https://support.gptzero.me/articles/9585228410-how-do-i-interpret-burstiness-or-perplexity)
- [Emi & Spero (2024) Technical Report on the Pangram AI-Generated Text Classifier](https://arxiv.org/abs/2402.14873)
- [Pangram 4 模型说明](https://www.pangram.com/research/model-card/pangram-4)
- [Turnitin (2024) AI paraphrasing detection](https://www.turnitin.ca/blog/ai-paraphrasing-detection-strengthening-the-integrity-of-academic-writing)
- [Originality.ai: How does AI content detection work?](https://originality.ai/blog/how-does-ai-content-detection-work)
- [ZeroGPT: DeepAnalyse](https://www.zerogpt.com/deep-analyse)
- [知网 AIGC 检测服务](https://chn.oversea.cnki.net/nindex/second/viewmore/AIGC.html)
- [Vishnyakov & Gaintseva (2026) SV-Detect: AI-generated Text Detection with Steering Vectors](https://arxiv.org/abs/2606.07313)
- [Dutta Chowdhury et al. (2026) Formatting and Surface Feature Confounds in AI Text Detection: A Clever Hans Effect](https://aclanthology.org/2026.findings-emnlp.1051/)
- [Kobak et al. (2025) Delving into LLM-assisted writing in biomedical publications through excess vocabulary](https://arxiv.org/abs/2406.07016)

**多模型与基线**

- [Epoch AI (2026) AI detectors: false negatives](https://epoch.ai/data-insights/ai-detectors-false-negatives)
- [Jabarian & Imas (2025) Artificial Writing and Automated Detection](https://bfi.uchicago.edu/wp-content/uploads/2025/09/BFI_WP_2025-116.pdf)
- [第二版研究计划](https://claude.ai/artifact/7esRHubRzHrT6wZhSu4ocd)
