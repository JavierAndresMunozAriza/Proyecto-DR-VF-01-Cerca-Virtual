# DR-VF-01: Collar Inteligente de Cerca Virtual y Monitoreo Ganadero (Arquitectura V4.1)

[![Estado del Proyecto](https://img.shields.io/badge/Status-Industrial%20Architecture%20V4.1-blue.svg)](../../)
[![Costo BOM Optimizado](https://img.shields.io/badge/BOM%20Target-%2456.80%20USD-green.svg)](../../)
[![Normativa](https://img.shields.io/badge/Compliance-IEC%2060335--2--76-orange.svg)](../../)
[![Microcontrolador](https://img.shields.io/badge/MCU-ESP32--S3--N16R8-red.svg)](../../)

## 🌟 Descripción General

El **DR-VF-01** es un nodo IoT embebido de grado industrial (*Edge Device*) diseñado para la contención geográfica de bovinos mediante cercas virtuales (*Virtual Fencing*) y la supervisión del bienestar animal en entornos rurale aislados. 

El sistema combina geolocalización multiconstelación de alta precisión, conectividad híbrida (LTE Cat-M1/NB-IoT + LoRa + BLE/Wi-Fi), algoritmos de Inteligencia Artificial en el Borde (*Edge AI*) para la clasificación del comportamiento animal y un sistema disuasivo electrostático aislado que cumple estrictamente con el bienestar animal y la normativa internacional **IEC 60335-2-76**.

---

## 🛠️ Arquitectura de Hardware (V4.1)

| Módulo | Componente Clave | Función Principal |
| :--- | :--- | :--- |
| **Unidad Central (MCU)** | ESP32-S3-N16R8 | Dual-core Xtensa LX7 @ 240 MHz, 16 MB Flash, 8 MB PSRAM, Servidor Web local y SoftAP/BLE. |
| **Módem Celular** | Quectel BG95-M3 | Telemetría en la nube mediante LTE Cat-M1 / NB-IoT / EGPRS. Modo PSM con consumo de $3.9\,\mu\text{A}$. |
| **Navegación GNSS** | u-blox MAX-M10S | Rastreo multiconstelación (GPS, GLONASS, Galileo, BeiDou) con filtro LNA/SAW integrado. |
| **Radio de Respaldo** | Semtech SX1262 | Transceptor LoRa de largo alcance ($+22\,\text{dBm}$) para telemetría offline sin red celular. |
| **IMU / Edge AI** | ST LSM6DSOX | Acelerómetro/Giroscopio de 6 ejes con *Machine Learning Core* (MLC) para detección de rumia y descanso. |
| **Memoria de Estado** | Fujitsu MB85RS64V | FRAM SPI de 64 Kb para almacenamiento de polígonos sin desgaste por escrituras continuas. |
| **Gestión Energética** | MAX17048 + CN3791 | Medidor de carga (*Fuel Gauge*) por $\text{I}^2\text{C}$ con algoritmo ModelGauge y cargador Solar MPPT para LiFePO4. |
| **Estímulo Disuasivo** | Etapa Aislada $0.2\,\text{J}$ | Pulso disuasivo de $0.2\,\text{J}$ @ $2.5\,\text{kV}$ accionado por optoacoplador VO617A e *interlock* por hardware. |

---

## 📊 Presupuesto de Componentes (BOM Estimado)

El sistema ha sido optimizado para un costo de lista de materiales (BOM) objetivo de **~$56.80 USD** a escala de producción:

```text
[ESP32-S3-N16R8] ── $3.20 USD ┐
[Quectel BG95-M3]  ── $14.50 USD│
[u-blox MAX-M10S]  ── $8.50 USD │── BOM Total Estimado: ~$56.80 USD
[Semtech SX1262]   ── $3.80 USD │   (Optimizado para producción masiva)
[ST LSM6DSOX]      ── $2.10 USD │
[Otros / PMIC/PCB] ── $24.70 USD┘