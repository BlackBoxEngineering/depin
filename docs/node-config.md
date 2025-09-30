# Node Configuration

## Configuration File Structure

### node.toml
```toml
[node]
id = "NODE_001"
location = "Central Library"
latitude = 40.7128
longitude = -74.0060

[network]
rpc_url = "https://api.mainnet-beta.solana.com"
program_id = "CivicCheckInProgram11111111111111111111111"
wallet_path = "/home/pi/.config/solana/id.json"

[hardware]
nfc_device = "/dev/ttyUSB0"
gps_device = "/dev/ttyAMA0"
led_pin = 18

[rewards]
base_reward = 100
daily_bonus = 50
streak_multiplier = 1.5
```

## Network Configuration

### Solana RPC Endpoints
```toml
# Mainnet
rpc_url = "https://api.mainnet-beta.solana.com"

# Devnet (for testing)
rpc_url = "https://api.devnet.solana.com"

# Custom RPC (recommended for production)
rpc_url = "https://your-rpc-provider.com"
```

### Wallet Setup
```bash
# Generate new wallet
solana-keygen new --outfile ~/.config/solana/id.json

# Fund wallet (devnet)
solana airdrop 2 --url devnet

# Check balance
solana balance
```

## Hardware Configuration

### NFC Reader Settings
```toml
[nfc]
enabled = true
read_timeout = 5000
retry_attempts = 3
supported_types = ["NTAG213", "NTAG215", "NTAG216"]
```

### GPS Configuration
```toml
[gps]
enabled = true
update_interval = 30
accuracy_threshold = 10.0
```

## Reward Parameters

### Token Distribution
```toml
[rewards]
# Base reward per check-in (in tokens)
base_reward = 100

# Daily first check-in bonus
daily_bonus = 50

# Consecutive day multiplier
streak_multiplier = 1.5

# Maximum daily rewards
daily_cap = 1000
```

### Location-Based Modifiers
```toml
[location_modifiers]
# Boost rewards for specific venue types
museum = 1.2
library = 1.1
park = 1.0
restaurant = 0.9
```

## Security Settings

### Access Control
```toml
[security]
# Require admin approval for config changes
admin_approval = true

# Encrypt sensitive data
encrypt_wallet = true

# Rate limiting
max_checkins_per_hour = 10
```

### Monitoring
```toml
[monitoring]
# Health check interval (seconds)
health_check_interval = 60

# Log level
log_level = "info"

# Remote monitoring endpoint
monitoring_url = "https://monitor.civic-nodes.org"
```

## Deployment Profiles

### Development
```toml
[profile.dev]
rpc_url = "https://api.devnet.solana.com"
log_level = "debug"
mock_hardware = true
```

### Production
```toml
[profile.prod]
rpc_url = "https://api.mainnet-beta.solana.com"
log_level = "warn"
encrypt_wallet = true
admin_approval = true
```