# Referencia Rápida de Branding - Moodle App

**Respuestas rápidas a preguntas comunes sobre personalización**

---

## ❓ PREGUNTAS FRECUENTES

### P: ¿Cuál es el archivo que debo modificar?
**R:** Principalmente `moodle.config.json`. Solo si necesitas cambiar colores: también `src/theme/_app-variables.scss`.

---

### P: ¿Qué debo cambiar primero?
**R:** En este orden:
1. `app_id` - Identificador único
2. `appname` - Nombre visible
3. `notificoncolor` - Color de marca
4. `sites` - URL de tu Moodle
5. `privacypolicy` - URL de políticas

---

### P: ¿Necesito recompilar después de cambiar `moodle.config.json`?
**R:**
- ❌ **NO** para: `appname`, `notificoncolor`, `sites`, URLs, menú, temas
- ✅ **SÍ** para: `app_id` (CRÍTICO)
- ✅ **SÍ** si modificas SCSS

---

### P: ¿Cómo cambio el color de notificaciones?
**R:**
```json
"notificoncolor": "#FF6B35"
```
Cambia `#FF6B35` por tu color hexadecimal de marca.

---

### P: ¿Cómo fuerzo tema claro/oscuro?
**R:**
```json
"forceColorScheme": "light"    // "light", "dark", o "" (permitir elegir)
```

---

### P: ¿Cómo uso mi logo en lugar del del servidor?
**R:**
1. Reemplaza `src/assets/img/login_logo.png` (240x60px)
2. Agrega en `moodle.config.json`:
```json
"forceLoginLogo": true
```

---

### P: ¿Cómo agrego links al menú principal?
**R:**
```json
"customMainMenuItems": [
  {
    "type": "browser",
    "url": "https://ejemplo.com",
    "icon": "fas-book",
    "label": "Mi Link"
  }
]
```

---

### P: ¿Cómo hago que solo se pueda acceder a mi Moodle?
**R:**
```json
"onlyallowlistedsites": true,
"sites": [
  { "url": "https://mi-moodle.edu" }
]
```

---

### P: ¿Cómo agrego múltiples idiomas al menú?
**R:**
```json
"customMainMenuItems": [
  {
    "type": "browser",
    "url": "https://ejemplo.com",
    "icon": "fas-globe",
    "label": {
      "es": "Mi Link",
      "en": "My Link",
      "fr": "Mon Lien"
    }
  }
]
```

---

### P: ¿Cómo cambio el idioma por defecto?
**R:**
```json
"default_lang": "es"    // ISO 639-1 code
```

---

### P: ¿Cómo cambio los colores principales?
**R:** Abre `src/theme/_app-variables.scss` y modifica:
```scss
$primary-color: #FF6B35;      // Tu color de marca
$secondary-color: #004E89;
$accent-color: #00A86B;
```

---

### P: ¿Cómo valido si mi JSON es válido?
**R:**
```bash
node -e "console.log(JSON.stringify(require('./moodle.config.json'), null, 2))"
```
Si ve el JSON formateado: ✅ Válido
Si ve error: ❌ Inválido

---

### P: ¿Cuánto es 240x60 en DPI?
**R:**
- 1x: 240x60px (desarrollo/web)
- 2x: 480x120px (Android/iOS real)
- 3x: 720x180px (iPhone Plus/Max)

**Recomendación:** Crear en 1x y dejar que navegador escale.

---

### P: ¿Qué tipo de archivos soporta?
**R:**
- Logos: PNG (recomendado)
- Splash screen: PNG o JPG
- Icons: PNG o ICO
- Fonts: TTF, WOFF, WOFF2

---

### P: ¿Cómo cambio el nombre de la app?
**R:**
```json
"appname": "Tu Nombre Aquí"
```

⚠️ Nota: El nombre en el icon de la app viene de Cordova (`config.xml`), no de `moodle.config.json`.

---

### P: ¿Puedo tener diferentes configs por plataforma?
**R:** No directamente, pero puedes:
1. Crear `moodle.config.json` base
2. Crear `moodle.config.android.json` y `moodle.config.ios.json`
3. En build time, seleccionar el correcto

---

### P: ¿Dónde veo mi app name en la UI?
**R:** En:
- Pantalla de inicio de sesión
- Página "Acerca de"
- Menú principal
- Como fallback cuando el sitio no tiene nombre

---

### P: ¿Cómo cambio el icono de notificaciones solo?
**R:**
```json
"notificoncolor": "#TU_COLOR"
```
Solo afecta el color del icono en Android. El icono en sí viene de los assets nativos.

---

### P: ¿Puedo tener apps diferentes para iOS y Android?
**R:**
- ❌ No en el mismo código base automáticamente
- ✅ Sí si compilas versiones separadas con diferentes `app_id`

---

### P: ¿Cómo seteo una política de privacidad?
**R:**
```json
"privacypolicy": "https://mi-sitio.com/politica"
```
Aparece en: Settings > About > Privacy Policy

---

### P: ¿Cómo agrego términos de servicio?
**R:**
```json
"legalDisclaimer": "https://mi-sitio.com/terminos"
```

---

### P: ¿Cómo agrego declaración de accesibilidad?
**R:**
```json
"a11yStatement": "https://mi-sitio.com/accesibilidad"
```

---

### P: ¿Qué es `customurlscheme`?
**R:** El protocolo para links tipo: `moodlemobile://user/1`

**No cambiar a menos que tengas razón específica.**

---

### P: ¿Cómo cambio el zoom por defecto?
**R:**
```json
"defaultZoomLevel": "medium",    // "none", "medium", o "high"
"zoomlevels": {
  "none": 100,
  "medium": 110,
  "high": 120
}
```

---

### P: ¿Puedo desactivar analytics?
**R:** Sí:
```json
"enableanalytics": false
```

---

### P: ¿Cómo desactivo el tutorial de bienvenida?
**R:**
```json
"enableonboarding": false
```

---

### P: ¿Cuál es la mejor paleta de colores para mi marca?
**R:** Comprueba:
1. Color primario: Logo/marca oficial
2. Color secundario: Botones, highlights
3. Color accent: Llamadas a acción importantes

**Validar:** Contraste mínimo 4.5:1 para accesibilidad

---

## 🔧 COMANDOS ÚTILES

```bash
# Validar JSON
node -e "require('./moodle.config.json'); console.log('✓ JSON válido')"

# Compilar desarrollo
npm start

# Compilar producción
npm run build:prod

# Ejecutar tests
npm test

# Linting
npm run lint

# Ver estructura
npm run examine

# Update lenguajes
npm run lang:update-langpacks
```

---

## 📐 MEDIDAS IMPORTANTES

```
Login Logo:      240x60 píxeles (PNG)
Top Logo:        120x30 píxeles (PNG)
Splash Screen:   1080x1920 píxeles (PNG)
App Icon:        1024x1024 píxeles (PNG)
Favicon:         64x64 píxeles (ICO/PNG)

Android:
  hdpi:    72x72
  xhdpi:   96x96
  xxhdpi:  144x144
  xxxhdpi: 192x192

iOS:
  iPhone 6+:     180x180
  iPad:          152x152
  iPad Pro:      167x167
```

---

## 🎨 COLORES SUGERIDOS

**Corporativo:**
```json
"notificoncolor": "#1F4E78"    // Azul oscuro
```

**Educativo:**
```json
"notificoncolor": "#FF6B35"    // Naranja
```

**Tech/Moderno:**
```json
"notificoncolor": "#000000"    // Negro
```

**Gobierno:**
```json
"notificoncolor": "#003B6F"    // Azul oficial
```

---

## 🌐 CÓDIGOS DE IDIOMA

```
af  - Afrikaans
ar  - Árabe
cs  - Checo
de  - Alemán
en  - Inglés
es  - Español
fr  - Francés
it  - Italiano
ja  - Japonés
ko  - Coreano
pt-br - Portugués (Brasil)
ru  - Ruso
tr  - Turco
zh-cn - Chino (simplificado)
```

Completa lista en: `src/types/config.d.ts`

---

## 📱 TIPOS DE ITEMS DE MENÚ

```json
{
  "type": "app",            // Navegación interna (ruta Angular)
  "type": "browser",        // Navegador externo del dispositivo
  "type": "inappbrowser",   // Navegador dentro de la app
  "type": "embedded"        // Iframe dentro de la app
}
```

---

## ✋ NO TOCAR ESTOS VALORES

```json
{
  "app_id": "❌ SIN CAMBIOS después de publicar",
  "wsservice": "❌ SIN CAMBIOS",
  "customurlscheme": "❌ SIN CAMBIOS",
  "ioswebviewscheme": "❌ SIN CAMBIOS"
}
```

---

## ⚡ LOS 5 CAMBIOS MÁS IMPORTANTES

1. **`appname`** → Nombre visible
2. **`notificoncolor`** → Color de marca
3. **`sites`** → URLs de tu Moodle
4. **`privacypolicy`** → Cumplimiento legal
5. **`forceLoginLogo: true`** + logo PNG → Branding visual

---

## 🚨 ERRORES COMUNES

### Error: "SyntaxError: Unexpected token"
**Causa:** JSON inválido
**Solución:**
```bash
node -e "require('./moodle.config.json')"
```

### Logo no aparece
**Causa:** `forceLoginLogo: false` o archivo no existe
**Solución:**
1. Verifica: `forceLoginLogo: true`
2. Verifica: `src/assets/img/login_logo.png` existe
3. Reinicia: `npm start`

### Color incorrecto
**Causa:** Formato hexadecimal incorrecto
**Solución:** Usa #RRGGBB válido (ej: #FF6B35)

### Texto ilegible
**Causa:** Contraste insuficiente
**Solución:** Usar [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/)

### Links de menú no funcionan
**Causa:** URL inválida o typo
**Solución:** Verificar URL completa y accesible

---

## 📊 MATRIZ RÁPIDA: CAMBIO vs RECOMPILACIÓN

| Cambio | Requiere Recompilación |
|--------|------------------------|
| `appname` | ❌ No |
| `app_id` | ✅ **SÍ - CRÍTICO** |
| `notificoncolor` | ❌ No |
| `sites` | ❌ No |
| `customMainMenuItems` | ❌ No |
| `forceColorScheme` | ❌ No |
| Logo PNG | ❌ No |
| Colores SCSS | ✅ Sí |
| Fonts | ✅ Sí |
| `default_lang` | ❌ No |

---

## 🎯 PLANTILLA MÍNIMA

```json
{
  "app_id": "com.miorg.app",
  "appname": "Mi App",
  "versionname": "5.1.0",
  "versioncode": 51001,
  "default_lang": "es",
  "languages": {
    "es": "Español",
    "en": "English"
  },
  "wsservice": "moodle_mobile_app",
  "sites": [
    { "url": "https://moodle.miorg.edu" }
  ],
  "notificoncolor": "#FF6B35",
  "privacypolicy": "https://miorg.edu/privacidad",
  "forceLoginLogo": true,
  "customurlscheme": "moodlemobile",
  "ioswebviewscheme": "moodleappfs"
}
```

Agrega más opciones según necesites.

---

## 🔐 SEGURIDAD

**Nunca incluyas en `moodle.config.json`:**
- ❌ Contraseñas reales
- ❌ Tokens de acceso
- ❌ API keys
- ❌ Información sensible

**OK para incluir:**
- ✅ URLs públicas
- ✅ Credenciales de demo
- ✅ IDs de tiendas de apps

---

## 📞 AYUDA RÁPIDA

**Problema:** App no abre
**Solución:** `npm start` y revisar consola (F12)

**Problema:** JSON inválido
**Solución:** Pega en https://jsonlint.com/

**Problema:** Logo pixelado
**Solución:** Asegúrate de 240x60px mínimo para 1x

**Problema:** No puedo encontrar una opción
**Solución:** Busca en `BRANDING_CUSTOMIZATION_GUIDE.md`

---

**Referencia Rápida - Moodle App 5.1.0**
**Actualizado:** Enero 2026
