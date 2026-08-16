# LOESINA v4.0 — Sistema Inteligente de Asistencia

Sistema completo de pase de lista escolar con RFID UHF (YRM100) + ESP32 + asistente de IA (LESI).

**Esta versión incluye precargado el grupo 207 INFORMÁTICA con 35 alumnos y sus fotografías.**

---

## 🚀 Inicio rápido

### 1. Servir la app localmente (recomendado)

Las funciones de Bluetooth, audio y cámara solo funcionan en `https://` o `localhost`. Para probar todo, sirve la carpeta con un servidor local.

**Opción A — VS Code (más fácil):**
1. Instala la extensión "Live Server"
2. Click derecho sobre `index.html` → **"Open with Live Server"**
3. Se abre automáticamente en `http://localhost:5500`

**Opción B — Python:**
```bash
cd loesina-v4
python -m http.server 8000
```
Luego abre: http://localhost:8000

**Opción C — Node.js:**
```bash
cd loesina-v4
npx serve .
```

### 2. Primer arranque

1. Abre la app
2. Presiona **"Crear Cuenta"** y registra tu cuenta de docente:
   - Nombre completo
   - Plantel / Escuela
   - Correo electrónico
   - Contraseña (mínimo 6 caracteres)
3. **Al crear la cuenta se carga automáticamente:**
   - Grupo: **207 INFORMÁTICA**
   - 35 alumnos con sus matrículas
   - 27 fotografías asociadas a sus alumnos
   - Los 8 alumnos sin foto aparecen con sus iniciales

---

## 📁 Estructura del proyecto

```
loesina-v4/
├── index.html               ← archivo principal
├── manifest.webmanifest     ← PWA
├── README.md                ← este archivo
├── css/
│   ├── style.css           ← estilos
│   └── animations.css      ← animaciones
├── js/
│   ├── config.js           ← configuración (IPs, Supabase, etc.)
│   ├── utils.js            ← utilidades
│   ├── store.js            ← almacenamiento local
│   ├── icons.js            ← íconos SVG
│   ├── toast.js            ← notificaciones flotantes
│   ├── modals.js           ← modales (alumno, grupo, confirmar)
│   ├── auth.js             ← autenticación
│   ├── app.js              ← navegación
│   ├── dashboard.js        ← pantalla principal
│   ├── classes.js          ← gestión de grupos y alumnos
│   ├── attendance.js       ← pase de lista
│   ├── rfid.js             ← WiFi + Bluetooth BLE
│   ├── lesi.js             ← chat con IA
│   ├── analytics.js        ← reportes y análisis
│   ├── notifications.js    ← centro de notificaciones
│   ├── settings.js         ← configuración
│   ├── initial-data.js     ← ★ datos de los 35 alumnos
│   └── init.js             ← arranque
└── docs/
    ├── MIGRACION_SUPABASE.md   ← cómo migrar a la nube
    ├── FIRMWARE_ESP32.md       ← código para el ESP32
    └── CHANGELOG.md            ← cambios vs v3
```

---

## ⚡ Funcionalidades

### Gestión de alumnos
- **Foto** de perfil (se sube desde cámara o galería, se comprime automáticamente)
- **Matrícula** única
- **Número de lista**
- **UID RFID** (se asigna escaneando la credencial)
- **Notas** opcionales
- Drag & drop para reordenar alumnos
- 3 vistas: lista, tarjetas, estadísticas
- Búsqueda por nombre, matrícula o UID

### Pase de lista (la pantalla principal)
- **Foto a color** = alumno presente (con resaltado verde)
- **Foto en escala de grises** = alumno ausente
- **Foto con tinte naranja** = alumno tarde
- Timer en vivo durante la sesión
- Contadores de presentes / tarde / ausentes
- Barra de progreso en tiempo real
- Click en cualquier alumno para cambiar su estado
- Botón "Todos presentes" para registrar rápido
- Exportación a TXT con resumen

### RFID (ESP32 + YRM100)
- **Modo WiFi**: WebSocket en tu red local
- **Modo Bluetooth BLE**: Web Bluetooth API
- Anti-duplicados (no registra el mismo UID dos veces en 3 segundos)
- Botón "Simular" para probar sin hardware

### LESI (asistente IA)
- Chat conversacional sobre tus datos de asistencia
- Respuestas locales básicas si no hay backend configurado
- Sugerencias rápidas (chips)
- Para activar IA real con Claude: ver `docs/MIGRACION_SUPABASE.md`

### Analytics
- Resumen general con métricas
- Detección automática de alumnos en riesgo (<70%)
- Reportes generados (con o sin IA)

### Backup
- Exportar/importar todo como JSON (incluye fotos)
- Exportar grupo a CSV (compatible con Excel)
- Backup automático antes de cada guardado

---

## 🔧 Hardware

- **ESP32-WROOM-32 DevKit** (cualquier ESP32 funciona)
- **YRM100 UHF RFID reader** (902-928 MHz, TTL 3.3V, UART)
- **5 cables Dupont hembra-hembra**
- **Credenciales RFID UHF** (EPC Gen2) para los alumnos

### Conexiones YRM100 → ESP32

| YRM100 | Color | ESP32 |
|--------|-------|-------|
| VCC    | Rojo  | 3V3   |
| GND    | Negro | GND   |
| TXD    | Azul  | GPIO16 (RX2) |
| RXD    | Verde | GPIO17 (TX2) |
| EN     | Morado| GPIO4 |

Ver `docs/FIRMWARE_ESP32.md` para el código del Arduino IDE.

---

## 🎓 Datos cargados (grupo 207 INFORMÁTICA)

**35 alumnos** del grupo, con la siguiente lista:

| Con foto (27) | Sin foto (8) |
|--------------|--------------|
| BARAJAS RIVERA GAEL EDUARDO | ARNAVE RAMIREZ MARIA JOSE |
| BAUTISTA MORALES DIANA LIZETH | GARCIA ROJAS EDUARDO ADRIEL |
| BELTRAN GALLARDO LIONEL USAIN | GARCIA RAMOS ANA LIZBETH |
| CASANON RODRIGUEZ WENDY PATRICIA | MARTINEZ GUERRERO GUILLERMO |
| CHAVEZ FERNANDEZ DIEGO ABRAHAM | MORALES SANTIAGO SANDRA VANESSA |
| CHAVEZ FERNANDEZ HELLEN MARISSA | ORTA LEAL VICTORIA JAZMIN |
| ESTEVEZ BUSTAMANTE SOLEDAD | SANCHEZ OLVERA BRAYAN ANTONIO |
| FLORES DE LA CRUZ ESLY ROSALIA | SANCHEZ OLVERA EDWARD RAFAEL |
| GARCIA RAMOS VALENTINA NICOLE | |
| HERNANDEZ CORONADO PILAR AIDEE | |
| HERNANDEZ GARCIA ESTRELLA GUADALUPE | |
| HERNANDEZ HERNANDEZ ALEJANDRO | |
| HERNANDEZ MEDINA JOSE ANGEL | |
| LOPEZ BAUTISTA DANIEL | |
| MACIAS MONTALVO MATIAS ANTONIO | |
| MARCOS RAMOS CHRISTOPHER JESUS | |
| MARTINEZ HERNANDEZ JULIO CESAR | |
| MATIAS GARCIA LESLIE EMIRETH | |
| MENDEZ ANGEL ANA ABIGAIL | |
| MONREAL SALDAÑA PAULINA JAZMIN | |
| NAJERA MORIN TERESA ABIGAIL | |
| PADILLA MARTINEZ FRANCES DAILYN | |
| PEREZ LEON ALBERTO ANGEL | |
| RAMIREZ MEZA STEPHANIE | |
| SANCHEZ MASCAREÑAS DARINE GISSELL | |
| SANCHEZ GARCIA EVELYN DENISSE | |
| VENTURA BENITO RUTH JARLENY | |

Para los alumnos sin foto, simplemente edítalos en la app y sube su fotografía.

---

## ➕ Agregar más alumnos o grupos

### Agregar un alumno nuevo
1. Abre el grupo → Botón **"+ Alumno"**
2. **Sube su foto** desde el celular (se comprime automáticamente)
3. Pon su **matrícula**, **número** y **nombre completo**
4. Si tienes su credencial RFID:
   - Conecta el lector
   - Click en **"Escanear"**
   - Acerca la credencial al lector → el UID se llena solo
5. Guardar

### Agregar un grupo nuevo
1. En el dashboard → **"+ Agregar Grupo"**
2. Pon nombre, materia, horario, color
3. Crear

---

## 📱 Instalación como app (PWA)

La app puede instalarse como aplicación nativa:

1. Abre la app en Chrome (Android) o Safari (iOS)
2. Menú del navegador → **"Agregar a pantalla de inicio"**
3. Aparecerá como ícono en tu escritorio y se comportará como app nativa

---

## ⚠️ Notas importantes

- **Almacenamiento**: los datos se guardan en `localStorage` del navegador (~5-10 MB disponibles). Las 27 fotos ocupan ~280 KB en total. Hay espacio para muchas fotos más.
- **Backup regular**: ve a Configuración → "Exportar todo" cada cierto tiempo. Te genera un JSON con TODO (alumnos, fotos, sesiones).
- **Web Bluetooth** solo funciona en Chrome, Edge y Opera. iOS Safari NO soporta BLE — usa modo WiFi.
- **Migración a la nube**: cuando quieras dejar de depender de tu computadora, sigue `docs/MIGRACION_SUPABASE.md` para mover todo a Supabase (gratis).

---

## 🐞 Solución de problemas

### "No se cargaron los alumnos al crear cuenta"
- Borra el `localStorage` (Configuración → "Borrar todos los datos")
- Recarga la página
- Crea cuenta nuevamente

### "La foto no se sube"
- Verifica que el archivo sea una imagen válida (.jpg, .png)
- Tamaño máximo: 10 MB (la app la comprime automáticamente a ~10 KB)

### "No conecta al ESP32 por WiFi"
- Asegúrate de que tu computadora/celular y el ESP32 estén en la misma red WiFi (2.4 GHz)
- Verifica la IP en Configuración → Lector RFID
- En la consola del navegador (F12), busca errores de WebSocket

### "BLE no encuentra el dispositivo"
- Solo funciona en Chrome/Edge/Opera con HTTPS o localhost
- En Android activa GPS (a veces es requerido para escanear BLE)
- iOS Safari NO soporta — usa Bluefy o cambia a modo WiFi

---

## 📝 Licencia

Proyecto académico de Daniel Rivera (Monterrey, MX) — uso educativo libre.

## 🙏 Créditos

- LOESINA es proyecto de Dany
- Asistido por Claude (Anthropic)
- Datos del grupo: 207 INFORMÁTICA — CONALEP
- RFID UHF: YRM100 (Hangzhou Yanzeo Sensing)
- Microcontrolador: Espressif ESP32
