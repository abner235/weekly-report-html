# weekly-report-html

> 一套团队共享的「一页纸周报」HTML 渲染规范 Skill。
> 把任意业务线的本周数字，渲染成风格统一、结论先行、可打印成一页 A4 的经营周报。

每个人安装后，**稍改数据与主色即可生成自己业务线的周报**——所有团队的周报长得一致、克制、专业。顶部是「一句话结论 + 三支柱」叙事，下接「KPI 计分卡 + 趋势/结构图 + 明细/风险表」证据，以「下周三步走」收尾。

---

## 🤖 如果你是 AI 助手（用户把本仓库地址贴给了你）

请按下面的协议自助上手，**不要让用户手动搬运代码**：

1. **读规范**：读完本 README，再读 `SKILL.md`、`references/design-spec.md`、`references/data-schema.md`。`SKILL.md` 是操作手册，`design-spec.md` 是不可随意发挥的设计契约。
2. **克隆代码**：
   ```bash
   git clone <这个仓库的地址> weekly-report-html && cd weekly-report-html
   ```
3. **引导配置**：按 `references/data-schema.md` 的清单，**访谈用户**收集本周数据（或让 ta 填 `templates/data.example.json`）。关键追问：
   - 你的团队/业务线叫什么？报告周期？数据口径有什么注意点？
   - 本周**一句话结论**是什么？——务必带上「但是」（张力所在）。
   - 本周的**赢点 / 最大风险 / 值得加注的方向**各是什么（各一个核心数字）？
   - 4–6 个 **KPI**（值、单位、环比）；一条**趋势**、一组**结构占比**；一张**分区/分段表**、一张**风险表**；2–4 条**下周行动**（含责任人、截止）。
4. **生成报告**：复制 `templates/report-template.html`，按注释里的 `🔧 替换` 标记逐段替换为用户数据；按 spec 重算 SVG 图表坐标；只改 `--accent` 一个变量即可换主色。
5. **自检交付**：对照 `SKILL.md` 的「Quality checklist」逐条确认，再把产物路径和「浏览器打开 / ⌘P 导 PDF」的用法告诉用户。

> 一句话给 AI 的指令模板，用户可以直接对你说：
> *"读这个仓库的 README 和 SKILL.md，clone 下来，然后引导我用它生成 XX 业务线本周的周报。"*

---

## 👤 如果你是人类用户

**方式 A：当作 Skill 安装（推荐）**
本仓库就是一个标准 Agent Skill（`SKILL.md` + `references/` + `templates/`），可直接被支持 Agent Skills 的工具安装：

- **Kiro IDE**：
  - 面板导入：打开 `Agent Steering & Skills` 面板 → 点 `+` → 选「Local folder import」指向本文件夹，或贴本仓库的 GitHub URL。
  - 或手动放置：全局 `~/.kiro/skills/weekly-report-html/`，或工作区 `.kiro/skills/weekly-report-html/`（工作区优先）。
  - 用法：在对话里 `/weekly-report-html` 手动触发，或直接说「做本周 XX 业务线的周报」由描述自动激活。
- **Claude Code**：放进 `~/.claude/skills/weekly-report-html/` 即可。

安装后直接对 AI 说「帮我做本周 XX 业务线的周报」，Skill 会按描述自动触发。

**方式 B：直接改模板**
1. 浏览器打开 `templates/report-template.html` —— 这就是成品样子（自带示例数据）。
2. 按文件内 `🔧 替换` 注释改成你的数据。
3. 想换主色：改 CSS 顶部 `:root { --accent: ... }` 一个值即可。
4. ⌘P → 另存为 PDF，得到一页纸。

**方式 C：把仓库地址贴给 AI**，让它按上面的「🤖 AI 协议」帮你跑完。

---

## 仓库结构

```
weekly-report-html/
├── README.md                    # 你在读的这份：上手与协议
├── SKILL.md                     # 操作手册：生成流程 + 核心规则摘要 + 自检清单
├── references/
│   ├── design-spec.md           # 设计契约：风格 / 配色 / 涨跌符号 / 图表选择 / 排版 / 打印 / 可访问性 / 反模式
│   └── data-schema.md           # 数据契约：字段清单 + 访谈提纲
└── templates/
    ├── report-template.html     # 可改肤模板，开箱即渲染
    └── data.example.json        # 示例数据
```

## 设计要点速记（完整版见 design-spec.md）

| 维度 | 约定 |
|---|---|
| 结构 | 结论先行 → 证据在后 → 行动收尾，一页纸 |
| 配色 | 中性灰 + **唯一主色**（默认深青 `#0f766e`）；正向=主色/绿，负向=暖橙红 `#c2410c` |
| 涨跌符号 | `▲/▼` 表数字方向、颜色表好坏；**两者冲突时（如成本上升）去掉箭头**，只留颜色 |
| 数字 | 全部 `tabular-nums` 对齐；符号与数值同写（`+12.3%`）；单位统一 |
| 图表 | 趋势→折线、迷你趋势→火花柱、占比→环图(≤5段)、明细→表格、状态→信号灯 pill；禁多片饼图/双轴/3D |
| 打印 | 必带 `@media print`（保留底色、防卡片跨页截断、A4 横向） |
| 可访问性 | 文字对比度 ≥4.5:1（含小号 pill）；标题层级单调 `h1→h2→h3`；状态不靠颜色单独表达 |

## 换肤：改一个变量

```css
:root{
  --accent: #0f766e;   /* ← 改成你团队的主色，全图表、强调、行动按钮随之统一 */
}
```
负向色 `--bad`、状态 pill 的配色已按对比度调过，**不建议改**；如要换，对照 spec 的「可访问性」一节复核对比度。

## 来源与许可

本规范由一份 CEO 一页纸经营分析报告（DEMO 6）提炼而来，并经过一轮多维对抗式审查加固（口径一致性、SVG 几何、CSS 渲染、视觉连贯、打印与对比度）。内部团队可自由复制、改肤、二次分发。
