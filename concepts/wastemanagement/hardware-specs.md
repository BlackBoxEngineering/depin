# Hardware Specifications & Bill of Materials

## Smart Bin Node Types

### Type A: Basic Fill Monitoring Node
**Use Case**: Standard waste bins, recycling containers
**Battery Life**: 6-12 months
**Connectivity**: WiFi
**Cost Target**: $45-65

#### Core Components
```
ESP32-S3-WROOM-1 Module
├── CPU: Dual-core Xtensa LX7 @ 240MHz
├── RAM: 512KB SRAM + 2MB PSRAM
├── Flash: 8MB
├── WiFi: 802.11 b/g/n
└── Power: 3.3V, sleep mode 10µA

HC-SR04 Ultrasonic Sensor
├── Range: 2cm - 400cm
├── Accuracy: ±3mm
├── Operating Voltage: 5V
├── Current: 15mA
└── Trigger: 10µs TTL pulse

Power System
├── Battery: 2x 18650 Li-ion (3.7V, 3000mAh)
├── Solar Panel: 6V 2W polycrystalline
├── Charge Controller: TP4056 with protection
└── Voltage Regulator: AMS1117-3.3V
```

#### Detailed BOM
| Component | Part Number | Qty | Unit Cost | Supplier | Notes |
|-----------|-------------|-----|-----------|----------|-------|
| ESP32-S3 Module | ESP32-S3-WROOM-1-N8R2 | 1 | $8.50 | Espressif | 8MB Flash, 2MB PSRAM |
| Ultrasonic Sensor | HC-SR04 | 1 | $2.80 | Generic | Waterproof version preferred |
| 18650 Battery | INR18650-30Q | 2 | $4.00 | Samsung | 3000mAh, high drain |
| Battery Holder | BH-18650-PC2 | 1 | $1.50 | Keystone | 2-cell holder with leads |
| Solar Panel | SP-6V2W | 1 | $12.00 | Voltaic | 6V 2W with junction box |
| Charge Controller | TP4056-MICRO | 1 | $1.20 | Generic | USB-C input, protection |
| Voltage Regulator | AMS1117-3.3 | 1 | $0.30 | AMS | 800mA LDO |
| Enclosure | IP65-150x110x70 | 1 | $8.50 | Bud Industries | ABS plastic, clear lid |
| PCB | Custom 2-layer | 1 | $3.00 | JLCPCB | 50x40mm, HASL finish |
| Connectors | JST-XH 2.54mm | 4 | $0.25 | JST | Battery, sensor connections |
| Mounting Hardware | M4 bolts, nuts | 1 | $2.00 | McMaster | Stainless steel |
| Antenna | PCB 2.4GHz | 1 | $1.50 | Johanson | Ceramic chip antenna |
| Capacitors | Various | 5 | $0.50 | Murata | Decoupling, filtering |
| Resistors | Various | 8 | $0.20 | Yageo | Pull-up, dividers |
| **Total** | | | **$46.25** | | **Qty 100+ pricing** |

### Type B: Advanced AI-Powered Node
**Use Case**: High-traffic areas, contamination monitoring
**Battery Life**: 3-6 months
**Connectivity**: LoRaWAN + WiFi
**Cost Target**: $180-220

#### Core Components
```
Raspberry Pi 4 Model B (4GB)
├── CPU: Quad-core ARM Cortex-A72 @ 1.5GHz
├── RAM: 4GB LPDDR4
├── Storage: 32GB microSD Class 10
├── Connectivity: WiFi, Bluetooth, Ethernet
└── Power: 5V, 3A max

Pi Camera Module v3
├── Sensor: Sony IMX708, 12MP
├── Resolution: 4608×2592 @ 30fps
├── Lens: f/1.8, 75° FOV
├── Interface: MIPI CSI-2
└── Power: 3.3V, 250mA

Load Cell System
├── Sensor: 50kg aluminum single point
├── ADC: HX711 24-bit
├── Accuracy: ±0.1% full scale
├── Interface: SPI
└── Calibration: Software adjustable
```

#### Detailed BOM
| Component | Part Number | Qty | Unit Cost | Supplier | Notes |
|-----------|-------------|-----|-----------|----------|-------|
| Raspberry Pi 4B | RPI4-MODBP-4GB | 1 | $75.00 | Raspberry Pi | 4GB RAM version |
| Pi Camera v3 | RPI-CAM-V3 | 1 | $25.00 | Raspberry Pi | 12MP autofocus |
| Load Cell | TAL220-50kg | 1 | $12.00 | HTC Sensor | Aluminum, IP65 |
| Load Cell Amp | HX711 | 1 | $3.50 | Avia Semi | 24-bit ADC |
| Ultrasonic Array | HC-SR04 | 4 | $2.80 | Generic | Waterproof versions |
| LoRaWAN Module | RFM95W-915S2 | 1 | $8.50 | HopeRF | 915MHz, SPI interface |
| LiFePO4 Battery | 12V-10Ah-LFP | 1 | $45.00 | Battle Born | Prismatic cells |
| Solar Panel | 20W-MONO | 1 | $25.00 | Renogy | Monocrystalline |
| MPPT Controller | CN3722-10A | 1 | $18.00 | Consonance | 10A MPPT |
| DC-DC Converter | LM2596S-5V | 1 | $3.50 | TI | 5V 3A output |
| Enclosure | AL-200x150x100 | 1 | $35.00 | Hammond | Aluminum, IP67 |
| microSD Card | 32GB-C10-A1 | 1 | $8.00 | SanDisk | Industrial grade |
| Cooling Fan | 30x30x7mm | 1 | $4.00 | Noctua | 5V, low noise |
| Mounting Plate | AL-6061-T6 | 1 | $12.00 | Custom | CNC machined |
| Cables & Connectors | Various | 1 | $15.00 | Various | Weatherproof |
| **Total** | | | **$292.30** | | **Qty 50+ pricing** |

## Collection Vehicle Tracker

### GPS & Telematics Module
**Use Case**: Waste collection trucks, route optimization
**Power**: Vehicle 12V system
**Connectivity**: GSM/LTE + GPS
**Cost Target**: $85-110

#### Core Components
```
ESP32-WROVER-E Module
├── CPU: Dual-core Xtensa LX6 @ 240MHz
├── RAM: 520KB SRAM + 8MB PSRAM
├── Flash: 16MB
├── WiFi: 802.11 b/g/n
└── Bluetooth: v4.2 BR/EDR + BLE

SIM800L GSM Module
├── Frequency: Quad-band GSM/GPRS
├── Data: GPRS Class 10
├── SMS: Text and PDU mode
├── Voice: Not required
└── Power: 3.4V-4.4V

NEO-8M GPS Module
├── Channels: 72 acquisition, 18 tracking
├── Accuracy: 2.5m CEP
├── Cold Start: 26s
├── Hot Start: 1s
└── Update Rate: 1-10Hz
```

#### Detailed BOM
| Component | Part Number | Qty | Unit Cost | Supplier | Notes |
|-----------|-------------|-----|-----------|----------|-------|
| ESP32-WROVER-E | ESP32-WROVER-E-N16R8 | 1 | $8.50 | Espressif | 16MB Flash, 8MB PSRAM |
| GSM Module | SIM800L-EVB | 1 | $8.00 | SIMCom | Quad-band GSM/GPRS |
| GPS Module | NEO-8M-001 | 1 | $12.00 | u-blox | High sensitivity |
| Accelerometer | MPU6050 | 1 | $2.50 | InvenSense | 6-axis IMU |
| OBD-II Interface | ELM327-UART | 1 | $15.00 | ELM Electronics | UART interface |
| Power Supply | LM2596S-ADJ | 1 | $2.50 | TI | Adjustable buck converter |
| SIM Card Holder | NANO-SIM-001 | 1 | $1.50 | Molex | Push-push type |
| Enclosure | ABS-100x80x30 | 1 | $6.00 | Bud Industries | Automotive grade |
| GPS Antenna | GPS-ANT-25dB | 1 | $4.00 | Taoglas | 25dB gain, magnetic |
| GSM Antenna | GSM-ANT-2dB | 1 | $3.50 | Taoglas | 2dBi gain, adhesive |
| Wiring Harness | OBD-HARNESS | 1 | $8.00 | Custom | 16-pin to terminals |
| Fuses & Protection | Various | 1 | $3.00 | Littelfuse | Automotive grade |
| PCB | 4-layer 80x60mm | 1 | $5.00 | JLCPCB | Impedance controlled |
| Assembly & Test | Labor | 1 | $12.00 | Contract | Final assembly |
| **Total** | | | **$92.50** | | **Qty 100+ pricing** |

## Power Consumption Analysis

### Type A Node (Basic)
```
Active Mode (10 seconds/hour):
├── ESP32-S3: 160mA @ 3.3V = 528mW
├── HC-SR04: 15mA @ 5V = 75mW
└── Total Active: 603mW

Sleep Mode (3590 seconds/hour):
├── ESP32-S3: 10µA @ 3.3V = 33µW
├── HC-SR04: 0mA (powered down)
└── Total Sleep: 33µW

Daily Energy Consumption:
├── Active: 603mW × (240s/86400s) = 1.67Wh
├── Sleep: 33µW × (86160s/86400s) = 0.79Wh
└── Total: 2.46Wh/day

Battery Capacity: 2 × 3000mAh × 3.7V = 22.2Wh
Battery Life: 22.2Wh ÷ 2.46Wh/day = 9.0 days

With Solar: 2W × 4 hours = 8Wh/day
Net Consumption: 2.46 - 8 = -5.54Wh/day (surplus)
```

### Type B Node (Advanced)
```
Active Mode (AI processing 5 minutes/hour):
├── Raspberry Pi 4: 2.5A @ 5V = 12.5W
├── Pi Camera: 250mA @ 3.3V = 825mW
├── Load Cell: 10mA @ 5V = 50mW
├── LoRaWAN: 120mA @ 3.3V = 396mW
└── Total Active: 13.77W

Idle Mode (55 minutes/hour):
├── Raspberry Pi 4: 600mA @ 5V = 3W
├── Pi Camera: 0mA (powered down)
├── Load Cell: 1mA @ 5V = 5mW
├── LoRaWAN: 1.5mA @ 3.3V = 5mW
└── Total Idle: 3.01W

Daily Energy Consumption:
├── Active: 13.77W × (2h/24h) = 1.15Wh
├── Idle: 3.01W × (22h/24h) = 2.76Wh
└── Total: 3.91Wh/day

Battery Capacity: 12V × 10Ah = 120Wh
Battery Life: 120Wh ÷ 3.91Wh/day = 30.7 days

With Solar: 20W × 5 hours = 100Wh/day
Net Consumption: 3.91 - 100 = -96.09Wh/day (large surplus)
```

## Environmental Specifications

### Operating Conditions
- **Temperature**: -20°C to +60°C
- **Humidity**: 0-95% RH, non-condensing
- **IP Rating**: IP65 minimum (dust-tight, water resistant)
- **UV Resistance**: UV stabilized materials
- **Vibration**: IEC 60068-2-6 (10-500Hz, 2g)

### Compliance Standards
- **FCC Part 15**: Radio frequency emissions
- **CE Marking**: European conformity
- **RoHS**: Restriction of hazardous substances
- **WEEE**: Waste electrical equipment directive
- **UL Listed**: Safety certification for North America

## Manufacturing & Assembly

### PCB Specifications
```
Type A Node PCB:
├── Size: 50mm × 40mm
├── Layers: 2 (signal + ground)
├── Thickness: 1.6mm
├── Copper: 1oz (35µm)
├── Finish: HASL lead-free
├── Solder Mask: Green
├── Silkscreen: White
└── Via: 0.2mm minimum

Type B Node PCB:
├── Size: 80mm × 60mm
├── Layers: 4 (signal, power, ground, signal)
├── Thickness: 1.6mm
├── Copper: 2oz (70µm)
├── Finish: ENIG (gold)
├── Impedance: 50Ω ±10%
└── Via: 0.15mm minimum
```

### Assembly Process
1. **SMT Placement**: Pick-and-place for surface mount components
2. **Reflow Soldering**: Lead-free SAC305 solder paste
3. **Through-Hole**: Wave soldering for connectors
4. **Testing**: In-circuit test (ICT) + functional test
5. **Programming**: Firmware flash and calibration
6. **Enclosure**: Final assembly with gaskets and seals
7. **QC**: Final inspection and burn-in test

### Quality Control
- **Incoming Inspection**: Component verification
- **Process Control**: SPC monitoring of assembly
- **Functional Test**: 100% end-of-line testing
- **Environmental Test**: Sample testing per IEC standards
- **Reliability**: MTBF > 50,000 hours target