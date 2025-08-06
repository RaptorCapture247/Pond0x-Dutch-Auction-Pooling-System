# PNDC Token Pooling System for Dutch Auction Bidding on Pond0x

This document outlines the design of a system for pooling ERC-20 PNDC tokens into pools labeled with either an SPL or ERC-20 token contract address. These pools enable users to participate in Dutch auctions hosted by Pond0x at [pond0x.com/mining/bid](https://pond0x.com/mining/bid). The auction system is external and not part of this design.

All code derived from AI
[Smart contracts and management bots](https://claude.ai/share/7e052df8-cea0-4f62-ab18-392200d94115)

[UI/App](https://v0.dev/chat/pndc-token-pools-k1Lj2g0y32Y)

## System Overview
The system allows users to:
- Create or join pools labeled with an SPL or ERC-20 token contract address.
- Pool PNDC tokens to bid in Dutch auctions.
- Set pool goals and timeframes for bidding eligibility.
- Store tokens in a multisig ERC-20 wallet linked to an SPL wallet.
- Distribute auction rewards to users based on their contribution to the pool.

## Pool Creation and Management
- **Pool Creation**:
  - Users can start a new pool by selecting an SPL or ERC-20 token contract address as the pool’s label.
  - Multiple pools with the same token label can exist.
  - Pools have a **maximum capacity of 1 trillion PNDC tokens**.
  - **Pool Goals**:
    - Users set a goal between **50 billion and 1 trillion PNDC tokens**, in increments of **50 billion**.
    - Once the goal is reached, the pool becomes eligible to bid in the Dutch auction but can continue accepting tokens until it reaches 1 trillion or wins an auction.
  - **Timeframe**:
    - Users set a bidding timeframe between **1 week and 25 weeks**, in increments of **1 week**.
    - If the pool does not reach its goal or win an auction within the timeframe, all tokens are returned to users proportional to their contributions.

- **User Contributions**:
  - Users can add PNDC tokens to multiple pools or the same pool multiple times, as long as the pool is open and active.
  - Tokens are **immediately transferred** to a **multisig ERC-20 wallet** linked to an SPL wallet for secure storage.

## Dutch Auction Bidding
- **Eligibility**:
  - Pools that meet their goal are eligible to bid in the Dutch auction hosted by Pond0x.
  - Eligible pools pause accepting additional tokens when the auction goes live and resume after the auction closes, unless they reach the 1 trillion token cap.
- **Bidding Process**:
  - The **largest pools bid first** in the Dutch auction.
  - The **first bid wins**, and the auction closes.
  - If a pool wins, it is **locked**, and its tokens are sent to the auction.

## Reward Distribution
- **Reward Receipt**:
  - Winning pools receive rewards in **SPL or ERC-20 tokens** from the auction.
  - **SPL rewards** are bridged and swapped for **ETH**.
- **Fee Retention**:
  - **3% of rewards** are retained by the system, converted to ETH, and used to cover gas fees for transactions.
- **Distribution**:
  - After a set (currently unspecified) time, rewards are distributed to users based on their percentage of the total pool at the time the winning bid was placed.
  - Rewards are sent to the wallet from which the user contributed tokens.
- **Pool Closure**:
  - Once all rewards are distributed, the pool is marked as **complete** and **permanently closed**.

## Key Constraints
- Maximum pool size and bid amount: **1 trillion PNDC tokens**.
- Pool goals: **50 billion to 1 trillion PNDC tokens**, in **50 billion increments**.
- Bidding timeframe: **1 to 25 weeks**, in **1-week increments**.
- Tokens are stored in a **multisig ERC-20 wallet** linked to an SPL wallet.
- The system does not include the Dutch auction itself, which is managed externally by Pond0x.

## Core Contracts

### 1. **PNDC Pool Manager** - Main pool management contract
- Pool creation with configurable goals (50B-1T PNDC in 50B increments)
- Time-based pool expiration (1-25 weeks)
- Multi-contribution support per user
- Automatic goal tracking and auction eligibility
- Reward distribution with 3% system fee
- Emergency token return for expired pools

### 2. **Auction Manager** - Handles dutch auction logic
- Auction lifecycle management
- Largest-pool-first bidding priority
- Winner selection and pool locking
- Integration with external Pond0x auction system

### 3. **Reward Distributor** - Cross-chain reward handling
- SPL token bridging to ETH
- ERC-20 reward distribution
- Automatic system fee collection
- Proportional reward calculation

### 4. **Multisig Wallet Manager** - Secure token storage
- Pool-specific multisig wallet management
- Cross-chain wallet coordination
- Authorization tracking

### 5. **Bridge Contract** - Cross-chain functionality
- SPL ↔ ERC-20 token bridging
- Transaction verification
- Emergency withdrawal capabilities

## Additional System Components

### 6. **Pool Factory** - Standardized pool creation
- Fee-based pool creation
- Authorization management
- Integrated multisig setup

### 7. **Pool Analytics** - System statistics
- Pool performance tracking
- User contribution analytics
- Global system metrics

### 8. **Emergency Manager** - System safety
- Emergency pause functionality
- System-wide emergency mode
- Multi-operator emergency response

### 9. **System Governance** - Decentralized governance
- Proposal creation and voting
- Parameter updates through governance
- Token-weighted voting system

## Key Features Implemented

✅ **Pool Management**: Create pools with goals, deadlines, and label tokens  
✅ **Multi-contribution**: Users can contribute to multiple pools multiple times  
✅ **Auction Integration**: Automatic bidding for eligible pools  
✅ **Cross-chain Support**: SPL token bridging and reward distribution  
✅ **Security**: Reentrancy protection, pausable contracts, emergency functions  
✅ **Governance**: Decentralized parameter updates and system management  
✅ **Analytics**: Comprehensive tracking of pool and user statistics  

## Security Considerations

- **Reentrancy Guard**: Prevents reentrancy attacks
- **Safe ERC20**: Secure token transfers
- **Pausable**: Emergency system shutdown
- **Access Control**: Role-based permissions
- **Input Validation**: Comprehensive parameter checking

## Deployment Order

1. Deploy core token contracts
2. Deploy PNDCPoolManager
3. Deploy AuctionManager, RewardDistributor
4. Deploy support contracts (Factory, Analytics, etc.)
5. Configure contract addresses and permissions
6. Initialize system parameters


## Core Bot Components

### 1. **Pond0x Auction Bot** - External Auction Monitor
- Monitors Pond0x.com for live auctions
- Automatically starts internal auctions when external ones begin
- Coordinates auction timing and state management
- Publishes auction events to other bots via Redis

### 2. **Pool Monitor Bot** - Pool State Tracking
- Tracks all pool states and activities
- Listens for pool creation, contributions, and goal achievements
- Monitors pool expiration and handles token returns
- Updates Redis cache for real-time pool data

### 3. **Bidding Bot** - Automated Bidding Logic
- Executes bidding strategy when auctions go live
- Prioritizes largest pools for bidding
- Implements smart timing and gas optimization
- Places bids automatically based on pool eligibility

### 4. **Reward Distribution Bot** - Cross-chain Rewards
- Handles reward distribution after auction wins
- Manages SPL token bridging to ETH
- Processes reward calculations and distributions
- Tracks pending rewards and completion status

### 5. **Bridge Bot** - Cross-chain Operations
- Monitors Solana network for bridge requests
- Handles SPL ↔ ERC-20 token conversions
- Processes bridge transactions automatically
- Maintains bridge transaction history

### 6. **Health Monitor Bot** - System Health
- Monitors all system components
- Checks Ethereum and Solana connections
- Validates contract health and Redis connectivity
- Sends alerts for system issues

### 7. **Analytics Bot** - Data Collection & Reporting
- Collects system metrics every 5 minutes
- Generates hourly and daily reports
- Tracks pool performance and user activity
- Provides system optimization recommendations

### 8. **Security Monitor Bot** - Security & Anti-fraud
- Detects suspicious contribution patterns
- Monitors for potential bot behavior
- Tracks unusual gas usage and failed transactions
- Flags accounts for investigation

## Supporting Infrastructure

### **Config Manager** - Dynamic Configuration
- Environment variable integration
- Runtime configuration updates
- Configuration file management
- Hot-reload capabilities

### **Database Manager** - Persistent Storage
- Pool and auction data storage
- Transaction history tracking
- System metrics archival
- Analytics data management

### **Notification Manager** - Alert System
- Multi-channel notifications (Webhook, Email, Discord)
- Template-based messaging
- Priority-based alerting
- Real-time system updates

### **Bot Manager** - Orchestration
- Centralized bot management
- Graceful startup and shutdown
- Health monitoring and status reporting
- HTTP health check endpoints

## Key Features

✅ **Real-time Monitoring**: 30-second check intervals for critical operations  
✅ **Fault Tolerance**: Automatic retry logic with exponential backoff  
✅ **Cross-chain Support**: Full Ethereum and Solana integration  
✅ **Security Monitoring**: Anti-fraud and suspicious activity detection  
✅ **Comprehensive Logging**: Winston-based structured logging  
✅ **Redis Integration**: Fast caching and inter-bot communication  
✅ **Graceful Shutdown**: Proper cleanup on system termination  
✅ **Health Checks**: HTTP endpoints for monitoring system status  

## Installation & Setup

```bash
# Install dependencies
npm install web3 ethers axios node-cron winston redis @solana/web3.js express

# Set environment variables
export ETH_RPC_URL="your_ethereum_rpc"
export ETH_PRIVATE_KEY="your_private_key"
export POOL_MANAGER_ADDRESS="deployed_contract_address"
export POND0X_API_KEY="your_api_key"
export REDIS_HOST="localhost"
export ALERT_WEBHOOK_URL="your_webhook_url"

# Run the bot system
node bot-system.js
```

## Configuration Options

The system supports extensive configuration through environment variables:
- **Network Settings**: RPC URLs, private keys, gas settings
- **Contract Addresses**: All deployed contract addresses
- **Monitoring Intervals**: Check frequencies and retry limits
- **Alert Settings**: Webhook URLs and notification channels
- **Database Settings**: Connection strings and options

## Monitoring & Alerts

The system provides multiple monitoring interfaces:
- **HTTP Health Checks**: `/health` and `/metrics` endpoints
- **Real-time Alerts**: Webhook notifications for critical events
- **Structured Logging**: Comprehensive log files for debugging
- **Performance Metrics**: System latency and resource usage tracking

