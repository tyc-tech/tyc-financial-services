# 💼 tyc-financial-services

> **投资 / 金融服务 AI SKILL 集** — 天眼查 OpenAPI + MCP 协议驱动的 23 个投资向 SKILL

[![License](https://img.shields.io/badge/License-Apache%202.0-green.svg)](LICENSE)
[![MCP](https://img.shields.io/badge/MCP-TYC%20天眼查-orange.svg)](https://agent.tianyancha.com)
[![Claude](https://img.shields.io/badge/Claude%20Code-Compatible-purple.svg)](https://claude.ai)

---

## 1. 项目简介

`tyc-financial-services` 为 PE/VC 投资经理、投行 IBD、财务顾问（FA）、行业研究员、上市公司研究员、私募合规、园区招商团队提供 AI Agent SKILL 集，覆盖投资全流程：

- 立项画像
- IC Memo 投决备忘录
- 尽调清单 / 尽调报告
- 股权穿透与集团族谱
- 关联方识别
- ESG / 信用 / 诉讼 / IP 专项分析
- 上市公司财务深度
- 行业格局 / 并购筛查 / 融资追踪
- 投后健康度 / 破产预警
- 私募基金合规 / 园区企业画像

---

## 2. 核心价值

```
+---------------------------------------------------------------+
|                                                               |
|   投资全流程 AI SKILL 矩阵 · 23 个 × MCP × 天眼查数据          |
|                                                               |
|   立项 → 尽调 → 投决 → 投后 → 退出                             |
|   ───────────────────────────────                             |
|   /tyc-profile      →  /tyc-ic-memo     →  /tyc-ic-memo       |
|   /tyc-industry-*   →  /tyc-dd-checklist   /tyc-health-scan   |
|   /tyc-competitor   →  /tyc-equity-graph →  /tyc-bankruptcy-* |
|   /tyc-merger-*     →  /tyc-related-party                     |
|                                                               |
|   + 数据强项：                                                 |
|   /tyc-equity-graph · /tyc-group-analysis · /tyc-pf-compliance|
|   /tyc-financial-deep · /tyc-park-research                    |
|                                                               |
+---------------------------------------------------------------+
```

---

## 3. 快速开始（2 种装法选一）

### 方式 A · bash 一键脚本（推荐 · 30 秒搞定）

```bash
bash <(curl -sL https://raw.githubusercontent.com/tyc-opensource/tyc-financial-services/main/install_tyc_mcp.sh)
```

脚本会：① 提示输入 `TYC_MCP_API_KEY`（[申请](https://agent.tianyancha.com)）→ ② 自动写入 `~/.claude/.mcp.json` → ③ 复制 SKILL 到 `~/.claude/skills/tyc-*` → ④ 复制命令到 `~/.claude/commands/`。完成后**重启 Claude Code** 即可使用。

### 方式 B · 本地 plugin-dir（开发 / 调试）

```bash
git clone https://github.com/tyc-opensource/tyc-financial-services.git
cd tyc-financial-services
export TYC_MCP_API_KEY="your_api_key_here"
claude --plugin-dir .
```

启动后项目级 `.mcp.json` 自动加载。

---

### 试用

```
/tyc-ic-memo Moonshot AI 月之暗面
```

---

## 4. SKILL 清单（23 个）

### A. 核心投资 SKILL（10）

| # | 命令 | 名称 |
|---|------|------|
| 1 | `/tyc-ic-memo` | IC Memo 投资备忘录 ⭐ |
| 2 | `/tyc-profile` | 企业画像速览 |
| 3 | `/tyc-equity-structure` | 股权结构穿透 |
| 4 | `/tyc-competitor` | 竞品对比分析 |
| 5 | `/tyc-exec-bg` | 高管背景调查 |
| 6 | `/tyc-funding-history` | 融资历史追踪 |
| 7 | `/tyc-health-scan` | 企业健康度扫描 |
| 8 | `/tyc-bankruptcy-monitor` | 破产清算预警 |
| 9 | `/tyc-vc-research` | VC 投后管理 |
| 10 | `/tyc-financial-deep` | 上市公司财务深度 |

### B. 尽调与评估（9）

| # | 命令 | 名称 |
|---|------|------|
| 11 | `/tyc-dd-checklist` | 尽调清单自动化 |
| 12 | `/tyc-credit-rating` | 企业信用评级 |
| 13 | `/tyc-merger-screening` | 并购筛查 |
| 14 | `/tyc-industry-analysis` | 行业分析辅助 |
| 15 | `/tyc-related-party` | 关联方穿透识别 |
| 16 | `/tyc-ip-due-diligence` | 知识产权尽调 |
| 17 | `/tyc-litigation-analysis` | 诉讼风险分析 |
| 18 | `/tyc-esg-assessment` | ESG 风险评估 |
| 19 | `/tyc-supply-chain-risk-fs` | 供应链风险（投资短链）|

### C. 数据强项专项 SKILL（4）

| # | 命令 | 名称 | 数据特色 |
|---|------|------|---------|
| 20 | `/tyc-equity-graph` | 多层股权穿透 + Mermaid 可视化 | 任意层穿透，自动可视化 |
| 21 | `/tyc-group-analysis` | 集团族谱图（母子公司全图）| 集团成员/投资方/股东全景 |
| 22 | `/tyc-pf-compliance` | 私募基金管理人合规 | 中基协 + 诚信记录一体化 |
| 23 | `/tyc-park-research` | 园区 / 区域企业画像 | 经纬度 + 行政区批量画像 |

---

## 5. 典型场景

### 场景 A: Pre-IPO 项目 IC Memo（30 秒）

```
/tyc-ic-memo 某 Pre-IPO 项目
  ↓ 并发 5 步：股权 / 融资 / 司法 / 知产 / 经营
Claude: 输出 9 章 IC Memo（项目概览 / 股权 / 融资 / 司法 / IP / 团队 / 竞品 / 经营 / 决议）
```

### 场景 B: 股权穿透可视化

```
/tyc-equity-graph 某集团母公司
  ↓ 3-5 层股权穿透
Claude: 输出 Mermaid 图 + TOP 10 股东表 + UBO 识别
```

### 场景 C: 投后监控

```
/tyc-health-scan 被投组合 A
/tyc-bankruptcy-monitor 被投组合 A
  ↓ 风险 + 破产双扫描
Claude: 综合健康度评级 + 预警建议
```

### 场景 D: 并购筛查

```
/tyc-merger-screening "光伏组件制造" --region 江苏 --tag 专精特新
  ↓ 筛候选池 + 匹配度打分 + TOP 5 深度
Claude: 生成 TOP 5 深度画像 + Go/No-Go 建议
```

---

## 6. 天眼查 MCP 集成

单一 `tyc` MCP server，23 SKILL 共享业务语义聚合层 163 个工具：

| Go 包 | 工具数 | 本仓重点使用 |
|-------|--------|--------------|
| `company` | 49 | 工商 / 股东 / 财务（invest 全部）+ 股权图谱（equity-graph）+ 园区（park-research）|
| `risk` | 35 | 尽调风险扫描 / 诉讼 / ESG |
| `operation` | 32 | 融资 / 招投标 / 私募基金（pf-compliance）|
| `executive` | 15 | 高管背调（exec-bg）|
| `history` | 18 | 融资历史 / 趋势分析 |
| `intellectual_property` | 14 | IP 尽调（ip-due-diligence）|

详见 [docs/MCP_CONFIGURATION.md](docs/MCP_CONFIGURATION.md)。

---

## 7. 配置说明

参考 [docs/MCP_CONFIGURATION.md](docs/MCP_CONFIGURATION.md)。

---

## 8. 常见问题

**Q: 为什么 `/tyc-kyb` 不在本仓？**
A: 银行 KYB 场景放 `tyc-banking-plugin`，本仓面向投资角色。两仓可共存（共享 `.mcp.json`）。

**Q: `/tyc-supply-chain-risk-fs` 与 `tyc-supply-chain` 的 `/tyc-supply-risk` 有何差异？**
A: 本仓短链（5 步、投资视角 go/no-go），`tyc-supply-chain` 仓长链（18 类、采购深度）。场景不同，可联动使用。

---

## 9-12. 贡献 / License / 致谢 / 联系

Apache-2.0 · [LICENSE](LICENSE) · contact@tianyancha.com
