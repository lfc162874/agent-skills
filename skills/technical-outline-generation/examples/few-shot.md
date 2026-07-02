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

### 内部推荐结构策略

内部可采用 `hybrid` 混合结构：

```text
评分标准包含技术方案、实施方案、质量保障、售后服务等得分项，同时采购需求包含多个明确建设模块，因此采用评分项覆盖优先的混合结构。
```

注意：当前兼容模式下，结构策略只能用于内部推导，不得输出到最终 JSON。

### 当前兼容输出示例

```json
{
  "documentTitle": "小舢大模型平台建设项目技术卷大纲",
  "chapters": [
    {
      "id": "ch-1",
      "title": "一、项目理解与需求分析",
      "expanded": true,
      "sortNo": 1,
      "children": [
        {
          "id": "s-1-1",
          "title": "1.1 项目建设目标理解",
          "expanded": false,
          "sortNo": 1,
          "children": [
            {
              "id": "t-1-1-1",
              "title": "1.1.1 建设背景与目标分析",
              "sortNo": 1
            },
            {
              "id": "t-1-1-2",
              "title": "1.1.2 核心需求与评分关注点分析",
              "sortNo": 2
            }
          ]
        },
        {
          "id": "s-1-2",
          "title": "1.2 采购需求响应分析",
          "expanded": false,
          "sortNo": 2,
          "children": []
        }
      ]
    },
    {
      "id": "ch-2",
      "title": "二、总体技术方案",
      "expanded": true,
      "sortNo": 2,
      "children": [
        {
          "id": "s-2-1",
          "title": "2.1 总体架构设计",
          "expanded": false,
          "sortNo": 1,
          "children": []
        },
        {
          "id": "s-2-2",
          "title": "2.2 技术路线与实现思路",
          "expanded": false,
          "sortNo": 2,
          "children": []
        }
      ]
    },
    {
      "id": "ch-3",
      "title": "三、核心功能模块建设方案",
      "expanded": true,
      "sortNo": 3,
      "children": [
        {
          "id": "s-3-1",
          "title": "3.1 模型中心建设方案",
          "expanded": false,
          "sortNo": 1,
          "children": []
        },
        {
          "id": "s-3-2",
          "title": "3.2 知识库中心建设方案",
          "expanded": false,
          "sortNo": 2,
          "children": []
        },
        {
          "id": "s-3-3",
          "title": "3.3 权限中心建设方案",
          "expanded": false,
          "sortNo": 3,
          "children": []
        }
      ]
    },
    {
      "id": "ch-4",
      "title": "四、项目实施与交付方案",
      "expanded": true,
      "sortNo": 4,
      "children": []
    },
    {
      "id": "ch-5",
      "title": "五、质量保障与运维服务方案",
      "expanded": true,
      "sortNo": 5,
      "children": []
    }
  ]
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

该大纲属于通用模板，未体现评分标准，未响应具体采购需求，也无法体现评分项覆盖关系。

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
