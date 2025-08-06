# Pond0x-Dutch-Auction-Pooling-System
A system designed to allow community members to launch and contribute to auction pools. System is designed to be permissionless and fully automated with the goal of creating a safe and efficient way for many users to contribute to pools.
All smart contracts and automation bots are written using AI. Nothing here has been tested or checked.

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

