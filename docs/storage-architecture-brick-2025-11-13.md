# Brick 数据存储架构详解

**专案名称：** bmad2 (Yu-Gi-Oh Duel Console)
**文档类型：** 数据存储架构设计（Brick）
**日期：** 2025-11-13
**作者：** yyx
**版本：** 1.0

---

## 📋 概要

本文档详细说明 Yu-Gi-Oh Duel Console 使用 **Brick** 进行数据持久化的完整架构设计。Brick 是一个强大的离线优先（offline-first）数据持久化框架，支持多数据源（SQLite + REST API + Memory Cache）统一接口。

---

## 🎯 为什么选择 Brick？

### Brick 核心优势

```
┌─────────────────────────────────────────────────────────┐
│                  Brick 架构特点                          │
└─────────────────────────────────────────────────────────┘

✅ 离线优先（Offline-First）
   - 优先使用本地缓存
   - 自动同步到远程（可选）
   - 网络恢复后自动重试

✅ 多数据源统一接口
   - SQLite（本地持久化）
   - REST API（远程服务，可选）
   - Memory（内存缓存）

✅ 自动代码生成
   - 使用注解定义模型
   - 自动生成适配器
   - 减少样板代码

✅ 强大的查询能力
   - 类型安全的查询 API
   - 支持复杂条件（where, orderBy, limit）
   - 自动优化查询性能

✅ 关系支持
   - 一对一、一对多、多对多
   - 自动加载关联数据
   - 级联操作
```

---

### Brick vs 其他方案对比

| 特性 | Brick | Hive | Isar | SQLite | 评价 |
|------|-------|------|------|--------|------|
| **离线优先** | ✅ 内置 | ❌ | ❌ | ❌ | Brick 独有 |
| **多数据源** | ✅ SQLite+REST | ❌ | ❌ | ❌ | Brick 独有 |
| **类型安全** | ✅ | ✅ | ✅ | 🟡 | 都支持 |
| **关系支持** | ✅ 强大 | ❌ | ✅ | ✅ | Brick/Isar/SQLite |
| **REST 集成** | ✅ 内置 | ❌ | ❌ | ❌ | Brick 独有 |
| **代码生成** | ✅ | ✅ | ✅ | ❌ | 现代化方案 |
| **学习曲线** | 🟡 中等 | ✅ 简单 | ✅ 简单 | 🟡 中等 | - |
| **性能** | ✅ 优秀 | ✅ 极快 | ✅ 最快 | ✅ 优秀 | 都很好 |
| **跨平台** | ✅ | ✅ | ✅ | ✅ | 完全支持 |

**选择 Brick 的理由：**
1. ✅ 为未来扩展预留空间（可轻松添加云端同步）
2. ✅ 统一的数据访问接口（本地 + 远程）
3. ✅ 强大的关系支持（对战记录 ↔ 生命值变化记录）
4. ✅ 离线优先策略（确保应用在无网络时完美运行）
5. ✅ 自动代码生成（减少开发工作量）

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

  # 状态管理
  flutter_riverpod: ^2.5.1
  riverpod_annotation: ^2.3.5

  # 数据存储 - Brick
  brick_offline_first: ^3.1.1        # Brick 核心（离线优先）
  brick_sqlite: ^3.1.0               # SQLite Provider
  brick_rest: ^3.0.1                 # REST API Provider（可选）
  sqflite: ^2.3.3+1                  # SQLite 底层依赖

  # 轻量设置存储
  shared_preferences: ^2.2.3         # 简单键值对

  # 工具库
  uuid: ^4.4.0                       # UUID 生成
  path: ^1.9.0                       # 路径处理

  # 蓝牙连接（Phase 4）
  flutter_blue_plus: ^1.32.12

  # 权限管理
  permission_handler: ^11.3.1

dev_dependencies:
  flutter_test:
    sdk: flutter
  flutter_lints: ^5.0.0

  # 代码生成
  build_runner: ^2.4.9
  riverpod_generator: ^2.4.0

  # Brick 代码生成
  brick_offline_first_build: ^3.0.2  # Brick 构建工具

flutter:
  uses-material-design: true
```

---

## 📁 目录结构

```
lib/
├─ brick/                           # Brick 配置目录（自动生成）
│  ├─ brick.g.dart                  # 主生成文件
│  ├─ db/
│  │  └─ schema.g.dart              # 数据库 Schema
│  └─ adapters/                     # 模型适配器（自动生成）
│     ├─ match_history_adapter.g.dart
│     ├─ player_adapter.g.dart
│     ├─ statistics_adapter.g.dart
│     └─ life_points_change_adapter.g.dart
│
├─ core/
│  ├─ storage/
│  │  ├─ brick_repository.dart      # Brick Repository 配置
│  │  ├─ preferences_service.dart   # SharedPreferences 封装
│  │  └─ storage_providers.dart     # 存储层 Riverpod Provider
│  │
│  └─ models/                       # 共享数据模型
│     └─ base_model.dart            # 基础模型（可选）
│
├─ features/
│  ├─ duel/
│  │  ├─ models/                    # 数据模型（带 Brick 注解）
│  │  │  ├─ match_history.dart      # 对战历史模型
│  │  │  ├─ player.dart             # 玩家模型
│  │  │  ├─ statistics.dart         # 统计数据模型
│  │  │  └─ life_points_change.dart # 生命值变化记录
│  │  │
│  │  └─ repositories/              # 业务仓库层
│  │     ├─ match_repository.dart   # 对战记录仓库
│  │     └─ statistics_repository.dart # 统计数据仓库
│  │
│  └─ settings/
│     ├─ models/
│     │  └─ app_settings.dart       # 应用设置（SharedPreferences）
│     └─ repositories/
│        └─ settings_repository.dart # 设置仓库
│
└─ main.dart                        # 应用入口
```

---

## 🎨 数据模型定义（使用 Brick 注解）

### 1. Player 模型

```dart
// lib/features/duel/models/player.dart
import 'package:brick_offline_first/brick_offline_first.dart';
import 'package:brick_sqlite/brick_sqlite.dart';

/// 玩家模型
@ConnectOfflineFirstWithRest(
  restConfig: RestSerializable(
    endpoint: '=> "/players"',
  ),
  sqliteConfig: SqliteSerializable(
    nullable: false,
  ),
)
class Player extends OfflineFirstWithRestModel {
  /// 主键 UUID
  @Sqlite(unique: true)
  final String? id;

  /// 玩家名称
  final String name;

  /// 玩家颜色（存储为 HEX 字符串）
  @Sqlite(name: 'player_color')
  final String colorHex;

  /// 当前生命值
  @Sqlite(name: 'life_points')
  int lifePoints;

  /// 创建时间
  @Sqlite(name: 'created_at')
  final DateTime createdAt;

  /// 更新时间
  @Sqlite(name: 'updated_at')
  DateTime updatedAt;

  Player({
    this.id,
    required this.name,
    required this.colorHex,
    this.lifePoints = 8000,
    DateTime? createdAt,
    DateTime? updatedAt,
  })  : createdAt = createdAt ?? DateTime.now(),
        updatedAt = updatedAt ?? DateTime.now();

  /// 生命值状态
  LifePointsStatus get status {
    if (lifePoints >= 6000) return LifePointsStatus.high;
    if (lifePoints >= 2000) return LifePointsStatus.medium;
    if (lifePoints >= 500) return LifePointsStatus.low;
    return LifePointsStatus.critical;
  }

  /// 呼吸周期（秒）- 根据生命值动态调整
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

  /// 粒子目标数量
  int get particleTargetCount {
    return (lifePoints / 10).clamp(100, 800).toInt();
  }

  @override
  String toString() => 'Player(id: $id, name: $name, LP: $lifePoints)';
}

/// 生命值状态枚举
enum LifePointsStatus {
  high,      // 6000+ （安全）
  medium,    // 2000-6000 （正常）
  low,       // 500-2000 （危险）
  critical,  // <500 （濒死）
}
```

**Brick 注解说明：**
- `@ConnectOfflineFirstWithRest` - 定义为离线优先模型
- `@Sqlite(unique: true)` - SQLite 唯一索引
- `@Sqlite(name: 'column_name')` - 自定义列名
- `restConfig` - REST API 端点配置（可选）
- `sqliteConfig` - SQLite 配置

---

### 2. LifePointsChange 模型（生命值变化记录）

```dart
// lib/features/duel/models/life_points_change.dart
import 'package:brick_offline_first/brick_offline_first.dart';
import 'package:brick_sqlite/brick_sqlite.dart';

/// 生命值变化记录
@ConnectOfflineFirstWithRest(
  restConfig: RestSerializable(
    endpoint: '=> "/life_points_changes"',
  ),
)
class LifePointsChange extends OfflineFirstWithRestModel {
  @Sqlite(unique: true)
  final String? id;

  /// 对战 ID（外键）
  @Sqlite(name: 'match_id', index: true)
  final String matchId;

  /// 玩家 ID（1 或 2）
  @Sqlite(name: 'player_id')
  final int playerId;

  /// 旧值
  @Sqlite(name: 'old_value')
  final int oldValue;

  /// 新值
  @Sqlite(name: 'new_value')
  final int newValue;

  /// 变化量
  final int delta;

  /// 时间戳
  final DateTime timestamp;

  /// 变化类型
  @Sqlite(name: 'change_type')
  final String changeType; // 'swipe', 'button', 'bluetooth', 'undo', 'reset'

  LifePointsChange({
    this.id,
    required this.matchId,
    required this.playerId,
    required this.oldValue,
    required this.newValue,
    required this.delta,
    DateTime? timestamp,
    this.changeType = 'swipe',
  }) : timestamp = timestamp ?? DateTime.now();

  /// 是否增加
  bool get isIncrease => delta > 0;

  /// 是否减少
  bool get isDecrease => delta < 0;

  /// 绝对值
  int get absoluteDelta => delta.abs();

  @override
  String toString() {
    final sign = isIncrease ? '+' : '';
    return 'Change(P$playerId: $oldValue → $newValue, $sign$delta)';
  }
}
```

**关键特性：**
- `@Sqlite(index: true)` - 为 `match_id` 创建索引，优化查询性能
- 外键关系 - `matchId` 关联到 `MatchHistory.id`

---

### 3. MatchHistory 模型（对战历史）

```dart
// lib/features/duel/models/match_history.dart
import 'package:brick_offline_first/brick_offline_first.dart';
import 'package:brick_sqlite/brick_sqlite.dart';
import 'life_points_change.dart';

/// 对战历史记录
@ConnectOfflineFirstWithRest(
  restConfig: RestSerializable(
    endpoint: '=> "/matches"',
  ),
  sqliteConfig: SqliteSerializable(
    nullable: false,
  ),
)
class MatchHistory extends OfflineFirstWithRestModel {
  /// 主键 UUID
  @Sqlite(unique: true)
  final String? id;

  /// 开始时间
  @Sqlite(name: 'start_time', index: true)
  final DateTime startTime;

  /// 结束时间
  @Sqlite(name: 'end_time')
  DateTime? endTime;

  /// 玩家 1 名称
  @Sqlite(name: 'player1_name')
  final String player1Name;

  /// 玩家 2 名称
  @Sqlite(name: 'player2_name')
  final String player2Name;

  /// 玩家 1 初始生命值
  @Sqlite(name: 'player1_initial_lp')
  final int player1InitialLP;

  /// 玩家 2 初始生命值
  @Sqlite(name: 'player2_initial_lp')
  final int player2InitialLP;

  /// 玩家 1 最终生命值
  @Sqlite(name: 'player1_final_lp')
  int player1FinalLP;

  /// 玩家 2 最终生命值
  @Sqlite(name: 'player2_final_lp')
  int player2FinalLP;

  /// 获胜者 ID（'1' 或 '2'，null 表示未结束）
  @Sqlite(name: 'winner_id')
  String? winnerId;

  /// 游戏模式
  @Sqlite(name: 'game_mode')
  final String mode; // 'local' 或 'bluetooth'

  /// 对战时长（毫秒）
  @Sqlite(name: 'duration_ms')
  int? durationMs;

  /// 关联的生命值变化记录（一对多关系）
  @OfflineFirst(where: {'matchId': "data['id']"})
  final List<LifePointsChange>? changes;

  MatchHistory({
    this.id,
    DateTime? startTime,
    this.endTime,
    required this.player1Name,
    required this.player2Name,
    this.player1InitialLP = 8000,
    this.player2InitialLP = 8000,
    this.player1FinalLP = 8000,
    this.player2FinalLP = 8000,
    this.winnerId,
    this.mode = 'local',
    this.durationMs,
    this.changes,
  }) : startTime = startTime ?? DateTime.now();

  /// 是否已完成
  bool get isCompleted => endTime != null;

  /// 对战时长
  Duration? get duration {
    if (durationMs == null) return null;
    return Duration(milliseconds: durationMs!);
  }

  /// 设置对战时长
  set duration(Duration? value) {
    durationMs = value?.inMilliseconds;
  }

  /// 获胜者名称
  String? get winnerName {
    if (winnerId == null) return null;
    return winnerId == '1' ? player1Name : player2Name;
  }

  /// 失败者名称
  String? get loserName {
    if (winnerId == null) return null;
    return winnerId == '1' ? player2Name : player1Name;
  }

  /// 格式化时长
  String get formattedDuration {
    if (duration == null) return '--:--';
    final minutes = duration!.inMinutes;
    final seconds = duration!.inSeconds % 60;
    return '${minutes.toString().padLeft(2, '0')}:${seconds.toString().padLeft(2, '0')}';
  }

  @override
  String toString() => 'Match($id: $player1Name vs $player2Name)';
}
```

**一对多关系：**
```dart
@OfflineFirst(where: {'matchId': "data['id']"})
final List<LifePointsChange>? changes;
```
- 自动加载关联的生命值变化记录
- `where` 子句定义关联条件

---

### 4. Statistics 模型（统计数据）

```dart
// lib/features/duel/models/statistics.dart
import 'package:brick_offline_first/brick_offline_first.dart';
import 'package:brick_sqlite/brick_sqlite.dart';
import 'dart:convert';
import 'match_history.dart';

/// 统计数据
@ConnectOfflineFirstWithRest(
  restConfig: RestSerializable(
    endpoint: '=> "/statistics"',
  ),
)
class Statistics extends OfflineFirstWithRestModel {
  @Sqlite(unique: true)
  final String? id;

  /// 总对战次数
  @Sqlite(name: 'total_matches')
  int totalMatches;

  /// 总胜场
  @Sqlite(name: 'total_wins')
  int totalWins;

  /// 总败场
  @Sqlite(name: 'total_losses')
  int totalLosses;

  /// 总游戏时长（毫秒）
  @Sqlite(name: 'total_play_time_ms')
  int totalPlayTimeMs;

  /// 总生命值变化量
  @Sqlite(name: 'total_lp_changed')
  int totalLifePointsChanged;

  /// 最后游戏日期
  @Sqlite(name: 'last_played_at')
  DateTime? lastPlayedDate;

  /// 每日对战统计（存储为 JSON 字符串）
  @Sqlite(
    name: 'daily_matches_json',
    fromGenerator: 'jsonEncode(%DATA_PROPERTY%)',
    toGenerator: 'Map<String, int>.from(jsonDecode(%DATA_PROPERTY% ?? "{}") as Map)',
  )
  Map<String, int> dailyMatches;

  Statistics({
    this.id = 'main',
    this.totalMatches = 0,
    this.totalWins = 0,
    this.totalLosses = 0,
    this.totalPlayTimeMs = 0,
    this.totalLifePointsChanged = 0,
    this.lastPlayedDate,
    Map<String, int>? dailyMatches,
  }) : dailyMatches = dailyMatches ?? {};

  /// 总游戏时长
  Duration get totalPlayTime {
    return Duration(milliseconds: totalPlayTimeMs);
  }

  set totalPlayTime(Duration value) {
    totalPlayTimeMs = value.inMilliseconds;
  }

  /// 胜率
  double get winRate {
    if (totalMatches == 0) return 0.0;
    return (totalWins / totalMatches) * 100;
  }

  /// 平均对战时长
  Duration get averageMatchDuration {
    if (totalMatches == 0) return Duration.zero;
    return Duration(
      milliseconds: totalPlayTimeMs ~/ totalMatches,
    );
  }

  /// 今日对战次数
  int get todayMatches {
    final today = DateTime.now().toIso8601String().split('T')[0];
    return dailyMatches[today] ?? 0;
  }

  /// 更新统计（基于对战记录）
  void updateWithMatch(MatchHistory match, bool isWin) {
    totalMatches++;
    if (isWin) {
      totalWins++;
    } else {
      totalLosses++;
    }

    if (match.duration != null) {
      totalPlayTimeMs += match.duration!.inMilliseconds;
    }

    lastPlayedDate = DateTime.now();

    // 更新每日统计
    final today = DateTime.now().toIso8601String().split('T')[0];
    dailyMatches[today] = (dailyMatches[today] ?? 0) + 1;
  }

  @override
  String toString() => 'Stats(Matches: $totalMatches, Wins: $totalWins, Rate: ${winRate.toStringAsFixed(1)}%)';
}
```

**JSON 字段存储：**
```dart
@Sqlite(
  name: 'daily_matches_json',
  fromGenerator: 'jsonEncode(%DATA_PROPERTY%)',      // 存储时编码
  toGenerator: 'Map<String, int>.from(jsonDecode(%DATA_PROPERTY% ?? "{}") as Map)', // 读取时解码
)
Map<String, int> dailyMatches;
```

---

## 🔧 Brick Repository 配置

### brick_repository.dart

```dart
// lib/core/storage/brick_repository.dart
import 'package:brick_offline_first/brick_offline_first.dart';
import 'package:brick_sqlite/brick_sqlite.dart';
import 'package:brick_rest/brick_rest.dart';
import 'package:sqflite/sqflite.dart';
import 'package:path/path.dart';
import '../../brick/brick.g.dart' show migrations, restModelDictionary;

/// Brick Repository 单例
///
/// 提供统一的数据访问接口，支持：
/// - SQLite（本地持久化）
/// - REST API（远程同步，可选）
/// - Memory Cache（内存缓存）
class BrickRepository extends OfflineFirstWithRestRepository {
  static BrickRepository? _instance;

  BrickRepository._({
    required super.sqliteProvider,
    required super.restProvider,
    required super.migrations,
    required super.offlineRequestQueue,
    super.memoryCacheProvider,
  });

  /// 获取单例实例
  factory BrickRepository() => _instance!;

  /// 初始化 Repository
  static Future<BrickRepository> initialize() async {
    if (_instance != null) return _instance!;

    print('🚀 初始化 Brick Repository...');

    // 1. 初始化 SQLite Provider
    final databasePath = await getDatabasesPath();
    final path = join(databasePath, 'duel_console.sqlite');

    print('📂 数据库路径: $path');

    final sqliteProvider = SqliteProvider(
      path,
      databaseFactory: databaseFactory,
    );

    // 2. 初始化 REST Provider（可选，如果需要远程同步）
    // 在 Phase 5 可以启用此功能，实现云端备份
    final restProvider = RestProvider(
      'https://api.example.com', // 你的 API 端点
      modelDictionary: restModelDictionary,
    );

    // 3. 创建离线请求队列
    // 自动处理离线时的请求，网络恢复后自动重试
    final offlineQueue = RestOfflineRequestQueue(
      client: restProvider.client,
    );

    // 4. 创建 Repository 实例
    _instance = BrickRepository._(
      sqliteProvider: sqliteProvider,
      restProvider: restProvider,
      migrations: migrations,
      offlineRequestQueue: offlineQueue,
      memoryCacheProvider: MemoryCacheProvider(),
    );

    // 5. 运行数据库迁移
    print('🔄 运行数据库迁移...');
    await _instance!.migrate();

    print('✅ Brick Repository 初始化完成');

    return _instance!;
  }

  /// 关闭数据库连接
  Future<void> close() async {
    await sqliteProvider.close();
    print('🔒 数据库已关闭');
  }

  /// 清空所有数据（用于测试或重置）
  Future<void> clearAll() async {
    await sqliteProvider.resetDb();
    print('🗑️ 所有数据已清空');
  }

  /// 获取数据库大小（字节）
  Future<int> getDatabaseSize() async {
    final dbPath = join(await getDatabasesPath(), 'duel_console.sqlite');
    final file = File(dbPath);
    if (await file.exists()) {
      return await file.length();
    }
    return 0;
  }

  /// 导出数据库文件（用于备份）
  Future<String> exportDatabase() async {
    final dbPath = join(await getDatabasesPath(), 'duel_console.sqlite');
    final backupDir = await getApplicationDocumentsDirectory();
    final timestamp = DateTime.now().toIso8601String().replaceAll(':', '-');
    final backupPath = join(backupDir.path, 'backup_$timestamp.sqlite');

    await File(dbPath).copy(backupPath);
    print('💾 数据库已备份到: $backupPath');

    return backupPath;
  }
}
```

**关键方法：**
- `initialize()` - 初始化单例
- `close()` - 关闭数据库
- `clearAll()` - 清空所有数据
- `getDatabaseSize()` - 获取数据库大小
- `exportDatabase()` - 导出备份

---

## 📚 Repository 层（业务仓库）

### 1. MatchRepository（对战记录仓库）

```dart
// lib/features/duel/repositories/match_repository.dart
import 'package:brick_offline_first/brick_offline_first.dart';
import 'package:uuid/uuid.dart';
import '../models/match_history.dart';
import '../models/life_points_change.dart';
import '../../../core/storage/brick_repository.dart';

/// 对战记录仓库
///
/// 提供对战历史的 CRUD 操作
class MatchRepository {
  final BrickRepository _repository;
  final _uuid = const Uuid();

  MatchRepository(this._repository);

  /// 创建新对战
  Future<MatchHistory> createMatch({
    required String player1Name,
    required String player2Name,
    int player1InitialLP = 8000,
    int player2InitialLP = 8000,
    String mode = 'local',
  }) async {
    final match = MatchHistory(
      id: _uuid.v4(),
      startTime: DateTime.now(),
      player1Name: player1Name,
      player2Name: player2Name,
      player1InitialLP: player1InitialLP,
      player2InitialLP: player2InitialLP,
      player1FinalLP: player1InitialLP,
      player2FinalLP: player2InitialLP,
      mode: mode,
    );

    // 保存到数据库
    await _repository.upsert<MatchHistory>(match);
    print('✅ 对战已创建: ${match.id}');

    return match;
  }

  /// 更新对战记录
  Future<void> updateMatch(MatchHistory match) async {
    await _repository.upsert<MatchHistory>(match);
  }

  /// 完成对战
  Future<void> finishMatch({
    required String matchId,
    required int player1FinalLP,
    required int player2FinalLP,
    String? winnerId,
  }) async {
    final match = await getMatchById(matchId);
    if (match == null) return;

    match.endTime = DateTime.now();
    match.player1FinalLP = player1FinalLP;
    match.player2FinalLP = player2FinalLP;
    match.winnerId = winnerId;
    match.duration = match.endTime!.difference(match.startTime);

    await updateMatch(match);
    print('🏁 对战已完成: ${match.id} (Winner: P$winnerId)');
  }

  /// 添加生命值变化记录
  Future<void> addLifePointsChange({
    required String matchId,
    required int playerId,
    required int oldValue,
    required int newValue,
    required int delta,
    String changeType = 'swipe',
  }) async {
    final change = LifePointsChange(
      id: _uuid.v4(),
      matchId: matchId,
      playerId: playerId,
      oldValue: oldValue,
      newValue: newValue,
      delta: delta,
      changeType: changeType,
    );

    await _repository.upsert<LifePointsChange>(change);
  }

  /// 获取单个对战记录（带关联数据）
  Future<MatchHistory?> getMatchById(String id) async {
    final matches = await _repository.get<MatchHistory>(
      query: Query.where('id', id, limit1: true),
    );
    return matches.isEmpty ? null : matches.first;
  }

  /// 获取所有对战记录（按时间倒序）
  Future<List<MatchHistory>> getAllMatches() async {
    return await _repository.get<MatchHistory>(
      query: Query(
        providerArgs: {
          'orderBy': 'start_time DESC',
        },
      ),
    );
  }

  /// 获取最近的对战记录
  Future<List<MatchHistory>> getRecentMatches({int limit = 10}) async {
    return await _repository.get<MatchHistory>(
      query: Query(
        providerArgs: {
          'orderBy': 'start_time DESC',
          'limit': limit,
        },
      ),
    );
  }

  /// 获取今日对战记录
  Future<List<MatchHistory>> getTodayMatches() async {
    final today = DateTime.now();
    final startOfDay = DateTime(today.year, today.month, today.day);

    return await _repository.get<MatchHistory>(
      query: Query.where(
        'startTime',
        Compare.greaterThanOrEqualTo,
        startOfDay,
        providerArgs: {
          'orderBy': 'start_time DESC',
        },
      ),
    );
  }

  /// 搜索对战（按玩家名称）
  Future<List<MatchHistory>> searchByPlayerName(String name) async {
    final allMatches = await getAllMatches();
    return allMatches.where((match) {
      return match.player1Name.contains(name) ||
          match.player2Name.contains(name);
    }).toList();
  }

  /// 获取对战时间范围
  Future<List<MatchHistory>> getMatchesByDateRange(
    DateTime start,
    DateTime end,
  ) async {
    return await _repository.get<MatchHistory>(
      query: Query.where(
        'startTime',
        Compare.between,
        [start, end],
        providerArgs: {
          'orderBy': 'start_time DESC',
        },
      ),
    );
  }

  /// 删除对战记录
  Future<void> deleteMatch(String id) async {
    final match = await getMatchById(id);
    if (match != null) {
      await _repository.delete<MatchHistory>(match);
      print('🗑️ 对战已删除: $id');
    }
  }

  /// 获取总对战数
  Future<int> getTotalMatches() async {
    final matches = await getAllMatches();
    return matches.length;
  }

  /// 清空所有记录
  Future<void> clearAll() async {
    final matches = await getAllMatches();
    for (final match in matches) {
      await _repository.delete<MatchHistory>(match);
    }
    print('🗑️ 所有对战记录已清空');
  }
}
```

**Brick 查询示例：**
```dart
// 简单查询
Query.where('id', matchId, limit1: true)

// 范围查询
Query.where('startTime', Compare.greaterThanOrEqualTo, startDate)

// 排序和限制
Query(providerArgs: {
  'orderBy': 'start_time DESC',
  'limit': 10,
})
```

---

### 2. StatisticsRepository（统计数据仓库）

```dart
// lib/features/duel/repositories/statistics_repository.dart
import 'package:brick_offline_first/brick_offline_first.dart';
import '../models/statistics.dart';
import '../models/match_history.dart';
import '../../../core/storage/brick_repository.dart';

/// 统计数据仓库
class StatisticsRepository {
  final BrickRepository _repository;
  static const String mainStatsId = 'main';

  StatisticsRepository(this._repository);

  /// 获取主统计数据
  Future<Statistics> getStatistics() async {
    final stats = await _repository.get<Statistics>(
      query: Query.where('id', mainStatsId, limit1: true),
    );

    if (stats.isEmpty) {
      // 创建默认统计
      final newStats = Statistics(id: mainStatsId);
      await _repository.upsert<Statistics>(newStats);
      print('📊 创建默认统计数据');
      return newStats;
    }

    return stats.first;
  }

  /// 更新统计（基于对战记录）
  Future<void> updateStatistics(
    MatchHistory match, {
    required bool isWin,
  }) async {
    final stats = await getStatistics();
    stats.updateWithMatch(match, isWin);
    await _repository.upsert<Statistics>(stats);
    print('📊 统计数据已更新');
  }

  /// 手动保存统计
  Future<void> saveStatistics(Statistics stats) async {
    await _repository.upsert<Statistics>(stats);
  }

  /// 重置统计数据
  Future<void> resetStatistics() async {
    final newStats = Statistics(id: mainStatsId);
    await _repository.upsert<Statistics>(newStats);
    print('🔄 统计数据已重置');
  }

  /// 获取胜率
  Future<double> getWinRate() async {
    final stats = await getStatistics();
    return stats.winRate;
  }

  /// 获取今日对战次数
  Future<int> getTodayMatches() async {
    final stats = await getStatistics();
    return stats.todayMatches;
  }

  /// 获取周统计（最近7天）
  Future<Map<String, int>> getWeeklyStats() async {
    final stats = await getStatistics();
    final result = <String, int>{};
    final now = DateTime.now();

    for (int i = 6; i >= 0; i--) {
      final date = now.subtract(Duration(days: i));
      final dateKey = date.toIso8601String().split('T')[0];
      result[dateKey] = stats.dailyMatches[dateKey] ?? 0;
    }

    return result;
  }
}
```

---

## 🎯 Riverpod 集成

### storage_providers.dart

```dart
// lib/core/storage/storage_providers.dart
import 'package:flutter_riverpod/flutter_riverpod.dart';
import 'brick_repository.dart';
import 'preferences_service.dart';
import '../../features/duel/repositories/match_repository.dart';
import '../../features/duel/repositories/statistics_repository.dart';
import '../../features/settings/repositories/settings_repository.dart';
import '../../features/duel/models/statistics.dart';
import '../../features/duel/models/match_history.dart';
import '../../features/settings/models/app_settings.dart';

// ============================================================
// Brick Repository Provider
// ============================================================

/// BrickRepository Provider（单例）
/// 必须在 main() 中初始化后覆盖
final brickRepositoryProvider = Provider<BrickRepository>((ref) {
  throw UnimplementedError(
    'BrickRepository must be initialized in main() using overrideWithValue',
  );
});

// ============================================================
// 仓库层 Provider
// ============================================================

/// MatchRepository Provider
final matchRepositoryProvider = Provider<MatchRepository>((ref) {
  final brick = ref.watch(brickRepositoryProvider);
  return MatchRepository(brick);
});

/// StatisticsRepository Provider
final statisticsRepositoryProvider = Provider<StatisticsRepository>((ref) {
  final brick = ref.watch(brickRepositoryProvider);
  return StatisticsRepository(brick);
});

/// SettingsRepository Provider（使用 SharedPreferences）
final settingsRepositoryProvider = Provider<SettingsRepository>((ref) {
  final prefs = ref.watch(preferencesServiceProvider);
  return SettingsRepository(prefs);
});

/// PreferencesService Provider
final preferencesServiceProvider = Provider<PreferencesService>((ref) {
  throw UnimplementedError('PreferencesService must be overridden');
});

// ============================================================
// 数据层 Provider（异步）
// ============================================================

/// 统计数据 Provider（FutureProvider）
final statisticsProvider = FutureProvider<Statistics>((ref) async {
  final repository = ref.watch(statisticsRepositoryProvider);
  return await repository.getStatistics();
});

/// 胜率 Provider
final winRateProvider = FutureProvider<double>((ref) async {
  final stats = await ref.watch(statisticsProvider.future);
  return stats.winRate;
});

/// 今日对战次数 Provider
final todayMatchesProvider = FutureProvider<int>((ref) async {
  final stats = await ref.watch(statisticsProvider.future);
  return stats.todayMatches;
});

/// 最近对战记录 Provider
final recentMatchesProvider = FutureProvider<List<MatchHistory>>((ref) async {
  final repository = ref.watch(matchRepositoryProvider);
  return await repository.getRecentMatches(limit: 10);
});

/// 今日对战记录 Provider
final todayMatchesListProvider = FutureProvider<List<MatchHistory>>((ref) async {
  final repository = ref.watch(matchRepositoryProvider);
  return await repository.getTodayMatches();
});

// ============================================================
// 设置 Provider（同步）
// ============================================================

/// 应用设置 Provider
final appSettingsProvider = StateNotifierProvider<AppSettingsNotifier, AppSettings>((ref) {
  final repository = ref.watch(settingsRepositoryProvider);
  return AppSettingsNotifier(repository);
});

class AppSettingsNotifier extends StateNotifier<AppSettings> {
  final SettingsRepository _repository;

  AppSettingsNotifier(this._repository) : super(_repository.loadSettings());

  Future<void> updateSettings(AppSettings settings) async {
    await _repository.saveSettings(settings);
    state = settings;
  }

  Future<void> toggleSound() async {
    final updated = state.copyWith(soundEnabled: !state.soundEnabled);
    await updateSettings(updated);
  }

  Future<void> toggleVibration() async {
    final updated = state.copyWith(vibrationEnabled: !state.vibrationEnabled);
    await updateSettings(updated);
  }

  Future<void> setStartingLifePoints(int value) async {
    final updated = state.copyWith(startingLifePoints: value);
    await updateSettings(updated);
  }

  Future<void> resetToDefaults() async {
    await _repository.resetToDefaults();
    state = AppSettings.defaultSettings();
  }
}
```

---

## 🚀 初始化流程

### main.dart

```dart
// lib/main.dart
import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';
import 'core/storage/brick_repository.dart';
import 'core/storage/preferences_service.dart';
import 'core/storage/storage_providers.dart';
import 'features/duel/presentation/screens/duel_screen.dart';

void main() async {
  // 确保 Flutter 绑定初始化
  WidgetsFlutterBinding.ensureInitialized();

  print('🚀 启动 Yu-Gi-Oh Duel Console...');

  try {
    // 1. 初始化 Brick Repository
    print('📦 初始化 Brick Repository...');
    final brickRepo = await BrickRepository.initialize();

    // 2. 初始化 SharedPreferences
    print('⚙️ 初始化 SharedPreferences...');
    final prefsService = PreferencesService();
    await prefsService.init();

    print('✅ 所有存储服务初始化完成');

    // 3. 运行应用
    runApp(
      ProviderScope(
        overrides: [
          // 覆盖 Provider，注入初始化后的实例
          brickRepositoryProvider.overrideWithValue(brickRepo),
          preferencesServiceProvider.overrideWithValue(prefsService),
        ],
        child: const DuelConsoleApp(),
      ),
    );
  } catch (e, stackTrace) {
    print('❌ 初始化失败: $e');
    print('Stack trace: $stackTrace');

    // 显示错误界面
    runApp(
      MaterialApp(
        home: Scaffold(
          body: Center(
            child: Text('初始化失败: $e'),
          ),
        ),
      ),
    );
  }
}

class DuelConsoleApp extends ConsumerWidget {
  const DuelConsoleApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context, WidgetRef ref) {
    // 监听应用设置
    final settings = ref.watch(appSettingsProvider);

    return MaterialApp(
      title: 'Yu-Gi-Oh Duel Console',
      theme: ThemeData.dark(),
      home: const DuelScreen(),
      debugShowCheckedModeBanner: false,
    );
  }
}
```

---

## ⚙️ 代码生成

### 1. build.yaml 配置（可选）

```yaml
# build.yaml
targets:
  $default:
    builders:
      brick_offline_first_build|brick_aggregate_builder:
        enabled: true
      brick_offline_first_build|brick_model_builder:
        enabled: true
```

### 2. 运行代码生成

```bash
# 清理旧的生成文件
flutter clean

# 获取依赖
flutter pub get

# 运行代码生成（一次性）
flutter pub run build_runner build

# 或者使用监听模式（推荐开发时使用）
flutter pub run build_runner watch

# 清理后重新生成（如果遇到问题）
flutter pub run build_runner build --delete-conflicting-outputs
```

### 3. 生成的文件

```
lib/brick/
├─ brick.g.dart                     # 主配置文件
├─ db/
│  └─ schema.g.dart                 # 数据库 Schema
└─ adapters/
   ├─ match_history_adapter.g.dart  # MatchHistory 适配器
   ├─ player_adapter.g.dart         # Player 适配器
   ├─ statistics_adapter.g.dart     # Statistics 适配器
   └─ life_points_change_adapter.g.dart # LifePointsChange 适配器
```

---

## 📊 使用示例

### 1. 在 GameStateNotifier 中集成

```dart
// lib/features/duel/providers/game_state_notifier.dart
import 'package:flutter_riverpod/flutter_riverpod.dart';
import '../models/game_state.dart';
import '../repositories/match_repository.dart';
import '../repositories/statistics_repository.dart';

class GameStateNotifier extends StateNotifier<GameState> {
  final MatchRepository _matchRepository;
  final StatisticsRepository _statsRepository;
  String? _currentMatchId;

  GameStateNotifier(
    this._matchRepository,
    this._statsRepository,
  ) : super(GameState.initial()) {
    _startNewMatch();
  }

  /// 开始新对战
  Future<void> _startNewMatch() async {
    final match = await _matchRepository.createMatch(
      player1Name: state.player1.name,
      player2Name: state.player2.name,
      player1InitialLP: state.player1.lifePoints,
      player2InitialLP: state.player2.lifePoints,
      mode: state.mode == GameMode.local ? 'local' : 'bluetooth',
    );
    _currentMatchId = match.id;
  }

  /// 修改生命值（自动保存）
  @override
  void changeLifePoints(int playerId, int delta, {ChangeType type = ChangeType.swipe}) {
    final oldValue = playerId == 1
        ? state.player1.lifePoints
        : state.player2.lifePoints;

    super.changeLifePoints(playerId, delta, type: type);

    // 保存生命值变化
    if (_currentMatchId != null) {
      _matchRepository.addLifePointsChange(
        matchId: _currentMatchId!,
        playerId: playerId,
        oldValue: oldValue,
        newValue: playerId == 1
            ? state.player1.lifePoints
            : state.player2.lifePoints,
        delta: delta,
        changeType: type.name,
      );
    }

    // 游戏结束时完成对战记录
    if (state.isGameOver && _currentMatchId != null) {
      _finishMatch();
    }
  }

  /// 完成对战
  Future<void> _finishMatch() async {
    await _matchRepository.finishMatch(
      matchId: _currentMatchId!,
      player1FinalLP: state.player1.lifePoints,
      player2FinalLP: state.player2.lifePoints,
      winnerId: state.winner?.id.toString(),
    );

    // 更新统计
    if (state.winner != null) {
      final match = await _matchRepository.getMatchById(_currentMatchId!);
      if (match != null) {
        await _statsRepository.updateStatistics(
          match,
          isWin: state.winner!.id == 1, // 假设统计 Player 1
        );
      }
    }
  }
}
```

---

### 2. 显示统计数据

```dart
// lib/features/statistics/presentation/widgets/statistics_card.dart
import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';
import '../../../../core/storage/storage_providers.dart';

class StatisticsCard extends ConsumerWidget {
  const StatisticsCard({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context, WidgetRef ref) {
    // 使用 AsyncValue 处理异步数据
    final statsAsync = ref.watch(statisticsProvider);

    return statsAsync.when(
      data: (stats) => Card(
        child: Padding(
          padding: const EdgeInsets.all(16),
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Text(
                '对战统计',
                style: Theme.of(context).textTheme.headlineSmall,
              ),
              const SizedBox(height: 16),
              _buildStatRow('总对战', '${stats.totalMatches} 场'),
              _buildStatRow('胜率', '${stats.winRate.toStringAsFixed(1)}%'),
              _buildStatRow('总胜场', '${stats.totalWins}'),
              _buildStatRow('总败场', '${stats.totalLosses}'),
              _buildStatRow('今日对战', '${stats.todayMatches} 场'),
              _buildStatRow(
                '平均时长',
                '${stats.averageMatchDuration.inMinutes} 分钟',
              ),
            ],
          ),
        ),
      ),
      loading: () => const Center(child: CircularProgressIndicator()),
      error: (error, stack) => Center(child: Text('加载失败: $error')),
    );
  }

  Widget _buildStatRow(String label, String value) {
    return Padding(
      padding: const EdgeInsets.symmetric(vertical: 4),
      child: Row(
        mainAxisAlignment: MainAxisAlignment.spaceBetween,
        children: [
          Text(label),
          Text(
            value,
            style: const TextStyle(fontWeight: FontWeight.bold),
          ),
        ],
      ),
    );
  }
}
```

---

### 3. 显示对战历史

```dart
// lib/features/history/presentation/screens/history_screen.dart
import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';
import '../../../../core/storage/storage_providers.dart';

class HistoryScreen extends ConsumerWidget {
  const HistoryScreen({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final matchesAsync = ref.watch(recentMatchesProvider);

    return Scaffold(
      appBar: AppBar(title: const Text('对战历史')),
      body: matchesAsync.when(
        data: (matches) {
          if (matches.isEmpty) {
            return const Center(child: Text('暂无对战记录'));
          }

          return ListView.builder(
            itemCount: matches.length,
            itemBuilder: (context, index) {
              final match = matches[index];
              return ListTile(
                title: Text('${match.player1Name} vs ${match.player2Name}'),
                subtitle: Text(
                  '${match.formattedDuration} | ${match.startTime.toString().split('.')[0]}',
                ),
                trailing: match.winnerId != null
                    ? Icon(
                        Icons.emoji_events,
                        color: Colors.amber,
                      )
                    : const Icon(Icons.play_arrow),
                onTap: () {
                  // 显示详情
                },
              );
            },
          );
        },
        loading: () => const Center(child: CircularProgressIndicator()),
        error: (error, stack) => Center(child: Text('加载失败: $error')),
      ),
    );
  }
}
```

---

## 📋 完整文件清单

```
✅ 数据模型（带 Brick 注解）
├─ lib/features/duel/models/player.dart
├─ lib/features/duel/models/life_points_change.dart
├─ lib/features/duel/models/match_history.dart
└─ lib/features/duel/models/statistics.dart

✅ 存储服务层
├─ lib/core/storage/brick_repository.dart
├─ lib/core/storage/preferences_service.dart
└─ lib/core/storage/storage_providers.dart

✅ 仓库层
├─ lib/features/duel/repositories/match_repository.dart
├─ lib/features/duel/repositories/statistics_repository.dart
└─ lib/features/settings/repositories/settings_repository.dart

✅ 自动生成文件（运行 build_runner 后）
├─ lib/brick/brick.g.dart
├─ lib/brick/db/schema.g.dart
└─ lib/brick/adapters/*.g.dart
```

---

## 🎯 最佳实践

### 1. 数据模型设计原则
- ✅ 使用 `@Sqlite(unique: true)` 定义主键
- ✅ 使用 `@Sqlite(index: true)` 为常查询字段创建索引
- ✅ 使用 `@Sqlite(name: 'column_name')` 自定义列名
- ✅ 使用 `@OfflineFirst(where: {...})` 定义关系
- ✅ 复杂类型使用 JSON 存储（fromGenerator/toGenerator）

### 2. Repository 模式
- ✅ 所有数据操作通过 Repository 层
- ✅ 使用 `upsert` 代替 insert/update
- ✅ 使用 Brick 的 Query API 进行查询
- ✅ 善用 `providerArgs` 传递 SQL 参数

### 3. Riverpod 集成
- ✅ 使用 FutureProvider 处理异步数据
- ✅ 使用 AsyncValue.when() 处理加载状态
- ✅ 在 main() 中初始化并覆盖 Provider
- ✅ 使用 ref.watch() 监听数据变化

### 4. 性能优化
- ✅ 为常查询字段创建索引
- ✅ 使用 limit1 优化单条查询
- ✅ 使用 Memory Cache 减少数据库访问
- ✅ 定期清理历史数据

---

## ✅ 总结

使用 **Brick** 的数据存储架构为 Yu-Gi-Oh Duel Console 提供了：

1. ✅ **离线优先** - 应用在无网络时完美运行
2. ✅ **多数据源统一接口** - SQLite + REST API + Memory
3. ✅ **强大的关系支持** - 一对多自动加载
4. ✅ **类型安全的查询** - 编译时检测错误
5. ✅ **自动代码生成** - 减少样板代码
6. ✅ **易于扩展** - 未来可轻松添加云端同步

**适用场景：**
- ✅ 需要本地持久化的应用
- ✅ 计划未来添加云端同步的应用
- ✅ 需要复杂查询和关系的应用
- ✅ 需要离线优先策略的应用

---

**🚀 Generated with BMad Method**
**📅 Date: 2025-11-13**
**👤 Author: yyx**
**🤖 Agent: Claude Code**
