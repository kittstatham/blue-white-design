# 浅蓝白 / light · 轻量执行规范

> v3.2 · 2026-11-19。采用浅蓝渐变与墨蓝正文，并统一排版参数；不声称逐像素还原或已完成用户验收。完整依据见 `design-spec.md`；使用 `../assets/blue-white-light-report-template.html`。

## 一眼识别

采用“**浅蓝开场，白浮长文，浅蓝收尾**”：浅蓝至白首屏 + 墨蓝标题正文 + 右上淡蓝径向光晕，白至浅蓝卡片，浅色总结。用于方案、长文阅读、待办说明。不是给深蓝模板只改背景。v3.1 只在首屏交界处的浮动导航条加一处玻璃，**按决定保留玻璃，不改实白底导航条**；正文一律保持不透明。

## 必要 token

| 角色 | 取值 |
|---|---|
| 正文 / 辅助 / 链接 | `#0f2a52` / `#3b5b8f` / `#1554b0` |
| 悬停 / 按下底 | `--tint:#e8f1ff` / `--tint-pressed:#c6dcfa`（按下必须更深；按钮、锚点、折叠共用） |
| 强调 / 细线 / 浅底 | `#1d6ef5` / `#dbe7f7` / `#e8f1ff` |
| 页面底 | `linear-gradient(180deg,#eef4fd,#f7fafd 40%,#eef4fd)` |
| 首屏 / 首屏描边 | `linear-gradient(120deg,#deedff,#f0f7ff 65%,#ffffff)` / `#c6dcfa` |
| 光晕 | `radial-gradient(circle,#abcfff66,transparent 70%)` |
| 卡片 | `linear-gradient(160deg,#ffffff,#f3f8ff)` |
| 表头 | `linear-gradient(135deg,#eaf2ff,#f5f9ff)` |
| 收尾 / 收尾描边 | `linear-gradient(135deg,#e1eeff,#ffffff)` / `#bbd6fb` |
| 阴影 s / m / l | `0 10px 30px rgba(29,110,245,.07)` / `0 12px 34px rgba(29,110,245,.09)` / `0 20px 50px rgba(29,110,245,.086)` |
| 玻璃底 / 软描边 | `rgb(255 255 255 / .55)` / `rgba(15,42,82,.12)` |
| 玻璃模糊 / 降级底 / 打印底 | `16px` / `rgb(255 255 255 / .88)` / `#ffffff` |
| 金融涨跌 | `--finance-red:#e64545` / `--finance-green:#1fa663`，由 `.finance` 组件落地 |

色值集中定义为语义变量，组件引用变量；**本套不声明用不到的 token**。装饰只在容器层，正文不做渐变。保持冷静低干扰，不把整页染蓝。

## 排版与组件

- 字阶成组引用 `--type-*`：`--type-hero`（`clamp(26px,3.15vw,34px)/1.29/800`）、`--type-h2` 24/36/800、`--type-h3` 18/28/700、`--type-lead` 20/32/700、`--type-body` 15/28/400、`--type-table` 14/24/400、`--type-button` 14/24/600、`--type-section-no` 14/36/700（文字式 `01 /`）、`--type-caption` 13/24/400、`--type-tag` 12/20/600。不散写字号 + 行高对。
- 容器1080px居中，桌面padding32/24/64，手机16/16/40；首屏padding40、手机24；卡片24、手机16；gap16；章距40。
- 间距只4/8/12/16/24/32/40/48/56/64；圆角只12/24/32；pill999/圆50%是形状例外。
- 1024px 中间宽度只调容器内边距与网格列数，不改字号；760px 下正文双栏转单栏。
- 方向用逻辑属性：`border-inline-start`、`padding-inline-start`、`text-align:start`、`inset-inline-start`。
- 首屏直接呈现标签、标题、引言、来源；不要套反白玻璃结论。章节用“01 / 标题”即可，不强制实心编号徽章。
- 导航用白底蓝字胶囊、44px触区、换行；只链接真实章节。提示条用浅蓝底 + 4px 强调逻辑起始边线。
- 长文优先单栏；比较时双栏；流程最多四栏，窄屏两栏；待办票据纵排；折叠用 details/summary。
- 表头极浅渐变、细分割；表格用 `--type-table`，允许局部滚动且可键盘聚焦。总结用浅蓝至白，不放深色收尾。
- 语义标签：蓝`#1554b0`/底`#e8f1ff`，完成`#146a45`/底`#e6f7ee`，警示`#895b11`/底`#fff3e0`，风险`#b42335`/底`#ffecec`；同时写出语义。
- 数字tabular-nums；金融数值用 `.finance` 组件红涨绿跌，并配正负号/箭头。无真实统计就不造KPI卡。
- 空态用 `.empty`：一句原因 + 一句边界 + 一个下一动作，起始对齐；**静态空态不加 live region**，只有异步召回才用 `role="status"`；无登记不等于无产出，0 与未知分开写。

## 玻璃层（v3.1，仅一处，保留玻璃）

依据文章方法：面板压在渐变交界上，先有能看清的半透明白底，再模糊背后内容；文字保持清晰；不支持 `backdrop-filter` 时保留高不透明底。

```css
.glass{position:relative;background:var(--glass-fill);backdrop-filter:blur(var(--glass-blur));-webkit-backdrop-filter:blur(var(--glass-blur));border:1px solid var(--glass-stroke-soft);border-radius:var(--radius-m);color:var(--ink);box-shadow:var(--shadow-s)}
@supports not ((backdrop-filter:blur(1px)) or (-webkit-backdrop-filter:blur(1px))){.glass{background:var(--glass-fallback)}}
@media print{.glass{backdrop-filter:none;-webkit-backdrop-filter:none;background:var(--glass-print);border-color:var(--line);box-shadow:none;color:var(--ink)}}
@media(forced-colors:active){.glass{backdrop-filter:none;-webkit-backdrop-filter:none;border-color:CanvasText;box-shadow:none}}
```

**允许**：跨浅蓝首屏与页面底色交界的浮动章节导航条（负外边距压住首屏下沿），用 `.55` 玻璃底 + 深色软描边 + shadow-s；条内胶囊仍是白底蓝字。**禁止**：结论直接排版、长文正文卡片、表格、details、票据、收尾、提示条、六态控件，以及任何反白玻璃或墨蓝字玻璃结论卡。

- 白高光描边在近白底不可见，必须改用 `--glass-stroke-soft`；玻璃只一层，不加第二层卡。
- **此条是完整规范 §2.4 明确列出的唯一例外**：浅底与页面底亮度接近，模糊视觉收益有限，但作为首屏到正文的层次签名保留，**不改成实白底导航条，也不删除 `backdrop-filter`**。两套规范不再互相禁止。
- 玻璃内文字一律 `--ink`（本套合成底上 ink ≥4.5:1）；条内胶囊自带白底，控件六态不受玻璃影响。
- 不用 `filter:blur()` 替代 `backdrop-filter`（会把文字一起糊掉），也不给面板整体加 `opacity<1`；透明度只在 `background` 的 alpha 上。
- 打印与不支持模糊时回退到 `--glass-fallback` / `--glass-print`，导航条仍是可读的白条。目标浏览器需实际看一次模糊与降级效果。
- 强制色彩模式（`forced-colors:active`）关闭模糊、描边改 `CanvasText`、去阴影；系统接管颜色后不再叠加半透明底。

## 六态与验收

| 状态 | light 落点 |
|---|---|
| 默认 | 白底蓝字 + link 描边 |
| 悬停 | `--tint` |
| 按下 | **`--tint-pressed:#c6dcfa`，比悬停更深** |
| 禁用 | 原生 `disabled` + 说明 |
| 加载 | `aria-disabled="true"` + `aria-busy="true"` + “处理中…” + `role="status" aria-live="polite"`，**不用原生 disabled**；减少动效下隐藏静态圆环，只留文字 |
| 错误 | danger 底 + 文字 + `aria-describedby` |

锚点类控件只需默认/悬停/按下/焦点四态，**按下用 `--tint-pressed`，折叠 `summary` 同等对待且必须有悬停反馈**；`focus-visible` 用 3px link 外框偏移 3px；触区至少 44×44px。

正文/标签≥4.5:1、大字≥3:1；不用`#7d90ad`小字、浅蓝底上的亮蓝小字。玻璃按合成底测量。760px 下双栏转单栏；1024px 中间宽度、360/390px 无整页横向滚动（窄屏用设备指标覆写，桌面缩窗口会被钳制在约 500px），长路径可换行；200% 缩放按整页缩放验收，等效视口减半。检查首/中/尾、空态、长数字、键盘、减少动效与打印；额外看一次不支持 `backdrop-filter` 的降级版本。

## 与参考页的取舍

保留首屏/卡片/表头/收尾配色、墨蓝正文和蓝影。将38px首屏padding→40px、25px卡片padding→24px、17pxgap→16px、42px章距→40px、30px首屏圆角→32px，统一正文15/28；这是轻量规范化，不是任意换风格。

**禁止**：深蓝首屏或收尾、白字、反白毛玻璃、正文渐变、满屏蓝色实块、无意义统计卡、层层套卡、给正文或表格套玻璃、把 deep 的深底玻璃卡搬过来、把本套玻璃导航条改成实白条、hover 与 active 同色、物理方向属性、未使用 token。

**调用句**：用蓝白技能的浅蓝白套装；浅蓝首屏、墨蓝正文、白浮卡片、浅蓝收尾，适合长文阅读，不混入深蓝反白模块；按 v3.2 只在跨首屏的浮动导航条保留一处玻璃。
