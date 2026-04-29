---
name: tyc-park-research
description: 园区企业研究（TYC 独有），园区画像/入园企业搜索/经纬度雷达/附近公司一站式
category: industry
version: 1.0
---

# 园区企业研究（TYC 独有扩展）

## 触发条件

园区招商画像、产业集群分析、商业地理选址、银行支行获客、政府区域经济研究。

关键词：产业园、招商、产业集群、商业地理、选址、获客

## 输入要求

输入二选一：
- **searchKey** (园区/企业模式): 园区名称 或 企业名称
- **longitude** + **latitude** + **radius** (经纬度模式): 地理坐标雷达扫描

## 执行流程

### Mode A: 园区画像（输入园区名称）
- `get_park_info` (`searchKey`) — 园区基本信息（面积/主管单位/所属地区）
- `search_park_companies` (`searchKey`) — 入园企业列表

### Mode B: 企业园区归属（输入企业名称）
- `get_park_info` (`searchKey`) — 反查企业所在园区
- `get_company_location` — 企业经纬度

### Mode C: 经纬度雷达（输入经纬度）
- `get_nearby_companies` (`longitude`, `latitude`, `radius`) — 半径内附近公司

### Step 后续：选定关键企业再深入
- `get_company_registration_info`
- `get_company_logo` — 用于报告封面

## 输出格式

```markdown
# 园区企业研究 — {searchKey or 经纬度}

## Mode A: 园区画像
- 园区名称: {parkName}
- 面积: ...
- 主管单位: ...
- 所属地区: ...

### 入园企业（TOP 20）
| 企业 | 类型 | 成立日期 | 状态 | 法定代表人 |
|------|------|---------|------|----------|

### 园区产业集群分析
- 主导行业: {industry list}
- 上市公司数: {n}
- 高新技术企业数: {n}

## Mode B: 企业园区归属
- 企业: {name}
- 所属园区: {park}
- 经纬度: {longitude}, {latitude}

## Mode C: 经纬度雷达
查询坐标: {longitude}, {latitude} · 半径: {radius} 米

附近公司（按距离排序）:
| 企业 | 距离 | 经营状态 | 经纬度 |
|------|------|---------|--------|

## 商业洞察
- 行业聚集度: ...
- 区域经济活跃度: ...
- 招商画像 / 选址建议: ...
```

## 示例

输入 (Mode A): `searchKey = "中关村软件园"`
输入 (Mode C): `longitude = 116.391, latitude = 39.907, radius = 1000`
