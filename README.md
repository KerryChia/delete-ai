# quaiwei · 中文去 AI 味编辑 Skill

**去 AI 味，而不是降 AI 率。** 一个用于 AI 编码助手的中文编辑 skill：对文本做三层诊断、在事实保护下做最小必要改写、缺材料时追问作者，而不是一台禁词替换机。

A Chinese editing skill for AI coding agents: diagnose before rewriting, protect facts, ask the author when material is missing — instead of running a blacklist of "AI-sounding" words. (中文为主，见下文。)

## 它做什么，不做什么

**做：**

- **三层诊断**：风格信号（重复、空泛、套话、机械节奏）→ 情境不匹配（口吻与读者/体裁不符、术语与标点不统一）→ 判断与材料缺口（无依据的断言、无来源的引用、缺取舍）。
- **四种范围模式**：只诊断 / 最小改写（默认）/ 保结构改写 / 自由改写，边界清晰。
- **事实保护**：数字、专名、引语、术语、否定、情态、因果方向、范围、结论强度、时态——触碰任何一项必须进"待确认清单"，不得静默修改。
- **"问"与"留"**：缺材料时向作者提问并给不编造的保守版本；看起来像套路但实际有功能的表达如实保留。
- **改后自检**：逐句对照原文 + 五维定性检查（直/实/变/散/真，不打分）+ 通读。

**不承诺：**

- 不判断文本是否 AI 所写，不针对任何 AI 检测器优化，输出中没有"AI 率""检测分数"。
- 不靠虚构亲身经历、编造数据来源、堆砌语气词来制造"人味"。
- 不把学术、公文、法律文本改成聊天体；"无需修改"是正常结论。

## 为什么不是又一个禁词表

开源社区已有不少优秀的去 AI 味 skill，它们共同的短板是：规则清单越长，误伤越多——把真实纠错、有意修辞、专业表达一起删掉。quaiwei 的设计针对这一点：

| | quaiwei | 典型规则清单式 skill |
|---|---|---|
| 规则命中后 | 语义判断决定删/改/问/留 | 通常直接改写 |
| 缺材料的抽象断言 | 追问作者，给保守版本 | 补细节（往往编造） |
| 事实与情态保护 | 十项清单 + 待确认机制 | 部分项目有，粒度不一 |
| "不是……而是" | 四分类判断 | 多数一刀切 |
| 编辑范围 | 四种模式，删除许可白名单 | 常无边界定义 |
| 评分 | 不打分（避免伪测量） | 部分用自评分强制重写 |

核心立场：**"AI 味"是阅读体验问题，不是作者身份问题。** 文本可以接受 AI 辅助，同时做到清晰、具体、有判断。

## 安装

把本仓库（或 `quaiwei/` 文件夹）复制到你的 agent 技能目录：

```bash
# ZCode / Claude Code / Codex 等支持 skill 目录的 agent
git clone https://github.com/KerryChia/delete-ai.git
cp -r delete-ai ~/.zcode/skills/quaiwei     # 或 ~/.claude/skills/quaiwei ~/.codex/skills/quaiwei
```

也可以直接把 `SKILL.md` 与 `references/` 的内容当作系统提示词使用——但按需加载 references 的完整体验需要 agent 的 skill 机制。

## 使用示例

输入（用户）：

> 帮我看看这段，去一下 AI 味：在信息爆炸的时代，效率工具的选择成为了每个人无法回避的重要课题。选择效率工具不是看功能多少，而是看它能否真正融入你的工作流。研究表明，使用合适工具的人工作效率能显著提升30%以上。

输出要点（skill 的实际行为）：

- 万能时代开头 → 删（R15）
- "不是看功能多少，而是看……" → **留**：有真实对比，是作者核心主张（R01 四分类之"有效对比"）
- "研究表明……30%" → **问**：来源？数字一字不动（R13/R14 + 事实保护）
- 学术句若与全文文体混杂 → 层 2 提示，请作者确认用途（R31）

## 目录结构

```text
quaiwei/
├── SKILL.md              # 红线、优先级、工作流程、输出格式
└── references/
    ├── rules.md          # 50 条规则、"不是而是"四分类、候选信号词表
    └── examples.md       # 13 组改写示例 + 反面清单
```

## 已知局限（诚实声明）

- 规则是**编辑启发式**，未经大规模用户盲测验证；欢迎用真实稿件测试并提 issue。
- 改写质量上限取决于执行它的底层模型；建议搭配中文能力强的模型。
- 本项目不能也不应用于规避 AIGC 检测、平台审核或学术诚信要求；涉及 AI 使用披露的场景请遵守相应规定。

## 评测建议

拿 5–10 篇真实草稿，比较：建议采纳率、误改率（本来干净的句子被改坏）、事实保真（数字/情态/引用是否原样）。本 skill 的验收标准是**读者和作者是否更愿意采用修改稿**，不是任何检测分数。

## 致谢

本项目的方法论融合自以下 MIT 许可的开源项目（E 节规则与词表有其贡献，均已注明）：

- [op7418/Humanizer-zh](https://github.com/op7418/Humanizer-zh) — 冲突优先级、31 检查点、交付前核对
- [MrGeDiao/shuorenhua](https://github.com/MrGeDiao/shuorenhua) — 范围模式、删除许可、"不删归属"原则
- [VincentOld/stop-slop-zh](https://github.com/VincentOld/stop-slop-zh) — 候选信号词表、名词化对照、场景边界
- [ai-zixun/humanizer-zh](https://github.com/ai-zixun/humanizer-zh) — 排版腔/判断腔分类、朗读检查
- [blader/humanizer](https://github.com/blader/humanizer) — "为读者编辑，不为检测器编辑"的定位
- [alchaincyf/nuwa-skill](https://github.com/alchaincyf/nuwa-skill) — 作者声音/反模式提取方法

观念参考（仅思想，未复制文本）：Wikipedia *Signs of AI writing*（CC BY-SA 4.0）。

## License

[MIT](LICENSE)
