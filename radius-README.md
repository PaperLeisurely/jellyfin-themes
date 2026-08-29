# Jellyfin · shadcn/ui 圆角 + 动效主题

参考 [shadcn/ui](https://github.com/shadcn-ui/ui) 规范，为 Jellyfin 10.11.x 所有界面添加：

- **第一步 · 统一圆角**（shadcn 圆角标准）
- **第二步 · shadcn 风格动效**

**本主题不改颜色、不动布局、不写任何插件避让规则**。
适合叠加在任何 Jellyfin 主题之上（把它放在 Custom CSS 的最前面）。

## 安装

1. Jellyfin → 设置 → 显示 → **自定义 CSS**
2. 粘贴 `jellyfin-radius-theme.css` 全部内容
3. 保存 → **Ctrl+F5**

> 与主界面主题（如 shadcn 黑白）叠加时：先贴本主题，再贴主主题。

## 动效清单（shadcn 风格）

| 元素 | 动效 |
| --- | --- |
| 卡片 | hover 阴影提升 + 上浮 2px + 图片缩放 1.04，active 回位 |
| 按钮 | hover 上浮 1px、active 回位；Fab hover 放大、active 缩小 |
| 列表项 / 导航项 | hover 背景平滑过渡（150ms） |
| 输入框 | 聚焦 **ring 光环**（2px 白 + 4px 深色） |
| 弹窗 | **fade + zoom** 缩放淡入（200ms） |
| 下拉菜单 | fade 淡入 |
| 滚动箭头 | hover 放大、active 缩小 |
| 图片加载 | 淡入动画 |
| 键盘焦点 | 统一 focus ring 光环 |

统一过渡时长 `150ms` / `200ms`，缓动曲线 `cubic-bezier(0.4, 0, 0.2, 1)`（shadcn 标准）；
系统开启"减弱动态效果"时全部自动禁用。

## shadcn/ui 圆角规范映射

| 层级 | 值 | 应用范围 |
| --- | --- | --- |
| `--radius-sm` | ≈ 6px | 按钮、输入框、下拉框、标签 |
| `--radius-lg` | 8px | 卡片、列表项、缩略图、导航项 |
| `--radius-xl` | ≈ 12px | 弹窗、对话框、详情页海报 |
| `--radius-pill` | 9999px | 圆形按钮、头像、滑块圆点、开关 |

## 覆盖的界面

- **卡片**：海报/剧集/相册卡片（`cardScalable`/`cardImageContainer`/`cardPadder` 全形状）
- **按钮**：主按钮/描边/ghost 6px，图标按钮保持圆形
- **输入控件**：输入框/下拉/文本域 6px
- **弹窗**：对话框/操作菜单/MUI 弹窗 12px
- **列表**：列表项/缩略图 8px
- **导航**：侧边栏导航项 8px（抽屉自身保持直角贴边）
- **播放器**：控制按钮圆形、设置弹窗 12px
- **徽标**：角标/分级标签/指示器 6px
- **进度条**：滑块圆点胶囊、轨道 6px
- **详情页**：海报 12px、影评块/章节预览 8px
- **其他**：头像圆形、滚动条胶囊、复选框 4px、单选圆形

## 技术说明

- 图片圆角用 `border-radius + overflow:hidden` 实现，**不覆盖** Jellyfin 行内 `background-image`（图片正常显示）；
- 顶栏/播放栏/抽屉等贴边元素保持直角（避免贴边处露缝），内部元素圆角；
- 深浅色模式通用（圆角与颜色无关）。

## 文件

- `jellyfin-radius-theme.css` — 主题（唯一粘贴内容）
- `README.md` — 本说明
