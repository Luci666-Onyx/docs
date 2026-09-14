# Source: https://tari.com/integration-guide

About TariBuildCommunity

active miners

[Download](https://tari.com/downloads)

About TariBuildCommunity

active miners

[Download](https://tari.com/downloads)

active miners

## Table of Contents

1. Overview & Architecture

2. Development Environment Setup

3. Node Setup & Configuration

4. Wallet Creation & Management

5. Deposit Monitoring

6. Withdrawal Processing

7. Security Best Practices

8. Production Deployment

9. Troubleshooting

10. Complete API Reference

11. AppendixWhite Paper wXTM

# Complete Tari Exchange Integration Guide

June 17th, 2025

## What You'll Learn

This comprehensive guide covers everything needed to integrate Minotari (XTM) into your cryptocurrency exchange, from node setup to transaction monitoring and fund management. Every example includes pseudocode for security understanding and grpcurl commands for testing.

⚠️ **Security Warning:**

This guide contains placeholder credentials and URLs marked as "PLACEHOLDER" or "secure\_password\_here". Replace ALL placeholders with actual values and use secure credential management (environment variables, secrets managers) in production. Never use hardcoded credentials.

## Table of Contents

1. Overview & Architecture

2. Development Environment Setup

3. Node Setup & Configuration

4. Wallet Creation & Management

5. Deposit Monitoring

6. Withdrawal Processing

7. Security Best Practices

8. Production Deployment

9. Troubleshooting

10. Complete API Reference

11. AppendixWhite Paper wXTM

## 🏗️ 1. Overview & Architecture

![Overview and Architecture](https://tari.com/_next/static/media/overview.0t0isdjmjsp7f.svg)

### 🎯 Integration Components

- **Minotari Base Node:** Syncs with the Tari network and provides blockchain data
- **Read-Only Wallet:** Monitors incoming deposits without spending ability
- **Cold Storage Wallet:** Secure wallet for processing withdrawals
- **gRPC Interface:** API communication layer

### 🌐 Network Support

🐢 Mainnet

📟 Testnet

🌈 Nextnet

## ⚙️ 2. Development Environment Setup

**⚠️ Prerequisites:**

Before starting, ensure you have administrative access to your deployment environment and understand basic cryptocurrency security principles.

### 🔧 System Requirements

- **OS:** Linux (Ubuntu 20.04+), macOS (10.15+), or Windows 10+
- **RAM:** Minimum 4GB, Recommended 8GB+
- **Storage:** 50GB+ SSD for blockchain data
- **Network:** Stable internet connection

### 🛠️ Development Tools Setup

Node.js

Python

Rust

PHP

grpcurl

Copied!

![Copy](https://tari.com/_next/static/media/copy.32ozctvwtsgtm.svg)

`# Install Node.js 18+ and npm curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash - sudo apt-get install -y nodejs # Install gRPC packages npm install @grpc/grpc-js @grpc/proto-loader # Install additional utilities npm install axios lodash`

### 📦 Install Tari Binaries

Linux

macOS

Windows

Copied!

![Copy](https://tari.com/_next/static/media/copy.32ozctvwtsgtm.svg)

`# Download latest release curl -L # PLACEHOLDER: Replace with actual Tari Linux download URL curl -L https://github.com/tari-project/tari/releases/latest/download/minotari-linux.tar.gz -o minotari.tar.gz # Extract tar -xzf minotari.tar.gz # Move to system path sudo mv minotari/* /usr/local/bin/ # Verify installation minotari_node --version`

## 🖥️ 3. Node Setup & Configuration

**📍 Step 1:**

Setting up your Minotari base node is the foundation of your exchange integration. This node will sync with the Tari network and provide blockchain data.

### 🚀 Initial Node Setup

#### Pre-Setup Checklist:

- Server meets minimum requirements
- Firewall configured for Tari ports (18141, 18142, 18143)
- Tor installed (Linux/macOS) or IP configured
- Backup strategy planned

#### 

1

Initialize the Node

Pseudocode

grpcurl Test

Command Line

Automated Script

🧠 Node Setup Logic

Copied!

![Copy](https://tari.com/_next/static/media/copy.32ozctvwtsgtm.svg)

`FUNCTION setupTariNode(): // Step 1: Check system requirements IF (disk_space < 50GB OR memory < 4GB): THROW "Insufficient system resources" // Step 2: Initialize node configuration IF (config_file_exists): LOAD existing_config ELSE: CREATE default_config SET network = "mainnet" // or "testnet" for testing SET grpc_enabled = true SET grpc_port = 18142 SET p2p_port = 18141 // Step 3: Generate node identity IF (NOT node_identity_exists): GENERATE new_keypair SAVE keypair_to_secure_location LOG "Node identity created: " + public_key // Step 4: Configure network transport IF (tor_available): SET transport = "tor" ELSE: SET transport = "tcp" CONFIGURE public_ip_address // Step 5: Start synchronization START node_process WAIT_FOR initial_sync RETURN node_status`

## 💼 4. Wallet Creation & Management

**💡 Concept:**

Your exchange needs two types of wallets: a read-only monitoring wallet for tracking deposits, and a secure cold storage wallet for processing withdrawals.

![Wallet Architecture](https://tari.com/_next/static/media/architecture.0_v5__qiz_h11.svg)

#### 

1

Create Monitoring Wallet

Pseudocode

grpcurl Test

Setup Commands

🧠 Wallet Creation Logic

Copied!

![Copy](https://tari.com/_next/static/media/copy.32ozctvwtsgtm.svg)

`FUNCTION createMonitoringWallet(): // Step 1: Initialize wallet with secure passphrase passphrase = generateSecurePassphrase(256) // 256-bit entropy // Step 2: Create wallet identity TRY: walletIdentity = tariWallet.createIdentity(passphrase) // Step 3: Configure for monitoring only walletConfig = { name: "exchange-monitoring", grpc_enabled: true, grpc_address: "127.0.0.1:18143", grpc_authentication: { username: "exchange_monitor", password: generateSecurePassword() }, base_node_service_peers: [trusted_node_address], network: "mainnet", // or "testnet" spending_key_access: false // Read-only } // Step 4: Generate deposit address depositAddress = walletIdentity.getOneSidedAddress() // Step 5: Store securely secureStorage.store({ wallet_id: walletIdentity.public_key, deposit_address: depositAddress, grpc_credentials: walletConfig.grpc_authentication, created_at: getCurrentTimestamp() }) // Step 6: Start monitoring service startWalletSync() RETURN { address: depositAddress, wallet_id: walletIdentity.public_key, status: "ready_for_deposits" } CATCH (error): LOG "Wallet creation failed: " + error.message THROW "Failed to create monitoring wallet"`

#### 

2

Cold Storage Wallet Setup

**🔒 Security Critical:**

The cold storage wallet should be on an air-gapped system or secure hardware wallet for maximum security.

Pseudocode

Setup Process

Security Measures

🧠 Cold Storage Setup Logic

Copied!

![Copy](https://tari.com/_next/static/media/copy.32ozctvwtsgtm.svg)

`FUNCTION setupColdStorageWallet(): // Step 1: Air-gapped environment validation IF (networkConnected() OR usbDevicesConnected()): THROW "Environment not sufficiently isolated" // Step 2: Generate seed phrase securely seedPhrase = generateMnemonic(24) // 24-word BIP39 mnemonic validateMnemonic(seedPhrase) // Step 3: Create physical backup printSeedPhrase(seedPhrase) // On paper, never digital createMetalBackup(seedPhrase) // Fire/water resistant // Step 4: Initialize wallet from seed coldWallet = createWalletFromSeed(seedPhrase) // Step 5: Generate multiple addresses for rotation addresses = [] FOR i = 0 TO 9: address = coldWallet.deriveAddress(i) addresses.append(address) // Step 6: Export public keys only for monitoring publicKeys = [] FOR EACH address IN addresses: publicKeys.append(address.getPublicKey()) // Step 7: Secure the seed securelyDestroySeedFromMemory() storeInSecureSafe(seedPhrasePaper) // Step 8: Create watch-only wallet for hot system watchOnlyWallet = createWatchOnlyWallet(publicKeys) RETURN { watch_only_keys: publicKeys, addresses: addresses, backup_completed: true }`

## 📥 5. Deposit Monitoring

**💡 Concept:**

Users send funds to your exchange's one-sided address with a payment ID. Your system monitors for these transactions and credits user accounts.

![Deposit Flow](https://tari.com/_next/static/media/depositflow.0ti4e3mnf6kll.svg)

#### 

1

Generate Deposit Address

Pseudocode

grpcurl

Node.js

Python

Rust

PHP

🧠 Deposit Address Generation Logic

Copied!

![Copy](https://tari.com/_next/static/media/copy.32ozctvwtsgtm.svg)

`FUNCTION generateDepositAddress(userId): // Step 1: Input validation IF (userId IS empty OR userId IS invalid): THROW "Invalid user ID" // Step 2: Generate unique payment identifier timestamp = getCurrentTimestamp() randomBytes = generateSecureRandom(8) // 8 bytes = 64 bits paymentId = "deposit-" + userId + "-" + timestamp + "-" + randomBytes // Step 3: Convert payment ID to bytes for gRPC paymentIdBytes = convertToBytes(paymentId, "UTF-8") // Step 4: Call wallet gRPC to get address with payment ID request = { payment_id: paymentIdBytes } TRY: response = walletClient.GetPaymentIdAddress(request) // Step 5: Extract address information address = response.one_sided_address_base58 emojiAddress = response.one_sided_address_emoji // Step 6: Store in database for tracking depositRecord = { userId: userId, paymentId: paymentId, address: address, status: "pending", createdAt: timestamp } database.save(depositRecord) // Step 7: Return deposit instructions to user RETURN { address: address, paymentId: paymentId, emojiAddress: emojiAddress, instructions: "Send XTM to this address with payment ID: " + paymentId, qrCode: generateQRCode(address, paymentId) } CATCH (grpcError): LOG "Failed to generate address: " + grpcError.message THROW "Address generation failed"`

#### 

2

Monitor for Incoming Transactions

Pseudocode

grpcurl

Node.js

Python

Rust

PHP

🧠 Transaction Monitoring Logic

Copied!

![Copy](https://tari.com/_next/static/media/copy.32ozctvwtsgtm.svg)

`FUNCTION monitorIncomingTransactions(): processedTransactions = loadProcessedTransactionsList() WHILE (service_is_running): TRY: // Method 1: Real-time streaming (preferred) IF (streaming_available): transactionStream = walletClient.StreamTransactionEvents() FOR EACH event IN transactionStream: IF (event.type == "Mined" AND event.direction == "Inbound"): processTransactionEvent(event) // Method 2: Polling (backup) ELSE: completedTransactions = walletClient.GetCompletedTransactions() FOR EACH transaction IN completedTransactions: IF (shouldProcessTransaction(transaction)): processTransaction(transaction) CATCH (connection_error): LOG "Connection lost, retrying in 5 seconds..." SLEEP(5) CONTINUE SLEEP(30) // Poll every 30 seconds if streaming not available FUNCTION shouldProcessTransaction(transaction): RETURN ( transaction.direction == "INBOUND" AND transaction.status == "CONFIRMED" AND NOT isAlreadyProcessed(transaction.tx_id) ) FUNCTION processTransaction(transaction): // Step 1: Extract and validate payment ID paymentId = extractPaymentIdFromBytes(transaction.payment_id) IF (paymentId IS empty): LOG "Transaction without payment ID: " + transaction.tx_id RETURN // Step 2: Look up deposit record depositRecord = database.findByPaymentId(paymentId) IF (depositRecord IS null): LOG "Unknown payment ID: " + paymentId RETURN // Step 3: Validate transaction amount amountXTM = transaction.amount / 1_000_000 // Convert microXTM to XTM IF (amountXTM < MINIMUM_DEPOSIT_AMOUNT): LOG "Deposit amount too small: " + amountXTM + " XTM" RETURN // Step 4: Credit user account TRY: accountBalance = getUserBalance(depositRecord.userId) newBalance = accountBalance + amountXTM // Atomic transaction BEGIN_DATABASE_TRANSACTION() updateUserBalance(depositRecord.userId, newBalance) markDepositAsProcessed(depositRecord.id, transaction.tx_id) createDepositHistoryRecord(depositRecord.userId, amountXTM, transaction.tx_id) COMMIT_DATABASE_TRANSACTION() // Step 5: Notify user sendDepositNotification(depositRecord.userId, amountXTM, transaction.tx_id) LOG "Processed deposit: " + amountXTM + " XTM for user " + depositRecord.userId CATCH (database_error): ROLLBACK_DATABASE_TRANSACTION() LOG "Failed to process deposit: " + database_error.message // Add to retry queue // Step 6: Mark as processed to avoid double-processing processedTransactions.add(transaction.tx_id)`

## 💸 6. Withdrawal Processing

**🔐 Security Alert:**

Withdrawal processing involves the cold storage wallet. Implement proper authorization, rate limiting, and multi-signature approval processes.

![Withdrawal Flow](https://tari.com/_next/static/media/withdrawalflow.0lm6a3m4_nnwg.svg)

#### 

1

Validate Withdrawal Request

Pseudocode

grpcurl

Node.js

Python

Rust

PHP

🧠 Withdrawal Validation Logic

Copied!

![Copy](https://tari.com/_next/static/media/copy.32ozctvwtsgtm.svg)

`FUNCTION validateWithdrawalRequest(userId, destinationAddress, amount): // Step 1: Validate user authentication and KYC status user = database.getUser(userId) IF (user IS null): THROW "User not found" IF (NOT user.isKYCVerified): THROW "User not KYC verified" IF (user.isBlocked OR user.withdrawalsSuspended): THROW "User account restricted" // Step 2: Validate withdrawal amount IF (amount <= 0): THROW "Invalid withdrawal amount" minimumWithdrawal = getMinimumWithdrawalAmount() IF (amount < minimumWithdrawal): THROW "Amount below minimum withdrawal: " + minimumWithdrawal + " XTM" // Step 3: Check user balance (including fees) withdrawalFee = calculateWithdrawalFee(amount) totalRequired = amount + withdrawalFee IF (user.availableBalance < totalRequired): THROW "Insufficient balance. Required: " + totalRequired + " XTM, Available: " + user.availableBalance + " XTM" // Step 4: Validate destination address IF (NOT isValidTariAddress(destinationAddress)): THROW "Invalid Tari address format" // Step 5: Check if address is interactive (not allowed for exchanges) IF (isInteractiveAddress(destinationAddress)): THROW "Interactive addresses not supported for withdrawals" // Step 6: Security checks IF (isBlacklistedAddress(destinationAddress)): THROW "Destination address is blacklisted" // Step 7: Rate limiting and daily limits dailyWithdrawalLimit = getDailyWithdrawalLimit(user.kycLevel) todayWithdrawals = getTodayWithdrawals(userId) IF (todayWithdrawals + amount > dailyWithdrawalLimit): THROW "Daily withdrawal limit exceeded" // Step 8: Check for suspicious activity IF (detectSuspiciousActivity(userId, amount, destinationAddress)): THROW "Withdrawal flagged for manual review" // Step 9: Create withdrawal record withdrawalId = generateUniqueId() withdrawalRecord = { id: withdrawalId, userId: userId, destinationAddress: destinationAddress, amount: amount, fee: withdrawalFee, status: "pending_approval", createdAt: getCurrentTimestamp(), approvalRequired: (amount > getManualApprovalThreshold()) } database.saveWithdrawal(withdrawalRecord) // Step 10: Reserve user balance database.reserveUserBalance(userId, totalRequired) RETURN { withdrawalId: withdrawalId, amount: amount, fee: withdrawalFee, estimatedProcessingTime: "15-30 minutes", approvalRequired: withdrawalRecord.approvalRequired }`

#### 

2

Process Approved Withdrawals

**🔒 Cold Storage Operation:**

This step requires access to the cold storage wallet. Ensure proper security protocols are followed.

Pseudocode

grpcurl

Node.js

Python

Rust

PHP

🧠 Withdrawal Processing Logic

Copied!

![Copy](https://tari.com/_next/static/media/copy.32ozctvwtsgtm.svg)

`FUNCTION processApprovedWithdrawal(withdrawalRecord): // Step 1: Final validation checks IF (withdrawalRecord.status != "approved"): THROW "Withdrawal not approved for processing" IF (isWithdrawalExpired(withdrawalRecord)): markWithdrawalAsExpired(withdrawalRecord.id) THROW "Withdrawal request has expired" // Step 2: Connect to cold storage wallet TRY: coldWallet = connectToColdWallet() IF (NOT coldWallet.isOnline()): THROW "Cold wallet not accessible" CATCH (connection_error): THROW "Failed to connect to cold wallet: " + connection_error.message // Step 3: Sync wallet and verify balance TRY: coldWallet.sync() currentBalance = coldWallet.getBalance() totalRequired = withdrawalRecord.amount + withdrawalRecord.fee IF (currentBalance.available < totalRequired): notifyAdminsLowBalance(currentBalance.available, totalRequired) THROW "Insufficient cold wallet balance" CATCH (sync_error): THROW "Wallet sync failed: " + sync_error.message // Step 4: Create and validate payment recipient recipient = { address: withdrawalRecord.destinationAddress, amount: withdrawalRecord.amount, fee_per_gram: 25, // Standard fee rate payment_type: "ONE_SIDED", // Always use one-sided for exchanges payment_id: withdrawalRecord.id // Use withdrawal ID as payment ID } // Step 5: Final security checks IF (isAddressCompromised(recipient.address)): THROW "Destination address flagged as compromised" // Step 6: Execute the transaction TRY: // Update status to processing updateWithdrawalStatus(withdrawalRecord.id, "processing") // Send the transaction transactionRequest = { recipients: [recipient] } response = coldWallet.transfer(transactionRequest) // Step 7: Validate transaction response IF (response.results.length == 0): THROW "No transaction result received" result = response.results[0] IF (NOT result.is_success): THROW "Transaction failed: " + result.failure_message transactionId = result.transaction_id // Step 8: Get transaction details for verification transactionDetails = coldWallet.getTransactionInfo([transactionId]) IF (transactionDetails.length == 0): THROW "Could not retrieve transaction details" txInfo = transactionDetails[0] // Step 9: Update database records BEGIN_DATABASE_TRANSACTION() updateWithdrawalStatus(withdrawalRecord.id, "completed") updateWithdrawalTransactionInfo(withdrawalRecord.id, { transactionId: transactionId, transactionHash: txInfo.excess_sig, broadcastTime: getCurrentTimestamp(), fee: txInfo.fee }) // Deduct from user's reserved balance deductReservedBalance(withdrawalRecord.userId, totalRequired) // Create audit log entry createAuditLog({ action: "withdrawal_processed", userId: withdrawalRecord.userId, amount: withdrawalRecord.amount, transactionId: transactionId, destinationAddress: withdrawalRecord.destinationAddress, processedBy: getCurrentOperator(), timestamp: getCurrentTimestamp() }) COMMIT_DATABASE_TRANSACTION() // Step 10: Notifications notifyUserWithdrawalCompleted(withdrawalRecord.userId, { amount: withdrawalRecord.amount, transactionId: transactionId, estimatedConfirmationTime: "10-15 minutes" }) // Step 11: Monitoring setup addTransactionToMonitoring(transactionId, withdrawalRecord.id) LOG "Successfully processed withdrawal: " + withdrawalRecord.id + " for user " + withdrawalRecord.userId + " amount " + (withdrawalRecord.amount / 1_000_000) + " XTM" RETURN { success: true, transactionId: transactionId, transactionHash: txInfo.excess_sig, estimatedConfirmationTime: "10-15 minutes" } CATCH (transaction_error): // Rollback database changes ROLLBACK_DATABASE_TRANSACTION() // Update withdrawal status to failed updateWithdrawalStatus(withdrawalRecord.id, "failed") addWithdrawalErrorLog(withdrawalRecord.id, transaction_error.message) // Notify admins of failure notifyAdminsWithdrawalFailed(withdrawalRecord, transaction_error.message) THROW "Transaction processing failed: " + transaction_error.message FINALLY: // Always disconnect from cold wallet for security IF (coldWallet): coldWallet.disconnect()`

## 🔐 7. Security Best Practices

**⚠️ Critical:**

Cryptocurrency exchange security requires defense in depth. Multiple layers of security are essential to protect user funds.

### 🛡️ Multi-Layer Security Architecture

Security Architecture

Access Control

Security Monitoring

Incident Response

🧠 Security Implementation Logic

Copied!

![Copy](https://tari.com/_next/static/media/copy.32ozctvwtsgtm.svg)

`FUNCTION implementSecurityMeasures(): // Layer 1: Network Security configureFirewall([ "ALLOW 18141/tcp FROM trusted_peers", "ALLOW 18142/tcp FROM localhost ONLY", "ALLOW 18143/tcp FROM localhost ONLY", "DENY ALL other traffic" ]) // Layer 2: Authentication & Authorization implementMFA({ admin_accounts: "hardware_tokens_required", operator_accounts: "totp_required", api_access: "jwt_with_refresh_rotation" }) // Layer 3: Wallet Security separateWallets({ hot_wallet: { balance_limit: "1% of total funds", withdrawal_limit: "daily_maximum", monitoring: "real_time" }, cold_storage: { air_gapped: true, multi_signature: "3_of_5_scheme", hardware_security_module: true } }) // Layer 4: Data Protection encryptSensitiveData({ keys: "AES-256-GCM", database: "TDE_enabled", backups: "encrypted_at_rest", transmission: "TLS_1.3_minimum" }) // Layer 5: Monitoring & Alerting implementMonitoring({ failed_logins: "immediate_alert_after_3", large_withdrawals: "manual_approval_required", unusual_patterns: "ML_based_detection", system_health: "24_7_monitoring" }) RETURN security_status`

## 🚀 8. Production Deployment

**📋 Production Ready:**

Deploying Tari in production requires careful planning, monitoring, and redundancy. This section covers Docker, Kubernetes, and monitoring setups.

### 🐳 Docker Deployment

Docker Compose

Dockerfile

Kubernetes

Copied!

![Copy](https://tari.com/_next/static/media/copy.32ozctvwtsgtm.svg)

`version: '3.8' services: tari-node: # WARNING: Use specific version tags in production, not 'latest' image: tariproject/minotari_node:latest container_name: tari-node restart: unless-stopped ports: - "18141:18141" # P2P port - "127.0.0.1:18142:18142" # gRPC port (localhost only) volumes: - tari-node-data:/root/.tari - ./config/node-config.toml:/root/.tari/mainnet/config/config.toml:ro environment: - TARI_NETWORK=mainnet - RUST_LOG=info networks: - tari-network tari-wallet: image: tariproject/minotari_console_wallet:latest container_name: tari-wallet restart: unless-stopped ports: - "127.0.0.1:18143:18143" # gRPC port (localhost only) volumes: - tari-wallet-data:/root/.tari - ./config/wallet-config.toml:/root/.tari/mainnet/config/config.toml:ro environment: - TARI_NETWORK=mainnet - TARI_WALLET__CUSTOM_BASE_NODE=tari-node:18142 depends_on: - tari-node networks: - tari-network redis: image: redis:7-alpine container_name: tari-redis restart: unless-stopped volumes: - redis-data:/data networks: - tari-network postgres: image: postgres:15 container_name: tari-postgres restart: unless-stopped environment: - POSTGRES_DB=tari_exchange - POSTGRES_USER=tari_user - POSTGRES_PASSWORD=${DB_PASSWORD} volumes: - postgres-data:/var/lib/postgresql/data - ./sql/init.sql:/docker-entrypoint-initdb.d/init.sql networks: - tari-network exchange-api: build: . container_name: tari-exchange-api restart: unless-stopped ports: - "3000:3000" environment: - NODE_ENV=production - DATABASE_URL=postgresql://tari_user:${DB_PASSWORD}@postgres:5432/tari_exchange - REDIS_URL=redis://redis:6379 - TARI_WALLET_GRPC=http://tari-wallet:18143 - TARI_NODE_GRPC=http://tari-node:18142 depends_on: - postgres - redis - tari-wallet networks: - tari-network volumes: tari-node-data: tari-wallet-data: redis-data: postgres-data: networks: tari-network: driver: bridge`

## 🔧 9. Troubleshooting

**🔧 Common Issues:**

This section covers the most frequently encountered problems and their solutions when integrating Tari into exchanges.

### 🐛 Common Issues and Solutions

Sync Issues

Connectivity

Transactions

Performance

🧪 Sync Troubleshooting with grpcurl

Copied!

![Copy](https://tari.com/_next/static/media/copy.32ozctvwtsgtm.svg)

`# Check node sync status grpcurl -plaintext localhost:18142 tari.rpc.BaseNode/GetSyncInfo # Check tip info grpcurl -plaintext localhost:18142 tari.rpc.BaseNode/GetTipInfo # Check connected peers grpcurl -plaintext localhost:18142 tari.rpc.BaseNode/ListConnectedPeers # Complete sync diagnostic script #!/bin/bash echo "=== Sync Diagnostic Script ===" echo "1. Base Node Sync Status:" SYNC_INFO=$(grpcurl -plaintext localhost:18142 tari.rpc.BaseNode/GetSyncInfo 2>/dev/null) if [ $? -eq 0 ]; then echo "$SYNC_INFO" | jq . else echo "❌ Cannot connect to base node" fi echo -e " 2. Peer Connections:" PEERS=$(grpcurl -plaintext localhost:18142 tari.rpc.BaseNode/ListConnectedPeers 2>/dev/null) PEER_COUNT=$(echo "$PEERS" | jq '.connected_peers | length') echo "Connected peers: $PEER_COUNT" if [ "$PEER_COUNT" -lt 3 ]; then echo "⚠️ Low peer count, checking network..." # Add more peers manually if needed fi echo -e " 3. Wallet Sync Status:" WALLET_STATE=$(grpcurl -plaintext localhost:18143 tari.rpc.Wallet/GetState 2>/dev/null) if [ $? -eq 0 ]; then SCANNED_HEIGHT=$(echo "$WALLET_STATE" | jq -r '.scanned_height') echo "Wallet scanned height: $SCANNED_HEIGHT" else echo "❌ Cannot connect to wallet" fi # Common fixes for sync issues: echo -e " 🔧 Common Sync Fixes:" echo "1. Restart services: docker-compose restart" echo "2. Clear peer ban list: rm ~/.tari/mainnet/peer_db/banned_peers" echo "3. Manual peer addition: Use SetBaseNode RPC call"`

## 📚 10. Complete API Reference

**📋 Complete Reference:**

All gRPC methods available for Tari wallet and base node integration with pseudocode and grpcurl examples.

### 💼 Wallet API Methods

Core Methods

Transaction Methods

Advanced Methods

🧠 Core Wallet Operations Logic

Copied!

![Copy](https://tari.com/_next/static/media/copy.32ozctvwtsgtm.svg)

`// Core wallet operations provide basic wallet information and state FUNCTION getWalletVersion(): // Simple version check - no parameters needed RETURN walletClient.GetVersion() FUNCTION getWalletState(): // Returns comprehensive wallet status including: // - Blockchain sync height // - Balance information // - Network connectivity status RETURN walletClient.GetState() FUNCTION checkWalletConnectivity(): // Quick connectivity check without full state RETURN walletClient.CheckConnectivity() FUNCTION getWalletIdentity(): // Returns wallet's public key and addresses RETURN walletClient.Identify() FUNCTION getWalletAddresses(): // Get basic address formats (binary) RETURN walletClient.GetAddress() FUNCTION getCompleteAddresses(): // Get all address formats (binary, base58, emoji) RETURN walletClient.GetCompleteAddress() FUNCTION generateAddressWithPaymentId(paymentId): // Generate address tied to specific payment ID paymentIdBytes = convertToBytes(paymentId) RETURN walletClient.GetPaymentIdAddress(paymentIdBytes)`

🧪 Core Wallet Methods with grpcurl

Copied!

![Copy](https://tari.com/_next/static/media/copy.32ozctvwtsgtm.svg)

`# 1. Get Wallet Version grpcurl -plaintext localhost:18143 tari.rpc.Wallet/GetVersion # 2. Get Wallet State (comprehensive status) grpcurl -plaintext localhost:18143 tari.rpc.Wallet/GetState # 3. Quick Connectivity Check grpcurl -plaintext localhost:18143 tari.rpc.Wallet/CheckConnectivity # 4. Get Wallet Identity grpcurl -plaintext localhost:18143 tari.rpc.Wallet/Identify # 5. Get Basic Addresses grpcurl -plaintext localhost:18143 tari.rpc.Wallet/GetAddress # 6. Get Complete Address Information grpcurl -plaintext localhost:18143 tari.rpc.Wallet/GetCompleteAddress # 7. Generate Address with Payment ID PAYMENT_ID="deposit-user123-$(date +%s)" PAYMENT_ID_B64=$(echo -n "$PAYMENT_ID" | base64 -w 0) grpcurl -plaintext -d "{"payment_id": "$PAYMENT_ID_B64"}" localhost:18143 tari.rpc.Wallet/GetPaymentIdAddress # Complete wallet status check script #!/bin/bash echo "=== Wallet Status Check ===" echo "1. Version:" grpcurl -plaintext localhost:18143 tari.rpc.Wallet/GetVersion echo -e " 2. Connectivity:" grpcurl -plaintext localhost:18143 tari.rpc.Wallet/CheckConnectivity echo -e " 3. State:" grpcurl -plaintext localhost:18143 tari.rpc.Wallet/GetState echo -e " 4. Identity:" grpcurl -plaintext localhost:18143 tari.rpc.Wallet/Identify`

### ⛓️ Base Node API Methods

Node Information

Blockchain Data

Network Methods

🧠 Node Information Logic

Copied!

![Copy](https://tari.com/_next/static/media/copy.32ozctvwtsgtm.svg)

`// Base node information provides blockchain state and node status FUNCTION getNodeVersion(): // Get base node software version RETURN baseNodeClient.GetVersion() FUNCTION getNodeIdentity(): // Get node's public key, addresses, and network identity RETURN baseNodeClient.Identify() FUNCTION getTipInfo(): // Get current blockchain tip information including: // - Best block height and hash // - Accumulated difficulty // - Sync status RETURN baseNodeClient.GetTipInfo() FUNCTION getSyncInfo(): // Get detailed synchronization status RETURN baseNodeClient.GetSyncInfo() FUNCTION getSyncProgress(): // Get sync progress with state information RETURN baseNodeClient.GetSyncProgress() FUNCTION getConsensusConstants(blockHeight): // Get network consensus rules for specific block height request = {block_height: blockHeight} RETURN baseNodeClient.GetConstants(request) FUNCTION checkForUpdates(): // Check if newer software version is available RETURN baseNodeClient.CheckForUpdates() FUNCTION getNetworkState(): // Get comprehensive network state including peers and difficulty RETURN baseNodeClient.GetNetworkState()`

🧪 Node Information with grpcurl

Copied!

![Copy](https://tari.com/_next/static/media/copy.32ozctvwtsgtm.svg)

`# 1. Get Node Version grpcurl -plaintext localhost:18142 tari.rpc.BaseNode/GetVersion # 2. Get Node Identity grpcurl -plaintext localhost:18142 tari.rpc.BaseNode/Identify # 3. Get Tip Information (current blockchain state) grpcurl -plaintext localhost:18142 tari.rpc.BaseNode/GetTipInfo # 4. Get Sync Information grpcurl -plaintext localhost:18142 tari.rpc.BaseNode/GetSyncInfo # 5. Get Sync Progress Details grpcurl -plaintext localhost:18142 tari.rpc.BaseNode/GetSyncProgress # 6. Get Consensus Constants for Current Tip TIP_HEIGHT=$(grpcurl -plaintext localhost:18142 tari.rpc.BaseNode/GetTipInfo | jq -r '.metadata.best_block_height // 0') grpcurl -plaintext -d "{"block_height": $TIP_HEIGHT}" localhost:18142 tari.rpc.BaseNode/GetConstants # 7. Check for Software Updates grpcurl -plaintext localhost:18142 tari.rpc.BaseNode/CheckForUpdates # 8. Get Network State grpcurl -plaintext localhost:18142 tari.rpc.BaseNode/GetNetworkState # Complete node status script #!/bin/bash echo "=== Base Node Status Check ===" echo "1. Node Version:" VERSION=$(grpcurl -plaintext localhost:18142 tari.rpc.BaseNode/GetVersion 2>/dev/null | jq -r '.value // "Unknown"') echo "Version: $VERSION" echo -e " 2. Node Identity:" grpcurl -plaintext localhost:18142 tari.rpc.BaseNode/Identify | jq . echo -e " 3. Blockchain Status:" TIP_INFO=$(grpcurl -plaintext localhost:18142 tari.rpc.BaseNode/GetTipInfo 2>/dev/null) if [ $? -eq 0 ]; then HEIGHT=$(echo "$TIP_INFO" | jq -r '.metadata.best_block_height // 0') HASH=$(echo "$TIP_INFO" | jq -r '.metadata.best_block_hash // "unknown"') SYNC_STATUS=$(echo "$TIP_INFO" | jq -r '.initial_sync_achieved // false') echo "Height: $HEIGHT" echo "Hash: ${HASH:0:16}..." echo "Synced: $SYNC_STATUS" else echo "❌ Could not get tip info" fi echo -e " 4. Sync Progress:" grpcurl -plaintext localhost:18142 tari.rpc.BaseNode/GetSyncProgress | jq . echo -e " 5. Software Updates:" UPDATE_INFO=$(grpcurl -plaintext localhost:18142 tari.rpc.BaseNode/CheckForUpdates 2>/dev/null) HAS_UPDATE=$(echo "$UPDATE_INFO" | jq -r '.has_update // false') if [ "$HAS_UPDATE" = "true" ]; then NEW_VERSION=$(echo "$UPDATE_INFO" | jq -r '.version // "unknown"') echo "⚠️ Update available: $NEW_VERSION" else echo "✅ Software up to date" fi`

### 🎉 Congratulations!

You've completed the comprehensive Tari exchange integration guide with pseudocode explanations and grpcurl testing examples. You now have all the tools and knowledge needed to successfully integrate Minotari (XTM) into your cryptocurrency exchange with a deep understanding of each operation.

### 📞 Support and Resources

### Documentation

![arrow](https://tari.com/_next/static/media/arrow.2wt19q8fu-hu7.svg)

[https://rfc.tari.com](https://rfc.tari.com)

### GitHub Repository

![arrow](https://tari.com/_next/static/media/arrow.2wt19q8fu-hu7.svg)

[https://github.com/tari-project/tari](https://github.com/tari-project/tari)

### Discord Community

![arrow](https://tari.com/_next/static/media/arrow.2wt19q8fu-hu7.svg)

[https://discord.gg/tari](https://discord.gg/tari)

### Downloads

![arrow](https://tari.com/_next/static/media/arrow.2wt19q8fu-hu7.svg)

[https://tari.com/downloads/](https://tari.com/downloads/)

### grpcurl Documentation

![arrow](https://tari.com/_next/static/media/arrow.2wt19q8fu-hu7.svg)

[https://github.com/fullstorydev/grpcurl](https://github.com/fullstorydev/grpcurl)

### 💡 Next Steps:

- Test all pseudocode logic before implementing
- Use grpcurl examples to validate your gRPC setup
- Test the integration thoroughly on testnet
- Implement comprehensive monitoring and alerting
- Set up proper backup and disaster recovery procedures
- Conduct security audits before going live
- Join the Tari community for ongoing support