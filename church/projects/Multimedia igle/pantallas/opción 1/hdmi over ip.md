# Opción 1 - Distribución por HDMI Over IP

## 🧩 Concepto

HDMI over IP permite enviar señales de video HDMI a través de una red local usando transmisores y receptores dedicados. Es útil para transmitir contenido sincronizado a múltiples TVs o proyectores sin usar PC intermedias.

---

### 🛠 Requisitos

- PC con **múltiples salidas HDMI** (dedicadas por contenido a transmitir).
- 1x **TX HDMI over IP** por cada salida HDMI. 
- 1x **RX HDMI over IP** por cada pantalla receptora.
- 1x **Switch Ethernet Gigabit** (con VLAN o IGMP Snooping si hay muchos dispositivos).
- Cable de red **Cat6 o superior** armado con estándar **TIA/EIA 568B**.
- Software en PC (OBS, Holyrics, etc.) que maneje múltiples monitores extendidos.

---

## 🖥️ Estructura

[[Pantallas 1.canvas|Pantallas 1]]
    PC -->|HDMI| TX1[TX HDMI #1]
    PC -->|HDMI| TX2[TX HDMI #2]
    PC -->|HDMI| TX3[TX HDMI #3]
    
    TX1 -->|CAT6| SW[Switch Gigabit]
    TX2 -->|CAT6| SW
    TX3 -->|CAT6| SW

    SW -->|CAT6| RX1[RX HDMI #1 - TV1]
    SW -->|CAT6| RX2[RX HDMI #2 - TV2]
    SW -->|CAT6| RX3[RX HDMI #3 - TV3]
    SW -->|CAT6| RX4[RX HDMI #4 - TV4]
    SW -->|CAT6| RX5[RX HDMI #5 - Conferencistas]
    SW -->|CAT6| RX6[RX HDMI #6 - Proyector]

## ✅ Ventajas

- Muy estable.
- Sin latencia visible.
- No requiere software en TVs.

## ❌ Desventajas

- Requiere hardware físico específico.
- No permite personalización por pantalla.
- Más costoso si hay muchas pantallas.