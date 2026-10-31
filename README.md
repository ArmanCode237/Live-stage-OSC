# 🎛️ Stage OSC — Controlador MIDI & OSC Profesional

**Stage OSC** es una superficie de control táctil multiplataforma (**Windows** y **Android**) de ultra baja latencia diseñada para músicos en vivo, tecladistas, productores e ingenieros de audio. Permite controlar estaciones de trabajo de audio digital (especialmente **REAPER**), sintetizadores e instrumentos virtuales mediante **OSC por red IP (UDP)**, **MIDI por cable USB-C** y **Bluetooth LE MIDI (BLE-MIDI)**.

---

## ✨ Características Principales

### 📡 Conectividad Triple (OSC + USB-C + Bluetooth)
* **Motor OSC / UDP Bidireccional:**
  * Envío de rutas OSC personalizadas (`/track/1/volume`, `/fxparam/...`) con normalización automática `n/` para curvas suaves en REAPER.
  * Emulación de teclado virtual MIDI sobre OSC (`/vkb_midi/...`).
  * **Feedback en tiempo real (RX):** Los faders, knobs y botones se actualizan automáticamente en pantalla cuando mueves los controles dentro de tu DAW.
* **Controlador MIDI Nativo por Cable USB-C:**
  * Integración directa con `Android MidiManager` (*USB Peripheral Port*) y `Web MIDI API` en Windows.
  * Cero latencia y reconocimiento *Plug & Play* como interfaz MIDI física en cualquier DAW.
* **Servidor Periférico Bluetooth LE MIDI (Android):**
  * Convierte tu teléfono o tablet Android en un controlador MIDI inalámbrico oficial (`BLE-MIDI GATT Server`) detectable por macOS y Windows.
* **Modo de Transporte Híbrido:** Permite enviar datos solo por IP (OSC), solo por puerto MIDI directo (USB/BT) o por **ambos simultáneamente**.

### 🎹 Temas de Hardware Realista y Personalización Visual
* **8 Temas Integrados**, incluyendo dos recreaciones detalladas de sintetizadores de escenario:
  * **Nord Stage:** Acabado en madera roja clásica, paneles en azul grisáceo, faders con botón cuadrado negro y **tira LED de 16 segmentos en verde (`#00fa1d`)**, knobs con marcas radiales intercaladas y botones negros con relieve ovalado central.
  * **Yamaha Montage:** Chasis negro grafito futurista, faders y knobs con **anillos/tiras de LEDs cuadrados rojos (`#ff0000`)**, botones cuadrados con marco negro e interior naranja (`#ff5900`), y modo exclusivo **Super Knob** (centro negro, aro azul rey neón `#0055ff` con brillo progresivo y parpadeo dinámico).
  * **Temas de Estudio y Escenario:** *Cyber Cyan*, *OLED Puro*, *Studio Ámbar*, *Escenario Rojo*, *Synth Esmeralda* y *Modo Claro*.
* **Escalado y Zoom Adaptativo:**
  * Zoom del lienzo (`50%` a `175%`) con función **`⤢ Auto-ajustar a pantalla`** para adaptarse a cualquier densidad de píxeles en teléfonos, tablets o monitores táctiles.
  * Escala independiente para barras y menús (`75%` a `130%`).
  * **Modo Escenario (Inmersivo):** Oculta barras del sistema y menús superiores, manteniendo la pantalla encendida durante toda la presentación.

### 🎚️ Controles Modulares y Gestor de Escenas
* **4 Tipos de Controles:** Faders verticales (con arrastre relativo anti-saltos), Knobs rotativos, Botones Toggle (con estética tipo Pad y retención ON/OFF) y Pads de disparo.
* **Modos de Nota Avanzados:** *Continuo / Latch*, *Gate táctil (Note Off al soltar)*, *Barrido / Glissando* y retorno automático por resorte (*Spring*).
* **Organización Completa:** Múltiples escenas, múltiples páginas por escena, cuadrícula magnética de 20px, duplicación rápida y exportación/importación de copias de seguridad en formato `.json`.

---

## 🗂️ Estructura del Proyecto

```text
stage-osc/
├── main.js                  # Proceso principal de Electron (Sockets UDP OSC, IPC, ventana nativa)
├── preload.js               # Puente seguro ContextBridge entre Electron y la interfaz web
├── build-apk.js             # Script opcional de empaquetado automatizado para Android
├── package.json             # Dependencias y scripts de compilación (Electron / Capacitor)
├── src/
│   ├── index.html           # Estructura principal, Inspector flotante y Modales
│   ├── styles.css           # Sistema de diseño, variables responsivas y temas Nord / Montage
│   ├── assets/
│   │   └── icon.png         # Logotipo e icono oficial de la aplicación
│   └── js/
│       ├── app.js           # Inicialización, eventos globales, páginas y escenas
│       ├── state.js         # Estado reactivo, caché de ejecución y persistencia
│       ├── platform.js      # Capa de abstracción híbrida (Electron vs Capacitor Android)
│       ├── network.js       # Comunicación OSC TX/RX y monitor de actividad
│       ├── webMidi.js       # Gestión de puertos USB-C MIDI y periférico Bluetooth LE MIDI
│       ├── midiRouter.js    # Enrutador de mensajes MIDI/OSC y escalado de valores
│       ├── widgets.js       # Renderizado SVG/DOM y motor multitáctil de controles
│       ├── inspector.js     # Panel lateral de edición en vivo y menú contextual
│       └── appearance.js    # Motor de temas, zoom de lienzo y escalado de interfaz
└── android/                 # Proyecto nativo Android (Capacitor + Plugins Java)
    └── app/src/main/java/com/freeosc/studio/
        ├── MainActivity.java        # Modo inmersivo pantalla completa y registro de plugins
        ├── StageOscUdpPlugin.java   # Plugin nativo UDP OSC bidireccional para Android
        └── StageOscMidiPlugin.java  # Plugin nativo USB-C MidiManager + BLE-MIDI GATT Server
```

---

## 🚀 Instalación y Ejecución en Desarrollo (Windows)

### Requisitos previos
* **Node.js** 18 o superior.

### Pasos
1. Clona o descarga este repositorio y abre una terminal en la carpeta raíz.
2. Instala las dependencias del proyecto:
   ```bash
   npm install
   ```
3. Inicia la aplicación en modo escritorio:
   ```bash
   npm start
   ```
4. Para generar el instalador `.exe` de Windows en la carpeta `dist/`:
   ```bash
   npm run build
   ```

---

## 📱 Compilación para Android (Android Studio)

### Requisitos previos
* **Android Studio** (con Android SDK API 34/35 y JDK 17 o 21).
* Capacitor inicializado en el proyecto (`npx cap add android`).

### Pasos para compilar el APK
1. Cada vez que realices cambios en la carpeta `src/`, sincroniza los archivos web con el contenedor nativo de Android:
   ```bash
   npx cap sync android
   ```
2. Abre la carpeta `android/` en **Android Studio** (o ejecuta `npx cap open android`).
3. Verifica que en `app/src/main/java/com/freeosc/studio/` estén presentes:
   * `MainActivity.java`
   * `StageOscUdpPlugin.java`
   * `StageOscMidiPlugin.java`
4. En el menú superior de Android Studio, selecciona:
   **`Build` → `Build Bundle(s) / APK(s)` → `Build APK(s)`**
5. Haz clic en **`Locate`** en la notificación final para obtener tu instalador `.apk`.

---

## 🔌 Guía de Conexión con REAPER / DAW

### 1. Conexión por Wi-Fi o Red USB (OSC Bidireccional)
1. En REAPER, ve a **`Options → Preferences → Control/OSC/Web`**.
2. Haz clic en **`Add`** y selecciona **`OSC (Open Sound Control)`**.
3. Configura:
   * **Pattern config:** `Default`
   * **Mode:** `Configure device IP + local port`
   * **Receive on port:** `8000` (Puerto donde escucha REAPER).
   * **Send to port:** `9000` y en **Device IP** coloca la IP de tu tablet/PC (visible dentro de Stage OSC en `☰ → Red`).
4. En **Stage OSC**, abre **`☰ → Red`**, ingresa la dirección IP de tu computadora, el puerto `8000`, activa **`Escuchar retorno OSC`** en el puerto `9000` y pulsa **Guardar**.

### 2. Conexión por Cable USB-C (Como Dispositivo MIDI Físico)
1. Conecta tu tablet o celular Android a la computadora mediante un cable USB-C.
2. Desliza la barra de notificaciones de Android, toca la notificación de configuración USB y cambia el modo a **"MIDI"** (*Usar USB para MIDI*).
3. En **Stage OSC**, ve a **`☰ → MIDI (USB/Bluetooth)`**:
   * Cambia el modo de envío a **`USB-C / Bluetooth (como dispositivo MIDI)`** (o *Simultáneo*).
   * Selecciona **`[USB-C a PC] USB Peripheral Port`** en la lista y pulsa **Guardar**.
4. En tu DAW, habilita la entrada MIDI correspondiente a tu dispositivo Android.

### 3. Conexión por Bluetooth (BLE-MIDI Inalámbrico)
1. Enciende el Bluetooth y la **Ubicación (GPS)** en tu dispositivo Android (requisito de Android para anunciar servicios BLE).
2. En **Stage OSC**, abre **`☰ → MIDI (USB/Bluetooth)`** y toca **`Activar BT MIDI`**.
3. En tu computadora:
   * **macOS:** Abre *Configuración de Audio MIDI* → *Ventana → Mostrar Estudio MIDI* → haz clic en el icono **Bluetooth** y pulsa **Conectar** junto a tu dispositivo.
   * **Windows:** Utiliza un puente BLE-MIDI (como *MIDIberry* junto con *loopMIDI*) para enrutar el dispositivo Bluetooth hacia REAPER.

---

## ⌨️ Atajos de Teclado (Versión PC)

| Atajo | Acción |
| :--- | :--- |
| **`Ctrl + E`** | Alternar entre Modo Edición (`✎ Editar`) y Modo Directo (`✓ Live`) |
| **`Ctrl +`** / **`Ctrl −`** | Aumentar / Reducir el zoom del lienzo (`5%`) |
| **`Ctrl + 0`** | Restablecer el zoom del lienzo al `100%` |
| **`Supr` (`Delete`)** | Eliminar el control seleccionado (en Modo Edición) |
| **`F11`** | Alternar Pantalla Completa |
| **`Esc`** | Salir del Modo Escenario o cerrar ventanas modales e Inspector |

---

## 📄 Licencia y Créditos

Desarrollado para la comunidad de producción musical e interpretación en directo.