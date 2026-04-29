---
name: tyc-health-scan
description: 企业经营状态的动态追踪工具，通过招聘/招投标/舆情综合判断目标当前的经营活跃度与增长势能
category: invest
version: 1.0
---

# 经营健康度扫描

## 触发条件

投前快速筛查、投后被投企业月度健康度跟踪、合作伙伴持续监控。

关键词：经营健康度、活跃度、增长势能、投后管理

## 输入要求

- **searchKey** (必填): 企业名称 或 信用代码

## 执行流程

### Step 1: 工商基本面
- `get_company_registration_info`
- `get_company_scale` — 参保人数变化

### Step 2: 招聘动态（劳动力扩张信号）
- `get_recruitment_info` — 招聘职位、薪资、地区

### Step 3: 业务活跃度
- `get_bidding_info` — 招投标中标
- `get_company_announcement` — 企业公告
- `get_administrative_license` — 新增行政许可

### Step 4: 媒体声量
- `get_news_sentiment` — 新闻舆情（情感倾向）

### Step 5: 风险信号
- `get_risk_overview` — 综合风险

## 输出格式

```markdown
# 经营健康度扫描 — {name}

## 一、规模指标
- 参保人数: {socialStaffNum}（环比变化推断）
- 分支机构: {branchNum}
- 对外投资: {investNum}

## 二、招聘热度（增长信号）
- 在招职位数: {n}
- 主要城市: ...
- 学历分布: ...
- 招聘热度评级: ↑↑↑ / ↑ / → / ↓

## 三、业务活跃度
- 中标项次（近 3 月）: {n}
- 企业公告（近 3 月）: {n}
- 新增许可: {n}

## 四、媒体声量
- 新闻条数（近 3 月）: {n}
- 情感倾向: 正面 {x}% / 中性 {y}% / 负面 {z}%

## 五、综合健康度评分
- 增长势能: 强 / 平稳 / 衰退
- 经营健康度: A / B / C / D
- 关注点: ...
```

## 示例

输入: `searchKey = "蜜雪冰城股份有限公司"`
