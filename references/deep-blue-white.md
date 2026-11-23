# 深蓝白 / deep · 轻量执行规范

> v3.2 · 2026-11-19。与完整规范成套维护；完整依据见 `design-spec.md`。使用 `../assets/blue-white-report-template.html`，保留旧模板路径。

## 一眼识别

采用“**首尾深蓝，中间白浮**”：深蓝渐变首屏 + 白色光晕 + 受保护的反白结论卡，白至浅蓝正文卡片，深蓝收尾。用于重点汇报、成果总览、竞品结论，不是整页暗色主题。v3.1 只在深底短信息层加玻璃，且一页最多 3 个模糊层，不改首尾与结论签名。

## 必要 token

| 角色 | 取值 |
|---|---|
| 正文 / 辅助 / 链接 | `#0f2a52` / `#3b5b8f` / `#1554b0` |
| 按下态文字底 | `--ink-pressed:#082247`（比 `--ink` 更深，白字对比度更高） |
| 锚点/折叠按下底 | `--tint-pressed:#c6dcfa`（与悬停 `--tint` 明确区分，两套通用） |
| 强调 / 细线 / 浅底 | `#1d6ef5` / `#dbe7f7` / `#e8f1ff` |
| 页面底 | `linear-gradient(180deg,#eef4fd,#f7fafd 40%,#eef4fd)` |
| 首屏 | `linear-gradient(120deg,#0b3ea8 0%,#1d6ef5 55%,#4f9cff 100%)` |
| 收尾 | `linear-gradient(135deg,#0b3ea8,#1d6ef5 60%,#3f8dfd)` |
| 卡片 | `linear-gradient(160deg,#ffffff,#f2f7ff)` |
| 白字保护层 / 结论底 | `rgba(15,42,82,.50)` / `rgba(15,42,82,.32)` |
| 阴影 s / m / l | `0 10px 30px rgba(29,110,245,.07)` / `0 16px 44px rgba(15,42,82,.16)` / `0 24px 60px rgba(29,110,245,.28)` |
| 玻璃底（本套默认） / 模糊 / 描边 | `rgb(255 255 255 / .72)` / `16px` / `rgb(255 255 255 / .70)` |
| 降级底 / 打印底 | `rgb(255 255 255 / .88)` / `#ffffff` |
| 金融涨跌 | `--finance-red:#e64545` / `--finance-green:#1fa663`，由 `.finance` 组件落地 |

色值集中定义为语义变量，组件只引用变量；**本套不声明用不到的 token**。保护层覆盖渐变与光晕、位于文字下方；首屏/收尾正文和辅助字不透明白。白字不得直接压在渐变最浅端；结论卡保持深墨蓝半透 + 反白，不改成白玻璃。

## 排版与组件

- 字阶成组引用 `--type-*`：`--type-hero`（`clamp(26px,3.15vw,34px)/1.29/800`）、`--type-h2` 24/36/800、`--type-h3` 18/28/700、`--type-lead` 20/32/700、`--type-body` 14/24/400、`--type-table` 14/24/400、`--type-button` 14/24/600、`--type-section-no` 14/24/800（40px 方块内）、`--type-caption` 13/24/400、`--type-tag` 12/20/600、`--type-stat` 40/48/800。不散写字号 + 行高对。
- 主容器1080px，桌面padding32/24/64，手机16/16/40；首屏padding40，手机24；卡片padding24，手机16；gap16；章距56。
- 间距只4/8/12/16/24/32/40/48/56/64；圆角只12/24/32，pill999和圆50%例外。
- 1024px 中间宽度只调容器内边距与网格列数，不改字号；760px 下双栏转单栏。
- 方向用逻辑属性：`border-inline-start`、`padding-inline-start`、`text-align:start`、`inset-inline-start`。
- 结论先行；统计/发现/人员明细/风险/节点按内容选用，不造无意义大数字。数字tabular-nums，不用渐变文字。
- 语义标签：蓝字`#1554b0`/底`#e8f1ff`，完成字`#146a45`/底`#e6f7ee`，警示字`#895b11`/底`#fff3e0`，风险字`#b42335`/底`#ffecec`。
- 金融数值用 `.finance` 组件：红涨绿跌，同时写出正负号或箭头；小字号数值改用 danger/success 深色。不凭涨跌色表达任务好坏。
- 空态用 `.empty`：一句原因 + 一句边界 + 一个下一动作，起始对齐；**静态空态不加 live region**，只有异步召回才用 `role="status"`；无登记不等于无产出，0 与未知分开写。

## 玻璃层（v3.1，最多 3 个模糊层）

方法：面板压在渐变/光晕/双色交界上，先有能看清的半透明白底，再用 `backdrop-filter:blur(16px)` 模糊**背后**内容；文字留在清晰层。示例色映射到 `--ink`，24px 圆角复用 `--radius-m`，不新增阴影档。

```css
.glass{position:relative;background:var(--glass-fill-strong);backdrop-filter:blur(var(--glass-blur));-webkit-backdrop-filter:blur(var(--glass-blur));border:1px solid var(--glass-stroke);border-radius:var(--radius-m);color:var(--ink);box-shadow:var(--shadow-s)}
@supports not ((backdrop-filter:blur(1px)) or (-webkit-backdrop-filter:blur(1px))){.glass{background:var(--glass-fallback)}}
@media print{.glass{backdrop-filter:none;-webkit-backdrop-filter:none;background:var(--glass-print);border-color:var(--line);box-shadow:none;color:var(--ink)}}
@media(forced-colors:active){.glass{backdrop-filter:none;-webkit-backdrop-filter:none;border-color:CanvasText;box-shadow:none}}
```

**允许**：① 跨首屏深蓝与浅色正文交界的浮动章节导航条；② 首屏内覆盖渐变的短口径条；③ 收尾深蓝区的「下一步」节点容器——`<ol class="flow glass">` 一层玻璃，三张 `li` 改不透明白底 + line 描边。**禁止**：结论主块、首屏标题/副标题、白底正文卡片、表格、details、来源、按钮六态、语义标签，以及纯白或近纯白底上的任何玻璃。

- 深底背景有渐变 + 光晕，属于“花背景”，把不透明度从 `.55` 提到 `.72`；白高光描边负责分层，最多再加 shadow-s。
- **玻璃内所有文字用 `--ink`**：按渐变最亮端 + 白色光晕 + 保护层合成，`.55` 底 ink 最差 5.64:1、`.72` 底 8.30:1；`--muted` 在 `.72` 底仅 3.97:1、`--link` 更低，均不达标。禁用 muted、透明白、渐变文字、低透明度小字。
- 玻璃只一层、只放标题 + 1~2 句短信息；不与反白结论卡并排承担同一层结论。玻璃容器内 `.flow .caption` 等继承色需显式改为 ink，避免残留白字。
- 跨首屏与正文交界的导航条同时压两种底色：容器描边保留白 70%，浅色侧不可见属已知取舍，控件识别由条内白底胶囊承担，不为本套新增软描边 token。
- 强制色彩模式（`forced-colors:active`）关闭模糊、描边改 `CanvasText`、去阴影；系统接管颜色后不再叠加半透明底。
- **`filter:blur()` 不能替代 `backdrop-filter`**：前者会把面板文字一起糊掉；透明度只写在 `background` 的 alpha，不给面板整体加 `opacity<1`。用 `position:relative;z-index` 分层。附录 A 的浏览器实测记录仅供复现，不是跨浏览器硬规则。
- 可选变体：仅在明确要求“玻璃结论卡”时才换结论主块，标题仍反白，并重新测合成对比度；默认不启用。

## 六态与验收

| 状态 | deep 落点 |
|---|---|
| 默认 | `--link` 蓝底白字，轮廓与文案可识别 |
| 悬停 | `--ink` |
| 按下 | **`--ink-pressed:#082247`，必须比悬停更深** |
| 禁用 | 原生 `disabled` + “暂不可用”文案 |
| 加载 | `aria-disabled="true"` + `aria-busy="true"` + “处理中…” + `role="status" aria-live="polite"`，**不用原生 disabled**；减少动效下隐藏静态圆环，只留文字 |
| 错误 | danger 底 + 文字说明 + `aria-describedby` |

锚点类控件只需默认/悬停/按下/焦点四态，**按下用 `--tint-pressed`，折叠 `summary` 同等对待且必须有悬停反馈**；`focus-visible` 为 3px 清晰外框，深色区用浅色外框。所有可点区域 44×44 以上。玻璃条内胶囊仍为不透明控件，六态不受玻璃影响。

正文/标签≥4.5:1、大字≥3:1；检查保护层合成后的最亮区，并按「渐变 + 光晕 + 保护层 + 玻璃白底」计算玻璃内文字的实际对比度，不只测原色。760px 下双栏转单栏；1024px 中间宽度、360/390px 不得整页横向溢出（窄屏用设备指标覆写，桌面缩窗口会被钳制在约 500px）；200% 缩放按整页缩放验收，等效视口减半；长链接换行、表格局部滚动。检查空态、长数字、键盘、减少动效和打印；额外看一次不支持 `backdrop-filter` 的降级版本。

**禁止**：整页深蓝、浅蓝首屏混入本套、裸白字落浅蓝端、浅灰蓝小字、彩虹头像、灰投影、无依据统计、给正文/表格/按钮套玻璃、玻璃里套玻璃、hover 与 active 同色、物理方向属性、未使用 token。

**调用句**：用蓝白技能的深蓝白套装做重点汇报，首尾深蓝、反白文字有保护层，中间白浮卡片；按 v3.2 在收尾节点容器与跨首屏的浮动导航条使用玻璃层（一页最多 3 个模糊层），结论主块仍反白，正文不玻璃化。
