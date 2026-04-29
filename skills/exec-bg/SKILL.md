---
name: tyc-exec-bg
description: 投资决策前的关键人员背调工具 V2.0，对创始人/董事/核心高管执行 18 维现状 + 14 维历史穿透
category: invest
version: 2.0
---

# 高管背景核查 V2.0（个人穿透升级版）

## 触发条件

投前对法定代表人、实际控制人、CEO/COO/CFO 等核心高管做个人风险背调；并购前关键人员尽调。

关键词：高管背调、关键人员、个人风险、董监高画像、个人涉诉

## 输入要求

- **searchKey** (必填): 企业名称 或 信用代码
- **humanName** (必填): 目标人员姓名（"企业名 + 人员姓名"双锚定）

## 执行流程

### Step 1: 人员画像基础
- `get_person_profile` (`searchKey`, `humanName`) — 简介 + 商业角色 + 相关公司
- `get_person_partners` (`searchKey`, `humanName`) — 合作伙伴网络

### Step 2: 个人现状风险（6 维）
- `get_personnel_dishonest` — 失信
- `get_personnel_judgment_debtor` — 被执行人
- `get_personnel_high_consumption_ban` — 限制高消费
- `get_personnel_terminated_cases` — 终本案件
- `get_person_judicial_assistance` (`searchKey`, `humanName`) — 司法协助（含股权冻结）
- `get_person_risk_overview` — 个人风险总览

### Step 3: 控制企业网络
- `get_personnel_controlled_companies` — 实际控制的全部企业
- `get_personnel_related_companies` — 全部关联企业（任何角色）
- `get_personnel_legal_rep_roles` — 担任法定代表人企业
- `get_personnel_positions` — 在外任职

### Step 4: 个人历史穿透（V2.0 新增）
- `get_personnel_historical_dishonest` — 历史失信
- `get_personnel_historical_judgment_debtor` — 历史被执行
- `get_personnel_historical_high_consumption_ban` — 历史限高

## 输出格式

```markdown
# 高管个人风险档案 — {humanName} @ {searchKey}

## 一、个人画像
- 姓名: {humanName}
- 当前任职: {位置（董事长/CEO/...）}
- 主企业: {searchKey}
- 个人简介: ...

## 二、商业角色全景
- 法定代表人企业: {n} 家
- 实际控制企业: {n} 家
- 全部关联企业（任意角色）: {n} 家

## 三、个人现状风险（6 维）
| 风险类型 | 当前条数 | 状态 |
|---------|---------|------|
| 失信被执行 | ... | ... |
| 被执行人 | ... | ... |
| 限制高消费 | ... | ... |
| 终本案件 | ... | ... |
| 司法协助（冻结） | ... | ... |
| 综合风险评级 | — | 低/中/高 |

## 四、个人历史风险（V2.0 14 维）
| 历史维度 | 已结案数 | 结清状态 |
|---------|---------|---------|
| 历史失信 | ... | ... |
| 历史被执行 | ... | ... |
| 历史限高 | ... | ... |

## 五、合作伙伴网络
（关键合作伙伴 + 共同公司列表）

## 六、控制企业风险评估
- 控制企业中的高风险企业: {n} 家
- 已注销 / 吊销: {n} 家
- 出现失信/被执行的关联企业: {n} 家

## 七、综合背调结论
- 个人合规等级: A/B/C/D
- 是否建议作为投资关键人: 是 / 否 / 加强尽调
- 关键风险点: ...
```

## 错误处理

- `humanName` 在该企业无对应记录 → 提示"建议确认人员姓名"
- 个人维度数据缺失（部分维度 TYC 不提供）→ 标注"该维度暂无数据"

## 示例

输入: `searchKey = "字节跳动"`, `humanName = "张一鸣"`

## V2.0 升级说明

V2.0 引入 T1.1 的 `executive` 分类 11 个个人维度工具 + 4 个人员画像扩展工具（人员画像/合作伙伴/风险总览/司法协助），实现"双参数实体强锚定"，告别旧版基于企业数据反推个人的模糊核查。
