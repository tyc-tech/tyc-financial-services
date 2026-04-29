---
name: tyc-ic-memo
description: PE/VC 投资决策的核心尽调工具，并行完成工商/股权/司法/知产全维度扫描，30 秒输出 IC 备忘录
category: invest
version: 1.0
featured: true
---

# IC Memo 投资备忘录 ⭐ 精选

## 触发条件

PE/VC 项目立项、投委会会前 IC Memo 出具、并购前期尽调时触发。

关键词：IC Memo、投委会、PE 尽调、VC 备忘录、PreMoney 评估

## 输入要求

- **searchKey** (必填): 目标企业名称 或 信用代码

## 执行流程

### Step 1: 主体与股权
- `get_company_registration_info` — 工商基础
- `get_actual_controller` — 实控人
- `get_shareholder_info` — 股东构成
- `get_equity_ratio` — 股权控制路径

### Step 2: 融资历史
- `get_financing_records` — 融资轮次、金额、估值、投资方

### Step 3: 司法风险
- `get_judicial_documents`
- `get_dishonest_info`
- `get_judgment_debtor_info`

### Step 4: 知识产权
- `get_patent_info` — 专利
- `get_trademark_info` — 商标
- `get_software_copyright_info` — 软件著作权
- `get_ipr_score` — 创新力评分

### Step 5: 经营画像
- `get_recruitment_info` — 招聘动态
- `get_news_sentiment` — 新闻舆情
- `get_team_members` — 核心团队
- `get_competitors` — 竞品

## 输出格式

```markdown
# IC Memo — {name}

> 项目代号: ... · 撰写人: AI · 出具时间: {ISO8601}

## 一、项目概览
- 公司名称: {name}
- 法定代表人: {legalPersonName}
- 实际控制人: {actualController.name} ({totalRatio})
- 成立日期 / 注册资本: {estiblishTime} / {regCapital}
- 业务定位: ...
- 行业: {industryAll.category}

## 二、股权结构
（含股权图，列出 TOP 5 股东）

## 三、融资历史
| 轮次 | 时间 | 金额 | 估值 | 投资方 |
|------|------|------|------|--------|

## 四、司法风险摘要
风险评级: 低 / 中 / 高

## 五、知识产权资产
- 专利: {n} 件（含发明授权 {x} 件）
- 商标: {n} 件
- 软件著作权: {n} 件
- 创新力评分: {score}

## 六、核心团队
（关键高管 + 简介）

## 七、竞品对照
（同行竞品列表 + 关键对照指标）

## 八、经营活跃度
- 招聘热度: ↑/↓
- 媒体声量: ↑/↓
- 中标记录: {n} 项

## 九、IC 决议建议
- 投资建议: 推进 / 暂缓 / 否决
- 估值区间: ...
- 主要风险: ...
- 后续 DD 重点: ...
```

## 示例

输入: `searchKey = "Moonshot AI 月之暗面 (北京月之暗面科技有限公司)"`

## V2.0 升级（向高管背调延伸）

可与 `tyc-exec-bg` 串联：完成 IC Memo 后自动对创始人/CEO 触发 30+ 维度个人风险扫描。
