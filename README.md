# 量化交易期末作业：A股多因子选股系统

## 项目概述

完整的量化交易流水线，包含数据获取、因子挖掘、因子分析、多因子模型构建和回测评估。

- **市场**：A股（沪深300精选成分股）
- **频率**：日频
- **策略**：多因子选股（月度调仓，Top-K选股）
- **因子**：20+手工因子 + RD-Agent自动因子挖掘
- **数据源**：akshare → 新浪财经（真实日频OHLCV，前复权）

## 项目结构

```
├── 01_data_download.ipynb       # 数据下载与清洗（akshare新浪源）
├── 02_factor_mining.ipynb       # 因子构建（手工+RD-Agent）
├── 03_factor_analysis.ipynb     # 因子IC/IR分析、筛选
├── 04_model.ipynb               # 多因子模型构建
├── 05_backtest.ipynb            # 回测与绩效归因
├── run_rdagent.ps1              # PowerShell调用WSL中RD-Agent
├── requirements.txt
├── data/                        # 数据缓存
├── rda_output/                  # RD-Agent输出
└── results/                     # 图表、回测结果
```

## 快速开始

### 环境要求
```bash
pip install -r requirements.txt
```

### 运行步骤
按顺序执行 01 ~ 05 号 Notebook：

1. **01_data_download.ipynb** — 下载沪深300成分股数据
2. **02_factor_mining.ipynb** — 构建因子 + 调用RD-Agent
3. **03_factor_analysis.ipynb** — 因子IC分析、筛选
4. **04_model.ipynb** — 多因子模型
5. **05_backtest.ipynb** — 回测评估

### RD-Agent 调用
```powershell
# 在PowerShell中运行（确保WSL已安装RD-Agent）
.\run_rdagent.ps1
```
注：RD-Agent需在WSL中预先安装配置，使用ubantu 启动RD-Agent

## 股票池

从沪深300中精选约40只行业代表股，覆盖：
- 金融（银行、保险、券商）
- 消费/食品饮料
- 科技/电子
- 家电/制造
- 医药
- 汽车
- 能源/材料
- 地产/基建
- 交通运输

## 因子体系

| 类别 | 因子 | 数量 |
|------|------|------|
| 动量 | 多周期动量、RSI、MACD、ROC | 6 |
| 波动 | 历史波动率、ATR、布林带 | 7 |
| 量价 | 成交量比率、OBV、MFI | 5 |
| 技术 | 均线偏离、威廉%R、CCI | 7 |
| 反转 | 短期反转、涨跌比、回撤 | 3 |
| RD-Agent | 自动化挖掘因子 | 动态 |

## 绩效指标

回测输出以下指标：
- 累计收益率
- 年化收益率 / 年化波动率
- 夏普比率 / Sortino比率 / Calmar比率
- 最大回撤
- 超额收益（vs 等权基准）
- 胜率 / 盈亏比

## 数据源

- [akshare](https://pypi.org/project/akshare/) — 新浪财经接口
- A股股票代码格式：6位数字（600xxx=上海，000xxx/002xxx/300xxx=深圳）

## 注意事项

1. A股交易成本：单边约0.1%（含印花税0.05%+佣金0.025%+过户费）
2. A股交易制度：T+1，涨跌停±10%（科创/创业±20%）
3. RD-Agent需在WSL中预先安装配置
