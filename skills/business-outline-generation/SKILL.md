---
name: business-outline-generation
description: 根据招标文件商务部分与证据片段生成商务卷目录结构，仅允许抽取与还原，不允许推理或补全结构
version: 1.0.0
language: zh-CN
---

# 商务卷大纲生成 Skill

## 使用场景

用于生成投标文件商务卷目录结构。

适用于：

1. 商务章节目录抽取
2. 招标文件商务结构还原
3. 商务条款整理与归类
4. 附件、声明、授权等结构整理

---

## ❗ 核心原则（最高优先级）

1. 仅允许“原文抽取”，禁止任何推理
2. 不得新增不存在的章节
3. 不得改写标题
4. 不得合并或拆分章节
5. 不得优化结构或顺序
6. 宁可缺失，不可臆造

---

## 📌 目录来源优先级

```text
商务目录结构 > templateRequirements > formatRequirements > attachmentRequirements > 证据片段
```

---

## 📌 层级规则

- 一级：商务主结构
- 二级：仅原文存在时输出
- 三级：仅原文明确出现时输出

---

## 🚫 禁止行为

❌ 生成商务方案/设计/策略
❌ 推导目录结构
❌ 补全缺失章节
❌ 引入技术内容（架构/系统/评分）

---

## 📦 输出要求

```json
{
  "documentTitle": "项目名称商务卷大纲",
  "chapters": [
    {
      "id": "ch-1",
      "title": "一、商务条款",
      "expanded": true,
      "sortNo": 1,
      "children": []
    }
  ]
}
```

---

## 📌 说明

- documentTitle = 项目名称 + 商务卷大纲
- chapters = 仅原文结构
- 不允许扩展字段
