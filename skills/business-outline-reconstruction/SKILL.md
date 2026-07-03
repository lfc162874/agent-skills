---
name: business-outline-reconstruction
description: 从招标文件OCR/解析结果中还原商务卷目录结构，仅进行结构识别与恢复，不进行任何生成或推理
version: 2.0.0
language: zh-CN
---

# 商务卷结构恢复 Skill（Reconstruction）

## 使用场景

用于从招标文件解析结果中还原商务卷目录结构。

数据来源可能包括：

- OCR文本
- chunk结构
- 解析后的目录片段
- 招标文件商务章节

---

## 🧠 核心定位（非常重要）

本 Skill 不是生成器，而是：

```text
结构恢复器（Structure Reconstruction Engine）
```

---

## ❗ 核心原则（最高优先级）

1. ❌ 不生成任何新章节
2. ❌ 不补全缺失结构
3. ❌ 不改写标题
4. ❌ 不合并或拆分章节
5. ❌ 不优化结构或顺序
6. ✔ 只做原文结构还原

---

## ⚙️ 处理流程

### Step 1：目录识别
判断文本是否为：

- 一级目录
- 二级目录
- 三级目录
- 非目录噪声

---

### Step 2：噪声过滤
删除以下内容：

- 页码
- 页眉页脚
- 装订说明
- 排版符号
- 无关说明文字

---

### Step 3：结构恢复

仅保留原始结构信息：

- 层级来源必须来自原文
- 顺序必须严格保持
- 标题必须完全一致

---

## 📌 目录来源优先级

```text
商务目录结构 > templateRequirements > formatRequirements > attachmentRequirements > 证据片段
```

---

## 🚫 禁止行为

❌ 生成商务方案或设计
❌ 推导目录结构
❌ 补全缺失章节
❌ 引入技术内容（架构/系统/评分）
❌ 合并/拆分章节

---

## 📦 输出格式

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

## 📌 输出说明

- documentTitle = 项目名称 + 商务卷大纲
- chapters = 仅原文结构恢复
- 不允许附加任何推理字段
