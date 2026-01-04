# Guía de Personalización Visual Avanzada - Moodle App

Esta guía cubre cómo personalizar colores, temas, fonts, y otros aspectos visuales de Moodle App.

---

## 📋 Tabla de Contenidos

1. [Sistema de Temas](#sistema-de-temas)
2. [Personalización de Colores](#personalización-de-colores)
3. [Assets de Branding](#assets-de-branding)
4. [Fonts Personalizadas](#fonts-personalizadas)
5. [Temas Light/Dark](#temas-lightdark)
6. [Iconos Personalizados](#iconos-personalizados)
7. [Splash Screen](#splash-screen)
8. [App Icons](#app-icons)
9. [Build Variables](#build-variables)

---

## Sistema de Temas

### Estructura del Sistema de Temas

Moodle App usa SCSS para gestionar temas:

```
src/theme/
├── _app-variables.scss       # Variables principales de tema
├── _app.scss                 # Estilos base de la app
├── _components.scss          # Estilos de componentes
├── _variables.scss           # Variables de Ionic
└── _mixins.scss              # Mixins reutilizables
```

### Flujo de Aplicación de Temas

```
moodle.config.json
    ↓
EnvironmentConfig.forceColorScheme
    ↓
CoreSettingsHelper.setColorScheme()
    ↓
--theme-light / --theme-dark (CSS variables)
    ↓
SCSS variables + Ionic theming
    ↓
Componentes visuales
```

---

## Personalización de Colores

### Variables Principales de Tema

El archivo `/src/theme/_app-variables.scss` contiene todas las variables de color:

```scss
// Colores primarios de marca
$primary-color: #0066CC;
$secondary-color: #FF6B35;
$accent-color: #00A86B;

// Colores neutrales
$background-color: #FFFFFF;
$text-primary: #1A1A1A;
$text-secondary: #666666;

// Estados
$success-color: #28A745;
$warning-color: #FFC107;
$danger-color: #DC3545;
$info-color: #17A2B8;

// Sombras y bordes
$border-color: #E0E0E0;
$shadow-color: rgba(0, 0, 0, 0.1);
```

### Cómo Modificar Colores

#### Opción 1: Modificar directamente en SCSS

**Archivo:** `src/theme/_app-variables.scss`

```scss
// ANTES
$primary-color: #0066CC;

// DESPUÉS (tu color de marca)
$primary-color: #FF6B35;
```

**Archivos afectados:**
- Botones primarios
- Links
- Headers
- Elementos enfatizados

#### Opción 2: Variables CSS en Tiempo de Ejecución

Si necesitas cambiar colores dinámicamente sin recompilación:

```typescript
// src/core/features/settings/services/settings-helper.ts
import { getComputedStyle } from '@ionic/core';

export class CoreThemeService {
    setCustomColor(colorName: string, hexValue: string): void {
        const root = document.documentElement;
        root.style.setProperty(`--app-${colorName}`, hexValue);
    }
}
```

---

### Paleta de Colores Recomendada

#### Opción 1: Colores Corporativos (Profesionales)

```scss
// Azul corporativo
$primary-color: #1F4E78;      // Azul oscuro
$secondary-color: #4A90E2;    // Azul claro
$accent-color: #00A86B;       // Verde

// Para modoLight
$background-color: #FFFFFF;
$text-primary: #1A1A1A;

// Para modoDark
$background-dark: #1E1E1E;
$text-dark: #FFFFFF;
```

#### Opción 2: Colores Vibrantes (Educativos)

```scss
$primary-color: #FF6B35;      // Naranja
$secondary-color: #004E89;    // Azul
$accent-color: #F7B801;       // Amarillo

$background-color: #F5F5F5;
$text-primary: #2C3E50;
```

#### Opción 3: Colores Minimalistas (Tech)

```scss
$primary-color: #000000;      // Negro
$secondary-color: #666666;    // Gris
$accent-color: #FF0000;       // Rojo (destaque)

$background-color: #FFFFFF;
$text-primary: #1A1A1A;
```

---

## Assets de Branding

### 1. Logo de Inicio de Sesión

**Ubicación:** `src/assets/img/login_logo.png`

**Especificaciones:**
- Dimensiones: 240x60 píxeles (o proporcional)
- Formato: PNG con transparencia
- Resolución: 1x para desarrollo, 2x y 3x para producción
- Colores: Preferiblemente con fondo transparente

**Estructura de archivos:**
```
src/assets/img/
├── login_logo.png          (1x - 240x60px)
├── login_logo@2x.png       (2x - 480x120px)
└── login_logo@3x.png       (3x - 720x180px)
```

**Cómo usarlo:**
1. Si `forceLoginLogo: true` en `moodle.config.json`
2. La app buscará automáticamente en `assets/img/login_logo.png`
3. El logo local reemplaza al del servidor Moodle

**Herramientas para crear:**
- Figma
- Adobe XD
- Canva
- Inkscape (gratuito)

---

### 2. Logo Superior

**Ubicación:** `src/assets/img/top_logo.png`

**Especificaciones:**
- Dimensiones: 120x30 píxeles (o proporcional)
- Formato: PNG con transparencia
- Resolución: 1x, 2x, 3x como con login_logo
- Se muestra en la parte superior de pantallas de contenido

**Control de visibilidad:**
```json
{
  "showTopLogo": "online"  // 'online', 'offline', o 'hidden'
}
```

---

### 3. Favicon

**Ubicación:** `src/assets/icons/favicon.ico`

**Especificaciones:**
- Formato: ICO o PNG
- Tamaño: 64x64 píxeles mínimo
- Usado solo en versión web/browser

---

### 4. Splash Screen (Pantalla de Carga)

**Ubicación:** `src/assets/images/splash.png`

**Especificaciones:**
- Dimensiones: 1242x2208 píxeles (iPhone) o 1080x1920 (Android)
- Formato: PNG
- Este se muestra mientras la app carga

**Para Android:**
```
platforms/android/app/src/main/res/
├── drawable/splash.png         (1x)
├── drawable-hdpi/splash.png    (1.5x)
├── drawable-xhdpi/splash.png   (2x)
├── drawable-xxhdpi/splash.png  (3x)
└── drawable-xxxhdpi/splash.png (4x)
```

**Para iOS:**
```
platforms/ios/[AppName]/Images.xcassets/
└── LaunchImage.launchimage/
    ├── LaunchImage.png
    ├── LaunchImage@2x.png
    ├── LaunchImage@3x.png
    └── LaunchImage iPad variants
```

---

### 5. App Icons

**Android:**

```
platforms/android/app/src/main/res/
├── mipmap-hdpi/ic_launcher.png
├── mipmap-xhdpi/ic_launcher.png
├── mipmap-xxhdpi/ic_launcher.png
└── mipmap-xxxhdpi/ic_launcher.png
```

**Dimensiones:**
- hdpi: 72x72
- xhdpi: 96x96
- xxhdpi: 144x144
- xxxhdpi: 192x192

**iOS:**

```
platforms/ios/[AppName]/Images.xcassets/
└── AppIcon.appiconset/
    ├── icon-20x20@2x.png
    ├── icon-29x29@3x.png
    ├── icon-40x40@2x.png
    ├── icon-60x60@3x.png
    └── ... (múltiples tamaños)
```

**Dimensiones comunes:**
- 180x180 (iPhone 6+)
- 167x167 (iPad Pro)
- 152x152 (iPad)

**Generador automático:**
- [AppIcon.co](https://appicon.co/) - Genera múltiples tamaños automáticamente
- [MakeAppIcon.com](https://makeappicon.com/)

---

## Fonts Personalizadas

### Instalación de Fonts Personalizadas

#### Para Web/Dev (Chrome, Safari):

**Ubicación:** `src/assets/fonts/`

**Ejemplo estructura:**
```
src/assets/fonts/
├── my-brand-font.ttf
├── my-brand-font.woff
└── my-brand-font.woff2
```

**Declaración en SCSS:**

```scss
// src/theme/_app-variables.scss
@font-face {
    font-family: 'MyBrandFont';
    src: url('assets/fonts/my-brand-font.woff2') format('woff2'),
         url('assets/fonts/my-brand-font.woff') format('woff');
    font-weight: normal;
    font-style: normal;
}

// Usar en variables
$font-family-base: 'MyBrandFont', -apple-system, BlinkMacSystemFont, 'Segoe UI';
```

#### Para Android:

**Ubicación:** `platforms/android/app/src/main/assets/fonts/`

```
platforms/android/app/src/main/assets/fonts/
├── my-brand-font.ttf
└── my-brand-font-bold.ttf
```

#### Para iOS:

**En Xcode:**
1. Agregar fonts a `Info.plist`
2. Fonts deben estar en el bundle del app

```plist
<key>UIAppFonts</key>
<array>
    <string>MyBrandFont.ttf</string>
    <string>MyBrandFont-Bold.ttf</string>
</array>
```

---

## Temas Light/Dark

### Estructura de Variables por Tema

```scss
// Light theme
.theme-light {
    --bg-primary: #FFFFFF;
    --bg-secondary: #F5F5F5;
    --text-primary: #1A1A1A;
    --text-secondary: #666666;
    --border-color: #E0E0E0;
}

// Dark theme
.theme-dark {
    --bg-primary: #1E1E1E;
    --bg-secondary: #2D2D2D;
    --text-primary: #FFFFFF;
    --text-secondary: #CCCCCC;
    --border-color: #404040;
}
```

### Cambio Forzado de Tema

**En `moodle.config.json`:**

```json
{
  "forceColorScheme": "light"  // Fuerza tema claro
}
```

**O:**

```json
{
  "forceColorScheme": "dark"   // Fuerza tema oscuro
}
```

**O permitir selección:**

```json
{
  "forceColorScheme": ""  // Usuario puede elegir
}
```

### Detectar Tema Actual en Componente

```typescript
import { CoreSettingsHelper, CoreColorScheme } from '@features/settings/services/settings-helper';

export class MyComponent {
    currentTheme: CoreColorScheme;

    constructor(private settingsHelper: CoreSettingsHelper) {
        this.currentTheme = this.settingsHelper.getCurrentColorScheme();
    }

    isDarkMode(): boolean {
        return this.currentTheme === CoreColorScheme.DARK;
    }
}
```

---

## Iconos Personalizados

### Iconos Soportados

Moodle App soporta múltiples conjuntos de iconos:

1. **Font Awesome** (por defecto)
   - Prefijos: `fa-`, `fas-`, `far-`, `fab-`
   - Más de 6000 iconos disponibles

2. **Moodle Icons** (iconos específicos de Moodle)
   - Prefijo: `moodle-`

### Usar Iconos en `customMainMenuItems`

```json
{
  "customMainMenuItems": [
    {
      "icon": "fas-graduation-cap",
      "label": "Cursos"
    },
    {
      "icon": "fab-github",
      "label": "GitHub"
    },
    {
      "icon": "far-heart",
      "label": "Favoritos"
    }
  ]
}
```

### Iconos Personalizados de Font Awesome

**Sitio:** https://fontawesome.com/icons

Búsqueda rápida de iconos populares:

```
fas-home              Inicio
fas-book              Libro
fas-users             Usuarios
fas-calendar          Calendario
fas-bell              Notificación
fas-envelope          Email
fas-message           Mensaje
fas-chart-bar         Gráfico
fas-cog               Configuración
fas-sign-out          Cerrar sesión
fas-download          Descargar
fas-upload            Subir
fas-trash             Eliminar
fas-edit              Editar
fas-save              Guardar
fas-check             Verificación
fas-times             Cerrar
fas-search            Buscar
fas-filter            Filtrar
fas-sort              Ordenar
fas-expand            Expandir
fas-collapse          Contraer
fas-lock              Bloqueado
fas-unlock            Desbloqueado
fas-star              Estrella
fas-heart             Corazón
fas-share             Compartir
fas-print             Imprimir
fas-image             Imagen
fas-video             Video
fas-music             Música
fas-file              Archivo
fas-folder            Carpeta
fas-cloud             Nube
fas-wifi              WiFi
fas-battery           Batería
fas-shield            Escudo
fas-exclamation       Advertencia
fas-info              Información
fas-question          Pregunta
fas-lightbulb         Idea
fas-rocket            Cohete
fas-target            Objetivo
fas-map               Mapa
fas-phone             Teléfono
fab-facebook          Facebook
fab-twitter           Twitter
fab-linkedin          LinkedIn
fab-instagram         Instagram
fab-youtube           YouTube
fab-github            GitHub
fab-google            Google
```

---

## Splash Screen

### Crear Splash Screen Personalizado

**Herramientas recomendadas:**
- Canva (fácil, plantillas disponibles)
- Figma
- Adobe XD
- Photoshop

**Componentes recomendados:**
1. Logo de la app (centrado)
2. Nombre de la app
3. Slogan o tagline (opcional)
4. Colores de marca
5. Información de copyright (opcional)

**Ejemplo de estructura:**
```
┌─────────────────────────────┐
│                             │
│     [Logo de App]           │
│                             │
│    Nombre de Aplicación     │
│                             │
│   Bienvenido a nuestro      │
│      servicio educativo     │
│                             │
│          [Loading...]       │
│                             │
│  © 2026 Tu Institución      │
└─────────────────────────────┘
```

### Implementar en Android

1. Crear imagen: 1080x1920px (XXXHDPI)
2. Guardar como: `platforms/android/app/src/main/res/drawable/splash.png`
3. Crear versiones para otras densidades

```bash
# Generar múltiples resoluciones desde 1x
# Usando ImageMagick:
convert splash.png -resize 432x768 splash-hdpi.png
convert splash.png -resize 576x1024 splash-xhdpi.png
convert splash.png -resize 864x1536 splash-xxhdpi.png
```

### Implementar en iOS

1. Crear imagen: 2208x1242px (mínimo)
2. En Xcode: Assets > LaunchImage
3. Ajustar para diferentes dispositivos

---

## App Icons

### Crear Icon Profesional

**Especificaciones generales:**
- Forma: Cuadrada (será redondeada por el SO)
- Fondo: Sólido o con degradado suave
- Seguridad: Dejar 10% de margen sin elementos críticos
- Formato: PNG sin compresión

**Ejemplo de diseño:**
- Logo/símbolo centrado
- Colores de marca
- Suficientemente distinguible en tamaño pequeño

### Generar Automáticamente

**Mejor herramienta:** AppIcon.co

1. Ir a https://appicon.co/
2. Subir imagen de 1024x1024px
3. Descargar assets generados
4. Copiar a `platforms/android/` e `platforms/ios/`

**Alternativa:** MakeAppIcon.com (también genera rápidamente)

---

## Build Variables

### Configuración por Entorno

**Archivo:** `angular.json`

```json
{
  "projects": {
    "app": {
      "architect": {
        "build": {
          "configurations": {
            "production": {
              "budgets": [
                {
                  "type": "bundle",
                  "maximumWarning": "2mb",
                  "maximumError": "5mb"
                }
              ],
              "optimization": true,
              "aot": true
            }
          }
        }
      }
    }
  }
}
```

### Variables de Compilación

En `environment.prod.ts`:

```typescript
export const environment = {
  production: true,
  appBrand: 'My University',
  appColors: {
    primary: '#0066CC',
    secondary: '#FF6B35',
    accent: '#00A86B'
  },
  apiUrl: 'https://moodle.ejemplo.edu'
};
```

### Usar Variables en Componentes

```typescript
import { environment } from '@env/environment';

@Component({
  selector: 'app-header',
  template: `<h1>{{ environment.appBrand }}</h1>`
})
export class HeaderComponent {
  environment = environment;
}
```

---

## Checklist Completo de Personalización Visual

- [ ] Colores primarios en `_app-variables.scss`
- [ ] Logo de login (240x60px PNG)
- [ ] Logo superior (120x30px PNG)
- [ ] Favicon (64x64px)
- [ ] Splash screen (1080x1920px)
- [ ] App icon (1024x1024px)
- [ ] Font personalizado (si aplica)
- [ ] Variables de tema Light/Dark
- [ ] `forceColorScheme` configurado en `moodle.config.json`
- [ ] `forceLoginLogo` configurado según necesidad
- [ ] `showTopLogo` configurado
- [ ] `notificoncolor` en color de marca
- [ ] Compilación de desarrollo: `npm start`
- [ ] Compilación de producción: `npm run build:prod`
- [ ] Prueba en dispositivos reales (Android + iOS)
- [ ] Validación de contraste WCAG (accesibilidad)

---

## Validación de Accesibilidad (WCAG 2.1)

### Contraste de Colores

Los colores de texto deben tener suficiente contraste:

**Mínimo recomendado:**
- Texto normal: 4.5:1 (AA)
- Texto grande: 3:1 (AA)
- Idealmente: 7:1 (AAA)

**Herramienta:** WebAIM Contrast Checker
- URL: https://webaim.org/resources/contrastchecker/

**Ejemplo válido:**
```
Fondo: #FFFFFF (blanco)
Texto: #1A1A1A (gris oscuro)
Contraste: 12.63:1 ✓ (AAA)
```

**Ejemplo inválido:**
```
Fondo: #FFFFFF
Texto: #CCCCCC (gris claro)
Contraste: 1.13:1 ✗ (Falló)
```

---

## Herramientas Útiles

| Herramienta | Propósito | URL |
|-------------|----------|-----|
| Figma | Diseño de logos, splash screens | https://figma.com |
| Canva | Diseño rápido | https://canva.com |
| AppIcon.co | Generador de iconos | https://appicon.co |
| ColorHexa | Paletas de colores | https://colorhexa.com |
| WebAIM | Validar contraste | https://webaim.org |
| TinyPNG | Comprimir imágenes | https://tinypng.com |
| ImageMagick | Redimensionar batch | https://imagemagick.org |
| FontAwesome | Buscar iconos | https://fontawesome.com |

---

## Ejemplo: Cambio Completo de Marca

### Paso 1: Modificar Colores

**Archivo:** `src/theme/_app-variables.scss`

```scss
// Cambiar de azul a naranja
$primary-color: #FF6B35;           // Era #0066CC
$secondary-color: #004E89;          // Era #004E89 (mantener)
$accent-color: #FFB703;             // Era #00A86B
```

### Paso 2: Agregar Logo

```bash
# Reemplazar archivos
cp mi-logo.png src/assets/img/login_logo.png
cp mi-logo-pequeño.png src/assets/img/top_logo.png
```

### Paso 3: Configurar

**Archivo:** `moodle.config.json`

```json
{
  "appname": "Mi Institución",
  "notificoncolor": "#FF6B35",
  "forceLoginLogo": true,
  "showTopLogo": "online",
  "forceColorScheme": "light"
}
```

### Paso 4: Compilar

```bash
npm run build:prod
```

### Paso 5: Validar

```bash
npm start
# Visitar http://localhost:8100
# Verificar colores, logos, y temas
```

---

**Documento creado:** Enero 2026
**Versión:** 1.0
