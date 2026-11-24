# 蓝白双风格报告设计

面向报告、周报、竞品分析、方案、待办清单和数据看板的设计 Skill。提供两套可独立使用的蓝白规范，以及无需外部资源、可离线打开的单文件 HTML 模板。

当前版本：**v3.2**

## 选择风格

| 风格 | 适用内容 | 规范与模板 |
| --- | --- | --- |
| **Deep 深蓝白** | 重点汇报、成果展示、需要更强视觉层级的短报告 | [深蓝白规范](references/deep-blue-white.md) · [HTML 模板](assets/blue-white-report-template.html) |
| **Light 浅蓝白** | 长文、阅读型方案、流程和待办说明 | [浅蓝白规范](references/light-blue-white.md) · [HTML 模板](assets/blue-white-light-report-template.html) |

只说“蓝白”时默认选择 Deep；明确要求长文阅读时选择 Light。两套规范各自定义首屏、文字、收尾和玻璃效果，不混搭两套组件。

## 包含内容

- [`SKILL.md`](SKILL.md)：风格选择、制作流程、无障碍与交付检查清单
- [`references/deep-blue-white.md`](references/deep-blue-white.md)：Deep 执行规范
- [`references/light-blue-white.md`](references/light-blue-white.md)：Light 执行规范
- [`references/design-spec.md`](references/design-spec.md)：完整设计参数、组件配方和维护规则
- [`assets/blue-white-report-template.html`](assets/blue-white-report-template.html)：Deep 单文件报告模板
- [`assets/blue-white-light-report-template.html`](assets/blue-white-light-report-template.html)：Light 单文件报告模板

## 使用方式

将仓库安装到支持 Agent Skill 的工具目录，并让工具读取 `SKILL.md`。例如：

```bash
# Codex
git clone https://github.com/kittstatham/blue-white-design.git ~/.codex/skills/blue-white-design

# Claude Code
git clone https://github.com/kittstatham/blue-white-design.git ~/.claude/skills/blue-white-design

# Workbuddy
git clone https://github.com/kittstatham/blue-white-design.git ~/.workbuddy/skills/blue-white-design
```

安装后可直接提出“按深蓝白规范生成汇报 HTML”或“按浅蓝白模板制作长文方案”等需求。Agent 会读取对应规范、复制对应模板，并将内容写入一份独立 HTML。

已有安装目录时，在仓库目录运行 `git pull` 获取更新。

## 设计与交付约束

模板将样式与脚本内嵌，不依赖外部脚本、字体或图片，可离线打开。规范覆盖键盘操作、焦点状态、窄屏布局、打印、减少动效、强制色彩模式，以及玻璃效果的降级处理。生成报告时应按实际内容选择章节，不补造数据或任务；完成情况以实际渲染和交互检查为准。

维护时以 `references/design-spec.md` 为完整参数来源，并同步更新两份轻量规范、入口说明和两份模板，确保 6 项资料保持一致。
