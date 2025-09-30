# Civic Check-In Nodes
## Decentralized Physical Infrastructure for Civic Engagement

A DePIN solution that rewards citizens for engaging with public spaces through tamper-proof check-in nodes deployed at museums, parks, libraries, and community centers.

## 🏛️ Overview

Civic Check-In Nodes create a decentralized network of physical infrastructure that:
- Verifies attendance at civic locations
- Rewards participation with NFTs and tokens
- Builds community engagement through blockchain incentives
- Operates independently with minimal maintenance

## 🔧 System Architecture

### Hardware Components
- **Raspberry Pi/ESP32**: Core processing unit
- **NFC Reader/QR Scanner**: User interaction interface
- **WiFi/LoRaWAN Module**: Blockchain connectivity
- **Solar Panel + Battery**: Off-grid power (optional)
- **Tamper-Proof Casing**: Public deployment protection

### Software Stack
- **Frontend**: Solana wallet integration (Phantom)
- **Node Logic**: Rust/Python microservice
- **Blockchain**: Solana smart contracts
- **Storage**: IPFS/Arweave for metadata
- **Admin Dashboard**: Web UI for civic organizations

## 🪙 Token Mechanics

### NFTs (Proof of Attendance)
- Unique collectibles per location
- Timestamped and location-verified
- Tradeable or redeemable for local benefits

### Civic Tokens
- Fungible rewards for participation
- Redeemable for discounts, merchandise, voting rights
- Stakeable to support local initiatives

## 🚀 Quick Start

### Prerequisites
- Solana CLI tools
- Rust toolchain
- Node.js and npm
- Hardware components (see [Hardware Guide](./docs/hardware.md))

### Installation
```bash
git clone https://github.com/your-org/civic-checkin-nodes
cd civic-checkin-nodes
npm install
cargo build --release
```

### Deploy a Node
```bash
# Configure your node
cp config/node.example.toml config/node.toml
# Edit config with your location details

# Deploy smart contracts
solana program deploy target/deploy/civic_checkin.so

# Start node service
cargo run --bin node-service
```

## 📁 Project Structure

```
civic-checkin-nodes/
├── contracts/          # Solana smart contracts
├── node-service/       # Hardware node software
├── frontend/           # User mobile/web app
├── admin-dashboard/    # Civic org management UI
├── hardware/           # PCB designs and 3D models
├── docs/              # Documentation
└── scripts/           # Deployment and utility scripts
```

## 🌐 Use Cases

### Museums & Cultural Sites
- Proof of visit NFTs with historical artwork
- Educational content unlocked through participation
- Membership rewards and exclusive access

### Parks & Recreation
- Environmental stewardship tokens
- Trail completion badges
- Community event check-ins

### Libraries & Education
- Reading program participation
- Study session rewards
- Community learning incentives

### Community Centers
- Volunteer hour tracking
- Event attendance verification
- Local governance participation

## 🔒 Security Features

- **Tamper Detection**: Hardware integrity monitoring
- **Cryptographic Verification**: All transactions signed and verified
- **Decentralized Storage**: No single point of failure
- **Privacy Preserving**: Minimal personal data collection

## 🛠️ Development

### Running Tests
```bash
# Smart contract tests
cd contracts && cargo test

# Node service tests
cd node-service && cargo test

# Frontend tests
cd frontend && npm test
```

### Contributing
1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests
5. Submit a pull request

## 📖 Documentation

- [Hardware Setup Guide](./docs/hardware.md)
- [Smart Contract API](./docs/contracts.md)
- [Node Configuration](./docs/node-config.md)
- [Admin Dashboard Guide](./docs/admin.md)
- [Token Economics](./docs/tokenomics.md)

## 🤝 Community

- **Discord**: [Join our community](https://discord.gg/civic-nodes)
- **Twitter**: [@CivicNodes](https://twitter.com/civicnodes)
- **Forum**: [Community discussions](https://forum.civic-nodes.org)

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- Solana Foundation for blockchain infrastructure
- Open source hardware community
- Civic organizations pioneering digital engagement

---

**Built by engineers, for communities. Sovereign infrastructure for the public good.**