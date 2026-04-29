---
name: tyc-related-party
description: 关联方穿透识别 — 股权穿透 + 实控人控制企业 + 共同董监高识别，IPO 关联交易披露核心
category: invest
version: 1.0
---

# 关联方穿透识别

## 触发条件

IPO 项目关联交易披露、上市公司审计独立性核查、反洗钱 PEP 识别、实控人调查时触发。

关键词：关联方、关联交易、IPO 披露、一致行动人、实控人控制企业、共同董监高

## 输入要求

- **searchKey** (必填): 目标企业名称 或 信用代码

## 执行流程

### Step 1: 股权侧关联方
- `get_shareholder_info` — 直接股东
- `get_beneficial_owners` — 受益所有人（UBO）
- `get_actual_controller` — 实际控制人
- `get_equity_tree` — 股权树（多层）
- `get_parent_company` — 母公司
- `get_external_investments` — 对外投资（识别子公司）

### Step 2: 实控人控制面（同一实控人下的兄弟公司）
若 Step 1 识别到实控人 A：
- `get_controlled_companies`（with searchKey=A.name）— A 控制的所有企业
- 得出「同一实控人」的兄弟公司清单

### Step 3: 董监高交叉关联
对目标公司核心董监高（法代、董事、监事）逐个调用：
- `get_personnel_controlled_companies` — 该人控制的其他企业
- `get_personnel_related_companies` — 该人关联的其他企业
- `get_person_partners` — 该人的合作伙伴

> 识别「共同董监高」关联方

### Step 4: 图谱验证
- `get_relation_graph` — 目标企业与已识别关联方的关系图（验证）
- `get_relation_path` — 必要时计算两点之间的最短关联路径

## 输出格式

```markdown
# 关联方穿透报告 — {name}

> 出具: {ISO8601} · 企业代号: {creditCode}

## 一、关联方清单（共 {N} 个）

### A. 股权关联（{n} 个）
| # | 关联方 | 关联类型 | 股权比例 / 层级 | 控制性 |
|---|-------|---------|----------------|-------|
| 1 | {实控人} | 实际控制人 | 穿透后 {ratio} | 控制 |
| 2 | {母公司} | 直接控股股东 | {ratio} | 控制 |
| 3 | {子公司 A} | 直接投资 | {ratio} | 控制 |
| ... | | | | |

### B. 同一实控人关联（{n} 个）
| # | 关联方 | 共同实控人 | 该方持股 | 备注 |
|---|-------|----------|---------|------|
| 1 | {兄弟公司} | {A} | {ratio} | 同业 / 上下游 / 其他 |

### C. 共同董监高关联（{n} 个）
| # | 关联方 | 共同人员 | 本企业角色 | 对方角色 |
|---|-------|---------|----------|---------|
| 1 | ... | {张三} | 董事 | 法定代表人 |

## 二、风险提示

### 关联交易风险
- 🔴 高: 与 {n} 家同一实控人企业存在业务重叠
- 🟡 中: 共同董监高 {n} 人需披露
- 🔵 低: 股权关联方已知清晰

### IPO / 审计披露建议
- 需披露关联方: {完整清单}
- 重点关注: {TOP 3}
- 建议尽调: 近 3 年关联交易金额、定价公允性

## 三、关系图（Mermaid）

```mermaid
graph TB
  TARGET[{name}]
  CTRL[{实控人}]
  PARENT[{母公司}]
  BRO1[{兄弟公司1}]
  SUB1[{子公司1}]

  CTRL -->|100%| PARENT
  PARENT -->|{ratio}| TARGET
  CTRL -->|{ratio}| BRO1
  TARGET -->|{ratio}| SUB1
```
```

## 错误处理

- `get_controlled_companies` 的实控人姓名为空 → 跳过 Step 2
- 董监高 > 10 人 → 仅对 TOP 5 核心（法代 + 董事长 + 总经理 + 财务总监 + 董秘）做 Step 3
- `_empty: true` 场景 → 报告标注"无公开关联方"，不报错

## 示例

输入: `searchKey = "某拟 IPO 公司"`

## 与其他 Skill 的关系

- 纯股权图谱可视化 → `/tyc-equity-graph`
- 集团族谱（母子公司全图） → `/tyc-group-analysis`
- 高管个人背调 → `/tyc-exec-bg`
