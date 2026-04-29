---
name: tyc-equity-structure
description: 投资决策前的控制权核查工具，自动完成多层股权穿透并生成可视化结构图
category: invest
version: 1.0
---

# 股权结构穿透分析

## 触发条件

DD 阶段控制权识别、关联交易风险评估、上市辅导前股权梳理。

关键词：股权穿透、控制权、关联交易、可视化股权图

## 输入要求

- **searchKey** (必填): 企业名称 或 信用代码

## 执行流程

### Step 1: 直接股权
- `get_shareholder_info` — 直接股东
- `get_shareholder_change` — 股权变更历史

### Step 2: 多层穿透
- `get_equity_ratio` — 控制结构 + 控股路径
- `get_actual_controller` — 实控人
- `get_beneficial_owners` — UBO

### Step 3: 关系图谱
- `get_equity_tree` — 对外投资树（向下）
- `get_relation_graph` — 一键图谱（含法人/股东/高管）

### Step 4: 控股识别
- `get_controlled_companies` — 实控人控制企业全集

## 输出格式

```markdown
# 股权结构穿透 — {name}

## 一、直接股权层
（TOP 10 股东表 + 出资比例）

## 二、控制路径
- 实控人: {name}
- 控制路径深度: {n} 层
- 总持股比例: {totalRatio}
- 表决权比例: {voteRatio}

## 三、最终受益人 (UBO)
（自然人列表 + 最终股份）

## 四、股权变更历史（最近 5 次）
| 时间 | 变更项 | 变更前 | 变更后 |
|------|-------|--------|--------|

## 五、关联企业网络
- 控股企业（实控人维度）: {n} 家
- 对外投资企业: {n} 家
- 一致行动人: ...

## 六、关联交易风险点
（识别可能的关联交易关键词）
```

## 示例

输入: `searchKey = "美团（北京三快在线科技有限公司）"`
