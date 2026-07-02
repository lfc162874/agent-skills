---
name: technical-outline-generation
description: 根据招标文件评分标准、技术/采购需求和格式要求，反向推导投标文件技术卷大纲。当前版本默认兼容 TechnicalOutlinePromptBuilder.java 的生产输出结构。
version: 1.0.1
language: zh-CN
compatibility: TechnicalOutlinePromptBuilder.java
---

# 技术卷大纲生成 Skill

## 使用场景

当系统需要根据招标文件生成投标文件技术卷大纲时，使用本 Skill。

本 Skill 适用于：

1. 根据评分标准反向推导技术卷大纲；
2. 根据采购需求和技术要求补充章节内容；
3. 自动选择总分总、流程型、模块型、评分项驱动型或混合型结构；
4. 生成可用于后续正文生成的技术卷章节树。

本 Skill 不用于：

1. 商务卷目录生成；
2. 资格文件目录生成；
3. 报价文件目录生成；
4. 简单抽取招标文件原目录；
5. 无证据的通用技术方案模板生成。

## 兼容性要求

当前版本必须兼容现有 `TechnicalOutlinePromptBuilder.java` 的输出结构。

默认输出只能包含：

1. `documentTitle`；
2. `chapters`。

默认不得输出以下增强字段：

1. `structureStrategy`；
2. `coverageMatrix`；
3. `warnings`；
4. `basis`；
5. `evidenceRefs`。

如后续 Java DTO、前端树结构和数据库字段已升级，才允许启用增强输出。启用增强输出前，必须同步更新 `schemas/output-schema.json` 和后端解析逻辑。

## 核心原则

1. 评分标准是最高优先级；
2. 技术卷不是目录抽取，而是基于评分标准反向推导；
3. 高分技术评分项必须在一级或二级章节中被覆盖；
4. 技术/采购需求用于确定章节边界；
5. 格式/目录参考只用于判断技术卷格式约束；
6. 不得将商务卷、资格卷、报价文件内容混入技术卷；
7. 输出必须是可解析 JSON；
8. 输出必须严格符合 `schemas/output-schema.json`。

## 输入要求

调用本 Skill 时，建议提供以下输入上下文。

如果沿用当前 Java 代码，可继续使用现有 PromptBuilder 方式拼接：

```text
【生成配置】
chapterDepth=3, bidType=technical, outlineStyle=formal

【评分标准证据】
- 评分项文本

【评分标准原文片段】
- chunkId=xxx, titlePath=xxx
原文内容

【技术/采购需求结构化证据】
- 需求项文本

【技术/采购需求原文片段】
- chunkId=xxx, titlePath=xxx
原文内容

【格式/目录参考证据】
- chunkId=xxx, titlePath=xxx
原文内容
```

如果系统已改造为 Skill Input JSON，可使用以下结构作为输入，但该结构只是输入，不代表输出也包含这些字段：

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
4. 根据评分标准和采购需求选择结构策略，但不要把结构策略字段输出到最终 JSON；
5. 读取 `docs/forbidden-content.md`；
6. 排除商务卷、资格卷、报价文件内容；
7. 生成技术卷章节树；
8. 根据 `schemas/output-schema.json` 输出兼容当前代码的 JSON；
9. 根据 `validators/quality-checklist.md` 自检；
10. 只输出最终 JSON，不输出 Markdown、解释、分析过程。

## 输出要求

最终只能输出一个 JSON 对象。

不得输出：

1. Markdown 代码块；
2. 解释文字；
3. 推导过程；
4. 多余前缀；
5. 多余后缀；
6. 当前代码不支持的增强字段。

输出必须符合：

- `schemas/output-schema.json`

## 当前兼容输出示例

```json
{
  "documentTitle": "项目名称技术卷大纲",
  "chapters": [
    {
      "id": "ch-1",
      "title": "一、一级章节标题",
      "expanded": true,
      "sortNo": 1,
      "children": [
        {
          "id": "s-1-1",
          "title": "1.1 二级章节标题",
          "expanded": false,
          "sortNo": 1,
          "children": [
            {
              "id": "t-1-1-1",
              "title": "1.1.1 三级章节标题",
              "sortNo": 1
            }
          ]
        }
      ]
    }
  ]
}
```

## 必须参考的 Skill 文件

执行本 Skill 时，应参考以下文件：

- `docs/execution-rules.md`
- `docs/structure-strategies.md`
- `docs/forbidden-content.md`
- `schemas/output-schema.json`
- `examples/few-shot.md`
- `validators/quality-checklist.md`
