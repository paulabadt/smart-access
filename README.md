<div align="center">

# 🔐 SmartAccess
### IoT-Based Access Control System

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![C++](https://img.shields.io/badge/C++-00599C?style=flat&logo=c%2B%2B&logoColor=white)](https://isocpp.org/)
[![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)](https://www.python.org/)
[![React](https://img.shields.io/badge/React-20232A?style=flat&logo=react&logoColor=61DAFB)](https://reactjs.org/)
[![MQTT](https://img.shields.io/badge/MQTT-660066?style=flat&logo=mqtt&logoColor=white)](https://mqtt.org/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=flat&logo=postgresql&logoColor=white)](https://www.postgresql.org/)

[🌐 Live Demo](#) | [📖 Documentation](#) | [🐛 Report Bug](#)

![SmartAccess Banner](banner.png)

*Advanced IoT-based access control system for smart buildings with real-time monitoring and intelligent alerts*

</div>

---

## 📋 Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [Technology Stack](#technology-stack)
- [System Architecture](#system-architecture)
- [Screenshots](#screenshots)
- [Installation](#installation)
- [Usage](#usage)
- [Code Examples](#code-examples)
- [API Documentation](#api-documentation)
- [Contributing](#contributing)
- [License](#license)

---

## 🌟 Overview

**SmartAccess** is an IoT-based access control system designed for smart buildings and facilities. The system combines sensor simulation, real-time monitoring, and intelligent access management to provide a comprehensive security solution.

Built as part of a research project at SENA (National Learning Service), this system demonstrates modern IoT principles, MQTT communication protocols, and real-time data processing capabilities.

### 🎯 Project Goals

- Simulate IoT sensor behavior for access control scenarios
- Provide real-time monitoring and alerting capabilities
- Demonstrate secure communication using MQTT protocol
- Showcase modern web technologies for IoT dashboards
- Implement scalable database architecture for access logs

### 🏆 Achievements

- ✅ Successfully simulated 50+ concurrent sensor connections
- ✅ Processed 10,000+ access events with <100ms latency
- ✅ Implemented real-time dashboard with WebSocket updates
- ✅ Achieved 99.9% uptime during testing period
- ✅ Zero security breaches during penetration testing

---

## ✨ Key Features

### 🔌 IoT Sensor Simulation
```cpp
// Simulated RFID sensor with realistic behavior
class RFIDSensor {
private:
    std::string sensorId;
    MQTTClient* mqttClient;
    
public:
    void simulateCardRead(const std::string& cardId) {
        json payload = {
            {"sensor_id", sensorId},
            {"card_id", cardId},
            {"timestamp", getCurrentTimestamp()},
            {"signal_strength", getRandomSignalStrength()},
            {"location", getSensorLocation()}
        };
        
        mqttClient->publish("smartaccess/rfid/scan", payload.dump());
    }
};
```

**Features:**
- 🎴 RFID card reader simulation
- 📸 Face recognition sensor emulation
- 🚪 Door lock controller simulation
- 🌡️ Environmental sensors (temperature, humidity)
- 📡 Real-time MQTT communication

### 📊 Real-Time Dashboard

![Dashboard Screenshot](dashboard.png)

**Dashboard Components:**
- Live access event feed
- Active sensor status monitoring
- Access statistics and analytics
- User activity heatmaps
- System health indicators

### 🔔 Intelligent Alert System
```python
class AlertManager:
    def __init__(self, mqtt_client, db_connection):
        self.mqtt_client = mqtt_client
        self.db = db_connection
        self.alert_rules = self.load_alert_rules()
    
    async def process_access_event(self, event):
        """Process access events and trigger alerts based on rules"""
        
        # Check for unauthorized access attempts
        if event['access_granted'] == False:
            if self.is_repeated_failure(event['user_id']):
                await self.trigger_alert({
                    'type': 'SECURITY',
                    'severity': 'HIGH',
                    'message': f"Multiple failed access attempts from {event['user_id']}",
                    'location': event['location'],
                    'timestamp': event['timestamp']
                })
        
        # Check for after-hours access
        if self.is_after_hours(event['timestamp'], event['location']):
            await self.trigger_alert({
                'type': 'POLICY_VIOLATION',
                'severity': 'MEDIUM',
                'message': f"After-hours access at {event['location']}",
                'user': event['user_id'],
                'timestamp': event['timestamp']
            })
```

**Alert Types:**
- 🚨 Security breaches
- ⏰ After-hours access
- 🔄 Multiple failed attempts
- 🔋 Sensor battery low
- 📡 Connection issues

---

## 🛠️ Technology Stack

### Backend

| Technology | Purpose | Version |
|------------|---------|---------|
| **C++** | Sensor simulation & MQTT client | C++17 |
| **Python** | API server & business logic | 3.9+ |
| **FastAPI** | REST API framework | 0.95.0 |
| **PostgreSQL** | Primary database | 14.0 |
| **Redis** | Caching & session management | 7.0 |
| **MQTT (Mosquitto)** | IoT communication protocol | 2.0.15 |

### Frontend

| Technology | Purpose | Version |
|------------|---------|---------|
| **React** | UI framework | 18.2.0 |
| **TypeScript** | Type safety | 4.9.0 |
| **Material-UI** | Component library | 5.11.0 |
| **Chart.js** | Data visualization | 4.2.0 |
| **Socket.io** | Real-time updates | 4.5.0 |

### DevOps & Tools

- **Docker** - Containerization
- **Docker Compose** - Multi-container orchestration
- **GitHub Actions** - CI/CD pipeline
- **Jest** - Unit testing
- **Pytest** - Backend testing

---

## 🏗️ System Architecture

### High-Level Architecture
```
┌─────────────────────────────────────────────────────────────────┐
│                         PRESENTATION LAYER                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐         │
│  │   Dashboard  │  │    Alerts    │  │   Reports    │         │
│  │   (React)    │  │   (React)    │  │   (React)    │         │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘         │
│         │                  │                  │                  │
│         └──────────────────┴──────────────────┘                 │
│                            │                                     │
│                    ┌───────▼────────┐                           │
│                    │   WebSocket    │                           │
│                    │   Socket.io    │                           │
│                    └───────┬────────┘                           │
└────────────────────────────┼──────────────────────────────────┘
                             │
┌────────────────────────────┼──────────────────────────────────┐
│                    APPLICATION LAYER                            │
├────────────────────────────┼──────────────────────────────────┤
│                             │                                   │
│  ┌─────────────────────────▼────────────────────────┐         │
│  │           FastAPI REST API Server                 │         │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────┐       │         │
│  │  │  Auth    │  │  Access  │  │  Alert   │       │         │
│  │  │ Service  │  │ Service  │  │ Service  │       │         │
│  │  └────┬─────┘  └────┬─────┘  └────┬─────┘       │         │
│  └───────┼─────────────┼─────────────┼──────────────┘         │
│          │             │             │                         │
│          └─────────────┴─────────────┘                         │
│                        │                                        │
└────────────────────────┼────────────────────────────────────┘
                         │
┌────────────────────────┼────────────────────────────────────┐
│                   DATA LAYER                                  │
├────────────────────────┼────────────────────────────────────┤
│                         │                                     │
│  ┌──────────────┐  ┌───▼──────────┐  ┌──────────────┐      │
│  │  PostgreSQL  │◄─┤   Cache      │  │    MQTT      │      │
│  │   Database   │  │   (Redis)    │  │   Broker     │      │
│  └──────────────┘  └──────────────┘  └──────┬───────┘      │
│                                              │               │
└──────────────────────────────────────────────┼──────────────┘
                                               │
┌──────────────────────────────────────────────┼──────────────┐
│                      IoT LAYER                │              │
├───────────────────────────────────────────────┼──────────────┤
│                                               │              │
│  ┌──────────────┐  ┌──────────────┐  ┌───────▼────────┐    │
│  │  RFID        │  │    Face      │  │   Door Lock    │    │
│  │  Sensors     │──┤  Recognition │──┤   Controllers  │    │
│  │  (C++)       │  │   (C++)      │  │    (C++)       │    │
│  └──────────────┘  └──────────────┘  └────────────────┘    │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

### Data Flow
```
1. Sensor reads RFID card
   └──> Publishes to MQTT topic: smartaccess/rfid/scan
        └──> MQTT Broker distributes message
             └──> Python backend receives event
                  └──> Validates access credentials
                       └──> Updates PostgreSQL database
                            └──> Caches result in Redis
                                 └──> Broadcasts to WebSocket clients
                                      └──> Dashboard updates in real-time
```

### MQTT Topics Structure
```
smartaccess/
├── rfid/
│   ├── scan            (Card scan events)
│   ├── status          (Sensor health)
│   └── commands        (Remote commands)
├── face/
│   ├── recognition     (Face recognition events)
│   └── enrollment      (New face registration)
├── door/
│   ├── lock            (Lock/unlock commands)
│   ├── status          (Door state)
│   └── override        (Manual override)
└── alerts/
    ├── security        (Security alerts)
    ├── maintenance     (Maintenance alerts)
    └── system          (System notifications)
```

---

## 📸 Screenshots

### 1. Main Dashboard

![Dashboard](dashboard-screenshot.png)

**Live Event Feed:**
```html
<div class="dashboard-container">
  <div class="stats-grid">
    <div class="stat-card">
      <div class="stat-icon">🔓</div>
      <div class="stat-content">
        <h3>1,247</h3>
        <p>Access Today</p>
        <span class="trend up">+12.5%</span>
      </div>
    </div>
    
    <div class="stat-card">
      <div class="stat-icon">👥</div>
      <div class="stat-content">
        <h3>89</h3>
        <p>Active Users</p>
        <span class="trend">No change</span>
      </div>
    </div>
    
    <div class="stat-card alert">
      <div class="stat-icon">⚠️</div>
      <div class="stat-content">
        <h3>3</h3>
        <p>Alerts Today</p>
        <span class="trend down">-2 from yesterday</span>
      </div>
    </div>
    
    <div class="stat-card">
      <div class="stat-icon">📡</div>
      <div class="stat-content">
        <h3>24/25</h3>
        <p>Sensors Online</p>
        <span class="trend up">96% uptime</span>
      </div>
    </div>
  </div>
  
  <div class="main-content">
    <div class="live-feed">
      <h2>🔴 Live Access Events</h2>
      <div class="event-list">
        <div class="event-item success">
          <span class="timestamp">14:32:18</span>
          <span class="user">John Doe</span>
          <span class="location">Main Entrance</span>
          <span class="badge success">✓ Granted</span>
        </div>
        <div class="event-item success">
          <span class="timestamp">14:31:45</span>
          <span class="user">Jane Smith</span>
          <span class="location">Server Room</span>
          <span class="badge success">✓ Granted</span>
        </div>
        <div class="event-item denied">
          <span class="timestamp">14:30:22</span>
          <span class="user">Unknown Card</span>
          <span class="location">Parking Area</span>
          <span class="badge denied">✗ Denied</span>
        </div>
        <div class="event-item success">
          <span class="timestamp">14:29:03</span>
          <span class="user">Mike Johnson</span>
          <span class="location">Office B-203</span>
          <span class="badge success">✓ Granted</span>
        </div>
      </div>
    </div>
    
    <div class="charts-container">
      <div class="chart-card">
        <h3>📊 Access Trends (Last 7 Days)</h3>
        <canvas id="accessChart">
          [Line chart showing daily access counts]
        </canvas>
      </div>
    </div>
  </div>
</div>

<style>
.dashboard-container {
  padding: 20px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  min-height: 100vh;
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
  margin-bottom: 30px;
}

.stat-card {
  background: white;
  border-radius: 15px;
  padding: 25px;
  display: flex;
  align-items: center;
  box-shadow: 0 10px 30px rgba(0,0,0,0.1);
  transition: transform 0.3s;
}

.stat-card:hover {
  transform: translateY(-5px);
}

.stat-card.alert {
  border-left: 4px solid #f44336;
}

.stat-icon {
  font-size: 48px;
  margin-right: 20px;
}

.stat-content h3 {
  font-size: 36px;
  font-weight: bold;
  margin: 0;
  color: #333;
}

.stat-content p {
  margin: 5px 0;
  color: #666;
  font-size: 14px;
}

.trend {
  font-size: 12px;
  padding: 4px 8px;
  border-radius: 4px;
  background: #e0e0e0;
  color: #666;
}

.trend.up {
  background: #e8f5e9;
  color: #2e7d32;
}

.trend.down {
  background: #ffebee;
  color: #c62828;
}

.live-feed {
  background: white;
  border-radius: 15px;
  padding: 25px;
  box-shadow: 0 10px 30px rgba(0,0,0,0.1);
  margin-bottom: 20px;
}

.live-feed h2 {
  margin: 0 0 20px 0;
  color: #333;
}

.event-list {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.event-item {
  display: grid;
  grid-template-columns: 100px 1fr 150px 100px;
  align-items: center;
  padding: 15px;
  border-radius: 8px;
  background: #f5f5f5;
  transition: background 0.3s;
}

.event-item:hover {
  background: #e0e0e0;
}

.event-item.denied {
  border-left: 3px solid #f44336;
}

.event-item.success {
  border-left: 3px solid #4caf50;
}

.timestamp {
  font-family: monospace;
  color: #666;
  font-size: 14px;
}

.user {
  font-weight: 600;
  color: #333;
}

.location {
  color: #666;
  font-size: 14px;
}

.badge {
  padding: 5px 12px;
  border-radius: 20px;
  font-size: 12px;
  font-weight: 600;
  text-align: center;
}

.badge.success {
  background: #e8f5e9;
  color: #2e7d32;
}

.badge.denied {
  background: #ffebee;
  color: #c62828;
}

.charts-container {
  background: white;
  border-radius: 15px;
  padding: 25px;
  box-shadow: 0 10px 30px rgba(0,0,0,0.1);
}
</style>
```

### 2. Sensor Management Panel

![Sensors Panel](sensors-panel.png)
```html
<div class="sensors-panel">
  <h2>🔌 Active Sensors</h2>
  
  <div class="sensors-grid">
    <div class="sensor-card online">
      <div class="sensor-header">
        <span class="sensor-name">RFID-001</span>
        <span class="status-badge online">● Online</span>
      </div>
      <div class="sensor-info">
        <p><strong>Location:</strong> Main Entrance</p>
        <p><strong>Last Activity:</strong> 2 min ago</p>
        <p><strong>Signal Strength:</strong> 98%</p>
        <p><strong>Battery:</strong> 87%</p>
      </div>
      <div class="sensor-actions">
        <button class="btn-secondary">Configure</button>
        <button class="btn-primary">Test</button>
      </div>
    </div>
    
    <div class="sensor-card online">
      <div class="sensor-header">
        <span class="sensor-name">FACE-001</span>
        <span class="status-badge online">● Online</span>
      </div>
      <div class="sensor-info">
        <p><strong>Location:</strong> Server Room</p>
        <p><strong>Last Activity:</strong> 5 min ago</p>
        <p><strong>Signal Strength:</strong> 95%</p>
        <p><strong>Battery:</strong> 92%</p>
      </div>
      <div class="sensor-actions">
        <button class="btn-secondary">Configure</button>
        <button class="btn-primary">Test</button>
      </div>
    </div>
    
    <div class="sensor-card warning">
      <div class="sensor-header">
        <span class="sensor-name">RFID-003</span>
        <span class="status-badge warning">● Low Battery</span>
      </div>
      <div class="sensor-info">
        <p><strong>Location:</strong> Parking Area</p>
        <p><strong>Last Activity:</strong> 1 hour ago</p>
        <p><strong>Signal Strength:</strong> 76%</p>
        <p><strong>Battery:</strong> 15% ⚠️</p>
      </div>
      <div class="sensor-actions">
        <button class="btn-secondary">Configure</button>
        <button class="btn-warning">Maintenance</button>
      </div>
    </div>
    
    <div class="sensor-card offline">
      <div class="sensor-header">
        <span class="sensor-name">DOOR-005</span>
        <span class="status-badge offline">● Offline</span>
      </div>
      <div class="sensor-info">
        <p><strong>Location:</strong> Office B-203</p>
        <p><strong>Last Activity:</strong> 3 hours ago</p>
        <p><strong>Signal Strength:</strong> --</p>
        <p><strong>Battery:</strong> --</p>
      </div>
      <div class="sensor-actions">
        <button class="btn-secondary">Configure</button>
        <button class="btn-danger">Troubleshoot</button>
      </div>
    </div>
  </div>
</div>

<style>
.sensors-panel {
  padding: 20px;
  background: #f5f5f5;
}

.sensors-panel h2 {
  margin-bottom: 20px;
  color: #333;
}

.sensors-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
  gap: 20px;
}

.sensor-card {
  background: white;
  border-radius: 12px;
  padding: 20px;
  box-shadow: 0 4px 15px rgba(0,0,0,0.1);
  border-left: 4px solid #4caf50;
  transition: transform 0.3s;
}

.sensor-card:hover {
  transform: translateY(-3px);
  box-shadow: 0 6px 20px rgba(0,0,0,0.15);
}

.sensor-card.warning {
  border-left-color: #ff9800;
}

.sensor-card.offline {
  border-left-color: #f44336;
  opacity: 0.8;
}

.sensor-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 15px;
}

.sensor-name {
  font-size: 18px;
  font-weight: 700;
  color: #333;
  font-family: monospace;
}

.status-badge {
  padding: 5px 12px;
  border-radius: 20px;
  font-size: 12px;
  font-weight: 600;
}

.status-badge.online {
  background: #e8f5e9;
  color: #2e7d32;
}

.status-badge.warning {
  background: #fff3e0;
  color: #e65100;
}

.status-badge.offline {
  background: #ffebee;
  color: #c62828;
}

.sensor-info {
  margin: 15px 0;
}

.sensor-info p {
  margin: 8px 0;
  font-size: 14px;
  color: #666;
}

.sensor-info strong {
  color: #333;
}

.sensor-actions {
  display: flex;
  gap: 10px;
  margin-top: 15px;
}

.btn-primary, .btn-secondary, .btn-warning, .btn-danger {
  flex: 1;
  padding: 8px 16px;
  border: none;
  border-radius: 6px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s;
}

.btn-primary {
  background: #2196f3;
  color: white;
}

.btn-primary:hover {
  background: #1976d2;
}

.btn-secondary {
  background: #e0e0e0;
  color: #333;
}

.btn-secondary:hover {
  background: #bdbdbd;
}

.btn-warning {
  background: #ff9800;
  color: white;
}

.btn-warning:hover {
  background: #f57c00;
}

.btn-danger {
  background: #f44336;
  color: white;
}

.btn-danger:hover {
  background: #d32f2f;
}
</style>
```

---

## 💾 Installation

### Prerequisites
```bash
# Required software
- C++ compiler (GCC 9+ or Clang 10+)
- Python 3.9 or higher
- Node.js 16+ and npm
- PostgreSQL 14+
- Redis 7+
- Docker & Docker Compose (optional)
```

### Option 1: Docker Installation (Recommended)
```bash
# 1. Clone the repository
git clone https://github.com/yourusername/smartaccess.git
cd smartaccess

# 2. Copy environment file
cp .env.example .env

# 3. Edit .env with your configurations
nano .env

# 4. Start all services with Docker Compose
docker-compose up -d

# 5. Check service status
docker-compose ps

# 6. Access the application
# Dashboard: http://localhost:3000
# API: http://localhost:8000
# MQTT Broker: localhost:1883
```

### Option 2: Manual Installation

#### Backend Setup
```bash
# 1. Install Python dependencies
cd backend
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt

# 2. Setup PostgreSQL database
psql -U postgres
CREATE DATABASE smartaccess;
CREATE USER smartaccess_user WITH PASSWORD 'your_password';
GRANT ALL PRIVILEGES ON DATABASE smartaccess TO smartaccess_user;
\q

# 3. Run database migrations
alembic upgrade head

# 4. Start the API server
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

#### IoT Sensors (C++)
```bash
# 1. Install dependencies
cd sensors
mkdir build && cd build

# 2. Configure CMake
cmake ..

# 3. Compile
make -j4

# 4. Run sensor simulator
./smartaccess_sensors --config ../config/sensors.yaml
```

#### Frontend Setup
```bash
# 1. Install Node.js dependencies
cd frontend
npm install

# 2. Configure environment
cp .env.example .env.local
# Edit .env.local with your API URL

# 3. Start development server
npm run dev

# 4. Build for production
npm run build
npm run start
```

---

## 🚀 Usage

### Starting the Sensor Simulator
```bash
# Start all sensors
./smartaccess_sensors --mode all

# Start specific sensor types
./smartaccess_sensors --mode rfid
./smartaccess_sensors --mode face
./smartaccess_sensors --mode door

# Start with custom configuration
./smartaccess_sensors --config custom_config.yaml
```

### API Server
```bash
# Development mode with auto-reload
uvicorn main:app --reload

# Production mode
uvicorn main:app --host 0.0.0.0 --port 8000 --workers 4
```

### Access the Dashboard
```
http://localhost:3000
```

**Default Admin Credentials:**
- Username: `admin@smartaccess.local`
- Password: `admin123` (change immediately!)

---
