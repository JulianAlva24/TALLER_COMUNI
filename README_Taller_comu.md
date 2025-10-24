# Taller de Comunicaciones — README

**Autores:** Daniel Felipe Pinilla Daza · Julian Sebastian Alvarado Monroy  
**Documento base:** `Taller_comu.pdf`  
**Asignatura/Contexto:** Comunicaciones / Automatización Industrial

---

## Descripción general
Este README resume el contenido del *Taller de Comunicaciones*, que aborda vigilancia tecnológica de protocolos industriales y el desarrollo de un proyecto práctico de **control de servo por RS‑485 usando Raspberry Pi**. Incluye comparación de modos de comunicación (simplex, half‑duplex, full‑duplex), configuración de sistema, implementación en Python, pruebas y lecciones aprendidas.

---

## Contenido del documento

### 1) Vigilancia Tecnológica de Protocolos de Comunicación Industrial
- **PROFIBUS**  
  - *Descripción:* bus de campo para automatización (DP/PA/FMS).  
  - *Técnico:* 9.6 kbit/s–12 Mbit/s, RS‑485 (DP) / MBP (PA), hasta 126 nodos, paso de token maestro‑esclavo.  
  - *Aplicaciones:* procesos continuos, líneas automotrices, DCS, variadores.  
  - *Tendencias:* migración gradual a **PROFINET**, gateways, diagnóstico predictivo, arquitecturas híbridas.
- **PROFINET**  
  - *Descripción:* evolución Ethernet de PROFIBUS para tiempo real.  
  - *Técnico:* 100 Mbit/s–1 Gbit/s; clases **TCP/IP**, **RT**, **IRT** (hasta ~250 μs); topologías flexibles; MRP; proxies PROFIBUS.  
  - *Aplicaciones:* motion CNC, robótica, manufactura flexible, Industria 4.0, gemelos digitales.  
  - *Tendencias:* TSN, OPC UA, edge computing, **APL**.
- **Ethernet Industrial**  
  - *Técnico general:* 10/100/1000 Mbit/s (y 10G), conectores M12, rangos −40 °C a 75 °C, HSR/PRP/MRP para alta disponibilidad.  
  - *Protocolos revisados:* **EtherNet/IP (CIP)**, **EtherCAT** (on‑the‑fly, ~100 μs), **Modbus TCP**, **POWERLINK**.  
  - *Aplicaciones:* SCADA‑PLC, backbone de planta, visión artificial, IIoT.  
  - *Tendencias:* TSN, 5G privado, SPE, SDN, ciberseguridad zero‑trust.
- **RS‑485**  
  - *Descripción:* capa física diferencial multipunto muy inmune a ruido.  
  - *Técnico:* 2 hilos A/B, hasta 32 nodos (ampliable), 10 Mbit/s (corto), ~1200 m a 100 kbit/s, terminación 120 Ω.  
  - *Aplicaciones:* Modbus RTU, BMS, variadores, DAQ, control de acceso, DMX512, *smart metering*.  
  - *Ventajas/limitaciones:* bajo costo, robusto; half‑duplex típico, requiere protocolo superior.
- **RS‑232**  
  - *Descripción:* serial clásico *single‑ended*; legado vigente para configuración/programación local.  
  - *Técnico:* 300–115.2 kbit/s; 15 m; DB9/DB25; señales TX/RX/RTS/CTS, etc.

### 2) Proyecto: Control de Servo con RS‑485 y Raspberry Pi
- **Modos de comunicación**  
  - *Simplex:* unidireccional; mayor *throughput* aparente pero sin verificación.  
  - *Half‑duplex:* bidireccional alternado; recomendado (equilibrio costo‑confiabilidad).  
  - *Full‑duplex:* bidireccional simultáneo; menor latencia, mayor complejidad/cableado.
- **Configuración de sistema**  
  - *Hardware:* Raspberry Pi 4, MAX485, servo con Modbus RTU, 24 VDC, par trenzado blindado, terminaciones 120 Ω.  
  - *SO y UART:* liberar UART0 (deshabilitar Bluetooth), `enable_uart=1`, `core_freq=250`, retirar consola serie.  
- **Implementación en Python**  
  - Cálculo CRC16 Modbus, control DE/RE, envío de `Write Single Register (0x06)` y lectura con `Read Holding Registers (0x03)`; verificación de eco y CRC.
- **Pruebas y métricas (500 comandos)**  
  - *Simplex:* 23.6 cmd/s; sin confirmación, sin detección de errores.  
  - *Half‑duplex:* 98.2% éxito; latencia ~52 ms; robusto con verificación CRC.  
  - *Full‑duplex:* 99.6% éxito; latencia ~28 ms; mayor costo/cableado.
- **Problemas y soluciones**  
  - *Turnaround MAX485:* añadir *delays* (~1 ms) al cambiar TX↔RX.  
  - *EMI/CRC:* usar STP, doble terminación 120 Ω, distancias a potencia, blindaje a tierra en un punto, reducir *baudrate*.  
  - *UART/Bluetooth:* asignar UART0 a GPIO; deshabilitar servicios BT.  
  - *Orden de bytes CRC:* usar *little‑endian* (CRC_L, CRC_H).

### 3) Recomendaciones finales
1. Baudrates conservadores (9600–19200) para robustez.  
2. Documentar direcciones/registros Modbus y tolerancias de posición.  
3. Ensayar variaciones de carga mecánica y ruido antes de *deployment*.  
4. Considerar full‑duplex solo si la latencia es crítica.

---

## Referencias (selección)
- PI (PROFIBUS & PROFINET International), IEC 61158, IEEE 802.3.  
- Modbus Application Protocol V1.1b3.  
- Maxim MAX485 Datasheet.  
- Documentación oficial Raspberry Pi — UART.


