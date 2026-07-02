# 技术卷大纲生成示例

## 示例一：流程型 + 模块响应型混合结构

### 输入特征

评分标准包含：

1. 技术方案；
2. 实施方案；
3. 质量保障；
4. 售后服务。

采购需求包含：

1. 模型中心；
2. 知识库中心；
3. 权限中心；
4. 智能审查中心；
5. 文档导出中心。

### 推荐结构策略

```json
{
  "type": "hybrid",
  "reason": "评分标准包含技术方案、实施方案、质量保障、售后服务等得分项，同时采购需求包含多个明确建设模块，因此采用评分项覆盖优先的混合结构。"
}
```

### 推荐大纲示例

```json
{
  "documentTitle": "小舢大模型平台建设项目技术卷大纲",
  "structureStrategy": {
    "type": "hybrid",
    "reason": "评分标准包含技术方案、实施方案等得分项，同时采购需求包含多个明确建设模块，因此采用评分项覆盖优先的混合结构。"
  },
  "chapters": [
    {
      "id": "ch-1",
      "title": "一、项目理解与需求分析",
      "expanded": true,
      "sortNo": 1,
      "basis": "承接技术方案评分项中的完整性、合理性要求，并对采购需求进行总体响应。",
      "evidenceRefs": [
        {
          "type": "scoring",
          "chunkId": "chunk-score-001",
          "reason": "技术方案评分项要求体现方案完整性和合理性。"
        }
      ],
      "children": [
        {
          "id": "s-1-1",
          "title": "1.1 项目建设目标理解",
          "expanded": false,
          "sortNo": 1,
          "basis": "围绕项目建设目标展开，为总体方案提供依据。",
          "evidenceRefs": [
            {
              "type": "requirement",
              "chunkId": "chunk-req-001",
              "reason": "采购需求明确要求建设多个平台功能模块。"
            }
          ],
          "children": [
            {
              "id": "t-1-1-1",
              "title": "1.1.1 建设背景与目标分析",
              "expanded": false,
              "sortNo": 1,
              "basis": "用于支撑正文中对项目目标的说明。",
              "evidenceRefs": []
            }
          ]
        }
      ]
    },
    {
      "id": "ch-2",
      "title": "二、总体技术方案",
      "expanded": true,
      "sortNo": 2,
      "basis": "直接覆盖技术方案评分项，说明总体架构、技术路线和平台设计思路。",
      "evidenceRefs": [
        {
          "type": "scoring",
          "chunkId": "chunk-score-001",
          "reason": "技术方案评分项要求方案完整、合理、先进。"
        }
      ],
      "children": []
    },
    {
      "id": "ch-3",
      "title": "三、核心功能模块建设方案",
      "expanded": true,
      "sortNo": 3,
      "basis": "响应采购需求中的模型中心、知识库中心、权限中心、智能审查中心、文档导出中心。",
      "evidenceRefs": [
        {
          "type": "requirement",
          "chunkId": "chunk-req-001",
          "reason": "采购需求明确列出多个建设模块。"
        }
      ],
      "children": []
    },
    {
      "id": "ch-4",
      "title": "四、项目实施与交付方案",
      "expanded": true,
      "sortNo": 4,
      "basis": "覆盖实施方案评分项，说明实施计划、进度安排、人员分工和交付保障。",
      "evidenceRefs": [
        {
          "type": "scoring",
          "chunkId": "chunk-score-002",
          "reason": "实施方案评分项要求描述项目实施计划和交付保障措施。"
        }
      ],
      "children": []
    }
  ],
  "coverageMatrix": [
    {
      "scoringItem": "技术方案",
      "maxScore": 20,
      "coveredBy": ["ch-1", "ch-2", "ch-3"],
      "coverageStatus": "covered",
      "reason": "项目理解、总体技术方案和核心功能模块建设方案共同覆盖技术方案完整性、合理性和先进性要求。"
    },
    {
      "scoringItem": "实施方案",
      "maxScore": 15,
      "coveredBy": ["ch-4"],
      "coverageStatus": "covered",
      "reason": "项目实施与交付方案直接覆盖实施计划、进度安排、人员分工和交付保障要求。"
    }
  ],
  "warnings": []
}
```

## 反例一：通用模板目录

### 错误大纲

```text
一、项目背景
二、总体方案
三、技术路线
四、实施计划
五、质量保障
六、售后服务
```

### 错误原因

该大纲属于通用模板，未体现评分标准，未响应具体采购需求，也没有说明评分项覆盖关系。

## 反例二：混入商务资格内容

### 错误大纲

```text
一、营业执照
二、企业资质
三、项目业绩
四、报价方案
五、售后服务
```

### 错误原因

营业执照、企业资质、项目业绩、报价方案属于商务卷、资格卷或报价文件内容，不得作为技术卷章节生成。
