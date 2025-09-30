# Documentation

This directory contains comprehensive documentation for the Civic Check-In Nodes project.

## 📚 Documentation Index

### Setup & Installation
- [Hardware Setup Guide](./hardware.md) - Physical node assembly and deployment
- [Software Installation](./installation.md) - Development environment setup
- [Node Configuration](./node-config.md) - Configuring nodes for deployment

### Technical Documentation
- [Smart Contract API](./contracts.md) - Solana program interfaces and methods
- [Node Service Architecture](./node-architecture.md) - Hardware node software design
- [Frontend Integration](./frontend.md) - Mobile and web app development
- [Admin Dashboard](./admin.md) - Civic organization management interface

### Economics & Governance
- [Token Economics](./tokenomics.md) - Reward mechanisms and token flows
- [Governance Model](./governance.md) - Community decision-making processes
- [Deployment Strategy](./deployment.md) - Rollout plans and partnerships

### Operations
- [Maintenance Guide](./maintenance.md) - Node upkeep and troubleshooting
- [Security Practices](./security.md) - Best practices for secure deployment
- [Monitoring & Analytics](./monitoring.md) - System health and usage tracking

## 🔧 Quick Reference

### Common Commands
```bash
# Deploy a new node
./scripts/deploy-node.sh --location "Central Library" --lat 40.7128 --lng -74.0060

# Update node firmware
./scripts/update-node.sh --node-id NODE_123

# Generate civic NFT collection
./scripts/create-collection.sh --org "City Museum" --theme "Historical"
```

### Configuration Templates
- [Node Config Template](../config/node.example.toml)
- [Smart Contract Config](../config/program.example.toml)
- [Frontend Environment](../config/.env.example)

## 🆘 Troubleshooting

### Common Issues
1. **Node Offline**: Check power and network connectivity
2. **Transaction Failures**: Verify Solana RPC endpoint and wallet balance
3. **NFC Not Working**: Ensure proper antenna positioning and power levels

### Support Channels
- GitHub Issues for bugs and feature requests
- Discord for community support
- Email support@civic-nodes.org for critical issues

## 🤝 Contributing to Documentation

We welcome contributions to improve our documentation:

1. Fork the repository
2. Edit or create documentation files
3. Follow our [documentation style guide](./style-guide.md)
4. Submit a pull request

### Documentation Standards
- Use clear, concise language
- Include code examples where relevant
- Add diagrams for complex concepts
- Keep information up-to-date with code changes