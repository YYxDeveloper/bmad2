# 技术架构详细规划

**专案名称：** bmad2 (Yu-Gi-Oh Duel Console)
**文档类型：** 技术架构设计
**日期：** 2025-11-13
**作者：** yyx
**版本：** 1.0

---

## 📋 概要

本文档详细说明 Yu-Gi-Oh Duel Console 的技术架构设计，包括核心系统、目录结构、性能优化策略和实现细节。

---

## 🏗️ 平台选择

### Flutter

**核心优势：**
- ✅ 60fps 稳定效能
- ✅ 原生实现（无复杂依赖）
- ✅ 7-9 週开发周期
- ✅ 跨平台完美支援（iOS + Android）

**技术栈版本：**
- Flutter: 3.35.7+
- Dart: 3.9.2+
- 最低 iOS: 12.0
- 最低 Android: API 21 (Android 5.0)

---

## 🎨 核心系统架构（3 大系统）

### 1. 渲染引擎（Rendering Core）

```
渲染引擎
├─ CustomPainter（主绘制器）
│  ├─ 呼吸光流绘制
│  ├─ 粒子系统渲染
│  └─ 节拍波纹渲染
│
├─ 粒子系统管理器
│  ├─ 对象池（减少 GC）
│  ├─ 空间分区优化
│  └─ 批次绘制（减少 draw calls）
│
├─ 光流路径生成器
│  ├─ 贝塞尔曲线计算
│  ├─ 呼吸曲线生成（sin/cos）
│  └─ 路径缓存优化
│
└─ 节拍波纹生成器
   ├─ 波纹扩散算法
   └─ 节奏条视觉化
```

**关键技术点：**
- **CustomPainter** - Flutter 原生高性能绘制
- **Canvas API** - 直接操作画布
- **Path + Gradient** - 光流效果实现
- **批次渲染** - 500-1000 粒子在 60fps

**实现文件：**
- `lib/features/duel/presentation/painters/breathing_light_painter.dart`
- `lib/features/duel/presentation/painters/particle_painter.dart`
- `lib/features/duel/presentation/painters/beat_wave_painter.dart`

---

### 2. 动画调度器（Animation Orchestrator）

```
动画调度器
├─ 单一 Ticker（统一时间源）
│  └─ 避免多个 AnimationController
│
├─ 呼吸曲线生成
│  ├─ 吸气阶段（0.3-0.5秒）
│  └─ 呼气阶段（0.5-0.8秒）
│
├─ 过渡动画管理
│  ├─ 解构（0.1s）
│  ├─ 过渡（0.1s）
│  └─ 重构（0.1s）
│
└─ 60fps 保证机制
   ├─ RepaintBoundary（隔离重绘）
   └─ 条件渲染（只绘制可见元素）
```

**关键技术点：**
- **单一 AnimationController** - 统一动画时间线
- **Tween + Curve** - 流畅动画插值
- **RepaintBoundary** - 性能优化

**呼吸节奏设计：**

```
吸气阶段（0.3-0.5秒）：
├─ 光线从触摸点向内收缩聚集
├─ 亮度逐渐增强
└─ 能量凝聚

呼气阶段（0.5-0.8秒）：
├─ 光线向外扩散释放
├─ 沿路径流向生命值显示区
└─ 柔和融入目标区域

生命值状态呼吸：
├─ 高生命值（6000-8000）：深沉缓慢（2-3秒/周期）
├─ 中等生命值（2000-6000）：正常节奏（1.5-2秒/周期）
├─ 低生命值（500-2000）：急促呼吸（1秒/周期）
└─ 瀕死（<500）：不规则、挣扎的呼吸
```

---

### 3. 状态管理器（State Manager）

```
状态管理器（Provider）
├─ 生命值状态（LifePointsProvider）
│  ├─ 双人生命值追踪
│  ├─ 历史记录管理
│  └─ 生命值变化事件
│
├─ 粒子状态（ParticleSystemProvider）
│  ├─ 粒子池管理
│  └─ 粒子密度计算
│
├─ 蓝牙连接状态（BluetoothProvider）
│  ├─ 设备配对
│  ├─ 连接管理
│  └─ 数据同步
│
└─ UI 模式状态（UIStateProvider）
   ├─ 单机 vs 蓝牙模式
   ├─ 精确 vs 感知模式
   └─ 布局状态管理
```

**关键技术点：**
- **Provider** - 轻量级状态管理
- **ChangeNotifier** - 响应式更新
- **Consumer** - 精确重建

**实现文件：**
- `lib/features/duel/providers/life_points_provider.dart`
- `lib/features/duel/providers/particle_system_provider.dart`
- `lib/features/bluetooth/providers/bluetooth_provider.dart`
- `lib/core/providers/ui_state_provider.dart`

---

## 📁 目录结构设计

```
bmad2/
├─ docs/                          # 文档
│  ├─ product-brief-bmad2-2025-11-13.md
│  ├─ bmm-brainstorming-session-2025-11-13.md
│  └─ technical-architecture-bmad2-2025-11-13.md
│
└─ app/                           # Flutter 应用
   ├─ lib/
   │  ├─ main.dart                # 应用入口
   │  │
   │  ├─ core/                    # 核心基础设施
   │  │  ├─ constants/            # 常量定义
   │  │  │  ├─ colors.dart        # 蓝橘色系定义
   │  │  │  ├─ animations.dart    # 动画参数常量
   │  │  │  └─ game_rules.dart    # 游戏规则常量
   │  │  │
   │  │  ├─ theme/                # 主题系统
   │  │  │  ├─ app_theme.dart     # 应用主题
   │  │  │  └─ tron_colors.dart   # 创：光速战记色系
   │  │  │
   │  │  └─ utils/                # 工具函数
   │  │     ├─ math_utils.dart    # 数学计算（贝塞尔曲线等）
   │  │     └─ gesture_utils.dart # 手势计算
   │  │
   │  ├─ features/                # 功能模块（Feature-First Architecture）
   │  │  ├─ duel/                 # 决斗核心功能
   │  │  │  ├─ presentation/
   │  │  │  │  ├─ screens/
   │  │  │  │  │  └─ duel_screen.dart
   │  │  │  │  ├─ widgets/
   │  │  │  │  │  ├─ life_points_display.dart
   │  │  │  │  │  └─ swipe_input_area.dart
   │  │  │  │  └─ painters/
   │  │  │  │     ├─ breathing_light_painter.dart
   │  │  │  │     ├─ particle_painter.dart
   │  │  │  │     └─ beat_wave_painter.dart
   │  │  │  ├─ providers/
   │  │  │  │  ├─ life_points_provider.dart
   │  │  │  │  └─ particle_system_provider.dart
   │  │  │  └─ models/
   │  │  │     ├─ player.dart
   │  │  │     └─ particle.dart
   │  │  │
   │  │  ├─ dice/                 # 骰子功能（Phase 3）
   │  │  │  ├─ presentation/
   │  │  │  │  └─ widgets/
   │  │  │  │     └─ dice_widget.dart
   │  │  │  ├─ providers/
   │  │  │  │  └─ dice_provider.dart
   │  │  │  └─ models/
   │  │  │     └─ dice_result.dart
   │  │  │
   │  │  ├─ counter/              # 计数器功能（Phase 3）
   │  │  │  ├─ presentation/
   │  │  │  │  └─ widgets/
   │  │  │  │     └─ counter_widget.dart
   │  │  │  ├─ providers/
   │  │  │  │  └─ counter_provider.dart
   │  │  │  └─ models/
   │  │  │     └─ counter.dart
   │  │  │
   │  │  └─ bluetooth/            # 蓝牙对战（Phase 4）
   │  │     ├─ presentation/
   │  │     │  └─ screens/
   │  │     │     └─ bluetooth_pairing_screen.dart
   │  │     ├─ providers/
   │  │     │  └─ bluetooth_provider.dart
   │  │     └─ models/
   │  │        └─ bluetooth_device.dart
   │  │
   │  └─ shared/                  # 共享组件
   │     ├─ widgets/              # 通用组件
   │     │  ├─ tron_button.dart
   │     │  └─ glow_container.dart
   │     └─ animations/           # 共享动画
   │        ├─ transition_animations.dart
   │        └─ glow_animations.dart
   │
   ├─ test/                       # 测试文件
   ├─ android/                    # Android 平台代码
   ├─ ios/                        # iOS 平台代码
   └─ pubspec.yaml                # 依赖配置
```

---

## ⚡ 性能优化策略

### 目标指标

```
✅ 帧率：稳定 60fps（16.67ms/帧）
✅ 启动时间：<1秒
✅ 内存使用：<100MB
✅ 崩溃率：<0.5%
```

### 优化方法

#### 1. 渲染优化

**单一 AnimationController：**
```dart
class DuelScreen extends StatefulWidget {
  @override
  _DuelScreenState createState() => _DuelScreenState();
}

class _DuelScreenState extends State<DuelScreen>
    with SingleTickerProviderStateMixin {
  late AnimationController _controller;

  @override
  void initState() {
    super.initState();
    // 单一 Ticker，避免多个 AnimationController
    _controller = AnimationController(
      duration: Duration(milliseconds: 16), // 60fps
      vsync: this,
    )..repeat();
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }
}
```

**批次绘制粒子：**
```dart
class ParticlePainter extends CustomPainter {
  final List<Particle> particles;

  ParticlePainter(this.particles);

  @override
  void paint(Canvas canvas, Size size) {
    // 批次绘制，减少 draw calls
    final paint = Paint()
      ..blendMode = BlendMode.plus
      ..style = PaintingStyle.fill;

    for (var particle in particles) {
      paint.color = particle.color.withOpacity(particle.alpha);
      canvas.drawCircle(
        particle.position,
        particle.size,
        paint,
      );
    }
  }

  @override
  bool shouldRepaint(ParticlePainter oldDelegate) => true;
}
```

#### 2. 对象池模式

**粒子对象池：**
```dart
class ParticlePool {
  final List<Particle> _pool = [];
  final int maxSize = 1000;

  Particle acquire() {
    if (_pool.isEmpty) {
      return Particle();
    }
    return _pool.removeLast();
  }

  void release(Particle particle) {
    if (_pool.length < maxSize) {
      particle.reset();
      _pool.add(particle);
    }
  }

  void clear() {
    _pool.clear();
  }
}
```

**使用示例：**
```dart
class ParticleSystemProvider extends ChangeNotifier {
  final ParticlePool _pool = ParticlePool();
  final List<Particle> _activeParticles = [];

  void spawnParticle(Offset position, Color color) {
    final particle = _pool.acquire();
    particle.initialize(position, color);
    _activeParticles.add(particle);
  }

  void update(double dt) {
    _activeParticles.removeWhere((particle) {
      particle.update(dt);
      if (particle.isDead) {
        _pool.release(particle);
        return true;
      }
      return false;
    });
  }
}
```

#### 3. RepaintBoundary 隔离

**隔离重绘区域：**
```dart
Widget build(BuildContext context) {
  return Stack(
    children: [
      // 背景层 - 不常变化
      RepaintBoundary(
        child: BackgroundGrid(),
      ),

      // 粒子层 - 频繁变化
      RepaintBoundary(
        child: CustomPaint(
          painter: ParticlePainter(particles),
        ),
      ),

      // UI 层 - 偶尔变化
      RepaintBoundary(
        child: LifePointsDisplay(),
      ),
    ],
  );
}
```

#### 4. 条件渲染

**只渲染可见元素：**
```dart
@override
void paint(Canvas canvas, Size size) {
  final visibleRect = Rect.fromLTWH(0, 0, size.width, size.height);

  for (var particle in particles) {
    // 只绘制在可见区域内的粒子
    if (visibleRect.contains(particle.position)) {
      canvas.drawCircle(
        particle.position,
        particle.size,
        paint,
      );
    }
  }
}
```

---

## 🎯 第一原理技术分析

### 原理 #1：光的本质

```
光 = 颜色 + 透明度 + 位置
实现 = CustomPainter + Paint + Path
结论 = 不需要 3D 引擎或复杂依赖
```

**实现示例：**
```dart
void drawLightBeam(Canvas canvas, Offset start, Offset end) {
  final path = Path()
    ..moveTo(start.dx, start.dy)
    ..lineTo(end.dx, end.dy);

  final paint = Paint()
    ..shader = LinearGradient(
      colors: [
        Colors.blue.withOpacity(0.0),
        Colors.blue.withOpacity(0.8),
        Colors.blue.withOpacity(0.0),
      ],
    ).createShader(Rect.fromPoints(start, end))
    ..strokeWidth = 2.0
    ..style = PaintingStyle.stroke;

  canvas.drawPath(path, paint);
}
```

### 原理 #2：3D 空间感

```
3D 感 = 视觉错觉
实现 = Stack + Transform + Opacity
```

**Z 轴层次结构：**
```
最前景（Z: +3）：光流粒子效果、即时光轨
前景（Z: +2）：生命值数字、主要操作区
中景（Z: 0）：界面框架、按钮控制
后景（Z: -2）：几何网格、呼吸背景光效
深景（Z: -4）：流动资料流
```

**实现示例：**
```dart
Stack(
  children: [
    // 深景（Z: -4）
    Transform.scale(
      scale: 0.8,
      child: Opacity(
        opacity: 0.3,
        child: DataFlowBackground(),
      ),
    ),

    // 后景（Z: -2）
    Transform.scale(
      scale: 0.9,
      child: Opacity(
        opacity: 0.5,
        child: GeometricGrid(),
      ),
    ),

    // 中景（Z: 0）
    UIControls(),

    // 前景（Z: +2）
    LifePointsDisplay(),

    // 最前景（Z: +3）
    ParticleEffects(),
  ],
)
```

### 原理 #3：呼吸光流

```
呼吸 = sin/cos 曲线
光流 = Path + Gradient + Animation
核心代码 < 50 行
```

**实现示例：**
```dart
class BreathingLightPainter extends CustomPainter {
  final double progress; // 0.0 to 1.0
  final Color color;

  BreathingLightPainter({
    required this.progress,
    required this.color,
  });

  @override
  void paint(Canvas canvas, Size size) {
    // 呼吸曲线（sin 波）
    final breathPhase = sin(progress * 2 * pi);
    final breathIntensity = 0.8 + breathPhase * 0.2;

    // 光流路径（贝塞尔曲线）
    final path = Path()
      ..moveTo(0, size.height / 2)
      ..quadraticBezierTo(
        size.width / 2,
        size.height / 2 + breathPhase * 50,
        size.width,
        size.height / 2,
      );

    // 渐变光效
    final paint = Paint()
      ..shader = LinearGradient(
        colors: [
          color.withOpacity(0.0),
          color.withOpacity(breathIntensity),
          color.withOpacity(0.0),
        ],
      ).createShader(Rect.fromLTWH(0, 0, size.width, size.height))
      ..strokeWidth = 2.0 + breathPhase * 1.0
      ..style = PaintingStyle.stroke;

    canvas.drawPath(path, paint);
  }

  @override
  bool shouldRepaint(BreathingLightPainter oldDelegate) {
    return progress != oldDelegate.progress;
  }
}
```

### 原理 #4：粒子系统

```
粒子 = {x, y, vx, vy, alpha, size, color}
优化 = 对象池 + 空间分区 + 批次绘制
效能 = 500-1000 粒子 @ 60fps
```

**粒子模型：**
```dart
class Particle {
  Offset position;
  Offset velocity;
  double alpha;
  double size;
  Color color;
  double lifetime;
  double age;

  Particle({
    this.position = Offset.zero,
    this.velocity = Offset.zero,
    this.alpha = 1.0,
    this.size = 2.0,
    this.color = Colors.blue,
    this.lifetime = 1.0,
    this.age = 0.0,
  });

  void update(double dt) {
    age += dt;
    position += velocity * dt;
    alpha = 1.0 - (age / lifetime);
  }

  bool get isDead => age >= lifetime;

  void reset() {
    position = Offset.zero;
    velocity = Offset.zero;
    alpha = 1.0;
    age = 0.0;
  }

  void initialize(Offset pos, Color col) {
    position = pos;
    color = col;
    // 随机速度
    final angle = Random().nextDouble() * 2 * pi;
    final speed = 50.0 + Random().nextDouble() * 50.0;
    velocity = Offset(cos(angle), sin(angle)) * speed;
    size = 1.0 + Random().nextDouble() * 3.0;
    lifetime = 0.5 + Random().nextDouble() * 1.0;
    age = 0.0;
    alpha = 1.0;
  }
}
```

**粒子密度设计：**
```
生命值 8000（满血）：
├─ 粒子数量：密集（500-800 个）
├─ 运动：缓慢螺旋上升
├─ 颜色：饱和蓝/橘色
└─ 呼吸：深沉稳定

生命值 1000（危险）：
├─ 粒子数量：稀疏（100-150 个）
├─ 运动：不规则闪烁、下坠
├─ 颜色：暗淡、闪烁
└─ 呼吸：急促

生命值变化互动：
减少：光流冲击粒子场 → 粒子蒸发消失
增加：光流注入 → 新粒子从光流中诞生
```

---

## 🔧 技术栈选择

| 需求 | 复杂方案 | 第一原理方案 | ✅ 选择 | 理由 |
|------|----------|--------------|--------|------|
| 光效 | Flame 引擎 | CustomPainter | **简单** | 原生性能最佳，无额外依赖 |
| 3D感 | three_dart | Stack+Transform | **简单** | 伪3D足够，更轻量 |
| 粒子 | 物理引擎 | 手写轻量系统 | **简单** | 精确控制，性能可预测 |
| 状态 | Redux | Provider | **简单** | 学习曲线低，足够应用 |
| 蓝牙 | 多个套件 | flutter_blue_plus | **简单** | 社区支持好，稳定 |

---

## 📦 依赖包清单

### pubspec.yaml

```yaml
name: duel_console
description: "Yu-Gi-Oh Duel Console - 沉浸式决斗体验平台"
publish_to: 'none'
version: 1.0.0+1

environment:
  sdk: ^3.9.2

dependencies:
  flutter:
    sdk: flutter

  # UI
  cupertino_icons: ^1.0.8

  # 状态管理
  provider: ^6.1.2

  # 蓝牙连接（Phase 4）
  flutter_blue_plus: ^1.32.12

  # 权限管理
  permission_handler: ^11.3.1

dev_dependencies:
  flutter_test:
    sdk: flutter
  flutter_lints: ^5.0.0

flutter:
  uses-material-design: true
```

### 依赖说明

**provider (^6.1.2)**
- 用途：应用状态管理
- 理由：轻量、官方推荐、学习曲线低
- 使用场景：LifePointsProvider, ParticleSystemProvider, BluetoothProvider

**flutter_blue_plus (^1.32.12)**
- 用途：蓝牙 BLE 连接
- 理由：社区支持好、文档完善、跨平台
- 使用场景：Phase 4 蓝牙对战功能

**permission_handler (^11.3.1)**
- 用途：权限请求管理
- 理由：简化权限处理流程
- 使用场景：蓝牙权限、通知权限

---

## 🎨 视觉设计技术规格

### 色系定义（Tron Colors）

```dart
// lib/core/theme/tron_colors.dart
class TronColors {
  // 主色系 - 蓝橘对比
  static const Color playerBlue = Color(0xFF00D9FF);      // 玩家1/增加
  static const Color playerOrange = Color(0xFFFF6B35);    // 玩家2/减少

  // 背景色
  static const Color darkBackground = Color(0xFF0A0E1A);
  static const Color gridLines = Color(0xFF1A2332);

  // 光效色
  static const Color glowBlue = Color(0xFF4DFFFF);
  static const Color glowOrange = Color(0xFFFFAA80);

  // 状态色
  static const Color dangerRed = Color(0xFFFF3366);
  static const Color successGreen = Color(0xFF00FF88);
  static const Color neutralWhite = Color(0xFFFFFFFF);
}
```

### 动画参数常量

```dart
// lib/core/constants/animations.dart
class AnimationConstants {
  // 呼吸动画
  static const Duration breathingCycleFull = Duration(milliseconds: 2000);
  static const Duration breathingCycleNormal = Duration(milliseconds: 1500);
  static const Duration breathingCycleFast = Duration(milliseconds: 1000);

  // 转场动画
  static const Duration transitionDeconstruct = Duration(milliseconds: 100);
  static const Duration transitionTransform = Duration(milliseconds: 100);
  static const Duration transitionReconstruct = Duration(milliseconds: 100);
  static const Duration transitionTotal = Duration(milliseconds: 300);

  // 粒子系统
  static const int particleCountMax = 1000;
  static const int particleCountMin = 100;
  static const double particleLifetimeMin = 0.5;
  static const double particleLifetimeMax = 2.0;

  // 手势响应
  static const Duration swipeResponseDelay = Duration(milliseconds: 50);
  static const double swipeMinDistance = 30.0;
  static const double swipeMaxDistance = 300.0;
}
```

### 游戏规则常量

```dart
// lib/core/constants/game_rules.dart
class GameRules {
  // 生命值
  static const int startingLifePoints = 8000;
  static const int minLifePoints = 0;
  static const int maxLifePoints = 99999;

  // 生命值阈值（用于视觉状态）
  static const int lifePointsHigh = 6000;
  static const int lifePointsMedium = 2000;
  static const int lifePointsLow = 500;

  // 骰子
  static const int diceMin = 1;
  static const int diceMax = 6;

  // 硬币
  static const List<String> coinSides = ['正面', '反面'];
}
```

---

## 🚀 核心功能实现示例

### 1. 呼吸光流完整实现

```dart
// lib/features/duel/presentation/painters/breathing_light_painter.dart
import 'dart:math';
import 'package:flutter/material.dart';

class BreathingLightPainter extends CustomPainter {
  final double progress; // 0.0 to 1.0 (动画进度)
  final int lifePoints;  // 当前生命值
  final Color color;     // 玩家颜色
  final bool isIncreasing; // 是否增加

  BreathingLightPainter({
    required this.progress,
    required this.lifePoints,
    required this.color,
    this.isIncreasing = false,
  });

  @override
  void paint(Canvas canvas, Size size) {
    // 1. 计算呼吸周期
    final breathCycle = _calculateBreathCycle(lifePoints);
    final breathPhase = sin(progress * 2 * pi / breathCycle);

    // 2. 计算光流强度
    final intensity = 0.8 + breathPhase * 0.2;
    final strokeWidth = 2.0 + breathPhase * 1.0;

    // 3. 绘制多层光流
    _drawLightLayers(canvas, size, breathPhase, intensity, strokeWidth);
  }

  double _calculateBreathCycle(int lifePoints) {
    if (lifePoints >= 6000) return 1.0;      // 缓慢
    if (lifePoints >= 2000) return 0.75;     // 正常
    if (lifePoints >= 500) return 0.5;       // 急促
    return 0.3;                               // 挣扎
  }

  void _drawLightLayers(
    Canvas canvas,
    Size size,
    double phase,
    double intensity,
    double strokeWidth,
  ) {
    // 主光流
    _drawMainLightFlow(canvas, size, phase, intensity, strokeWidth);

    // 边缘辉光
    _drawEdgeGlow(canvas, size, phase, intensity * 0.5);

    // 粒子轨迹
    _drawParticleTrails(canvas, size, phase);
  }

  void _drawMainLightFlow(
    Canvas canvas,
    Size size,
    double phase,
    double intensity,
    double strokeWidth,
  ) {
    final path = Path();
    final centerY = size.height / 2;

    path.moveTo(0, centerY);
    path.quadraticBezierTo(
      size.width / 2,
      centerY + phase * 50,
      size.width,
      centerY,
    );

    final paint = Paint()
      ..shader = LinearGradient(
        colors: [
          color.withOpacity(0.0),
          color.withOpacity(intensity),
          color.withOpacity(0.0),
        ],
      ).createShader(Rect.fromLTWH(0, 0, size.width, size.height))
      ..strokeWidth = strokeWidth
      ..style = PaintingStyle.stroke
      ..strokeCap = StrokeCap.round;

    canvas.drawPath(path, paint);
  }

  void _drawEdgeGlow(
    Canvas canvas,
    Size size,
    double phase,
    double intensity,
  ) {
    final glowPaint = Paint()
      ..color = color.withOpacity(intensity * 0.3)
      ..maskFilter = MaskFilter.blur(BlurStyle.normal, 10.0);

    canvas.drawRect(
      Rect.fromLTWH(0, 0, size.width, size.height),
      glowPaint,
    );
  }

  void _drawParticleTrails(
    Canvas canvas,
    Size size,
    double phase,
  ) {
    final trailPaint = Paint()
      ..color = color.withOpacity(0.2)
      ..strokeWidth = 1.0
      ..style = PaintingStyle.stroke;

    for (int i = 0; i < 5; i++) {
      final offset = (phase + i * 0.2) % 1.0;
      final x = size.width * offset;
      final y = size.height / 2 + sin(offset * 4 * pi) * 20;

      canvas.drawCircle(
        Offset(x, y),
        2.0,
        trailPaint,
      );
    }
  }

  @override
  bool shouldRepaint(BreathingLightPainter oldDelegate) {
    return progress != oldDelegate.progress ||
           lifePoints != oldDelegate.lifePoints;
  }
}
```

### 2. 滑动手势力道计算

```dart
// lib/core/utils/gesture_utils.dart
import 'dart:math';
import 'package:flutter/material.dart';

class GestureUtils {
  /// 计算滑动力道
  /// 返回值：1-100 的整数
  static int calculateSwipeForce(
    Offset start,
    Offset end,
    Duration duration,
  ) {
    // 1. 计算距离
    final distance = (end - start).distance;

    // 2. 计算速度 (像素/秒)
    final velocity = distance / (duration.inMilliseconds / 1000.0);

    // 3. 权重计算
    final distanceScore = _normalizeDistance(distance);
    final velocityScore = _normalizeVelocity(velocity);

    // 4. 综合评分（速度权重 0.6，距离权重 0.4）
    final force = (velocityScore * 0.6 + distanceScore * 0.4) * 100;

    return force.clamp(1, 100).toInt();
  }

  static double _normalizeDistance(double distance) {
    const minDistance = 30.0;
    const maxDistance = 300.0;

    return ((distance - minDistance) / (maxDistance - minDistance))
        .clamp(0.0, 1.0);
  }

  static double _normalizeVelocity(double velocity) {
    const minVelocity = 100.0;  // 像素/秒
    const maxVelocity = 2000.0;

    return ((velocity - minVelocity) / (maxVelocity - minVelocity))
        .clamp(0.0, 1.0);
  }

  /// 将力道映射到生命值变化
  static int mapForceToLifePointsChange(int force) {
    // 力道等级
    if (force < 20) return 100;      // 轻微
    if (force < 40) return 500;      // 轻度
    if (force < 60) return 1000;     // 中等
    if (force < 80) return 2000;     // 重度
    return 4000;                      // 极重
  }
}
```

### 3. 生命值状态管理

```dart
// lib/features/duel/providers/life_points_provider.dart
import 'package:flutter/foundation.dart';
import '../models/player.dart';

class LifePointsProvider extends ChangeNotifier {
  Player _player1 = Player(id: 1, lifePoints: 8000, color: Colors.blue);
  Player _player2 = Player(id: 2, lifePoints: 8000, color: Colors.orange);

  final List<LifePointsChange> _history = [];

  Player get player1 => _player1;
  Player get player2 => _player2;
  List<LifePointsChange> get history => List.unmodifiable(_history);

  /// 修改生命值
  void changeLifePoints(int playerId, int delta) {
    final player = playerId == 1 ? _player1 : _player2;
    final oldValue = player.lifePoints;
    final newValue = (oldValue + delta).clamp(0, 99999);

    if (oldValue != newValue) {
      player.lifePoints = newValue;

      // 记录历史
      _history.add(LifePointsChange(
        playerId: playerId,
        oldValue: oldValue,
        newValue: newValue,
        delta: delta,
        timestamp: DateTime.now(),
      ));

      notifyListeners();
    }
  }

  /// 重置游戏
  void reset() {
    _player1.lifePoints = 8000;
    _player2.lifePoints = 8000;
    _history.clear();
    notifyListeners();
  }

  /// 撤销上一步
  void undo() {
    if (_history.isEmpty) return;

    final lastChange = _history.removeLast();
    final player = lastChange.playerId == 1 ? _player1 : _player2;
    player.lifePoints = lastChange.oldValue;

    notifyListeners();
  }
}

class LifePointsChange {
  final int playerId;
  final int oldValue;
  final int newValue;
  final int delta;
  final DateTime timestamp;

  LifePointsChange({
    required this.playerId,
    required this.oldValue,
    required this.newValue,
    required this.delta,
    required this.timestamp,
  });
}
```

---

## 📱 平台特定配置

### iOS 配置

**Info.plist 添加蓝牙权限：**
```xml
<!-- ios/Runner/Info.plist -->
<key>NSBluetoothAlwaysUsageDescription</key>
<string>需要蓝牙权限以连接对战设备</string>
<key>NSBluetoothPeripheralUsageDescription</key>
<string>需要蓝牙权限以进行双人对战</string>
```

### Android 配置

**AndroidManifest.xml 添加权限：**
```xml
<!-- android/app/src/main/AndroidManifest.xml -->
<uses-permission android:name="android.permission.BLUETOOTH"/>
<uses-permission android:name="android.permission.BLUETOOTH_ADMIN"/>
<uses-permission android:name="android.permission.BLUETOOTH_SCAN"/>
<uses-permission android:name="android.permission.BLUETOOTH_CONNECT"/>
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
```

---

## 🧪 测试策略

### 单元测试

**测试覆盖：**
- 手势力道计算
- 生命值状态管理
- 粒子系统逻辑

### Widget 测试

**测试覆盖：**
- UI 组件渲染
- 用户交互响应
- 状态变化更新

### 性能测试

**测试指标：**
- 帧率监控（60fps）
- 内存使用（<100MB）
- 启动时间（<1s）

---

## 📊 开发时程

```
Phase 1（核心功能）：2-3 週
├─ 生命值显示 + 基础动画
├─ 滑动输入
└─ 简单光流效果

Phase 2（视觉增强）：2 週
├─ 粒子系统
├─ 呼吸效果
└─ 3D 空间感

Phase 3（功能扩充）：1-2 週
├─ 骰子/硬币
├─ 计数器
└─ 节拍视觉化

Phase 4（连接功能）：2 週
├─ 蓝牙对战
└─ 同步系统

Phase 5（优化发布）：1-2 週
├─ 性能优化
├─ Bug 修复
└─ 应用商店准备

总计：8-11 週完整开发
```

---

## 🔗 相关文档

- **产品简报：** `docs/product-brief-bmad2-2025-11-13.md`
- **头脑风暴报告：** `docs/bmm-brainstorming-session-2025-11-13.md`
- **技术架构：** `docs/technical-architecture-bmad2-2025-11-13.md`（本文档）

---

## ✅ 核心要点总结

### 技术选择原则

1. ✅ **简单优先** - 使用 Flutter 原生能力，避免过度依赖
2. ✅ **性能至上** - 60fps 是不可妥协的底线
3. ✅ **可维护性** - 清晰的架构和代码组织
4. ✅ **渐进实现** - 从简单到复杂，逐步增强

### 成功的关键因素

1. ✅ **第一原理思维** - 从基础原理重建，避免过度工程
2. ✅ **性能优化策略** - 对象池、批次渲染、RepaintBoundary
3. ✅ **清晰架构** - Feature-First 目录结构，职责分离
4. ✅ **渐进开发** - 分阶段实现，每阶段都可独立验证

---

**🚀 Generated with BMad Method**
**📅 Date: 2025-11-13**
**👤 Author: yyx**
**🤖 Agent: Claude Code**
