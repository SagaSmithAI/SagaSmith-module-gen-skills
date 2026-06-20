# ✍️ SagaSmith Module Generator

**D&D 5e 冒险模组 AI 生成器**

> 一句话让 AI 写出完整可跑的 D&D 冒险——带章节结构、NPC 欲望与秘密、伏笔回收链、DC 数值、怪物数据。

独立 skill，零依赖，纯 `SKILL.md`。兼容 Claude Code / Codex / Cursor / Copilot / OpenClaw / Hermes 等所有支持 SKILL.md 标准的 agent。

---

## 不是"创意点子生成器"

大多数 AI 写模组是这样的：你让它"想个冒险"，它给你一段散文式描述，没有结构、没有数值、没有关卡设计。

SagaSmith Module Generator 产出的是**可直接导入跑团的完整模组文件**：

- ✅ 按章节组织的 `# heading` 结构（`dnd_module action=import` 直读）
- ✅ 房间级 `####` 子标题 + 类型标注（`room` / `statblock` / `list`）
- ✅ NPC 全维度：`name / race / class / alignment / want / fear / secret`
- ✅ 伏笔-回收表 + 势力关系网 + 结局分支
- ✅ DC 数值 + 怪物数据 + 魔法物品
- ✅ 导出场景索引 JSON（DM 无需查库即可导航）

---

## 5 种类型 × 25 种范式

| 类型 | 章节 | 默认范式 | 更多范式 | 产出规模 |
|------|------|----------|----------|----------|
| 🎯 **One-shot** | 1 | Five-Room Dungeon | Heist, Mystery, Reverse Dungeon | 1 次，3-6h |
| 📖 **Short** | 3 | Three-Act | Kishōtenketsu, Race Against Time, Island Design | 3-8 次 |
| 📚 **Medium** | 5 | Hero's Journey | Plot Point, Faction Turn, Fish Tank | 2-4 月战役 |
| 🏰 **Long** | 8 | Double Triangle | Conspyramid, Megadungeon, Technoir | 6+ 月战役 |
| 🗺️ **Sandbox** | 4-6 区域 | Hexcrawl | Node-Based, Blorb, Decision-Based | 开放探索 |

---

## 生成范式

| 驱动类型 | 范式 | 一句话 |
|----------|------|--------|
| 空间 | **Five-Room Dungeon** | 入口守卫 → 谜题/社交 → 陷阱/反转 → Boss → 奖励 |
| 空间 | **Hexcrawl** | 六角格地图，每格独立遭遇与地点 |
| 空间 | **Megadungeon** | 多层地城，环形路线，内部派系 |
| 故事 | **Three-Act** | 建立 → 对抗 → 解决，Ch.2 中场反转 |
| 故事 | **Hero's Journey** | 平凡世界 → 跨越门槛 → 试炼 → 深渊 → 归来 |
| 故事 | **Double Triangle** | Ch.1-4 崛起 → Ch.5-6 陨落 → Ch.7-8 救赎 |
| 故事 | **Kishōtenketsu** | 起承转合，无需反派 |
| 故事 | **Seven-Point Story** | Hook → PT1 → Pinch1 → Midpoint → Pinch2 → PT2 → Resolution |
| 玩法 | **Heist** | 情报 → 计划 → 执行 → 意外 → 脱逃 |
| 玩法 | **Mystery** | Hook → 3 地点 → 每点 3 线索 → 揭示（三线索法则） |
| 玩法 | **Conspyramid** | 6 层阴谋 + 6 层应对金字塔 |
| 玩法 | **Faction Turn** | 派系独立于玩家推进 |
| 玩法 | **Race Against Time** | 限时：X 轮完成 Y 目标 |
| 角色 | **Plot Point** | 主线章节 + 角色个人线并行 |
| 角色 | **Fish Tank** | 事件 + 派系已就位，玩家是投入的变量 |
| 角色 | **Blorb** | 准备实体，不准备情节。Prep > rules > improv |
| 混合 | **Iceberg Diagram** | 表面钩子 → 隐藏深度 |
| 混合 | **Reverse Dungeon** | 玩家防守，怪物进攻 |

---

## 生成流程

### 单步（One-shot / Short）
一次性产出完整模组 → 写入文件 → 导入数据库

### 多步（Medium / Long）
**M1** → 骨架（概要 + 势力 + 章节大纲 + NPC 全维度）→ 用户审核  
**M2** → 前半章节正文 → 用户审核  
**M3** → 后半章节 + 附录（伏笔回收表、势力变化、怪物、魔法物品）→ 组装成最终文件

### 沙盒
**S1** → 世界骨架（区域总览 + 势力网 + 随机遭遇表）  
**S2-N** → 每个区域独立生成（特征 + 势力 + 事件线 + 地点 × 3-6）

每一步都有用户审核节点——不会在无人确认的情况下推进到下一步，避免幻觉堆积。

---

## 快速安装

```bash
# Claude Code / Codex / Cursor / Copilot
npx skills add dajiaohuang/SagaSmith-modulegen

# ClawHub
npx clawhub install sagasmith-modulegen
```

---

## 生成示例

对 Agent 说：

> *"帮我生成一个 3 级海盗主题的 short 模组，用 Three-Act 范式，反派是水元素祭祀，两处反转，2 个结局分支。"*

Agent 会：

1. 确认参数 → 按模板逐章生成
2. 生成带 `####` 房间标题、NPC 对话提示、DC 值的完整 Markdown
3. 产出伏笔-回收表 + 怪物数据附录
4. 写入文件，可直接导入 SagaSmith 或其他 D&D agent 跑团

---

## 生态

| 仓库 | 定位 |
|------|------|
| ✍️ **SagaSmith-modulegen**（本仓库） | 独立模组生成 skill |
| 🎲 [SagaSmith-agent](https://github.com/dajiaohuang/SagaSmith-agent) | 完整 AI DM 运行时 |
| 📦 [SagaSmith-skill](https://github.com/dajiaohuang/SagaSmith-skill) | 全家桶 skill 插件包 |

---

## 致谢

- [ackiles/dnd-dm-skill](https://github.com/ackiles/dnd-dm-skill) — D&D DM skill 先驱
- 25 种范式源自 TRPG 社区数十年的设计积累（Five-Room Dungeon、Three Clue Rule、Hexcrawl 等）

---

## 许可证

MIT
