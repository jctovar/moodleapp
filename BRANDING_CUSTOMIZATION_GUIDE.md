# Guía Completa de Personalización de Moodle App (Branding)

**Última actualización:** Enero 2026
**Versión de Moodle App:** 5.1.0
**Configuración:** `moodle.config.json` + `src/types/config.d.ts`

---

## Tabla de Contenidos

1. [Introducción](#introducción)
2. [Opciones de Personalización Disponibles](#opciones-de-personalización-disponibles)
3. [Guía Detallada por Categoría](#guía-detallada-por-categoría)
4. [Ejemplos Prácticos](#ejemplos-prácticos)
5. [Casos de Uso](#casos-de-uso)
6. [Consideraciones Técnicas](#consideraciones-técnicas)

---

## Introducción

Moodle App permite crear versiones personalizadas (branded) modificando el archivo de configuración `moodle.config.json`. Esta guía documenta todas las opciones disponibles para personalizar la apariencia, comportamiento y funcionalidad de la aplicación sin modificar código.

### Ubicación de Archivos Clave

- **Configuración:** `/moodle.config.json`
- **Tipos TypeScript:** `/src/types/config.d.ts`
- **Componentes de Branding:**
  - `/src/core/components/site-logo/`
  - `/src/core/features/login/`
  - `/src/core/features/mainmenu/`

---

## Opciones de Personalización Disponibles

### Nivel de Impacto en la Interfaz

| Categoría | Opciones | Impacto |
|-----------|----------|--------|
| **Identidad de App** | `appname`, `app_id`, `versionname` | Alto |
| **Apariencia Visual** | `notificoncolor`, `forceLoginLogo`, `showTopLogo`, `forceColorScheme` | Alto |
| **Navegación** | `customMainMenuItems`, `multisitesdisplay` | Medio |
| **Sitios** | `demo_sites`, `onlyallowlistedsites`, `sites` | Medio |
| **Información Legal** | `privacypolicy`, `a11yStatement`, `legalDisclaimer` | Bajo |
| **Búsqueda de Sitios** | `sitefindersettings` | Bajo |

---

## Guía Detallada por Categoría

### 1. IDENTIDAD Y NOMBRES DE LA APLICACIÓN

#### `appname` (Requerido)
**Tipo:** `string`
**Ubicación en UI:**
- Pantalla de inicio de sesión
- Menú de información
- Página "Acerca de"
- Splash screen

**Ejemplo de configuración:**
```json
{
  "appname": "Universidad XYZ Mobile"
}
```

**Cómo se usa en código:**
```typescript
// src/core/classes/sites/unauthenticated-site.ts
getSiteName(): Promise<string> {
    if (this.isDemoModeSite()) {
        return CoreConstants.CONFIG.appname;  // Fallback a nombre de app
    }
    // ... obtener del sitio Moodle
}
```

**Impacto:**
- Aparece en el título de la aplicación (navegador de desarrollo)
- Fallback cuando el sitio Moodle no proporciona su propio nombre
- Se muestra en la pantalla de onboarding

---

#### `app_id` (Requerido)
**Tipo:** `string`
**Formato:** Formato Java reverse domain notation: `com.organizacion.appname`

**Ejemplo:**
```json
{
  "app_id": "com.universidad.mobile"
}
```

**Impacto:**
- Identificador único en Google Play Store (Android)
- Identificador único en App Store (iOS)
- Identifica la aplicación en el sistema operativo
- Debe ser único si desea distribuir múltiples versiones

**Importante:** Cambiar este valor requiere compilación y re-publicación en tiendas.

---

#### `versionname` (Requerido)
**Tipo:** `string`
**Formato:** Semántica de versiones `MAJOR.MINOR.PATCH`

**Ejemplo:**
```json
{
  "versionname": "2.5.3"
}
```

**Impacto:**
- Mostrado en la página "Acerca de"
- Usado por tiendas de aplicaciones para actualizaciones
- Visible para los usuarios

---

### 2. APARIENCIA VISUAL Y TEMAS

#### `notificoncolor` (Notificaciones)
**Tipo:** `string` (color hexadecimal)
**Ubicación en UI:** Iconos de notificaciones push (Android)

**Ejemplo:**
```json
{
  "notificoncolor": "#FF6B35"
}
```

**Valores recomendados:**
- Colores vibrantes y diferenciadores
- Debe contrastar bien en notificaciones Android
- Ejemplos:
  - `#FF6B35` - Naranja (valor por defecto: `#f98012`)
  - `#2E86AB` - Azul
  - `#A23B72` - Púrpura
  - `#06A77D` - Verde

**Cómo se usa:**
```typescript
// src/core/features/pushnotifications/services/pushnotifications.ts
protected async getOptions(): Promise<PushOptions> {
    return {
        android: {
            sound: !!soundEnabled,
            icon: 'smallicon',
            iconColor: CoreConstants.CONFIG.notificoncolor,  // Aplicado aquí
        },
    };
}
```

**Impacto:**
- Solo visible en Android
- Afecta el color del icono de notificación en la bandeja del sistema
- Mejora la identidad visual en notificaciones

---

#### `forceLoginLogo` (Forzar Logo Local)
**Tipo:** `boolean`
**Valores:** `true` | `false`

**Ejemplo:**
```json
{
  "forceLoginLogo": true
}
```

**Comportamiento:**
- `true`: Usa el logo local (`assets/img/login_logo.png`)
- `false`: Descarga el logo del servidor Moodle (si disponible)

**Cuándo usar:**
- `true`: Para garantizar consistencia de marca (offline, sin depender del servidor)
- `false`: Para permitir que cada institución personalice su logo (entorno multi-sitio)

**Cómo se usa:**
```typescript
// src/core/classes/sites/unauthenticated-site.ts
forcesLocalLogo(): boolean {
    return CoreConstants.CONFIG.forceLoginLogo || this.isDemoModeSite();
}

getLogoUrl(config?: CoreSitePublicConfigResponse): string | undefined {
    config = config ?? this.publicConfig;
    if (!config || this.forcesLocalLogo()) {
        return undefined;  // Usa logo local
    }
    return config.logourl || config.compactlogourl;
}
```

**Logo local a personalizar:**
- Ubicación: `src/assets/img/login_logo.png`
- Tamaño recomendado: 240x60 píxeles
- Formato: PNG con transparencia

---

#### `showTopLogo` (Visibilidad del Logo Superior)
**Tipo:** `string`
**Valores:** `'online'` | `'offline'` | `'hidden'`

**Ejemplo:**
```json
{
  "showTopLogo": "online"
}
```

**Valores y comportamiento:**
- `'online'`: Mostrar logo solo cuando hay conexión a internet
- `'offline'`: Mostrar logo solo sin conexión a internet
- `'hidden'`: Nunca mostrar logo (recomendado para pantallas pequeñas)

**Logo a personalizar:**
- Ubicación: `src/assets/img/top_logo.png`
- Tamaño recomendado: 120x30 píxeles
- Formato: PNG con transparencia

**Cómo se usa:**
```typescript
// src/core/components/site-logo/site-logo.ts
if (this.logoType === 'top' && site.getShowTopLogo() === 'hidden') {
    this.showLogo = false;
}

// Visible solo según conexión
if (this.logoType === 'top' && showTopLogo === 'online' && !this.isOnline) {
    this.showLogo = false;
}
```

---

#### `forceColorScheme` (Forzar Esquema de Colores)
**Tipo:** `string` (CoreColorScheme)
**Valores:** `''` | `'light'` | `'dark'` | `'auto'`

**Ejemplo:**
```json
{
  "forceColorScheme": "light"
}
```

**Comportamiento:**
- `''` (vacío): Permitir al usuario elegir tema (defecto)
- `'light'`: Forzar tema claro
- `'dark'`: Forzar tema oscuro
- `'auto'`: Seguir preferencia del sistema

**Impacto:**
- Cuando se fuerza, el usuario NO puede cambiar el tema en configuración
- Afecta colores de: fondo, texto, componentes Ionic
- Ideal para mantener consistencia de marca

**Cómo se usa:**
```typescript
// src/core/features/settings/pages/general/general.ts
if (!CoreConstants.CONFIG.forceColorScheme) {
    // Mostrar selector de tema
    this.colorSchemes = CoreSettingsHelper.getAllowedColorSchemes();
} else {
    // No mostrar opción, está forzado
}
```

---

### 3. NAVEGACIÓN Y MENÚ PRINCIPAL

#### `customMainMenuItems` (Items de Menú Personalizados)
**Tipo:** `CoreMainMenuLocalizedCustomItem[]`

**Estructura de un item:**
```typescript
interface CoreMainMenuCustomItem {
    type: 'app' | 'inappbrowser' | 'browser' | 'embedded';  // Tipo de acción
    url: string;                                              // URL/path
    label: string | Record<CoreLangLanguage, string>;         // Etiqueta (multiidioma)
    icon: string;                                             // Nombre del icono
}
```

**Ejemplo completo:**
```json
{
  "customMainMenuItems": [
    {
      "type": "browser",
      "url": "https://ejemplo.com/documentacion",
      "icon": "fas-book",
      "label": {
        "en": "Documentation",
        "es": "Documentación",
        "fr": "Documentation"
      }
    },
    {
      "type": "embedded",
      "url": "https://ejemplo.com/support?device={{devicetype}}&version={{osversion}}",
      "icon": "fas-headset",
      "label": "Soporte Técnico"
    },
    {
      "type": "inappbrowser",
      "url": "https://ejemplo.com/noticias",
      "icon": "fas-newspaper",
      "label": {
        "en": "News",
        "es": "Noticias"
      }
    }
  ]
}
```

**Tipos de items:**

| Tipo | Comportamiento | Uso |
|------|----------------|-----|
| `app` | Navega dentro de la app (ruta angular) | Links internos |
| `browser` | Abre en navegador externo del dispositivo | URLs externas |
| `inappbrowser` | Abre en navegador dentro de la app (Cordova IAB) | URLs externas (controladas) |
| `embedded` | Iframe embebido en la app | Contenido interno personalizado |

**Parámetros dinámicos soportados:**

```
{{devicetype}}  →  'Android', 'iPhone or iPad', u 'Other'
{{osversion}}   →  Versión del SO (ej: '14.5')
```

**Ejemplo con parámetros:**
```json
{
  "type": "browser",
  "url": "https://ejemplo.com/soporte?os={{devicetype}}&version={{osversion}}",
  "icon": "fas-question",
  "label": "Soporte"
}
```

**Iconos disponibles:**

Puedes usar cualquier icono de Font Awesome:
- `fas-` : Solid icons
- `far-` : Regular icons
- `fab-` : Brand icons

**Ejemplos:**
- `fas-globe` - Globo
- `fas-phone` - Teléfono
- `fas-envelope` - Correo
- `fas-map-marker` - Ubicación
- `fab-github` - GitHub
- `fab-twitter` - Twitter

**Cómo se usa:**
```typescript
// src/core/features/mainmenu/services/mainmenu.ts
protected async getCustomItemsFromConfig(): Promise<CoreMainMenuCustomItem[]> {
    const items = CoreConstants.CONFIG.customMainMenuItems;

    if (!items) {
        return [];
    }

    const currentLang = await CoreLang.getCurrentLanguage();
    const fallbackLang = CoreConstants.CONFIG.default_lang || 'en';

    const replacements = {
        devicetype: CorePlatform.isAndroid() ? 'Android' :
                   CorePlatform.isIOS() ? 'iPhone or iPad' : 'Other',
        osversion: Device.version,
    };

    return items
        .filter(item => typeof item.label === 'string' ||
                       currentLang in item.label ||
                       fallbackLang in item.label)
        .map(item => ({
            ...item,
            url: CoreText.replaceArguments(item.url, replacements, 'uri'),
            label: typeof item.label === 'string'
                ? item.label
                : item.label[currentLang] ?? item.label[fallbackLang],
        }));
}
```

---

#### `multisitesdisplay` (Modo de Visualización Multi-Sitio)
**Tipo:** `CoreLoginSiteSelectorListMethod`
**Valores:** `''` | `'list'` | `'grid'` | `'compact'`

**Ejemplo:**
```json
{
  "multisitesdisplay": "grid"
}
```

**Comportamiento:**
- `''` (vacío): Comportamiento por defecto
- `'list'`: Mostrar sitios en lista (compacta)
- `'grid'`: Mostrar sitios en rejilla
- `'compact'`: Vista muy compacta

**Impacto:**
- Cómo se muestran los sitios guardados en la pantalla de selección
- Afecta solo a usuarios con múltiples sitios configurados

---

### 4. CONFIGURACIÓN DE SITIOS

#### `demo_sites` (Sitios de Demostración)
**Tipo:** `Record<string, CoreSitesDemoSiteData>`

**Estructura:**
```json
{
  "demo_sites": {
    "student": {
      "url": "https://school.moodledemo.net",
      "username": "student",
      "password": "moodle25"
    },
    "teacher": {
      "url": "https://school.moodledemo.net",
      "username": "teacher",
      "password": "moodle25"
    }
  }
}
```

**Ubicación en UI:**
- Pantalla de selección de sitios (botón "Demo")
- Acceso rápido para probar la app

**Casos de uso:**
- Demostración a nuevos usuarios
- Entrenamiento y onboarding
- Testing

**Seguridad:** Las credenciales de demo son públicas. No usar con datos reales.

---

#### `sites` (Sitios Precargados)
**Tipo:** `CoreLoginSiteInfo[]`

**Estructura:**
```json
{
  "sites": [
    {
      "url": "https://moodle.ejemplo.com",
      "title": "Institución Principal",
      "image": "https://ejemplo.com/logo.png"
    }
  ]
}
```

**Impacto:**
- Sitios que aparecen por defecto en la lista de selección
- Usuarios aún pueden agregar otros sitios

---

#### `onlyallowlistedsites` (Restringir a Sitios Autorizados)
**Tipo:** `boolean`

**Ejemplo:**
```json
{
  "onlyallowlistedsites": true,
  "sites": [
    { "url": "https://moodle1.ejemplo.com" },
    { "url": "https://moodle2.ejemplo.com" }
  ]
}
```

**Comportamiento:**
- `true`: Los usuarios SOLO pueden conectarse a sitios en la lista `sites`
- `false`: Los usuarios pueden agregar cualquier sitio Moodle

**Caso de uso:** Control de acceso en instituciones educativas.

---

#### `skipssoconfirmation` (Saltar Confirmación SSO)
**Tipo:** `boolean`

**Ejemplo:**
```json
{
  "skipssoconfirmation": true
}
```

**Comportamiento:**
- `true`: Proceder automáticamente con SSO sin confirmación
- `false`: Mostrar diálogo de confirmación antes de usar SSO

---

#### `sitefindersettings` (Configuración del Buscador de Sitios)
**Tipo:** `Partial<CoreLoginSiteFinderSettings>`

**Estructura completa:**
```typescript
{
    displayalias: boolean;           // Mostrar alias del sitio
    displaycity: boolean;            // Mostrar ciudad
    displaycountry: boolean;         // Mostrar país
    displayimage: boolean;           // Mostrar imagen/logo
    displaysitename: boolean;        // Mostrar nombre del sitio
    displayurl: boolean;             // Mostrar URL
    defaultimageurl?: string;        // URL de imagen por defecto
}
```

**Ejemplo:**
```json
{
  "sitefindersettings": {
    "displaysitename": true,
    "displayimage": true,
    "displayalias": false,
    "displaycity": false,
    "displaycountry": false,
    "displayurl": false,
    "defaultimageurl": "https://ejemplo.com/default-logo.png"
  }
}
```

**Impacto:**
- Controla qué información se muestra en la búsqueda de sitios Moodle
- Simplifica o enriquece la interfaz según necesidad

---

### 5. INFORMACIÓN LEGAL Y POLÍTICAS

#### `privacypolicy` (Política de Privacidad)
**Tipo:** `string` (URL)

**Ejemplo:**
```json
{
  "privacypolicy": "https://ejemplo.com/politica-privacidad"
}
```

**Ubicación en UI:**
- Página "Acerca de" (sección de configuración)
- Texto clickeable que abre la URL en navegador

**Fallback jerárquico:**
1. Política específica del sitio (desde el servidor Moodle)
2. Política en `moodle.config.json`
3. Política por defecto de Moodle (si no hay nada)

---

#### `a11yStatement` (Declaración de Accesibilidad)
**Tipo:** `string | false` (URL o false para desactivar)

**Ejemplo:**
```json
{
  "a11yStatement": "https://ejemplo.com/accesibilidad"
}
```

**Ubicación en UI:**
- Página "Acerca de" (sección de configuración)
- Oculto si es `false`

---

#### `legalDisclaimer` (Aviso Legal)
**Tipo:** `string | false` (URL o false para desactivar)

**Ejemplo:**
```json
{
  "legalDisclaimer": "https://ejemplo.com/terminos"
}
```

**Ubicación en UI:**
- Página "Acerca de" (sección de configuración)
- Oculto si es `false`

---

### 6. CACHÉ Y SINCRONIZACIÓN

#### `cache_update_frequency_*` (Frecuencias de Actualización)
**Tipo:** `number` (milisegundos)

**Valores por defecto:**
```json
{
  "cache_update_frequency_usually": 420000,      // 7 minutos
  "cache_update_frequency_often": 1200000,       // 20 minutos
  "cache_update_frequency_sometimes": 3600000,   // 1 hora
  "cache_update_frequency_rarely": 43200000      // 12 horas
}
```

**Categorías:**
- `usually`: Datos que cambian frecuentemente (mensajes, notificaciones)
- `often`: Datos que cambian regularmente (calificaciones, eventos)
- `sometimes`: Datos que cambian ocasionalmente (contenido del curso)
- `rarely`: Datos estables (estructura del curso, usuarios)

**Impacto en rendimiento:**
- Valores menores = más actualizaciones = más uso de red/batería
- Valores mayores = menos actualizaciones = datos potencialmente desactualizados

**Recomendaciones:**
- Conexión lenta: aumentar valores
- Aplicación crítica: disminuir valores
- Tablets para demostración: aumentar significativamente

---

### 7. IDIOMAS Y LOCALIZACIÓN

#### `default_lang` (Idioma por Defecto)
**Tipo:** `string` (código de idioma ISO 639-1)

**Ejemplo:**
```json
{
  "default_lang": "es"
}
```

**Idiomas soportados:** Más de 50 idiomas disponibles
- `en` - English
- `es` - Español
- `fr` - Français
- `de` - Deutsch
- `pt-br` - Português (Brasil)
- etc.

**Fallback:**
1. Idioma del dispositivo
2. Idioma configurado en sitio Moodle
3. `default_lang` de `moodle.config.json`
4. Inglés (fallback final)

---

#### `languages` (Idiomas Disponibles)
**Tipo:** `Record<string, string>`

**Estructura:**
```json
{
  "languages": {
    "en": "English",
    "es": "Español",
    "fr": "Français",
    "de": "Deutsch",
    "pt-br": "Português (Brasil)"
  }
}
```

**Impacto:**
- Solo los idiomas listados aparecen en el selector de idiomas
- Reduce complejidad si no necesitas todos los idiomas

---

### 8. NOTIFICACIONES Y UI

#### `notificoncolor` (Ya documentado en sección 2)

---

#### `toastDurations` (Duración de Notificaciones Toast)
**Tipo:** `Record<ToastDuration, number>` (milisegundos)

**Estructura:**
```json
{
  "toastDurations": {
    "short": 2000,    // 2 segundos
    "long": 3500,     // 3.5 segundos
    "sticky": 0       // No desaparece (0 = infinito)
  }
}
```

**Ubicaciones:**
- Mensajes de confirmación
- Mensajes de error
- Notificaciones en pantalla

---

#### `defaultZoomLevel` (Zoom por Defecto)
**Tipo:** `CoreZoomLevel` (string)
**Valores:** `'none'` | `'medium'` | `'high'`

**Ejemplo:**
```json
{
  "defaultZoomLevel": "medium",
  "zoomlevels": {
    "none": 100,
    "medium": 110,
    "high": 120
  }
}
```

**Impacto:**
- Tamaño de texto y UI por defecto
- Usuario puede cambiar en configuración

---

### 9. CONFIGURACIÓN AVANZADA

#### `wsrequestqueuelimit` y `wsrequestqueuedelay`
**Tipo:** `number`

**Ejemplo:**
```json
{
  "wsrequestqueuelimit": 10,       // Max 10 requests en cola
  "wsrequestqueuedelay": 100       // Esperar 100ms antes de procesar
}
```

**Impacto:**
- Evita sobrecargar el servidor con muchas peticiones simultáneas
- Mejora compatibilidad con servidores de baja capacidad
- Reduce uso de ancho de banda

---

#### `disableTokenFile` (Desactivar Descarga con Token)
**Tipo:** `boolean`

**Ejemplo:**
```json
{
  "disableTokenFile": false
}
```

**Comportamiento:**
- `false`: Usar `tokenpluginfile.php` (recomendado, más rápido)
- `true`: Usar `pluginfile.php` (fallback, compatible)

**Impacto:**
- Solo afecta descarga de archivos
- Usa para compatibilidad con versiones antiguas de Moodle

---

#### `enableanalytics` (Habilitar Analíticas)
**Tipo:** `boolean`

**Ejemplo:**
```json
{
  "enableanalytics": false
}
```

**Impacto:**
- Recopila datos de uso anónimos
- Ayuda a Moodle a mejorar la app
- Respetar privacidad de usuarios

---

#### `enableonboarding` (Habilitar Tutorial de Bienvenida)
**Tipo:** `boolean`

**Ejemplo:**
```json
{
  "enableonboarding": true
}
```

**Impacto:**
- Mostrar/ocultar tutorial al primer inicio
- Útil para usuarios nuevos

---

#### `collapsibleItemsExpanded` (Items Colapsables Expandidos)
**Tipo:** `boolean`

**Ejemplo:**
```json
{
  "collapsibleItemsExpanded": false
}
```

**Impacto:**
- Cómo se muestran las secciones/contenido expandibles por defecto
- Afecta navegación en cursos

---

---

## Ejemplos Prácticos

### Ejemplo 1: Universidad Pública Estándar

```json
{
  "app_id": "com.univ.edu.mobile",
  "appname": "Universidad Pública - App",
  "versioncode": 51001,
  "versionname": "5.1.0",
  "cache_update_frequency_usually": 420000,
  "cache_update_frequency_often": 1200000,
  "cache_update_frequency_sometimes": 3600000,
  "cache_update_frequency_rarely": 43200000,
  "default_lang": "es",
  "languages": {
    "es": "Español",
    "en": "English",
    "pt-br": "Português"
  },
  "wsservice": "moodle_mobile_app",
  "notificoncolor": "#0066CC",
  "privacypolicy": "https://univ.edu/privacidad",
  "forceLoginLogo": true,
  "showTopLogo": "online",
  "forceColorScheme": "",
  "customMainMenuItems": [
    {
      "type": "browser",
      "url": "https://univ.edu/soporte",
      "icon": "fas-headset",
      "label": {
        "es": "Soporte Técnico",
        "en": "Technical Support"
      }
    }
  ],
  "sites": [
    {
      "url": "https://moodle.univ.edu",
      "title": "Universidad Pública - Campus Virtual"
    }
  ],
  "onlyallowlistedsites": false,
  "collapsibleItemsExpanded": false
}
```

---

### Ejemplo 2: Institución Privada con Control Estricto

```json
{
  "app_id": "com.institutoelite.mobile",
  "appname": "Instituto Elite",
  "versionname": "3.2.1",
  "notificoncolor": "#8B0000",
  "forceLoginLogo": true,
  "showTopLogo": "hidden",
  "forceColorScheme": "light",
  "privacypolicy": "https://elite.edu.mx/legal/privacidad",
  "a11yStatement": "https://elite.edu.mx/legal/accesibilidad",
  "legalDisclaimer": "https://elite.edu.mx/legal/terminos",
  "demo_sites": {
    "student": {
      "url": "https://demo.elite.edu.mx",
      "username": "demo",
      "password": "demo"
    }
  },
  "sites": [
    {
      "url": "https://moodle.elite.edu.mx",
      "title": "Sistema de Aprendizaje - Instituto Elite"
    }
  ],
  "onlyallowlistedsites": true,
  "sitefindersettings": {
    "displaysitename": true,
    "displayimage": true,
    "displayalias": false,
    "displaycity": false,
    "displaycountry": false,
    "displayurl": false
  },
  "enableanalytics": false,
  "enableonboarding": false,
  "customMainMenuItems": [
    {
      "type": "embedded",
      "url": "https://elite.edu.mx/portal/noticias",
      "icon": "fas-newspaper",
      "label": "Noticias"
    },
    {
      "type": "browser",
      "url": "https://elite.edu.mx/portal/contacto",
      "icon": "fas-envelope",
      "label": "Contacto"
    }
  ],
  "wsrequestqueuelimit": 5,
  "wsrequestqueuedelay": 200
}
```

---

### Ejemplo 3: Organización Multi-Idioma Global

```json
{
  "app_id": "com.globaltraining.app",
  "appname": "Global Training Platform",
  "versionname": "1.0.0",
  "default_lang": "en",
  "languages": {
    "en": "English",
    "es": "Español",
    "fr": "Français",
    "de": "Deutsch",
    "pt-br": "Português (Brasil)",
    "zh-cn": "简体中文",
    "ja": "日本語"
  },
  "notificoncolor": "#FF9500",
  "forceLoginLogo": false,
  "showTopLogo": "online",
  "forceColorScheme": "auto",
  "privacypolicy": "https://globaltraining.com/privacy",
  "customMainMenuItems": [
    {
      "type": "browser",
      "url": "https://globaltraining.com/docs?lang={{lang}}&device={{devicetype}}",
      "icon": "fas-book",
      "label": {
        "en": "Documentation",
        "es": "Documentación",
        "fr": "Documentation",
        "de": "Dokumentation",
        "pt-br": "Documentação",
        "zh-cn": "文档",
        "ja": "ドキュメント"
      }
    },
    {
      "type": "browser",
      "url": "https://globaltraining.com/support/tickets",
      "icon": "fas-ticket-alt",
      "label": {
        "en": "Support Tickets",
        "es": "Tickets de Soporte",
        "fr": "Tickets de Support",
        "de": "Support-Tickets",
        "pt-br": "Tickets de Suporte",
        "zh-cn": "支持票",
        "ja": "サポートチケット"
      }
    }
  ],
  "demo_sites": {
    "trainer": {
      "url": "https://demo.globaltraining.com",
      "username": "trainer",
      "password": "training123"
    }
  },
  "enableonboarding": true,
  "collapsibleItemsExpanded": true
}
```

---

### Ejemplo 4: Educación Corporativa (LMS Empresarial)

```json
{
  "app_id": "com.megacorp.learning",
  "appname": "MegaCorp Learning Hub",
  "versionname": "2.0.0",
  "notificoncolor": "#1F4E78",
  "forceLoginLogo": true,
  "showTopLogo": "hidden",
  "forceColorScheme": "light",
  "privacypolicy": "https://megacorp.intranet/legal/privacy",
  "sites": [
    {
      "url": "https://learning.megacorp.com",
      "title": "MegaCorp Learning Platform"
    }
  ],
  "onlyallowlistedsites": true,
  "sitefindersettings": {
    "displaysitename": true,
    "displayimage": true,
    "displayalias": false,
    "displaycity": false,
    "displaycountry": false,
    "displayurl": false,
    "defaultimageurl": "https://megacorp.intranet/logo.png"
  },
  "customMainMenuItems": [
    {
      "type": "browser",
      "url": "https://megacorp.intranet/hr-portal",
      "icon": "fas-users",
      "label": "HR Portal"
    },
    {
      "type": "browser",
      "url": "https://megacorp.intranet/compliance",
      "icon": "fas-shield-alt",
      "label": "Compliance Center"
    },
    {
      "type": "embedded",
      "url": "https://megacorp.intranet/announcements",
      "icon": "fas-bullhorn",
      "label": "Company Announcements"
    }
  ],
  "cache_update_frequency_usually": 300000,
  "cache_update_frequency_often": 600000,
  "cache_update_frequency_sometimes": 1800000,
  "cache_update_frequency_rarely": 21600000,
  "wsrequestqueuelimit": 15,
  "wsrequestqueuedelay": 50,
  "enableanalytics": true,
  "enableonboarding": false,
  "collapsibleItemsExpanded": true,
  "multisitesdisplay": "compact",
  "disableCallWSInBackground": false
}
```

---

## Casos de Uso

### Caso 1: Garantizar Marca Consistente Offline
**Objetivo:** Que la app se vea igual sin importar conexión a internet

**Configuración:**
```json
{
  "forceLoginLogo": true,
  "showTopLogo": "hidden",
  "forceColorScheme": "light",
  "notificoncolor": "#BRAND_COLOR"
}
```

**Archivos a personalizar:**
- `src/assets/img/login_logo.png` - Logo de inicio de sesión
- `src/theme/_app-variables.scss` - Colores de marca

---

### Caso 2: Simplificar para Usuarios Nuevos
**Objetivo:** Interfaz limpia y guiada para principiantes

**Configuración:**
```json
{
  "enableonboarding": true,
  "collapsibleItemsExpanded": false,
  "multisitesdisplay": "list",
  "showTopLogo": "online",
  "customMainMenuItems": []  // Sin items extras
}
```

---

### Caso 3: Maximizar Funcionalidad para Power-Users
**Objetivo:** Acceso rápido a herramientas externas

**Configuración:**
```json
{
  "enableonboarding": false,
  "collapsibleItemsExpanded": true,
  "customMainMenuItems": [
    { "type": "browser", "url": "...", "icon": "fas-tools" },
    { "type": "embedded", "url": "...", "icon": "fas-chart" },
    { "type": "inappbrowser", "url": "...", "icon": "fas-cog" }
  ]
}
```

---

### Caso 4: Entorno Institucional Cerrado
**Objetivo:** Control total sobre acceso y datos

**Configuración:**
```json
{
  "onlyallowlistedsites": true,
  "sites": [ /* sitios autorizados */ ],
  "enableanalytics": false,
  "skipssoconfirmation": false,
  "disableTokenFile": false
}
```

---

## Consideraciones Técnicas

### Proceso de Actualización de Configuración

1. **Modificar** `moodle.config.json`
2. **Compilar** la aplicación:
   ```bash
   npm run build:prod
   ```
3. **Re-publicar** en tiendas de aplicaciones

### Atención: Cambios que Requieren Recompilación

- `app_id` - **SIEMPRE requiere recompilación**
- `appname` - Se actualiza en tiempo de ejecución (sin recompilación necesaria en desarrollo)
- `notificoncolor` - Se actualiza en tiempo de ejecución
- `forceLoginLogo`, `showTopLogo` - Se actualizan en tiempo de ejecución

### Cambios que Requieren Personalización de Assets

- `forceLoginLogo: true` → Preparar logo personalizado en `src/assets/img/`
- Esquema de colores → Personalizar `src/theme/_app-variables.scss`

### Validación de JSON

Asegurarse de que `moodle.config.json` sea JSON válido:

```bash
# Validar sintaxis
node -e "require('./moodle.config.json')"
```

### Versiones de Moodle Soportadas

- Compatible con Moodle 3.1+
- Recomendado: Moodle 3.9 o superior
- Última versión recomendada: Moodle 4.x

### Seguridad

- **No almacenar credenciales** en la configuración en producción
- `demo_sites` debe tener **credenciales públicas/demo**
- Usar **URLs HTTPS** para `privacypolicy` y otros enlaces
- Validar URLs en `customMainMenuItems`

---

## Checklist para Crear tu Versión Branded

- [ ] Define `app_id` único
- [ ] Personaliza `appname`
- [ ] Elige `notificoncolor` basado en tu marca
- [ ] Prepara logos:
  - [ ] `src/assets/img/login_logo.png` (240x60px)
  - [ ] `src/assets/img/top_logo.png` (120x30px)
- [ ] Configura `forceLoginLogo` y `showTopLogo`
- [ ] Elige esquema de colores (`forceColorScheme`)
- [ ] Personaliza `customMainMenuItems` según necesidad
- [ ] Configura `sites` autorizados (si necesario)
- [ ] Agrega URLs legales (`privacypolicy`, `a11yStatement`)
- [ ] Prueba en ambas plataformas (Android e iOS)
- [ ] Valida JSON con: `node -e "require('./moodle.config.json')"`
- [ ] Compila versión de producción: `npm run build:prod`

---

## Referencias Adicionales

- **Documentación Oficial:** https://moodledev.io/general/app
- **Repositorio:** https://github.com/moodlehq/moodleapp
- **Archivo de Tipos:** `/src/types/config.d.ts`
- **Configuración Ejemplo:** `/moodle.config.json`

---

**Documento creado:** Enero 2026
**Versión de Moodle App:** 5.1.0
**Autor:** Claude Code Analysis
