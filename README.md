# QuantumResistant

A cross-chain AMM liquidity pool with quantum-resistant Bitcoin security implemented on the Stacks blockchain using Clarity smart contracts.

## Description

QuantumResistant is a decentralized exchange protocol that implements quantum-resistant cryptographic security measures for cross-chain Bitcoin and STX trading pairs. The protocol provides automated market making (AMM) functionality with enhanced security features designed to withstand future quantum computing threats.

## Features

- **Quantum-Resistant Security**: Advanced cryptographic protection using double hashing and quantum-resistant signature verification
- **Cross-Chain Support**: Enables secure trading between Bitcoin and Stacks-based tokens
- **AMM Liquidity Pools**: Automated market making with constant product formula
- **Liquidity Mining**: Pool token rewards for liquidity providers
- **Slippage Protection**: Built-in minimum output guarantees for trades
- **Emergency Controls**: Contract pause/resume functionality for security
- **Trading Fees**: 0.3% trading fee distributed to liquidity providers
- **Cross-Chain Verification**: Multi-confirmation system for cross-chain transactions

## Technical Specifications

- **Blockchain**: Stacks
- **Language**: Clarity
- **Contract Version**: 1.0.0
- **Clarity Version**: 2
- **Epoch**: 2.5
- **Trading Fee**: 0.3% (30 basis points)
- **Quantum Hash Iterations**: 10,000
- **Minimum Cross-Chain Confirmations**: 6

### Security Parameters

- **Quantum Nonce**: Dynamic nonce generation for quantum resistance
- **Double Hashing**: SHA-256 applied twice for enhanced security
- **Signature Limits**: Maximum 1,000 signatures per address to prevent replay attacks
- **Block-based Verification**: Time-locked signature verification

## Installation

### Prerequisites

- Node.js (v16 or higher)
- Clarinet CLI
- Stacks CLI (optional)

### Setup

1. Clone the repository:
```bash
git clone <repository-url>
cd QuantumResistant
```

2. Navigate to the contract directory:
```bash
cd QuantumResistant_contract
```

3. Install dependencies:
```bash
npm install
```

4. Run tests:
```bash
npm test
```

5. Check contract syntax:
```bash
clarinet check
```

## Usage Examples

### Creating a Liquidity Pool

```clarity
;; Create a new trading pair pool
(contract-call? .QuantumResistant create-pool
  'SP000000000000000000002Q6VF78.token-a
  'SP000000000000000000002Q6VF78.token-b
  u1000000  ;; 1,000,000 token-a
  u500000   ;; 500,000 token-b
)
```

### Adding Liquidity

```clarity
;; Add liquidity to an existing pool
(contract-call? .QuantumResistant add-liquidity
  'SP000000000000000000002Q6VF78.token-a
  'SP000000000000000000002Q6VF78.token-b
  u100000   ;; Amount of token-a
  u50000    ;; Amount of token-b
  u1000     ;; Minimum shares expected
)
```

### Swapping Tokens

```clarity
;; Swap token-a for token-b
(contract-call? .QuantumResistant swap-tokens
  'SP000000000000000000002Q6VF78.token-a  ;; Input token
  'SP000000000000000000002Q6VF78.token-b  ;; Output token
  u10000    ;; Input amount
  u4900     ;; Minimum output amount (slippage protection)
)
```

### Removing Liquidity

```clarity
;; Remove liquidity from pool
(contract-call? .QuantumResistant remove-liquidity
  'SP000000000000000000002Q6VF78.token-a
  'SP000000000000000000002Q6VF78.token-b
  u500      ;; Shares to remove
  u45000    ;; Minimum token-a to receive
  u22500    ;; Minimum token-b to receive
)
```

## Contract Functions Documentation

### Public Functions

#### `create-pool`
Creates a new trading pair pool with initial liquidity.

**Parameters:**
- `token-a`: Principal of the first token
- `token-b`: Principal of the second token
- `amount-a`: Initial amount of token-a
- `amount-b`: Initial amount of token-b

**Returns:** `(response uint uint)` - Initial liquidity shares minted

#### `add-liquidity`
Adds liquidity to an existing pool proportionally.

**Parameters:**
- `token-a`: Principal of the first token
- `token-b`: Principal of the second token
- `amount-a`: Amount of token-a to add
- `amount-b`: Amount of token-b to add
- `min-shares`: Minimum shares expected (slippage protection)

**Returns:** `(response {shares: uint, amount-a: uint, amount-b: uint} uint)`

#### `swap-tokens`
Exchanges one token for another using the AMM formula.

**Parameters:**
- `token-in`: Principal of input token
- `token-out`: Principal of output token
- `amount-in`: Amount of input tokens
- `min-amount-out`: Minimum output expected

**Returns:** `(response uint uint)` - Actual amount received

#### `remove-liquidity`
Removes liquidity from a pool and returns underlying tokens.

**Parameters:**
- `token-a`: Principal of the first token
- `token-b`: Principal of the second token
- `shares`: Number of pool shares to burn
- `min-amount-a`: Minimum token-a to receive
- `min-amount-b`: Minimum token-b to receive

**Returns:** `(response {amount-a: uint, amount-b: uint} uint)`

#### `verify-cross-chain-tx`
Verifies cross-chain transactions with quantum-resistant signatures.

**Parameters:**
- `tx-hash`: Transaction hash buffer
- `from-chain`: Source blockchain identifier
- `to-chain`: Destination blockchain identifier
- `amount`: Transaction amount
- `quantum-signature`: Quantum-resistant signature

**Returns:** `(response bool uint)` - Verification status

### Read-Only Functions

#### `get-pool-info`
Returns complete pool information including reserves and shares.

#### `get-user-shares`
Returns liquidity shares owned by a specific user in a pool.

#### `get-swap-amount-out`
Calculates expected output amount for a given input (including fees).

#### `get-cross-chain-tx-status`
Returns status and confirmation count for cross-chain transactions.

#### `is-contract-paused`
Returns current pause status of the contract.

#### `get-total-liquidity`
Returns total liquidity across all pools.

## Deployment Guide

### Local Development

1. Start Clarinet console:
```bash
clarinet console
```

2. Deploy contract in console:
```clarity
(contract-call? .QuantumResistant create-pool ...)
```

### Testnet Deployment

1. Configure testnet settings in `settings/Testnet.toml`

2. Deploy using Clarinet:
```bash
clarinet deployments generate --testnet
clarinet deployments apply --testnet
```

### Mainnet Deployment

1. Configure mainnet settings in `settings/Mainnet.toml`

2. Deploy to mainnet:
```bash
clarinet deployments generate --mainnet
clarinet deployments apply --mainnet
```

## Security Notes

### Quantum Resistance
- Double SHA-256 hashing provides protection against quantum attacks
- Dynamic nonce generation prevents rainbow table attacks
- Signature replay protection with usage limits

### Cross-Chain Security
- 6-block confirmation requirement for cross-chain transactions
- Quantum-resistant signature verification for bridge operations
- Transaction hash tracking prevents double-spending

### Access Controls
- Contract owner can pause/resume operations in emergencies
- Pool creation is permissionless but requires initial liquidity
- Slippage protection on all trading operations

### Known Limitations
- Quantum signature verification is simplified for demonstration
- Production deployment should integrate proper post-quantum cryptography
- Cross-chain bridge implementation requires additional infrastructure

## Testing

Run the complete test suite:
```bash
npm test
```

Run tests with coverage and cost analysis:
```bash
npm run test:report
```

Watch mode for development:
```bash
npm run test:watch
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Add tests for new functionality
4. Ensure all tests pass
5. Submit a pull request

## License

This project is licensed under the ISC License.

## Contact

For questions, issues, or contributions, please open an issue on the project repository.