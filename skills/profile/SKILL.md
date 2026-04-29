---
name: tyc-profile
description: 投资前快速了解目标公司的轻量尽调工具，3 分钟生成一页纸企业画像
category: invest
version: 1.0
---

# 企业画像速览

## 触发条件

LP 推介前快速概览、项目初筛、内部立项汇报、销售前置背景调查。

关键词：企业画像、一页纸、quick profile、LP 推介、项目初筛

## 输入要求

- **searchKey** (必填): 企业名称 或 信用代码

## 执行流程

### Step 1: 工商核心
- `get_company_registration_info`
- `get_company_profile` — 企业简介
- `get_company_tags` — 企业标签
- `get_company_logo` — Logo

### Step 2: 关键风险（轻量）
- `get_risk_overview` — 综合风险一句话

### Step 3: 知产资产（轻量）
- `get_patent_info` — 专利数（取 total）
- `get_trademark_info` — 商标数（取 total）
- `get_ipr_score` — 创新力评分

### Step 4: 经营标签
- `get_ranking_list_info` — 上榜榜单
- `get_honor_info` — 荣誉

## 输出格式

```markdown
# {name} · 一页纸画像

[Logo]  📍 {city}{district}  📅 {estiblishTime}  💰 {regCapital}

## 主体信息
- 法人 / 实控人: {legalPersonName} / {actualController}
- 行业: {industryAll.category}
- 状态: {regStatus}
- 参保: {socialStaffNum} 人

## 业务简介
{profile}

## 标签
{tags}

## 创新力
- 专利 {n} | 商标 {n} | 软著 {n} | 创新力评分 {score}

## 风险
{risk_overview._summary}

## 荣誉
{honor list}

## 上榜榜单
{ranking list}
```

## 示例

输入: `searchKey = "理想汽车（北京理想汽车有限公司）"`
