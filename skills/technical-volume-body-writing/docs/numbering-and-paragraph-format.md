# 编号与段落格式规则

本文件定义技术卷正文生成、前端渲染和 Word 导出时必须遵守的编号体系、段落样式和格式映射规则。

## 总体原则

1. AI 不直接输出任何视觉格式；
2. AI 不直接输出章节编号或列表编号；
3. AI 不通过空格、换行、全角空格、制表符控制段落缩进；
4. AI 只输出结构化 blocks 和 `styleKey`；
5. 前端根据 blocks 渲染 TipTap JSON；
6. 后端根据 blocks 和 `styleKey` 生成 docx 样式。

## 技术卷编号体系

技术卷正文推荐使用以下编号体系：

| 层级 | 类型 | 编号样式 | 示例 | 控制方 |
|---|---|---|---|---|
| 一级章 | heading1 | 中文章序号 | 第一章 项目理解与需求分析 | 大纲/后端 |
| 二级节 | heading2 | 阿拉伯分级编号 | 1.1 招标人现状分析 | 大纲/后端 |
| 三级节 | heading3 | 阿拉伯分级编号 | 1.1.1 信息化系统现状 | 大纲/后端 |
| 四级小节 | heading4 | 中文括号编号 | （一）数据资源现状 | 渲染器，默认慎用 |
| 正文列表 | orderedList | 中文括号或阿拉伯括号编号 | （1）全面梳理数据来源 | 渲染器 |
| 无序列表 | unorderedList | 项目符号 | · 数据接入能力 | 渲染器 |

AI 只输出对应 block，不得把编号写进 `text` 或 `items`。

## 章节标题规则

### heading1

用于一级章标题。

格式建议：

- 编号：第一章、第二章、第三章；
- 字体：黑体；
- 字号：三号；
- 加粗：是；
- 对齐：居中；
- 首行缩进：0；
- 段前：12 磅；
- 段后：12 磅；
- 是否允许 AI 生成：默认不允许，由大纲树控制。

### heading2

用于二级节标题。

格式建议：

- 编号：1.1、1.2、2.1；
- 字体：黑体；
- 字号：小三或四号；
- 加粗：是；
- 对齐：左对齐；
- 首行缩进：0；
- 段前：6 磅；
- 段后：6 磅；
- 是否允许 AI 生成：默认不允许，由大纲树控制。

### heading3

用于三级节标题。

格式建议：

- 编号：1.1.1、1.1.2、2.1.1；
- 字体：黑体；
- 字号：四号；
- 加粗：是；
- 对齐：左对齐；
- 首行缩进：0；
- 段前：6 磅；
- 段后：3 磅；
- 是否允许 AI 生成：仅当 `generationConfig.allowSubHeadings=true` 时允许。

## 正文段落规则

### body

用于普通正文段落。

格式建议：

- 字体：宋体；
- 字号：小四；
- 加粗：否；
- 对齐：两端对齐；
- 首行缩进：2 字符；
- 行距：1.5 倍；
- 段前：0；
- 段后：0 或 6 磅；
- 段落之间：通过段后控制，不通过空行控制。

AI 输出要求：

```json
{
  "type": "paragraph",
  "text": "正文段落内容",
  "styleKey": "body"
}
```

禁止输出：

```json
{
  "type": "paragraph",
  "text": "　　正文段落内容",
  "styleKey": "body"
}
```

## 列表段落规则

### orderedList

用于正文分点表达。

格式建议：

- 编号样式：默认使用中文括号编号，例如（1）（2）（3）；
- 字体：宋体；
- 字号：小四；
- 行距：1.5 倍；
- 左缩进：根据 Word 模板统一控制；
- 悬挂缩进：根据 Word 模板统一控制；
- item 内不得手写编号。

AI 输出要求：

```json
{
  "type": "list",
  "ordered": true,
  "styleKey": "orderedList",
  "items": [
    "全面梳理现有系统、数据资源和接口条件。",
    "明确建设目标、实施边界和交付要求。"
  ]
}
```

禁止输出：

```json
{
  "type": "list",
  "ordered": true,
  "styleKey": "orderedList",
  "items": [
    "1. 全面梳理现有系统、数据资源和接口条件。",
    "2. 明确建设目标、实施边界和交付要求。"
  ]
}
```

### unorderedList

用于并列能力、特性或非顺序事项。

格式建议：

- 项目符号：由渲染器控制；
- 字体：宋体；
- 字号：小四；
- 行距：1.5 倍；
- item 内不得使用 `-`、`*`、`·` 手写项目符号。

## 表格段落规则

### standardTable

用于阶段、任务、成果、职责、风险、措施等结构化表达。

格式建议：

- 字体：宋体；
- 字号：五号；
- 表头：加粗；
- 边框：单线；
- 单元格内段落：不使用首行缩进；
- 单元格内文本：垂直居中或顶端对齐，由模板决定。

AI 输出要求：

```json
{
  "type": "table",
  "styleKey": "standardTable",
  "columns": ["阶段", "主要任务", "交付成果"],
  "rows": [
    ["需求确认", "确认业务需求、数据范围和接口条件", "需求确认记录"]
  ]
}
```

## 段落间距和空行规则

1. paragraph block 之间不得通过空字符串段落制造空行；
2. paragraph 的 `text` 字段不得包含连续换行；
3. 段前段后由样式统一控制；
4. 如果需要表达逻辑分组，应使用多个 paragraph 或 list block，而不是在一个 text 中换行。

## 前端 TipTap 映射建议

| block | TipTap 节点 | attrs |
|---|---|---|
| paragraph + body | paragraph | styleKey=body |
| list + orderedList | orderedList/listItem | numberingStyle=cn-parentheses |
| list + unorderedList | bulletList/listItem | bulletStyle=default |
| table + standardTable | table/tableRow/tableCell | styleKey=standardTable |
| heading + heading3 | heading | level=3, styleKey=heading3 |

前端展示建议：

```css
.bid-editor .ProseMirror p {
  text-indent: 2em;
  line-height: 1.8;
  text-align: justify;
}

.bid-editor .ProseMirror h1,
.bid-editor .ProseMirror h2,
.bid-editor .ProseMirror h3,
.bid-editor .ProseMirror h4 {
  text-indent: 0;
}

.bid-editor .ProseMirror li p,
.bid-editor .ProseMirror td p,
.bid-editor .ProseMirror th p {
  text-indent: 0;
}
```

## Word 导出映射建议

| styleKey | Word 样式 | 首行缩进 | 编号 |
|---|---|---|---|
| heading1 | 标题1 | 0 | 第一章 |
| heading2 | 标题2 | 0 | 1.1 |
| heading3 | 标题3 | 0 | 1.1.1 |
| body | 正文 | 2 字符 | 无 |
| orderedList | 正文列表 | 0 或悬挂缩进 | （1） |
| unorderedList | 项目符号列表 | 0 或悬挂缩进 | 项目符号 |
| standardTable | 表格正文 | 0 | 无 |

## 校验规则

生成后必须检查：

1. `paragraph.text` 不得以空格、全角空格或制表符开头；
2. `paragraph.text` 不得以 `1.`、`1、`、`一、`、`（一）`、`1.1` 开头；
3. `list.items` 不得包含手写编号；
4. `heading.text` 不得包含手写编号，除非该标题来自大纲树并由系统生成；
5. `text` 字段不得包含 Markdown 符号；
6. `styleKey` 必须属于允许范围。
