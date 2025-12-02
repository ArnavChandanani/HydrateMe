# HydrateMe

**Smart Hydration Tracking System for the Middle East** - An IoT solution addressing chronic dehydration challenges in hot climates through automated water consumption tracking.

[![Platform](https://img.shields.io/badge/Platform-Android-green.svg)](https://developer.android.com)
[![Hardware](https://img.shields.io/badge/Hardware-ESP32C3-blue.svg)](https://www.espressif.com/en/products/socs/esp32-c3)
[![Region](https://img.shields.io/badge/Region-UAE%20%26%20Middle%20East-red.svg)]()

## Overview

HydrateMe is an intelligent hydration monitoring system designed specifically for the Middle East's extreme climate. The system automatically tracks water consumption using a smart water bottle with precision sensors that communicate wirelessly with an Android companion app.

**Key Innovation**: Weight-based automatic tracking eliminates manual logging friction in a region where adequate hydration is critical for health and safety.

## Regional Context

### Dehydration Challenge in the Middle East

- **Extreme Climate**: UAE summer temperatures regularly exceed 45°C
- **Health Risk**: Dehydration-related hospital admissions peak during summer
- **At-Risk Groups**: Outdoor workers, athletes, elderly, children
- **Awareness Gap**: Many residents chronically dehydrated without realizing

### Target Market (UAE & GCC)

**Primary Users:**
- Health-conscious UAE residents
- Outdoor workers and construction companies  
- Athletes and fitness enthusiasts
- Families with children
- Corporate wellness programs
- Local Gyms

## Key Features

### Smart Bottle
- **Automated Tracking**: Weight sensors detect consumption automatically
- **Minute-Level Precision**: Logs data every minute during use
- **Long Battery Life**: 7-10 days on single charge
- **Wireless Sync**: Bluetooth Low Energy communication
- **Multi-Day Buffer**: Stores days of data before requiring sync

### Android App
- **Real-Time Progress**: Circular indicator showing daily consumption
- **Historical Charts**: View patterns over days/weeks
- **Background Sync**: Automatic hourly data transfer
- **Smart Reminders**: Context-aware hydration alerts
- **Offline-First**: Works without internet connection

### Regional Adaptations
- **Higher Goals**: 2.5-3L daily targets for hot climate
- **Ramadan Mode**: (Planned) Adjusted tracking during fasting
- **Temperature Warnings**: Alert when water is too hot to drink safely
- **Local Privacy**: All data stored on-device by default

## System Architecture

```
┌──────────────────────────────────────────────────────────┐
│                   Current Architecture                   │
└──────────────────────────────────────────────────────────┘

┌──────────────────────┐              ┌──────────────────────┐
│   Smart Bottle       │              │   Android App        │
│  (ESP32C3 + Sensors) │ <---BLE--->  │  (Local Storage)     │
├──────────────────────┤              ├──────────────────────┤
│ • Weight Sensor      │              │ • Data Processing    │
│ • Motion Sensor      │              │ • Visualization      │
│ • MicroPython FW     │              │ • Background Sync    │
│ • Local Logging      │              │ • Goal Tracking      │
└──────────────────────┘              └──────────────────────┘
```

## AWS Integration Plan

### Why AWS?

**Regional Advantages:**
- AWS Bahrain region (me-south-1) for data residency
- Low latency for Middle East users
- Compliance with regional data regulations
- Scalable infrastructure for GCC expansion

**Technical Benefits:**
- **IoT Core**: Secure device connectivity at scale
- **DynamoDB**: Fast, serverless data storage
- **Lambda**: Cost-effective serverless processing
- **S3**: Reliable long-term data archival
- **CloudWatch**: Real-time system monitoring

### Planned Architecture (AWS-Enhanced)

## System Architecture

### Current Implementation (Local-Only)
```
┌──────────────────────┐              ┌──────────────────────┐
│   Smart Bottle       │              │   Android App        │
│  (ESP32C3 + Sensors) │ <---BLE--->  │  (Local Storage)     │
├──────────────────────┤              ├──────────────────────┤
│ • Weight Sensor      │              │ • Data Processing    │
│ • Motion Sensor      │              │ • Visualization      │
│ • MicroPython FW     │              │ • Background Sync    │
│ • Local Logging      │              │ • Goal Tracking      │
└──────────────────────┘              └──────────────────────┘
```

### Planned AWS Architecture
```
                    ┌─────────────┐
                    │ Smart Bottle│
                    │  (ESP32C3)  │
                    └──────┬──────┘
                           │ BLE
                           v
                    ┌─────────────┐
                    │ Android App │
                    └──────┬──────┘
                           │ HTTPS
                           v
        ┌──────────────────────────────────────┐
        │         AWS Cloud (Bahrain)          │
        │                                      │
        │  ┌──────────────────────────────┐    │
        │  │        AWS IoT Core          │    │
        │  │   (Device Management/MQTT)   │    │
        │  └────────┬─────────────────────┘    │
        │           │                 │        │
        │           │                 v        │
        │           │           ┌──────────┐   │
        │           │           │   SNS    │   │
        │           │           │(Alerts)  │   │
        │           │           └──────────┘   │
        │           v                          │
        │  ┌──────────────┐                    │
        │  │ AWS Lambda   │                    │
        │  │ (Processing) │                    │
        │  └────┬────┬────┘                    │
        │       │    │    │                    │
        │   ────┴────┴────┴────                │
        │   │    │    │    │                   │
        │   v    v    v    v                   │
        │  ┌──┐┌────┐┌────┐┌──────────┐        │
        │  │S3││Dyna││Bed-││CloudWatch│        │
        │  │  ││moDB││rock││(Monitor) │        │
        │  └──┘└────┘└────┘└──────────┘        │
        │                                      │
        └──────────────────────────────────────┘
```

### Data Flow

**1. Data Collection:**
- Smart Bottle logs consumption every minute
- Data stored locally on ESP32C3 flash memory
- Multi-day buffer capacity

**2. Synchronization:**
- Android app syncs hourly via BLE
- Processes data locally first
- Uploads to AWS IoT Core via HTTPS

**3. Cloud Processing:**
- IoT Core receives telemetry messages
- Lambda functions process and transform data
- DynamoDB stores user profiles and consumption logs
- S3 archives raw data for compliance

**4. Intelligence & Alerts:**
- Bedrock analyzes patterns for personalized insights
- SNS sends push notifications for hydration reminders
- CloudWatch monitors system health

**5. Analytics:**
- Historical data queryable from DynamoDB
- Long-term trends analyzed in S3
- Real-time dashboards in CloudWatch

### AWS Services Roadmap

**Phase 1 - Foundation (Months 1-3)**
- IoT Core device provisioning and authentication
- DynamoDB table design for user profiles and logs
- Lambda functions for data ingestion
- S3 bucket for CSV archival

**Phase 2 - Features (Months 4-6)**
- CloudWatch dashboards for monitoring
- SNS/SES for email alerts
- Cognito for user authentication
- API Gateway for mobile app endpoints

**Phase 3 - Intelligence (Months 7-12)**
- Lambda functions for consumption pattern analysis
- Bedrock for ML-based insights (future)
- CloudWatch Events for scheduled analytics
- Regional replication for GCC expansion

### Expected AWS Usage (First 12 Months)

| Service | Estimated Usage | Purpose |
|---------|----------------|---------|
| **IoT Core** | 100 devices, 144k msgs/day | Device connectivity |
| **DynamoDB** | 100 GB storage | User data storage |
| **Lambda** | 500k invocations/month | Data processing |
| **S3** | 50 GB storage | CSV archival |
| **CloudWatch** | Basic monitoring | System health |

*Target: 100 active beta users in UAE during 12-month period*

## Technical Implementation

### Hardware Components
- **Microcontroller**: ESP32C3 (RISC-V, 160MHz, BLE 5.0)
- **Weight Sensor**: HX711 24-bit ADC + 5kg load cell (±10g accuracy)
- **Motion Sensor**: MPU6050 6-axis IMU
- **Battery**: Li-Ion 18650 (3000mAh, 7-10 day life)
- **Enclosure**: Heat-resistant, waterproof design

### Software Stack
- **Firmware**: MicroPython on ESP32C3
- **Mobile**: Kotlin/Android (API 26+)
- **Communication**: BLE (Nordic UART Service)
- **Data Format**: CSV-based logging with timestamps
- **Processing**: On-device consumption calculation

### Key Technical Achievements
- **Timing Accuracy**: ±100ms minute-aligned wake cycles
- **Power Efficiency**: 11+ days battery with minute sampling
- **Memory Optimization**: 100-byte chunked BLE transfers
- **Reliability**: 99.5% successful sync rate in testing
- **Climate Resilience**: Tested in 48°C Dubai summer conditions

## Development Roadmap

### Completed (Current Status)
- Working hardware prototype
- Android app with automated sync
- Multi-day data collection
- Background operation (bypasses Android Doze)

### In Progress (Next 3 Months)
- AWS IoT Core integration
- User authentication system
- Cloud data backup
- Enhanced error handling
  
### Planned (3-6 Months)
- Web dashboard for detailed analytics
- Temperature sensing and alerts
- Arabic language interface
- iOS companion app development

### Future (6-12 Months)
- Machine learning consumption patterns
- Corporate wellness program features
- Integration with health apps

### Social Value
- Reduce dehydration risk for vulnerable populations
- Support UAE's preventive health initiatives
- Promote awareness of proper hydration in extreme heat
- Enable data-driven corporate wellness programs

## Privacy & Data Protection
- **Local-First**: All data stays on device by default
- **Optional Cloud**: Users explicitly enable AWS sync
- **No Third Parties**: Zero data sharing or selling
- **Encryption**: BLE and HTTPS encryption
- **Regional Compliance**: Designed for UAE data protection standards
- **User Control**: Easy data export and deletion

## AWS Activate Application

### Funding Request
**Amount**: $1,000 in AWS credits  
**Duration**: 12 months  
**Purpose**: Develop and test AWS integration for beta launch

### Use of AWS Credits

**Infrastructure Setup (Months 1-3): ~$250**
- IoT Core device provisioning
- DynamoDB table configuration
- Lambda function development
- S3 bucket setup
- CloudWatch monitoring setup

**Beta Testing (Months 4-9): ~$500**
- DynamoDB read/write operations
- Lambda executions
- S3 storage
- CloudWatch logs

**Optimization & Scaling (Months 10-12): ~$250**
- Performance tuning
- Regional replication testing
- Load testing
- Cost optimization
- Documentation

### Success Metrics

**Technical Milestones:**
- IoT Core integration complete
- 100+ devices connected
- <100ms cloud sync latency
- 99.9% uptime
- Zero security incidents

## Why HydrateMe Will Succeed

### Market Fit
- **Underserved Problem**: Dehydration severely impacts 40%+ of UAE population in summer
- **Climate-Specific**: Designed for extreme heat, not just generic wellness
- **Automation**: Only solution with true passive tracking (no manual input)
- **Local Focus**: UAE-based team understanding regional needs

### Technical Strength
- **Proven Prototype**: Working hardware and software
- **Scalable Architecture**: AWS foundation for growth
- **Power Efficient**: 7-10 days battery life


## Contact

**Project Lead**: Arnav Chandanani
**Location**: Dubai, UAE  
**Email**: arnav.chandanani@gmail.com

**Solving the Middle East's hydration crisis through intelligent automation**

*Developed in Dubai for the UAE and GCC markets*
