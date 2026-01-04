# Ejemplos de Configuración de Branding - Plantillas Listas para Usar

Este documento contiene plantillas de configuración listas para copiar y personalizar según tu caso de uso.

---

## 📋 Tabla de Plantillas Disponibles

1. [Plantilla Básica](#plantilla-básica-educación-primaria)
2. [Educación Superior](#plantilla-educación-superior)
3. [Formación Profesional](#plantilla-formación-profesional)
4. [LMS Empresarial](#plantilla-lms-empresarial)
5. [Instituto Multi-Campus](#plantilla-instituto-multi-campus)
6. [Plataforma de Educación a Distancia](#plantilla-educación-distancia)
7. [Entorno Corporativo Cerrado](#plantilla-corporativo-cerrado)

---

## Plantilla Básica: Educación Primaria

```json
{
  "app_id": "com.educacion.primaria.app",
  "appname": "Mi Escuela",
  "versioncode": 51001,
  "versionname": "5.1.0",
  "cache_update_frequency_usually": 600000,
  "cache_update_frequency_often": 1800000,
  "cache_update_frequency_sometimes": 3600000,
  "cache_update_frequency_rarely": 43200000,
  "default_lang": "es",
  "languages": {
    "es": "Español",
    "en": "English"
  },
  "wsservice": "moodle_mobile_app",
  "demo_sites": {
    "student": {
      "url": "https://demo.moodledemo.net",
      "username": "student",
      "password": "moodle25"
    }
  },
  "defaultZoomLevel": "medium",
  "zoomlevels": {
    "none": 100,
    "medium": 110,
    "high": 120
  },
  "customurlscheme": "moodlemobile",
  "sites": [],
  "multisitesdisplay": "",
  "sitefindersettings": {},
  "onlyallowlistedsites": false,
  "forcedefaultlanguage": false,
  "privacypolicy": "https://miescuela.edu/privacidad",
  "notificoncolor": "#FF6B35",
  "enableanalytics": false,
  "enableonboarding": true,
  "forceColorScheme": "",
  "forceLoginLogo": false,
  "showTopLogo": "online",
  "ioswebviewscheme": "moodleappfs",
  "appstores": {
    "android": "com.educacion.primaria.app",
    "ios": "id123456789"
  },
  "wsrequestqueuelimit": 10,
  "wsrequestqueuedelay": 100,
  "calendarreminderdefaultvalue": 3600,
  "toastDurations": {
    "short": 2000,
    "long": 3500,
    "sticky": 0
  },
  "disableTokenFile": false,
  "collapsibleItemsExpanded": true
}
```

**Cambios necesarios:**
- Reemplazar `app_id` con tu identificador único
- Actualizar `privacypolicy` con tu URL
- Reemplazar IDs de tiendas (`appstores`)

---

## Plantilla: Educación Superior

```json
{
  "app_id": "com.universidad.mobile",
  "appname": "Universidad de Excelencia - App",
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
    "pt-br": "Português (Brasil)"
  },
  "wsservice": "moodle_mobile_app",
  "demo_sites": {
    "student": {
      "url": "https://moodle-demo.universidad.edu",
      "username": "student",
      "password": "demo1234"
    },
    "teacher": {
      "url": "https://moodle-demo.universidad.edu",
      "username": "teacher",
      "password": "demo1234"
    }
  },
  "defaultZoomLevel": "none",
  "zoomlevels": {
    "none": 100,
    "medium": 110,
    "high": 120
  },
  "customurlscheme": "moodlemobile",
  "sites": [
    {
      "url": "https://moodle.universidad.edu",
      "title": "Sistema de Aprendizaje Virtual - Universidad de Excelencia"
    }
  ],
  "multisitesdisplay": "list",
  "sitefindersettings": {
    "displaysitename": true,
    "displayimage": true,
    "displayalias": false,
    "displaycity": false,
    "displaycountry": false,
    "displayurl": false
  },
  "onlyallowlistedsites": false,
  "forcedefaultlanguage": false,
  "privacypolicy": "https://universidad.edu/legal/privacidad",
  "a11yStatement": "https://universidad.edu/legal/accesibilidad",
  "notificoncolor": "#0066CC",
  "enableanalytics": true,
  "enableonboarding": true,
  "forceColorScheme": "",
  "forceLoginLogo": true,
  "showTopLogo": "online",
  "ioswebviewscheme": "moodleappfs",
  "appstores": {
    "android": "com.universidad.mobile",
    "ios": "id987654321"
  },
  "wsrequestqueuelimit": 10,
  "wsrequestqueuedelay": 100,
  "calendarreminderdefaultvalue": 3600,
  "toastDurations": {
    "short": 2000,
    "long": 3500,
    "sticky": 0
  },
  "disableTokenFile": false,
  "customMainMenuItems": [
    {
      "type": "browser",
      "url": "https://universidad.edu/soporte",
      "icon": "fas-headset",
      "label": {
        "es": "Soporte Técnico",
        "en": "Technical Support",
        "pt-br": "Suporte Técnico"
      }
    },
    {
      "type": "browser",
      "url": "https://universidad.edu/biblioteca",
      "icon": "fas-book",
      "label": {
        "es": "Biblioteca Digital",
        "en": "Digital Library",
        "pt-br": "Biblioteca Digital"
      }
    }
  ],
  "collapsibleItemsExpanded": false
}
```

**Cambios necesarios:**
- `app_id`: Crear identificador único
- `appname`: Nombre de tu universidad
- `sites`: URL de tu Moodle
- URLs en `customMainMenuItems`
- `appstores`: Tus IDs reales
- Logo: Preparar `src/assets/img/login_logo.png`

---

## Plantilla: Formación Profesional

```json
{
  "app_id": "com.centrofp.learning",
  "appname": "Centro de Formación Profesional XYZ",
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
    "eu": "Euskara"
  },
  "wsservice": "moodle_mobile_app",
  "demo_sites": {},
  "defaultZoomLevel": "none",
  "zoomlevels": {
    "none": 100,
    "medium": 110,
    "high": 120
  },
  "customurlscheme": "moodlemobile",
  "sites": [
    {
      "url": "https://lms.centrofp.edu",
      "title": "Aula Virtual - Centro de Formación Profesional XYZ"
    }
  ],
  "multisitesdisplay": "compact",
  "sitefindersettings": {
    "displaysitename": true,
    "displayimage": false,
    "displayalias": false,
    "displaycity": false,
    "displaycountry": false,
    "displayurl": false
  },
  "onlyallowlistedsites": false,
  "forcedefaultlanguage": false,
  "privacypolicy": "https://centrofp.edu/privacidad",
  "legalDisclaimer": "https://centrofp.edu/terminos",
  "notificoncolor": "#FF6B35",
  "enableanalytics": false,
  "enableonboarding": false,
  "forceColorScheme": "light",
  "forceLoginLogo": true,
  "showTopLogo": "hidden",
  "ioswebviewscheme": "moodleappfs",
  "appstores": {
    "android": "com.centrofp.learning",
    "ios": "id111222333"
  },
  "wsrequestqueuelimit": 8,
  "wsrequestqueuedelay": 150,
  "calendarreminderdefaultvalue": 3600,
  "toastDurations": {
    "short": 2000,
    "long": 3500,
    "sticky": 0
  },
  "disableTokenFile": false,
  "customMainMenuItems": [
    {
      "type": "browser",
      "url": "https://centrofp.edu/horarios",
      "icon": "fas-calendar-alt",
      "label": "Horarios de Clase"
    },
    {
      "type": "browser",
      "url": "https://centrofp.edu/practicas",
      "icon": "fas-tools",
      "label": "Prácticas en Empresa"
    },
    {
      "type": "browser",
      "url": "https://centrofp.edu/empleo",
      "icon": "fas-briefcase",
      "label": "Bolsa de Empleo"
    }
  ],
  "collapsibleItemsExpanded": false
}
```

---

## Plantilla: LMS Empresarial

```json
{
  "app_id": "com.megacorp.learningplatform",
  "appname": "MegaCorp Learning Hub",
  "versioncode": 51001,
  "versionname": "5.1.0",
  "cache_update_frequency_usually": 300000,
  "cache_update_frequency_often": 600000,
  "cache_update_frequency_sometimes": 1800000,
  "cache_update_frequency_rarely": 21600000,
  "default_lang": "en",
  "languages": {
    "en": "English",
    "es": "Español",
    "fr": "Français",
    "de": "Deutsch"
  },
  "wsservice": "moodle_mobile_app",
  "demo_sites": {},
  "defaultZoomLevel": "none",
  "zoomlevels": {
    "none": 100,
    "medium": 110,
    "high": 120
  },
  "customurlscheme": "moodlemobile",
  "sites": [
    {
      "url": "https://learning.megacorp.com",
      "title": "MegaCorp Learning Platform"
    }
  ],
  "multisitesdisplay": "compact",
  "sitefindersettings": {
    "displaysitename": true,
    "displayimage": true,
    "displayalias": false,
    "displaycity": false,
    "displaycountry": false,
    "displayurl": false,
    "defaultimageurl": "https://megacorp.intranet/logo-default.png"
  },
  "onlyallowlistedsites": true,
  "forcedefaultlanguage": false,
  "privacypolicy": "https://megacorp.intranet/legal/privacy",
  "notificoncolor": "#1F4E78",
  "enableanalytics": true,
  "enableonboarding": false,
  "forceColorScheme": "light",
  "forceLoginLogo": true,
  "showTopLogo": "hidden",
  "ioswebviewscheme": "moodleappfs",
  "appstores": {
    "android": "com.megacorp.learningplatform",
    "ios": "id444555666"
  },
  "wsrequestqueuelimit": 15,
  "wsrequestqueuedelay": 50,
  "calendarreminderdefaultvalue": 3600,
  "toastDurations": {
    "short": 2000,
    "long": 3500,
    "sticky": 0
  },
  "disableTokenFile": false,
  "removeaccountonlogout": true,
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
    },
    {
      "type": "browser",
      "url": "https://megacorp.intranet/support?device={{devicetype}}&version={{osversion}}",
      "icon": "fas-headset",
      "label": "Support"
    }
  ],
  "collapsibleItemsExpanded": true
}
```

---

## Plantilla: Instituto Multi-Campus

```json
{
  "app_id": "com.institutomulticampus.app",
  "appname": "Instituto Multi-Campus",
  "versioncode": 51001,
  "versionname": "5.1.0",
  "cache_update_frequency_usually": 420000,
  "cache_update_frequency_often": 1200000,
  "cache_update_frequency_sometimes": 3600000,
  "cache_update_frequency_rarely": 43200000,
  "default_lang": "es",
  "languages": {
    "es": "Español",
    "en": "English"
  },
  "wsservice": "moodle_mobile_app",
  "demo_sites": {
    "student": {
      "url": "https://demo.institutomulticampus.edu",
      "username": "student",
      "password": "demo"
    }
  },
  "defaultZoomLevel": "none",
  "zoomlevels": {
    "none": 100,
    "medium": 110,
    "high": 120
  },
  "customurlscheme": "moodlemobile",
  "sites": [
    {
      "url": "https://moodle-centro1.institutomulticampus.edu",
      "title": "Centro 1 - Campus Urbano"
    },
    {
      "url": "https://moodle-centro2.institutomulticampus.edu",
      "title": "Centro 2 - Campus Tecnológico"
    },
    {
      "url": "https://moodle-centro3.institutomulticampus.edu",
      "title": "Centro 3 - Campus Virtual"
    }
  ],
  "multisitesdisplay": "grid",
  "sitefindersettings": {
    "displaysitename": true,
    "displayimage": true,
    "displayalias": true,
    "displaycity": true,
    "displaycountry": false,
    "displayurl": false
  },
  "onlyallowlistedsites": false,
  "forcedefaultlanguage": false,
  "privacypolicy": "https://institutomulticampus.edu/privacidad",
  "notificoncolor": "#00A86B",
  "enableanalytics": false,
  "enableonboarding": true,
  "forceColorScheme": "",
  "forceLoginLogo": true,
  "showTopLogo": "online",
  "ioswebviewscheme": "moodleappfs",
  "appstores": {
    "android": "com.institutomulticampus.app",
    "ios": "id777888999"
  },
  "wsrequestqueuelimit": 10,
  "wsrequestqueuedelay": 100,
  "calendarreminderdefaultvalue": 3600,
  "toastDurations": {
    "short": 2000,
    "long": 3500,
    "sticky": 0
  },
  "disableTokenFile": false,
  "customMainMenuItems": [
    {
      "type": "browser",
      "url": "https://institutomulticampus.edu/soporte",
      "icon": "fas-headset",
      "label": "Soporte Técnico"
    },
    {
      "type": "browser",
      "url": "https://institutomulticampus.edu/admisiones",
      "icon": "fas-user-check",
      "label": "Admisiones"
    }
  ],
  "collapsibleItemsExpanded": false
}
```

---

## Plantilla: Educación Distancia

```json
{
  "app_id": "com.educaciononline.platform",
  "appname": "Educación Online Global",
  "versioncode": 51001,
  "versionname": "5.1.0",
  "cache_update_frequency_usually": 600000,
  "cache_update_frequency_often": 1800000,
  "cache_update_frequency_sometimes": 3600000,
  "cache_update_frequency_rarely": 43200000,
  "default_lang": "en",
  "languages": {
    "en": "English",
    "es": "Español",
    "fr": "Français",
    "de": "Deutsch",
    "it": "Italiano",
    "pt-br": "Português",
    "zh-cn": "简体中文",
    "ja": "日本語"
  },
  "wsservice": "moodle_mobile_app",
  "demo_sites": {
    "free": {
      "url": "https://demo.educaciononline.com",
      "username": "demo",
      "password": "demo123"
    }
  },
  "defaultZoomLevel": "none",
  "zoomlevels": {
    "none": 100,
    "medium": 110,
    "high": 120
  },
  "customurlscheme": "moodlemobile",
  "sites": [],
  "multisitesdisplay": "list",
  "sitefindersettings": {
    "displaysitename": true,
    "displayimage": true,
    "displayalias": false,
    "displaycity": true,
    "displaycountry": true,
    "displayurl": true
  },
  "onlyallowlistedsites": false,
  "forcedefaultlanguage": false,
  "privacypolicy": "https://educaciononline.com/privacy",
  "legalDisclaimer": "https://educaciononline.com/terms",
  "notificoncolor": "#FF5722",
  "enableanalytics": true,
  "enableonboarding": true,
  "forceColorScheme": "auto",
  "forceLoginLogo": false,
  "showTopLogo": "online",
  "ioswebviewscheme": "moodleappfs",
  "appstores": {
    "android": "com.educaciononline.platform",
    "ios": "id333444555"
  },
  "wsrequestqueuelimit": 10,
  "wsrequestqueuedelay": 100,
  "calendarreminderdefaultvalue": 3600,
  "toastDurations": {
    "short": 2000,
    "long": 3500,
    "sticky": 0
  },
  "disableTokenFile": false,
  "customMainMenuItems": [
    {
      "type": "browser",
      "url": "https://educaciononline.com/courses?lang={{lang}}",
      "icon": "fas-graduation-cap",
      "label": {
        "en": "Browse Courses",
        "es": "Ver Cursos",
        "fr": "Parcourir les Cours"
      }
    },
    {
      "type": "browser",
      "url": "https://educaciononline.com/community",
      "icon": "fas-users",
      "label": {
        "en": "Community",
        "es": "Comunidad",
        "fr": "Communauté"
      }
    },
    {
      "type": "browser",
      "url": "https://educaciononline.com/certificates",
      "icon": "fas-certificate",
      "label": {
        "en": "My Certificates",
        "es": "Mis Certificados",
        "fr": "Mes Certificats"
      }
    }
  ],
  "collapsibleItemsExpanded": true,
  "displayqroncredentialscreen": true
}
```

---

## Plantilla: Corporativo Cerrado

```json
{
  "app_id": "com.secretcompany.training",
  "appname": "Secret Company Training",
  "versioncode": 51001,
  "versionname": "5.1.0",
  "cache_update_frequency_usually": 300000,
  "cache_update_frequency_often": 600000,
  "cache_update_frequency_sometimes": 1800000,
  "cache_update_frequency_rarely": 21600000,
  "default_lang": "en",
  "languages": {
    "en": "English"
  },
  "wsservice": "moodle_mobile_app",
  "demo_sites": {},
  "defaultZoomLevel": "none",
  "zoomlevels": {
    "none": 100,
    "medium": 110,
    "high": 120
  },
  "customurlscheme": "moodlemobile",
  "sites": [
    {
      "url": "https://training.secretcompany.internal",
      "title": "Secret Company - Training Platform"
    }
  ],
  "multisitesdisplay": "compact",
  "sitefindersettings": {
    "displaysitename": true,
    "displayimage": false,
    "displayalias": false,
    "displaycity": false,
    "displaycountry": false,
    "displayurl": false
  },
  "onlyallowlistedsites": true,
  "forcedefaultlanguage": true,
  "privacypolicy": "https://training.secretcompany.internal/privacy",
  "notificoncolor": "#1A1A1A",
  "enableanalytics": false,
  "enableonboarding": false,
  "forceColorScheme": "light",
  "forceLoginLogo": true,
  "showTopLogo": "hidden",
  "ioswebviewscheme": "moodleappfs",
  "appstores": {
    "android": "com.secretcompany.training",
    "ios": "id666777888"
  },
  "wsrequestqueuelimit": 5,
  "wsrequestqueuedelay": 200,
  "calendarreminderdefaultvalue": 1800,
  "toastDurations": {
    "short": 2000,
    "long": 3500,
    "sticky": 0
  },
  "disableTokenFile": true,
  "removeaccountonlogout": true,
  "clearIABSessionWhenAutoLogin": "all",
  "customMainMenuItems": [
    {
      "type": "browser",
      "url": "https://training.secretcompany.internal/compliance",
      "icon": "fas-lock",
      "label": "Security & Compliance"
    },
    {
      "type": "browser",
      "url": "https://training.secretcompany.internal/reporting",
      "icon": "fas-chart-bar",
      "label": "Training Reports"
    }
  ],
  "collapsibleItemsExpanded": false,
  "displayqroncredentialscreen": false,
  "disabledFeatures": "plugin_call_external,plugin_web_worker"
}
```

---

## 🚀 Paso a Paso para Usar una Plantilla

### 1. Copiar la Plantilla
```bash
# Guardar la plantilla que necesites
cp moodle.config.json moodle.config.json.backup
# Pegar el JSON de la plantilla
```

### 2. Personalizar Valores Clave

```json
{
  "app_id": "com.TU_ORG.TU_APP",        // ← Cambiar
  "appname": "Tu Nombre de App",         // ← Cambiar
  "sites": [                             // ← Cambiar URLs
    { "url": "https://tu-moodle.edu" }
  ],
  "privacypolicy": "https://tu-sitio.com/privacidad",  // ← Cambiar
  "notificoncolor": "#TU_COLOR"          // ← Cambiar a tu color de marca
}
```

### 3. Validar JSON
```bash
node -e "console.log(JSON.stringify(require('./moodle.config.json'), null, 2))" > /dev/null && echo "✓ JSON válido"
```

### 4. Personalizar Assets
```
src/assets/img/
├── login_logo.png      (240x60px)  - Logo de inicio de sesión
└── top_logo.png        (120x30px)  - Logo superior
```

### 5. Compilar
```bash
npm run build:prod
```

### 6. Probar en Desarrollo
```bash
npm start
# http://localhost:8100
```

---

## 📝 Notas Importantes

- **Reemplaza siempre** los IDs de tiendas (`appstores`) con los tuyos reales
- **Valida URLs** en `customMainMenuItems` antes de compilar
- **Usa HTTPS** para todas las URLs en producción
- **Credenciales de demo** deben ser públicas y de prueba solamente
- **Logo local** reemplaza automáticamente `src/assets/img/login_logo.png`

---

## 🔍 Validación de Configuración

Checklist antes de compilar:

- [ ] `app_id` es único y sigue formato: `com.organizacion.app`
- [ ] `appname` representa tu marca/institución
- [ ] `sites` contiene URLs válidas y accesibles
- [ ] `notificoncolor` es un color hexadecimal válido
- [ ] URLs en `customMainMenuItems` son válidas
- [ ] `privacypolicy` apunta a un documento existente
- [ ] JSON pasa validación: `node -e "require('./moodle.config.json')"`
- [ ] Assets de logo están preparados (si `forceLoginLogo: true`)
- [ ] IDs de tiendas (`appstores`) son correctos

---

**Última actualización:** Enero 2026
**Versión de Moodle App:** 5.1.0
