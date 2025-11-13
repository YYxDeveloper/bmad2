# BMAD2 设计系统规范
## Design System Specifications

**项目**: Yu-Gi-Oh Duel Console (BMAD2)
**版本**: v1.0.0
**创建日期**: 2025-11-13
**设计风格**: Tron: Legacy Cyberpunk
**目标平台**: Flutter (iOS & Android)

---

## 目录

1. [设计原则 Design Principles](#设计原则)
2. [设计令牌 Design Tokens](#设计令牌)
3. [颜色系统 Color System](#颜色系统)
4. [字体系统 Typography System](#字体系统)
5. [间距系统 Spacing System](#间距系统)
6. [阴影与发光 Shadows & Glows](#阴影与发光)
7. [动画系统 Animation System](#动画系统)
8. [组件库 Component Library](#组件库)
9. [图标系统 Icon System](#图标系统)
10. [布局规范 Layout Specifications](#布局规范)
11. [交互规范 Interaction Guidelines](#交互规范)
12. [资源组织 Asset Organization](#资源组织)

---

## 1. 设计原则 Design Principles

### 1.1 核心原则

#### **视觉优先 (Visual First)**
- 每个界面元素都应该有视觉冲击力
- 动效优先于静态设计
- 使用发光、粒子、呼吸灯增强视觉感受

#### **沉浸体验 (Immersive Experience)**
- 黑色背景营造深邃空间感
- 蓝/橙色对比强化阵营感
- 全屏无干扰的决斗环境

#### **直觉交互 (Intuitive Interaction)**
- 滑动力度对应数值变化幅度
- 实时反馈（粒子爆发、声音、震动）
- 手势优先，减少按钮点击

#### **赛博朋克美学 (Cyberpunk Aesthetic)**
- 受 Tron: Legacy 启发的视觉语言
- 霓虹发光线条与几何图形
- 科技感与未来感并存

---

## 2. 设计令牌 Design Tokens

### 2.1 Token 结构

```dart
// lib/core/theme/design_tokens.dart

class DesignTokens {
  // Colors
  static const ColorTokens colors = ColorTokens();

  // Typography
  static const TypographyTokens typography = TypographyTokens();

  // Spacing
  static const SpacingTokens spacing = SpacingTokens();

  // Shadows & Glows
  static const ShadowTokens shadows = ShadowTokens();

  // Animation
  static const AnimationTokens animation = AnimationTokens();

  // Borders & Radius
  static const BorderTokens borders = BorderTokens();
}
```

---

## 3. 颜色系统 Color System

### 3.1 主色调 Primary Colors

| Token Name | Hex Code | RGB | Usage |
|------------|----------|-----|-------|
| `primary-blue` | `#00D9FF` | `0, 217, 255` | Player 1 主色、强调色 |
| `primary-orange` | `#FF6B35` | `255, 107, 53` | Player 2 主色、警告色 |
| `neutral-black` | `#000000` | `0, 0, 0` | 背景色 |
| `neutral-white` | `#FFFFFF` | `255, 255, 255` | 文本、图标 |

### 3.2 功能色 Functional Colors

| Token Name | Hex Code | Usage |
|------------|----------|-------|
| `success-green` | `#00FF88` | 成功提示、正向变化 |
| `error-red` | `#FF0044` | 错误提示、生命值为0 |
| `warning-yellow` | `#FFD700` | 警告提示、投掷骰子 |
| `info-purple` | `#9D4EDD` | 信息提示 |

### 3.3 透明度梯度 Opacity Scale

| Token Name | Opacity | Usage |
|------------|---------|-------|
| `opacity-full` | `1.0` | 前景元素 |
| `opacity-high` | `0.87` | 主要文本 |
| `opacity-medium` | `0.60` | 次要文本 |
| `opacity-low` | `0.38` | 禁用状态 |
| `opacity-subtle` | `0.12` | 分隔线、背景 |

### 3.4 发光颜色 Glow Colors

```dart
class GlowColors {
  // Blue glow variations
  static const Color blueGlow = Color(0xFF00D9FF);
  static const Color blueGlowStrong = Color(0xFF00A8CC);
  static const Color blueGlowSubtle = Color(0xFF0088AA);

  // Orange glow variations
  static const Color orangeGlow = Color(0xFFFF6B35);
  static const Color orangeGlowStrong = Color(0xFFFF4500);
  static const Color orangeGlowSubtle = Color(0xFFCC5522);

  // Particle colors
  static const Color particleBlue = Color(0x8800D9FF); // 53% opacity
  static const Color particleOrange = Color(0x88FF6B35);
  static const Color particleWhite = Color(0x44FFFFFF);
}
```

### 3.5 渐变 Gradients

```dart
class GradientTokens {
  // Blue radial gradient for Player 1 area
  static const RadialGradient blueRadial = RadialGradient(
    center: Alignment.center,
    radius: 0.8,
    colors: [
      Color(0xFF00D9FF),
      Color(0xFF0088AA),
      Color(0xFF000000),
    ],
    stops: [0.0, 0.4, 1.0],
  );

  // Orange radial gradient for Player 2 area
  static const RadialGradient orangeRadial = RadialGradient(
    center: Alignment.center,
    radius: 0.8,
    colors: [
      Color(0xFFFF6B35),
      Color(0xFFCC4422),
      Color(0xFF000000),
    ],
    stops: [0.0, 0.4, 1.0],
  );

  // Shimmer effect gradient
  static const LinearGradient shimmer = LinearGradient(
    begin: Alignment.topLeft,
    end: Alignment.bottomRight,
    colors: [
      Color(0x00FFFFFF),
      Color(0x44FFFFFF),
      Color(0x00FFFFFF),
    ],
    stops: [0.0, 0.5, 1.0],
  );
}
```

---

## 4. 字体系统 Typography System

### 4.1 字体家族 Font Families

**主字体**: **Orbitron** (几何无衬线，科技感)
**次要字体**: **Roboto Mono** (等宽字体，用于数字)
**中文字体**: **Noto Sans SC** (思源黑体)

```dart
class FontFamilyTokens {
  static const String primary = 'Orbitron';
  static const String mono = 'RobotoMono';
  static const String chinese = 'NotoSansSC';
}
```

### 4.2 字体尺寸 Font Sizes

| Token Name | Size (sp) | Line Height | Usage |
|------------|-----------|-------------|-------|
| `display-xl` | 96 | 1.0 | 生命值超大数字 |
| `display-lg` | 72 | 1.0 | 生命值大数字 |
| `display-md` | 48 | 1.1 | 历史记录数字 |
| `headline-lg` | 32 | 1.2 | 页面标题 |
| `headline-md` | 24 | 1.2 | 区域标题 |
| `headline-sm` | 20 | 1.3 | 卡片标题 |
| `body-lg` | 18 | 1.5 | 强调正文 |
| `body-md` | 16 | 1.5 | 常规正文 |
| `body-sm` | 14 | 1.5 | 次要信息 |
| `caption` | 12 | 1.4 | 辅助文本 |
| `overline` | 10 | 1.4 | 标签、提示 |

### 4.3 字重 Font Weights

| Token Name | Weight | Usage |
|------------|--------|-------|
| `weight-light` | 300 | 次要信息 |
| `weight-regular` | 400 | 正文 |
| `weight-medium` | 500 | 按钮文本 |
| `weight-semibold` | 600 | 小标题 |
| `weight-bold` | 700 | 大标题、数字 |
| `weight-black` | 900 | 超大数字 |

### 4.4 TextStyle 定义

```dart
class TypographyTokens {
  // Life Points Display
  static const TextStyle lifePointsXL = TextStyle(
    fontFamily: 'RobotoMono',
    fontSize: 96,
    fontWeight: FontWeight.w900,
    height: 1.0,
    letterSpacing: -2.0,
  );

  static const TextStyle lifePointsLG = TextStyle(
    fontFamily: 'RobotoMono',
    fontSize: 72,
    fontWeight: FontWeight.w900,
    height: 1.0,
    letterSpacing: -1.5,
  );

  // Headings
  static const TextStyle headlineLG = TextStyle(
    fontFamily: 'Orbitron',
    fontSize: 32,
    fontWeight: FontWeight.w700,
    height: 1.2,
    letterSpacing: 0.5,
  );

  static const TextStyle headlineMD = TextStyle(
    fontFamily: 'Orbitron',
    fontSize: 24,
    fontWeight: FontWeight.w600,
    height: 1.2,
    letterSpacing: 0.25,
  );

  // Body text
  static const TextStyle bodyMD = TextStyle(
    fontFamily: 'NotoSansSC',
    fontSize: 16,
    fontWeight: FontWeight.w400,
    height: 1.5,
    letterSpacing: 0.15,
  );

  // Buttons
  static const TextStyle button = TextStyle(
    fontFamily: 'Orbitron',
    fontSize: 16,
    fontWeight: FontWeight.w600,
    height: 1.0,
    letterSpacing: 1.0,
  );
}
```

---

## 5. 间距系统 Spacing System

### 5.1 基础间距 Base Spacing

基础单位: **4px**

| Token Name | Value (px) | Usage |
|------------|------------|-------|
| `space-xxxs` | 2 | 最小间隔 |
| `space-xxs` | 4 | 紧密间隔 |
| `space-xs` | 8 | 小间隔 |
| `space-sm` | 12 | 较小间隔 |
| `space-md` | 16 | 中等间隔（默认） |
| `space-lg` | 24 | 较大间隔 |
| `space-xl` | 32 | 大间隔 |
| `space-xxl` | 48 | 超大间隔 |
| `space-xxxl` | 64 | 超超大间隔 |

### 5.2 布局间距 Layout Spacing

```dart
class SpacingTokens {
  static const double xxxs = 2.0;
  static const double xxs = 4.0;
  static const double xs = 8.0;
  static const double sm = 12.0;
  static const double md = 16.0;
  static const double lg = 24.0;
  static const double xl = 32.0;
  static const double xxl = 48.0;
  static const double xxxl = 64.0;

  // Safe area padding
  static const double safeAreaTop = 44.0; // iOS status bar
  static const double safeAreaBottom = 34.0; // iOS home indicator

  // Component-specific
  static const double buttonPaddingH = 24.0;
  static const double buttonPaddingV = 12.0;
  static const double cardPadding = 16.0;
  static const double listItemHeight = 72.0;
}
```

---

## 6. 阴影与发光 Shadows & Glows

### 6.1 标准阴影 Standard Shadows

```dart
class ShadowTokens {
  // Standard elevation shadows (not used much due to black bg)
  static const BoxShadow shadowSM = BoxShadow(
    color: Color(0x40000000),
    offset: Offset(0, 2),
    blurRadius: 4,
  );

  static const BoxShadow shadowMD = BoxShadow(
    color: Color(0x60000000),
    offset: Offset(0, 4),
    blurRadius: 8,
  );

  static const BoxShadow shadowLG = BoxShadow(
    color: Color(0x80000000),
    offset: Offset(0, 8),
    blurRadius: 16,
  );
}
```

### 6.2 发光效果 Glow Effects

```dart
class GlowTokens {
  // Blue glows for Player 1
  static const List<BoxShadow> blueGlowSoft = [
    BoxShadow(
      color: Color(0x4000D9FF),
      blurRadius: 20,
      spreadRadius: 0,
    ),
  ];

  static const List<BoxShadow> blueGlowMedium = [
    BoxShadow(
      color: Color(0x6000D9FF),
      blurRadius: 30,
      spreadRadius: 2,
    ),
    BoxShadow(
      color: Color(0x3000D9FF),
      blurRadius: 60,
      spreadRadius: 4,
    ),
  ];

  static const List<BoxShadow> blueGlowStrong = [
    BoxShadow(
      color: Color(0x8000D9FF),
      blurRadius: 40,
      spreadRadius: 4,
    ),
    BoxShadow(
      color: Color(0x5000D9FF),
      blurRadius: 80,
      spreadRadius: 8,
    ),
    BoxShadow(
      color: Color(0x2000D9FF),
      blurRadius: 120,
      spreadRadius: 12,
    ),
  ];

  // Orange glows for Player 2
  static const List<BoxShadow> orangeGlowSoft = [
    BoxShadow(
      color: Color(0x40FF6B35),
      blurRadius: 20,
      spreadRadius: 0,
    ),
  ];

  static const List<BoxShadow> orangeGlowMedium = [
    BoxShadow(
      color: Color(0x60FF6B35),
      blurRadius: 30,
      spreadRadius: 2,
    ),
    BoxShadow(
      color: Color(0x30FF6B35),
      blurRadius: 60,
      spreadRadius: 4,
    ),
  ];

  static const List<BoxShadow> orangeGlowStrong = [
    BoxShadow(
      color: Color(0x80FF6B35),
      blurRadius: 40,
      spreadRadius: 4,
    ),
    BoxShadow(
      color: Color(0x50FF6B35),
      blurRadius: 80,
      spreadRadius: 8,
    ),
    BoxShadow(
      color: Color(0x20FF6B35),
      blurRadius: 120,
      spreadRadius: 12,
    ),
  ];

  // White glow for neutral elements
  static const List<BoxShadow> whiteGlow = [
    BoxShadow(
      color: Color(0x40FFFFFF),
      blurRadius: 20,
      spreadRadius: 0,
    ),
  ];
}
```

### 6.3 呼吸灯效果 Breathing Light

```dart
class BreathingLightEffect {
  // Breathing animation for glows
  // Min opacity: 0.3, Max opacity: 1.0
  // Duration: 2-3 seconds per cycle

  static double getBreathingOpacity(double time) {
    // Using sine wave for smooth breathing
    return 0.3 + 0.7 * (0.5 + 0.5 * sin(time * 2 * pi / 2.5));
  }

  static Color applyBreathing(Color baseColor, double time) {
    final opacity = getBreathingOpacity(time);
    return baseColor.withOpacity(opacity);
  }
}
```

---

## 7. 动画系统 Animation System

### 7.1 动画时长 Duration

| Token Name | Duration (ms) | Usage |
|------------|---------------|-------|
| `duration-instant` | 0 | 无动画 |
| `duration-fast` | 150 | 快速响应 |
| `duration-normal` | 300 | 标准动画 |
| `duration-slow` | 500 | 慢速动画 |
| `duration-slower` | 800 | 更慢动画 |
| `duration-breathing` | 2500 | 呼吸灯周期 |

### 7.2 缓动曲线 Easing Curves

```dart
class AnimationTokens {
  // Durations
  static const Duration durationInstant = Duration.zero;
  static const Duration durationFast = Duration(milliseconds: 150);
  static const Duration durationNormal = Duration(milliseconds: 300);
  static const Duration durationSlow = Duration(milliseconds: 500);
  static const Duration durationSlower = Duration(milliseconds: 800);
  static const Duration durationBreathing = Duration(milliseconds: 2500);

  // Curves
  static const Curve easeIn = Curves.easeIn;
  static const Curve easeOut = Curves.easeOut;
  static const Curve easeInOut = Curves.easeInOut;
  static const Curve easeOutCubic = Curves.easeOutCubic;
  static const Curve elasticOut = Curves.elasticOut;
  static const Curve bounceOut = Curves.bounceOut;

  // Custom curves for specific effects
  static const Curve lifePointsChange = Curves.easeOutCubic;
  static const Curve particleBurst = Curves.easeOut;
  static const Curve glowPulse = Curves.easeInOut;
}
```

### 7.3 动画模式 Animation Patterns

#### **生命值变化动画**
```dart
// 1. 数字滚动 (300ms, easeOutCubic)
// 2. 发光脉冲 (300ms, easeInOut)
// 3. 粒子爆发 (500ms, easeOut)
// 4. 震动反馈 (50ms, 轻震动)

AnimationSequence lifePointsChangeSequence = [
  AnimationStep(
    duration: 300,
    curve: Curves.easeOutCubic,
    property: 'number',
  ),
  AnimationStep(
    duration: 300,
    curve: Curves.easeInOut,
    property: 'glow',
    delay: 0, // Parallel with number
  ),
  AnimationStep(
    duration: 500,
    curve: Curves.easeOut,
    property: 'particles',
    delay: 0, // Parallel with others
  ),
];
```

#### **页面转场动画**
```dart
// Fade + Slide transition
PageTransition fadeSlide = PageTransition(
  type: PageTransitionType.fadeSlide,
  duration: Duration(milliseconds: 300),
  curve: Curves.easeInOut,
  fadeInCurve: Curves.easeIn,
  fadeOutCurve: Curves.easeOut,
);
```

#### **骰子投掷动画**
```dart
// 1. 3D 旋转 (1000ms, elasticOut)
// 2. 反弹落地 (500ms, bounceOut)
// 3. 发光闪烁 (300ms, easeInOut)

AnimationSequence diceRollSequence = [
  AnimationStep(
    duration: 1000,
    curve: Curves.elasticOut,
    property: 'rotation3D',
  ),
  AnimationStep(
    duration: 500,
    curve: Curves.bounceOut,
    property: 'bounce',
    delay: 800, // Start before rotation ends
  ),
  AnimationStep(
    duration: 300,
    curve: Curves.easeInOut,
    property: 'glow',
    delay: 1200, // After landing
  ),
];
```

---

## 8. 组件库 Component Library

### 8.1 按钮 Buttons

#### **Primary Button (发光按钮)**

```dart
class GlowButton extends StatelessWidget {
  final String text;
  final VoidCallback onPressed;
  final Color glowColor;
  final bool enabled;

  @override
  Widget build(BuildContext context) {
    return Container(
      decoration: BoxDecoration(
        color: Colors.transparent,
        borderRadius: BorderRadius.circular(8),
        border: Border.all(
          color: glowColor.withOpacity(0.6),
          width: 2,
        ),
        boxShadow: GlowTokens.blueGlowMedium,
      ),
      child: Material(
        color: Colors.transparent,
        child: InkWell(
          onTap: enabled ? onPressed : null,
          borderRadius: BorderRadius.circular(8),
          splashColor: glowColor.withOpacity(0.3),
          child: Padding(
            padding: EdgeInsets.symmetric(
              horizontal: SpacingTokens.buttonPaddingH,
              vertical: SpacingTokens.buttonPaddingV,
            ),
            child: Text(
              text,
              style: TypographyTokens.button.copyWith(
                color: enabled ? glowColor : glowColor.withOpacity(0.3),
              ),
            ),
          ),
        ),
      ),
    );
  }
}
```

**规格**:
- 最小宽度: 88px
- 高度: 48px
- 边框: 2px, 60% opacity
- 发光: medium glow
- 圆角: 8px

#### **Icon Button (图标按钮)**

```dart
class GlowIconButton extends StatelessWidget {
  final IconData icon;
  final VoidCallback onPressed;
  final Color glowColor;
  final double size;

  @override
  Widget build(BuildContext context) {
    return Container(
      width: size,
      height: size,
      decoration: BoxDecoration(
        shape: BoxShape.circle,
        border: Border.all(
          color: glowColor.withOpacity(0.6),
          width: 2,
        ),
        boxShadow: GlowTokens.whiteGlow,
      ),
      child: Material(
        color: Colors.transparent,
        shape: CircleBorder(),
        child: InkWell(
          onTap: onPressed,
          customBorder: CircleBorder(),
          splashColor: glowColor.withOpacity(0.3),
          child: Icon(
            icon,
            color: glowColor,
            size: size * 0.5,
          ),
        ),
      ),
    );
  }
}
```

**规格**:
- 尺寸: 48px (默认), 40px (小), 56px (大)
- 边框: 2px, 60% opacity
- 图标大小: 容器的 50%

### 8.2 卡片 Cards

#### **Glow Card (发光卡片)**

```dart
class GlowCard extends StatelessWidget {
  final Widget child;
  final Color borderColor;
  final EdgeInsets padding;

  @override
  Widget build(BuildContext context) {
    return Container(
      decoration: BoxDecoration(
        color: Color(0x0FFFFFFF), // 6% white overlay
        borderRadius: BorderRadius.circular(12),
        border: Border.all(
          color: borderColor.withOpacity(0.4),
          width: 1,
        ),
        boxShadow: GlowTokens.blueGlowSoft,
      ),
      child: Padding(
        padding: padding,
        child: child,
      ),
    );
  }
}
```

**规格**:
- 背景: 6% white overlay on black
- 边框: 1px, 40% opacity
- 圆角: 12px
- 内边距: 16px (默认)
- 发光: soft glow

### 8.3 输入框 Input Fields

#### **Glow Text Field**

```dart
class GlowTextField extends StatelessWidget {
  final String label;
  final String hint;
  final TextEditingController controller;
  final Color glowColor;

  @override
  Widget build(BuildContext context) {
    return TextField(
      controller: controller,
      style: TypographyTokens.bodyMD.copyWith(color: Colors.white),
      decoration: InputDecoration(
        labelText: label,
        hintText: hint,
        labelStyle: TypographyTokens.bodyMD.copyWith(
          color: glowColor.withOpacity(0.6),
        ),
        hintStyle: TypographyTokens.bodyMD.copyWith(
          color: Colors.white.withOpacity(0.3),
        ),
        enabledBorder: OutlineInputBorder(
          borderRadius: BorderRadius.circular(8),
          borderSide: BorderSide(
            color: glowColor.withOpacity(0.4),
            width: 1,
          ),
        ),
        focusedBorder: OutlineInputBorder(
          borderRadius: BorderRadius.circular(8),
          borderSide: BorderSide(
            color: glowColor,
            width: 2,
          ),
        ),
        filled: true,
        fillColor: Color(0x0FFFFFFF),
      ),
    );
  }
}
```

### 8.4 对话框 Dialogs

#### **Glow Dialog**

```dart
class GlowDialog extends StatelessWidget {
  final String title;
  final Widget content;
  final List<Widget> actions;

  @override
  Widget build(BuildContext context) {
    return Dialog(
      backgroundColor: Colors.black,
      shape: RoundedRectangleBorder(
        borderRadius: BorderRadius.circular(16),
        side: BorderSide(
          color: Color(0xFF00D9FF).withOpacity(0.6),
          width: 2,
        ),
      ),
      child: Container(
        decoration: BoxDecoration(
          borderRadius: BorderRadius.circular(16),
          boxShadow: GlowTokens.blueGlowStrong,
        ),
        padding: EdgeInsets.all(24),
        child: Column(
          mainAxisSize: MainAxisSize.min,
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              title,
              style: TypographyTokens.headlineMD.copyWith(
                color: Color(0xFF00D9FF),
              ),
            ),
            SizedBox(height: SpacingTokens.lg),
            content,
            SizedBox(height: SpacingTokens.xl),
            Row(
              mainAxisAlignment: MainAxisAlignment.end,
              children: actions,
            ),
          ],
        ),
      ),
    );
  }
}
```

### 8.5 滑动条 Sliders

#### **Glow Slider (未来功能)**

```dart
class GlowSlider extends StatelessWidget {
  final double value;
  final ValueChanged<double> onChanged;
  final Color glowColor;
  final double min;
  final double max;

  @override
  Widget build(BuildContext context) {
    return SliderTheme(
      data: SliderThemeData(
        activeTrackColor: glowColor,
        inactiveTrackColor: glowColor.withOpacity(0.2),
        thumbColor: glowColor,
        overlayColor: glowColor.withOpacity(0.2),
        trackHeight: 4,
        thumbShape: RoundSliderThumbShape(
          enabledThumbRadius: 8,
          elevation: 4,
          pressedElevation: 8,
        ),
      ),
      child: Slider(
        value: value,
        min: min,
        max: max,
        onChanged: onChanged,
      ),
    );
  }
}
```

### 8.6 分隔线 Dividers

```dart
class GlowDivider extends StatelessWidget {
  final Color color;
  final double height;
  final double thickness;

  @override
  Widget build(BuildContext context) {
    return Container(
      height: height,
      child: Center(
        child: Container(
          height: thickness,
          decoration: BoxDecoration(
            gradient: LinearGradient(
              colors: [
                Colors.transparent,
                color.withOpacity(0.6),
                Colors.transparent,
              ],
            ),
            boxShadow: [
              BoxShadow(
                color: color.withOpacity(0.4),
                blurRadius: 8,
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

---

## 9. 图标系统 Icon System

### 9.1 图标风格

- **风格**: 线性图标 (Line icons)
- **粗细**: 2px stroke
- **圆角**: 圆角处理
- **发光**: 所有图标应带有颜色对应的发光效果

### 9.2 核心图标

| 图标名称 | 描述 | 使用场景 |
|---------|------|---------|
| `icon_plus` | 加号 | 增加生命值 |
| `icon_minus` | 减号 | 减少生命值 |
| `icon_dice` | 骰子 | 投掷骰子功能 |
| `icon_coin` | 硬币 | 投掷硬币功能 |
| `icon_history` | 历史记录 | 查看对战记录 |
| `icon_settings` | 设置 | 打开设置页面 |
| `icon_bluetooth` | 蓝牙 | 蓝牙对战 |
| `icon_info` | 信息 | 帮助信息 |
| `icon_reset` | 重置 | 重新开始对战 |
| `icon_undo` | 撤销 | 撤销操作 |
| `icon_volume` | 音量 | 音效设置 |
| `icon_vibrate` | 震动 | 震动设置 |
| `icon_close` | 关闭 | 关闭弹窗 |
| `icon_check` | 确认 | 确认操作 |
| `icon_arrow_back` | 返回箭头 | 返回上一页 |
| `icon_menu` | 菜单 | 打开菜单 |

### 9.3 图标尺寸

| Token Name | Size (px) | Usage |
|------------|-----------|-------|
| `icon-xs` | 16 | 极小图标 |
| `icon-sm` | 20 | 小图标 |
| `icon-md` | 24 | 标准图标 |
| `icon-lg` | 32 | 大图标 |
| `icon-xl` | 48 | 超大图标 |

### 9.4 自定义图标组件

```dart
class GlowIcon extends StatelessWidget {
  final IconData icon;
  final Color color;
  final double size;
  final bool withGlow;

  @override
  Widget build(BuildContext context) {
    return Container(
      decoration: withGlow ? BoxDecoration(
        boxShadow: [
          BoxShadow(
            color: color.withOpacity(0.6),
            blurRadius: size * 0.5,
          ),
        ],
      ) : null,
      child: Icon(
        icon,
        color: color,
        size: size,
      ),
    );
  }
}
```

---

## 10. 布局规范 Layout Specifications

### 10.1 屏幕布局结构

#### **对战主界面 (Battle Screen)**

```
┌─────────────────────────────────┐
│       Player 2 Area (上半)       │ <- 50% 高度
│                                 │
│     [生命值] [粒子效果]            │
│                                 │
├─────────────────────────────────┤ <- 中央分隔线
│                                 │
│     [生命值] [粒子效果]            │
│                                 │
│       Player 1 Area (下半)       │ <- 50% 高度
└─────────────────────────────────┘
```

**布局代码**:
```dart
class BattleScreenLayout extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final screenHeight = MediaQuery.of(context).size.height;
    final playerAreaHeight = screenHeight / 2;

    return Column(
      children: [
        // Player 2 Area (inverted)
        Transform.rotate(
          angle: pi, // 180° rotation
          child: PlayerArea(
            playerId: 2,
            height: playerAreaHeight,
            color: DesignTokens.colors.orangePrimary,
          ),
        ),

        // Center Divider
        GlowDivider(
          color: Colors.white,
          height: 2,
          thickness: 1,
        ),

        // Player 1 Area
        PlayerArea(
          playerId: 1,
          height: playerAreaHeight,
          color: DesignTokens.colors.bluePrimary,
        ),
      ],
    );
  }
}
```

### 10.2 玩家区域布局

```
┌─────────────────────────────────┐
│  [设置按钮]      [历史按钮]        │ <- Top Bar (56px)
│                                 │
│                                 │
│        [生命值显示区]              │ <- Center (Flex)
│          8000                   │
│                                 │
│  [骰子] [硬币] [撤销] [重置]        │ <- Bottom Toolbar (72px)
└─────────────────────────────────┘
```

### 10.3 响应式断点 Breakpoints

| 设备类型 | 宽度范围 | 布局调整 |
|---------|---------|---------|
| 小屏手机 | < 360px | 减小字体，隐藏次要功能 |
| 标准手机 | 360-414px | 标准布局 |
| 大屏手机 | 414-768px | 增大字体和间距 |
| 平板 | > 768px | 横向布局，并排显示 |

### 10.4 安全区域 Safe Area

```dart
class SafeAreaLayout extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return SafeArea(
      minimum: EdgeInsets.only(
        top: SpacingTokens.safeAreaTop,
        bottom: SpacingTokens.safeAreaBottom,
      ),
      child: child,
    );
  }
}
```

---

## 11. 交互规范 Interaction Guidelines

### 11.1 手势交互

#### **滑动调整生命值**

| 手势 | 方向 | 效果 | 反馈 |
|-----|------|------|------|
| 短滑动 | 上/下 | ±100 | 轻震动 + 少量粒子 |
| 中滑动 | 上/下 | ±500 | 中等震动 + 中量粒子 |
| 长滑动 | 上/下 | ±1000 | 强震动 + 大量粒子 |
| 快速滑动 | 上/下 | ±2000 | 强震动 + 粒子爆发 |

**计算公式**:
```dart
int calculateDelta(double velocity, double distance) {
  // velocity: pixels/second
  // distance: pixels

  final speedFactor = velocity.abs() / 1000.0; // Normalize
  final distanceFactor = distance.abs() / 100.0;

  final magnitude = (speedFactor * 0.7 + distanceFactor * 0.3);

  if (magnitude < 0.5) return 100;
  if (magnitude < 1.5) return 500;
  if (magnitude < 3.0) return 1000;
  return 2000;
}
```

#### **点击交互**

| 元素 | 点击效果 | 反馈 |
|-----|---------|------|
| 按钮 | 发光脉冲 + 水波纹 | 轻震动 + 音效 |
| 生命值 | 显示调整菜单 | 轻震动 |
| 骰子/硬币 | 投掷动画 | 中等震动 + 音效 |

#### **长按交互**

| 元素 | 长按效果 | 反馈 |
|-----|---------|------|
| 生命值 | 重置为初始值 (8000) | 确认对话框 |
| 历史记录项 | 删除记录 | 确认对话框 |

### 11.2 触觉反馈 Haptic Feedback

```dart
class HapticFeedbackTokens {
  static void light() {
    HapticFeedback.lightImpact();
  }

  static void medium() {
    HapticFeedback.mediumImpact();
  }

  static void heavy() {
    HapticFeedback.heavyImpact();
  }

  static void selection() {
    HapticFeedback.selectionClick();
  }
}
```

### 11.3 音效系统 Sound Effects

| 交互 | 音效 | 音量 | 说明 |
|-----|------|------|------|
| 生命值增加 | `sfx_life_up.wav` | 70% | 上升音效 |
| 生命值减少 | `sfx_life_down.wav` | 70% | 下降音效 |
| 投掷骰子 | `sfx_dice_roll.wav` | 80% | 骰子滚动 |
| 投掷硬币 | `sfx_coin_flip.wav` | 80% | 硬币旋转 |
| 按钮点击 | `sfx_button_click.wav` | 60% | 轻点音效 |
| 撤销操作 | `sfx_undo.wav` | 60% | 撤销提示音 |
| 对战开始 | `sfx_battle_start.wav` | 90% | 宣告音效 |
| 生命值归零 | `sfx_defeat.wav` | 100% | 失败音效 |

---

## 12. 资源组织 Asset Organization

### 12.1 目录结构

```
assets/
├── images/
│   ├── icons/
│   │   ├── icon_plus.svg
│   │   ├── icon_minus.svg
│   │   ├── icon_dice.svg
│   │   ├── icon_coin.svg
│   │   └── ...
│   ├── backgrounds/
│   │   └── grid_pattern.png
│   └── splash/
│       └── app_icon.png
│
├── sounds/
│   ├── sfx_life_up.wav
│   ├── sfx_life_down.wav
│   ├── sfx_dice_roll.wav
│   ├── sfx_coin_flip.wav
│   └── ...
│
├── fonts/
│   ├── Orbitron/
│   │   ├── Orbitron-Regular.ttf
│   │   ├── Orbitron-Bold.ttf
│   │   └── Orbitron-Black.ttf
│   ├── RobotoMono/
│   │   ├── RobotoMono-Regular.ttf
│   │   └── RobotoMono-Bold.ttf
│   └── NotoSansSC/
│       ├── NotoSansSC-Regular.otf
│       └── NotoSansSC-Bold.otf
│
└── animations/
    └── lottie/
        ├── dice_roll.json
        └── particle_burst.json
```

### 12.2 命名规范

#### **图标命名**
- 格式: `icon_<name>_<variant>.svg`
- 示例: `icon_settings_filled.svg`, `icon_dice_outline.svg`

#### **音效命名**
- 格式: `sfx_<action>_<variant>.wav`
- 示例: `sfx_button_click_primary.wav`

#### **图片命名**
- 格式: `<category>_<description>_<size>.png`
- 示例: `bg_grid_pattern_2x.png`

### 12.3 图片资源规格

| 资源类型 | 格式 | 分辨率 | 说明 |
|---------|------|--------|------|
| 应用图标 | PNG | 1024x1024 | iOS/Android |
| SVG 图标 | SVG | 24x24 viewBox | 矢量图标 |
| 背景纹理 | PNG | 1080x2340 | @2x, @3x |
| 粒子贴图 | PNG | 32x32 | 半透明 |

### 12.4 pubspec.yaml 配置

```yaml
flutter:
  assets:
    - assets/images/icons/
    - assets/images/backgrounds/
    - assets/sounds/
    - assets/animations/lottie/

  fonts:
    - family: Orbitron
      fonts:
        - asset: assets/fonts/Orbitron/Orbitron-Regular.ttf
        - asset: assets/fonts/Orbitron/Orbitron-Bold.ttf
          weight: 700
        - asset: assets/fonts/Orbitron/Orbitron-Black.ttf
          weight: 900

    - family: RobotoMono
      fonts:
        - asset: assets/fonts/RobotoMono/RobotoMono-Regular.ttf
        - asset: assets/fonts/RobotoMono/RobotoMono-Bold.ttf
          weight: 700

    - family: NotoSansSC
      fonts:
        - asset: assets/fonts/NotoSansSC/NotoSansSC-Regular.otf
        - asset: assets/fonts/NotoSansSC/NotoSansSC-Bold.otf
          weight: 700
```

---

## 附录 A: 设计交付清单

### A.1 设计文件

- [ ] Figma 主设计文件 (包含所有页面和组件)
- [ ] 设计系统组件库 (可复用组件)
- [ ] 颜色样式库 (Color Styles)
- [ ] 文本样式库 (Text Styles)
- [ ] 效果样式库 (Effect Styles - 发光和阴影)

### A.2 导出资源

- [ ] 所有 SVG 图标 (24x24)
- [ ] 应用图标 PNG (1024x1024)
- [ ] 背景纹理 PNG (@2x, @3x)
- [ ] 粒子贴图 PNG (32x32)
- [ ] Lottie 动画 JSON (骰子、硬币)

### A.3 文档

- [ ] 设计系统规范文档 (本文档)
- [ ] 组件使用指南
- [ ] 动画规范文档
- [ ] 交互流程图
- [ ] 标注说明文档

### A.4 原型

- [ ] Figma 交互原型 (所有核心流程)
- [ ] 动效演示视频 (生命值变化、粒子效果)
- [ ] 手势交互演示

---

## 附录 B: 开发者交接说明

### B.1 设计令牌使用

所有设计令牌已在代码示例中定义，开发时请使用：

```dart
// ✅ 正确做法
Container(
  padding: EdgeInsets.all(SpacingTokens.md),
  decoration: BoxDecoration(
    color: ColorTokens.primaryBlue,
    boxShadow: GlowTokens.blueGlowMedium,
  ),
)

// ❌ 错误做法
Container(
  padding: EdgeInsets.all(16), // 硬编码
  decoration: BoxDecoration(
    color: Color(0xFF00D9FF), // 硬编码
  ),
)
```

### B.2 组件复用

所有自定义组件 (GlowButton, GlowCard, etc.) 应该：
1. 放在 `lib/core/widgets/` 目录
2. 使用设计令牌而非硬编码值
3. 支持主题色切换 (蓝/橙)
4. 包含完整的文档注释

### B.3 性能优化建议

1. **粒子系统**: 使用对象池 (Object Pool) 复用粒子对象
2. **发光效果**: 使用 `RepaintBoundary` 隔离重绘区域
3. **动画**: 使用 `AnimatedBuilder` 而非 `setState` 全局刷新
4. **Canvas 绘制**: 缓存不变的绘制对象 (`Paint`, `Path`)

### B.4 测试检查项

- [ ] 所有动画 60fps 流畅运行
- [ ] 触觉反馈在所有设备正常工作
- [ ] 音效在静音模式下不播放
- [ ] 发光效果在低端设备可降级
- [ ] 所有文本在小屏设备不截断
- [ ] Safe Area 在刘海屏设备正确显示

---

## 附录 C: 设计工具与资源

### C.1 推荐设计工具

- **Figma**: 主设计工具，用于 UI 设计和原型制作
- **Principle**: 用于复杂动效演示
- **After Effects**: 用于粒子效果参考
- **Rive**: 用于交互动画 (可选替代 Lottie)

### C.2 字体资源

- **Orbitron**: [Google Fonts](https://fonts.google.com/specimen/Orbitron)
- **Roboto Mono**: [Google Fonts](https://fonts.google.com/specimen/Roboto+Mono)
- **Noto Sans SC**: [Google Fonts](https://fonts.google.com/specimen/Noto+Sans+SC)

### C.3 图标资源

- **Heroicons**: 高质量线性图标
- **Phosphor Icons**: 科技感图标集
- **Feather Icons**: 简约线性图标

### C.4 参考作品

- **Tron: Legacy** 电影视觉设计
- **Cyberpunk 2077** 游戏 UI
- **Hyper Light Drifter** 像素发光效果
- **Monument Valley** 几何美学

---

## 变更记录 Change Log

| 版本 | 日期 | 变更内容 | 作者 |
|------|------|---------|------|
| v1.0.0 | 2025-11-13 | 初始版本，完整设计系统规范 | UX Designer |

---

## 结语

这份设计系统规范是 BMAD2 项目视觉和交互的核心指南。所有设计决策都围绕 **"视觉冲击 + 沉浸体验"** 的核心价值展开。

在实际开发过程中，如果遇到规范未覆盖的场景，请遵循以下原则：
1. **保持一致性**: 参考现有组件和模式
2. **视觉优先**: 优先考虑视觉效果的震撼性
3. **性能平衡**: 在视觉效果和性能之间找到最佳平衡点
4. **用户体验**: 始终以用户的使用感受为第一优先级

如有疑问或需要补充，请随时更新本文档。

---

**文档版本**: v1.0.0
**最后更新**: 2025-11-13
**维护者**: BMAD2 UX Team
