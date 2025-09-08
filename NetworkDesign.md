# Network Design - MEL Group 11

## 1. Project Assumptions

### 1.1 Location Details
- **Headquarters Location**: Melbourne, Victoria, Australia
- **Primary Branch Office**: Brisbane, Queensland, Australia  
- **Additional Branch Locations**: Perth (WA), Adelaide (SA), Hobart (TAS)

### 1.2 Staff Numbers
- **Headquarters (Melbourne)**: 65 staff members
  - Administrative staff: 25
  - Project management consultants: 15
  - Senior leadership team: 8
  - ICT staff: 12
  - Marketing team: 5
- **Branch Office (Brisbane)**: 25 staff members
  - Electricians: 12
  - Electrical engineers: 6
  - Site supervisors: 4
  - Support staff: 3

### 1.3 Infrastructure Assumptions
- **Building Size**: 
  - Headquarters: 3-floor building (Ground, Level 1, Level 2)
  - Branch office: Single-floor office space
- **Network Requirements**: Wired and wireless connectivity for all areas
- **Security Systems**: CCTV, IoT sensors, RFID access control integration
- **Internet Connectivity**: High-speed fiber connections at both locations
- **Redundancy**: Required for critical WAN links and core infrastructure

## 2. Network Design Overview

### 2.1 Design Philosophy
Our network design follows a hierarchical approach with three layers:
- **Core Layer**: High-speed backbone connectivity
- **Distribution Layer**: Policy enforcement and traffic aggregation  
- **Access Layer**: End-user and device connectivity

The design emphasizes:
- **Redundancy**: Multiple paths for critical connections
- **Scalability**: Easy expansion for future growth
- **Security**: Segmented networks with proper access controls
- **Performance**: High-bandwidth connections for business applications

## 3. IP Addressing Scheme

### 3.1 IP Address Requirements
- **First Octet**: 51 (last two digits of student ID 12302551)
- **Network Masks**: Only /16 and /24 subnets allowed
- **Address Range**: 51.0.0.0 - 51.255.255.255

### 3.2 IP Address Allocation

#### 3.2.1 Headquarters (Melbourne) - 51.10.0.0/16
| Network Segment | IP Range | Subnet Mask | Purpose |
|----------------|----------|-------------|---------|
| Core Network | 51.10.1.0/24 | /24 | Core switches and routers |
| Management VLAN | 51.10.2.0/24 | /24 | Network management devices |
| Server Network | 51.10.10.0/24 | /24 | Internal servers and applications |
| Ground Floor LAN | 51.10.20.0/24 | /24 | Ground floor user devices |
| Level 1 LAN | 51.10.21.0/24 | /24 | Level 1 user devices |
| Level 2 LAN | 51.10.22.0/24 | /24 | Level 2 user devices |
| WiFi Network | 51.10.30.0/24 | /24 | Wireless devices |
| Guest WiFi | 51.10.31.0/24 | /24 | Guest wireless access |
| Security Systems | 51.10.40.0/24 | /24 | CCTV, IoT sensors, RFID |
| VoIP Network | 51.10.50.0/24 | /24 | IP phones and communication |
| WAN Links | 51.10.100.0/24 | /24 | WAN connectivity to branches |

#### 3.2.2 Branch Office (Brisbane) - 51.20.0.0/16
| Network Segment | IP Range | Subnet Mask | Purpose |
|----------------|----------|-------------|---------|
| Branch Core | 51.20.1.0/24 | /24 | Branch router and core switch |
| Branch LAN | 51.20.10.0/24 | /24 | User devices and computers |
| Branch WiFi | 51.20.20.0/24 | /24 | Wireless devices |
| Branch Servers | 51.20.30.0/24 | /24 | Local servers and applications |
| WAN to HQ | 51.20.100.0/24 | /24 | WAN link to headquarters |

#### 3.2.3 Additional Branch Offices
- **Perth Branch**: 51.30.0.0/16
- **Adelaide Branch**: 51.40.0.0/16  
- **Hobart Branch**: 51.50.0.0/16

## 4. Network Architecture

### 4.1 Headquarters Network Design

#### 4.1.1 Core Layer
- **Primary Core Switch**: Cisco Catalyst 9500-48Y4C
  - 48 x 25G SFP28 ports, 4 x 100G QSFP28 ports
  - High availability with redundant power supplies
  - Advanced Layer 3 routing capabilities

- **Secondary Core Switch**: Cisco Catalyst 9500-32QC (Redundancy)
  - 32 x 100G QSFP28 ports
  - Provides backup connectivity for critical services
  - Stacked configuration with primary core switch

#### 4.1.2 Distribution Layer
- **Distribution Switch (Per Floor)**: Cisco Catalyst 9300-48P
  - 48 x 1G copper ports, 4 x 10G SFP+ uplinks
  - PoE+ support for IP phones and access points
  - Advanced security features and QoS

#### 4.1.3 Access Layer
- **Access Switches**: Cisco Catalyst 9200-24P
  - 24 x 1G copper ports, 4 x 10G SFP+ uplinks
  - PoE+ support for end devices
  - Energy efficient design

#### 4.1.4 WAN Connectivity
- **Primary WAN Router**: Cisco ISR 4431
  - Multiple WAN interfaces (fiber, ethernet)
  - VPN capabilities for branch connectivity
  - Advanced security features

- **Backup WAN Router**: Cisco ISR 4321 (Redundancy)
  - Secondary internet connection
  - Automatic failover capabilities
  - Load balancing support

### 4.2 Branch Office Network Design

#### 4.2.1 Branch Infrastructure
- **Branch Router**: Cisco ISR 4321
  - WAN connectivity to headquarters
  - Local internet breakout capability
  - VPN tunnel termination

- **Branch Switch**: Cisco Catalyst 9200-48P
  - 48 x 1G copper ports for user devices
  - 4 x 10G SFP+ uplinks to router
  - PoE+ support for IP phones and wireless APs

## 5. WiFi Network Design

### 5.1 WiFi Architecture
- **Wireless Controller**: Cisco 9800-CL (Cloud-based)
- **Access Points**: Cisco Catalyst 9120AXI (WiFi 6)
- **Coverage**: Full building coverage with 20% overlap

### 5.2 WiFi Settings and Configuration

#### 5.2.1 Headquarters WiFi
| Setting | Value | Justification |
|---------|-------|---------------|
| **SSID Names** | Truelec-Corporate, Truelec-Guest | Clear identification for different access levels |
| **Security** | WPA3-Enterprise (Corporate), WPA3-Personal (Guest) | Latest security standard |
| **Authentication** | 802.1X with RADIUS (Corporate), PSK (Guest) | Enterprise-grade authentication |
| **Encryption** | AES-256 | Strong encryption for data protection |
| **Channel Width** | 80MHz (5GHz), 20MHz (2.4GHz) | Optimal performance balance |
| **Band Selection** | 5GHz preferred, 2.4GHz fallback | Better performance on 5GHz |
| **Power Settings** | Auto (15-20dBm max) | Optimal coverage without interference |
| **Load Balancing** | Enabled (max 50 clients per AP) | Distribute load evenly |

#### 5.2.2 Access Point Placement
- **Headquarters**: 15 access points total
  - Ground Floor: 6 APs (reception, meeting rooms, open areas)
  - Level 1: 5 APs (offices, conference rooms)
  - Level 2: 4 APs (executive areas, IT room)
- **Branch Office**: 4 access points
  - Strategic placement for full coverage of office space

### 5.3 Guest Network Configuration
- **Isolation**: Guest traffic isolated from corporate network
- **Bandwidth Limiting**: 10Mbps per user maximum
- **Time Restrictions**: 8-hour session limit
- **Content Filtering**: Basic web filtering enabled
- **Captive Portal**: Terms of use acceptance required

## 6. Hardware Recommendations

### 6.1 Core Networking Equipment

#### 6.1.1 Headquarters Equipment
| Device | Model | Specifications | Quantity | Unit Price (AUD) | Total Price (AUD) |
|--------|--------|----------------|-----------|------------------|-------------------|
| **Core Switch** | Cisco Catalyst 9500-48Y4C | 48x25G + 4x100G ports, redundant PSU | 1 | $35,000 | $35,000 |
| **Backup Core Switch** | Cisco Catalyst 9500-32QC | 32x100G ports, stacking capability | 1 | $45,000 | $45,000 |
| **Distribution Switch** | Cisco Catalyst 9300-48P | 48x1G + 4x10G, PoE+, Layer 3 | 3 | $8,500 | $25,500 |
| **Access Switch** | Cisco Catalyst 9200-24P | 24x1G + 4x10G, PoE+, management | 6 | $3,200 | $19,200 |
| **WAN Router (Primary)** | Cisco ISR 4431 | Multi-service router, VPN, security | 1 | $12,000 | $12,000 |
| **WAN Router (Backup)** | Cisco ISR 4321 | Backup router, failover capability | 1 | $8,500 | $8,500 |
| **Firewall** | Cisco ASA 5516-X | Next-gen firewall, IPS, VPN | 1 | $6,500 | $6,500 |
| **Wireless Controller** | Cisco 9800-CL | Cloud-based controller, 500 AP license | 1 | $4,200 | $4,200 |
| **WiFi Access Points** | Cisco Catalyst 9120AXI | WiFi 6, 2.5G ethernet, PoE+ | 15 | $850 | $12,750 |

**Headquarters Subtotal**: $168,650 AUD

#### 6.1.2 Branch Office Equipment
| Device | Model | Specifications | Quantity | Unit Price (AUD) | Total Price (AUD) |
|--------|--------|----------------|-----------|------------------|-------------------|
| **Branch Router** | Cisco ISR 4321 | WAN connectivity, VPN, local routing | 1 | $8,500 | $8,500 |
| **Branch Switch** | Cisco Catalyst 9200-48P | 48x1G + 4x10G, PoE+, management | 1 | $3,200 | $3,200 |
| **Branch Firewall** | Cisco ASA 5508-X | Small office firewall, VPN | 1 | $3,800 | $3,800 |
| **WiFi Access Points** | Cisco Catalyst 9120AXI | WiFi 6, managed by central controller | 4 | $850 | $3,400 |

**Branch Office Subtotal**: $18,900 AUD

### 6.2 Server Infrastructure

#### 6.2.1 Headquarters Servers
| Server Type | Model | Specifications | Quantity | Unit Price (AUD) | Total Price (AUD) |
|-------------|--------|----------------|-----------|------------------|-------------------|
| **Application Server** | Dell PowerEdge R650 | 2x Intel Xeon Silver 4314, 64GB RAM, 2TB SSD | 3 | $8,500 | $25,500 |
| **Domain Controller** | Dell PowerEdge R350 | Intel Xeon E-2378G, 32GB RAM, 1TB SSD | 2 | $5,200 | $10,400 |
| **File Server** | Dell PowerEdge R750 | 2x Intel Xeon Silver 4314, 128GB RAM, 20TB HDD | 1 | $12,000 | $12,000 |
| **Backup Server** | Dell PowerEdge R350 | Intel Xeon E-2378G, 64GB RAM, 10TB HDD | 1 | $7,500 | $7,500 |

**Server Subtotal**: $55,400 AUD

#### 6.2.2 Branch Office Server
| Server Type | Model | Specifications | Quantity | Unit Price (AUD) | Total Price (AUD) |
|-------------|--------|----------------|-----------|------------------|-------------------|
| **Local Server** | Dell PowerEdge R350 | Intel Xeon E-2378G, 32GB RAM, 2TB SSD | 1 | $5,200 | $5,200 |

### 6.3 Infrastructure Equipment

#### 6.3.1 Power and Cooling
| Equipment | Model | Specifications | Quantity | Unit Price (AUD) | Total Price (AUD) |
|-----------|--------|----------------|-----------|------------------|-------------------|
| **UPS System** | APC Smart-UPS SRT 5000VA | 5kVA, online double conversion, 208V | 2 | $4,500 | $9,000 |
| **Network Rack** | APC NetShelter SX 42U | 42U server rack with cable management | 2 | $1,800 | $3,600 |
| **Patch Panels** | Panduit Mini-Com | 48-port Cat6A patch panel | 6 | $250 | $1,500 |

**Infrastructure Subtotal**: $14,100 AUD

### 6.4 Total Investment Summary
- **Headquarters Equipment**: $168,650 AUD
- **Branch Office Equipment**: $18,900 AUD
- **Server Infrastructure**: $60,600 AUD
- **Infrastructure Equipment**: $14,100 AUD

**Grand Total**: $262,250 AUD

*Note: Prices include GST and are based on current Australian market rates. Installation and configuration services not included.*

## 7. Design Justifications

### 7.1 Topology Selection
**Hierarchical Design Choice**: Selected for scalability, manageability, and performance:
- **Core Layer**: Provides high-speed backbone with redundancy
- **Distribution Layer**: Implements policies and aggregates traffic
- **Access Layer**: Connects end devices with PoE support

### 7.2 Redundancy Implementation
**Critical Path Redundancy**:
- Dual core switches with stacking for seamless failover
- Redundant WAN routers with automatic failover
- Multiple uplinks between distribution and access layers
- Redundant power supplies in all critical equipment

### 7.3 Security Segmentation
**Network Segmentation Strategy**:
- Separate subnets for different user groups and functions
- Guest network isolation from corporate resources
- Security systems on dedicated network segment
- Firewall protection at network perimeter

### 7.4 WiFi Design Rationale
**WiFi 6 Selection**:
- Future-proof technology with better performance
- Improved device density support
- Enhanced security with WPA3
- Better battery life for mobile devices

### 7.5 IP Addressing Logic
**Structured IP Scheme**:
- 51.x.x.x range meets student ID requirement
- /24 subnets provide adequate host capacity
- Logical grouping by function and location
- Room for future expansion within allocated ranges

## 8. Network Diagrams

### 8.1 Headquarters Network Topology
```
[Network Diagram to be created in draw.io - showing hierarchical design with:
- Internet connections and WAN routers
- Core switch layer with redundancy
- Distribution switches on each floor  
- Access switches connecting end devices
- WiFi access points and controller
- Server farm connectivity
- Security system integration]
```

### 8.2 Branch Office Network Topology
```
[Network Diagram to be created in draw.io - showing:
- WAN connection to headquarters
- Branch router with local internet breakout
- Single switch serving all users
- WiFi access points
- Local server connectivity
- VPN tunnel to headquarters]
```

### 8.3 WAN Connectivity Diagram
```
[Network Diagram to be created in draw.io - showing:
- Headquarters as hub
- MPLS/VPN connections to all branches
- Internet connectivity and backup paths
- Redundant connections between sites]
```

## 9. Implementation Considerations

### 9.1 Deployment Phases
1. **Phase 1**: Core infrastructure installation and configuration
2. **Phase 2**: Access layer deployment and user migration  
3. **Phase 3**: WiFi network implementation and testing
4. **Phase 4**: Security systems integration and testing
5. **Phase 5**: Branch office connectivity and final testing

### 9.2 Quality of Service (QoS)
- **Voice Traffic**: Highest priority (DSCP EF)
- **Business Applications**: High priority (DSCP AF31)
- **General Data**: Medium priority (DSCP AF21)
- **Guest Traffic**: Lowest priority (DSCP CS1)

### 9.3 Monitoring and Management
- **Network Management**: Cisco DNA Center for centralized management
- **Monitoring Tools**: SNMP monitoring for all devices
- **Backup Solutions**: Automated configuration backups
- **Documentation**: Complete network documentation and procedures

This network design provides a robust, scalable, and secure foundation for Truelec's operations while meeting all specified requirements and maintaining compliance with the project constraints.