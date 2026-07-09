---
name: technical-volume-body-writing
description: 根据招标文件、评分标准、技术卷大纲和知识库证据生成、改写、审查投标技术卷正文。用于技术卷、技术标、技术方案、实施方案、需求分析、项目理解、系统现状分析、质量保障、进度保障、售后服务、评分点响应、正文 JSON blocks 生成等场景；强制输出结构化 JSON，统一约束文章编号、段落格式、列表格式，避免 Markdown、手写编号、乱造标题和无证据事实。
version: 1.0.1
language: zh-CN
compatibility: 技术卷正文生成、TipTap JSON 渲染、后端 docx 模板导出
---

# 技术卷正文生成 Skill

## 使用场景

当系统需要生成、改写、润色或审查投标文件技术卷正文时，使用本 Skill。

适用于：

1. 根据技术卷大纲生成单章节正文；
2. 根据评分标准补强技术方案、实施方案、需求分析、质量保障等内容；
3. 根据招标文件技术/采购要求生成正式投标正文；
4. 将 AI 正文结果约束为结构化 JSON blocks；
5. 检查正文是否存在 Markdown 残留、手写编号、跑题、空泛、虚构事实等问题；
6. 为 TipTap 编辑器和后端 docx 导出提供稳定正文结构；
7. 统一控制技术卷文章编号、正文段落、列表段落、表格段落和 Word 导出样式映射。

不适用于：

1. 商务卷、资格卷、报价文件正文生成；
2. 单纯抽取招标文件目录；
3. 无评分标准、无招标要求、无章节上下文的通用文章生成；
4. 需要直接输出 Markdown 或 HTML 的场景。

## 核心原则

AI 只负责写内容，系统负责控制格式。

正文必须通过结构化 JSON 表达，不能依赖 Markdown、空格、手写编号来表示标题、列表、缩进、加粗或段落格式。

必须同时守住三条边界：

1. 大纲边界：只写当前章节，不得生成大纲之外的标题；
2. 证据边界：不得编造资质、业绩、人员、证书、金额、客户名称等事实；
3. 格式边界：不得输出 Markdown、手写编号、段首空格或其他不可控格式符号。

## 输入要求

调用本 Skill 时，建议提供以下上下文：

1. projectInfo：项目名称、招标人、投标人、项目类型；
2. sectionInfo：章节 ID、章节路径、章节标题、章节层级、父级标题；
3. outlineTree：技术卷大纲树，用于判断章节边界；
4. scoreCriteria：评分项、评分标准原文、分值、权重、证据引用；
5. tenderRequirements：招标文件技术要求、采购需求、服务要求、格式要求；
6. knowledgeContext：企业知识库、项目案例、资质证照、技术模板、人员能力等证据；
7. generationConfig：是否允许小标题、是否允许列表、段落数量、输出语言等配置。

如果部分输入缺失，可以基于已有信息生成，但不得虚构缺失的企业事实。

## 执行流程

执行时必须按以下流程完成：

1. 读取 `docs/execution-rules.md`；
2. 根据 `sectionInfo.sectionTitle` 和 `sectionInfo.sectionPath` 判断章节类型；
3. 读取 `docs/section-writing-strategies.md`，选择对应写作策略；
4. 解析 `scoreCriteria`，优先覆盖高分项、核心项和明确评分点；
5. 解析 `tenderRequirements`，确保正文回应招标要求；
6. 使用 `knowledgeContext` 作为事实依据，不得编造未提供材料；
7. 读取 `docs/numbering-and-paragraph-format.md`，确定编号、段落、列表、表格和样式映射规则；
8. 生成符合 `schemas/output-schema.json` 的 JSON blocks；
9. 读取 `docs/forbidden-content.md`，清除 Markdown、手写编号和不合规内容；
10. 根据 `validators/quality-checklist.md` 完成自检；
11. 只输出最终 JSON，不输出解释、分析过程或 Markdown 代码块。

## 输出要求

最终只能输出一个 JSON 对象，且必须符合：

- `schemas/output-schema.json`

禁止输出：

1. Markdown 代码块；
2. 解释文字；
3. 推导过程；
4. “以下是正文”“生成结果如下”等提示语；
5. 大纲之外的标题；
6. 当前 schema 不支持的字段。

## 格式硬约束

1. 禁止输出 Markdown；
2. 禁止使用 `#`、`**`、`-`、`>`、代码块、Markdown 表格等符号；
3. 禁止自行生成章节编号，例如 `1.1`、`一、`、`（一）`、`1.`、`1、`；
4. 禁止用空格、全角空格、制表符模拟首行缩进；
5. 分点内容必须使用 `list` block，不得在 paragraph 中手写编号；
6. 标题、编号、缩进、字体、字号、行距由前端和后端渲染器统一控制。

## 编号与段落格式硬约束

执行正文生成时，必须遵守 `docs/numbering-and-paragraph-format.md`。

默认技术卷编号体系：

1. 一级章：由系统根据大纲生成，样式为“第一章 项目理解与需求分析”；
2. 二级节：由系统根据大纲生成，样式为“1.1 招标人现状分析”；
3. 三级节：由系统根据大纲生成，样式为“1.1.1 信息化系统现状”；
4. 正文列表：由渲染器生成，默认样式为“（1）（2）（3）”；
5. AI 不得把上述编号写入 `text` 或 `items` 字段。

默认段落格式：

1. `paragraph + body`：宋体、小四、两端对齐、首行缩进 2 字符、1.5 倍行距；
2. `heading1`、`heading2`、`heading3`：不首行缩进；
3. `orderedList`：编号由渲染器控制，item 文本不写编号；
4. `unorderedList`：项目符号由渲染器控制，item 文本不写项目符号；
5. `standardTable`：单元格正文不使用首行缩进。

## 允许的 block 类型

默认允许：

1. `paragraph`：正文段落；
2. `list`：有序或无序列表；
3. `table`：标准表格。

仅当 `generationConfig.allowSubHeadings=true` 时，允许使用：

1. `heading`：章节内小标题。

## 质量要求

生成内容必须满足：

1. 贴合当前章节标题和父级章节语义；
2. 回应招标文件技术/采购要求；
3. 覆盖评分标准中的核心得分点；
4. 语言正式、稳健、专业，符合投标文件表达习惯；
5. 内容应包含具体动作、机制、流程、交付物或保障措施；
6. 避免空泛口号和重复套话；
7. 不得编造企业资质、项目案例、人员、证书、金额、编号、客户名称；
8. 每个 paragraph 只表达一个明确主题；
9. 列表只在能提升可读性时使用；
10. 不得暴露 AI 生成痕迹。

## 当前标准输出示例

```json
{
  "sectionId": "sec_technical_001",
  "sectionTitle": "招标人信息化及生产系统现状分析",
  "blocks": [
    {
      "type": "paragraph",
      "text": "为深入理解招标人当前信息化建设基础、生产系统运行现状及数据汇聚中心系统建设需求，本章节围绕现有系统架构、数据资源分布、业务流程协同、接口条件及运行管理机制等方面开展分析，为后续系统建设方案设计提供需求依据和实施方向。",
      "styleKey": "body"
    },
    {
      "type": "list",
      "ordered": true,
      "styleKey": "orderedList",
      "items": [
        "全面梳理现有信息系统、生产系统及管理系统的数据来源、数据类型、数据更新方式和接口条件。",
        "识别多源数据在汇聚、流转、共享、治理和应用过程中的关键痛点，明确后续建设的重点方向。",
        "结合系统边界、网络环境、数据安全和运维管理要求，形成符合项目实际情况的实施依据。"
      ]
    }
  ],
  "scoreCoverage": [
    {
      "itemKey": "score_001",
      "itemName": "需求理解与分析",
      "covered": true,
      "coverageDescription": "正文围绕现有系统、数据资源、接口条件、业务流程和建设目标进行了分析，能够支撑需求理解类评分点。"
    }
  ],
  "evidenceRefs": [],
  "qualityHints": {
    "hasMarkdown": false,
    "hasManualNumbering": false,
    "hasFakeInfoRisk": false,
    "hasUnsupportedClaim": false,
    "outlineViolation": false
  }
}
```

## 必须参考的 Skill 文件

执行本 Skill 时，应参考以下文件：

- `docs/execution-rules.md`
- `docs/section-writing-strategies.md`
- `docs/numbering-and-paragraph-format.md`
- `docs/forbidden-content.md`
- `schemas/output-schema.json`
- `examples/few-shot.md`
- `validators/quality-checklist.md`
