---
name: stock-analysis-cn
description: A股自选股智能分析系统，支持股票AI分析、行情查询、历史记录、组合管理和策略回测。
tags:
  - finance
  - stock
  - analysis
  - ai
  - trading
---

# A股自选股智能分析系统

连接本地运行的 daily_stock_analysis A股自选股智能分析系统 (http://192.168.1.49:8000)，提供完整的股票分析与投资管理功能。

## 概述

本技能基于 [ZhuLinsen/daily_stock_analysis](https://github.com/ZhuLinsen/daily_stock_analysis) 项目，通过 MCP 协议连接本地服务，为 OpenClaw 提供股票分析、行情查询、历史记录、组合管理和策略回测等完整功能。

## 功能特性

| 功能 | 说明 |
|------|------|
| **🤖 AI 股票分析** | 触发智能分析，生成个股深度报告（技术面+基本面+舆情） |
| **📊 行情查询** | 获取实时行情数据与历史 K 线数据 |
| **📝 历史记录** | 查询历史分析报告，支持按股票代码/日期筛选 |
| **💼 组合管理** | 账户管理、交易记录、风险分析与持仓优化 |
| **📈 策略回测** | 策略回测与绩效分析，评估投资策略有效性 |
| **🎯 Agent 问股** | 多轮对话问行情，支持 11 种内置策略 |

## 快速使用示例

### 股票分析
```
"分析贵州茅台 600519"
"帮我看看茅台的趋势"
"600519 怎么样，值得买入吗"
"分析 AAPL 的走势"
```

### 行情查询
```
"茅台现在的价格"
"600519实时行情"
"AAPL 昨天跌了多少"
```

### 历史记录
```
"茅台之前分析过吗"
"茅台的历史分析报告"
"查看最近的分析记录"
```

### 组合管理
```
"我的持仓情况"
"帮我计算一下持仓风险"
"查看最近的交易记录"
```

### 策略回测
```
"帮我回测一下均线策略"
"测试双均线策略在茅台上的表现"
```

## 配置要求

### 前置条件

1. **安装 daily_stock_analysis 项目**
   ```bash
   git clone https://github.com/ZhuLinsen/daily_stock_analysis.git
   cd daily_stock_analysis
   pip install -r requirements.txt
   ```

2. **配置环境变量**
   - 复制 `cp .env.example .env`
   - 配置 AI 模型（至少一个）：`GEMINI_API_KEY` / `OPENAI_API_KEY` / `AIHUBMIX_KEY`
   - 配置数据源：`TUSHARE_TOKEN`（可选，增强行情数据）

3. **启动服务**
   ```bash
   python main.py --webui
   # 或仅启动 API
   python main.py --webui-only
   ```

### MCP 配置

本技能通过 `mcporter` 管理 MCP 连接，配置文件位于 `/root/.openclaw/skills/stock-analysis-cn/mcp-config.json`。

默认配置：
```json
{
  "mcpServers": {
    "stock-analysis-cn": {
      "command": "uv",
      "args": [
        "--directory",
        "/tmp/daily_stock_analysis",
        "run",
        "mcp-server"
      ],
      "env": {
        "ANALYSIS_API_URL": "http://192.168.1.49:8000"
      }
    }
  }
}
```

## 使用场景

当用户需要以下服务时自动使用此技能：

- ✅ 分析 A股/港股/美股的个股或板块
- ✅ 查询特定股票的实时行情或历史走势
- ✅ 查看过往生成的分析报告
- ✅ 管理投资组合，进行风险监控
- ✅ 运行投资策略回测
- ✅ 通过 Agent 问股进行多轮对话分析

## 技术架构

```
OpenClaw
  ↓
stock-analysis-cn 技能（MCP 客户端）
  ↓
daily_stock_analysis 服务 (本地)
  ↓
AI 模型（LiteLLM 统一调用）
  ↓
数据源（AkShare / Tushare / YFinance / 新闻搜索）
```

## 支持市场

- **A股**：完整支持
- **港股**：完整支持
- **美股**：完整支持（含美股指数 SPX/DJI/IXIC）

## 注意事项

1. **服务状态**：daily_stock_analysis 服务需保持运行状态
2. **API 密钥**：至少配置一个 AI 模型 API Key
3. **数据质量**：配置 Tushare Pro 可增强历史行情数据质量
4. **时效性**：新闻搜索依赖配置的新闻搜索 API（Tavily/Brave/SerpAPI）

## 相关链接

- **项目主页**：[ZhuLinsen/daily_stock_analysis](https://github.com/ZhuLinsen/daily_stock_analysis)
- **完整文档**：https://github.com/ZhuLinsen/daily_stock_analysis/blob/main/docs/full-guide.md
- **常见问题**：https://github.com/ZhuLinsen/daily_stock_analysis/blob/main/docs/FAQ.md

---

*本技能基于开源项目 daily_stock_analysis，遵循 MIT 许可证开源。*
