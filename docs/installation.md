# Software Installation

## Development Environment

### Prerequisites
- Node.js 18+
- Rust 1.70+
- Solana CLI 1.16+
- Anchor Framework 0.28+

### Quick Setup
```bash
# Install Node.js dependencies
npm install

# Install Rust
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# Install Solana CLI
sh -c "$(curl -sSfL https://release.solana.com/v1.16.0/install)"

# Install Anchor
cargo install --git https://github.com/coral-xyz/anchor avm --locked --force
avm install latest
avm use latest
```

## Node Software Installation

### Raspberry Pi Setup
```bash
# Update system
sudo apt update && sudo apt upgrade -y

# Install dependencies
sudo apt install -y python3-pip git nodejs npm

# Clone repository
git clone https://github.com/your-org/civic-nodes.git
cd civic-nodes

# Install node service
cd node-service
npm install
sudo npm install -g pm2

# Configure service
cp config/node.example.toml config/node.toml
# Edit config/node.toml with your settings

# Start service
pm2 start ecosystem.config.js
pm2 startup
pm2 save
```

### Smart Contract Deployment
```bash
# Build programs
anchor build

# Deploy to devnet
anchor deploy --provider.cluster devnet

# Initialize program
anchor run initialize
```

## Frontend Development

### Mobile App (React Native)
```bash
cd mobile-app
npm install

# iOS
cd ios && pod install && cd ..
npx react-native run-ios

# Android
npx react-native run-android
```

### Web Dashboard
```bash
cd web-dashboard
npm install
npm run dev
```

## Configuration

### Environment Variables
```bash
# Copy example files
cp .env.example .env
cp config/node.example.toml config/node.toml

# Edit with your values
SOLANA_RPC_URL=https://api.devnet.solana.com
PROGRAM_ID=your_program_id_here
WALLET_PRIVATE_KEY=your_private_key_here
```

### Database Setup
```bash
# Install PostgreSQL
sudo apt install postgresql postgresql-contrib

# Create database
sudo -u postgres createdb civic_nodes

# Run migrations
npm run migrate
```