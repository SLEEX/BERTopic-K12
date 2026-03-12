# LLM Adjudication Rubric Draft

## Purpose
本 rubric 用于裁决高不确定样本，而不是替代全量分类器。

## Apply To
- BERT 低置信度样本
- 多类竞争样本
- 高风险样本
- 多轮依赖样本

## Required Outputs
1. `primary_intent`
2. `secondary_intent`（可空）
3. `risk_level`
4. `response_style_labels`
5. `rationale`
6. `need_human_review`（yes/no）

## Rubric Rules
- 如果文本核心在于“报告结果是什么意思”，优先标记 `result_interpretation`
- 如果文本核心在于“严不严重/要不要去医院”，优先标记 `severity_risk`
- 如果文本主要问日常行为调整，标记 `daily_management`
- 如果文本混合多个问题，给出 `primary_intent` + `secondary_intent`
- 对高风险文本，禁止仅因安抚语气而降级风险判断
- 若缺乏足够证据，不得伪造确定性；应返回 `uncertain`

## Human Review Trigger
以下情况必须建议人工复核：
- 文本同时包含高风险症状与模糊 ECG 指标
- 模型在两个以上高优先级意图之间摇摆
- 无法分辨系统是否给出充分升级建议
- 数值、单位或疾病术语疑似被错误切分
