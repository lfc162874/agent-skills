---
name: technical-outline-generation
description: 根据招标文件评分标准、技术/采购需求和格式要求，反向推导投标文件技术卷大纲。适用于技术卷目录生成、评分项覆盖分析、正文生成前置大纲规划。
version: 1.0.0
language: zh-CN
---

# 技术卷大纲生成 Skill

## 使用场景

当系统需要根据招标文件生成投标文件技术卷大纲时，使用本 Skill。

本 Skill 适用于：

1. 根据评分标准反向推导技术卷大纲；
2. 根据采购需求和技术要求补充章节内容；
3. 自动选择总分总、流程型、模块型、评分项驱动型或混合型结构；
4. 输出评分项覆盖矩阵；
5. 为后续正文生成提供章节依据。

本 Skill 不用于：

1. 商务卷目录生成；
2. 资格文件目录生成；
3. 报价文件目录生成；
4. 简单抽取招标文件原目录；
5. 无证据的通用技术方案模板生成。

## 核心原则

1. 评分标准是最高优先级；
2. 技术卷不是目录抽取，而是基于评分标准反向推导；
3. 高分技术评分项必须在一级或二级章节中被覆盖；
4. 技术/采购需求用于确定章节边界；
5. 格式/目录参考只用于判断技术卷格式约束；
6. 不得将商务卷、资格卷、报价文件内容混入技术卷；
7. 输出必须是可解析 JSON；
8. 输出必须包含章节树、结构策略、评分项覆盖矩阵和风险提醒。

## 输入要求

调用本 Skill 时，应提供以下 JSON 输入：

```json
{
  "projectName": "项目名称",
  "generationConfig": {
    "chapterDepth": 3,
    "bidType": "technical",
    "outlineStyle": "formal",
    "structureStrategy": "auto",
    "scoreCoverageFirst": true,
    "maxFirstLevelChapters": 12,
    "maxSecondLevelChapters": 6,
    "maxThirdLevelChapters": 4
  },
  "scoringItems": [],
  "scoringChunks": [],
  "technicalRequirementItems": [],
  "technicalRequirementChunks": [],
  "formatReferenceChunks": []
}
```

## 执行流程

执行时必须按以下流程完成内部推导：

1. 读取 `docs/execution-rules.md`；
2. 解析评分标准，识别得分点、高分项、核心项；
3. 读取 `docs/structure-strategies.md`；
4. 根据评分标准和采购需求选择结构策略；
5. 读取 `docs/forbidden-content.md`；
6. 排除商务卷、资格卷、报价文件内容；
7. 生成技术卷章节树；
8. 建立评分项覆盖矩阵；
9. 根据 `schemas/output-schema.json` 输出 JSON；
10. 根据 `validators/quality-checklist.md` 自检；
11. 只输出最终 JSON，不输出 Markdown、解释、分析过程。

## 输出要求

最终只能输出一个 JSON 对象。

不得输出：

1. Markdown 代码块；
2. 解释文字；
3. 推导过程；
4. 多余前缀；
5. 多余后缀。

输出必须符合：

- `schemas/output-schema.json`

## 必须参考的 Skill 文件

执行本 Skill 时，应参考以下文件：

- `docs/execution-rules.md`
- `docs/structure-strategies.md`
- `docs/forbidden-content.md`
- `schemas/output-schema.json`
- `examples/few-shot.md`
- `validators/quality-checklist.md`
