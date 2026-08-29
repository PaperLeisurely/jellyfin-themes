# Jellyfin 管理控制台 · shadcn/ui 黑白主题

独立的管理控制台（Dashboard）主题，纯 CSS，黑白界面（shadcn/ui 风格）。
**只作用于控制台界面**（`body.dashboardDocument`），不影响主界面/播放器/Media Bar。

## 安装

1. Jellyfin → 设置（齿轮）→ 高级 → **自定义 CSS**
2. 粘贴 `jellyfin-dashboard-theme.css` 全部内容
3. 保存 → 进入控制台 → **Ctrl+F5** 刷新

## 覆盖的界面

| 控制台区域 | 覆盖内容 |
| --- | --- |
| 顶栏 | MUI AppBar / Toolbar / 图标按钮 |
| 侧边抽屉 | Drawer / 导航项 hover·选中 / 分组标题 / 分隔线 |
| 仪表盘统计卡片 | Widget 卡片、标题、大字数值 |
| 表格 | material-react-table / MUI Table（表头/表体/hover/排序） |
| 按钮 | contained / outlined / text / error 四种变体 |
| 表单 | 输入框（聚焦 ring）、下拉、复选框、单选、开关 Switch |
| 标签页 / Chip | Tabs 指示条、状态 Chip（成功/错误/警告/信息） |
| 弹窗 / 提示 | Dialog、Snackbar、Alert |
| 其他 | 滚动条、focus ring 光环、深浅色自动切换 |

## 说明

- **作用域隔离**：所有规则带 `body.dashboardDocument` 前缀——控制台入口
  （`AppLayout.tsx`）会给 body 添加该类，所以主题**不会泄漏到主界面**；
- **深浅色**：`prefers-color-scheme` 自动跟随（浅色白底黑字 / 深色黑底白字）；
- **MUI 变量桥接**：覆盖 `--jf-palette-*`，控制台 MUI 组件自动跟随黑白灰。

## 与主 shadcn 主题配合

控制台主题与主界面主题（`jellyfin-shadcn-theme.css`）互不冲突：
- 想整体黑白 → 两个都装（Custom CSS 里两段都粘贴）；
- 只想改控制台 → 只装这个；
- 只想改主界面 → 只装主 shadcn 主题。
