# DR-VF-01: Guía de Justificación Técnica y Defensa de Componentes (V4.1)

Este documento contiene el análisis de alternativas (*Trade-off Analysis*) y la argumentación técnica para la defensa oral del proyecto y el capítulo de hardware de la Tesis de Grado (UNAL Sede Manizales).

---

## 1. Unidad Central de Procesamiento (MCU)
* **Elección:** ESP32-S3-N16R8 (Dual-Core Xtensa LX7 @ 240 MHz, 16 MB Flash, 8 MB PSRAM).
* **¿Por qué este y no STM32 / Arduino / PIC?**
  1. **Doble Núcleo:** Permite separar en FreeRTOS la ejecución en tiempo real (lectura de geolocalización y control disuasivo) de las pilas de comunicación (Wi-Fi, BLE, pila celular).
  2. **Aceleración por Hardware Vectorial:** Facilita la ejecución de modelos TinyML para procesar datos del acelerómetro sin sobrecargar la CPU.
  3. **Memoria Holgada:** 8 MB de PSRAM interna evitan el desbordamiento de memoria al gestionar buffers de telemetría y matrices del mapa de cercas.

---

## 2. Geolocalización (GNSS)
* **Elección:** u-blox MAX-M10S.
* **¿Por qué este y no un GPS estándar tipo NEO-6M?**
  1. **Multiconstelación Simultánea:** Rastrea GPS, GLONASS, Galileo y BeiDou al mismo tiempo. En la topografía montañosa de Caldas o zonas boscosas, previene la pérdida de señal (*Fix*) y reduce el error de posición a menos de 2 metros.
  2. **Filtro LNA + SAW Integrado:** Protege la recepción de la señal satelital contra el ruido electromagnético generado por las antenas cercanas de LTE y LoRa en la misma PCB.
  3. **Bajo Consumo:** Consume menos de 25 mW en modo de rastreo continuo.

---

## 3. Conectividad Híbrida (Telemetría)
* **Elección:** Quectel BG95-M3 (LTE Cat-M1 / NB-IoT) + Semtech SX1262 (LoRa 915 MHz).
* **¿Por qué esta combinación?**
  1. **LTE Cat-M1/NB-IoT:** Diseñado para IoT en zonas rurales aisladas. Excelente penetración en vegetación y modo de ahorro de energía PSM ($3.9\,\mu\text{A}$).
  2. **LoRa de Respaldo:** Si el animal entra en un potrero sin cobertura de red celular, el collar transmite alertas críticas de cerca virtual a través de una red privada de finca a varios kilómetros.

---

## 4. Sensor de Movimiento e Inteligencia Artificial
* **Elección:** ST LSM6DSOX (IMU de 6 ejes con Machine Learning Core).
* **¿Por qué este y no un MPU6050 común?**
  1. **Machine Learning Core (MLC):** Clasifica el comportamiento del animal (rumia, pastoreo, descanso, marcha) dentro del propio silicio del sensor.
  2. **Ahorro de Batería:** El microcontrolador ESP32-S3 puede permanecer dormido mientras el sensor clasifica la conducta, despertando al sistema solo para enviar resúmenes.

---

## 5. Memoria de Estado No Volátil
* **Elección:** Fujitsu MB85RS64V (64 Kb FRAM por SPI).
* **¿Por qué FRAM y no EEPROM / Flash?**
  1. **Ciclos de Escritura:** Admite $10^{12}$ (un billón) de escrituras frente a los $100,000$ ciclos de la Flash/EEPROM, evitando que la memoria se destruya por la actualización constante de coordenadas.
  2. **Velocidad e Inmunidad:** Escribe instantáneamente a la velocidad del bus SPI sin requerir ciclos de borrado de página ni consumir picos de corriente.

---

## 6. Sistema de Alimentación y Batería
* **Elección:** Batería LiFePO4 ($3.2\,\text{V}$) + Cargador CN3791 (MPPT) + Fuel Gauge MAX17048.
* **¿Por qué LiFePO4 y no Li-Po de 3.7V?**
  1. **Seguridad Térmica:** Las baterías Li-Po se degradan o inflan expuestas al sol directo en el campo (>45 °C). La química LiFePO4 es estable hasta 60 °C–70 °C.
  2. **Ciclo de Vida:** Ofrece más de 2,000 ciclos de carga/descarga con una curva de voltaje muy plana durante la descarga.

---

## 7. Etapa Disuasiva y Normativa
* **Elección:** Pulso aislado de $0.2\,\text{J}$ max @ $2.5\,\text{kV}$ con optoacoplador VO617A.
* **¿Por qué estas especificaciones?**
  1. **IEC 60335-2-76:** Cumple con la norma de cercas eléctricas, utilizando una energía 10 veces menor a la de un energizador de potrero ($2.0\,\text{J} - 5.0\,\text{J}$), logrando persuadir al animal sin causarle daño.
  2. **Aislamiento Óptico e Interlock:** Garantiza aislamiento galvánico para que ningún pico de alta tensión dañe la electrónica digital, e incluye un bloqueo por hardware para evitar que el pulso quede encendido si el código se congela.