Solana 交易机器人（Beta版）使用指南
目录

    项目概述

快速开始
准备工作
配置设置
启动机器人
核心配置详解
钱包配置
网络连接
机器人参数
买入设置
卖出设置
高级功能
Warp 交易服务
常见问题解决
RPC 节点问题
代币账户缺失
技术支持
免责声明
项目概述

Solana 交易机器人是一款自动化交易工具，专为 Solana 区块链设计，用于自动买卖代币。核心功能包括：

    🚀 实时监控市场条件（流动性池销毁、铸币权限放弃等）
    ⚡ 支持毫秒级交易响应
    🛡️ 多层风控机制（止盈止损、池流动性检测）
    📊 灵活的狙击列表功能

    ​​适用场景​​：代币狙击、流动性监控、自动化交易策略执行

快速开始
准备工作

    创建新的空 Solana 钱包（推荐Phantom钱包

    ）
    转入少量 SOL（建议1-2 SOL）
    将部分 SOL 兑换为：
        USDC（如果配置中设置 QUOTE_MINT=USDC）
        WSOL（如果配置中设置 QUOTE_MINT=WSOL）

配置设置

    复制配置文件模板：

cp .env.copy .env

编辑 .env 文件：

    # ===== 基本配置 =====
    PRIVATE_KEY=你的钱包私钥

    # ===== 网络配置 =====
    RPC_ENDPOINT=https://your-rpc-endpoint
    RPC_WEBSOCKET_ENDPOINT=wss://your-websocket-endpoint
    COMMITMENT_LEVEL=finalized

    # ===== 交易设置 =====
    QUOTE_MINT=USDC
    QUOTE_AMOUNT=0.5
    AUTO_SELL=true

启动机器人

# 安装依赖
npm install

# 启动交易机器人
npm run start

    ✅ 成功启动后将显示实时监控界面：

    readme/output.png

核心配置详解
钱包配置
参数	必填	示例值	说明
PRIVATE_KEY	是	3zH5...d9B	Base58格式的Solana私钥
网络连接
参数	推荐值	说明
RPC_ENDPOINT	https://mainnet.helius-rpc.com/	Solana主网RPC地址
RPC_WEBSOCKET_ENDPOINT	wss://mainnet.helius-rpc.com	WebSocket实时数据地址
COMMITMENT_LEVEL	finalized	交易确认等级
机器人参数
参数	默认值	功能说明
LOG_LEVEL	info	日志等级 (debug/trace/error)
ONE_TOKEN_AT_A_TIME	true	单次只处理一个代币
TRANSACTION_EXECUTOR	(空)	交易执行器 (warp/jito)
CUSTOM_FEE	(空)	Warp交易手续费(SOL)
买入设置
参数	示例值	说明
QUOTE_MINT	USDC	交易基准代币
QUOTE_AMOUNT	0.5	每次买入金额
AUTO_BUY_DELAY	1000	买入延迟(ms)
BUY_SLIPPAGE	10	买入滑点容差(%)
MAX_BUY_RETRIES	3	最大买入重试次数
卖出设置
参数	示例值	说明
AUTO_SELL	true	启用自动卖出
TAKE_PROFIT	30	盈利30%时止盈
STOP_LOSS	20	亏损20%时止损
PRICE_CHECK_DURATION	60000	价格监控时长(ms)
SELL_SLIPPAGE	15	卖出滑点容差(%)
高级功能

# ===== 狙击列表配置 =====
USE_SNIPE_LIST=true
SNIPE_LIST_REFRESH_INTERVAL=30000

# ===== 安全过滤设置 =====
MIN_POOL_SIZE=1.0      # 最小池流动性(SOL)
MAX_POOL_SIZE=100       # 最大池流动性(SOL)
CHECK_IF_BURNED=true    # 仅交易已销毁流动性的代币
CHECK_IF_MINT_IS_RENOUNCED=true # 检查铸币权限是否放弃

    ​​狙击列表使用​​：

        创建 snipe-list.txt 文件
        每行添加一个代币地址：

    7iK1...a5d
    9jM8...b2e

        机器人会实时监控列表中的代币

Warp 交易服务
启用方法

TRANSACTION_EXECUTOR=warp
CUSTOM_FEE=0.006  # 推荐手续费(最低0.0001 SOL)

安全说明

    🔐 ​​私钥安全​​：私钥仅在本地签名
    🗑️ ​​无数据存储​​：不保存任何交易记录
    ⚠️ ​​费用说明​​：仅交易成功时收费

性能对比
执行器	成功率	延迟	费用
标准RPC	~60%	>1000ms	仅Gas费
Warp	~95%	<500ms	Gas+0.006SOL
常见问题解决
bigint 模块错误

bigint: Failed to load bindings, pure JS will be used

​​解决方案​​：

# Windows 用户
npm install --global windows-build-tools
npm rebuild bigint --update-binary

# macOS 用户
xcode-select --install
npm rebuild bigint --update-binary

# Linux 用户
sudo apt-get install build-essential python3
npm rebuild bigint --update-binary

# 所有系统通用
rm -rf node_modules package-lock.json
npm install --force

RPC 节点问题

​​错误提示​​：

Error: 410 Gone: The RPC call or parameters have been disabled

​​解决方案​​：

    更换高性能RPC节点：

    RPC_ENDPOINT=https://mainnet.helius-rpc.com/
    RPC_WEBSOCKET_ENDPOINT=wss://mainnet.helius-rpc.com

    联系提供商开通高级API权限

代币账户缺失

​​错误提示​​：

Error: No SOL token account found in wallet

​​解决方法​​：

    创建USDC/WSOL账户：

# 使用@solana/spl-token创建
npx spl-token create-account USDC

兑换初始资金：
readme/wsol.png

# 免责声明 
 
Solana交易机器人按原样提供，用于学习目的。 
交易加密货币和代币涉及风险，过去的表现并不预示未来的结果。 
使用此机器人的风险由您自行承担，我们不对使用机器人时产生的任何损失负责。
