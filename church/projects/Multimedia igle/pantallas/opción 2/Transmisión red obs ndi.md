# Opción 2 - Transmisión por red local con OBS, NDI, RTMP, etc.

## 🧩 Concepto

Se utiliza la red local (LAN) para distribuir contenido desde OBS a múltiples dispositivos, utilizando protocolos como **NDI** (NewTek), **RTMP local**, o herramientas como **OBS.Ninja**.

---

### 🛠 Requisitos

- PC con OBS + plugins (NDI, VLC, RTMP).
- TVs **Smart TV con navegador o apps (VLC, YouTube, etc)** o Raspberry Pi por pantalla.
- 1x Switch Gigabit.
- Red cableada con **Cat6 o superior**.
- Servidor RTMP (local o nube) si se usa ese protocolo.
- Plugins OBS Studio para múltiples salidas (Multi-RTMP, NDI Tools, etc.).

---

## 🖥️ Diagrama de red
[[Pantallas 2.canvas|Pantallas 2]]

    PC -->|LAN| SW[Switch Gigabit]
    
    SW -->|LAN| TV1[Smart TV - Hall]
    SW -->|LAN| TV2[Smart TV - Auditorio]
    SW -->|LAN| TV3[Smart TV - Hall]
    SW -->|LAN| TV4[Smart TV - Auditorio]
    SW -->|LAN| TV5[Smart TV - Conferencista]
    SW -->|LAN| TV6[Smart TV - Proyector]

    PC -->|NDI/RTMP/VLC| TV1
    PC -->|NDI/RTMP/VLC| TV2
    PC -->|NDI/RTMP/VLC| TV3
    PC -->|NDI/RTMP/VLC| TV4
    PC -->|NDI/RTMP/VLC| TV5
    PC -->|NDI/RTMP/VLC| TV6

## 🔧 Configuración OBS (PC 1)

- Instalar OBS NDI Plugin
- Crear una escena para las letras.
- Activar NDI Output (Tools > NDI Output Settings).
- Nombrar la salida como “Letras-TV”.
- Las Raspberry o dispositivos deben tener un receptor NDI (por ejemplo: NDI Monitor, NDI HX Player, o navegador con OBS.Ninja).

## ✅ Ventajas

- Permite contenido diferente por pantalla si se desea (configurable).
- No requiere hardware físico adicional (más económico si ya hay Smart TVs).
- Escalable.

## ❌ Desventajas

- Requiere configuración avanzada.
- Puede haber algo de latencia según red.
- Necesita buen rendimiento de red local.

## 🧠 Alternativas de transmisión

- OBS.Ninja: Compartís una escena como link de navegador. Las TVs (si tienen navegador) pueden acceder directamente.
- RTMP local con nginx: Se monta un servidor interno RTMP para que OBS emita, y las TVs accedan al stream.

## 📝 Recomendación

- Usar esta opción si tenés TVs smart o Raspberry Pi.
- Muy útil si querés controlar remotamente qué se ve en cada pantalla.
- Aconsejable tener todo por cable Ethernet y switch Gigabit.