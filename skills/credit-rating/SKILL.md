---
name: tyc-credit-rating
description: 企业信用评级 — 多维度信用评分 + 授信额度与利率建议，适用银行信贷 / 供应链金融 / 保理
category: invest
version: 1.0
---

# 企业信用评级

## 触发条件

银行对公信贷评审、供应链金融白名单准入、保理公司授信、ToB 赊销准入时触发。

关键词：信用评级、授信额度、贷后预警、AA 评级、保理、赊销

## 输入要求

- **searchKey** (必填): 目标企业名称 或 信用代码

## 执行流程

### Step 1: 基础资信（正向维度）
- `get_company_registration_info` — 工商基础
- `get_financial_data` — 财务表现
- `get_company_scale` — 企业规模

### Step 2: 信用加分项
- `get_credit_evaluation` — 信用评价
- `get_honor_info` — 荣誉资质
- `get_import_export_credit` — 进出口信用

### Step 3: 风险扣分项（司法）
- `get_dishonest_info` — 失信
- `get_judgment_debtor_info` — 被执行人
- `get_high_consumption_restriction` — 限高

### Step 4: 风险扣分项（财产）
- `get_equity_pledge_info` — 股权出质
- `get_equity_freeze` — 股权冻结
- `get_chattel_mortgage_info` — 动产抵押
- `get_stock_pledge_info` — 股权质押（上市）

### Step 5: 风险扣分项（税务与经营）
- `get_tax_arrears_notice` — 欠税
- `get_tax_violation` — 税收违法
- `get_business_exception` — 经营异常

## 输出格式

```markdown
# 企业信用评级报告 — {name}

> 评级出具: {ISO8601} · 评级模型: TYC-CR-v1.0

## 一、综合评级
- **评级结果：AA / A / BBB / BB / B / CCC / D**
- 综合得分：{score} / 100
- 建议授信额度：¥{min} - ¥{max}
- 建议利率区间：{minRate}% - {maxRate}%

## 二、评分明细

| 维度 | 得分 | 权重 | 加权 | 说明 |
|------|------|------|------|------|
| 基础资信 | 85 | 25% | 21.3 | 注册资本实缴 {ratio} |
| 财务能力 | 78 | 20% | 15.6 | 最近营收 {amount} |
| 经营稳定性 | 72 | 15% | 10.8 | 成立 {years} 年 |
| 信用正向 | 90 | 10% | 9.0 | 荣誉 {n} 项 |
| 司法风险 | -15 | 15% | -2.25 | 失信 {n} 条 |
| 财产风险 | -8 | 10% | -0.8 | 股权冻结 {n} 条 |
| 税务风险 | -5 | 5% | -0.25 | 欠税 {amount} |
| **合计** | **—** | **100%** | **{total}** | |

## 三、风险预警
- 🔴 高危: {风险清单}
- 🟡 关注: {风险清单}
- 🔵 信息: {风险清单}

## 四、授信建议
- **建议通过 / 人工复核 / 谢绝**
- 还款能力: 强 / 中 / 弱
- 担保要求: 无 / 保证 / 抵押 / 混合
- 复评周期: 1 / 3 / 6 / 12 个月

## 五、历史趋势（如适用）
（若有历史评级记录，展示评级变化）
```

## 错误处理

- 财务数据不可得（非上市+未披露） → 该维度降权至 5%
- 风险维度全部 `_empty` → 记为"无公开风险"，不扣分
- 税务违法 tyc `error_code: 300000` → 该维度得 0（不扣分）

## 示例

输入: `searchKey = "宁德时代新能源科技股份有限公司"`

## 与其他 Skill 的关系

- 银行客群专用入口 → `/tyc-kyb` (banking)
- 贷后监控 → `/tyc-credit-monitor` (banking)
- 供应商准入 → `/tyc-new-supplier` (supply)
