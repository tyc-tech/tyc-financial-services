---
name: tyc-ip-due-diligence
description: 知识产权尽调 — 专利/商标/软著/著作权全维度盘点 + 技术竞争力评估，科技投资 / IPO 必备
category: invest
version: 1.0
---

# 知识产权尽调

## 触发条件

科技企业投资尽调、IPO 项目知识产权专项、技术并购标的评估、专利侵权风险评估时触发。

关键词：IP 尽调、专利、商标、软著、知识产权、创新力

## 输入要求

- **searchKey** (必填): 目标企业名称 或 信用代码

## 执行流程

### Step 1: 专利资产
- `get_patent_info` — 企业专利清单
- `search_patents` with `applicant={name}` — 补全申请人维度
- `get_ipr_score` — 创新力综合评分

### Step 2: 商标资产
- `get_trademark_info` — 商标清单
- `search_trademarks` with `applicant={name}` — 申请人维度补全
- `get_trademark_detail`（若需逐个看状态）

### Step 3: 软著与版权
- `get_software_copyright_info` — 软件著作权
- `get_copyright_work_info` — 作品著作权
- `get_internet_service_info` — 互联网服务（ICP / APP 备案）

### Step 4: 知产风险扫描
- `get_ipr_pledge` — 知产出质（融资抵押或侵权风险）

## 输出格式

```markdown
# 知识产权尽调报告 — {name}

> 出具: {ISO8601} · 企业代号: {creditCode}

## 一、综合评分
- **创新力评分（TYC 算法）: {score} / 100**
- 同行业排名: TOP {percentile}%
- 综合评价: 头部 / 腰部 / 初创

## 二、专利资产

### 核心数据
- 总量: {n} 件
- 发明专利: {n} 件（占比 {ratio}）
- 实用新型: {n} 件
- 外观设计: {n} 件
- 有效 / 失效: {n} / {n}

### 技术方向分布（IPC 主分类）
| IPC 分类 | 数量 | 占比 |
|---------|------|------|
| {A01B} | ... | ... |

### 近期专利（近 3 年）
| 申请日 | 名称 | 类型 | 状态 |
|--------|------|------|------|
| ... | ... | ... | ... |

## 三、商标资产
- 总量: {n} 件（按申请人维度核算）
- 注册 / 待审 / 失效: {n} / {n} / {n}
- 核心品类（尼斯分类）: ...
- 核心品牌: ...

## 四、软件著作权与版权
- 软件著作权: {n} 项
- 作品著作权: {n} 项
- 互联网服务: {ICP 备案 / APP 备案状态}

## 五、知产风险

### 出质 / 抵押
- 专利出质: {n} 件（已出质用于融资）
- 商标出质: {n} 件

### 侵权风险信号
（如有 `get_ipr_pledge` 返回异常项 → 标注）

## 六、综合结论
- 技术竞争力: 强 / 中 / 弱
- IP 保护完善度: 高 / 中 / 低
- 投资 IP 加分项: ...
- IP 相关尽调建议: 核心专利 FTO 排查、发明人稳定性、待审专利优先级
```

## 错误处理

- 专利/商标搜索返回 0 → 标注"无公开知识产权"，不报错
- `get_ipr_pledge` 无匹配 → 记为"无已知出质"
- 软著/作品数据缺失 → 该章节标注"数据暂不可用"

## 示例

输入: `searchKey = "华为技术有限公司"`

## 与其他 Skill 的关系

- 资产盘点向（偏法务） → `/tyc-ip-asset` (legal)
- 侵权排查向 → `/tyc-ip-infringement` (legal)
- IC Memo 投资决策 → `/tyc-ic-memo`
