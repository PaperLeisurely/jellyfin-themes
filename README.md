# Jellyfin 10.11.11 · Linear 风格暗紫主题

一个 **暗紫基调** 的 Jellyfin 深色主题 CSS，专为 **Jellyfin 10.11.11** 主界面打造，**完全兼容 Media Bar 插件** 与 **jellyfin-danmaku 弹幕插件**。

纯 CSS 轻量方案：单文件、无 JS、**零外部依赖**（不加载任何网络资源），直接把文件全部内容粘贴到 Jellyfin 自定义 CSS 即可。

## 风格构成

| 元素 | 实现 |
| --- | --- |
| 大面积暗色 | 暗紫画布 `#0A0910`，面板 `#0F0D17` / `#16131F` |
| 渐变 | 页面紫罗兰径向渐变、按钮/进度条紫罗兰渐变、顶栏/播放栏渐变 |
| 纤细描边 | 1px 紫调 hairline：`rgba(163,142,255,.1/.2)` |
| 模糊 / 磨砂 | 顶栏、侧边栏、播放栏、弹窗、按钮、头图控件全部 backdrop-blur；页面叠加极细磨砂噪点 |
| 动态流光 | 头图横幅慢速流光扫过、卡片悬停流光、激活圆点呼吸辉光 |
| 微动效 | 卡片悬停上浮+紫光描边、按钮渐变+辉光、导航选中左侧紫条 |
| 简洁 | 所有动效克制低调（低透明度、长周期），并尊重系统"减弱动态效果"设置 |
| **不改尺寸** | **所有 UI 一律保持 Jellyfin / 插件原版尺寸**：只改配色、圆角、磨砂、动效。仅保留两处必要兼容修复（顶栏 `contain:none` 防内容被裁、头图按 ElegantFin 方案下移到顶栏下方），均不改动任何元素的原尺寸 |

## 安装方法

1. 打开 Jellyfin → **控制台 / Dashboard** → **常规 / General**
2. 找到 **自定义 CSS (Custom CSS)** 输入框
3. 将 `jellyfin-linear-theme.css` 的**全部内容**粘贴进去
4. 保存，刷新页面

> 提示：建议先关闭 Jellyfin 自带的**主题（Themes）**中的暗色主题再使用本 CSS；本 CSS 自带完整暗色背景，不依赖任何主题。

## Media Bar 插件兼容说明

针对以下 Media Bar 插件家族的 `.featured-content` DOM 结构做了完整适配（该类插件在首页顶部注入横幅 Hero）：

| 插件 | 兼容情况 |
| --- | --- |
| [IAmParadox27/jellyfin-plugin-media-bar](https://github.com/IAmParadox27/jellyfin-plugin-media-bar)（10.11 分支） | ✅ 完全兼容 |
| [MakD/Jellyfin-Media-Bar](https://github.com/MakD/Jellyfin-Media-Bar)（slideshowpure 自定义代码版） | ✅ 完全兼容 |
| [CodeDevMLH/jellyfin-plugin-media-bar-enhanced](https://github.com/CodeDevMLH/jellyfin-plugin-media-bar-enhanced) | ✅ 完全兼容（含 `.media-bar-*` 设置弹窗、MUI 弹窗、自定义叠加层） |

覆盖的插件元素：`.featured-content`、`.backdrop-container`、`.backdrop`、`.backdrop-overlay`、`.gradient-overlay`、`.info-container`、`.logo`、`.plot`、`.genre`、`.date`、`.runTime`、`.age-rating`、`.misc-info`、`.star-rating-container`、`.community-rating-star`、`.critic-rating`、`.play-button`、`.detail-button`、`.favorite-button`、`.dots-container`、`.dot`、`.arrow`、`.progress-*`、`.bar-loading`、`.video-container`、`.with-video`、`.media-bar-*`（增强版）、MUI `.MuiDialog-paper`、`.custom-overlay-*`、`.is-focused` 等。

> 由于插件 CSS 由插件自身注入，本主题对插件区规则统一使用 `!important`，确保无论加载顺序如何，主题始终生效。

### 头图圆角矩形框（一套规则适配所有设备）

- **卡片**：宽 = `视口宽 − 2×0.5cm`（左右留白 0.5cm，水平居中），高 = **55vh**（随屏幕长度缩放）；顶边 = 0（藏在磨砂固定顶栏后，只保留底部圆角），桌面顶距让开顶栏（窄屏 11.25em / 宽屏 7em）
- **"我的媒体"位置 = 头图顶部 + 头图高度 − 1.5cm**（`--lin-hero-bottom-margin: -1.5cm`，可改）：板块标题上移 1.5cm、压进卡片底部（板块 z-index 高于卡片，文字浮在卡片底部画面上）
- **手机版（≤767px）内容全居中**：LOGO 顶部居中（62% 宽），评分/类型水平居中、按钮/圆点底部居中、简介隐藏——垂直均衡排布、全部压进 55vh 卡片内，不溢出不杂乱；**仅手机端生效，桌面/平板不受影响**（桌面/平板头图内部仍保持插件原版布局）
- **左侧黑色遮罩**：卡片左侧黑→透明渐变（`0.8 → 0.45 → 0`），`pointer-events: none` 不挡点击

**顶栏避让**：`#slides-container` 改为绝对定位并显式下移到固定导航栏下方（方案同 [ElegantFin 的 media-bar-plugin-support 附加包](https://github.com/lscambo13/ElegantFin)）：默认 `top: 11.25em`，宽屏（≥75em）`top: 6.25em`，横幅高度 62%，**不做底部过渡虚化**——头图在圆角卡片内干净收尾，仅保留左侧渐变保证文字可读性。

可改主题文件顶部 `:root` 变量调整圆角/留白；想恢复插件原版全屏头图，删除「17.15 头图圆角矩形框」整块即可。

## jellyfin-danmaku 弹幕插件兼容说明

针对 [Izumiko/jellyfin-danmaku](https://github.com/Izumiko/jellyfin-danmaku)（油猴脚本 `ede.user.js`，或 Nginx/Caddy 注入方式）注入的 DOM 做了全套暗紫化：

| 注入元素 | 说明 |
| --- | --- |
| `#displayDanmaku` | 播放器控制栏"弹幕开关"按钮（开启时紫色图标） |
| `#danmakuWrapper` | 弹幕画布层（保持插件定位，不破坏弹幕坐标） |
| `#danmakuSidebar` | 设置侧边栏（暗紫磨砂面板、细边框） |
| `.danmaku-tab-button` / `.danmaku-tab-content` | 设置选项卡（激活态紫罗兰） |
| `.danmakuSwitchCard` 等设置卡片 | Linear 卡片（细边框、悬停提亮） |
| `.styledRange` / `.styledTextInput` / `.range-value-label` | 滑块、输入框、数值标签（紫色强调） |
| `.dialogOverlay` / `.inputDialog` / `.selectDialog` | 搜索 / 选择弹幕对话框（磨砂遮罩 + 圆角面板） |
| `#dialogConfirm` / `#dialogCancel` | 对话框确认（紫罗兰渐变）/ 取消（幽灵）按钮 |

## 自定义调整

打开 `jellyfin-linear-theme.css` 顶部 **第 1 节「设计变量」**，修改即可：

```css
:root {
  --lin-bg: #0A0910;            /* 页面背景（暗紫） */
  --lin-accent: #8B5CF6;        /* 强调色（改这里换主题色，如 #7C3AED、#A855F7） */
  --lin-text: #EDEBF6;          /* 主文字 */
  --lin-radius: 6px;            /* 圆角 */
  --lin-hero-radius: 16px;      /* Media Bar 头图圆角 */
  --lin-hero-margin: 12px;      /* Media Bar 头图四周留白 */
}
```

### 动效开关

- 全部动效都遵循系统设置：系统开启"减弱动态效果"时自动关闭
- 想手动关掉某个动效：
  - 头图流光 → 删除「17.15」里的 `#slides-container .slide::after` 块
  - 卡片悬停流光 → 删除「7. 卡片」里的 `.cardScalable::after` 块
  - 圆点呼吸辉光 → 删除「17.8」里 `.dot.active` 的 `animation` 一行

## 兼容性

- 目标版本：**Jellyfin 10.11.x**（在 10.11.11 验证的 DOM 结构：`#mainDrawer` / `.navMenuOption` / `.card` / `.skinHeader` / `.homeSectionsContainer` 等）
- 桌面端 / 移动端 / TV 布局均已适配（`layout-mobile`、`layout-tv`）
- 若个别控件未被覆盖，通常是因为 Jellyfin 内置主题的规则优先级更高：请先停用内置主题，或把对应规则补充到本文件末尾

## 常见问题（FAQ）

### 应用主题后图片不显示？

这是旧版主题文件的一个已知问题：卡片图片由懒加载器以**行内样式** `background-image` 直接设在 `.cardImageContainer` 上，而旧文件用 `background` 简写 + `!important` 把它覆盖掉了。
**解决办法：重新把本文件全部内容粘贴到自定义 CSS 并保存刷新**（当前版本已修复，改用 `background-color` 只设底色，不影响图片）。

### 顶栏被裁切错位 / 弹窗尺寸不对？

旧版主题给 `body` 设了 `font-size: 14px`，而 Jellyfin 的 html 根字号是 **20px(1080p)/27px(4K)**——所有按 em/rem 计算的布局（顶栏高度、侧边栏、弹窗、按钮）被整体缩小导致错位。当前版本已移除该覆写，**顶栏、侧边栏、播放栏、弹窗、头图按钮、登录页全部保持 Jellyfin 原始尺寸**，只补圆角。重新粘贴全部内容即可。

### 视频弹幕被遮挡？

旧版顶栏规则把 `backdrop-filter: blur` 也应用到了播放器 OSD 顶栏（`.osdHeader`，覆盖视频顶部约 7.5em），把顶部弹幕磨糊了。当前版本已排除 `.osdHeader`（保持 Jellyfin 原始渐变、无模糊），并给弹幕画布层 `#danmakuWrapper` 补了 `z-index: 300` 保险（高于顶栏/播放栏，低于弹幕自身的侧边栏与对话框，且弹幕层 `pointer-events: none` 不影响任何点击）。重新粘贴全部内容即可。

### 进度条 / 滑块拖动原点不居中？

旧版给原生滑块轨道设了 4px 背景、把原点改成 14px 且没做偏移补偿，导致原点偏离轨道中线。当前版本**完全还原 Jellyfin 原版滑块结构**（可见轨道由 `.mdl-slider-background-flex` 绘制、原点 1.08em 由原版保证居中），只换配色；弹幕设置里那种普通 range 滑块则补了 `margin-top` 居中补偿。重新粘贴全部内容即可。

### Media Bar 头图与顶栏互相裁切 / 冲突？

只保留两条必要兼容修复（均不改任何元素的原尺寸）：
1. 顶栏加 `contain: none`：解除 Jellyfin 自带 `contain: paint`（会把溢出顶栏边界的内容裁掉，且与 backdrop-filter 触发渲染裁切）+ `z-index: 100000` 置于最前
2. `#slides-container` 改为绝对定位下移到顶栏下方（方案与下移量同 ElegantFin：窄屏 11.25em / 宽屏 6.25em，高 62%）——这是插件头图不与顶栏重叠的唯一正确做法，尺寸取自成熟主题而非自定义值

除此之外不再对任何 UI 元素追加尺寸/位置修改。重新粘贴全部内容即可。
