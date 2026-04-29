---
name: tyc-bankruptcy-monitor
description: 债权管理中的破产风险持续监控工具，破产重整/清算公告关键节点实时预警
category: invest
version: 1.0
---

# 企业破产预警监控

## 触发条件

债权管理周期性体检、重要客户破产风险跟踪、关联企业破产事件预警。

关键词：破产预警、债权保全、清算公告、重整申请

## 输入要求

- **searchKey** (必填): 监控企业名称 或 信用代码

## 执行流程

### Step 1: 破产现状
- `get_bankruptcy_reorganization` — 破产重整记录
- `get_simple_cancellation_info` — 简易注销公告
- `get_liquidation_info` — 清算信息

### Step 2: 司法风险（破产前置信号）
- `get_judgment_debtor_info` — 被执行
- `get_dishonest_info` — 失信
- `get_terminated_cases` — 终本案件
- `get_cancellation_record_info` — 注销备案

### Step 3: 资产保全风险
- `get_judicial_auction` — 司法拍卖
- `get_equity_freeze` — 股权冻结

### Step 4: 综合
- `get_risk_overview`

## 输出格式

```markdown
# 破产风险监控 — {name}

## 一、破产现状
- 破产重整: 是 / 否（{n} 次）
- 简易注销公告: 是 / 否
- 清算公告: 是 / 否

## 二、破产前置信号
| 信号 | 数量 | 严重度 |
|------|------|--------|
| 被执行人 | ... | ... |
| 失信记录 | ... | ... |
| 终本案件 | ... | ... |

## 三、资产处置
- 司法拍卖: {n} 项
- 股权冻结: {n} 项

## 四、破产风险评级
- 当前等级: 安全 / 黄色预警 / 红色预警
- 预警触发原因: ...

## 五、债权人行动建议
- 是否触发紧急债权申报: 是 / 否
- 建议措施: 加速回款 / 财产保全 / 强制执行 / 维持
```

## 示例

输入: `searchKey = "ABC 房地产开发有限公司"`
