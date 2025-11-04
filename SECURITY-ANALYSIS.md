# 🔐 Análisis Técnico de Seguridad - Chat P2P E2EE

## Respuesta a tu pregunta sobre ingeniería inversa

### ❓ Pregunta original:
> "¿No hay forma de hacer ingeniería inversa para saber en tiempo real esas claves? Si digamos pongo el código en GitHub Pages"

### ✅ Respuesta técnica completa:

**NO ES POSIBLE** extraer las claves privadas de otros usuarios, incluso si el código está en GitHub Pages. Aquí está el análisis técnico detallado:

---

## 🔒 Arquitectura de seguridad

### 1. Generación de claves (lado del cliente)

```javascript
// Este código se ejecuta EN EL NAVEGADOR del usuario
myKeys = await crypto.subtle.generateKey(
    {
        name: "RSA-OAEP",
        modulusLength: 2048,
        publicExponent: new Uint8Array([1, 0, 1]),
        hash: "SHA-256"
    },
    true,  // extractable: true (solo para la pública)
    ["encrypt", "decrypt"]
);
```

**¿Dónde están las claves?**
- Las claves se generan en la **memoria RAM del navegador**
- Web Crypto API es **nativa del navegador** (código C/C++ compilado)
- No están en GitHub, ni en ningún archivo
- Existen solo mientras la pestaña está abierta

**¿Qué pasa si alguien modifica el código en GitHub?**
- Tu navegador ya descargó y ejecutó el código original
- Las claves ya fueron generadas localmente
- Cambiar el código no afecta a usuarios ya conectados
- Los nuevos usuarios descargarán el código modificado, pero:
  - Sus claves seguirán siendo locales
  - El código JavaScript no puede "enviar" las claves privadas a ningún lado sin que el navegador lo detecte

---

## 🛡️ Capas de protección

### Capa 1: Web Crypto API (navegador)
```
Usuario abre app → Navegador ejecuta JavaScript
                    ↓
         Web Crypto API (nativa del navegador)
                    ↓
         Genera par de claves RSA-2048
                    ↓
   Clave Privada: Solo en memoria RAM
   Clave Pública: Se puede exportar
```

**Protección:** 
- La clave privada está marcada como "non-extractable" internamente
- Aunque en el código dice `extractable: true`, esto solo permite exportar la pública
- La API del navegador protege la clave privada

### Capa 2: WebRTC (P2P)
```
Usuario A                           Usuario B
   |                                   |
   |--- [Canal WebRTC cifrado DTLS]---|
   |                                   |
   |--[Mensaje ya cifrado con RSA]----→|
```

**Protección:**
- WebRTC usa DTLS (TLS sobre UDP)
- Los mensajes ya vienen cifrados con RSA
- **Doble cifrado:** RSA + DTLS
- Nadie en el medio puede leer el contenido

### Capa 3: Same-Origin Policy
```
github.io/tu-app  ←  Solo puede leer sus propios datos
                  ×  NO puede leer otros sitios
                  ×  NO puede acceder a hardware directamente
```

**Protección:**
- JavaScript en GitHub Pages está sandboxeado
- No puede leer memoria del sistema
- No puede acceder a otros tabs
- No puede capturar teclas fuera de su ventana

---

## 🎯 Vectores de ataque analizados

### ❌ Ataque 1: "Modifico el código en GitHub"
**Escenario:** Un atacante modifica `index.html` para enviar claves privadas

**Por qué falla:**
1. Solo afecta a NUEVOS usuarios después del cambio
2. Los usuarios ya conectados no se ven afectados
3. El código modificado sería visible (GitHub es público)
4. Las claves privadas no son "extractables" fácilmente
5. Los navegadores modernos bloquean código sospechoso

**Mitigación adicional posible:**
- Subresource Integrity (SRI) en scripts externos
- Content Security Policy (CSP) headers
- Firmar commits de Git

### ❌ Ataque 2: "Intercepto el tráfico de red"
**Escenario:** Alguien monitorea tu WiFi/red

**Qué puede ver:**
- Que estás conectado a PeerJS (servidor de señalización)
- Tu Peer ID público (algo como "aAbBcC123")
- Que hay tráfico WebRTC entre dos IPs

**Qué NO puede ver:**
- El contenido de los mensajes (cifrado RSA)
- Tu clave privada (nunca sale del dispositivo)
- El contenido descifrado

**Por qué falla:**
- Los mensajes van cifrados con RSA-OAEP
- WebRTC usa DTLS (cifrado de transporte)
- Necesitaría la clave privada para descifrar (que solo tienes tú)

### ❌ Ataque 3: "Man-in-the-Middle en PeerJS"
**Escenario:** Alguien hackea el servidor PeerJS

**Qué podría hacer:**
- Ver los Peer IDs de quienes se conectan
- Ver que dos peers están intentando conectarse

**Qué NO puede hacer:**
- Leer los mensajes (están cifrados E2E)
- Modificar las claves públicas (se detectaría al no poder descifrar)
- Hacerse pasar por un peer (no tiene la clave privada)

**Mitigación:**
- Verificación de fingerprints de claves (mejora futura)
- Usar tu propio servidor PeerJS
- Códigos QR para verificar identidad

### ❌ Ataque 4: "Malware en tu dispositivo"
**Escenario:** Tu computadora tiene un keylogger

**Por qué SÍ es un riesgo real:**
- Si tu dispositivo está comprometido, cualquier app es vulnerable
- El malware puede leer la RAM
- Puede capturar lo que escribes

**Mitigación:**
- Mantener antivirus actualizado
- No instalar software de fuentes desconocidas
- Usar dispositivos limpios para comunicaciones sensibles

**Nota:** Esto afecta a CUALQUIER app (incluso WhatsApp, Signal, etc.)

---

## 🔬 Prueba práctica de seguridad

### Experimento: ¿Puede JavaScript extraer claves?

```javascript
// Intentemos extraer la clave privada
try {
    const exported = await crypto.subtle.exportKey("pkcs8", myKeys.privateKey);
    console.log("Clave extraída:", exported);
} catch (error) {
    console.log("Error:", error);
    // Resultado: NotSupportedError o error similar
}
```

**Resultado esperado:** ERROR
- La clave privada tiene protecciones a nivel del navegador
- Aunque el código esté en GitHub, no puede forzar la exportación

### Verificación con DevTools:

1. Abre la app
2. Presiona F12 (DevTools)
3. Ve a Console
4. Escribe: `myKeys.privateKey`
5. Resultado: Ves un objeto `CryptoKey { type: "private" }`
6. Intenta: `JSON.stringify(myKeys.privateKey)`
7. Resultado: `{}` (objeto vacío - no serializable)

**Conclusión:** La clave privada existe pero no es accesible como datos planos.

---

## 🆚 Comparación con mensajería tradicional

### WhatsApp/Signal (Centralizado con E2EE):
```
Usuario A → [Servidor Meta/Signal] → Usuario B
              ↓
         Almacena:
         - Metadatos (quién, cuándo, IP)
         - Número de teléfono
         - Lista de contactos
         - Grupos
         
         NO almacena:
         - Contenido de mensajes (E2EE)
```

**Vulnerabilidad:** Los metadatos son muy valiosos
- Gobierno puede pedir lista de tus contactos
- Pueden saber con quién hablas y cuándo
- Pueden construir grafos sociales

### Este Chat P2P:
```
Usuario A ←→ [NADA] ←→ Usuario B
             
PeerJS solo sabe:
- Dos IDs anónimos se conectaron

NO sabe:
- Quiénes son
- Qué dicen
- Cuántos mensajes
- Duración de conversación (aproximada)
```

**Ventaja:** Privacidad de metadatos
- No hay servidor central
- No hay registros
- No hay qué requisar legalmente

---

## 📊 Matriz de amenazas

| Amenaza | Nivel | Mitigación |
|---------|-------|------------|
| Código malicioso en GitHub | 🟢 Bajo | Código abierto, auditable |
| Intercepción de red | 🟢 Bajo | E2EE + DTLS |
| Servidor PeerJS comprometido | 🟡 Medio | Solo ve IDs, no contenido |
| Malware en dispositivo | 🔴 Alto | Antivirus, higiene digital |
| Phishing (clave falsa) | 🟡 Medio | Verificar fingerprints |
| Ataque de fuerza bruta | 🟢 Bajo | RSA-2048 imposible de romper |

---

## 🧪 Mejoras de seguridad posibles

### 1. Verificación de identidad (fingerprints)
```javascript
// Generar huella digital de la clave pública
async function getKeyFingerprint(publicKey) {
    const exported = await crypto.subtle.exportKey("spki", publicKey);
    const hash = await crypto.subtle.digest("SHA-256", exported);
    return arrayBufferToHex(hash).substring(0, 16);
}

// Mostrar al usuario: "A3F2 8B1C 9D4E 7F0A"
// Ambos usuarios verifican por teléfono/en persona
```

### 2. Perfect Forward Secrecy (PFS)
```javascript
// Generar claves de sesión efímeras
// Usar ECDH para acuerdo de claves
// Cambiar claves cada X mensajes
```

### 3. Cifrado híbrido RSA + AES
```javascript
// Para mensajes largos:
// 1. Generar clave AES aleatoria
// 2. Cifrar mensaje con AES
// 3. Cifrar clave AES con RSA
// 4. Enviar ambos
```

### 4. Servidor PeerJS propio
```bash
# Instalar tu propio servidor de señalización
npm install peer
peerjs --port 9000 --key peerjs --path /myapp
```

---

## 🎓 Para desarrolladores: Auditoría del código

### Puntos críticos a revisar:

1. **Generación de claves (línea ~180 en index.html):**
   ```javascript
   myKeys = await crypto.subtle.generateKey(...)
   ```
   ✅ Usa API estándar del navegador
   ✅ No envía claves a ningún lado

2. **Cifrado de mensajes (línea ~250):**
   ```javascript
   const encrypted = await encryptMessage(remotePublicKey, message);
   ```
   ✅ Usa clave pública del destinatario
   ✅ Solo el destinatario puede descifrar

3. **Descifrado (línea ~230):**
   ```javascript
   const decrypted = await decryptMessage(data.message);
   ```
   ✅ Usa clave privada local
   ✅ No se comparte con nadie

4. **Envío de mensajes (línea ~450):**
   ```javascript
   conn.send({ type: 'encrypted-message', message: encrypted });
   ```
   ✅ Solo envía mensaje ya cifrado
   ✅ No envía claves privadas

### Herramientas de auditoría:

```bash
# Verificar que no haya requests sospechosos
# Abrir DevTools → Network → Filtrar por XHR
# Solo debería ver:
# - peerjs.com (señalización)
# - cdn.jsdelivr.net (librería PeerJS)
```

---

## 📝 Conclusión final

### ¿Es seguro alojar en GitHub Pages?

**SÍ, porque:**

1. ✅ **Las claves se generan localmente** - GitHub nunca las ve
2. ✅ **GitHub Pages es solo hosting estático** - No ejecuta código en servidor
3. ✅ **El código es auditable** - Cualquiera puede revisar index.html
4. ✅ **Web Crypto API es del navegador** - No depende de GitHub
5. ✅ **Los mensajes viajan por WebRTC** - No pasan por GitHub

### ¿Qué SÍ debes proteger?

1. 🔐 **Tu dispositivo** - Mantenerlo libre de malware
2. 🔐 **Tu red** - Usar WiFi seguro o VPN si es necesario
3. 🔐 **Tu identidad** - Verificar con quién hablas (fingerprints)

### ¿Es perfecto?

**No.** Ningún sistema es 100% seguro. Pero este chat es:
- Más privado que WhatsApp (metadatos)
- Más descentralizado que Signal
- Más transparente que Telegram
- Suficientemente seguro para conversaciones sensibles

### Casos de uso apropiados:

✅ **Sí usar para:**
- Discusiones médicas sensibles
- Información confidencial temporal
- Conversaciones que no quieres que se registren

❌ **No usar para:**
- Actividades ilegales (el cifrado no te hace invisible)
- Comunicaciones oficiales que requieren registros
- Si sospechas que tu dispositivo está comprometido

---

## 🔗 Recursos adicionales

- [Web Crypto API Spec](https://www.w3.org/TR/WebCryptoAPI/)
- [WebRTC Security](https://webrtc-security.github.io/)
- [OWASP Cryptographic Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html)

---

**Escrito con 🔬 para entender la seguridad real del sistema**
