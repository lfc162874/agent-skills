# Agent Skills

用于维护投标智能体、标准智能体等 Agent 可复用 Skill。

## 当前 Skill

| Skill | 说明 |
|---|---|
| `skills/technical-outline-generation` | 根据评分标准、技术/采购需求和格式约束，反向推导投标文件技术卷大纲 |

## Skill 目录规范

每个 Skill 独立成目录，推荐结构如下：

```text
skills/<skill-name>/
├── SKILL.md
├── docs/
├── schemas/
├── examples/
└── validators/
```

设计原则：

1. `SKILL.md` 只作为入口文件，描述触发场景、输入输出、执行路由。
2. 具体规则放入 `docs/`。
3. 输出 JSON 结构放入 `schemas/`。
4. 正例、反例、样例输入放入 `examples/`。
5. 后处理校验规则放入 `validators/`。
