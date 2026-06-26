# ✍️ SagaSmith Module Generator

**D&D 5e 冒险模组 AI 生成器** — 独立 skill，零依赖，纯 Markdown。

> 5 种类型 × 25 种叙事范式 × 结构化模板 → 一键产出完整可玩模组

---

## 支持的类型

| 类型 | 章节 | 推荐范式 | 产出规模 |
|------|------|----------|----------|
| 🎯 **One-shot** | 1 章 | Five-Room Dungeon, Heist, Mystery | 1 次，3-6h |
| 📖 **Short** | 3 章 | Three-Act, Kishōtenketsu, Race Against Time | 3-8 次 |
| 📚 **Medium** | 5 章 | Hero's Journey, Plot Point, Faction Turn | 2-4 月 |
| 🏰 **Long** | 8 章 | Double Triangle, Conspyramid, Megadungeon | 6+ 月 |
| 🗺️ **Sandbox** | 4-6 区域 | Hexcrawl, Node-Based, Blorb | 开放 |

---

## 范式总览（25 种）

- **空间驱动**：Five-Room Dungeon · Node-Based · Hexcrawl · Megadungeon · Island Design
- **故事驱动**：Three-Act · Hero's Journey · Double Triangle · Kishōtenketsu · Beat Charts · Seven-Point Story
- **玩法驱动**：Heist · Mystery · Conspyramid · Faction Turn · Race Against Time · Survival
- **角色驱动**：Plot Point · Fish Tank Intrigue · Technoir · Decision-Based · Blorb
- **混合型**：Iceberg Diagram · Reverse Dungeon

---

## 快速安装

```bash
# Claude Code / Codex / Cursor / Copilot
npx skills add dajiaohuang/SagaSmith-modulegen

# ClawHub
npx clawhub install sagasmith-modulegen
```

---

## 使用方式

Agent 加载此 skill 后，对用户说"帮我生成一个 5 级的海盗主题 medium 模组"即可触发。

生成流程：
1. **Step 0** — 确认类型、范式、主题、环境、反派、等级
2. **单步生成**（one-shot/short）— 一次性产出完整模组 Markdown
3. **多步生成**（medium/long）— 骨架 → 章节正文 → 附录，每步用户审核
4. **沙盒生成** — 世界骨架 → 逐区域独立生成

---

## 致谢

- [ackiles/dnd-dm-skill](https://github.com/ackiles/dnd-dm-skill) — D&D DM skill 先驱

---

## 许可证

MIT
