# Smart Contracts

Solana programs powering the Civic Check-In Node network.

## 📋 Overview

The smart contract layer handles:
- NFT minting for proof of attendance
- Civic token distribution and management
- Node registration and verification
- Governance and voting mechanisms

## 🏗️ Contract Architecture

### Core Programs

#### `civic_checkin.rs`
Main program handling check-ins and rewards
- **check_in()**: Process user attendance verification
- **mint_attendance_nft()**: Create proof-of-attendance NFT
- **distribute_tokens()**: Award civic tokens
- **verify_node()**: Validate node authenticity

#### `node_registry.rs`
Node management and registration
- **register_node()**: Add new node to network
- **update_node_status()**: Report node health
- **deactivate_node()**: Remove compromised nodes

#### `governance.rs`
Community governance and voting
- **create_proposal()**: Submit governance proposals
- **vote()**: Cast votes on proposals
- **execute_proposal()**: Implement approved changes

## 🚀 Deployment

### Prerequisites
```bash
# Install Solana CLI
sh -c "$(curl -sSfL https://release.solana.com/v1.16.0/install)"

# Install Anchor framework
npm install -g @coral-xyz/anchor-cli
```

### Build and Deploy
```bash
# Build programs
anchor build

# Deploy to devnet
anchor deploy --provider.cluster devnet

# Deploy to mainnet
anchor deploy --provider.cluster mainnet-beta
```

### Program IDs
- **Devnet**: `CivicDev1111111111111111111111111111111111`
- **Mainnet**: `CivicMain111111111111111111111111111111111`

## 🔧 Development

### Testing
```bash
# Run all tests
anchor test

# Run specific test suite
anchor test --skip-deploy tests/civic_checkin.ts
```

### Local Development
```bash
# Start local validator
solana-test-validator

# Deploy to local
anchor deploy --provider.cluster localnet
```

## 📊 Program Data Structures

### CheckInEvent
```rust
pub struct CheckInEvent {
    pub user: Pubkey,
    pub node_id: String,
    pub timestamp: i64,
    pub location: String,
    pub nft_mint: Option<Pubkey>,
}
```

### Node
```rust
pub struct Node {
    pub id: String,
    pub location: String,
    pub coordinates: [f64; 2],
    pub owner: Pubkey,
    pub status: NodeStatus,
    pub total_checkins: u64,
}
```

### CivicToken
```rust
pub struct CivicToken {
    pub mint: Pubkey,
    pub total_supply: u64,
    pub rewards_pool: u64,
    pub governance_threshold: u64,
}
```

## 🔐 Security Considerations

### Access Control
- Node registration requires admin signature
- Check-ins validated through cryptographic proofs
- Governance proposals require minimum token stake

### Anti-Fraud Measures
- Rate limiting on check-ins per user
- Geographic validation for node locations
- Tamper detection through hardware attestation

## 📖 API Reference

### Instructions

#### check_in
Process user attendance at civic location
```rust
pub fn check_in(
    ctx: Context<CheckIn>,
    node_id: String,
    user_signature: [u8; 64],
) -> Result<()>
```

#### register_node
Add new node to the network
```rust
pub fn register_node(
    ctx: Context<RegisterNode>,
    node_id: String,
    location: String,
    coordinates: [f64; 2],
) -> Result<()>
```

## 🧪 Testing Framework

### Unit Tests
Located in `tests/` directory:
- `civic_checkin.ts` - Core check-in functionality
- `node_registry.ts` - Node management
- `governance.ts` - Voting mechanisms

### Integration Tests
End-to-end scenarios:
- User journey from check-in to reward
- Node lifecycle management
- Governance proposal execution

## 📈 Monitoring

### Program Logs
Monitor program execution:
```bash
solana logs <PROGRAM_ID>
```

### Metrics Tracking
- Total check-ins processed
- Active nodes in network
- Token distribution rates
- Governance participation

## 🤝 Contributing

1. Follow Rust and Anchor best practices
2. Add comprehensive tests for new features
3. Update documentation for API changes
4. Ensure security review for critical functions