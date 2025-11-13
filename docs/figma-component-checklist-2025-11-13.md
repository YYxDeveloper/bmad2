# BMAD2 Figma 组件清单
## Component Checklist for Figma Design

**项目**: Yu-Gi-Oh Duel Console (BMAD2)
**版本**: v1.0.0
**创建日期**: 2025-11-13

---

## 使用说明

本清单列出了所有需要在 Figma 中创建的组件。建议按照以下顺序创建:

1. ✅ **基础原子组件** (Buttons, Icons, etc.)
2. ✅ **复合组件** (Cards, Input Fields, etc.)
3. ✅ **布局组件** (Navigation Bar, Toolbar, etc.)
4. ✅ **页面模板** (Screen Templates)

每个组件创建完成后，请在前面打勾 ✅

---

## 1. 基础原子组件 (Atomic Components)

### 1.1 按钮 (Buttons)

#### Glow Button (发光按钮)
- [ ] Component Set: `Button/Glow`
- [ ] **Variants**:
  - [ ] Color: Blue, Orange
  - [ ] Size: Small (88×40), Medium (120×48), Large (200×56)
  - [ ] State: Default, Hover, Pressed, Disabled
- [ ] **总计**: 24 个变体 (2 colors × 3 sizes × 4 states)

**设计要点**:
- ✅ 使用 Auto Layout
- ✅ Padding: Horizontal 24px, Vertical 12px
- ✅ Border: 2px, 60% opacity
- ✅ 应用对应的 Glow Effect Style
- ✅ 文本使用 Button Text Style

#### Icon Button (图标按钮)
- [ ] Component Set: `Button/Icon`
- [ ] **Variants**:
  - [ ] Color: Blue, Orange, White
  - [ ] Size: Small (40×40), Medium (48×48), Large (56×56)
  - [ ] State: Default, Pressed, Disabled
- [ ] **总计**: 27 个变体 (3 colors × 3 sizes × 3 states)

**设计要点**:
- ✅ 圆形边框
- ✅ 图标大小 = 容器的 50%
- ✅ 应用对应的 Glow Effect Style

#### Text Button (文本按钮)
- [ ] Component Set: `Button/Text`
- [ ] **Variants**:
  - [ ] Color: Blue, Orange, White
  - [ ] State: Default, Pressed, Disabled
- [ ] **总计**: 9 个变体 (3 colors × 3 states)

**设计要点**:
- ✅ 无边框，仅文本
- ✅ 轻微发光效果
- ✅ Pressed 状态增加透明度

### 1.2 图标 (Icons)

#### 功能图标
- [ ] `icon_plus` (加号)
- [ ] `icon_minus` (减号)
- [ ] `icon_dice` (骰子)
- [ ] `icon_coin` (硬币)
- [ ] `icon_history` (历史记录)
- [ ] `icon_settings` (设置)
- [ ] `icon_bluetooth` (蓝牙)
- [ ] `icon_info` (信息)
- [ ] `icon_reset` (重置)
- [ ] `icon_undo` (撤销)
- [ ] `icon_volume` (音量)
- [ ] `icon_vibrate` (震动)
- [ ] `icon_close` (关闭)
- [ ] `icon_check` (确认)
- [ ] `icon_arrow_back` (返回)
- [ ] `icon_menu` (菜单)
- [ ] `icon_share` (分享)
- [ ] `icon_delete` (删除)

**设计要点**:
- ✅ 统一尺寸: 24×24px
- ✅ 2px 线宽
- ✅ 圆角处理
- ✅ 创建为 Component
- ✅ 支持颜色覆盖 (Tint)

### 1.3 粒子 (Particles)

#### Particle Components
- [ ] `Particle/Blue/Small` (2px, 80% opacity, blur 1px)
- [ ] `Particle/Blue/Medium` (3px, 60% opacity, blur 2px)
- [ ] `Particle/Blue/Large` (4px, 40% opacity, blur 2px)
- [ ] `Particle/Orange/Small`
- [ ] `Particle/Orange/Medium`
- [ ] `Particle/Orange/Large`
- [ ] `Particle/White/Small`
- [ ] `Particle/White/Medium`
- [ ] `Particle/White/Large`

**总计**: 9 个粒子组件

### 1.4 分隔线 (Dividers)

#### Glow Divider
- [ ] Component Set: `Divider/Glow`
- [ ] **Variants**:
  - [ ] Color: Blue, Orange, White
  - [ ] Orientation: Horizontal, Vertical
- [ ] **总计**: 6 个变体

**设计要点**:
- ✅ 使用渐变 (透明 → 颜色 @ 60% → 透明)
- ✅ 应用轻微发光效果
- ✅ 高度/宽度: 1px

---

## 2. 复合组件 (Compound Components)

### 2.1 卡片 (Cards)

#### Glow Card
- [ ] Component Set: `Card/Glow`
- [ ] **Variants**:
  - [ ] Border Color: Blue, Orange, White
  - [ ] Size: Small, Medium, Large
- [ ] **总计**: 9 个变体

**设计要点**:
- ✅ 背景: #FFFFFF @ 6%
- ✅ 边框: 1px, 40% opacity
- ✅ 圆角: 12px
- ✅ 使用 Auto Layout
- ✅ Padding: 16px
- ✅ 支持 Slot 插入内容

#### List Item Card (列表项卡片)
- [ ] Component: `Card/List Item`
- [ ] 用于历史记录列表

**设计要点**:
- ✅ 固定高度: 72px
- ✅ 包含: 时间戳、玩家信息、对战时长
- ✅ 支持点击交互

### 2.2 输入框 (Input Fields)

#### Glow Text Field
- [ ] Component Set: `Input/TextField`
- [ ] **Variants**:
  - [ ] Color: Blue, Orange
  - [ ] State: Default, Focused, Error, Disabled
- [ ] **总计**: 8 个变体

**设计要点**:
- ✅ 包含: Label, Input, Helper Text
- ✅ 使用 Auto Layout (垂直)
- ✅ 圆角: 8px
- ✅ Focused 状态: 边框 2px, 100% opacity

#### Number Input (数字输入框)
- [ ] Component Set: `Input/Number`
- [ ] 用于设置初始生命值
- [ ] **Variants**:
  - [ ] State: Default, Focused
- [ ] 包含 +/- 按钮

### 2.3 对话框 (Dialogs)

#### Glow Dialog
- [ ] Component: `Dialog/Glow`
- [ ] **包含**:
  - [ ] Title (Text)
  - [ ] Content Slot
  - [ ] Action Buttons (使用 Glow Button)

**设计要点**:
- ✅ 宽度: 327px (固定)
- ✅ 高度: Auto
- ✅ 背景: #000000
- ✅ 边框: 2px, 60% opacity
- ✅ 圆角: 16px
- ✅ Strong Glow Effect
- ✅ Padding: 24px

#### Confirm Dialog (确认对话框)
- [ ] Component: `Dialog/Confirm`
- [ ] 用于危险操作确认 (如重置、删除)

#### Alert Dialog (提示对话框)
- [ ] Component: `Dialog/Alert`
- [ ] 用于信息提示

### 2.4 开关与滑动条 (Switches & Sliders)

#### Glow Switch
- [ ] Component Set: `Switch/Glow`
- [ ] **Variants**:
  - [ ] Color: Blue, Orange
  - [ ] State: On, Off, Disabled
- [ ] **总计**: 6 个变体

**设计要点**:
- ✅ 尺寸: 51×31px
- ✅ On 状态: 发光效果
- ✅ Off 状态: 灰色，无发光

#### Glow Slider
- [ ] Component Set: `Slider/Glow`
- [ ] **Variants**:
  - [ ] Color: Blue, Orange
  - [ ] State: Default, Disabled
- [ ] **总计**: 4 个变体

**设计要点**:
- ✅ 轨道高度: 4px
- ✅ 滑块直径: 20px
- ✅ Active 部分发光
- ✅ Inactive 部分 20% opacity

### 2.5 加载与进度 (Loading & Progress)

#### Loading Spinner (加载动画)
- [ ] Component Set: `Loading/Spinner`
- [ ] **Variants**:
  - [ ] Color: Blue, Orange, White
  - [ ] Size: Small (20px), Medium (32px), Large (48px)
- [ ] **总计**: 9 个变体

**设计要点**:
- ✅ 旋转的圆环
- ✅ 发光效果
- ✅ 创建多帧展示旋转动画

#### Progress Bar (进度条)
- [ ] Component Set: `Progress/Bar`
- [ ] **Variants**:
  - [ ] Color: Blue, Orange
  - [ ] Progress: 0%, 25%, 50%, 75%, 100%
- [ ] **总计**: 10 个变体

---

## 3. 布局组件 (Layout Components)

### 3.1 导航栏 (Navigation Bars)

#### Top Navigation Bar
- [ ] Component Set: `Navigation/Top Bar`
- [ ] **Variants**:
  - [ ] Type: With Back Button, Without Back Button
  - [ ] Actions: With Action Button, Without Action
- [ ] **总计**: 4 个变体

**设计要点**:
- ✅ 高度: 56px
- ✅ 包含: Back Button, Title, Action Button
- ✅ 使用 Auto Layout (Horizontal)
- ✅ Padding: 16px horizontal

#### Bottom Toolbar (底部工具栏)
- [ ] Component: `Navigation/Bottom Toolbar`
- [ ] 用于对战界面

**设计要点**:
- ✅ 高度: 72px
- ✅ 包含 4 个 Icon Button
- ✅ 均匀分布
- ✅ 使用 Auto Layout

### 3.2 标签页 (Tabs)

#### Tab Bar
- [ ] Component Set: `Tabs/Bar`
- [ ] **Variants**:
  - [ ] Tab Count: 2, 3, 4
  - [ ] Selected: Tab 1, Tab 2, Tab 3, Tab 4
- [ ] 未来扩展使用

---

## 4. 特殊组件 (Special Components)

### 4.1 生命值显示 (Life Points Display)

#### Life Points Component
- [ ] Component Set: `LifePoints/Display`
- [ ] **Variants**:
  - [ ] Color: Blue, Orange
  - [ ] Size: Large (96px), Medium (72px), Small (48px)
  - [ ] State: Normal, Critical (< 2000), Zero (= 0)
- [ ] **总计**: 18 个变体

**设计要点**:
- ✅ 字体: Roboto Mono Black
- ✅ Normal: 对应玩家颜色 + Strong Glow
- ✅ Critical: 黄色警告 (#FFD700)
- ✅ Zero: 红色 (#FF0044) + 强烈发光

### 4.2 骰子 (Dice)

#### Dice Face
- [ ] Component Set: `Dice/Face`
- [ ] **Variants**:
  - [ ] Value: 1, 2, 3, 4, 5, 6
  - [ ] Color: Blue, Orange, White
- [ ] **总计**: 18 个变体

**设计要点**:
- ✅ 尺寸: 100×100px
- ✅ 3D 视觉效果 (阴影、高光)
- ✅ 圆角: 12px
- ✅ 骰子点使用圆形

#### Dice Animation Frames
- [ ] Frame 1: 骰子旋转初始状态
- [ ] Frame 2: 旋转中 (模糊)
- [ ] Frame 3: 旋转中 (不同角度)
- [ ] Frame 4: 减速
- [ ] Frame 5: 停止 (显示结果)

### 4.3 硬币 (Coin)

#### Coin Face
- [ ] Component Set: `Coin/Face`
- [ ] **Variants**:
  - [ ] Side: Heads, Tails
  - [ ] Color: Blue, Orange
- [ ] **总计**: 4 个变体

**设计要点**:
- ✅ 尺寸: 80×80px
- ✅ 圆形
- ✅ 3D 金属质感
- ✅ 正面: ⚔️ (剑标志)
- ✅ 反面: 🛡️ (盾标志)

#### Coin Animation Frames
- [ ] Frame 1-10: 硬币旋转动画

### 4.4 粒子系统 (Particle Systems)

#### Particle Burst (粒子爆发)
- [ ] Component: `Particles/Burst`
- [ ] 预设 20-30 个粒子的爆发效果

**使用场景**:
- 生命值变化
- 特殊事件触发

#### Particle Ambient (环境粒子)
- [ ] Component: `Particles/Ambient`
- [ ] 预设 50-100 个粒子的环境效果

**使用场景**:
- 玩家区域背景装饰

---

## 5. 状态组件 (State Components)

### 5.1 空状态 (Empty States)

#### Empty State - History
- [ ] Component: `EmptyState/History`
- [ ] 包含: 插图 + 文案 + 按钮

#### Empty State - Search
- [ ] Component: `EmptyState/Search`
- [ ] 用于未来搜索功能

### 5.2 错误状态 (Error States)

#### Error State - Network
- [ ] Component: `ErrorState/Network`
- [ ] 包含: 图标 + 错误信息 + 重试按钮

#### Error State - Bluetooth
- [ ] Component: `ErrorState/Bluetooth`
- [ ] 包含: 蓝牙图标 + 错误信息 + 重试按钮

### 5.3 加载状态 (Loading States)

#### Loading State - Full Screen
- [ ] Component: `LoadingState/FullScreen`
- [ ] 包含: Loading Spinner + 文案

#### Skeleton Screen (骨架屏)
- [ ] Component: `LoadingState/Skeleton`
- [ ] 用于列表加载

---

## 6. 页面模板 (Screen Templates)

### 6.1 对战界面模板 (Battle Templates)

#### Battle Screen Template
- [ ] Template: `Template/Battle/Default`
- [ ] 包含所有布局元素，可复用

**包含组件**:
- ✅ Player 1 Area (Frame)
- ✅ Player 2 Area (Frame, 旋转 180°)
- ✅ Center Divider
- ✅ Life Points Display × 2
- ✅ Bottom Toolbar × 2
- ✅ Top Navigation Bar × 2

### 6.2 列表界面模板 (List Templates)

#### List Screen Template
- [ ] Template: `Template/List/Standard`
- [ ] 包含: Top Bar + Scrollable List + Safe Area

### 6.3 设置界面模板 (Settings Templates)

#### Settings Screen Template
- [ ] Template: `Template/Settings/Standard`
- [ ] 包含: Top Bar + Sections + List Items

---

## 7. 动画与交互 (Animations & Interactions)

### 7.1 微交互组件 (Micro-interactions)

#### Button Press Animation
- [ ] 创建 3 帧展示按钮按下效果
  - [ ] Frame 1: Default
  - [ ] Frame 2: Pressed (Scale 0.95)
  - [ ] Frame 3: Released (Scale 1.0)

#### Glow Pulse Animation
- [ ] 创建 4 帧展示发光脉冲
  - [ ] Frame 1: Normal Glow
  - [ ] Frame 2: Strong Glow
  - [ ] Frame 3: Stronger Glow
  - [ ] Frame 4: Back to Normal

#### Number Change Animation
- [ ] 创建数字滚动效果
  - [ ] Frame 1-10: 8000 → 7500 (每帧递减 50)

### 7.2 转场动画 (Transitions)

#### Fade In/Out
- [ ] 创建淡入淡出关键帧

#### Slide In/Out
- [ ] 创建滑入滑出关键帧

---

## 8. 组件使用说明 (Component Documentation)

### 8.1 为每个组件添加描述

在 Figma 中为每个组件添加描述:

```
组件名称: Glow Button
描述: 带有发光边框的主按钮，用于主要操作
使用场景: 确认、提交、开始对战等
设计规范: 参考 design-system-specifications.md

变体说明:
- Color: 蓝色用于 Player 1 相关操作，橙色用于 Player 2
- Size: Small 用于紧凑布局，Medium 为默认，Large 用于重要操作
- State: Default 为默认状态，Hover 为悬停，Pressed 为按下，Disabled 为禁用
```

为以下组件添加文档:
- [ ] Glow Button
- [ ] Icon Button
- [ ] Glow Card
- [ ] Glow Text Field
- [ ] Glow Dialog
- [ ] Life Points Display
- [ ] Dice Face
- [ ] Coin Face

### 8.2 创建组件使用示例页面

- [ ] 创建页面: `Component Examples`
- [ ] 展示每个组件的所有变体
- [ ] 添加使用场景说明
- [ ] 标注尺寸和间距

---

## 9. 质量检查 (Quality Check)

### 9.1 组件检查清单

为每个组件执行以下检查:

**命名检查**:
- [ ] 组件命名遵循规范 (如 `Button/Glow/Blue/Medium/Default`)
- [ ] 图层命名清晰 (无 "Rectangle 1" 之类)

**结构检查**:
- [ ] 使用 Auto Layout (如适用)
- [ ] 使用 Constraints 实现响应式
- [ ] 图层结构清晰有序

**样式检查**:
- [ ] 使用 Color Styles (无硬编码颜色)
- [ ] 使用 Text Styles (无硬编码文本样式)
- [ ] 使用 Effect Styles (无硬编码效果)

**交互检查**:
- [ ] 支持 Component Property 覆盖
- [ ] Variants 切换正常
- [ ] 在不同尺寸下显示正常

### 9.2 性能优化

- [ ] 删除隐藏的不必要图层
- [ ] 合并重复的矢量路径
- [ ] 优化复杂的布尔运算
- [ ] 减少不必要的嵌套

---

## 10. 导出准备 (Export Preparation)

### 10.1 为导出设置标记

为需要导出的组件添加 Export 设置:

**图标 (SVG)**:
- [ ] 为所有图标添加 SVG 导出设置
- [ ] 命名格式: `icon_[name].svg`

**图片 (PNG)**:
- [ ] App Icon: `app_icon_1024.png` (1024×1024)
- [ ] 粒子贴图: `particle_glow.png` (32×32)
- [ ] 其他必要图片资源

**动画 (Lottie/JSON)**:
- [ ] 骰子旋转动画
- [ ] 硬币旋转动画
- [ ] 粒子爆发动画

### 10.2 创建导出页面

- [ ] 创建页面: `Export Assets`
- [ ] 将所有需要导出的资源整理在此页面
- [ ] 添加导出说明

---

## 进度追踪

### 统计

**基础原子组件**: 0 / 4 完成
**复合组件**: 0 / 5 完成
**布局组件**: 0 / 2 完成
**特殊组件**: 0 / 4 完成
**状态组件**: 0 / 3 完成
**页面模板**: 0 / 3 完成
**动画与交互**: 0 / 2 完成

**总计**: 0 / 23 组件类别完成

### 完成时间估算

- **基础原子组件**: 3-4 天
- **复合组件**: 3-4 天
- **布局组件**: 1-2 天
- **特殊组件**: 2-3 天
- **状态组件**: 1-2 天
- **页面模板**: 2-3 天
- **动画与交互**: 2-3 天
- **质量检查**: 1-2 天

**总计**: 15-23 工作日

---

## 附录: 组件设计检查表

复制此检查表用于每个组件:

```
组件名称: __________________

✅ 设计检查:
- [ ] 遵循设计系统规范
- [ ] 颜色使用 Color Styles
- [ ] 文本使用 Text Styles
- [ ] 效果使用 Effect Styles
- [ ] 间距符合 4px 网格

✅ 结构检查:
- [ ] 使用 Auto Layout
- [ ] 使用 Constraints
- [ ] 图层命名规范
- [ ] 结构清晰有序

✅ 功能检查:
- [ ] 所有 Variants 创建完成
- [ ] Component Properties 设置正确
- [ ] 在不同状态下显示正常
- [ ] 在不同尺寸下显示正常

✅ 文档检查:
- [ ] 添加组件描述
- [ ] 说明使用场景
- [ ] 标注特殊注意事项

✅ 导出检查:
- [ ] 设置导出格式 (如需要)
- [ ] 命名规范正确
```

---

**文档版本**: v1.0.0
**最后更新**: 2025-11-13
**维护者**: BMAD2 UX Team

---

## 使用建议

1. **打印此清单** 或在第二屏幕上显示，设计时逐项勾选
2. **从简单到复杂** 依次创建组件
3. **创建一个组件库页面** 展示所有组件
4. **定期提交** Figma 版本历史，方便回溯
5. **与开发团队同步** 确保组件可实现

祝设计顺利! 🎨✨
