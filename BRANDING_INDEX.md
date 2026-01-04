# Índice Completo de Documentación de Branding - Moodle App

Documentación completa para personalizar Moodle App (crear una versión branded) sin modificar código.

---

## 📚 Documentos Disponibles

### 1. **BRANDING_CUSTOMIZATION_GUIDE.md** ⭐ COMIENZA AQUÍ
**Guía completa y detallada de todas las opciones de personalización**

- Introducción al sistema de branding
- Todas las opciones disponibles en `moodle.config.json`
- Explicación detallada de cada propiedad
- Cómo se usan en el código
- Casos de uso prácticos
- Checklist de implementación

**Secciones principales:**
- ✅ Identidad y nombres de la aplicación
- ✅ Apariencia visual y temas
- ✅ Navegación y menú principal
- ✅ Configuración de sitios
- ✅ Información legal y políticas
- ✅ Caché y sincronización
- ✅ Idiomas y localización
- ✅ Notificaciones y UI
- ✅ Configuración avanzada

**Para quién:** Gerentes de producto, arquitectos técnicos, desarrolladores que necesitan entender todas las opciones.

---

### 2. **BRANDING_EXAMPLES.md** 📋 PLANTILLAS LISTAS PARA USAR
**7 plantillas de configuración listos para copiar y personalizar**

Plantillas preconstruidas para diferentes tipos de instituciones:

1. ✅ Educación Primaria (básica)
2. ✅ Educación Superior (universidad)
3. ✅ Formación Profesional
4. ✅ LMS Empresarial (corporativo)
5. ✅ Instituto Multi-Campus
6. ✅ Educación a Distancia (global)
7. ✅ Corporativo Cerrado (acceso restringido)

**Cada plantilla incluye:**
- Configuración JSON completa
- Valores recomendados por tipo de institución
- Cambios necesarios para personalización
- Instrucciones paso a paso

**Para quién:** Quienes necesitan una solución rápida, instituciones que quieren partir de un ejemplo similar a la suya.

---

### 3. **BRANDING_VISUAL_CUSTOMIZATION.md** 🎨 PERSONALIZACIÓN VISUAL AVANZADA
**Cómo personalizar colores, logos, fonts, temas y assets visuales**

Guía técnica para cambios visuales más profundos:

- Sistema de temas SCSS
- Personalización de colores
- Assets de branding (logos, splash, icons)
- Fonts personalizadas
- Temas Light/Dark
- Iconos personalizados
- Herramientas de diseño
- Validación de accesibilidad WCAG

**Para quién:** Diseñadores, desarrolladores frontend, especialistas en branding.

---

## 🚀 Guía Rápida de Inicio

### Si tienes 5 minutos:
1. Lee la sección de introducción en **BRANDING_CUSTOMIZATION_GUIDE.md**
2. Busca tu tipo de institución en **BRANDING_EXAMPLES.md**
3. Copia la plantilla relevante

### Si tienes 15 minutos:
1. Lee **BRANDING_CUSTOMIZATION_GUIDE.md** completamente
2. Personaliza `app_id`, `appname`, `notificoncolor`
3. Configura `sites` y URLs legales
4. Valida JSON: `node -e "require('./moodle.config.json')"`

### Si tienes 1 hora:
1. Lee los tres documentos
2. Prepara assets visuales (logos, icons)
3. Personaliza colores en SCSS
4. Configura temas Light/Dark
5. Compila y prueba

### Si tienes 1 día:
1. Implementación completa con personalización visual
2. Testing en Android e iOS
3. Optimización y validación de accesibilidad

---

## 📊 Matriz de Decisión

### ¿Qué necesito cambiar?

**Solo nombre y URLs:**
→ 10 minutos + BRANDING_EXAMPLES.md

**Nombre, colores, logos:**
→ 30 minutos + BRANDING_EXAMPLES.md + BRANDING_VISUAL_CUSTOMIZATION.md

**Nombre, colores, logos, fonts, temas:**
→ 2-3 horas + todos los documentos + diseño gráfico

**Control máximo (Corporativo):**
→ 1 día + personalización SCSS completa + testing exhaustivo

---

## 📋 Opciones por Categoría

### 1️⃣ IDENTIDAD (Nombre, ID, Versión)

| Opción | Modificable | Requiere Recompilación | Ubicación |
|--------|-----------|----------------------|-----------|
| `app_id` | ❌ No cambiar después | ✅ SÍ | `moodle.config.json` |
| `appname` | ✅ Sí | ❌ No | `moodle.config.json` |
| `versionname` | ✅ Sí | ❌ No | `moodle.config.json` |

### 2️⃣ APARIENCIA (Colores, Logos, Temas)

| Opción | Modificable | Requiere Recompilación | Ubicación |
|--------|-----------|----------------------|-----------|
| `notificoncolor` | ✅ Sí | ❌ No | `moodle.config.json` |
| `forceLoginLogo` | ✅ Sí | ❌ No | `moodle.config.json` |
| `showTopLogo` | ✅ Sí | ❌ No | `moodle.config.json` |
| `forceColorScheme` | ✅ Sí | ❌ No | `moodle.config.json` |
| Colores SCSS | ✅ Sí | ✅ SÍ | `src/theme/_app-variables.scss` |
| Logos (PNG) | ✅ Sí | ❌ No | `src/assets/img/` |

### 3️⃣ NAVEGACIÓN Y MENÚ

| Opción | Modificable | Requiere Recompilación | Ubicación |
|--------|-----------|----------------------|-----------|
| `customMainMenuItems` | ✅ Sí | ❌ No | `moodle.config.json` |
| `multisitesdisplay` | ✅ Sí | ❌ No | `moodle.config.json` |
| `sitefindersettings` | ✅ Sí | ❌ No | `moodle.config.json` |

### 4️⃣ CONFIGURACIÓN DE SITIOS

| Opción | Modificable | Requiere Recompilación | Ubicación |
|--------|-----------|----------------------|-----------|
| `sites` | ✅ Sí | ❌ No | `moodle.config.json` |
| `onlyallowlistedsites` | ✅ Sí | ❌ No | `moodle.config.json` |
| `demo_sites` | ✅ Sí | ❌ No | `moodle.config.json` |

---

## 🎯 Opciones Más Impactantes (Recomendadas para Comenzar)

Estas opciones tienen el mayor impacto visual/funcional con mínima configuración:

```json
{
  "appname": "Tu Institución",                    // ⭐ Impacto: ALTO
  "notificoncolor": "#TU_COLOR",                  // ⭐ Impacto: ALTO
  "forceLoginLogo": true,                         // ⭐ Impacto: ALTO
  "showTopLogo": "online",                        // ⭐ Impacto: MEDIO
  "forceColorScheme": "light",                    // ⭐ Impacto: MEDIO
  "sites": [{"url": "tu-moodle.edu"}],           // ⭐ Impacto: FUNCIONAL
  "privacypolicy": "https://tu-sitio.edu",       // ⭐ Impacto: LEGAL
  "customMainMenuItems": [...]                    // ⭐ Impacto: FUNCIONAL
}
```

---

## 🛠️ Flujo de Implementación

### Fase 1: Planificación (30 min)
- [ ] Define identidad de marca
- [ ] Selecciona color primario
- [ ] Reúne logos y assets
- [ ] Elige plantilla base

### Fase 2: Configuración (1 hora)
- [ ] Copia plantilla base
- [ ] Personaliza valores clave
- [ ] Prepara logos (PNG)
- [ ] Valida JSON

### Fase 3: Personalización Visual (1-2 horas)
- [ ] Ajusta colores SCSS (si necesario)
- [ ] Configura fonts (si necesario)
- [ ] Prepara splash screen
- [ ] Prepara app icons

### Fase 4: Compilación y Testing (1-2 horas)
- [ ] Compilación: `npm run build:prod`
- [ ] Test en navegador: `npm start`
- [ ] Test en Android (si necesario)
- [ ] Test en iOS (si necesario)

### Fase 5: Publicación (variable)
- [ ] Upload a Google Play
- [ ] Upload a App Store
- [ ] Comunicar usuarios

---

## 📈 Niveles de Complejidad

### 🟢 BÁSICO (15-30 minutos)

Ideal para: Modificar solo nombres y URLs

**Cambios necesarios:**
```json
{
  "appname": "Tu Escuela",
  "sites": [{"url": "https://tu-moodle.edu"}],
  "privacypolicy": "https://tu-sitio.edu/privacidad"
}
```

**Archivos modificados:** Solo `moodle.config.json`

---

### 🟡 INTERMEDIO (1-2 horas)

Ideal para: Branding básico con colores y logos

**Cambios necesarios:**
- `moodle.config.json` (colores, logos, menús)
- `src/assets/img/login_logo.png` (reemplazar)
- `src/assets/img/top_logo.png` (reemplazar)

**Archivos modificados:** 3

---

### 🔴 AVANZADO (4-8 horas)

Ideal para: Control visual completo

**Cambios necesarios:**
- `moodle.config.json` (configuración completa)
- `src/theme/_app-variables.scss` (colores)
- Assets personalizados (logos, splash, icons, fonts)
- Testing exhaustivo

**Archivos modificados:** 10+

---

## ✅ Checklist Técnico Final

### Configuración JSON
- [ ] `app_id` es único y válido
- [ ] `appname` representa tu marca
- [ ] `sites` apuntan a Moodle activo
- [ ] URLs de políticas son válidas
- [ ] `customMainMenuItems` tiene URLs válidas
- [ ] `notificoncolor` es hexadecimal válido
- [ ] JSON pasa validación

### Assets Visuales
- [ ] Logo login: 240x60px PNG
- [ ] Logo top: 120x30px PNG
- [ ] Splash screen: 1080x1920px PNG
- [ ] App icon: 1024x1024px PNG
- [ ] Favicon: 64x64px ICO/PNG

### Colores y Temas
- [ ] Colores SCSS actualizados (si aplica)
- [ ] Contraste WCAG validado
- [ ] Tema light probado
- [ ] Tema dark probado

### Compilación y Deploy
- [ ] `npm install` ejecutado
- [ ] `npm run build:prod` exitosa
- [ ] `npm start` sin errores
- [ ] Verificación visual en navegador
- [ ] Testing en dispositivo Android (si aplica)
- [ ] Testing en dispositivo iOS (si aplica)

---

## 🔗 Enlaces Rápidos

### Documentación Oficial
- [Documentación Moodle App](https://moodledev.io/general/app)
- [Repositorio GitHub](https://github.com/moodlehq/moodleapp)
- [Bug Tracker](https://moodle.atlassian.net/browse/MOBILE)

### Configuración Local
- Archivo principal: `/moodle.config.json`
- Tipos TypeScript: `/src/types/config.d.ts`
- Temas SCSS: `/src/theme/_app-variables.scss`
- Assets: `/src/assets/img/`

### Herramientas Útiles
- [AppIcon.co](https://appicon.co/) - Generador de iconos
- [ColorHexa](https://colorhexa.com/) - Paletas de colores
- [WebAIM](https://webaim.org/resources/contrastchecker/) - Validar contraste
- [FontAwesome](https://fontawesome.com/icons) - Buscar iconos

---

## 📞 Soporte y Ayuda

Si encuentras problemas:

1. **Validar JSON:** `node -e "require('./moodle.config.json')"`
2. **Ver logs:** Consola del navegador (F12)
3. **Revisar documentos:** Busca la sección relevante
4. **GitHub Issues:** https://github.com/moodlehq/moodleapp/issues
5. **Foro Moodle:** https://moodle.org/mod/forum/

---

## 📝 Resumen Ejecutivo

**Moodle App permite crear versiones branded completamente personalizadas:**

- ✅ **Sin modificar código** - Solo cambiar `moodle.config.json`
- ✅ **Multi-idioma** - Soporta 50+ idiomas
- ✅ **Multi-sitio** - Múltiples Moodle en una app
- ✅ **Personalizable** - Colores, logos, menús
- ✅ **Segura** - Conexiones HTTPS, token management
- ✅ **Offline-first** - Funciona sin internet
- ✅ **Profesional** - Distribución en App Stores

**Tiempo de implementación:**
- Básico (nombre + URL): 15-30 min
- Estándar (+ colores + logos): 1-2 horas
- Completo (+ fonts + temas): 4-8 horas

---

**Documentación de Branding - Moodle App 5.1.0**
**Actualizado:** Enero 2026
**Versión:** 2.0
