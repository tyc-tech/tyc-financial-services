---
name: tyc-esg-assessment
description: ESG 风险评估 — 环境 / 社会 / 治理三维评分，ESG 基金 / 绿色金融 / 可持续投资筛选
category: invest
version: 1.0
---

# ESG 风险评估

## 触发条件

ESG 基金投前筛选、绿色信贷审批、可持续投资 due diligence、ESG 报告信息采集时触发。

关键词：ESG、环境、社会、治理、绿色金融、可持续投资、双碳

## 输入要求

- **searchKey** (必填): 目标企业名称 或 信用代码

## 执行流程

### Step 1: E（环境）维度
- `get_environmental_penalty` — 环境行政处罚
- `get_land_mortgage_info` — 土地使用（含 EIA 间接信号）
- `get_administrative_license` — 环评许可（若有 EIA 类许可）
- `get_historical_environmental_penalty` — 历史环保处罚

### Step 2: S（社会）维度
- `get_recruitment_info` — 招聘（用工规模与合规信号）
- `get_disciplinary_list` — 劳动监察黑名单
- `get_administrative_penalty` — 行政处罚（筛选涉劳动 / 涉消费者项）
- `get_company_scale` — 员工规模与参保人数
- `get_import_export_credit` — 海关信用（供应链 S 维度）

### Step 3: G（治理）维度
- `get_actual_controller` — 实控人结构清晰度
- `get_shareholder_info` — 股东透明度
- `get_equity_freeze` — 股权冻结（治理风险信号）
- `get_key_personnel` — 董事会构成（独立董事占比）
- `get_judicial_documents` — 涉诉（治理规范性）
- `get_dishonest_info` — 失信（治理合规性）

### Step 4: 正向加分项
- `get_honor_info` — 荣誉（含 ESG / CSR 荣誉）
- `get_credit_evaluation` — 信用评价
- `get_ranking_list_info` — 榜单（ESG 专项榜单）

## 输出格式

```markdown
# ESG 风险评估报告 — {name}

> 出具: {ISO8601} · 评估框架: TYC-ESG-v1.0

## 一、综合评级
- **综合评级: AAA / AA / A / BBB / BB / B / CCC**
- 综合得分: {score} / 100
- 同行业分位: TOP {percentile}%
- 投资建议: 可投 / 观察 / 负面清单

## 二、三维评分

### E（环境）: {escore} / 100
| 指标 | 评分 | 说明 |
|------|------|------|
| 环保处罚 | -{n} | 近 3 年 {n} 次 |
| 历史环保处罚 | -{n} | 累计 {n} 次 |
| 环评许可 | +{n} | 持有 EIA 类许可 {n} 项 |
| 土地合规 | {n} | 土地抵押状态 |

### S（社会）: {sscore} / 100
| 指标 | 评分 | 说明 |
|------|------|------|
| 劳动监察 | -{n} | 黑名单 {n} 次 |
| 涉劳动处罚 | -{n} | 近 3 年 {n} 次 |
| 用工规模 | +{n} | 参保 {n} 人 |
| 海关信用 | +{n} | {等级} |

### G（治理）: {gscore} / 100
| 指标 | 评分 | 说明 |
|------|------|------|
| 股权透明度 | {n} | 穿透 {层} / 实控人清晰度 |
| 股权冻结 | -{n} | 冻结 {n} 次 |
| 董事会结构 | {n} | 独立董事 {n} 人 |
| 涉诉规模 | -{n} | 裁判文书 {n} 篇 |
| 失信 | -{n} | 失信 {n} 次 |

## 三、正向加分项
- 荣誉: ESG / CSR 类荣誉 {n} 项
- 信用评价: {A / AA / AAA 等级}
- 榜单: 入选 {榜单名称}

## 四、主要 ESG 风险
- 🔴 高: {风险点}
- 🟡 中: {风险点}
- 🔵 低: {风险点}

## 五、改进建议
- E: ...
- S: ...
- G: ...

## 六、ESG 信息披露建议
- 需披露事项: 环保处罚、劳动监察、治理结构
- 建议 ESG 报告章节: ...
```

## 错误处理

- 环保 / 劳动数据 `_empty` → 该子维度评为"无已知风险"（满分）
- 全部风险维度 `_empty` → 输出"ESG 公开数据不足，建议结合企业自披露"
- 部分 tyc `error_code: 300000` → 归一化 0 count，不报错

## 示例

输入: `searchKey = "某 ESG 筛池候选企业"`

## 与其他 Skill 的关系

- 广义健康扫描 → `/tyc-health-scan`
- 法律风险可视化 → `/tyc-legal-risk` (legal)
- 供应链风险 → `/tyc-supply-chain-risk-fs`
