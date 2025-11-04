# 🚀 Guía Rápida de Despliegue - Chat P2P E2EE

## ✅ Checklist de archivos

Asegúrate de tener estos archivos antes de subir:
- [x] index.html
- [x] sw.js
- [x] manifest.json
- [x] README.md
- [ ] icon-192.png (crear)
- [ ] icon-512.png (crear)

---

## 🎨 Paso 1: Crear íconos

### Opción A: Usar herramienta online
1. Ve a: https://www.favicon-generator.org/
2. Sube una imagen de 512x512 (puede ser simple)
3. Descarga los íconos generados
4. Renombra a `icon-192.png` y `icon-512.png`

### Opción B: Crear con Canva
1. Ve a: https://www.canva.com/
2. Crea diseño personalizado 512x512
3. Usa emojis: 🔒💬🛡️
4. Colores sugeridos: #667eea (morado) o #2c3e50 (azul oscuro)
5. Descarga como PNG
6. Crea versión de 192x192 redimensionando

### Opción C: Placeholder simple (temporal)
Puedes usar emojis convertidos a imagen desde:
- https://emoji.gg/
- Busca: "lock", "shield", "chat"

---

## 📦 Paso 2: Subir a GitHub

### Si ya tienes Git instalado:

```bash
# 1. Ve a la carpeta con los archivos
cd ruta/a/chat-p2p-encrypted

# 2. Inicializa Git
git init

# 3. Agrega todos los archivos
git add .

# 4. Primer commit
git commit -m "Initial commit - Chat P2P con E2EE"

# 5. Crea el repositorio en GitHub primero (desde la web)
# Luego conecta tu repositorio local:
git remote add origin https://github.com/TU-USUARIO/chat-p2p-encrypted.git

# 6. Sube los archivos
git branch -M main
git push -u origin main
```

### Si NO tienes Git instalado:

1. **Crea el repositorio en GitHub:**
   - Ve a: https://github.com/new
   - Nombre: `chat-p2p-encrypted`
   - Descripción: "Chat P2P con cifrado E2EE"
   - Público
   - Click en "Create repository"

2. **Sube archivos manualmente:**
   - Click en "uploading an existing file"
   - Arrastra todos los archivos
   - Click en "Commit changes"

---

## 🌐 Paso 3: Activar GitHub Pages

1. Ve a tu repositorio
2. Click en **Settings** (Configuración)
3. En el menú izquierdo, click en **Pages**
4. En "Source", selecciona:
   - Branch: `main`
   - Folder: `/ (root)`
5. Click en **Save**
6. Espera 1-2 minutos
7. Tu app estará en: `https://TU-USUARIO.github.io/chat-p2p-encrypted/`

---

## ✅ Paso 4: Probar la aplicación

### Prueba básica:
1. Abre la URL en tu navegador
2. Debe aparecer "Tu ID de conexión" después de unos segundos
3. Si ves errores de consola, revisa que todos los archivos estén subidos

### Prueba de conexión:
1. Abre la app en dos ventanas (o dos dispositivos)
2. Copia el ID de la ventana 1
3. Pégalo en la ventana 2 y conecta
4. Haz lo mismo en sentido inverso
5. Debe aparecer "🔐 Cifrado E2EE establecido"
6. Envía mensajes de prueba

---

## 🐛 Solución de problemas

### Problema: "Failed to fetch" o errores de CORS
**Solución:** Asegúrate de usar HTTPS (GitHub Pages lo usa automáticamente)

### Problema: Los íconos no se muestran
**Solución:** 
- Verifica que `icon-192.png` y `icon-512.png` estén en la raíz
- Limpia caché: Ctrl + Shift + R (Chrome)

### Problema: No se puede conectar con otro usuario
**Solución:**
- Ambos deben estar usando la misma versión de la app
- Verifica que ambos tengan conexión a internet
- Algunos firewalls corporativos bloquean WebRTC
- Prueba desde datos móviles

### Problema: Service Worker no se registra
**Solución:**
- Solo funciona en HTTPS o localhost
- GitHub Pages usa HTTPS automáticamente
- Si usas dominio personalizado, activa HTTPS en Settings

---

## 🔧 Personalización rápida

### Cambiar colores:
Edita `index.html`, busca:
```css
background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
```
Cambia a tus colores favoritos.

### Cambiar nombre:
Edita `manifest.json`, línea 2:
```json
"name": "Tu Nombre de App Aquí",
```

---

## 📱 Instalar en móvil

### Android:
1. Abre la app en Chrome
2. Menú (⋮) → "Añadir a pantalla de inicio"

### iOS:
1. Abre la app en Safari
2. Botón compartir → "Añadir a pantalla de inicio"

---

## 🎉 ¡Listo!

Tu chat P2P cifrado está funcionando. Algunas ideas de uso:

### Para estudiantes de medicina:
- Discusión de casos clínicos sin exponer datos del paciente
- Consultas entre colegas sobre diagnósticos
- Comunicación segura durante guardias

### Para cualquier usuario:
- Conversaciones privadas sin metadatos
- No deja rastro en servidores
- Ideal para información sensible

---

## 📚 Recursos adicionales

- **Código fuente completo:** Ver `index.html`
- **Documentación de seguridad:** Ver `README.md`
- **PeerJS Docs:** https://peerjs.com/docs/
- **Web Crypto API:** https://developer.mozilla.org/en-US/docs/Web/API/Web_Crypto_API

---

## 🆘 Soporte

Si tienes problemas:
1. Lee el README.md completo
2. Revisa la consola del navegador (F12)
3. Verifica que todos los archivos estén subidos
4. Prueba en navegador diferente
5. Verifica que GitHub Pages esté activo

---

**Desarrollado con 🔒 para proteger tu privacidad**
