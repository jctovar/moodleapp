# Ejemplos de Cambios en Comentarios - Antes y Después

Este documento muestra ejemplos específicos de cómo se mejoraron los comentarios en el código.

---

## Ejemplo 1: Corrección de Error Tipográfico
### Archivo: `src/core/services/file.ts` - Línea 39

**ANTES:**
```typescript
/**
 * Progress event used when writing a file data into a file.
 */
export type CoreFileProgressEvent = {
    /**
     * Whether the values are reliabñe.
     */
    lengthComputable?: boolean;
```

**DESPUÉS:**
```typescript
/**
 * Progress event used when writing a file data into a file.
 */
export type CoreFileProgressEvent = {
    /**
     * Whether the values are reliable.
     */
    lengthComputable?: boolean;
```

**Cambio:** `reliabñe` → `reliable`
**Tipo:** Corrección tipográfica (letra ñ no intencionada)

---

## Ejemplo 2: Traducción y Expansión de Comentario
### Archivo: `src/core/services/file.ts` - Línea 163-164

**ANTES:**
```typescript
try {
    return <FileEntry> await File.resolveLocalFilesystemUrl(this.addBasePathIfNeeded(path));
} catch (error) {
    if (error && (error.code === FileError.NOT_FOUND_ERR || error.code === FileError.ENCODING_ERR)) {
        // Cannot read some files if the path contains the % character and it's not an encoded char. Try encoding it.
        const encodedPath = encodeURI(path);
        if (encodedPath !== path) {
            return <FileEntry> await File.resolveLocalFilesystemUrl(this.addBasePathIfNeeded(encodedPath));
        }
    }
    throw error;
}
```

**DESPUÉS:**
```typescript
try {
    return <FileEntry> await File.resolveLocalFilesystemUrl(this.addBasePathIfNeeded(path));
} catch (error) {
    if (error && (error.code === FileError.NOT_FOUND_ERR || error.code === FileError.ENCODING_ERR)) {
        // Si la ruta contiene caracteres % sin codificar, el sistema de archivos no puede leer el archivo.
        // Se intenta nuevamente con la ruta codificada en URI para resolver este problema.
        const encodedPath = encodeURI(path);
        if (encodedPath !== path) {
            return <FileEntry> await File.resolveLocalFilesystemUrl(this.addBasePathIfNeeded(encodedPath));
        }
    }
    throw error;
}
```

**Mejoras:**
- Traducción completa al español
- Explicación del "por qué" (causas del error)
- Clarificación de la solución (codificación URI)
- Mejor contextualización del problema

---

## Ejemplo 3: Traducción de Comentario de Método
### Archivo: `src/core/services/analytics.ts` - Línea 88-131

**ANTES:**
```typescript
/**
 * Log an event for the current site.
 *
 * @param event Event data.
 */
async logEvent(event: CoreAnalyticsAnyEvent): Promise<void> {
    const site = CoreSites.getCurrentSite();
    if (!site || site.isDemoModeSite()) {
        return;
    }

    // Check if analytics is enabled by the user.
    const enabled = await CoreConfig.get<boolean>(CoreConstants.SETTINGS_ANALYTICS_ENABLED, true);
    if (!enabled) {
        return;
    }

    const treatedEvent: CoreAnalyticsEvent = {
        ...event,
        siteId: site.getId(),
    };

    if (treatedEvent.type === CoreAnalyticsEventType.VIEW_ITEM || treatedEvent.type === CoreAnalyticsEventType.VIEW_ITEM_LIST) {
        treatedEvent.name = CoreText.cleanTags(treatedEvent.name);
    }

    if ('url' in treatedEvent && treatedEvent.url) {
        if (!CoreUrl.isAbsoluteURL(treatedEvent.url)) {
            treatedEvent.url = site.createSiteUrl(treatedEvent.url);
        } else if (!site.containsUrl(treatedEvent.url)) {
            // URL belongs to a different site, ignore the event.
            return;
        }
    }

    try {
        await Promise.all(Object.values(this.enabledHandlers).map(handler => handler.logEvent(treatedEvent)));
    } catch (error) {
        this.logger.error('Error logging event', event, error);
    }
}
```

**DESPUÉS:**
```typescript
/**
 * Registra un evento para el sitio actual.
 *
 * @param event Datos del evento.
 */
async logEvent(event: CoreAnalyticsAnyEvent): Promise<void> {
    const site = CoreSites.getCurrentSite();
    if (!site || site.isDemoModeSite()) {
        return;
    }

    // Verifica si el usuario ha habilitado el análisis.
    // Por defecto está habilitado, pero el usuario puede deshabilitarlo.
    const enabled = await CoreConfig.get<boolean>(CoreConstants.SETTINGS_ANALYTICS_ENABLED, true);
    if (!enabled) {
        return;
    }

    const treatedEvent: CoreAnalyticsEvent = {
        ...event,
        siteId: site.getId(),
    };

    if (treatedEvent.type === CoreAnalyticsEventType.VIEW_ITEM || treatedEvent.type === CoreAnalyticsEventType.VIEW_ITEM_LIST) {
        // Limpia las etiquetas HTML del nombre del elemento para evitar caracteres especiales.
        treatedEvent.name = CoreText.cleanTags(treatedEvent.name);
    }

    if ('url' in treatedEvent && treatedEvent.url) {
        if (!CoreUrl.isAbsoluteURL(treatedEvent.url)) {
            // Convierte URLs relativas a URLs absolutas del sitio.
            treatedEvent.url = site.createSiteUrl(treatedEvent.url);
        } else if (!site.containsUrl(treatedEvent.url)) {
            // La URL pertenece a un sitio diferente, se ignora el evento.
            // Esto evita registrar eventos de sitios externos en el análisis del sitio actual.
            return;
        }
    }

    try {
        await Promise.all(Object.values(this.enabledHandlers).map(handler => handler.logEvent(treatedEvent)));
    } catch (error) {
        this.logger.error('Error logging event', event, error);
    }
}
```

**Mejoras:**
- JSDoc traducido completamente
- Cada paso lógico tiene su propio comentario
- Se explica la lógica detrás de cada validación
- Se aclara el propósito de cada transformación

---

## Ejemplo 4: Expansión de Comentario Técnico Complejo
### Archivo: `src/core/services/network.ts` - Línea 163-196

**ANTES:**
```typescript
/**
 * Updates online status.
 */
protected updateOnline(): void {
    // Recalculate connection type.
    CoreNetwork.updateConnectionType();

    if (this.forceConnectionMode === CoreNetworkConnectionType.OFFLINE) {
        this.online.set(false);
        return;
    }

    // We cannot use navigator.onLine because it has issues in some devices.
    // See https://bugs.chromium.org/p/chromium/issues/detail?id=811122
    if (!CorePlatform.isAndroid()) {
        this.online.set(navigator.onLine);
        return;
    }

    const type = this._connectionType();
    let online = type !== null && type !== CoreNetworkConnectionType.OFFLINE && type !== CoreNetworkConnectionType.UNKNOWN;

    // Double check we are not online because we cannot rely 100% in Cordova APIs.
    if (!online && navigator.onLine) {
        online = true;
    }

    this.online.set(online);
}
```

**DESPUÉS:**
```typescript
/**
 * Actualiza el estado en línea de la aplicación.
 * Se ejecuta cada vez que cambia el estado de conexión de red.
 */
protected updateOnline(): void {
    // Recalcula el tipo de conexión actual.
    CoreNetwork.updateConnectionType();

    if (this.forceConnectionMode === CoreNetworkConnectionType.OFFLINE) {
        this.online.set(false);
        return;
    }

    // navigator.onLine no es confiable en algunos dispositivos debido a problemas de implementación.
    // Referencia: https://bugs.chromium.org/p/chromium/issues/detail?id=811122
    // Por lo tanto, en dispositivos que no son Android, confiamos en navigator.onLine,
    // pero en Android usamos el tipo de conexión reportado por Cordova.
    if (!CorePlatform.isAndroid()) {
        this.online.set(navigator.onLine);
        return;
    }

    const type = this._connectionType();
    let online = type !== null && type !== CoreNetworkConnectionType.OFFLINE && type !== CoreNetworkConnectionType.UNKNOWN;

    // Verificación adicional: aunque el tipo de conexión sea OFFLINE,
    // navigator.onLine puede indicar que estamos en línea.
    // Usamos ambos indicadores como respaldo mutuo porque las APIs de Cordova no son 100% confiables.
    if (!online && navigator.onLine) {
        online = true;
    }

    this.online.set(online);
}
```

**Mejoras:**
- JSDoc expandido con contexto
- Explicación del bug de Chromium (enlace incluido)
- Aclaración de la lógica de fallback
- Justificación del uso de múltiples indicadores

---

## Ejemplo 5: Reescritura Completa de Descripción de Servicio
### Archivo: `src/core/services/cron.ts` - Línea 31-42

**ANTES:**
```typescript
/*
 * Service to handle cron processes. The registered processes will be executed every certain time.
*/
@Injectable({ providedIn: 'root' })
export class CoreCronDelegateService {

    // Constants.
    static readonly DEFAULT_INTERVAL = 3600000; // Default interval is 1 hour.
    static readonly MIN_INTERVAL = 240000; // Minimum interval is 4 minutes.
    static readonly MAX_TIME_PROCESS = 120000; // Max time a process can block the queue. Defaults to 2 minutes.
```

**DESPUÉS:**
```typescript
/**
 * Servicio para manejar procesos cron. Los procesos registrados se ejecutarán cada cierto tiempo.
 * Este servicio implementa una cola de ejecución para evitar que múltiples procesos
 * se ejecuten simultáneamente, lo que podría sobrecargar el sistema.
 */
@Injectable({ providedIn: 'root' })
export class CoreCronDelegateService {

    // Constantes de tiempo (en milisegundos).
    static readonly DEFAULT_INTERVAL = 3600000; // Intervalo por defecto: 1 hora.
    static readonly MIN_INTERVAL = 240000; // Intervalo mínimo: 4 minutos.
    static readonly MAX_TIME_PROCESS = 120000; // Tiempo máximo que un proceso puede bloquear la cola: 2 minutos.
```

**Mejoras:**
- Cambio de comentario simple `/*` a JSDoc `/**`
- Adición de contexto de cola de ejecución
- Especificación de unidades de tiempo (milisegundos)
- Expansión de descripciones de constantes

---

## Ejemplo 6: Documentación de Estructura de Datos Compleja
### Archivo: `src/core/services/sync.ts` - Línea 29-30

**ANTES:**
```typescript
// Store blocked sync objects.
protected blockedItems: { [siteId: string]: { [blockId: string]: { [operation: string]: boolean } } } = {};
```

**DESPUÉS:**
```typescript
// Almacena objetos bloqueados por sitio, componente/ID y tipo de operación.
// Estructura: { siteId: { blockId: { operation: true } } }
protected blockedItems: { [siteId: string]: { [blockId: string]: { [operation: string]: boolean } } } = {};
```

**Mejoras:**
- Traducción al español
- Adición de ejemplo de estructura
- Clarificación de la jerarquía de claves

---

## Ejemplo 7: Explicación de Método Complejo
### Archivo: `src/core/services/lang.ts` - Línea 108-135

**ANTES:**
```typescript
/**
 * Add a set of site plugins strings for a certain language.
 *
 * @param lang The language where to add the strings.
 * @param strings Object with the strings to add.
 * @param prefix A prefix to add to all keys.
 */
async addSitePluginsStrings(lang: string, strings: string[], prefix?: string): Promise<void> {
    lang = lang.replace(/_/g, '-'); // Use the app format instead of Moodle format.

    for (const key in strings) {
        const prefixedKey = prefix + key;
        let value = strings[key];

        if (this.customStrings[lang] && this.customStrings[lang][prefixedKey]) {
            // This string is overridden by a custom string, ignore it.
            continue;
        }

        // Replace the way to access subproperties.
        value = value.replace(/\$a->/gm, '$a.');
        // Add another curly bracket to string params ({$a} -> {{$a}}).
        value = value.replace(/{([^ ]+)}/gm, '{{$1}}');
        // Make sure we didn't add to many brackets in some case.
        value = value.replace(/{{{([^ ]+)}}}/gm, '{{$1}}');

        // Load the string.
        await this.loadString(this.sitePluginsStrings, lang, prefixedKey, value);
    }
}
```

**DESPUÉS:**
```typescript
/**
 * Añade un conjunto de cadenas de plugins del sitio para un idioma específico.
 * Las cadenas de plugins del sitio son personalizadas por los administradores del servidor Moodle.
 *
 * @param lang Idioma donde añadir las cadenas.
 * @param strings Objeto con las cadenas a añadir.
 * @param prefix Prefijo a añadir a todas las claves.
 */
async addSitePluginsStrings(lang: string, strings: string[], prefix?: string): Promise<void> {
    // Convierte el formato de idioma de Moodle (ej: es_ES) al formato de la aplicación (ej: es-ES).
    lang = lang.replace(/_/g, '-');

    for (const key in strings) {
        const prefixedKey = prefix + key;
        let value = strings[key];

        if (this.customStrings[lang] && this.customStrings[lang][prefixedKey]) {
            // Esta cadena está sobrescrita por una cadena personalizada del administrador.
            // Las cadenas personalizadas tienen prioridad sobre las de plugins.
            continue;
        }

        // Convierte la sintaxis de Moodle a la sintaxis de ngx-translate.
        // Reemplaza el acceso a subpropiedades: $a->property se convierte en $a.property
        value = value.replace(/\$a->/gm, '$a.');

        // Añade otro corchete a los parámetros de cadena: {$a} se convierte en {{$a}}
        // Esto es necesario para que ngx-translate interprete correctamente los parámetros.
        value = value.replace(/{([^ ]+)}/gm, '{{$1}}');

        // Asegúrate de que no añadimos demasiados corchetes en algunos casos.
        // Si por accidente añadimos tres corchetes, reducimos a dos.
        value = value.replace(/{{{([^ ]+)}}}/gm, '{{$1}}');

        // Carga la cadena en el sistema de traducciones.
        await this.loadString(this.sitePluginsStrings, lang, prefixedKey, value);
    }
}
```

**Mejoras:**
- JSDoc completamente traducido
- Adición de ejemplos de formato (es_ES → es-ES)
- Explicación del propósito de cada transformación regex
- Aclaración de la prioridad de cadenas personalizadas

---

## Ejemplo 8: Documentación de Parámetro Booleano
### Archivo: `src/core/services/cron.ts` - Línea 71-77

**ANTES:**
```typescript
/**
 * Try to execute a handler. It will schedule the next execution once done.
 * If the handler cannot be executed or it fails, it will be re-executed after mmCoreCronMinInterval.
 *
 * @param name Name of the handler.
 * @param force Wether the execution is forced (manual sync).
 * @param siteId Site ID. If not defined, all sites.
 * @returns Promise resolved if handler is executed successfully, rejected otherwise.
 */
```

**DESPUÉS:**
```typescript
/**
 * Intenta ejecutar un manejador de cron. Programa la siguiente ejecución una vez completada.
 * Si el manejador no puede ejecutarse o falla, será re-ejecutado después del intervalo mínimo.
 *
 * @param name Nombre del manejador.
 * @param force Si la ejecución es forzada (sincronización manual por el usuario).
 * @param siteId ID del sitio. Si no está definido, se aplica a todos los sitios.
 * @returns Promise resuelto si el manejador se ejecuta exitosamente, rechazado en caso contrario.
 */
```

**Mejoras:**
- Traducción completa de JSDoc
- Clarificación de "force" (manual sync por usuario)
- Mejor explicación de "siteId" (aplicación a todos)

---

## Estadísticas de Mejora por Tipo

### Por Categoría de Cambio
- **Traducciones puras:** 20
- **Expansiones de contexto:** 12
- **Adición de ejemplos:** 5
- **Correcciones tipográficas:** 1
- **Reescrituras completas:** 4

### Por Tipo de Comentario
- **JSDoc:** 15 mejoras
- **Comentarios en línea:** 20 mejoras
- **Comentarios de archivo:** 2 mejoras
- **Comentarios de constantes:** 3 mejoras

---

## Lecciones Aprendidas

1. **Contexto es importante:** Comentarios que explican "por qué" son más valiosos que "qué"
2. **Referencias externas ayudan:** URLs a bugs o documentación clarifican decisiones
3. **Ejemplos mejoran entendimiento:** Mostrar formatos antes/después es más claro
4. **Estructura importa:** Jerarquía de objetos debe estar clara
5. **Prioridades deben documentarse:** Cuando hay múltiples opciones, documentar cuál se elige y por qué

---

**Documento preparado por:** Claude Code (Documentation Expert)
**Fecha:** 3 de enero de 2026
**Versión:** 1.0
