# FlectonePulse 配置详解

> 依据 survival 服 `config.yml`、`command.yml`、`message.yml`、`integration.yml`、`permission.yml` 和中文本地化文件整理。只有命令配置 `enable: true` 的命令才列为已开启；语言文件中存在文案不代表命令已开启。

## 一、基本信息

| 项目 | 当前设置 |
|---|---|
| 插件版本 | 1.14.0（配置版本） |
| 语言 | 简体中文，允许按玩家语言 |
| 网络 | Velocity 集成开启；Redis、BungeeCord 模式关闭 |
| 数据 | MySQL；不在文档中暴露连接凭据 |
| 聊天范围 | 全局消息走代理（`PROXY`）；本地聊天关闭 |

## 二、已启用的玩家命令

配置别名默认为命令名本身；命令缺省参数以游戏内命令提示和 Tab 补全为准。

| 命令 | 用途 |
|---|---|
| `/anon` | 匿名发送文本 |
| `/ball` | 随机问答 |
| `/chatcolor` | 管理个人聊天显示/输出颜色；对其他玩家改色需管理权限 |
| `/chatsetting` | 打开聊天设置菜单，切换聊天类型、个人颜色和消息通知 |
| `/coin`、`/dice` | 抛硬币、掷骰子 |
| `/do`、`/me` | 角色扮演动作消息 |
| `/helper` | 发起求助 |
| `/ignore`、`/ignorelist` | 屏蔽玩家、查看名单 |
| `/mail` | 插件邮件功能 |
| `/minesweeper` | 扫雷小游戏 |
| `/online`、`/ping` | 查看在线玩家/延迟 |
| `/poll` | 投票 |
| `/reply`、`/tell` | 回复或发送私聊 |
| `/rockpaperscissors`、`/tictactoe` | 与其他玩家发起小游戏 |
| `/sprite`、`/symbol` | 发送插件提供的图标/符号 |
| `/stream` | 直播消息功能 |
| `/toponline` | 在线时长排行 |
| `/translateto` | 文本翻译命令；是否可成功使用受翻译后端配置影响 |
| `/try` | 随机尝试结果 |
| `/whois` | 玩家信息查询；敏感字段受权限约束 |

`/afk` 在 `command.yml` 中为关闭状态；尽管 `message.yml` 的 AFK 消息模块启用，也不表示玩家 AFK 命令可用。

## 三、已启用的管理命令

下列命令配置启用，但 `permission.yml` 将多数处罚和服务器管理功能设为 OP 权限。不要授予普通玩家：

- 封禁管理：`/ban`（别名 `/tempban`）、`/banlist`、`/unban`
- 警告管理：`/warn`、`/warnlist`、`/unwarn`
- 禁言管理：`/mute`、`/mutelist`、`/unmute`
- 服务器管理：`/broadcast`、`/kick`、`/maintenance`、`/whitelist`
- 聊天管理：`/clearchat`、`/clearmail`、`/deletemessage`、`/spy`
- 查询/诊断：`/geolocate`、`/flectonepulse`

权限文档的 `TRUE` 表示按默认节点配置开放，`OP` 表示 OP 默认许可。`clearchat` 等带 `other` 子权限的命令，对自身与对他人操作可能分别受不同节点限制。

## 四、消息与聊天特性开关

| 特性 | 状态与参数 |
|---|---|
| 全局聊天 | 开启，范围 `PROXY`，跨代理后端广播 |
| 本地聊天 | 关闭（配置的半径 100 不生效） |
| AFK 自动消息 | 开启；延迟 36000 tick，按服务器范围，消息间隔 20 tick |
| 聊天气泡 | 开启；最多同时 3 条、每条 30 字、显示距离 30 格、每 5 tick 更新 |
| 提及 | 开启；需要 `@` 前缀，通知使用 Toast 和提示音；`@here` 为配置的全体标签 |
| 格式/颜色 | 开启，支持旧颜色代码和配置的 Adventure 标签 |
| 远近淡出 | 开启，隐藏不同世界玩家的远距离聊天，渐变距离 10–50 格 |
| 动态颜色 | 开启，配置了 4 个聊天色槽 |
| BossBar 消息 | 模块启用，公告型消息呈现为标题；是否实际出现还取决于触发来源 |
| 玩家品牌文本 | 开启并随机显示 |
| 自动公告 | 关闭；公告配置段是模板，不会自动播报 |
| 铁砧/书本消息处理 | 关闭 |
| Caps/刷屏/脏话过滤 | 当前 `message.yml` 中相关 moderation 子开关关闭 |

`/chatsetting` 可切换自己可见的加入/退出、进度、聊天、死亡、睡觉及多种社交命令通知。Discord、Telegram、Twitch 通知项虽然出现在设置菜单配置中，但相应平台集成关闭，不能视为已连接。

## 五、集成状态

| 集成 | 状态 |
|---|---|
| LuckPerms、PlaceholderAPI、TAB | 开启 |
| Simple Voice Chat、SkinsRestorer | 开启 |
| Discord、Telegram、Twitch | 关闭 |
| CMI、AdvancedBan、LiteBans、Maintenance | 关闭 |
| Geyser、Floodgate、InteractiveChat、ItemsAdder、DeepL、ICU 翻译 | 关闭 |

## 六、使用与安全注意

1. 此插件与 X-Chat/CMI 等都可能注册聊天、私聊或忽略命令。发生命令冲突时以启动日志及 `plugins` 命令映射为准。
2. `integration.yml` 关闭的外部平台不会因本地化文件有文案而连通；不要在 Wiki 发布空配置里的 token/频道 ID。
3. `geolocate` 涉及 IP 地理位置查询，严格限制其权限，只用于服务器管理与安全排查。
4. 调整开关后，按插件支持方式重载或重启；认证、代理集成和命令注册更改建议完整重启验证。
