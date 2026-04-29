---
name: tyc-equity-graph
description: 股权关系图谱深挖（TYC 独有），股权树/最短路径/关系图谱/控股穿透/总公司一站式分析
category: group
version: 1.0
---

# 股权关系图谱深挖（TYC 独有扩展）

## 触发条件

复杂股权架构梳理、两家企业关联关系研判、隐性关联交易挖掘、上市辅导前股权重组。

关键词：股权图谱、关系网络、最短路径、控股穿透、复杂架构

## 输入要求

- **searchKey** (必填): 主企业名称 或 ID
- **searchKey2** (可选): 第二端企业（启用最短关系路径分析）

## 执行流程

### Step 1: 总体股权结构
- `get_company_registration_info`
- `get_actual_controller` — 实控人
- `get_equity_ratio` — 控制结构 + 控股路径

### Step 2: 向上 / 向下穿透
- `get_parent_company` — 总公司（向上看）
- `get_controlled_companies` — 控股企业（向下穿透）
- `get_equity_tree` — 对外投资关系树（多层）

### Step 3: 关系图谱
- `get_relation_graph` (`searchKey`) — 一键图谱（节点 + 边）
- `get_shareholder_change` — 股权变更历史

### Step 4: 双企业最短路径（若提供 searchKey2）
- `get_relation_path` (`searchKey`, `searchKey2`) — 最短关联路径

## 输出格式

```markdown
# 股权关系图谱 — {searchKey}

## 一、总体架构
- 实控人: {actualController}
- 总持股: {totalRatio}
- 控制层级: {n} 层
- 总公司（如有）: {parentCompany}

## 二、向下控股穿透
| 层级 | 企业 | 持股比例 | 投资链 |
|------|------|---------|--------|
| 1 | 直接控股 | ... | ... |
| 2 | 间接控股 | ... | ... |
| 3+ | 深层控股 | ... | ... |

控制企业总数: {n} 家（含间接）

## 三、对外投资关系树
（树状结构，含直接 + 间接投资）

## 四、关系图谱（节点/边）
- 节点总数（人 + 企业）: {n}
- 边总数（关系）: {n}
- 关键节点: ...

## 五、股权变更历史
| 时间 | 变更项 | 变更前 | 变更后 |
|------|-------|--------|--------|

## 六、双企业最短路径分析（若提供 searchKey2）
- 起点: {searchKey}
- 终点: {searchKey2}
- 最短路径: A → 法人 → B → 投资 → C
- 关系强度: 强关联 / 弱关联 / 无关联

## 七、风险与建议
- 复杂架构风险: ...
- 隐性关联交易嫌疑点: ...
- 上市股权重组建议: ...
```

## 示例

输入: `searchKey = "腾讯控股有限公司"`, `searchKey2 = "美团（北京三快在线科技有限公司）"`
