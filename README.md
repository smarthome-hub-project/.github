<div align="center">

# 🏠 Smart Home Hub Project

### Sistema embebido IoT distribuido con control multimodal

[![GitHub](https://img.shields.io/badge/GitHub-Organization-181717?logo=github)](https://github.com/smarthome-hub-project)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Project](https://img.shields.io/badge/Project-Academic-blue.svg)](#)

</div>

---

## 🎯 Visión general

**Smart Home Hub** es un sistema embebido distribuido que permite controlar un sistema de iluminación inteligente desde múltiples interfaces:

- 🔵 **Bluetooth Low Energy** (Web App local)
- 🌐 **Web Bluetooth** (cualquier navegador moderno)
- 🎤 **Reconocimiento de voz** (en el navegador)
- 📱 **Telegram** (control remoto mundial)
- 🔘 **Botones físicos** (control local)

El sistema está diseñado con una arquitectura en **5 capas** que separa claramente el hardware, el control local, el gateway, la nube y los usuarios.

---

## 🏗 Arquitectura

```
┌─────────────────────────────────────────────────────────────┐
│  📱 USUARIO                                                 │
│  Telegram | Web App (BLE) | Voz | Botones                   │
└──────────────────┬──────────────────────────────────────────┘
                   ↓
┌─────────────────────────────────────────────────────────────┐
│  ☁️ CLOUD                                                   │
│  Bot Python (Railway) | HiveMQ MQTT Broker                  │
└──────────────────┬──────────────────────────────────────────┘
                   ↓
┌─────────────────────────────────────────────────────────────┐
│  📶 GATEWAY (ESP32-C6)                                      │
│  WiFi + MQTT + BLE + UART Bridge                            │
└──────────────────┬──────────────────────────────────────────┘
                   ↓
┌─────────────────────────────────────────────────────────────┐
│  🧠 CONTROL LOCAL (STM32 L476RG)                            │
│  FSM event-driven + Drivers (GPIO, ADC, PWM, UART)          │
└──────────────────┬──────────────────────────────────────────┘
                   ↓
┌─────────────────────────────────────────────────────────────┐
│  💡 HARDWARE                                                │
│  Multifunction Shield (LEDs, botones, potenciómetro)        │
└─────────────────────────────────────────────────────────────┘
```

---

## 📦 Repositorios del proyecto

| Componente | Repositorio | Tecnologías | Descripción |
|------------|-------------|-------------|-------------|
| 🔌 STM32 (CMSIS) | [smarthome-hub-stm32-cmsis](https://github.com/smarthome-hub-project/smarthome-hub-stm32-cmsis) | C, CMSIS | Firmware bare-metal con acceso directo a registros |
| 🚀 STM32 (Zephyr) | [smarthome-hub-stm32-zephyr](https://github.com/smarthome-hub-project/smarthome-hub-stm32-zephyr) | C, Zephyr RTOS | Firmware con threads, mutex, message queues |
| 📶 ESP32-C6 | [smarthome-hub-esp32](https://github.com/smarthome-hub-project/smarthome-hub-esp32) | C++, Arduino | Bridge BLE/MQTT entre STM32 y la nube |
| 🤖 Telegram Bot | [smarthome-hub-telegram-bot](https://github.com/smarthome-hub-project/smarthome-hub-telegram-bot) | Python, MQTT | Bot conversacional para control remoto |
| 🌐 Web App | [smarthome-hub-web](https://github.com/smarthome-hub-project/smarthome-hub-web) | HTML, JS | Interfaz web con Web Bluetooth API |

---

## ✨ Características principales

### 🎛 Modos de operación
- 🌙 **Night** - Iluminación tenue (15%)
- ☀️ **Day** - Iluminación máxima (100%)
- 😌 **Relax** - Iluminación ambiental
- 🚨 **Alarm** - Parpadeo de alerta
- 🎉 **Party** - Secuencias dinámicas rotativas
- ⏹ **Standby** - Modo piloto (5%)

### 🌐 Conectividad
- **BLE 5.0** (GATT Server con NimBLE)
- **WiFi** (cliente con soporte multi-red)
- **MQTT** (publish/subscribe con QoS)
- **UART** (115200 bps, USART3)

### 🧠 Inteligencia
- **FSM event-driven** con 7 estados
- **Eventos por intención** (no por fuente)
- **Auto-reporte** de cambios de temperatura
- **Notificaciones push** vía Telegram
- **Comandos en lenguaje natural** (voz)

---

## 🔄 Flujo de un comando

Ejemplo: usuario envía `/night` desde Telegram

```
1. Telegram → Bot Python (Railway)
2. Bot publica "N" en MQTT topic
3. HiveMQ Broker reenvía a suscriptores
4. ESP32-C6 recibe el mensaje
5. ESP32 reenvía "N" por UART al STM32
6. STM32 dispara EVT_ENTER_NIGHT_MODE
7. FSM transita a estado NIGHT
8. LEDs se atenúan al 15%
9. STM32 envía "ACK:NIGHT" por UART
10. ESP32 publica "NIGHT" en topic ACK
11. Bot recibe el ACK
12. Bot envía "🌙 Nocturno activado" al usuario
```

Tiempo total: **~500-1500 ms** (incluyendo viaje internet)

---

## 🛠 Stack tecnológico

### Hardware
- STM32 Nucleo L476RG (ARM Cortex-M4 @ 80 MHz)
- nanoESP32-C6 v1.0 (RISC-V con WiFi + BLE)
- Multifunction Shield (LEDs, botones, potenciómetro)

### Firmware
- C bare-metal (CMSIS)
- C con Zephyr RTOS
- C++ con Arduino framework

### Software
- Python 3.11 (Telegram bot)
- HTML/CSS/JavaScript (Web App)

### Cloud y servicios
- **Railway.app** - Hosting del bot 24/7
- **HiveMQ Public Broker** - MQTT
- **Vercel** - Hosting de la Web App
- **Telegram Bot API** - Mensajería
- **GitHub** - Control de versiones

### Protocolos
- BLE 5.0 (GATT)
- WiFi 802.11 b/g/n
- MQTT 3.1.1 (ISO/IEC 20922)
- UART 115200 bps
- HTTPS + WebSockets

---

## 📊 Estadísticas del proyecto

| Métrica | Valor |
|---------|-------|
| Repositorios | 5 |
| Lenguajes de programación | 4 (C, C++, Python, JavaScript) |
| Frameworks | 4 (CMSIS, Zephyr, Arduino, python-telegram-bot) |
| Líneas de código (aprox.) | ~5,000 |
| Tiempo de desarrollo | ~3 semanas |
| Servicios cloud usados | 4 (Railway, HiveMQ, Vercel, Telegram) |
| Costo mensual | $0 (tier gratis de todos los servicios) |

---

## 🎓 Conceptos demostrados

### Embebidos
- ✅ Programación bare-metal (CMSIS sin HAL)
- ✅ Programación con RTOS (Zephyr)
- ✅ Drivers de periféricos (GPIO, ADC, PWM, UART)
- ✅ Máquinas de estados finitos (FSM)
- ✅ Interrupciones y debounce
- ✅ PWM por software con resolución
- ✅ Buffer circular para UART

### Sistemas operativos
- ✅ Threads concurrentes
- ✅ Sincronización con mutex
- ✅ Comunicación con message queues
- ✅ Timers del kernel
- ✅ DeviceTree (configuración declarativa)

### Comunicaciones
- ✅ UART (asincrónico)
- ✅ BLE (GATT Server/Client)
- ✅ WiFi (cliente con multi-red)
- ✅ MQTT (publish/subscribe)
- ✅ HTTPS (Web Bluetooth requiere HTTPS)

### Cloud / IoT
- ✅ Arquitectura en capas
- ✅ Comunicación máquina-a-máquina (M2M)
- ✅ Patrón Publicador/Suscriptor
- ✅ Hosting cloud (PaaS)
- ✅ CI/CD básico (GitHub → Railway)

### Software
- ✅ Programación asíncrona (async/await)
- ✅ Manejo de variables de entorno
- ✅ Reconocimiento de voz (Web Speech API)
- ✅ Procesamiento de texto natural

---

## 📐 Diagrama de la FSM

```
                    EVT_SYSTEM_READY
                          ↓
                    ┌─────────┐
                    │  INIT   │
                    └────┬────┘
                         ↓
                    ┌─────────┐
                    │ STANDBY │ ← Estado inicial (5%)
                    └────┬────┘
                         │
        ┌────────────────┼─────────────────┐
        ↓                ↓                  ↓
   ┌─────────┐      ┌─────────┐       ┌─────────┐
   │  NIGHT  │      │   DAY   │       │  RELAX  │
   │  (15%)  │←────→│  (100%) │       │ (40% L1)│
   └─────────┘      └─────────┘       └─────────┘
        │                │                  │
        └────────┬───────┴──────┬───────────┘
                 ↓              ↓
            ┌─────────┐    ┌─────────┐
            │  ALARM  │    │  PARTY  │
            │ (blink) │    │  (seq.) │
            └─────────┘    └─────────┘
```

---

## 🚀 Cómo empezar

1. **Lee la documentación** de cada repositorio
2. **Comienza por el STM32** (CMSIS o Zephyr)
3. **Configura el ESP32-C6** como bridge
4. **Despliega el bot** en Railway
5. **Abre la Web App** desde Vercel
6. **Empieza a interactuar** con el sistema

Cada repositorio tiene su propio README con instrucciones detalladas.

---

## 📚 Documentación

| Documento | Ubicación |
|-----------|-----------|
| README STM32 CMSIS | [smarthome-hub-stm32-cmsis](https://github.com/smarthome-hub-project/smarthome-hub-stm32-cmsis#readme) |
| README STM32 Zephyr | [smarthome-hub-stm32-zephyr](https://github.com/smarthome-hub-project/smarthome-hub-stm32-zephyr#readme) |
| README ESP32-C6 | [smarthome-hub-esp32](https://github.com/smarthome-hub-project/smarthome-hub-esp32#readme) |
| README Bot Telegram | [smarthome-hub-telegram-bot](https://github.com/smarthome-hub-project/smarthome-hub-telegram-bot#readme) |
| README Web App | [smarthome-hub-web](https://github.com/smarthome-hub-project/smarthome-hub-web#readme) |

---

## 👨‍💻 Autor

<div align="center">

### David Henao Rojas

[![GitHub](https://img.shields.io/badge/GitHub-dahenaor--source-181717?logo=github)](https://github.com/dahenaor-source)

**Proyecto académico**  
Estructuras Computacionales  
Universidad Nacional de Colombia (o tu universidad)

</div>

---

## 🙏 Agradecimientos

Este proyecto integra herramientas y plataformas de:

- [Espressif Systems](https://www.espressif.com/) (ESP32-C6)
- [STMicroelectronics](https://www.st.com/) (STM32 Nucleo)
- [PlatformIO](https://platformio.org/) (entorno de desarrollo)
- [Zephyr Project](https://zephyrproject.org/) (RTOS)
- [HiveMQ](https://www.hivemq.com/) (broker MQTT)
- [Railway](https://railway.app/) (hosting)
- [Vercel](https://vercel.com/) (hosting web)
- [Telegram](https://telegram.org/) (Bot API)

---

## 📄 Licencia

Todos los repositorios de este proyecto están bajo la **Licencia MIT**.

---

<div align="center">

⭐ Si este proyecto te resulta útil, dale una estrella a los repositorios ⭐

</div>
