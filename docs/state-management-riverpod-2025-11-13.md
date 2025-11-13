# Riverpod 状态管理架构详解

**专案名称：** bmad2 (Yu-Gi-Oh Duel Console)
**文档类型：** 状态管理架构设计（Riverpod）
**日期：** 2025-11-13
**作者：** yyx
**版本：** 1.0

---

## 📋 概要

本文档详细说明 Yu-Gi-Oh Duel Console 使用 **Riverpod** 进行状态管理的完整架构设计，包括数据模型、Notifier 实现、Provider 定义、UI 集成示例和最佳实践。

---

## 🎯 为什么选择 Riverpod？

### Riverpod vs Provider 对比

| 特性 | Provider | Riverpod | 优势说明 |
|------|----------|----------|----------|
| **类型安全** | 🟡 中等 | ✅ 完全 | 编译时错误检测，减少运行时错误 |
| **依赖注入** | 需要 BuildContext | ❌ 不需要 | 在任何地方访问状态，更灵活 |
| **测试友好** | 🟡 一般 | ✅ 优秀 | 易于模拟（mock）和单元测试 |
| **性能** | ✅ 好 | ✅ 更好 | 细粒度重建，减少不必要的渲染 |
| **组合性** | 🟡 有限 | ✅ 强大 | Provider 之间可以互相依赖组合 |
| **异步支持** | 🟡 一般 | ✅ 内置 | FutureProvider、StreamProvider |
| **DevTools** | ✅ 支持 | ✅ 更好 | 更强大的调试工具 |

### Riverpod 核心优势

1. **编译时安全** - 所有错误在编译时捕获
2. **无需 Context** - 不依赖 BuildContext，更简洁
3. **声明式 API** - 代码更清晰易读
4. **自动依赖追踪** - Provider 之间依赖关系自动管理
5. **内置缓存** - 自动优化性能

---

## 🏗️ Riverpod 状态管理架构

### 整体架构图

```
┌─────────────────────────────────────────────────────────────┐
│                        应用层（UI）                          │
│  ConsumerWidget / Consumer / ConsumerStatefulWidget         │
│  └─ ref.watch() / ref.read() / ref.listen()                │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│                    状态层（Providers）                       │
│  ├─ gameStateProvider          # 游戏状态                   │
│  ├─ player1Provider            # 玩家1（派生）              │
│  ├─ player2Provider            # 玩家2（派生）              │
│  ├─ particleSystemProvider     # 粒子系统                   │
│  ├─ animationProvider          # 动画状态                   │
│  └─ isGameOverProvider         # 游戏结束状态（派生）        │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│              业务逻辑层（Notifiers）                         │
│  ├─ GameStateNotifier          # 游戏状态管理逻辑           │
│  ├─ ParticleSystemNotifier     # 粒子系统逻辑               │
│  └─ AnimationNotifier          # 动画控制逻辑               │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│                   数据层（Models）                           │
│  ├─ GameState                  # 游戏状态数据模型           │
│  ├─ Player                     # 玩家数据模型               │
│  ├─ Particle                   # 粒子数据模型               │
│  └─ LifePointsChange           # 历史记录模型               │
└─────────────────────────────────────────────────────────────┘
```

---

## 📦 依赖配置

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

  cupertino_icons: ^1.0.8

  # 状态管理 - Riverpod（替代 Provider）
  flutter_riverpod: ^2.5.1
  riverpod_annotation: ^2.3.5

  # 蓝牙连接（Phase 4）
  flutter_blue_plus: ^1.32.12

  # 权限管理
  permission_handler: ^11.3.1

dev_dependencies:
  flutter_test:
    sdk: flutter
  flutter_lints: ^5.0.0

  # Riverpod 代码生成（可选，用于自动生成 Provider）
  build_runner: ^2.4.9
  riverpod_generator: ^2.4.0

flutter:
  uses-material-design: true
```

---

## 📁 目录结构

```
lib/features/duel/
├─ models/                          # 数据模型层
│  ├─ player.dart                   # 玩家模型
│  ├─ life_points_change.dart       # 生命值变化历史
│  ├─ game_state.dart               # 游戏状态模型
│  └─ particle.dart                 # 粒子模型
│
├─ providers/                       # 状态管理层
│  ├─ game_state_notifier.dart      # 游戏状态 Notifier
│  ├─ particle_system_notifier.dart # 粒子系统 Notifier
│  └─ game_providers.dart           # Provider 定义集合
│
└─ presentation/                    # UI 层
   ├─ screens/
   │  └─ duel_screen.dart           # 主决斗屏幕
   └─ widgets/
      ├─ life_points_display.dart   # 生命值显示组件
      └─ swipe_input_area.dart      # 滑动输入区域
```

---

## 🎨 数据模型层（Models）

### 1. Player 模型

```dart
// lib/features/duel/models/player.dart
import 'package:flutter/material.dart';

/// 玩家数据模型
class Player {
  final int id;
  final String name;
  final Color color;
  int lifePoints;

  Player({
    required this.id,
    required this.name,
    required this.color,
    this.lifePoints = 8000,
  });

  /// 复制方法（用于不可变更新）
  Player copyWith({
    int? id,
    String? name,
    Color? color,
    int? lifePoints,
  }) {
    return Player(
      id: id ?? this.id,
      name: name ?? this.name,
      color: color ?? this.color,
      lifePoints: lifePoints ?? this.lifePoints,
    );
  }

  /// 生命值状态枚举
  LifePointsStatus get status {
    if (lifePoints >= 6000) return LifePointsStatus.high;
    if (lifePoints >= 2000) return LifePointsStatus.medium;
    if (lifePoints >= 500) return LifePointsStatus.low;
    return LifePointsStatus.critical;
  }

  /// 呼吸周期（秒）- 根据生命值状态动态调整
  double get breathingCycle {
    switch (status) {
      case LifePointsStatus.high:
        return 2.5;  // 深沉缓慢
      case LifePointsStatus.medium:
        return 1.75; // 正常节奏
      case LifePointsStatus.low:
        return 1.0;  // 急促呼吸
      case LifePointsStatus.critical:
        return 0.6;  // 挣扎呼吸
    }
  }

  /// 粒子目标数量 - 根据生命值计算
  int get particleTargetCount {
    return (lifePoints / 10).clamp(100, 800).toInt();
  }

  @override
  String toString() => 'Player($id: $name, LP: $lifePoints)';

  @override
  bool operator ==(Object other) =>
      identical(this, other) ||
      other is Player &&
          runtimeType == other.runtimeType &&
          id == other.id &&
          lifePoints == other.lifePoints;

  @override
  int get hashCode => id.hashCode ^ lifePoints.hashCode;
}

/// 生命值状态枚举
enum LifePointsStatus {
  high,      // 6000+ （安全）
  medium,    // 2000-6000 （正常）
  low,       // 500-2000 （危险）
  critical,  // <500 （濒死）
}
```

---

### 2. LifePointsChange 历史记录模型

```dart
// lib/features/duel/models/life_points_change.dart

/// 生命值变化历史记录
class LifePointsChange {
  final int playerId;
  final int oldValue;
  final int newValue;
  final int delta;
  final DateTime timestamp;
  final ChangeType type;

  LifePointsChange({
    required this.playerId,
    required this.oldValue,
    required this.newValue,
    required this.delta,
    required this.timestamp,
    required this.type,
  });

  /// 绝对变化量
  int get absoluteDelta => delta.abs();

  /// 是否增加
  bool get isIncrease => delta > 0;

  /// 是否减少
  bool get isDecrease => delta < 0;

  /// 格式化显示
  @override
  String toString() {
    final sign = isIncrease ? '+' : '';
    return 'P$playerId: $oldValue → $newValue ($sign$delta) [${type.name}]';
  }

  /// 复制方法
  LifePointsChange copyWith({
    int? playerId,
    int? oldValue,
    int? newValue,
    int? delta,
    DateTime? timestamp,
    ChangeType? type,
  }) {
    return LifePointsChange(
      playerId: playerId ?? this.playerId,
      oldValue: oldValue ?? this.oldValue,
      newValue: newValue ?? this.newValue,
      delta: delta ?? this.delta,
      timestamp: timestamp ?? this.timestamp,
      type: type ?? this.type,
    );
  }
}

/// 变化类型枚举
enum ChangeType {
  swipe,       // 滑动输入
  button,      // 按钮输入
  bluetooth,   // 蓝牙同步
  undo,        // 撤销操作
  reset,       // 重置游戏
}
```

---

### 3. GameState 游戏状态模型

```dart
// lib/features/duel/models/game_state.dart
import 'package:flutter/material.dart';
import 'player.dart';
import 'life_points_change.dart';

/// 游戏状态数据模型
class GameState {
  final Player player1;
  final Player player2;
  final List<LifePointsChange> history;
  final GameMode mode;
  final bool isPaused;

  GameState({
    required this.player1,
    required this.player2,
    this.history = const [],
    this.mode = GameMode.local,
    this.isPaused = false,
  });

  /// 复制方法（不可变更新）
  GameState copyWith({
    Player? player1,
    Player? player2,
    List<LifePointsChange>? history,
    GameMode? mode,
    bool? isPaused,
  }) {
    return GameState(
      player1: player1 ?? this.player1,
      player2: player2 ?? this.player2,
      history: history ?? this.history,
      mode: mode ?? this.mode,
      isPaused: isPaused ?? this.isPaused,
    );
  }

  /// 工厂方法：创建初始状态
  factory GameState.initial() {
    return GameState(
      player1: Player(
        id: 1,
        name: 'Player 1',
        color: const Color(0xFF00D9FF), // Tron Blue
        lifePoints: 8000,
      ),
      player2: Player(
        id: 2,
        name: 'Player 2',
        color: const Color(0xFFFF6B35), // Tron Orange
        lifePoints: 8000,
      ),
      history: [],
      mode: GameMode.local,
      isPaused: false,
    );
  }

  /// 游戏是否结束
  bool get isGameOver {
    return player1.lifePoints <= 0 || player2.lifePoints <= 0;
  }

  /// 获胜者
  Player? get winner {
    if (!isGameOver) return null;
    if (player1.lifePoints <= 0 && player2.lifePoints <= 0) {
      return null; // 平局（同时归零）
    }
    return player1.lifePoints > 0 ? player1 : player2;
  }

  /// 失败者
  Player? get loser {
    if (!isGameOver) return null;
    if (player1.lifePoints <= 0 && player2.lifePoints <= 0) {
      return null; // 平局
    }
    return player1.lifePoints <= 0 ? player1 : player2;
  }

  /// 是否可以撤销
  bool get canUndo => history.isNotEmpty && !isPaused;

  @override
  String toString() {
    return 'GameState(P1: ${player1.lifePoints}, P2: ${player2.lifePoints}, '
        'History: ${history.length}, Mode: ${mode.name})';
  }
}

/// 游戏模式枚举
enum GameMode {
  local,      // 单机模式（一台手机，双人对战）
  bluetooth,  // 蓝牙对战模式（两台手机）
}
```

---

### 4. Particle 粒子模型（用于粒子系统）

```dart
// lib/features/duel/models/particle.dart
import 'dart:math';
import 'package:flutter/material.dart';

/// 粒子数据模型
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

  /// 更新粒子状态
  void update(double dt) {
    age += dt;
    position += velocity * dt;

    // 透明度随时间衰减
    alpha = (1.0 - (age / lifetime)).clamp(0.0, 1.0);

    // 可选：大小随时间变化
    size = 2.0 * (1.0 - age / lifetime);
  }

  /// 是否死亡
  bool get isDead => age >= lifetime;

  /// 重置粒子（用于对象池）
  void reset() {
    position = Offset.zero;
    velocity = Offset.zero;
    alpha = 1.0;
    size = 2.0;
    age = 0.0;
  }

  /// 初始化粒子（从对象池获取后初始化）
  void initialize(Offset pos, Color col) {
    position = pos;
    color = col;

    // 随机速度
    final random = Random();
    final angle = random.nextDouble() * 2 * pi;
    final speed = 50.0 + random.nextDouble() * 50.0;
    velocity = Offset(cos(angle), sin(angle)) * speed;

    // 随机属性
    size = 1.0 + random.nextDouble() * 3.0;
    lifetime = 0.5 + random.nextDouble() * 1.0;
    age = 0.0;
    alpha = 1.0;
  }
}
```

---

## 🔧 业务逻辑层（Notifiers）

### 1. GameStateNotifier（核心游戏状态管理）

```dart
// lib/features/duel/providers/game_state_notifier.dart
import 'package:flutter_riverpod/flutter_riverpod.dart';
import '../models/game_state.dart';
import '../models/player.dart';
import '../models/life_points_change.dart';

/// 游戏状态管理 Notifier
class GameStateNotifier extends StateNotifier<GameState> {
  GameStateNotifier() : super(GameState.initial());

  /// 修改生命值
  void changeLifePoints(
    int playerId,
    int delta, {
    ChangeType type = ChangeType.swipe,
  }) {
    // 游戏结束或暂停时不允许操作
    if (state.isGameOver || state.isPaused) return;

    final player = playerId == 1 ? state.player1 : state.player2;
    final oldValue = player.lifePoints;
    final newValue = (oldValue + delta).clamp(0, 99999);

    // 值没变化则返回
    if (oldValue == newValue) return;

    // 更新玩家生命值
    final updatedPlayer = player.copyWith(lifePoints: newValue);

    // 创建历史记录
    final change = LifePointsChange(
      playerId: playerId,
      oldValue: oldValue,
      newValue: newValue,
      delta: delta,
      timestamp: DateTime.now(),
      type: type,
    );

    // 更新状态（不可变更新）
    state = state.copyWith(
      player1: playerId == 1 ? updatedPlayer : state.player1,
      player2: playerId == 2 ? updatedPlayer : state.player2,
      history: [...state.history, change],
    );
  }

  /// 设置生命值（直接设置到指定值）
  void setLifePoints(int playerId, int value) {
    final player = playerId == 1 ? state.player1 : state.player2;
    final delta = value - player.lifePoints;
    if (delta != 0) {
      changeLifePoints(playerId, delta, type: ChangeType.button);
    }
  }

  /// 撤销上一步操作
  void undo() {
    if (!state.canUndo) return;

    final lastChange = state.history.last;
    final player = lastChange.playerId == 1 ? state.player1 : state.player2;
    final updatedPlayer = player.copyWith(lifePoints: lastChange.oldValue);

    state = state.copyWith(
      player1: lastChange.playerId == 1 ? updatedPlayer : state.player1,
      player2: lastChange.playerId == 2 ? updatedPlayer : state.player2,
      history: state.history.sublist(0, state.history.length - 1),
    );
  }

  /// 重置游戏
  void reset() {
    state = GameState.initial();
  }

  /// 暂停/恢复游戏
  void togglePause() {
    state = state.copyWith(isPaused: !state.isPaused);
  }

  /// 设置暂停状态
  void setPaused(bool paused) {
    state = state.copyWith(isPaused: paused);
  }

  /// 切换游戏模式
  void setMode(GameMode mode) {
    state = state.copyWith(mode: mode);
  }

  /// 交换玩家（旋转屏幕）
  void swapPlayers() {
    state = state.copyWith(
      player1: state.player2.copyWith(id: 1),
      player2: state.player1.copyWith(id: 2),
    );
  }
}
```

---

### 2. ParticleSystemNotifier（粒子系统状态管理）

```dart
// lib/features/duel/providers/particle_system_notifier.dart
import 'package:flutter_riverpod/flutter_riverpod.dart';
import '../models/particle.dart';

/// 粒子系统状态
class ParticleSystemState {
  final List<Particle> particles;
  final int targetCount;
  final bool isActive;

  ParticleSystemState({
    required this.particles,
    required this.targetCount,
    this.isActive = true,
  });

  ParticleSystemState copyWith({
    List<Particle>? particles,
    int? targetCount,
    bool? isActive,
  }) {
    return ParticleSystemState(
      particles: particles ?? this.particles,
      targetCount: targetCount ?? this.targetCount,
      isActive: isActive ?? this.isActive,
    );
  }
}

/// 粒子系统管理 Notifier
class ParticleSystemNotifier extends StateNotifier<ParticleSystemState> {
  ParticleSystemNotifier()
      : super(ParticleSystemState(
          particles: [],
          targetCount: 500,
          isActive: true,
        ));

  /// 更新目标粒子数量（基于生命值）
  /// 生命值 8000 = 800 粒子
  /// 生命值 1000 = 100 粒子
  void updateTargetCount(int lifePoints) {
    final count = (lifePoints / 10).clamp(100, 800).toInt();
    if (state.targetCount != count) {
      state = state.copyWith(targetCount: count);
    }
  }

  /// 更新粒子列表
  void updateParticles(List<Particle> particles) {
    state = state.copyWith(particles: particles);
  }

  /// 激活/停用粒子系统
  void setActive(bool active) {
    state = state.copyWith(isActive: active);
  }

  /// 清空所有粒子
  void clear() {
    state = state.copyWith(particles: []);
  }

  /// 添加单个粒子
  void addParticle(Particle particle) {
    state = state.copyWith(
      particles: [...state.particles, particle],
    );
  }

  /// 批量添加粒子
  void addParticles(List<Particle> newParticles) {
    state = state.copyWith(
      particles: [...state.particles, ...newParticles],
    );
  }
}
```

---

## 🎯 Provider 定义层

### game_providers.dart（Provider 集合）

```dart
// lib/features/duel/providers/game_providers.dart
import 'package:flutter_riverpod/flutter_riverpod.dart';
import 'game_state_notifier.dart';
import 'particle_system_notifier.dart';
import '../models/game_state.dart';
import '../models/player.dart';
import '../models/life_points_change.dart';

// ============================================================
// 核心状态 Provider
// ============================================================

/// 游戏状态 Provider（核心）
final gameStateProvider = StateNotifierProvider<GameStateNotifier, GameState>(
  (ref) => GameStateNotifier(),
);

// ============================================================
// 派生状态 Provider（基于 gameStateProvider）
// ============================================================

/// Player 1 Provider（派生状态）
final player1Provider = Provider<Player>((ref) {
  return ref.watch(gameStateProvider).player1;
});

/// Player 2 Provider（派生状态）
final player2Provider = Provider<Player>((ref) {
  return ref.watch(gameStateProvider).player2;
});

/// 游戏是否结束 Provider
final isGameOverProvider = Provider<bool>((ref) {
  return ref.watch(gameStateProvider).isGameOver;
});

/// 获胜者 Provider
final winnerProvider = Provider<Player?>((ref) {
  return ref.watch(gameStateProvider).winner;
});

/// 历史记录列表 Provider
final historyProvider = Provider<List<LifePointsChange>>((ref) {
  return ref.watch(gameStateProvider).history;
});

/// 历史记录数量 Provider
final historyCountProvider = Provider<int>((ref) {
  return ref.watch(gameStateProvider).history.length;
});

/// 是否可以撤销 Provider
final canUndoProvider = Provider<bool>((ref) {
  return ref.watch(gameStateProvider).canUndo;
});

/// 游戏模式 Provider
final gameModeProvider = Provider<GameMode>((ref) {
  return ref.watch(gameStateProvider).mode;
});

/// 游戏是否暂停 Provider
final isPausedProvider = Provider<bool>((ref) {
  return ref.watch(gameStateProvider).isPaused;
});

// ============================================================
// 粒子系统 Provider（自动响应生命值变化）
// ============================================================

/// 粒子系统 Provider（Player 1）
final particleSystemPlayer1Provider =
    StateNotifierProvider<ParticleSystemNotifier, ParticleSystemState>(
  (ref) {
    final notifier = ParticleSystemNotifier();

    // 监听 Player 1 生命值变化，自动更新粒子目标数量
    ref.listen<Player>(
      player1Provider,
      (previous, next) {
        if (previous?.lifePoints != next.lifePoints) {
          notifier.updateTargetCount(next.lifePoints);
        }
      },
    );

    return notifier;
  },
);

/// 粒子系统 Provider（Player 2）
final particleSystemPlayer2Provider =
    StateNotifierProvider<ParticleSystemNotifier, ParticleSystemState>(
  (ref) {
    final notifier = ParticleSystemNotifier();

    // 监听 Player 2 生命值变化，自动更新粒子目标数量
    ref.listen<Player>(
      player2Provider,
      (previous, next) {
        if (previous?.lifePoints != next.lifePoints) {
          notifier.updateTargetCount(next.lifePoints);
        }
      },
    );

    return notifier;
  },
);

// ============================================================
// Family Provider（参数化 Provider）
// ============================================================

/// 根据玩家 ID 获取玩家 Provider
final playerProvider = Provider.family<Player, int>((ref, playerId) {
  final gameState = ref.watch(gameStateProvider);
  return playerId == 1 ? gameState.player1 : gameState.player2;
});

/// 根据玩家 ID 获取粒子系统 Provider
final particleSystemProvider = StateNotifierProvider.family<
    ParticleSystemNotifier,
    ParticleSystemState,
    int
>((ref, playerId) {
  return playerId == 1
      ? ref.watch(particleSystemPlayer1Provider.notifier)
      : ref.watch(particleSystemPlayer2Provider.notifier);
});
```

---

## 🎨 UI 集成示例

### 1. 应用入口（ProviderScope）

```dart
// lib/main.dart
import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';
import 'features/duel/presentation/screens/duel_screen.dart';
import 'core/theme/app_theme.dart';

void main() {
  runApp(
    // ProviderScope 必须包裹整个应用
    const ProviderScope(
      child: DuelConsoleApp(),
    ),
  );
}

class DuelConsoleApp extends StatelessWidget {
  const DuelConsoleApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Yu-Gi-Oh Duel Console',
      theme: AppTheme.darkTheme,
      home: const DuelScreen(),
      debugShowCheckedModeBanner: false,
    );
  }
}
```

---

### 2. ConsumerWidget（生命值显示组件）

```dart
// lib/features/duel/presentation/widgets/life_points_display.dart
import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';
import '../../providers/game_providers.dart';
import '../../models/player.dart';

/// 生命值显示组件
class LifePointsDisplay extends ConsumerWidget {
  final int playerId;

  const LifePointsDisplay({
    Key? key,
    required this.playerId,
  }) : super(key: key);

  @override
  Widget build(BuildContext context, WidgetRef ref) {
    // 使用 ref.watch 监听玩家状态
    final player = ref.watch(playerProvider(playerId));

    return Container(
      padding: const EdgeInsets.all(24),
      decoration: BoxDecoration(
        border: Border.all(
          color: player.color.withOpacity(0.6),
          width: 2,
        ),
        borderRadius: BorderRadius.circular(16),
        gradient: LinearGradient(
          colors: [
            player.color.withOpacity(0.1),
            player.color.withOpacity(0.05),
          ],
          begin: Alignment.topCenter,
          end: Alignment.bottomCenter,
        ),
        boxShadow: [
          BoxShadow(
            color: player.color.withOpacity(0.3),
            blurRadius: 20,
            spreadRadius: 2,
          ),
        ],
      ),
      child: Column(
        mainAxisSize: MainAxisSize.min,
        children: [
          // 玩家名称
          Text(
            player.name,
            style: TextStyle(
              fontSize: 18,
              color: player.color,
              fontWeight: FontWeight.bold,
              letterSpacing: 1.2,
            ),
          ),
          const SizedBox(height: 8),

          // 生命值数字
          Text(
            '${player.lifePoints}',
            style: TextStyle(
              fontSize: 64,
              color: player.color,
              fontWeight: FontWeight.bold,
              shadows: [
                Shadow(
                  color: player.color.withOpacity(0.8),
                  blurRadius: 10,
                ),
              ],
            ),
          ),
          const SizedBox(height: 8),

          // 状态指示器
          _buildStatusIndicator(player.status, player.color),
        ],
      ),
    );
  }

  Widget _buildStatusIndicator(LifePointsStatus status, Color color) {
    String text;
    IconData icon;

    switch (status) {
      case LifePointsStatus.high:
        text = '安全';
        icon = Icons.favorite;
        break;
      case LifePointsStatus.medium:
        text = '正常';
        icon = Icons.favorite_border;
        break;
      case LifePointsStatus.low:
        text = '危险';
        icon = Icons.warning;
        break;
      case LifePointsStatus.critical:
        text = '濒死';
        icon = Icons.dangerous;
        break;
    }

    return Container(
      padding: const EdgeInsets.symmetric(horizontal: 12, vertical: 4),
      decoration: BoxDecoration(
        color: color.withOpacity(0.2),
        borderRadius: BorderRadius.circular(12),
        border: Border.all(color: color.withOpacity(0.5)),
      ),
      child: Row(
        mainAxisSize: MainAxisSize.min,
        children: [
          Icon(icon, color: color, size: 16),
          const SizedBox(width: 4),
          Text(
            text,
            style: TextStyle(
              color: color,
              fontSize: 14,
              fontWeight: FontWeight.w600,
            ),
          ),
        ],
      ),
    );
  }
}
```

---

### 3. ConsumerStatefulWidget（滑动输入组件）

```dart
// lib/features/duel/presentation/widgets/swipe_input_area.dart
import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';
import '../../providers/game_providers.dart';
import '../../../../core/utils/gesture_utils.dart';

/// 滑动输入区域组件
class SwipeInputArea extends ConsumerStatefulWidget {
  final int playerId;

  const SwipeInputArea({
    Key? key,
    required this.playerId,
  }) : super(key: key);

  @override
  ConsumerState<SwipeInputArea> createState() => _SwipeInputAreaState();
}

class _SwipeInputAreaState extends ConsumerState<SwipeInputArea> {
  Offset? _startPosition;
  DateTime? _startTime;
  bool _isDragging = false;

  @override
  Widget build(BuildContext context) {
    final player = ref.watch(playerProvider(widget.playerId));

    return GestureDetector(
      onVerticalDragStart: _onDragStart,
      onVerticalDragUpdate: _onDragUpdate,
      onVerticalDragEnd: _onDragEnd,
      child: Container(
        color: Colors.transparent,
        child: Center(
          child: AnimatedOpacity(
            opacity: _isDragging ? 0.3 : 1.0,
            duration: const Duration(milliseconds: 200),
            child: Column(
              mainAxisSize: MainAxisSize.min,
              children: [
                Icon(
                  Icons.swipe_vertical,
                  size: 48,
                  color: player.color.withOpacity(0.3),
                ),
                const SizedBox(height: 8),
                Text(
                  '上滑增加 / 下滑减少',
                  style: TextStyle(
                    color: player.color.withOpacity(0.5),
                    fontSize: 12,
                  ),
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }

  void _onDragStart(DragStartDetails details) {
    setState(() {
      _startPosition = details.globalPosition;
      _startTime = DateTime.now();
      _isDragging = true;
    });
  }

  void _onDragUpdate(DragUpdateDetails details) {
    // 可选：显示实时反馈
  }

  void _onDragEnd(DragEndDetails details) {
    if (_startPosition == null || _startTime == null) return;

    final velocity = details.velocity.pixelsPerSecond;
    final duration = DateTime.now().difference(_startTime!);

    // 计算滑动力道
    final force = GestureUtils.calculateSwipeForce(
      _startPosition!,
      velocity,
      duration,
    );

    // 映射到生命值变化
    final delta = GestureUtils.mapForceToLifePointsChange(force);

    // 判断方向（向上增加，向下减少）
    final isIncrease = velocity.dy < 0;
    final finalDelta = isIncrease ? delta : -delta;

    // 修改状态（使用 ref.read 避免重建）
    ref.read(gameStateProvider.notifier).changeLifePoints(
          widget.playerId,
          finalDelta,
        );

    // 重置
    setState(() {
      _startPosition = null;
      _startTime = null;
      _isDragging = false;
    });
  }
}
```

---

### 4. 主屏幕（ref.listen 监听状态变化）

```dart
// lib/features/duel/presentation/screens/duel_screen.dart
import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';
import '../../providers/game_providers.dart';
import '../widgets/life_points_display.dart';
import '../widgets/swipe_input_area.dart';

class DuelScreen extends ConsumerStatefulWidget {
  const DuelScreen({Key? key}) : super(key: key);

  @override
  ConsumerState<DuelScreen> createState() => _DuelScreenState();
}

class _DuelScreenState extends ConsumerState<DuelScreen> {
  @override
  void initState() {
    super.initState();

    // 使用 addPostFrameCallback 避免在 build 期间调用
    WidgetsBinding.instance.addPostFrameCallback((_) {
      // 监听游戏结束状态
      ref.listen<bool>(isGameOverProvider, (previous, next) {
        if (next && !previous!) {
          _showGameOverDialog();
        }
      });
    });
  }

  @override
  Widget build(BuildContext context) {
    // 监听历史记录数量（用于撤销按钮状态）
    final canUndo = ref.watch(canUndoProvider);

    return Scaffold(
      backgroundColor: const Color(0xFF0A0E1A), // Tron Dark Background
      appBar: AppBar(
        title: const Text('Yu-Gi-Oh Duel Console'),
        backgroundColor: Colors.transparent,
        elevation: 0,
        actions: [
          // 撤销按钮
          IconButton(
            icon: const Icon(Icons.undo),
            onPressed: canUndo
                ? () => ref.read(gameStateProvider.notifier).undo()
                : null,
            tooltip: '撤销',
          ),
          // 重置按钮
          IconButton(
            icon: const Icon(Icons.refresh),
            onPressed: () => _showResetConfirmDialog(),
            tooltip: '重置游戏',
          ),
          // 菜单按钮
          IconButton(
            icon: const Icon(Icons.more_vert),
            onPressed: () => _showMenuDialog(),
            tooltip: '菜单',
          ),
        ],
      ),
      body: Column(
        children: [
          // Player 2 区域（倒置 180°）
          Expanded(
            child: Transform.rotate(
              angle: 3.14159, // π (180 度)
              child: Column(
                children: [
                  const Padding(
                    padding: EdgeInsets.all(16.0),
                    child: LifePointsDisplay(playerId: 2),
                  ),
                  Expanded(
                    child: SwipeInputArea(playerId: 2),
                  ),
                ],
              ),
            ),
          ),

          // 分隔线
          Container(
            height: 2,
            decoration: BoxDecoration(
              gradient: LinearGradient(
                colors: [
                  Colors.blue.withOpacity(0.5),
                  Colors.orange.withOpacity(0.5),
                ],
              ),
            ),
          ),

          // Player 1 区域
          Expanded(
            child: Column(
              children: [
                Expanded(
                  child: SwipeInputArea(playerId: 1),
                ),
                const Padding(
                  padding: EdgeInsets.all(16.0),
                  child: LifePointsDisplay(playerId: 1),
                ),
              ],
            ),
          ),
        ],
      ),
    );
  }

  void _showGameOverDialog() {
    final winner = ref.read(winnerProvider);
    if (winner == null) return;

    showDialog(
      context: context,
      barrierDismissible: false,
      builder: (context) => AlertDialog(
        title: const Text('游戏结束'),
        content: Text(
          '${winner.name} 获胜！',
          style: TextStyle(
            fontSize: 24,
            color: winner.color,
            fontWeight: FontWeight.bold,
          ),
        ),
        actions: [
          TextButton(
            onPressed: () {
              Navigator.of(context).pop();
              ref.read(gameStateProvider.notifier).reset();
            },
            child: const Text('重新开始'),
          ),
        ],
      ),
    );
  }

  void _showResetConfirmDialog() {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('重置游戏'),
        content: const Text('确定要重置游戏吗？所有进度将被清除。'),
        actions: [
          TextButton(
            onPressed: () => Navigator.of(context).pop(),
            child: const Text('取消'),
          ),
          TextButton(
            onPressed: () {
              Navigator.of(context).pop();
              ref.read(gameStateProvider.notifier).reset();
            },
            child: const Text('确定'),
          ),
        ],
      ),
    );
  }

  void _showMenuDialog() {
    showDialog(
      context: context,
      builder: (context) => AlertDialog(
        title: const Text('菜单'),
        content: Column(
          mainAxisSize: MainAxisSize.min,
          children: [
            ListTile(
              leading: const Icon(Icons.history),
              title: const Text('历史记录'),
              onTap: () {
                Navigator.of(context).pop();
                // TODO: 显示历史记录
              },
            ),
            ListTile(
              leading: const Icon(Icons.settings),
              title: const Text('设置'),
              onTap: () {
                Navigator.of(context).pop();
                // TODO: 显示设置
              },
            ),
          ],
        ),
        actions: [
          TextButton(
            onPressed: () => Navigator.of(context).pop(),
            child: const Text('关闭'),
          ),
        ],
      ),
    );
  }
}
```

---

## 🚀 Riverpod 高级特性

### 1. Family Modifier（参数化 Provider）

```dart
// 为任意玩家 ID 创建 Provider
final playerProvider = Provider.family<Player, int>((ref, playerId) {
  final gameState = ref.watch(gameStateProvider);
  return playerId == 1 ? gameState.player1 : gameState.player2;
});

// 使用
final player1 = ref.watch(playerProvider(1));
final player2 = ref.watch(playerProvider(2));
```

### 2. AutoDispose（自动清理）

```dart
// 当没有监听者时自动清理
final tempDataProvider = StateNotifierProvider.autoDispose<
    TempDataNotifier,
    TempData
>((ref) {
  final notifier = TempDataNotifier();

  // 清理逻辑
  ref.onDispose(() {
    notifier.cleanup();
    print('Provider disposed');
  });

  return notifier;
});
```

### 3. Select（精确订阅）

```dart
// 只监听生命值变化，不监听其他字段
final lifePoints = ref.watch(
  player1Provider.select((player) => player.lifePoints),
);

// 只有 lifePoints 变化时才重建 Widget
```

### 4. Ref.listen（副作用监听）

```dart
@override
Widget build(BuildContext context, WidgetRef ref) {
  // 监听状态变化并执行副作用
  ref.listen<Player>(player1Provider, (previous, next) {
    if (previous != null && next.lifePoints < previous.lifePoints) {
      // 生命值减少时播放音效
      AudioService.playDamageSound();
    }
  });

  return Container();
}
```

### 5. Provider 组合

```dart
// Provider 可以依赖其他 Provider
final totalLifePointsProvider = Provider<int>((ref) {
  final player1LP = ref.watch(player1Provider).lifePoints;
  final player2LP = ref.watch(player2Provider).lifePoints;
  return player1LP + player2LP;
});
```

---

## 📊 状态管理架构总结

### ✅ Riverpod 核心优势

1. **编译时安全** - 所有错误在编译时捕获，减少运行时错误
2. **无需 BuildContext** - 在任何地方访问状态，不受 Widget 树限制
3. **细粒度控制** - 精确控制 Widget 重建范围，优化性能
4. **易于测试** - Provider 可以轻松 mock，单元测试友好
5. **强大的组合能力** - Provider 之间可以互相依赖和组合
6. **自动依赖追踪** - 框架自动管理 Provider 依赖关系
7. **内置异步支持** - FutureProvider、StreamProvider 开箱即用

### 📁 完整文件清单

```
lib/features/duel/
├─ models/
│  ├─ player.dart                    # ✅ 玩家模型
│  ├─ life_points_change.dart        # ✅ 历史记录模型
│  ├─ game_state.dart                # ✅ 游戏状态模型
│  └─ particle.dart                  # ✅ 粒子模型
│
├─ providers/
│  ├─ game_state_notifier.dart       # ✅ 游戏状态 Notifier
│  ├─ particle_system_notifier.dart  # ✅ 粒子系统 Notifier
│  └─ game_providers.dart            # ✅ Provider 定义集合
│
└─ presentation/
   ├─ screens/
   │  └─ duel_screen.dart            # ✅ 主屏幕
   └─ widgets/
      ├─ life_points_display.dart    # ✅ 生命值显示
      └─ swipe_input_area.dart       # ✅ 滑动输入区域
```

### 🎯 实现检查清单

**Phase 1 - MVP 状态管理：**
- [x] 数据模型定义（Player, GameState, LifePointsChange）
- [x] GameStateNotifier 实现
- [x] Provider 定义
- [x] 基础 UI 组件集成
- [ ] 单元测试编写
- [ ] 集成测试编写

**Phase 2 - 视觉增强：**
- [x] ParticleSystemNotifier 实现
- [x] 粒子系统与生命值自动联动
- [ ] AnimationNotifier 实现
- [ ] 动画状态管理

**Phase 3 - 功能扩充：**
- [ ] DiceProvider 实现（骰子功能）
- [ ] CounterProvider 实现（计数器功能）
- [ ] BeatVisualizationProvider 实现（节拍视觉化）

**Phase 4 - 蓝牙对战：**
- [ ] BluetoothProvider 实现
- [ ] 双机同步状态管理
- [ ] 网络状态处理

---

## 🧪 测试示例

### 单元测试（GameStateNotifier）

```dart
// test/features/duel/providers/game_state_notifier_test.dart
import 'package:flutter_test/flutter_test.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';

void main() {
  group('GameStateNotifier Tests', () {
    late ProviderContainer container;

    setUp(() {
      container = ProviderContainer();
    });

    tearDown(() {
      container.dispose();
    });

    test('初始状态正确', () {
      final gameState = container.read(gameStateProvider);

      expect(gameState.player1.lifePoints, 8000);
      expect(gameState.player2.lifePoints, 8000);
      expect(gameState.history, isEmpty);
      expect(gameState.isGameOver, false);
    });

    test('修改生命值正确', () {
      final notifier = container.read(gameStateProvider.notifier);

      notifier.changeLifePoints(1, -1000);

      final gameState = container.read(gameStateProvider);
      expect(gameState.player1.lifePoints, 7000);
      expect(gameState.history.length, 1);
    });

    test('撤销操作正确', () {
      final notifier = container.read(gameStateProvider.notifier);

      notifier.changeLifePoints(1, -1000);
      notifier.undo();

      final gameState = container.read(gameStateProvider);
      expect(gameState.player1.lifePoints, 8000);
      expect(gameState.history, isEmpty);
    });

    test('游戏结束判断正确', () {
      final notifier = container.read(gameStateProvider.notifier);

      notifier.changeLifePoints(1, -8000);

      final gameState = container.read(gameStateProvider);
      expect(gameState.isGameOver, true);
      expect(gameState.winner, gameState.player2);
    });
  });
}
```

---

## 📚 参考资源

### 官方文档
- Riverpod 官方文档：https://riverpod.dev
- Flutter Riverpod 包：https://pub.dev/packages/flutter_riverpod

### 推荐阅读
- Riverpod 最佳实践
- StateNotifier 模式详解
- Provider 组合策略

---

## ✅ 总结

使用 **Riverpod** 的状态管理架构为 Yu-Gi-Oh Duel Console 提供了：

1. ✅ **类型安全的状态管理**
2. ✅ **清晰的数据流向**（数据模型 → Notifier → Provider → UI）
3. ✅ **易于测试和维护**
4. ✅ **高性能的细粒度更新**
5. ✅ **强大的组合和扩展能力**

---

**🚀 Generated with BMad Method**
**📅 Date: 2025-11-13**
**👤 Author: yyx**
**🤖 Agent: Claude Code**
