# Visual Design Studio

**通用视觉设计与 AI 图像创作工作流** · Skill：`visual-design-workflow`

一套按任务选择方法的视觉创作技能，覆盖海报、信息图、原创 IP、视觉系统、AI 图像生成与编辑。不限主题、画风、图像服务或设计软件；它提供工作方法，不自带图像模型、API 账号或付费额度。

## 模块

| 模块 | 内容 |
|---|---|
| A | 信息架构、视觉层级、构图、中文排版、素材拆分、封面与海报 |
| B | 历史文化、建筑解析、数据图、时间轴、流程、关系与爆炸图，含事实来源管理 |
| C | 原创角色、服饰与性格、三视图、表情、动作、立绘和多角色一致性 |
| D | 色彩与字体规范、组件、系列母版、主题可迁移模板与素材库 |
| E | 文生图、图像参考、局部替换、扩图、融合、透明与无文字素材，按真实工具能力执行 |

核心工作流：需求与输出确认 → 参考分类 → 视觉规范 → 版式/资产规划 → 按需生成编辑 → 一致性检查 → 最终导出。复合任务叠加模块，不重复流程。

## 生成前询问与长对话记录

系列创作优先采用用户母版：查看母版、提取保留项与允许变化，再延展作品。没有母版时先询问未明确的要求；用户也不知道方向时，生成一组有差异的候选模板预览（默认建议三种）供选择，选定后登记为母版，再制作完整系列。已知要求不重问；候选出图同样先核对生成与保存约定。不能出图时明确提供的是方案，不冒充实际生成结果。

首次出图前核对用途、风格、尺寸、数量、参考图角色、保留项，以及存放目录、格式、透明背景、命名和可编辑文件。只询问尚缺的最重要一项；已明确或授权默认的内容不重复问。设置未确认前可分析和规划，暂不调用依赖它的生成工具。后续沿用记录，变化时重新核对。

每次本技能负责的对话都读取项目状态和视觉规范，对照最新要求，结束时记录变化。角色表与系列清单跟踪身份、母版和局部覆盖，减少长对话遗忘。宿主必须加载技能并能读取记录；新会话未加载技能时不能保证自动生效。

## 本地安装

本项目目录本身就是技能根目录，`SKILL.md` 位于第一层。完整复制该目录到宿主识别的技能目录，保留 agents、references 和 assets；不要只复制 SKILL.md。本地存放不等于安装完成。

Codex 可放在 `~/.codex/skills/visual-design-workflow/SKILL.md`；设置了 `CODEX_HOME` 时使用其实际 `skills` 目录。支持跨宿主技能目录的环境也可按宿主文档选择 `~/.agents/skills`，选择一种范围以免重复。重新加载宿主后查看技能列表。

PowerShell 安装示例（先检查目标不存在；已有同名技能时先比较、备份，不直接覆盖）：

```powershell
$skillSource = (Resolve-Path '.\visual-design-workflow').Path
$skillDestination = Join-Path $env:USERPROFILE '.codex\skills\visual-design-workflow'
if (Test-Path -LiteralPath $skillDestination) {
    throw '目标已存在，请先比较或备份。'
}
Copy-Item -LiteralPath $skillSource -Destination $skillDestination -Recurse
```

显式调用：

```text
使用 $visual-design-workflow 设计三张文化活动系列海报。
第一张已认可，保留网格与字体。生成前核对保存目录和格式。
```

自动触发默认开启；是否自动选中仍由宿主决定。调试时优先显式调用，不把本地模拟当作宿主触发成功。

## 使用案例

- “制作古建筑结构信息海报，来源不足的内部结构只作示意。”
- “设计左脸雀斑、右手义肢的原创动漫机械师，并制作三视图。”
- “做科技宣传海报，背景无文字，中文保留为可编辑对象。”
- “只替换原图右下灯具，保留画幅和构图，第二张图仅参考材质。”
- “延展五张系列海报，图三暖色，其他图不变。”
- “建立适用于科技、文化、美食的通用海报模板，并测试长标题。”

## 文件结构

```text
visual-design-workflow/
├── SKILL.md
├── agents/openai.yaml
├── references/
│   ├── design-workflow.md
│   ├── visual-style-system.md
│   ├── poster-design-guide.md
│   ├── infographic-design-guide.md
│   ├── original-ip-design-guide.md
│   ├── image-generation-and-editing.md
│   ├── visual-consistency-guide.md
│   ├── template-system.md
│   └── quality-control.md
├── assets/template-examples/
│   ├── project-records.md
│   ├── asset-manifest.csv
│   ├── poster-template.json
│   └── poster-layout.svg
├── tests/
│   ├── scenarios.md
│   ├── baseline.md
│   ├── forward-evaluation.md
│   └── TEST-REPORT.md
└── README.md
```

项目生成的 `project-state.md`、`visual-spec.md`、角色表和图片保存在用户项目工作目录，不能混入技能模板。SVG 示例为原生可编辑对象，不代表 AI 出图效果；JSON 配置不含自动布局或渲染程序。

## 验证、限制与扩展

格式依据 [Agent Skills 官方规范](https://agentskills.io/specification)。可在包含验证器的环境执行 `agentskills validate <技能目录>`；本次验证详情见 [测试报告](tests/TEST-REPORT.md)。行为用例见 [scenarios](tests/scenarios.md)。真实视觉质量需实际图片、目标工具和导出检查，不能用文字测试替代。

工具不支持遮罩、图层或特定格式时明确限制。整图生成不等同严格局部保留；字体、参考控制、透明输出和超分依赖实际能力。可编辑交付仅包含真实创建且验证过的源稿，不把位图称为 PSD 分层文件。

扩展新模块时添加独立 references 文件、核心路由和测试，复用现有项目记录与输出合同。UI/UX、品牌、游戏美术、3D、分镜、动态视觉、展陈和 PPT 是未来扩展方向，当前未实现专用流程。

仓库仅包含技能说明、原创模板示例和测试记录，不包含用户原始素材。本项目根据 [MIT License](LICENSE) 发布。
