
# ExecutableItems 配置详解

> 本文档面向管理员，介绍栖云野服务器中 ExecutableItems 的配置结构与自定义物品。
>
> 玩家请查看 [自定义物品](../../player/自定义物品.md)。

---

## 一、插件信息

| 项目 | 值 |
|---|---|
| 插件名 | ExecutableItems |
| 版本 | 7.25.12.24 |
| 前置 | SCore |
| 作用 | 自定义物品、激活器、指令 |
| 目录 | `plugins/ExecutableItems/` |

---

## 二、目录结构

```
plugins/ExecutableItems/
├── items/              # 自定义物品定义
├── config.yml          # 主配置
└── ...
```

---

## 三、栖云野自定义物品

### 1. 孔雀羽毛扇

| 项目 | 值 |
|---|---|
| 材质 | FEATHER |
| 名称 | &a孔雀羽毛扇 |
| 功能 | 右键二段跳 |
| 激活器 | PLAYER_RIGHT_CLICK |
| 冷却 | 6 秒 |
| 消耗 | 无 |
| 获取 | 副本 NPC 交易 |

**实现方式**：右键调用 Skript 指令 `/doublejump`。

### 2. 避雷针

| 项目 | 值 |
|---|---|
| 材质 | COPPER_SWORD |
| 名称 | &e避雷针 |
| 功能 | 攻击目标后 3 秒召唤闪电，对周围 8 格特定怪物造成 40 伤害 |
| 激活器 | PLAYER_HIT_ENTITY |
| 冷却 | 5 秒 |
| 获取 | 副本 NPC 交易 |

**实现方式**：`entityCommands` 里用 `MOB_AROUND` 选择怪物，延迟 3 秒后召唤闪电。

**白名单怪物**：SKELETON、WITHER_SKELETON、SLIME、PIGLIN、ZOMBIFIED_PIGLIN、ZOMBIE

### 3. 桃木剑

| 项目 | 值 |
|---|---|
| 材质 | WOODEN_SWORD |
| 名称 | &e桃木剑 |
| 功能 | 攻击目标时使目标飞起 |
| 激活器 | PLAYER_HIT_ENTITY |
| 冷却 | 6 秒 |
| 获取 | 副本 NPC 交易 |

**实现方式**：`entityCommands` 里用 `/effect give %entity_uuid% levitation 1 19`。

### 4. 缓慢之剑

| 项目 | 值 |
|---|---|
| 材质 | IRON_SWORD |
| 名称 | &b缓慢之剑 |
| 功能 | 攻击目标时施加缓慢 III + 发光，持续 6 秒；对半径 5 格僵尸、骷髅造成 2 伤害 |
| 激活器 | PLAYER_HIT_ENTITY |
| 冷却 | 无 |
| 附魔 | 锋利 V、耐久 I |
| 获取 | 副本 NPC 交易 |

**实现方式**：`entityCommands` 里用 `/effect give` + `MOB_AROUND`。

### 5. 破旧不堪的金斧

| 项目 | 值 |
|---|---|
| 材质 | GOLDEN_AXE |
| 名称 | &6破旧不堪的金斧 |
| 功能 | 攻击目标时 1% 概率施加 10000 伤害 |
| 激活器 | PLAYER_HIT_ENTITY |
| 冷却 | 无 |
| 限制世界 | `i1_world`、`i2_world`、`instance` |
| 获取 | 副本 NPC 交易 |

**实现方式**：`placeholdersConditions` 里用 `%rng_1,100%` 判断是否等于 1。

### 6. 钻石权杖

| 项目 | 值 |
|---|---|
| 材质 | DIAMOND_HOE |
| 名称 | &b钻石权杖 |
| 功能 | 右键释放紫色魔法粒子，6 秒后对半径 50 格特定怪物造成 25 伤害 |
| 激活器 | PLAYER_RIGHT_CLICK |
| 冷却 | 30 秒 |
| 获取 | 副本 NPC 交易 |

**实现方式**：`playerCommands` 里用 `PARTICLE SPELL_WITCH` + `MOB_AROUND` + `DELAY 6`。

---

## 四、激活器说明

ExecutableItems 的激活器（Activator）决定物品在什么情况下触发。

### 常用激活器

| 激活器 | 触发时机 |
|---|---|
| `PLAYER_RIGHT_CLICK` | 玩家右键 |
| `PLAYER_LEFT_CLICK` | 玩家左键 |
| `PLAYER_HIT_ENTITY` | 玩家攻击实体 |
| `PLAYER_BREAK_BLOCK` | 玩家破坏方块 |
| `PLAYER_PLACE_BLOCK` | 玩家放置方块 |
| `PLAYER_DROP_ITEM` | 玩家丢弃物品 |
| `PLAYER_SNEAK` | 玩家潜行 |
| `PLAYER_JUMP` | 玩家跳跃 |

### 激活器参数

| 参数 | 说明 |
|---|---|
| `option` | 激活器类型 |
| `cancelEvent` | 是否取消原版事件 |
| `cooldown` | 冷却时间（秒） |
| `detailedSlots` | 生效槽位（-1 = 主手，40 = 副手） |
| `playerCommands` | 玩家身份执行的指令 |
| `entityCommands` | 对目标执行的指令 |
| `playerConditions` | 玩家条件 |
| `entityConditions` | 目标条件 |

---

## 五、SCore 自定义指令

ExecutableItems 的指令基于 SCore，常用指令如下：

### 消息类

| 指令 | 说明 |
|---|---|
| `SEND_MESSAGE text:<消息>` | 发送消息 |
| `SEND_CENTERED_MESSAGE text:<消息>` | 发送居中消息 |
| `ACTIONBAR <文本> <秒数>` | 显示动作栏 |

### 效果类

| 指令 | 说明 |
|---|---|
| `PARTICLE <粒子> <数量> <扩散> <速度>` | 生成粒子 |
| `SOUND <音效> <音量> <音调>` | 播放音效 |
| `POTION <类型> <时长> <等级>` | 施加药水效果 |

### 伤害类

| 指令 | 说明 |
|---|---|
| `DAMAGE <数值>` | 对目标造成伤害 |
| `MOB_AROUND <半径> WHITELIST(<怪物>) DAMAGE <数值>` | 对周围怪物造成伤害 |
| `MOB_AROUND <半径> WHITELIST(<怪物>) PARTICLE <粒子> <数量> <扩散> <速度>` | 对周围怪物生成粒子 |

### 控制类

| 指令 | 说明 |
|---|---|
| `DELAY <秒数>` | 延迟 |
| `DELAYTICK <tick>` | 延迟（tick） |
| `SUDO <指令>` | 以玩家身份执行 |
| `SUDO_OP <指令>` | 以 OP 身份执行 |

### 物品类

| 指令 | 说明 |
|---|---|
| `SET_ITEM_NAME slot:<槽位> name:<名称>` | 设置物品名 |
| `SET_ITEM_LORE slot:<槽位> line:<行号> text:<文本>` | 设置 Lore |
| `ADD_ITEM_ENCHANTMENT slot:<槽位> enchantment:<附魔> level:<等级>` | 添加附魔 |
| `SET_ITEM_COOLDOWN material:<材质> cooldown:<秒数>` | 设置物品冷却 |

---

## 六、常见问题

### 1. `SUDO doublejump` 不生效

**原因**：EI 的指令默认以控制台身份执行，Skript 指令没有 `executable by: console`。

**解决**：Skript 指令加上 `executable by: console, player`，或者用 `SUDO` 显式以玩家身份执行。

### 2. 冷却不生效

**原因**：EI 的 `cooldown` 是秒，不是 tick。

**解决**：确认 `cooldownFeatures.cooldown` 的值是秒数。

### 3. 权限问题

**原因**：普通玩家需要 `ei.item.*` 权限。

**解决**：LuckPerms 里给 `default` 组加权限：

```
/lp group default permission set ei.item.* true
```

### 4. 物品不是 EI 物品

**原因**：用 `/give` 拿的普通物品没有 EI 的 NBT 标签。

**解决**：必须用 `/ei give <玩家> <物品ID>` 获取。

---

## 七、修改指南

### 修改物品属性

编辑 `items/` 下对应文件，改 `name`、`lore`、`material`、`enchantments` 等字段，然后 `/ei reload`。

### 修改激活器

编辑 `activators` 下的 `playerCommands` 或 `entityCommands`，然后 `/ei reload`。

### 添加新物品

1. 在 `items/` 下新建文件
2. 参考现有物品格式定义
3. `/ei reload`

### 调试

```
/ei reload              # 重载配置
/ei give <玩家> <物品>   # 给物品
/ei editor              # 打开编辑器
```

---

## 八、参考

- [ExecutableItems 官方 Wiki](https://spluginstore.com/wiki/executableitems/)
- [SCore 自定义指令](https://spluginstore.com/wiki/score/)
