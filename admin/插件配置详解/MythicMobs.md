```
# MythicMobs 配置详解

> 本文档面向管理员，介绍栖云野服务器中 MythicMobs 的配置结构与自定义内容。
>
> 玩家请查看 [自定义物品](../../player/自定义物品.md) 和 [副本攻略](../../player/副本攻略.md)。

---

## 一、插件信息

| 项目 | 值 |
|---|---|
| 插件名 | MythicMobs |
| 版本 | 5.13.0 |
| 作用 | 自定义怪物、技能、掉落、物品 |
| 目录 | `plugins/MythicMobs/` |

---

## 二、目录结构

```
plugins/MythicMobs/
├── mobs/                 # 怪物定义
├── skills/               # 技能定义
├── items/                # 自定义物品
├── DropTables/           # 掉落表
├── RandomSpawns/         # 随机生成
├── Spawners/             # 固定刷怪点
├── config.yml            # 主配置
└── config-spawning.yml   # 刷怪配置
```

---

## 三、栖云野自定义怪物

### 1. 骷髅王（SkeletonKingBoss）

| 项目 | 值 |
|---|---|
| 类型 | WITHER_SKELETON |
| 血量 | 1024 |
| 伤害 | 18 |
| 护甲 | 12 |
| 刷新点 | `i1_world` 50, 91, -28 |
| 掉落 | 栖云野Boss掉落 |

**技能列表**：

| 技能 | 阶段 | 说明 |
|---|---|---|
| SKSoulSlash | 一 | 灵魂斩击，附加凋零 |
| SKBoneBarrage | 一 | 骸骨弹幕，三连发 |
| SKThrowBone | 一 | 投掷巨骨 |
| SKTridentThrow | 一 | 三叉戟投掷 |
| SKBoneWhirl | 一 | 骸骨旋风，三段伤害 |
| SKShadowStep | 一 | 暗影步，瞬移背刺 |
| SKShadowAssault | 一 | 暗影突袭，多段冲撞 |
| SKSummonGuards | 一 | 召唤骷髅士兵 |
| SKPhaseTwo | 二 | 阶段二：加速+增伤 |
| SKSkullRain | 二 | 骷髅雨，三波范围伤害 |
| SKThrowSkull | 二 | 投掷骷髅头，附加凋零 |
| SKLightningStrike | 二 | 闪电打击 |
| SKSoulNova | 二 | 灵魂新星，范围击退 |
| SKPhaseThree | 三 | 阶段三：再加速+增伤+抗性 |
| SKWitherStorm | 三 | 凋零风暴，大范围伤害 |
| SKDeathWave | 三 | 死亡波纹，三波递增 |
| SKDeathRay | 三 | 死亡射线，穿透三连 |
| SKDeath | 死亡 | 死亡特效 |

**阶段阈值**：

- 阶段一：血量 100% ~ 60%
- 阶段二：血量 60% ~ 25%
- 阶段三：血量 25% 以下

### 2. 骷髅士兵（SkeletonSoldier）

| 项目 | 值 |
|---|---|
| 类型 | SKELETON |
| 血量 | 40 |
| 伤害 | 5 |
| 护甲 | 2 |
| 生成 | `i1_world` 随机生成 |
| 掉落 | 栖云野普通掉落 |

**装备随机**：

- 主手：木剑、石剑、铁剑、金剑、弓
- 副手：空、盾牌、火把
- 头盔：皮革 ~ 下界合金
- 胸甲：皮革 ~ 金甲、空
- 护腿：皮革 ~ 金裤
- 靴子：皮革 ~ 下界合金、空

**技能列表**：

| 技能 | 说明 |
|---|---|
| SkeletonSoldierSlash | 斩击，附加真实伤害 |
| SkeletonSoldierLeap | 跳跃突袭 |
| SkeletonSoldierDeath | 死亡特效 |

---

## 四、栖云野自定义物品

### 副本材料

| 物品 | 材质 | 用途 |
|---|---|---|
| 风蚀石 | 圆石 | 副本材料 |
| 野穗 | 小麦 | 副本材料 |
| 兽骨 | 骨头 | 副本材料 |
| 云絮 | 白色羊毛 | 副本材料 |
| 栖木枝 | 橡树苗 | 副本材料 |
| 旧铁片 | 铁粒 | 副本材料 |
| 栖云石 | 紫水晶碎片 | 副本材料 |
| 金色棍子 | 烈焰棒 | 副本材料 |
| 归云令 | 纸 | 副本材料 |

### 掉落表

**栖云野Boss掉落**：

- 必掉：风蚀石 4~8、野穗 6~12、兽骨 4~8、云絮 2~4
- 高概率：栖木枝 1~3、旧铁片 2~4、栖云石 1~2
- 中概率：金色棍子 1、归云令 1
- 原版：下界合金锭 2~4、钻石 8~16、金锭 16~32、骨头 32~64、附魔金苹果 1~2、下界之星 1

**栖云野普通掉落**：

- 野穗 1~2、兽骨 1、风蚀石 1、旧铁片 1

---

## 五、随机生成

### RandomSkeletonSoldier

| 项目 | 值 |
|---|---|
| 类型 | SkeletonSoldier |
| 世界 | `i1_world` |
| 概率 | 0.8（80%） |
| 动作 | ADD |
| 条件 | 世界内怪物 < 50、室外 |

### 已知问题

`mobsinworld < 50` 条件在 MM 5.13 中报错：

```
Failed to construct condition mobsinworld < 50
Caused by: NumberFormatException: empty String
```

**建议**：把条件删掉，或改用 `mobsInRadius`。

---

## 六、固定刷怪点

### BossSpawner

| 项目 | 值 |
|---|---|
| 怪物 | SkeletonKingBoss |
| 世界 | `i1_world` |
| 坐标 | 50, 91, -28 |
| 半径 | 0（精确位置） |
| 最大数量 | 1 |
| 冷却 | 60 秒 |

---

## 七、已知问题

### 1. 粒子名不兼容

MM 5.13 在 1.21.11 上，粒子名需要改：

| 旧名 | 新名 |
|---|---|
| `bone_meal` | `item;item=bone_meal` |
| `block soul_soil` | `block;material=SOUL_SOIL` |

### 2. `mobsinworld` 条件报错

见上文。

### 3. `teleport` 不支持相对坐标

MM 的 `teleport` 不支持 `~` 相对坐标，需要用绝对坐标或 `velocity` 替代。

---

## 八、修改指南

### 修改怪物属性

编辑 `mobs/` 下对应文件，改 `Health`、`Damage`、`Armor` 等字段，然后 `/mm reload`。

### 修改技能

编辑 `skills/` 下对应文件，改 `Cooldown`、`Skills` 列表，然后 `/mm reload`。

### 添加新怪物

1. 在 `mobs/` 下新建文件
2. 定义怪物类型、属性、技能
3. `/mm reload`

### 调试

```
/mm reload              # 重载配置
/mm mobs list           # 列出所有怪物
/mm mobs spawn <怪物名>  # 生成怪物
/mm debug 4             # 开启详细日志
```

---

## 九、参考

- [MythicMobs 官方 Wiki](https://mythicmobs.net/)
- [MythicMobs 中文 Wiki](https://gitlab.com/ruany/mythicmobs-wiki)
```