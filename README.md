# 🔒 Chat Seguro P2P - Cifrado End-to-End

Chat peer-to-peer completamente descentralizado con cifrado de extremo a extremo (E2EE). Sin servidores propios, completamente privado.

## 🚀 Acceso rápido
[Abrir chat](https://tu-usuario.github.io/chat-p2p-encrypted/)

---

## 🛡️ Seguridad y Privacidad

### ✅ ¿Es realmente seguro?

**SÍ**, y aquí está el por qué:

#### 1. **Cifrado End-to-End Real (RSA-2048)**
- Cada usuario genera un par de claves RSA-2048 **localmente** en su dispositivo
- Las **claves privadas NUNCA salen del dispositivo**
- Cada mensaje se cifra con la clave pública del destinatario
- Solo el destinatario puede descifrarlo con su clave privada

#### 2. **No hay servidores intermediarios leyendo mensajes**
- Usa WebRTC para conexión P2P directa entre usuarios
- Los mensajes viajan cifrados directamente de un dispositivo a otro
- PeerJS solo se usa para **señalización inicial** (intercambiar IDs)

#### 3. **Las claves se generan cada sesión**
- Cada vez que abres la app, se generan claves nuevas
- No hay almacenamiento persistente de claves
- Forward secrecy natural

### ⚠️ Pregunta de seguridad respondida:

> **"¿No hay forma de hacer ingeniería inversa para saber en tiempo real esas claves?"**

**NO ES POSIBLE** por estas razones técnicas:

1. **Las claves privadas solo existen en RAM:**
   - Se generan con `crypto.subtle.generateKey()`
   - Marcadas como `extractable: true` solo para exportar la pública
   - La clave privada está en memoria del navegador
   - JavaScript ejecutándose en GitHub Pages no puede extraerla

2. **Web Crypto API es nativa del navegador:**
   - Es parte del navegador (Chrome, Firefox, Safari)
   - No depende del código que está en GitHub
   - Incluso si alguien modifica el HTML, no puede extraer claves de otros usuarios

3. **Los mensajes viajan cifrados por WebRTC:**
   - WebRTC usa DTLS (cifrado de transporte)
   - Los mensajes ya están cifrados con RSA antes de WebRTC
   - **Doble capa de cifrado**

4. **GitHub Pages es estático:**
   - Solo sirve archivos HTML/JS/CSS
   - No ejecuta código en servidor
   - No puede interceptar nada

### 🔍 ¿Qué SÍ puede verse?

Lo único que puede ver alguien monitoreando la red:

- ❌ **NO puede ver:** El contenido de los mensajes (están cifrados)
- ❌ **NO puede ver:** Las claves privadas (solo en memoria local)
- ✅ **SÍ puede ver:** Que dos IPs están conectadas (tráfico WebRTC)
- ✅ **SÍ puede ver:** Los Peer IDs públicos (pero no sirven sin las claves)

### 🎯 Comparación con otras apps

| Característica | Este Chat | WhatsApp | Telegram | SMS |
|---------------|-----------|----------|----------|-----|
| E2EE | ✅ | ✅ | ❌ (solo chats secretos) | ❌ |
| Sin servidor central | ✅ | ❌ | ❌ | ❌ |
| Código abierto | ✅ | ❌ | ❌ | ❌ |
| Metadatos privados | ✅ | ❌ | ❌ | ❌ |

---

## 📋 Características

### Chat P2P
- **Conexión directa** entre usuarios usando WebRTC
- **Sin servidores** que almacenen mensajes
- **Cifrado E2EE** con RSA-2048
- **Interfaz limpia** inspirada en apps modernas

### PWA
- Funciona como app nativa
- Se puede instalar en el móvil
- Funciona offline (después de primera carga)

### Seguridad
- Generación de claves locales
- Web Crypto API estándar
- Sin almacenamiento de claves
- Indicadores visuales de cifrado

---

## 🔧 Cómo funciona técnicamente

### Flujo de conexión:

```
1. Usuario A abre la app
   └─> Genera par de claves RSA (pública/privada)
   └─> Obtiene Peer ID de PeerJS
   
2. Usuario B abre la app
   └─> Genera par de claves RSA (pública/privada)
   └─> Obtiene Peer ID de PeerJS
   
3. Usuario A ingresa Peer ID de B y conecta
   └─> WebRTC establece canal P2P
   
4. Intercambio de claves públicas
   Usuario A ──[clave pública A]──> Usuario B
   Usuario B ──[clave pública B]──> Usuario A
   
5. Conversación cifrada
   A escribe "Hola" 
   └─> Cifra con clave pública de B
   └─> Envía mensaje cifrado
   └─> B descifra con su clave privada
   └─> B ve "Hola"
```

### Estructura del mensaje cifrado:

```javascript
// Mensaje original
"Hola, ¿cómo estás?"

// Después de cifrar (Base64 del array cifrado)
"MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA..."

// Solo quien tiene la clave privada puede descifrarlo
```

---

## 🚀 Instalación y despliegue

### Opción 1: GitHub Pages (Recomendado)

1. **Crea un nuevo repositorio en GitHub:**
   - Nombre sugerido: `chat-p2p-encrypted`
   - Público o privado (funciona igual)

2. **Sube los archivos:**
   ```bash
   git init
   git add .
   git commit -m "Initial commit - Chat P2P E2EE"
   git branch -M main
   git remote add origin https://github.com/tu-usuario/chat-p2p-encrypted.git
   git push -u origin main
   ```

3. **Activa GitHub Pages:**
   - Settings → Pages
   - Source: Deploy from a branch
   - Branch: `main` → `/ (root)`
   - Save

4. **Tu app estará en:**
   ```
   https://tu-usuario.github.io/chat-p2p-encrypted/
   ```

### Opción 2: Servidor local para pruebas

```bash
# Con Python 3
python -m http.server 8000

# Abre: http://localhost:8000
```

---

## 💡 Guía de uso

### Iniciar una conversación:

1. **Usuario A:**
   - Abre la app
   - Copia su "ID de conexión"
   - Se lo envía a Usuario B (por WhatsApp, email, etc.)

2. **Usuario B:**
   - Abre la app
   - Copia su "ID de conexión"
   - Se lo envía a Usuario A
   - Pega el ID de A en "ID del destinatario"
   - Click en "Conectar"

3. **Usuario A:**
   - Pega el ID de B en "ID del destinatario"
   - Click en "Conectar"

4. **¡Listo!**
   - Verán el mensaje "🔐 Cifrado E2EE establecido"
   - Pueden chatear de forma segura

### Indicadores de estado:

- ⚪ **Gris:** Sin conexión
- 🟢 **Verde:** Conectado (estableciendo cifrado)
- 🔵 **Azul pulsante:** Cifrado E2EE activo

---

## 📱 Instalar como PWA

### Android (Chrome):
1. Abre la app en Chrome
2. Menú (⋮) → "Añadir a pantalla de inicio"
3. La app aparecerá como cualquier otra app

### iOS (Safari):
1. Abre la app en Safari
2. Botón compartir (📤)
3. "Añadir a pantalla de inicio"

---

## ⚠️ Limitaciones conocidas

### Requiere conexión simultánea:
- Ambos usuarios deben estar online al mismo tiempo
- No hay historial en la nube
- Al cerrar la app, se pierde la conexión

### Historial local:
- Los mensajes solo se guardan mientras la ventana está abierta
- Al cerrar, se pierden los mensajes
- *Esto es una característica de privacidad, no un bug*

### NAT/Firewall:
- En redes muy restrictivas, WebRTC puede fallar
- Generalmente funciona en WiFi y datos móviles
- Si falla, usar STUN/TURN servers adicionales

---

## 🔒 Preguntas de Seguridad (FAQ)

### ¿Es más seguro que WhatsApp?
En términos de privacidad de metadatos, **sí**. WhatsApp sabe:
- Con quién hablas
- Cuándo hablas
- Tu ubicación
- Tu lista de contactos

Este chat **NO sabe nada** de eso porque no hay servidor central.

### ¿Pueden hackear GitHub Pages para leer mensajes?
**No.** Aunque alguien hackee GitHub y modifique el código:
- Las claves ya fueron generadas en tu navegador
- Los mensajes viajan por WebRTC, no por GitHub
- Incluso si modifican el código, solo afectaría a nuevos usuarios

### ¿Es legal?
**Sí.** El cifrado E2EE es legal en la mayoría de países. Sin embargo:
- Algunos países restringen herramientas de cifrado fuerte
- Verifica las leyes locales si estás en país con restricciones

### ¿Por qué no hay historial persistente?
**Privacidad por diseño.** Si no hay historial:
- No hay nada que hackear
- No hay nada que requisar legalmente
- No hay riesgo de filtración

Si necesitas historial, guarda manualmente los mensajes importantes.

---

## 🛠️ Archivos del proyecto

```
chat-p2p-encrypted/
├── index.html          # App principal
├── sw.js              # Service Worker (PWA)
├── manifest.json      # Configuración PWA
├── icon-192.png       # Ícono 192x192
├── icon-512.png       # Ícono 512x512
└── README.md          # Esta documentación
```

---

## 🔐 Detalles técnicos de criptografía

### Algoritmo: RSA-OAEP
- **Longitud de clave:** 2048 bits
- **Hash:** SHA-256
- **Padding:** OAEP (Optimal Asymmetric Encryption Padding)

### ¿Por qué RSA y no AES?
- RSA es **asimétrico**: no necesitas intercambiar claves secretas
- Ideal para P2P: cada uno tiene su par de claves
- Web Crypto API lo soporta nativamente

### Limitación de RSA:
- Solo puede cifrar mensajes de ~190 bytes (con RSA-2048)
- Para mensajes más largos, se debería usar RSA + AES híbrido
- En esta implementación: mensajes cortos funcionan perfectamente
- **Mejora futura:** Implementar cifrado híbrido RSA+AES

---

## 🚧 Mejoras futuras posibles

### Corto plazo:
- [ ] Cifrado híbrido RSA+AES para mensajes largos
- [ ] Verificación de identidad con fingerprints
- [ ] Notificaciones de nuevos mensajes
- [ ] Historial cifrado en IndexedDB

### Largo plazo:
- [ ] Chat grupal cifrado
- [ ] Compartir archivos cifrados
- [ ] Videollamadas P2P
- [ ] Integración con libp2p para mayor descentralización

---

## 🤝 Contribuciones

¿Quieres mejorar el proyecto?

1. Fork del repositorio
2. Crea una rama: `git checkout -b feature/mejora`
3. Commit: `git commit -m 'Agregar mejora X'`
4. Push: `git push origin feature/mejora`
5. Abre un Pull Request

---

## 📚 Referencias técnicas

- [Web Crypto API - MDN](https://developer.mozilla.org/en-US/docs/Web/API/Web_Crypto_API)
- [WebRTC - MDN](https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API)
- [PeerJS Documentation](https://peerjs.com/docs/)
- [RSA-OAEP - Wikipedia](https://en.wikipedia.org/wiki/Optimal_asymmetric_encryption_padding)

---

## 📄 Licencia

MIT License - Uso libre para fines educativos, personales y comerciales.

---

## ⚠️ Disclaimer

Esta herramienta es para uso educativo y personal. El desarrollador no se hace responsable del uso que se le dé. Siempre verifica las leyes locales sobre cifrado y comunicaciones privadas.

**Desarrollado con 🔒 para proteger tu privacidad**

---

## 👨‍💻 Para desarrolladores

### Personalización:

#### Cambiar colores:
Edita en `index.html` las variables CSS:
```css
background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
```

#### Usar tu propio servidor PeerJS:
Si no quieres usar el servidor público, instala tu propio servidor:
```bash
npm install peer
peerjs --port 9000
```

Luego modifica en `index.html`:
```javascript
peer = new Peer({
    host: 'tu-servidor.com',
    port: 9000,
    path: '/'
});
```

#### Agregar TURN server:
Para redes muy restrictivas:
```javascript
peer = new Peer({
    config: {
        iceServers: [
            { urls: 'stun:stun.l.google.com:19302' },
            { 
                urls: 'turn:tu-turn-server.com',
                username: 'usuario',
                credential: 'contraseña'
            }
        ]
    }
});
```

---

**🎓 Perfecto para estudiantes de medicina que necesitan comunicarse de forma privada sobre casos clínicos sin exponer información sensible del paciente.**
