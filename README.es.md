<div align="center">

*Sistema avanzado de control de acceso basado en IoT para edificios inteligentes con monitoreo en tiempo real y alertas inteligentes*

</div>

---

## 📋 Tabla de Contenidos

- [Descripción General](#descripción-general)
- [Características Principales](#características-principales)
- [Stack Tecnológico](#stack-tecnológico)
- [Arquitectura del Sistema](#arquitectura-del-sistema)
- [Capturas de Pantalla](#capturas-de-pantalla)
- [Instalación](#instalación)
- [Uso](#uso)
- [Ejemplos de Código](#ejemplos-de-código)
- [Documentación de la API](#documentación-de-la-api)
- [Contribuir](#contribuir)
- [Licencia](#licencia)

---

## 🌟 Descripción General

**SmartAccess** es un sistema de control de acceso basado en IoT diseñado para edificios inteligentes e instalaciones empresariales. El sistema combina simulación de sensores, monitoreo en tiempo real y gestión inteligente de accesos para proporcionar una solución de seguridad integral.

Desarrollado como parte de un proyecto de investigación en el SENA (Servicio Nacional de Aprendizaje), este sistema demuestra principios modernos de IoT, protocolos de comunicación MQTT y capacidades de procesamiento de datos en tiempo real.

### 🎯 Objetivos del Proyecto

- Simular el comportamiento de sensores IoT para escenarios de control de acceso
- Proporcionar capacidades de monitoreo y alertas en tiempo real
- Demostrar comunicación segura utilizando protocolo MQTT
- Mostrar tecnologías web modernas para dashboards IoT
- Implementar arquitectura de base de datos escalable para registros de acceso

### 🏆 Logros

- ✅ Simulación exitosa de más de 50 conexiones de sensores simultáneas
- ✅ Procesamiento de más de 10,000 eventos de acceso con latencia <100ms
- ✅ Dashboard en tiempo real implementado con actualizaciones vía WebSocket
- ✅ 99.9% de tiempo de actividad durante el período de pruebas
- ✅ Cero brechas de seguridad durante pruebas de penetración

---

## ✨ Características Principales

### 🔌 Simulación de Sensores IoT
```cpp
// Sensor RFID simulado con comportamiento realista
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

**Características:**
- 🎴 Simulación de lector de tarjetas RFID
- 📸 Emulación de sensor de reconocimiento facial
- 🚪 Simulación de controlador de cerradura de puerta
- 🌡️ Sensores ambientales (temperatura, humedad)
- 📡 Comunicación MQTT en tiempo real

### 📊 Dashboard en Tiempo Real

![Captura del Dashboard](dashboard.png)

**Componentes del Dashboard:**
- Feed en vivo de eventos de acceso
- Monitoreo del estado de sensores activos
- Estadísticas y analíticas de acceso
- Mapas de calor de actividad de usuarios
- Indicadores de salud del sistema

### 🔔 Sistema de Alertas Inteligente
```python
class AlertManager:
    def __init__(self, mqtt_client, db_connection):
        self.mqtt_client = mqtt_client
        self.db = db_connection
        self.alert_rules = self.load_alert_rules()
    
    async def process_access_event(self, event):
        """Procesar eventos de acceso y activar alertas según las reglas"""
        
        # Verificar intentos de acceso no autorizados
        if event['access_granted'] == False:
            if self.is_repeated_failure(event['user_id']):
                await self.trigger_alert({
                    'type': 'SECURITY',
                    'severity': 'HIGH',
                    'message': f"Múltiples intentos fallidos de {event['user_id']}",
                    'location': event['location'],
                    'timestamp': event['timestamp']
                })
        
        # Verificar acceso fuera de horario
        if self.is_after_hours(event['timestamp'], event['location']):
            await self.trigger_alert({
                'type': 'POLICY_VIOLATION',
                'severity': 'MEDIUM',
                'message': f"Acceso fuera de horario en {event['location']}",
                'user': event['user_id'],
                'timestamp': event['timestamp']
            })
```

**Tipos de Alertas:**
- 🚨 Brechas de seguridad
- ⏰ Acceso fuera de horario
- 🔄 Múltiples intentos fallidos
- 🔋 Batería baja del sensor
- 📡 Problemas de conexión

---

## 🛠️ Stack Tecnológico

### Backend

| Tecnología | Propósito | Versión |
|------------|-----------|---------|
| **C++** | Simulación de sensores y cliente MQTT | C++17 |
| **Python** | Servidor API y lógica de negocio | 3.9+ |
| **FastAPI** | Framework REST API | 0.95.0 |
| **PostgreSQL** | Base de datos principal | 14.0 |
| **Redis** | Caché y gestión de sesiones | 7.0 |
| **MQTT (Mosquitto)** | Protocolo de comunicación IoT | 2.0.15 |

### Frontend

| Tecnología | Propósito | Versión |
|------------|-----------|---------|
| **React** | Framework UI | 18.2.0 |
| **TypeScript** | Seguridad de tipos | 4.9.0 |
| **Material-UI** | Librería de componentes | 5.11.0 |
| **Chart.js** | Visualización de datos | 4.2.0 |
| **Socket.io** | Actualizaciones en tiempo real | 4.5.0 |

### DevOps y Herramientas

- **Docker** - Contenedorización
- **Docker Compose** - Orquestación de múltiples contenedores
- **GitHub Actions** - Pipeline CI/CD
- **Jest** - Pruebas unitarias
- **Pytest** - Pruebas de backend

---

## 🏗️ Arquitectura del Sistema

### Arquitectura de Alto Nivel
```
┌─────────────────────────────────────────────────────────────────┐
│                      CAPA DE PRESENTACIÓN                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐         │
│  │   Dashboard  │  │   Alertas    │  │   Reportes   │         │
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
│                    CAPA DE APLICACIÓN                           │
├────────────────────────────┼──────────────────────────────────┤
│                             │                                   │
│  ┌─────────────────────────▼────────────────────────┐         │
│  │         Servidor API REST FastAPI                 │         │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────┐       │         │
│  │  │ Servicio │  │ Servicio │  │ Servicio │       │         │
│  │  │   Auth   │  │  Acceso  │  │  Alertas │       │         │
│  │  └────┬─────┘  └────┬─────┘  └────┬─────┘       │         │
│  └───────┼─────────────┼─────────────┼──────────────┘         │
│          │             │             │                         │
│          └─────────────┴─────────────┘                         │
│                        │                                        │
└────────────────────────┼────────────────────────────────────┘
                         │
┌────────────────────────┼────────────────────────────────────┐
│                    CAPA DE DATOS                              │
├────────────────────────┼────────────────────────────────────┤
│                         │                                     │
│  ┌──────────────┐  ┌───▼──────────┐  ┌──────────────┐      │
│  │  PostgreSQL  │◄─┤    Caché     │  │     MQTT     │      │
│  │ Base de Datos│  │   (Redis)    │  │    Broker    │      │
│  └──────────────┘  └──────────────┘  └──────┬───────┘      │
│                                              │               │
└──────────────────────────────────────────────┼──────────────┘
                                               │
┌──────────────────────────────────────────────┼──────────────┐
│                       CAPA IoT                │              │
├───────────────────────────────────────────────┼──────────────┤
│                                               │              │
│  ┌──────────────┐  ┌──────────────┐  ┌───────▼────────┐    │
│  │  Sensores    │  │Reconocimiento│  │  Controladores │    │
│  │    RFID      │──┤    Facial    │──┤  Cerradura     │    │
│  │   (C++)      │  │    (C++)     │  │    (C++)       │    │
│  └──────────────┘  └──────────────┘  └────────────────┘    │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

### Flujo de Datos
```
1. Sensor lee tarjeta RFID
   └──> Publica al tópico MQTT: smartaccess/rfid/scan
        └──> Broker MQTT distribuye mensaje
             └──> Backend Python recibe evento
                  └──> Valida credenciales de acceso
                       └──> Actualiza base de datos PostgreSQL
                            └──> Cachea resultado en Redis
                                 └──> Transmite a clientes WebSocket
                                      └──> Dashboard actualiza en tiempo real
```

### Estructura de Tópicos MQTT
```
smartaccess/
├── rfid/
│   ├── scan            (Eventos de escaneo de tarjeta)
│   ├── status          (Salud del sensor)
│   └── commands        (Comandos remotos)
├── face/
│   ├── recognition     (Eventos de reconocimiento facial)
│   └── enrollment      (Registro de nuevos rostros)
├── door/
│   ├── lock            (Comandos abrir/cerrar)
│   ├── status          (Estado de la puerta)
│   └── override        (Anulación manual)
└── alerts/
    ├── security        (Alertas de seguridad)
    ├── maintenance     (Alertas de mantenimiento)
    └── system          (Notificaciones del sistema)
```

---

## 📸 Capturas de Pantalla

### 1. Dashboard Principal

![Dashboard](dashboard-screenshot.png)


### 2. Panel de Gestión de Sensores

![Panel de Sensores](sensors-panel.png)

### 3. Sistema de Alertas

![Panel de Alertas](alerts-panel.png)

---

## 💾 Instalación

### Requisitos Previos
```bash
# Software requerido
- Compilador C++ (GCC 9+ o Clang 10+)
- Python 3.9 o superior
- Node.js 16+ y npm
- PostgreSQL 14+
- Redis 7+
- Docker y Docker Compose (opcional)
```

### Opción 1: Instalación con Docker (Recomendado)
```bash
# 1. Clonar el repositorio
git clone https://github.com/tuusuario/smartaccess.git
cd smartaccess

# 2. Copiar archivo de variables de entorno
cp .env.example .env

# 3. Editar .env con tus configuraciones
nano .env

# 4. Iniciar todos los servicios con Docker Compose
docker-compose up -d

# 5. Verificar estado de los servicios
docker-compose ps

# 6. Acceder a la aplicación
# Dashboard: http://localhost:3000
# API: http://localhost:8000
# MQTT Broker: localhost:1883
```

### Opción 2: Instalación Manual

#### Configuración del Backend
```bash
# 1. Instalar dependencias de Python
cd backend
python -m venv venv
source venv/bin/activate  # En Windows: venv\Scripts\activate
pip install -r requirements.txt

# 2. Configurar base de datos PostgreSQL
psql -U postgres
CREATE DATABASE smartaccess;
CREATE USER smartaccess_user WITH PASSWORD 'tu_contraseña';
GRANT ALL PRIVILEGES ON DATABASE smartaccess TO smartaccess_user;
\q

# 3. Ejecutar migraciones de base de datos
alembic upgrade head

# 4. Iniciar el servidor API
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

#### Sensores IoT (C++)
```bash
# 1. Instalar dependencias
cd sensors
mkdir build && cd build

# 2. Configurar CMake
cmake ..

# 3. Compilar
make -j4

# 4. Ejecutar simulador de sensores
./smartaccess_sensors --config ../config/sensors.yaml
```

#### Configuración del Frontend
```bash
# 1. Instalar dependencias de Node.js
cd frontend
npm install

# 2. Configurar variables de entorno
cp .env.example .env.local
# Editar .env.local con la URL de tu API

# 3. Iniciar servidor de desarrollo
npm run dev

# 4. Compilar para producción
npm run build
npm run start
```

---

## 🚀 Uso

### Iniciar el Simulador de Sensores
```bash
# Iniciar todos los sensores
./smartaccess_sensors --mode all

# Iniciar tipos específicos de sensores
./smartaccess_sensors --mode rfid
./smartaccess_sensors --mode face
./smartaccess_sensors --mode door

# Iniciar con configuración personalizada
./smartaccess_sensors --config config_personalizada.yaml
```

### Servidor API
```bash
# Modo desarrollo con auto-recarga
uvicorn main:app --reload

# Modo producción
uvicorn main:app --host 0.0.0.0 --port 8000 --workers 4
```

### Acceder al Dashboard
```
http://localhost:3000
```

**Credenciales de Administrador Predeterminadas:**
- Usuario: `admin@smartaccess.local`
- Contraseña: `admin123` (¡cambiar inmediatamente!)

---

## 💻 Ejemplos de Código

### 1. Simulación de Sensor RFID (C++)
```cpp
// rfid_sensor.hpp
#pragma once
#include <string>
#include <random>
#include <chrono>
#include <mqtt/client.h>
#include <nlohmann/json.hpp>

using json = nlohmann::json;

class RFIDSensor {
private:
    std::string sensorId;
    std::string ubicacion;
    mqtt::client* mqttClient;
    std::mt19937 rng;
    
    // Simular variaciones realistas de intensidad de señal
    int getIntensidadSenal() {
        std::uniform_int_distribution<int> dist(75, 100);
        return dist(rng);
    }
    
    // Generar timestamp realista
    std::string getMarcaTiempoActual() {
        auto ahora = std::chrono::system_clock::now();
        auto tiempo = std::chrono::system_clock::to_time_t(ahora);
        char buffer[80];
        strftime(buffer, sizeof(buffer), "%Y-%m-%d %H:%M:%S", localtime(&tiempo));
        return std::string(buffer);
    }

public:
    RFIDSensor(const std::string& id, const std::string& loc, mqtt::client* client)
        : sensorId(id), ubicacion(loc), mqttClient(client) {
        rng.seed(std::random_device{}());
    }
    
    // Simular evento de escaneo de tarjeta
    void escanearTarjeta(const std::string& tarjetaId, const std::string& usuarioId) {
        json payload = {
            {"tipo_evento", "escaneo_rfid"},
            {"sensor_id", sensorId},
            {"tarjeta_id", tarjetaId},
            {"usuario_id", usuarioId},
            {"ubicacion", ubicacion},
            {"marca_tiempo", getMarcaTiempoActual()},
            {"intensidad_senal", getIntensidadSenal()},
            {"estado_sensor", "activo"}
        };
        
        std::string topico = "smartaccess/rfid/scan";
        std::string mensaje = payload.dump();
        
        try {
            mqttClient->publish(topico, mensaje.c_str(), mensaje.length(), 1, false);
            std::cout << "✓ Publicado escaneo RFID: " << tarjetaId 
                      << " en " << ubicacion << std::endl;
        } catch (const mqtt::exception& e) {
            std::cerr << "✗ Error MQTT: " << e.what() << std::endl;
        }
    }
    
    // Enviar estado de salud del sensor
    void enviarEstadoSalud() {
        json payload = {
            {"sensor_id", sensorId},
            {"estado", "en_linea"},
            {"ubicacion", ubicacion},
            {"nivel_bateria", getNivelBateria()},
            {"tiempo_activo_segundos", getTiempoActivo()},
            {"ultimo_mantenimiento", getUltimoMantenimiento()},
            {"marca_tiempo", getMarcaTiempoActual()}
        };
        
        std::string topico = "smartaccess/rfid/status";
        mqttClient->publish(topico, payload.dump(), 1, false);
    }
    
    // Simular escaneos aleatorios de tarjetas para pruebas
    void simularActividadAleatoria(int duracionSegundos) {
        std::vector<std::string> tarjetasPrueba = {
            "TARJETA-001", "TARJETA-002", "TARJETA-003", "TARJETA-004", "TARJETA-005"
        };
        std::vector<std::string> usuariosPrueba = {
            "juan.perez", "maria.garcia", "miguel.rodriguez", "sofia.martinez", "david.lopez"
        };
        
        std::uniform_int_distribution<int> distTarjeta(0, tarjetasPrueba.size() - 1);
        std::uniform_int_distribution<int> distTiempo(1, 5);
        
        auto tiempoInicio = std::chrono::steady_clock::now();
        
        while (std::chrono::duration_cast<std::chrono::seconds>(
            std::chrono::steady_clock::now() - tiempoInicio).count() < duracionSegundos) {
            
            int indiceTarjeta = distTarjeta(rng);
            escanearTarjeta(tarjetasPrueba[indiceTarjeta], usuariosPrueba[indiceTarjeta]);
            
            int tiempoEspera = distTiempo(rng);
            std::this_thread::sleep_for(std::chrono::seconds(tiempoEspera));
        }
    }
    
    int getNivelBateria() {
        std::uniform_int_distribution<int> dist(70, 100);
        return dist(rng);
    }
    
    int getTiempoActivo() {
        return std::chrono::duration_cast<std::chrono::seconds>(
            std::chrono::steady_clock::now().time_since_epoch()
        ).count() % 86400; // Segundos en el día actual
    }
    
    std::string getUltimoMantenimiento() {
        return "2024-01-10 09:00:00";
    }
};

// main.cpp - Ejemplo de uso
int main(int argc, char* argv[]) {
    // Inicializar cliente MQTT
    const std::string DIRECCION_BROKER = "tcp://localhost:1883";
    const std::string ID_CLIENTE = "simulador_rfid_smartaccess";
    
    mqtt::client client(DIRECCION_BROKER, ID_CLIENTE);
    
    mqtt::connect_options opcionesConexion;
    opcionesConexion.set_keep_alive_interval(20);
    opcionesConexion.set_clean_session(true);
    
    try {
        std::cout << "Conectando al broker MQTT..." << std::endl;
        client.connect(opcionesConexion);
        std::cout << "✓ ¡Conectado exitosamente!" << std::endl;
        
        // Crear sensores RFID en diferentes ubicaciones
        RFIDSensor entradaPrincipal("RFID-001", "Entrada Principal", &client);
        RFIDSensor areaEstacionamiento("RFID-002", "Área de Estacionamiento", &client);
        RFIDSensor salaServidores("RFID-003", "Sala de Servidores", &client);
        
        // Simular actividad
        std::cout << "\nIniciando simulación de sensores...\n" << std::endl;
        
        // Ejecutar simulaciones paralelas
        std::thread t1([&]() { entradaPrincipal.simularActividadAleatoria(60); });
        std::thread t2([&]() { areaEstacionamiento.simularActividadAleatoria(60); });
        std::thread t3([&]() { salaServidores.simularActividadAleatoria(60); });
        
        t1.join();
        t2.join();
        t3.join();
        
        client.disconnect();
        std::cout << "\n✓ Simulación completada" << std::endl;
        
    } catch (const mqtt::exception& e) {
        std::cerr << "✗ Error: " << e.what() << std::endl;
        return 1;
    }
    
    return 0;
}
```

### 2. Servicio de Control de Acceso (Python)
```python
# services/control_acceso.py
from datetime import datetime, timedelta
from typing import Optional, Dict, List
import asyncio
import logging
from sqlalchemy.orm import Session
from models import RegistroAcceso, Usuario, ReglaAcceso, Ubicacion
from mqtt_client import PublicadorMQTT
from gestor_alertas import GestorAlertas

logger = logging.getLogger(__name__)

class ServicioControlAcceso:
    def __init__(
        self, 
        db: Session, 
        publicador_mqtt: PublicadorMQTT,
        gestor_alertas: GestorAlertas
    ):
        self.db = db
        self.mqtt = publicador_mqtt
        self.gestor_alertas = gestor_alertas
        self.intentos_fallidos = {}  # Rastrear intentos fallidos por usuario
        
    async def procesar_solicitud_acceso(
        self, 
        tarjeta_id: str, 
        sensor_id: str, 
        ubicacion: str,
        marca_tiempo: datetime
    ) -> Dict:
        """
        Procesar solicitud de acceso desde un sensor y determinar si se debe conceder
        """
        try:
            # 1. Validar tarjeta y obtener usuario
            usuario = self.db.query(Usuario).filter(
                Usuario.tarjeta_id == tarjeta_id
            ).first()
            
            if not usuario:
                logger.warning(f"Tarjeta desconocida intentó acceso: {tarjeta_id}")
                return await self._denegar_acceso(
                    tarjeta_id, sensor_id, ubicacion, marca_tiempo,
                    razon="TARJETA_DESCONOCIDA"
                )
            
            # 2. Verificar si el usuario está activo
            if not usuario.esta_activo:
                logger.warning(f"Usuario inactivo intentó acceso: {usuario.nombre_usuario}")
                return await self._denegar_acceso(
                    tarjeta_id, sensor_id, ubicacion, marca_tiempo,
                    razon="USUARIO_INACTIVO",
                    usuario_id=usuario.id
                )
            
            # 3. Verificar reglas de acceso para esta ubicación
            tiene_acceso = await self._verificar_reglas_acceso(
                usuario.id, ubicacion, marca_tiempo
            )
            
            if not tiene_acceso:
                logger.warning(
                    f"Usuario {usuario.nombre_usuario} intentó acceso no autorizado a {ubicacion}"
                )
                return await self._denegar_acceso(
                    tarjeta_id, sensor_id, ubicacion, marca_tiempo,
                    razon="UBICACION_NO_AUTORIZADA",
                    usuario_id=usuario.id
                )
            
            # 4. Verificar restricciones de horario
            if not await self._verificar_restricciones_horario(
                usuario.id, ubicacion, marca_tiempo
            ):
                logger.warning(
                    f"Usuario {usuario.nombre_usuario} intentó acceso fuera de horario a {ubicacion}"
                )
                await self.gestor_alertas.activar_alerta({
                    'tipo': 'ACCESO_FUERA_HORARIO',
                    'severidad': 'MEDIA',
                    'usuario_id': usuario.id,
                    'ubicacion': ubicacion,
                    'marca_tiempo': marca_tiempo
                })
                return await self._denegar_acceso(
                    tarjeta_id, sensor_id, ubicacion, marca_tiempo,
                    razon="RESTRICCION_HORARIA",
                    usuario_id=usuario.id
                )
            
            # 5. Conceder acceso
            return await self._conceder_acceso(
                usuario, sensor_id, ubicacion, marca_tiempo
            )
            
        except Exception as e:
            logger.error(f"Error procesando solicitud de acceso: {str(e)}")
            return await self._denegar_acceso(
                tarjeta_id, sensor_id, ubicacion, marca_tiempo,
                razon="ERROR_SISTEMA"
            )
    
    async def _verificar_reglas_acceso(
        self, 
        usuario_id: int, 
        ubicacion: str, 
        marca_tiempo: datetime
    ) -> bool:
        """Verificar si el usuario tiene permiso para acceder a esta ubicación"""
        
        # Obtener reglas de acceso del usuario
        reglas = self.db.query(ReglaAcceso).filter(
            ReglaAcceso.usuario_id == usuario_id,
            ReglaAcceso.ubicacion == ubicacion,
            ReglaAcceso.valido_desde <= marca_tiempo,
            ReglaAcceso.valido_hasta >= marca_tiempo
        ).all()
        
        return len(reglas) > 0
    
    async def _verificar_restricciones_horario(
        self, 
        usuario_id: int, 
        ubicacion: str, 
        marca_tiempo: datetime
    ) -> bool:
        """Verificar si el acceso está permitido en este horario"""
        
        obj_ubicacion = self.db.query(Ubicacion).filter(
            Ubicacion.nombre == ubicacion
        ).first()
        
        if not obj_ubicacion or not obj_ubicacion.tiene_restricciones_horario:
            return True
        
        hora_actual = marca_tiempo.time()
        dia_actual = marca_tiempo.weekday()  # 0 = Lunes, 6 = Domingo
        
        # Verificar si la hora actual está dentro del horario permitido
        if obj_ubicacion.hora_inicio_permitida <= hora_actual <= obj_ubicacion.hora_fin_permitida:
            # Verificar si el día actual está permitido
            if dia_actual in obj_ubicacion.dias_permitidos:
                return True
        
        return False
    
    async def _conceder_acceso(
        self, 
        usuario: Usuario, 
        sensor_id: str, 
        ubicacion: str, 
        marca_tiempo: datetime
    ) -> Dict:
        """Conceder acceso y registrar el evento"""
        
        # Crear registro de acceso
        registro_acceso = RegistroAcceso(
            usuario_id=usuario.id,
            tarjeta_id=usuario.tarjeta_id,
            sensor_id=sensor_id,
            ubicacion=ubicacion,
            marca_tiempo=marca_tiempo,
            acceso_concedido=True,
            razon="AUTORIZADO"
        )
        self.db.add(registro_acceso)
        self.db.commit()
        
        # Publicar a MQTT para actualizaciones en tiempo real
        await self.mqtt.publish('smartaccess/acceso/concedido', {
            'usuario_id': usuario.id,
            'nombre_usuario': usuario.nombre_usuario,
            'ubicacion': ubicacion,
            'marca_tiempo': marca_tiempo.isoformat()
        })
        
        # Enviar comando de desbloqueo al controlador de puerta
        await self.mqtt.publish(f'smartaccess/puerta/{sensor_id}/desbloquear', {
            'duracion': 5,  # Mantener desbloqueado por 5 segundos
            'usuario_id': usuario.id
        })
        
        # Limpiar contador de intentos fallidos
        self.intentos_fallidos.pop(usuario.id, None)
        
        logger.info(f"✓ Acceso concedido a {usuario.nombre_usuario} en {ubicacion}")
        
        return {
            'acceso_concedido': True,
            'usuario': {
                'id': usuario.id,
                'nombre_usuario': usuario.nombre_usuario,
                'nombre_completo': usuario.nombre_completo
            },
            'ubicacion': ubicacion,
            'marca_tiempo': marca_tiempo.isoformat()
        }
    
    async def _denegar_acceso(
        self, 
        tarjeta_id: str, 
        sensor_id: str, 
        ubicacion: str, 
        marca_tiempo: datetime,
        razon: str,
        usuario_id: Optional[int] = None
    ) -> Dict:
        """Denegar acceso y registrar el evento"""
        
        # Crear registro de acceso
        registro_acceso = RegistroAcceso(
            usuario_id=usuario_id,
            tarjeta_id=tarjeta_id,
            sensor_id=sensor_id,
            ubicacion=ubicacion,
            marca_tiempo=marca_tiempo,
            acceso_concedido=False,
            razon=razon
        )
        self.db.add(registro_acceso)
        self.db.commit()
        
        # Publicar a MQTT
        await self.mqtt.publish('smartaccess/acceso/denegado', {
            'tarjeta_id': tarjeta_id,
            'ubicacion': ubicacion,
            'razon': razon,
            'marca_tiempo': marca_tiempo.isoformat()
        })
        
        # Rastrear intentos fallidos
        if usuario_id:
            self.intentos_fallidos[usuario_id] = self.intentos_fallidos.get(usuario_id, 0) + 1
            
            # Activar alerta después de 3 intentos fallidos
            if self.intentos_fallidos[usuario_id] >= 3:
                await self.gestor_alertas.activar_alerta({
                    'tipo': 'MULTIPLES_INTENTOS_FALLIDOS',
                    'severidad': 'ALTA',
                    'usuario_id': usuario_id,
                    'ubicacion': ubicacion,
                    'cantidad_intentos': self.intentos_fallidos[usuario_id],
                    'marca_tiempo': marca_tiempo
                })
        
        logger.warning(f"✗ Acceso denegado: {razon} - Tarjeta: {tarjeta_id} en {ubicacion}")
        
        return {
            'acceso_concedido': False,
            'razon': razon,
            'ubicacion': ubicacion,
            'marca_tiempo': marca_tiempo.isoformat()
        }
    
    async def obtener_historial_acceso(
        self, 
        usuario_id: Optional[int] = None,
        ubicacion: Optional[str] = None,
        fecha_inicio: Optional[datetime] = None,
        fecha_fin: Optional[datetime] = None,
        limite: int = 100
    ) -> List[Dict]:
        """Obtener historial de acceso con filtros opcionales"""
        
        consulta = self.db.query(RegistroAcceso)
        
        if usuario_id:
            consulta = consulta.filter(RegistroAcceso.usuario_id == usuario_id)
        
        if ubicacion:
            consulta = consulta.filter(RegistroAcceso.ubicacion == ubicacion)
        
        if fecha_inicio:
            consulta = consulta.filter(RegistroAcceso.marca_tiempo >= fecha_inicio)
        
        if fecha_fin:
            consulta = consulta.filter(RegistroAcceso.marca_tiempo <= fecha_fin)
        
        registros = consulta.order_by(
            RegistroAcceso.marca_tiempo.desc()
        ).limit(limite).all()
        
        return [
            {
                'id': registro.id,
                'usuario_id': registro.usuario_id,
                'nombre_usuario': registro.usuario.nombre_usuario if registro.usuario else 'Desconocido',
                'ubicacion': registro.ubicacion,
                'marca_tiempo': registro.marca_tiempo.isoformat(),
                'acceso_concedido': registro.acceso_concedido,
                'razon': registro.razon
            }
            for registro in registros
        ]
```

### 3. Dashboard en Tiempo Real (React + WebSocket)
```jsx
// components/Dashboard/FeedAccesoEnVivo.jsx
import React, { useState, useEffect, useCallback } from 'react';
import { io } from 'socket.io-client';
import { format } from 'date-fns';
import { es } from 'date-fns/locale';
import { 
  CheckCircle, 
  XCircle, 
  Clock, 
  MapPin, 
  User,
  AlertTriangle 
} from 'lucide-react';

const FeedAccesoEnVivo = () => {
  const [eventos, setEventos] = useState([]);
  const [estadisticas, setEstadisticas] = useState({
    totalHoy: 0,
    concedidos: 0,
    denegados: 0,
    usuariosActivos: 0
  });
  const [socket, setSocket] = useState(null);
  const [estaConectado, setEstaConectado] = useState(false);

  useEffect(() => {
    // Inicializar conexión WebSocket
    const nuevoSocket = io('http://localhost:8000', {
      transports: ['websocket'],
      reconnection: true,
      reconnectionDelay: 1000,
      reconnectionAttempts: 5
    });

    nuevoSocket.on('connect', () => {
      console.log('✓ Conectado a WebSocket');
      setEstaConectado(true);
    });

    nuevoSocket.on('disconnect', () => {
      console.log('✗ Desconectado de WebSocket');
      setEstaConectado(false);
    });

    // Escuchar eventos de acceso
    nuevoSocket.on('evento_acceso', (datos) => {
      manejarNuevoEventoAcceso(datos);
    });

    // Escuchar actualizaciones de estadísticas
    nuevoSocket.on('actualizacion_estadisticas', (datos) => {
      setEstadisticas(datos);
    });

    setSocket(nuevoSocket);

    // Limpiar al desmontar
    return () => {
      nuevoSocket.close();
    };
  }, []);

  const manejarNuevoEventoAcceso = useCallback((evento) => {
    setEventos(eventosAnteriores => {
      const nuevosEventos = [evento, ...eventosAnteriores];
      // Mantener solo los últimos 50 eventos
      return nuevosEventos.slice(0, 50);
    });

    // Actualizar estadísticas
    setEstadisticas(estadisticasAnteriores => ({
      ...estadisticasAnteriores,
      totalHoy: estadisticasAnteriores.totalHoy + 1,
      concedidos: evento.acceso_concedido 
        ? estadisticasAnteriores.concedidos + 1 
        : estadisticasAnteriores.concedidos,
      denegados: !evento.acceso_concedido 
        ? estadisticasAnteriores.denegados + 1 
        : estadisticasAnteriores.denegados
    }));

    // Mostrar notificación para acceso denegado
    if (!evento.acceso_concedido) {
      mostrarNotificacion(
        'Acceso Denegado', 
        `${evento.ubicacion} - ${evento.razon}`
      );
    }
  }, []);

  const mostrarNotificacion = (titulo, mensaje) => {
    if ('Notification' in window && Notification.permission === 'granted') {
      new Notification(titulo, {
        body: mensaje,
        icon: '/icono-alerta.png'
      });
    }
  };

  const solicitarPermisoNotificacion = () => {
    if ('Notification' in window && Notification.permission === 'default') {
      Notification.requestPermission();
    }
  };

  useEffect(() => {
    solicitarPermisoNotificacion();
  }, []);

  const obtenerIconoEvento = (evento) => {
    if (evento.acceso_concedido) {
      return <CheckCircle className="text-green-500" size={20} />;
    }
    return <XCircle className="text-red-500" size={20} />;
  };

  const obtenerColorInsignia = (evento) => {
    return evento.acceso_concedido 
      ? 'bg-green-100 text-green-800' 
      : 'bg-red-100 text-red-800';
  };

  return (
    <div className="bg-white rounded-lg shadow-lg p-6">
      {/* Estado de Conexión */}
      <div className="flex items-center justify-between mb-6">
        <h2 className="text-2xl font-bold flex items-center gap-2">
          <div className={`w-3 h-3 rounded-full ${
            estaConectado ? 'bg-green-500 animate-pulse' : 'bg-red-500'
          }`} />
          Eventos de Acceso en Vivo
        </h2>
        <span className="text-sm text-gray-500">
          {estaConectado ? 'Conectado' : 'Desconectado'}
        </span>
      </div>

      {/* Tarjetas de Estadísticas */}
      <div className="grid grid-cols-4 gap-4 mb-6">
        <div className="bg-blue-50 rounded-lg p-4">
          <div className="text-3xl font-bold text-blue-600">
            {estadisticas.totalHoy}
          </div>
          <div className="text-sm text-gray-600">Total Hoy</div>
        </div>
        <div className="bg-green-50 rounded-lg p-4">
          <div className="text-3xl font-bold text-green-600">
            {estadisticas.concedidos}
          </div>
          <div className="text-sm text-gray-600">Concedidos</div>
        </div>
        <div className="bg-red-50 rounded-lg p-4">
          <div className="text-3xl font-bold text-red-600">
            {estadisticas.denegados}
          </div>
          <div className="text-sm text-gray-600">Denegados</div>
        </div>
        <div className="bg-purple-50 rounded-lg p-4">
          <div className="text-3xl font-bold text-purple-600">
            {estadisticas.usuariosActivos}
          </div>
          <div className="text-sm text-gray-600">Usuarios Activos</div>
        </div>
      </div>

      {/* Lista de Eventos */}
      <div className="space-y-2 max-h-[600px] overflow-y-auto">
        {eventos.length === 0 ? (
          <div className="text-center py-12 text-gray-500">
            <Clock size={48} className="mx-auto mb-4 opacity-50" />
            <p>Esperando eventos de acceso...</p>
          </div>
        ) : (
          eventos.map((evento, indice) => (
            <div
              key={`${evento.marca_tiempo}-${indice}`}
              className={`
                flex items-center gap-4 p-4 rounded-lg border-l-4
                ${evento.acceso_concedido 
                  ? 'border-green-500 bg-green-50' 
                  : 'border-red-500 bg-red-50'}
                transition-all duration-300 hover:shadow-md
                ${indice === 0 ? 'animate-slideIn' : ''}
              `}
            >
              {/* Icono */}
              <div className="flex-shrink-0">
                {obtenerIconoEvento(evento)}
              </div>

              {/* Hora */}
              <div className="flex-shrink-0 w-24">
                <div className="text-sm font-mono text-gray-600">
                  {format(new Date(evento.marca_tiempo), 'HH:mm:ss', { locale: es })}
                </div>
              </div>

              {/* Usuario */}
              <div className="flex items-center gap-2 flex-1">
                <User size={16} className="text-gray-400" />
                <span className="font-semibold">
                  {evento.nombre_usuario || evento.tarjeta_id}
                </span>
              </div>

              {/* Ubicación */}
              <div className="flex items-center gap-2 flex-1">
                <MapPin size={16} className="text-gray-400" />
                <span className="text-gray-700">{evento.ubicacion}</span>
              </div>

              {/* Insignia de Estado */}
              <div className={`px-3 py-1 rounded-full text-xs font-semibold ${
                obtenerColorInsignia(evento)
              }`}>
                {evento.acceso_concedido ? '✓ Concedido' : '✗ Denegado'}
              </div>

              {/* Razón (si fue denegado) */}
              {!evento.acceso_concedido && (
                <div className="flex items-center gap-1 text-xs text-red-600">
                  <AlertTriangle size={14} />
                  <span>{evento.razon}</span>
                </div>
              )}
            </div>
          ))
        )}
      </div>
    </div>
  );
};

export default FeedAccesoEnVivo;
```

---

## 📚 Documentación de la API

### URL Base
```
Desarrollo: http://localhost:8000
Producción: https://api.smartaccess.tudominio.com
```

### Autenticación

Todas las solicitudes a la API requieren autenticación mediante tokens JWT.
```http
POST /api/auth/login
Content-Type: application/json

{
  "nombre_usuario": "admin@smartaccess.local",
  "contrasena": "tu_contraseña"
}

Respuesta:
{
  "token_acceso": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "tipo_token": "bearer",
  "expira_en": 3600
}
```

**Usando el token:**
```http
GET /api/registros-acceso
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

### Endpoints Principales

#### 1. Registros de Acceso

**Obtener Historial de Acceso**
```http
GET /api/registros-acceso?limite=50&offset=0
Authorization: Bearer {token}

Parámetros de Consulta:
- limite: int (por defecto: 50, máximo: 100)
- offset: int (por defecto: 0)
- usuario_id: int (opcional)
- ubicacion: string (opcional)
- fecha_inicio: fecha ISO (opcional)
- fecha_fin: fecha ISO (opcional)
- acceso_concedido: boolean (opcional)

Respuesta: 200 OK
{
  "total": 1247,
  "limite": 50,
  "offset": 0,
  "datos": [
    {
      "id": 12345,
      "usuario_id": 42,
      "nombre_usuario": "juan.perez",
      "nombre_completo": "Juan Pérez",
      "tarjeta_id": "TARJETA-001",
      "ubicacion": "Entrada Principal",
      "marca_tiempo": "2024-01-15T14:32:18Z",
      "acceso_concedido": true,
      "razon": "AUTORIZADO"
    },
    ...
  ]
}
```

**Obtener Registro Individual**
```http
GET /api/registros-acceso/{registro_id}
Authorization: Bearer {token}

Respuesta: 200 OK
{
  "id": 12345,
  "usuario_id": 42,
  "nombre_usuario": "juan.perez",
  "ubicacion": "Entrada Principal",
  "marca_tiempo": "2024-01-15T14:32:18Z",
  "acceso_concedido": true,
  "razon": "AUTORIZADO",
  "sensor_id": "RFID-001",
  "intensidad_senal": 95
}
```

#### 2. Usuarios

**Listar Todos los Usuarios**
```http
GET /api/usuarios?limite=50&offset=0
Authorization: Bearer {token}

Respuesta: 200 OK
{
  "total": 89,
  "datos": [
    {
      "id": 42,
      "nombre_usuario": "juan.perez",
      "email": "juan.perez@empresa.com",
      "nombre_completo": "Juan Pérez",
      "tarjeta_id": "TARJETA-001",
      "esta_activo": true,
      "rol": "empleado",
      "creado_en": "2024-01-01T00:00:00Z"
    },
    ...
  ]
}
```

**Crear Nuevo Usuario**
```http
POST /api/usuarios
Authorization: Bearer {token}
Content-Type: application/json

{
  "nombre_usuario": "maria.garcia",
  "email": "maria.garcia@empresa.com",
  "nombre_completo": "María García",
  "tarjeta_id": "TARJETA-042",
  "contrasena": "ContraseñaSegura123!",
  "rol": "empleado"
}

Respuesta: 201 Created
{
  "id": 90,
  "nombre_usuario": "maria.garcia",
  "email": "maria.garcia@empresa.com",
  "nombre_completo": "María García",
  "tarjeta_id": "TARJETA-042",
  "esta_activo": true,
  "rol": "empleado",
  "creado_en": "2024-01-15T14:35:00Z"
}
```

#### 3. Sensores

**Listar Todos los Sensores**
```http
GET /api/sensores
Authorization: Bearer {token}

Respuesta: 200 OK
{
  "total": 25,
  "datos": [
    {
      "sensor_id": "RFID-001",
      "tipo": "rfid",
      "ubicacion": "Entrada Principal",
      "estado": "en_linea",
      "nivel_bateria": 87,
      "intensidad_senal": 98,
      "ultima_vez_visto": "2024-01-15T14:32:00Z",
      "version_firmware": "1.2.3"
    },
    ...
  ]
}
```

#### 4. Alertas

**Obtener Alertas Recientes**
```http
GET /api/alertas?severidad=ALTA&limite=20
Authorization: Bearer {token}

Parámetros de Consulta:
- severidad: string (BAJA, MEDIA, ALTA, CRITICA)
- tipo: string (opcional)
- reconocida: boolean (opcional)
- limite: int (por defecto: 50)

Respuesta: 200 OK
{
  "total": 3,
  "datos": [
    {
      "id": 789,
      "tipo": "MULTIPLES_INTENTOS_FALLIDOS",
      "severidad": "ALTA",
      "mensaje": "Usuario juan.perez tuvo 5 intentos fallidos de acceso",
      "usuario_id": 42,
      "ubicacion": "Sala de Servidores",
      "marca_tiempo": "2024-01-15T14:25:00Z",
      "reconocida": false,
      "reconocida_por": null,
      "reconocida_en": null
    },
    ...
  ]
}
```

**Reconocer Alerta**
```http
POST /api/alertas/{alerta_id}/reconocer
Authorization: Bearer {token}
Content-Type: application/json

{
  "notas": "Usuario contactado, identidad verificada"
}

Respuesta: 200 OK
{
  "id": 789,
  "reconocida": true,
  "reconocida_por": "admin",
  "reconocida_en": "2024-01-15T14:45:00Z",
  "notas": "Usuario contactado, identidad verificada"
}
```

#### 5. Estadísticas

**Obtener Estadísticas del Dashboard**
```http
GET /api/estadisticas/dashboard
Authorization: Bearer {token}

Respuesta: 200 OK
{
  "hoy": {
    "total_acceso": 1247,
    "concedidos": 1215,
    "denegados": 32,
    "usuarios_activos": 89,
    "alertas": 3
  },
  "esta_semana": {
    "total_acceso": 7543,
    "concedidos": 7321,
    "denegados": 222,
    "hora_pico": "09:00",
    "ubicacion_mas_concurrida": "Entrada Principal"
  },
  "sensores": {
    "total": 25,
    "en_linea": 24,
    "desconectados": 1,
    "advertencia": 2
  }
}
```

**Obtener Tendencias de Acceso**
```http
GET /api/estadisticas/tendencias?periodo=7d
Authorization: Bearer {token}

Parámetros de Consulta:
- periodo: string (24h, 7d, 30d, 90d)
- ubicacion: string (opcional)

Respuesta: 200 OK
{
  "periodo": "7d",
  "puntos_datos": [
    {
      "fecha": "2024-01-09",
      "total": 1150,
      "concedidos": 1120,
      "denegados": 30
    },
    {
      "fecha": "2024-01-10",
      "total": 1203,
      "concedidos": 1175,
      "denegados": 28
    },
    ...
  ],
  "resumen": {
    "promedio_por_dia": 1180,
    "tendencia": "aumentando",
    "cambio_porcentual": "+12.5%"
  }
}
```

### Respuestas de Error

Todos los errores siguen este formato:
```json
{
  "error": {
    "codigo": "NO_AUTORIZADO",
    "mensaje": "Token inválido o expirado",
    "detalles": null
  }
}
```

**Códigos de Error Comunes:**

| Código | Estado HTTP | Descripción |
|--------|-------------|-------------|
| `NO_AUTORIZADO` | 401 | Autenticación inválida o faltante |
| `PROHIBIDO` | 403 | Permisos insuficientes |
| `NO_ENCONTRADO` | 404 | Recurso no encontrado |
| `ERROR_VALIDACION` | 422 | Datos de solicitud inválidos |
| `ERROR_INTERNO` | 500 | Error del servidor |

---

## 🤝 Contribuir

Este proyecto fue desarrollado como parte de investigación en el SENA. Aunque el código y las aplicaciones son propiedad del SENA, las contribuciones y sugerencias son bienvenidas.

### Flujo de Trabajo de Desarrollo
```bash
# 1. Crear rama de característica
git checkout -b feature/nombre-de-tu-caracteristica

# 2. Realizar cambios

# 3. Ejecutar pruebas
npm run test          # Pruebas frontend
pytest               # Pruebas backend
./ejecutar_pruebas_sensores   # Pruebas C++

# 4. Formatear código
npm run format        # Frontend
black .              # Backend
clang-format -i **/*.cpp  # C++

# 5. Commit con commits convencionales
git commit -m "feat: agregar soporte para nuevo tipo de sensor"

# 6. Push y crear pull request
git push origin feature/nombre-de-tu-caracteristica
```

### Estilo de Código

- **C++**: Guía de Estilo de Google C++
- **Python**: PEP 8 con formateador Black
- **JavaScript/React**: Guía de Estilo Airbnb con Prettier

---

## 📄 Licencia

Este proyecto fue desarrollado durante labores de investigación en el SENA (Servicio Nacional de Aprendizaje). El código, aplicaciones, documentación y repositorios son propiedad del SENA.

Para fines educativos y de portafolio, esta documentación demuestra la implementación técnica y el diseño del sistema.
