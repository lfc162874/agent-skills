# Few-shot 示例

本文件提供技术卷正文生成的输入和输出示例。

## 示例一：需求分析类章节

### 输入摘要

章节标题：招标人信息化及生产系统现状分析

评分标准：对项目现状、建设需求、数据汇聚目标理解准确，分析全面得分高。

招标要求：建设数据汇聚中心系统，支持多源异构数据接入、统一管理、数据治理和共享应用。

### 正确输出

```json
{
  "sectionId": "sec_001",
  "sectionTitle": "招标人信息化及生产系统现状分析",
  "blocks": [
    {
      "type": "paragraph",
      "text": "为深入理解招标人当前信息化建设基础、生产系统运行现状及数据汇聚中心系统建设需求，本章节围绕现有系统架构、数据资源分布、业务流程协同、接口条件及运行管理机制等方面开展分析，为后续系统建设方案设计提供需求依据和实施方向。",
      "styleKey": "body"
    },
    {
      "type": "paragraph",
      "text": "从项目建设目标来看，数据汇聚中心系统需要面向多源异构数据接入、统一数据管理、数据质量提升和业务协同支撑等核心需求，形成覆盖数据采集、汇聚、治理、共享和应用支撑的整体能力。系统建设不仅需要关注技术架构的完整性，也需要充分考虑招标人现有业务系统、生产系统和管理流程之间的衔接关系。",
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

### 错误输出

```json
{
  "sectionId": "sec_001",
  "sectionTitle": "招标人信息化及生产系统现状分析",
  "blocks": [
    {
      "type": "paragraph",
      "text": "1. **现状分析** 招标人信息化基础较好，我公司将全力以赴完成项目。",
      "styleKey": "body"
    }
  ],
  "scoreCoverage": [],
  "evidenceRefs": [],
  "qualityHints": {
    "hasMarkdown": true,
    "hasManualNumbering": true,
    "hasFakeInfoRisk": false,
    "hasUnsupportedClaim": true,
    "outlineViolation": true
  }
}
```

错误原因：包含 Markdown、手写编号、未经允许的小标题、空泛表达。

## 示例二：实施方案类章节

### 输入摘要

章节标题：项目实施计划

评分标准：实施计划合理、阶段清晰、进度保障措施完整。

### 正确输出

```json
{
  "sectionId": "sec_002",
  "sectionTitle": "项目实施计划",
  "blocks": [
    {
      "type": "paragraph",
      "text": "项目实施计划将围绕需求确认、方案设计、系统建设、联调测试、试运行和验收交付等关键环节展开，确保各阶段目标清晰、任务明确、成果可检查、问题可追踪。实施过程中将结合招标文件要求建立计划管理、沟通协调和风险预警机制，保障项目按计划推进。",
      "styleKey": "body"
    },
    {
      "type": "list",
      "ordered": true,
      "styleKey": "orderedList",
      "items": [
        "需求确认阶段重点完成业务需求、数据范围、接口条件、系统边界和验收要求的确认，形成后续设计和实施依据。",
        "方案设计阶段结合需求确认结果完成总体架构、功能模块、数据流程、接口方案和安全控制设计。",
        "建设实施阶段按照确认后的方案开展系统配置、功能开发、数据接入、规则配置和环境部署等工作。",
        "联调测试阶段围绕功能完整性、数据准确性、接口稳定性、权限安全和异常处理开展验证。",
        "试运行和验收阶段配合招标人开展问题整改、运行观察、资料整理和验收交付。"
      ]
    }
  ],
  "scoreCoverage": [
    {
      "itemKey": "score_002",
      "itemName": "实施计划",
      "covered": true,
      "coverageDescription": "正文明确了实施阶段、阶段任务、交付控制和风险管理机制。"
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
