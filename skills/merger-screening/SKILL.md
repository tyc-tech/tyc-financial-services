---
name: tyc-merger-screening
description: 并购筛查 — 潜在标的批量筛选 + 匹配度打分 + 交易可行性分析，M&A / 战略投资场景
category: invest
version: 1.0
---

# 并购筛查

## 触发条件

企业战略并购方向立项、产业投资基金挖掘标的、券商 MA 业务推盘、买方 FA 筛池时触发。

关键词：并购、M&A、战投、产业并购、标的筛选、可行性

## 输入要求

- **industry** (必填): 行业关键词（如"光伏组件"、"精密减速器"）
- **region** (可选): 地域限定（如"江苏"、"长三角"）
- **tag** (可选): 标签（如"专精特新"、"小巨人"）
- **revenueMin** / **revenueMax** (可选): 营收区间过滤

## 执行流程

### Step 1: 目标池生成
- `search_companies_by_industry_region` — 按行业 + 地域初筛
- `search_companies_by_tag` — 如需标签过滤
- `search_companies_by_ranking` — 如需榜单对齐

> 生成 TOP-N 候选池（默认 N=20）

### Step 2: 逐个标的快速画像（并发调用）
对每个候选标的并发调用：
- `get_company_registration_info`
- `get_company_scale` — 规模
- `get_financial_data` — 财务
- `get_actual_controller` — 控制架构
- `get_shareholder_info` — 股东构成
- `get_risk_overview` — 风险

### Step 3: 匹配度评分（每个标的）
按以下维度打分（权重可配）：
- 行业契合度（30%）— `get_products_info` 业务线重合度
- 规模匹配度（20%）— 按 revenue / 员工数
- 控制可获得性（20%）— 分散股权加分 / 国资控股减分
- 风险健康度（15%）— 基于风险总览
- 创新能力（15%）— `get_ipr_score`

### Step 4: TOP-5 标的深度画像
对 TOP-5 拉取：
- `get_financing_records` — 历史融资估值（重要：判断定价基准）
- `get_competitors` — 竞品对标
- `get_patent_info` / `get_trademark_info` — 资产盘点

## 输出格式

```markdown
# 并购标的筛查报告 — {industry}

> 筛查条件: 行业={industry} · 地域={region} · 标签={tag}
> 候选池: {N} 家 · 精选: TOP 5 · 出具: {ISO8601}

## 一、标的池总览

| 排名 | 公司 | 注册资本 | 规模 | 所在地 | 匹配分 | 状态 |
|-----|------|---------|------|--------|--------|------|
| 1 | ... | ... | ... | ... | 87 | ✅ 推荐 |
| ... | | | | | | |

## 二、TOP 5 深度画像

### #1 {name}
- 基础: {creditCode} / {estiblishTime} / {regCapital}
- 实控: {actualController.name} ({totalRatio})
- 股东结构: 分散 / 集中 / 国资 / PE 持有
- 财务: 营收 {amount} / 净利润 {amount}
- 近期融资: {round} / {amount} / 估值 {valuation}
- 专利 / 商标: {n} / {n}
- 风险: 低 / 中 / 高
- **交易可行性分析**:
  - 控股权可获得性: 易 / 中 / 难
  - 估值锚点: 近期融资估值 {valuation}
  - 主要风险: ...

...（依次列出 #2 - #5）

## 三、Go / No-Go 建议

| 标的 | 推进优先级 | 主要逻辑 |
|------|---------|---------|
| #1 | P0 | 规模契合 + 股权分散 + 专利壁垒 |
| #2 | P1 | 规模契合但国资控股 |
| #3 | P2 | 创新力突出但估值偏高 |
```

## 错误处理

- 搜索返回 0 → 扩大行业关键词或放开地域/标签约束
- 单个标的数据不全（无财务） → 该标的标注 `[!] 财务数据不足`，仍纳入排序
- 所有候选均有 `_empty` → 输出"行业内公开数据不足"提示

## 示例

输入:
- `industry = "光伏组件制造"`
- `region = "江苏"`
- `tag = "专精特新"`

## 与其他 Skill 的关系

- 已锁定单一标的 → `/tyc-ic-memo`
- 做行业格局分析 → `/tyc-industry-analysis`
- 评估控制权穿透 → `/tyc-equity-graph`
