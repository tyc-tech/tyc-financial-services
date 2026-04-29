---
name: tyc-dd-checklist
description: 尽调清单自动化 — 标准化尽调清单自动填充 + 进度追踪，PE/VC/投行项目组首日 DD 套件
category: invest
version: 1.0
---

# 尽调清单自动化

## 触发条件

PE/VC、投行项目组启动尽调首日，需要标准化尽调清单 + 企业数据自动填充时触发。

关键词：尽调清单、DD Checklist、due diligence、尽调进度、VDR、尽调分工

## 输入要求

- **searchKey** (必填): 目标企业名称 或 信用代码

## 执行流程

### Step 1: 主体与股权（工商维度）
- `get_company_registration_info` — 工商登记基本信息
- `get_shareholder_info` — 股东构成
- `get_actual_controller` — 实控人
- `get_key_personnel` — 主要人员（法代 / 高管）
- `get_branches` — 分支机构
- `get_external_investments` — 对外投资

### Step 2: 财务（财务维度）
- `get_financial_data` — 财务数据
- `get_company_scale` — 企业规模
- `get_annual_reports` — 年报

### Step 3: 风险扫描（合规维度）
- `get_risk_overview` — 风险总览
- `get_dishonest_info` — 失信
- `get_judgment_debtor_info` — 被执行人
- `get_administrative_penalty` — 行政处罚
- `get_bankruptcy_reorganization` — 破产

### Step 4: 知识产权（技术维度）
- `get_patent_info` — 专利
- `get_trademark_info` — 商标
- `get_software_copyright_info` — 软件著作权
- `get_ipr_score` — 创新力评分

### Step 5: 经营与资质（商业维度）
- `get_bidding_info` — 中标记录
- `get_qualifications` — 企业资质
- `get_administrative_license` — 行政许可
- `get_financing_records` — 融资历史

## 输出格式

```markdown
# 尽调清单 — {name}

> 出具时间: {ISO8601} · 撰写人: AI · 企业代号: {creditCode}

## A. 主体与股权（已填充 6/6）
- [x] A1. 工商登记核验（{name} / {creditCode} / {legalPersonName}）
- [x] A2. 股东构成（TOP 5：...）
- [x] A3. 实控人（{actualController}，累计持股 {totalRatio}）
- [x] A4. 主要人员（法代、董事、监事清单）
- [x] A5. 分支机构（{总数} 家，{涉及省份}）
- [x] A6. 对外投资（{总数} 家）

## B. 财务（已填充 3/3）
- [x] B1. 财务数据（最近 3 年营收、利润）
- [x] B2. 企业规模（参保人数、人员规模）
- [x] B3. 年报（最近 {n} 年年报摘要）

## C. 合规风险（已填充 5/5）
- [x] C1. 风险总览（总数 {n}）
- [x] C2. 失信（{n} 条）
- [x] C3. 被执行人（{n} 条）
- [x] C4. 行政处罚（{n} 条）
- [x] C5. 破产清算（{状态}）

## D. 知识产权（已填充 4/4）
- [x] D1. 专利（{总数}，发明授权 {n}）
- [x] D2. 商标（{n} 件）
- [x] D3. 软件著作权（{n} 件）
- [x] D4. 创新力评分（{score}）

## E. 经营资质（已填充 4/4）
- [x] E1. 招投标（{n} 次中标）
- [x] E2. 企业资质（{高资质清单}）
- [x] E3. 行政许可（{n} 项）
- [x] E4. 融资历史（最近轮次 {round}）

---

## 二、待 PM 补充（人工环节）
- [ ] F. 现场访谈（创始人 / CFO / CTO）
- [ ] G. 合同调阅（VDR）
- [ ] H. 审计底稿对照

## 三、进度汇总
- 自动填充: **22/22 条**
- 人工环节: **待 0/3 条**
- 总体进度: 22/25 (88%)
```

## 错误处理

- 单条命令失败 → 对应 checklist 项标注 `[!]` + 失败原因
- `_empty: true` → 标注 `[~] 暂无数据`（非失败）

## 示例

输入: `searchKey = "北京百度网讯科技有限公司"`

## 与其他 Skill 的关系

- 完整版备忘录 → `/tyc-ic-memo`
- 深度股权穿透 → `/tyc-equity-graph`
- 财务深度分析（上市） → `/tyc-financial-deep`
