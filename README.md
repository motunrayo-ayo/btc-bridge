# Bitcoin-Stacks Bridge Protocol

A high-performance smart contract enabling secure cross-chain asset transfers between Bitcoin and Stacks networks. This enterprise-grade bridge implements atomic swaps with multi-validator consensus, automated confirmations, and comprehensive security measures.

## Features

- **Multi-Validator Consensus**: Secure transaction validation through distributed validator network
- **Automated Confirmations**: Required confirmations (6) for transaction finality
- **Emergency Controls**: Pause mechanism and emergency withdrawal capabilities
- **Comprehensive Validation**: Extensive checks for transactions, addresses, and signatures
- **Balance Management**: Precise tracking of bridged assets and user balances
- **Flexible Administration**: Validator management and bridge status controls

## Technical Specifications

### Constants

- Minimum Deposit: 100,000 units
- Maximum Deposit: 1,000,000,000 units
- Required Confirmations: 6 blocks

### Error Codes

| Code | Description              |
| ---- | ------------------------ |
| 1000 | Not Authorized           |
| 1001 | Invalid Amount           |
| 1002 | Insufficient Balance     |
| 1003 | Invalid Bridge Status    |
| 1004 | Invalid Signature        |
| 1005 | Already Processed        |
| 1006 | Bridge Paused            |
| 1007 | Invalid Validator        |
| 1008 | Invalid Recipient        |
| 1009 | Invalid BTC Address      |
| 1010 | Invalid Transaction Hash |
| 1011 | Invalid Signature Format |

## Core Functions

### Bridge Management

```clarity
(define-public (toggle-bridge-status))
```

Toggles the operational status of the bridge. Only callable by contract deployer.

```clarity
(define-public (manage-validator (validator principal) (add bool)))
```

Adds or removes validators from the bridge network.

### Deposit Operations

```clarity
(define-public (initiate-deposit
    (tx-hash (buff 32))
    (amount uint)
    (recipient principal)
    (btc-sender (buff 33))
))
```

Initiates a deposit from Bitcoin to Stacks. Requires validator authorization.

```clarity
(define-public (confirm-deposit
    (tx-hash (buff 32))
    (signature (buff 65))
))
```

Confirms a deposit with validator signature after required confirmations.

### Withdrawal Operations

```clarity
(define-public (withdraw
    (amount uint)
    (btc-recipient (buff 34))
))
```

Initiates a withdrawal from Stacks to Bitcoin.

```clarity
(define-public (emergency-withdraw (amount uint) (recipient principal)))
```

Emergency withdrawal function for contract administrator.

## Query Functions

```clarity
(define-read-only (get-deposit (tx-hash (buff 32))))
(define-read-only (get-bridge-status))
(define-read-only (get-validator-status (validator principal)))
(define-read-only (get-bridge-balance (user principal)))
```

## Security Measures

### Validation Checks

- Principal validation
- Bitcoin address format validation (33 bytes)
- Transaction hash validation (32 bytes)
- Signature validation (65 bytes)
- Balance and amount bounds checking
- Duplicate transaction prevention
- Multi-validator consensus requirement

### Safety Features

1. Bridge pause mechanism
2. Emergency withdrawal capability
3. Strict balance tracking
4. Required confirmations for finality
5. Validator authorization checks

## Data Structures

### Deposits Map

```clarity
{
    tx-hash: (buff 32),
    amount: uint,
    recipient: principal,
    processed: bool,
    confirmations: uint,
    timestamp: uint,
    btc-sender: (buff 33)
}
```

### Validator Signatures Map

```clarity
{
    tx-hash: (buff 32),
    validator: principal,
    signature: (buff 65),
    timestamp: uint
}
```

## Usage Flow

1. **Deposit Process**

   - User initiates Bitcoin transaction
   - Validator calls `initiate-deposit`
   - Wait for required confirmations
   - Validator confirms with signature
   - Assets credited to recipient

2. **Withdrawal Process**
   - User calls `withdraw`
   - Specifies Bitcoin recipient address
   - Bridge processes withdrawal
   - Assets transferred on Bitcoin network

## Security Considerations

- Always verify transaction confirmations
- Maintain secure validator key management
- Monitor bridge status regularly
- Validate all input parameters
- Keep withdrawal amounts within limits
- Ensure proper validator authorization

## Best Practices

1. **For Validators**

   - Maintain secure private keys
   - Verify all transaction details
   - Monitor confirmation counts
   - Check bridge status before operations

2. **For Users**
   - Verify recipient addresses
   - Stay within deposit/withdrawal limits
   - Wait for required confirmations
   - Check transaction status

## Development and Testing

For local development and testing:

1. Deploy contract to testnet first
2. Test with minimal amounts
3. Verify all error conditions
4. Test pause functionality
5. Validate emergency procedures
