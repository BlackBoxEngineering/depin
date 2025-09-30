# Smart Contract API

## Program Overview

The Civic Check-In program manages location-based check-ins, NFT rewards, and token distribution on Solana. This documentation covers all program interfaces, account structures, and client integration patterns.

**Program ID**: `CivicMain111111111111111111111111111111111`

## Core Instructions

### initialize
Initialize the program with global settings and create the global state account.

```rust
pub fn initialize(
    ctx: Context<Initialize>,
    authority: Pubkey,
    base_reward: u64,
    min_checkin_interval: i64,
) -> Result<()>
```

**Parameters:**
- `authority`: Program authority public key
- `base_reward`: Base token reward per check-in (in lamports)
- `min_checkin_interval`: Minimum seconds between check-ins

**Accounts:**
- `global_state` (mut, init): Global program state
- `authority` (signer): Program authority
- `system_program`: Solana system program

### register_node
Register a new check-in node with location verification.

```rust
pub fn register_node(
    ctx: Context<RegisterNode>,
    node_id: String,
    location: String,
    latitude: f64,
    longitude: f64,
    node_type: NodeType,
) -> Result<()>
```

**Parameters:**
- `node_id`: Unique identifier (max 32 chars)
- `location`: Human-readable location name
- `latitude`: GPS latitude (-90.0 to 90.0)
- `longitude`: GPS longitude (-180.0 to 180.0)
- `node_type`: Museum, Park, Library, or Community

**Accounts:**
- `global_state` (mut): Global program state
- `node_account` (mut, init): New node account
- `authority` (signer): Program authority
- `system_program`: Solana system program

### check_in
Process a user check-in at a registered node.

```rust
pub fn check_in(
    ctx: Context<CheckIn>,
    node_id: String,
    timestamp: i64,
    signature_proof: [u8; 64],
) -> Result<()>
```

**Parameters:**
- `node_id`: Target node identifier
- `timestamp`: Check-in timestamp (Unix seconds)
- `signature_proof`: Hardware node signature proof

**Accounts:**
- `global_state` (mut): Global program state
- `node_account` (mut): Target node account
- `user_profile` (mut, init_if_needed): User profile
- `user` (signer): User wallet
- `token_mint`: Civic token mint
- `user_token_account` (mut): User's token account
- `rewards_vault` (mut): Program rewards vault
- `token_program`: SPL Token program
- `system_program`: Solana system program

### mint_attendance_nft
Mint commemorative NFT for verified attendance.

```rust
pub fn mint_attendance_nft(
    ctx: Context<MintAttendanceNFT>,
    metadata_uri: String,
    collection: Pubkey,
    node_id: String,
) -> Result<()>
```

**Parameters:**
- `metadata_uri`: IPFS/Arweave metadata URI
- `collection`: NFT collection address
- `node_id`: Associated node identifier

**Accounts:**
- `nft_mint` (mut, init): New NFT mint
- `nft_metadata` (mut, init): NFT metadata account
- `user_nft_account` (mut, init): User's NFT token account
- `user` (signer): NFT recipient
- `node_account`: Associated node
- `collection_mint`: Collection mint
- `token_program`: SPL Token program
- `metadata_program`: Metaplex metadata program
- `system_program`: Solana system program

### update_node_status
Update node operational status and health metrics.

```rust
pub fn update_node_status(
    ctx: Context<UpdateNodeStatus>,
    node_id: String,
    is_active: bool,
    battery_level: u8,
    last_ping: i64,
) -> Result<()>
```

**Parameters:**
- `node_id`: Node identifier
- `is_active`: Node operational status
- `battery_level`: Battery percentage (0-100)
- `last_ping`: Last communication timestamp

### create_governance_proposal
Create a new governance proposal for community voting.

```rust
pub fn create_governance_proposal(
    ctx: Context<CreateProposal>,
    title: String,
    description: String,
    proposal_type: ProposalType,
    voting_period: i64,
) -> Result<()>
```

## Account Structures

### GlobalState
Program-wide configuration and statistics.

```rust
#[account]
pub struct GlobalState {
    pub authority: Pubkey,           // Program authority
    pub total_nodes: u64,            // Total registered nodes
    pub total_checkins: u64,         // All-time check-ins
    pub base_reward: u64,            // Base reward per check-in
    pub min_checkin_interval: i64,   // Minimum seconds between check-ins
    pub token_mint: Pubkey,          // Civic token mint
    pub rewards_vault: Pubkey,       // Rewards distribution vault
    pub nft_collection: Pubkey,      // Default NFT collection
    pub governance_threshold: u64,   // Min tokens for governance
    pub bump: u8,                    // PDA bump seed
}
```

**Size**: 8 + 32 + 8 + 8 + 8 + 8 + 32 + 32 + 32 + 8 + 1 = 177 bytes

### NodeAccount
Individual node registration and status.

```rust
#[account]
pub struct NodeAccount {
    pub node_id: String,             // Unique identifier (32 chars max)
    pub location: String,            // Location name (64 chars max)
    pub latitude: f64,               // GPS latitude
    pub longitude: f64,              // GPS longitude
    pub node_type: NodeType,         // Museum, Park, Library, Community
    pub owner: Pubkey,               // Node owner/operator
    pub total_checkins: u64,         // Check-ins at this node
    pub is_active: bool,             // Operational status
    pub battery_level: u8,           // Battery percentage (0-100)
    pub last_ping: i64,              // Last communication timestamp
    pub created_at: i64,             // Registration timestamp
    pub bump: u8,                    // PDA bump seed
}
```

**Size**: 8 + 36 + 68 + 8 + 8 + 1 + 32 + 8 + 1 + 1 + 8 + 8 + 1 = 188 bytes

### UserProfile
User engagement history and rewards.

```rust
#[account]
pub struct UserProfile {
    pub wallet: Pubkey,              // User wallet address
    pub total_checkins: u64,         // Lifetime check-ins
    pub current_streak: u64,         // Consecutive days with check-ins
    pub max_streak: u64,             // Highest streak achieved
    pub last_checkin: i64,           // Last check-in timestamp
    pub last_checkin_node: String,   // Last node visited (32 chars)
    pub total_rewards: u64,          // Total tokens earned
    pub nfts_earned: u64,            // Total NFTs minted
    pub governance_votes: u64,       // Governance participation count
    pub created_at: i64,             // Profile creation timestamp
    pub bump: u8,                    // PDA bump seed
}
```

**Size**: 8 + 32 + 8 + 8 + 8 + 8 + 36 + 8 + 8 + 8 + 8 + 1 = 141 bytes

### CheckInRecord
Individual check-in transaction record.

```rust
#[account]
pub struct CheckInRecord {
    pub user: Pubkey,                // User wallet
    pub node_id: String,             // Node identifier (32 chars)
    pub timestamp: i64,              // Check-in timestamp
    pub reward_amount: u64,          // Tokens awarded
    pub nft_minted: Option<Pubkey>,  // NFT mint if created
    pub streak_bonus: u64,           // Bonus for streak
    pub signature_proof: [u8; 64],   // Hardware signature
    pub bump: u8,                    // PDA bump seed
}
```

### GovernanceProposal
Community governance proposal.

```rust
#[account]
pub struct GovernanceProposal {
    pub id: u64,                     // Proposal ID
    pub proposer: Pubkey,            // Proposal creator
    pub title: String,               // Proposal title (64 chars)
    pub description: String,         // Description (256 chars)
    pub proposal_type: ProposalType, // Type of proposal
    pub votes_for: u64,              // Supporting votes
    pub votes_against: u64,          // Opposing votes
    pub voting_ends: i64,            // Voting deadline
    pub executed: bool,              // Execution status
    pub created_at: i64,             // Creation timestamp
    pub bump: u8,                    // PDA bump seed
}
```

## Enums and Types

### NodeType
```rust
#[derive(AnchorSerialize, AnchorDeserialize, Clone, PartialEq, Eq)]
pub enum NodeType {
    Museum,
    Park,
    Library,
    Community,
    Government,
    Educational,
}
```

### ProposalType
```rust
#[derive(AnchorSerialize, AnchorDeserialize, Clone, PartialEq, Eq)]
pub enum ProposalType {
    ParameterChange,     // Change program parameters
    NodeAddition,        // Add new node type
    RewardAdjustment,    // Modify reward structure
    GovernanceUpdate,    // Update governance rules
}
```

## Error Codes

```rust
#[error_code]
pub enum ErrorCode {
    #[msg("Node not found or inactive")]
    NodeNotFound = 6000,
    
    #[msg("User already checked in within minimum interval")]
    AlreadyCheckedIn = 6001,
    
    #[msg("Invalid location coordinates")]
    InvalidLocation = 6002,
    
    #[msg("Insufficient rewards balance in vault")]
    InsufficientRewards = 6003,
    
    #[msg("Invalid node signature proof")]
    InvalidSignatureProof = 6004,
    
    #[msg("Node ID already exists")]
    NodeIdExists = 6005,
    
    #[msg("Unauthorized operation")]
    Unauthorized = 6006,
    
    #[msg("Invalid timestamp - too far in past or future")]
    InvalidTimestamp = 6007,
    
    #[msg("Insufficient governance tokens for proposal")]
    InsufficientGovernanceTokens = 6008,
    
    #[msg("Voting period has ended")]
    VotingPeriodEnded = 6009,
    
    #[msg("Proposal already executed")]
    ProposalAlreadyExecuted = 6010,
}
```

## Client SDK Usage

### JavaScript/TypeScript

#### Program Initialization
```typescript
import { Program, AnchorProvider, web3 } from '@coral-xyz/anchor';
import { CivicCheckIn } from './types/civic_check_in';
import { PublicKey, Connection } from '@solana/web3.js';

const connection = new Connection('https://api.mainnet-beta.solana.com');
const provider = new AnchorProvider(connection, wallet, {});
const program = new Program<CivicCheckIn>(idl, programId, provider);
```

#### Check-in Transaction
```typescript
// Derive PDAs
const [globalState] = PublicKey.findProgramAddressSync(
  [Buffer.from('global')],
  program.programId
);

const [nodeAccount] = PublicKey.findProgramAddressSync(
  [Buffer.from('node'), Buffer.from(nodeId)],
  program.programId
);

const [userProfile] = PublicKey.findProgramAddressSync(
  [Buffer.from('user'), userWallet.toBuffer()],
  program.programId
);

// Execute check-in
const tx = await program.methods
  .checkIn(nodeId, Math.floor(Date.now() / 1000), signatureProof)
  .accounts({
    globalState,
    nodeAccount,
    userProfile,
    user: userWallet,
    tokenMint: civicTokenMint,
    userTokenAccount: userTokenAccount,
    rewardsVault: rewardsVault,
    tokenProgram: TOKEN_PROGRAM_ID,
    systemProgram: web3.SystemProgram.programId,
  })
  .signers([userKeypair])
  .rpc();
```

#### Register Node
```typescript
const registerNodeTx = await program.methods
  .registerNode(
    nodeId,
    "Central Library",
    40.7128,
    -74.0060,
    { library: {} } // NodeType enum
  )
  .accounts({
    globalState,
    nodeAccount,
    authority: authorityKeypair.publicKey,
    systemProgram: web3.SystemProgram.programId,
  })
  .signers([authorityKeypair])
  .rpc();
```

#### Mint Attendance NFT
```typescript
const nftMint = web3.Keypair.generate();
const userNftAccount = await getAssociatedTokenAddress(
  nftMint.publicKey,
  userWallet
);

const mintNftTx = await program.methods
  .mintAttendanceNft(
    "https://arweave.net/metadata-hash",
    collectionMint,
    nodeId
  )
  .accounts({
    nftMint: nftMint.publicKey,
    nftMetadata: nftMetadataAccount,
    userNftAccount,
    user: userWallet,
    nodeAccount,
    collectionMint,
    tokenProgram: TOKEN_PROGRAM_ID,
    metadataProgram: METADATA_PROGRAM_ID,
    systemProgram: web3.SystemProgram.programId,
  })
  .signers([nftMint, userKeypair])
  .rpc();
```

### Rust Client

#### Setup and Configuration
```rust
use anchor_client::solana_sdk::{
    signature::{Keypair, Signer},
    pubkey::Pubkey,
    system_program,
};
use anchor_client::{Client, Cluster, Program};
use std::rc::Rc;

let payer = Keypair::new();
let client = Client::new(Cluster::Mainnet, Rc::new(payer));
let program = client.program(program_id)?;
```

#### Check-in Implementation
```rust
// Derive program addresses
let (global_state, _) = Pubkey::find_program_address(
    &[b"global"],
    &program.id(),
);

let (node_account, _) = Pubkey::find_program_address(
    &[b"node", node_id.as_bytes()],
    &program.id(),
);

let (user_profile, _) = Pubkey::find_program_address(
    &[b"user", user_wallet.as_ref()],
    &program.id(),
);

// Execute check-in
let tx = program
    .request()
    .accounts(civic_check_in::accounts::CheckIn {
        global_state,
        node_account,
        user_profile,
        user: user_wallet,
        token_mint: civic_token_mint,
        user_token_account,
        rewards_vault,
        token_program: spl_token::id(),
        system_program: system_program::id(),
    })
    .args(civic_check_in::instruction::CheckIn {
        node_id: node_id.to_string(),
        timestamp: chrono::Utc::now().timestamp(),
        signature_proof,
    })
    .send()?;
```

### Python Client (using Anchorpy)
```python
from anchorpy import Program, Provider, Wallet
from solana.rpc.async_api import AsyncClient
from solana.keypair import Keypair

# Initialize
client = AsyncClient("https://api.mainnet-beta.solana.com")
wallet = Wallet(Keypair())
provider = Provider(client, wallet)
program = Program.load(idl, program_id, provider)

# Check-in
tx = await program.rpc["check_in"](
    node_id,
    timestamp,
    signature_proof,
    ctx=Context(
        accounts={
            "global_state": global_state,
            "node_account": node_account,
            "user_profile": user_profile,
            "user": wallet.public_key,
            "token_mint": civic_token_mint,
            "user_token_account": user_token_account,
            "rewards_vault": rewards_vault,
            "token_program": TOKEN_PROGRAM_ID,
            "system_program": SYSTEM_PROGRAM_ID,
        },
        signers=[wallet.payer],
    ),
)
```

## Events

### CheckInEvent
Emitted when a user successfully checks in at a node.

```rust
#[event]
pub struct CheckInEvent {
    pub user: Pubkey,                // User wallet address
    pub node_id: String,             // Node identifier
    pub timestamp: i64,              // Check-in timestamp
    pub reward_amount: u64,          // Tokens awarded
    pub streak_count: u64,           // Current user streak
    pub streak_bonus: u64,           // Bonus for streak
    pub total_checkins: u64,         // User's total check-ins
    pub nft_minted: Option<Pubkey>,  // NFT mint if created
}
```

### NodeRegisteredEvent
Emitted when a new node is registered.

```rust
#[event]
pub struct NodeRegisteredEvent {
    pub node_id: String,             // Unique node identifier
    pub location: String,            // Location name
    pub latitude: f64,               // GPS latitude
    pub longitude: f64,              // GPS longitude
    pub node_type: NodeType,         // Type of location
    pub owner: Pubkey,               // Node owner
    pub timestamp: i64,              // Registration timestamp
}
```

### NFTMintedEvent
Emitted when an attendance NFT is minted.

```rust
#[event]
pub struct NFTMintedEvent {
    pub user: Pubkey,                // NFT recipient
    pub nft_mint: Pubkey,            // NFT mint address
    pub node_id: String,             // Associated node
    pub metadata_uri: String,        // Metadata URI
    pub collection: Pubkey,          // NFT collection
    pub timestamp: i64,              // Mint timestamp
}
```

### GovernanceProposalEvent
Emitted when governance proposals are created or executed.

```rust
#[event]
pub struct GovernanceProposalEvent {
    pub proposal_id: u64,            // Proposal identifier
    pub proposer: Pubkey,            // Proposal creator
    pub proposal_type: ProposalType, // Type of proposal
    pub action: String,              // "created" or "executed"
    pub timestamp: i64,              // Event timestamp
}
```

### NodeStatusUpdateEvent
Emitted when node status is updated.

```rust
#[event]
pub struct NodeStatusUpdateEvent {
    pub node_id: String,             // Node identifier
    pub is_active: bool,             // New status
    pub battery_level: u8,           // Battery percentage
    pub last_ping: i64,              // Last communication
    pub timestamp: i64,              // Update timestamp
}
```

## Program Derived Addresses (PDAs)

### Global State
```rust
seeds = [b"global"]
```

### Node Account
```rust
seeds = [b"node", node_id.as_bytes()]
```

### User Profile
```rust
seeds = [b"user", user_wallet.as_ref()]
```

### Check-in Record
```rust
seeds = [b"checkin", user_wallet.as_ref(), node_id.as_bytes(), timestamp.to_le_bytes().as_ref()]
```

### Governance Proposal
```rust
seeds = [b"proposal", proposal_id.to_le_bytes().as_ref()]
```

### Rewards Vault
```rust
seeds = [b"rewards_vault"]
```

## Integration Examples

### Event Listening (TypeScript)
```typescript
// Listen for check-in events
program.addEventListener('CheckInEvent', (event, slot) => {
  console.log(`User ${event.user} checked in at ${event.nodeId}`);
  console.log(`Earned ${event.rewardAmount} tokens, streak: ${event.streakCount}`);
  
  if (event.nftMinted) {
    console.log(`NFT minted: ${event.nftMinted}`);
  }
});

// Listen for new node registrations
program.addEventListener('NodeRegisteredEvent', (event, slot) => {
  console.log(`New node registered: ${event.nodeId} at ${event.location}`);
});
```

### Account Fetching
```typescript
// Fetch all nodes
const allNodes = await program.account.nodeAccount.all();

// Fetch user profile
const [userProfilePda] = PublicKey.findProgramAddressSync(
  [Buffer.from('user'), userWallet.toBuffer()],
  program.programId
);
const userProfile = await program.account.userProfile.fetch(userProfilePda);

// Fetch global statistics
const [globalStatePda] = PublicKey.findProgramAddressSync(
  [Buffer.from('global')],
  program.programId
);
const globalState = await program.account.globalState.fetch(globalStatePda);
```

### Error Handling
```typescript
try {
  await program.methods.checkIn(nodeId, timestamp, proof)
    .accounts({ /* accounts */ })
    .rpc();
} catch (error) {
  if (error.code === 6001) {
    console.log('Already checked in today');
  } else if (error.code === 6000) {
    console.log('Node not found');
  } else {
    console.log('Unexpected error:', error);
  }
}
```

## Testing

### Unit Test Example
```typescript
import * as anchor from '@coral-xyz/anchor';
import { Program } from '@coral-xyz/anchor';
import { CivicCheckIn } from '../target/types/civic_check_in';
import { expect } from 'chai';

describe('civic-check-in', () => {
  const provider = anchor.AnchorProvider.env();
  anchor.setProvider(provider);
  const program = anchor.workspace.CivicCheckIn as Program<CivicCheckIn>;
  
  it('Initializes the program', async () => {
    const tx = await program.methods
      .initialize(
        provider.wallet.publicKey,
        new anchor.BN(1000000), // 1 token base reward
        new anchor.BN(86400)     // 24 hour minimum interval
      )
      .rpc();
    
    const globalState = await program.account.globalState.fetch(globalStatePda);
    expect(globalState.authority.toString()).to.equal(provider.wallet.publicKey.toString());
  });
  
  it('Registers a new node', async () => {
    await program.methods
      .registerNode(
        'TEST_NODE_001',
        'Test Library',
        40.7128,
        -74.0060,
        { library: {} }
      )
      .rpc();
    
    const nodeAccount = await program.account.nodeAccount.fetch(nodeAccountPda);
    expect(nodeAccount.nodeId).to.equal('TEST_NODE_001');
    expect(nodeAccount.isActive).to.be.true;
  });
  
  it('Processes check-in successfully', async () => {
    const timestamp = Math.floor(Date.now() / 1000);
    const signatureProof = new Array(64).fill(0);
    
    await program.methods
      .checkIn('TEST_NODE_001', new anchor.BN(timestamp), signatureProof)
      .rpc();
    
    const userProfile = await program.account.userProfile.fetch(userProfilePda);
    expect(userProfile.totalCheckins.toNumber()).to.equal(1);
  });
});
```

## Security Considerations

### Access Control
- Only program authority can register nodes
- Users can only check in with valid signature proofs
- Node status updates require proper authentication

### Anti-Fraud Measures
- Minimum time intervals between check-ins
- Hardware signature verification
- Geographic validation for node locations
- Rate limiting on reward distribution

### Best Practices
- Always validate input parameters
- Use PDAs for deterministic account addresses
- Implement proper error handling
- Monitor program logs for suspicious activity
- Regular security audits of smart contract code

## Deployment Guide

### Prerequisites
```bash
# Install Solana CLI
sh -c "$(curl -sSfL https://release.solana.com/stable/install)"

# Install Anchor
npm install -g @coral-xyz/anchor-cli

# Verify installation
anchor --version
solana --version
```

### Build and Deploy
```bash
# Build the program
anchor build

# Deploy to devnet
anchor deploy --provider.cluster devnet

# Deploy to mainnet (requires sufficient SOL)
anchor deploy --provider.cluster mainnet-beta
```

### Post-Deployment Setup
```bash
# Initialize the program
anchor run initialize

# Register initial nodes
anchor run register-nodes

# Set up token mint and rewards vault
anchor run setup-tokens
```