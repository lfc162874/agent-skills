# 技术卷大纲质量校验清单

模型输出后，必须根据以下规则进行自检。后端程序也建议按照本清单进行二次校验。

## 1. JSON 格式校验

必须满足：

1. 输出是单个 JSON 对象；
2. 不包含 Markdown 代码块；
3. 不包含解释文字；
4. 不包含多余前后缀；
5. 可被 JSON parser 正常解析；
6. 符合 `schemas/output-schema.json`。

## 2. 必填字段校验

必须包含：

1. `documentTitle`；
2. `structureStrategy`；
3. `chapters`；
4. `coverageMatrix`；
5. `warnings`。

## 3. 结构策略校验

`structureStrategy.type` 只能是：

1. `score_driven`；
2. `total_sub_total`；
3. `process_driven`；
4. `module_driven`；
5. `hybrid`；
6. `unknown`。

`structureStrategy.reason` 不能为空。

如果 `type = unknown`，则 `warnings` 必须说明原因。

## 4. 章节层级校验

必须满足 `generationConfig.chapterDepth`：

1. `chapterDepth = 1`：不得出现子级章节，或子级章节必须为空；
2. `chapterDepth = 2`：不得出现三级章节；
3. `chapterDepth = 3`：可以出现三级章节；
4. 不得超过配置深度。

## 5. 编号格式校验

建议满足：

1. 一级章节标题以中文序号开头，例如：`一、`、`二、`、`三、`；
2. 二级章节标题以数字编号开头，例如：`1.1`、`1.2`；
3. 三级章节标题以数字编号开头，例如：`1.1.1`、`1.1.2`；
4. `sortNo` 应与章节顺序一致。

## 6. 评分项覆盖校验

必须检查：

1. `coverageMatrix` 是否覆盖所有 `scoringItems`；
2. 高分评分项是否存在 `coveredBy`；
3. `coveredBy` 中引用的章节 id 是否真实存在；
4. `coverageStatus` 是否合理；
5. `not_covered` 的评分项是否写入 `warnings`；
6. `partially_covered` 的评分项是否说明原因。

## 7. 证据引用校验

必须检查：

1. 核心一级章节 `evidenceRefs` 不应为空；
2. 关键二级章节 `evidenceRefs` 不应为空；
3. `evidenceRefs.type` 只能是 `scoring`、`requirement`、`format`、`inferred`；
4. `evidenceRefs.chunkId` 应来自输入证据；
5. `type = inferred` 时，`reason` 必须说明推导依据。

## 8. 技术卷边界校验

章节标题不得混入商务卷、资格卷、报价文件内容。具体禁用内容以 `docs/forbidden-content.md` 为准。

如招标文件明确要求某类边界内容进入技术卷，必须在 `basis` 中说明依据。

## 9. 通用模板风险校验

如果出现以下章节，应检查是否有证据支撑：

1. 项目背景；
2. 项目概述；
3. 总体方案；
4. 技术路线；
5. 实施计划；
6. 质量保障；
7. 风险控制；
8. 应急预案；
9. 售后服务；
10. 培训方案；
11. 服务承诺。

没有证据支撑时，应视为低质量大纲。

## 10. 可用于正文生成校验

章节应满足：

1. 标题清晰；
2. 不重复；
3. 不交叉；
4. 不过细；
5. 不过泛；
6. 可以直接作为正文生成任务输入；
7. `basis` 能说明章节写作方向；
8. `evidenceRefs` 能提供正文生成依据。

## 11. 最终判定

如果满足以下条件，可判定为合格大纲：

1. JSON 可解析；
2. Schema 校验通过；
3. 高分评分项均已覆盖；
4. 无商务、资格、报价内容混入；
5. 章节结构符合 `chapterDepth`；
6. 章节依据基本完整；
7. `warnings` 合理；
8. 可支撑后续正文生成。
