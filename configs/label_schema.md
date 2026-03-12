# Label Schema Draft

## Intent Labels
- `result_interpretation`：请求解释 ECG 结果、术语或指标含义
- `definition_explanation`：询问疾病或指标定义
- `severity_risk`：询问是否严重、是否危险、是否紧急
- `daily_management`：饮食、作息、运动、观察建议
- `treatment_plan`：药物、手术、治疗路径、是否需要处理
- `cause_mechanism`：诱因、病因、发病机制
- `recheck_test`：复查、再做检查、监测方式
- `device_measurement`：设备使用、测量条件、佩戴问题
- `other`：无法可靠归入以上类别

## Risk Labels
- `low`：偏解释型、一般管理型、未出现急性风险信号
- `moderate`：涉及风险担忧、复查建议、症状变化，但未明显紧急
- `high`：涉及明显危险、急性恶化、强烈就医/急诊判断需求
- `uncertain`：仅凭当前文本无法稳定判断

## Response Style Labels
- `metric_explanation`
- `uncertainty_notice`
- `medical_escalation`
- `lifestyle_advice`
- `reassurance_tone`
- `followup_suggestion`
- `empty_or_failed_response`

## Additional Flags
- `contains_ecg_metric`
- `contains_condition_name`
- `contains_numeric_value`
- `contains_symptom_description`
- `contains_time_reference`
