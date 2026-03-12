# BERTopic Experiment Specification

## 1. Scope

本文件定义 `BERTopic/` 目录下的正式实验主线。其角色不是替代总研究计划，而是把研究问题转成可执行的实验流程。后续所有实验应以该文件为准，而不是继续在零散 notebook 中随意试跑。

## 2. Input Data

### Primary Source
- `BERTopic/chata_data/chat.json`

### Existing Reference Outputs
- `BERTopic/analysis_output/summary.json`
- `BERTopic/analysis_output/analysis_summary.md`

### Required Derived Tables
建议在正式执行前构造以下中间表：
- `turn_table.parquet`：每轮问答一行
- `session_table.parquet`：每个 session 一行
- `user_table.parquet`：每个 user 一行
- `sampling_table.parquet`：LLM 审查与人工复核样本池

## 3. Directory Policy

### `BERTopic/configs/`
存放：
- tokenization 配置
- BERTopic 参数配置
- BERT 分类标签定义
- LLM adjudication rubric

### `BERTopic/artifacts/`
存放：
- 嵌入文件
- topic 输出
- representative documents
- sampled hard cases
- error analysis tables

### `BERTopic/reports/`
存放：
- 实验规范
- 过程报告
- 阶段性结论

## 4. Experimental Phases

## Phase A: Data Structuring
目标：把原始 JSON 转换成适合统计分析和模型输入的标准结构。

关键输出：
- turn/session/user 三层数据表
- 长度、重复率、时间分布、空值报告
- user/session 连接键

## Phase B: ECG-Aware Preprocessing Comparison
目标：比较不同文本切分方案。

至少比较三组：
1. 原始文本直接送 tokenizer
2. Jieba + 用户词典 + 停用词
3. 规则增强切分（保护缩写、数值和单位）

评估维度：
- 术语保真度
- 指标 token 完整性
- 下游分类与 topic 质量变化

## Phase C: Embedding Strategy Comparison
建议比较：
- 现有 sentence-transformer embedding
- 中文 BERT embedding
- 医疗增强 encoder embedding（若本地模型可用）

输出：
- embedding 质量对 topic coherence/stability 的影响
- 对高频 ECG 指标问题簇的可分性比较

## Phase D: BERTopic Main Run
最少包含以下配置维度：
- min_topic_size
- UMAP dimensionality / neighbors
- HDBSCAN min_cluster_size / min_samples
- representation model
- 是否合并 outlier

关键输出：
- 全语料 topic 分布
- 高风险 topic 列表
- 结果解释相关主簇
- 长尾 outlier 样本池

## Phase E: Supervised Labeling Layer
使用 BERT 对下列任务建模：
- user intent classification
- risk-level classification
- response-style multi-label coding

训练数据来源：
- 规则弱标签
- topic representative documents
- LLM adjudicated hard cases
- 后续人工修订小样本

## Phase F: LLM Adjudication Layer
只处理：
- BERT 置信度低样本
- 高风险样本
- 主题边界样本
- 多轮语义依赖样本

要求：
- 固定 rubric
- 保存 prompt
- 保存输出
- 保存最终裁决标签

## Phase G: HCI Synthesis
在分析层面交叉以下维度：
- 意图 × 风险
- 主题 × 回答风格
- 用户复访 × 回答策略
- 就医建议 × 不确定性提示

目标不是再出一个 topic 图，而是产出可用于论文和系统设计的结论。

## 5. Minimum Output Set

后续正式实验至少应输出：
- `BERTopic/reports/analysis_flow_report.md`
- `BERTopic/artifacts/topic_summary.csv`
- `BERTopic/artifacts/topic_representatives.csv`
- `BERTopic/artifacts/hard_case_pool.csv`
- `BERTopic/artifacts/response_style_audit.csv`
- `BERTopic/artifacts/intent_transition_summary.csv`

## 6. Reliability Requirements

- 每个 BERTopic 主结果至少比较 3 个随机种子
- 每个关键分类任务提供 confusion matrix 与 hard-case review
- 每条高层 HCI 结论必须能回溯到 topic / sample / response evidence
- 不允许只凭单次 notebook 运行截图得出论文结论

## 7. Execution Rule

正式实验时，notebook 可以作为探索工具，但**不能再作为唯一记录载体**。所有关键参数、结果摘要、错误分析和阶段结论，必须同步写入 `BERTopic/reports/` 或 `BERTopic/artifacts/`，保证可审计与可复现。
