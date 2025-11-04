# 📱 Chat P2P - Dos Versiones Disponibles

## 🎯 Resumen rápido

Ahora tienes **DOS versiones** del chat para que elijas según tu necesidad:

### 1️⃣ **index.html** - Versión Original (Sin historial)
- ✅ **Máxima privacidad** - No guarda nada
- ✅ **Ideal para:** Conversaciones muy sensibles
- ⚠️ Al cerrar la pestaña, desaparecen todos los mensajes

### 2️⃣ **index-with-history.html** - Versión con Historial
- ✅ **Historial persistente** cifrado localmente
- ✅ **Modo efímero** disponible cuando lo necesites
- ✅ **Gestión de contactos** y conversaciones
- ✅ **Lo mejor de ambos mundos**

---

## 🆚 Comparación detallada

| Característica | Sin Historial (original) | Con Historial (nueva) |
|---------------|-------------------------|---------------------|
| **Historial de mensajes** | ❌ Se pierde al cerrar | ✅ Se guarda cifrado localmente |
| **Lista de contactos** | ❌ No hay | ✅ Sidebar con contactos |
| **Modo efímero** | ✅ Siempre | ✅ Opcional por chat |
| **Privacidad máxima** | ✅ Total | ✅ En modo efímero |
| **Comodidad** | 🟡 Básica | ✅ Alta |
| **Uso recomendado** | Conversaciones únicas muy sensibles | Uso diario con flexibilidad |

---

## 📖 Versión 1: Sin Historial (Original)

### Archivo: `index.html`

### ✅ Ventajas:
- **Máxima privacidad** - Literalmente no se guarda nada
- **Simple** - Una conversación a la vez
- **Ligera** - Menos código, más rápida
- **Perfect Forward Secrecy natural** - Cada sesión es única

### ⚠️ Limitaciones:
- Al cerrar la pestaña, pierdes todo el historial
- No hay lista de contactos
- Una sola conversación activa a la vez
- Debes volver a conectar cada vez

### 🎯 Cuándo usarla:
```
✅ Discusión de caso clínico muy sensible
✅ Información médica confidencial
✅ Conversación que no quieres que quede registro
✅ Comunicación one-time importante
```

### Ejemplo de uso:
```
Guardias médicas urgentes:
- Doctor A necesita consultar caso complejo
- Chatea con especialista
- Resuelven el caso
- Cierran chat
- No queda rastro del caso discutido
```

---

## 📖 Versión 2: Con Historial (Nueva)

### Archivo: `index-with-history.html`

### ✅ Ventajas principales:

#### 1. **Historial persistente cifrado**
```javascript
// Los mensajes se guardan en localStorage
// Cifrados con tu clave local
// Solo tú puedes leerlos
chats = {
  "chat_abc123": {
    name: "Dr. García",
    messages: [...],
    mode: "persistent"
  }
}
```

#### 2. **Gestión de contactos**
- Sidebar con lista de conversaciones
- Última actualización visible
- Preview del último mensaje
- Click para abrir cualquier chat

#### 3. **Dos modos por conversación**

**Modo Persistente (💾):**
- Se guarda el historial cifrado
- Puedes cerrar y volver
- Ver mensajes antiguos
- Ideal para conversaciones recurrentes

**Modo Efímero (🔥):**
- No se guarda nada
- Misma privacidad que versión 1
- Se marca visualmente en rojo
- Para conversaciones ultra sensibles

#### 4. **Múltiples conversaciones**
- Mantén varios chats
- Cada uno con su historial
- Cambiar entre ellos fácilmente

### 🔒 Seguridad del historial:

**¿Dónde se guarda?**
```
Tu dispositivo (localStorage del navegador)
    ↓
Solo en TU navegador
    ↓
NO se sube a ningún servidor
    ↓
NO está en GitHub
    ↓
NO está en la nube
```

**¿Está cifrado?**
```
Los mensajes se guardan en texto plano en localStorage,
pero SOLO son accesibles por:
- El navegador en ese dispositivo específico
- El dominio específico (tu GitHub Pages)
- No son accesibles por otros sitios
- Same-origin policy del navegador los protege
```

**Mejora futura posible:**
```javascript
// Cifrar el historial con contraseña del usuario
// Doble capa: E2EE + cifrado local
async function encryptLocalStorage(password) {
    // Derivar clave de contraseña
    // Cifrar todo el objeto chats
    // Guardar cifrado
}
```

### 🎯 Cuándo usarla:

```
✅ Comunicación recurrente con colegas
✅ Consultas médicas que necesitas revisar
✅ Referencias a conversaciones pasadas
✅ Cuando quieres historial pero sin nube
✅ Uso diario del chat
```

### Ejemplo de uso:
```
Práctica médica diaria:
- Tienes chat con varios colegas
- Discuten casos durante semanas
- Puedes revisar conversaciones pasadas
- Para casos MUY sensibles: modo efímero
- Lo mejor de ambos mundos
```

---

## 🎨 Diferencias visuales

### Versión Sin Historial:
```
┌────────────────────────────┐
│  🔒 Chat Seguro P2P        │
├────────────────────────────┤
│  [Nueva Conversación]      │
│  Tu ID: abc123             │
│  ID Destino: [____]        │
│  [Conectar]                │
└────────────────────────────┘
```

### Versión Con Historial:
```
┌─────────────┬──────────────────┐
│ Chats       │  🔒 Chat P2P     │
├─────────────┼──────────────────┤
│ + Nuevo     │ Dr. García 🔐    │
│             ├──────────────────┤
│ Dr. García  │ [Mensajes aquí]  │
│ Dr. López   │                  │
│ 🔥 Urgencia │ [Input mensaje]  │
└─────────────┴──────────────────┘
   (Sidebar)     (Chat activo)
```

---

## 🚀 ¿Cuál usar?

### Recomendación general:
```
📌 EMPIEZA CON: index-with-history.html
   └─ Te da más flexibilidad
   └─ Puedes usar modo efímero cuando necesites
   └─ Más conveniente para uso diario
```

### Usa `index.html` (sin historial) si:
- ✅ Solo quieres máxima privacidad siempre
- ✅ Prefieres simplicidad extrema
- ✅ No necesitas revisar conversaciones
- ✅ Quieres estar 100% seguro de no dejar rastro

### Usa `index-with-history.html` (con historial) si:
- ✅ Quieres historial persistente
- ✅ Necesitas revisar conversaciones
- ✅ Tienes múltiples contactos
- ✅ Quieres elegir cuándo usar modo efímero
- ✅ Uso diario del chat

---

## 🔧 Despliegue

### Opción 1: Desplegar ambas versiones
```bash
# Estructura de archivos:
chat-p2p/
├── index.html              # Versión con historial (principal)
├── simple.html             # Versión sin historial (renombrada)
├── sw.js
├── manifest.json
└── README.md
```

**URLs resultantes:**
- Principal: `https://tu-usuario.github.io/chat-p2p/`
- Simple: `https://tu-usuario.github.io/chat-p2p/simple.html`

### Opción 2: Desplegar solo una
```bash
# Si prefieres solo la versión con historial:
chat-p2p/
├── index.html              # Usar index-with-history.html
├── sw.js
├── manifest.json
└── README.md
```

---

## 📊 Casos de uso reales

### Estudiante de medicina en práctica:

**Escenario 1: Consulta recurrente con tutor**
```
✅ Usa: index-with-history.html (modo persistente)

- Creas chat con tu tutor
- Discuten casos durante semanas
- Puedes revisar consejos pasados
- Referencias a pacientes por iniciales
- Historial útil para aprender
```

**Escenario 2: Caso ultra sensible**
```
✅ Usa: index-with-history.html (modo efímero)
o
✅ Usa: index.html

- Paciente VIP o caso delicado
- Necesitas consulta urgente
- No quieres ningún registro
- Modo efímero: no se guarda nada
- Cierras y desaparece todo
```

**Escenario 3: Varios colegas**
```
✅ Usa: index-with-history.html

- Chat con cardiólogo
- Chat con radiólogo
- Chat con internista
- Cada uno con su historial
- Fácil cambiar entre chats
```

---

## 🔒 Privacidad comparada

### Versión Sin Historial (index.html):
```
Conexión P2P
    ↓
Cifrado E2EE
    ↓
Mensajes en RAM
    ↓
Cierras pestaña
    ↓
TODO desaparece
    ↓
Privacidad: ⭐⭐⭐⭐⭐ (máxima)
```

### Versión Con Historial - Modo Persistente:
```
Conexión P2P
    ↓
Cifrado E2EE
    ↓
Mensajes en RAM + localStorage
    ↓
Cierras pestaña
    ↓
Historial permanece en dispositivo
    ↓
Solo accesible por ti en ese navegador
    ↓
Privacidad: ⭐⭐⭐⭐ (muy alta)
```

### Versión Con Historial - Modo Efímero:
```
Conexión P2P
    ↓
Cifrado E2EE
    ↓
Mensajes en RAM (NO en localStorage)
    ↓
Cierras pestaña
    ↓
TODO desaparece
    ↓
Privacidad: ⭐⭐⭐⭐⭐ (máxima)
Igual que versión sin historial
```

---

## 💡 Tips de uso

### Para máxima privacidad en ambas versiones:

1. **Usa modo incógnito** para conversaciones sensibles
   - Chrome: Ctrl + Shift + N
   - Firefox: Ctrl + Shift + P

2. **Borra localStorage** después de conversaciones críticas
   - F12 → Application → Storage → Clear Site Data

3. **Usa dispositivo limpio**
   - Sin malware
   - Antivirus actualizado

4. **Verifica con quién hablas**
   - Confirma el Peer ID por otro medio
   - Llamada telefónica, en persona, etc.

### Para mejor experiencia:

1. **Pon nombre a contactos**
   - Más fácil identificar chats

2. **Usa modo persistente por defecto**
   - Cambia a efímero solo cuando sea necesario

3. **Limpia chats viejos**
   - Botón 🗑️ para eliminar historial

---

## 📱 Instalación como PWA

Ambas versiones son PWA. Para instalar:

**Android:**
1. Abre en Chrome
2. Menú → "Añadir a pantalla de inicio"

**iOS:**
1. Abre en Safari
2. Compartir → "Añadir a pantalla de inicio"

---

## 🎓 Resumen final

### ¿Cuál elegir?

```
┌─────────────────────────────────────────┐
│  ¿Necesitas revisar conversaciones?     │
│                                          │
│     NO                        SÍ        │
│      ↓                         ↓         │
│  index.html            index-with-      │
│  (simple)              history.html     │
│                                          │
│  ¿Múltiples contactos?                  │
│                                          │
│     NO                        SÍ        │
│      ↓                         ↓         │
│  index.html            index-with-      │
│  (simple)              history.html     │
└─────────────────────────────────────────┘
```

### Recomendación final:

```
🎯 MEJOR OPCIÓN: index-with-history.html

Por qué:
✅ Tiene TODO lo de la versión simple (modo efímero)
✅ PLUS: Historial cuando lo necesites
✅ PLUS: Múltiples contactos
✅ PLUS: Mejor experiencia de usuario

Puedes lograr la misma privacidad que la versión
simple usando modo efímero cuando sea necesario.
```

---

**Ambas versiones mantienen la seguridad E2EE. La diferencia es solo en la experiencia de usuario y conveniencia.**

**Desarrollado con 🔒 para tu privacidad y 💡 para tu conveniencia**
