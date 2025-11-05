# SEGUNDA MODIFICACIÓN

# Informe de DevSecOps: Hardening y Personalización de hat.sh

## Tabla de Contenidos

1. Ingeniería Inversa y Análisis de la Aplicación Original
2. Análisis de Vulnerabilidades con DockerScout y Herramientas FOSS
3. Corrección de Vulnerabilidades (Hardening)
4. Personalización del Branding (Fase 4)
5. Recompilación y Despliegue (Fase 5 y 6)
6. Despliegue en Docker Hub
7. Actualizaciones Recientes: Corrección de Bugs y Mejoras
8. Conclusiones

## 1. Ingeniería Inversa y Análisis de la Aplicación Original

### Descripción del Proyecto
Hat.sh es una aplicación web de código abierto para cifrado y descifrado de archivos en el navegador, construida con Next.js y React. La aplicación utiliza WebAssembly y la biblioteca libsodium para operaciones criptográficas del lado del cliente.

### Estructura del Repositorio
```
CHANGELOG.md
DEVSECOPS_REPORT.md
docker-compose.yml
Dockerfile
.vscode/
Hat-DepSecOps/
Hat-DepSecOps/App.tsx
Hat-DepSecOps/index.html
Hat-DepSecOps/index.tsx
Hat-DepSecOps/logo-devsecops.png
Hat-DepSecOps/metadata.json
Hat-DepSecOps/package-lock.json
Hat-DepSecOps/package.json
Hat-DepSecOps/README.md
Hat-DepSecOps/translations.ts
Hat-DepSecOps/tsconfig.json
Hat-DepSecOps/types.ts
Hat-DepSecOps/vite.config.ts
Hat-DepSecOps/components/
Hat-DepSecOps/components/DecryptionPanel.tsx
Hat-DepSecOps/components/EncryptionPanel.tsx
Hat-DepSecOps/components/FileDropzone.tsx
Hat-DepSecOps/components/icons.tsx
Hat-DepSecOps/contexts/
Hat-DepSecOps/contexts/LanguageContext.tsx
Hat-DepSecOps/hooks/
Hat-DepSecOps/hooks/useSodium.ts
Hat-DepSecOps/services/
Hat-DepSecOps/services/cryptoService.ts
```

**Componentes principales identificados:**
- `src/components/`: Componentes React principales (EncryptionPanel, DecryptionPanel, Hero, etc.)
- `pages/`: Páginas Next.js
- `public/`: Archivos estáticos e imágenes
- `service-worker/`: Service Worker para operaciones criptográficas
- `locales/`: Traducciones multiidioma

### Tecnologías Utilizadas
- **Frontend**: Next.js 12.1.6, React 17.0.2
- **UI**: Material-UI v4
- **Criptografía**: libsodium-wrappers, WebAssembly
- **Build**: Browserify, Babel
- **Testing**: Cypress

## 2. Análisis de Vulnerabilidades con DockerScout y Herramientas FOSS

### Análisis de Dependencias (SCA)
Se realizó un análisis exhaustivo de vulnerabilidades utilizando `npm audit`:

```
$ npm audit
found 0 vulnerabilities
```

**Estado actual:**
- **0 vulnerabilidades totales**
- Todas las dependencias han sido actualizadas y securizadas
- Se implementaron mejores prácticas de seguridad en el proceso de construcción

### Análisis de Código Fuente (SAST)
**Vulnerabilidades identificadas:**
1. **XSS en dangerouslySetInnerHTML**: Uso no sanitizado en `pages/about.js`
2. **Uso de eval()**: No encontrado
3. **innerHTML directo**: No encontrado
4. **navigator.clipboard**: Uso seguro (solo escritura)
5. **window.open**: Uso potencialmente inseguro en descargas

### Análisis de Imagen Docker
- **Base Image**: node:alpine (desactualizada)
- **Usuario**: root (privilegios elevados)
- **Permisos**: Sin restricciones específicas
- **Actualizaciones**: No automáticas

## 3. Corrección de Vulnerabilidades (Hardening)

### Mitigación de Dependencias
Se actualizaron todas las dependencias vulnerables:

```
$ npm audit fix
found 0 vulnerabilities
```

**Actualizaciones principales:**
- Next.js: 12.1.6 → 16.0.1
- React: 17.0.2 → 18.2.0
- React-DOM: 17.0.2 → 18.2.0
- Vite: Actualizado a versión compatible
- Todas las dependencias de seguridad actualizadas

### Mitigación XSS
Se implementó sanitización con DOMPurify:

```javascript
import DOMPurify from "isomorphic-dompurify";

// Antes (vulnerable)
<div dangerouslySetInnerHTML={{ __html: marked(docContent) }}></div>

// Después (seguro)
<div dangerouslySetInnerHTML={{ __html: DOMPurify.sanitize(marked(docContent)) }}></div>
```

**Resultado de la implementación:**
- Todas las entradas HTML peligrosas son sanitizadas
- Se previene la ejecución de scripts maliciosos
- Se mantiene la funcionalidad de marcado seguro

### Hardening del Contenedor Docker
Se implementaron mejores prácticas de seguridad:

```dockerfile
FROM node:18-alpine as builder
# ... build steps ...

FROM nginx:1.25-alpine

# Install security updates
RUN apk update && apk upgrade && apk add --no-cache curl

# Create non-root user
RUN addgroup -g 1001 -S nodejs && adduser -S nextjs -u 1001

# Set proper permissions
RUN chown -R nextjs:nodejs /usr/share/nginx/html && \
    chown -R nextjs:nodejs /var/cache/nginx && \
    chown -R nextjs:nodejs /var/log/nginx && \
    chown -R nextjs:nodejs /etc/nginx/conf.d && \
    touch /var/run/nginx.pid && \
    chown -R nextjs:nodejs /var/run/nginx.pid

# Switch to non-root user
USER nextjs

EXPOSE 3991
ENTRYPOINT ["nginx", "-g", "daemon off;"]
```

## 4. Personalización del Branding (Fase 4)

### Nuevo Logo DevSecOps
Se creó un logo personalizado que combina elementos de seguridad con el branding original:

```
🛡️ DevSecOps Hat.sh
```

**Elementos del logo:**
- Escudo de seguridad (🛡️)
- Colores azul y verde (seguridad/tecnología)
- Tipografía moderna y profesional
- Branding personalizado "DevSecOps Edition"

### Actualización de Componentes
Se modificó el componente Hero para incluir el nuevo branding:

```javascript
export default function Hero() {
  return (
    <Container maxWidth="sm" component="main" className={classes.heroContent}>
      <img
        src="/assets/images/logo-devsecops.png"
        alt="DevSecOps Hat.sh Logo"
        style={{ width: '100px', height: '100px', marginBottom: '20px' }}
      />
      <Typography variant="h5" align="center" gutterBottom className={classes.heroTitle}>
        {"Hat.sh - DevSecOps Edition"}
      </Typography>
      <Typography variant="subtitle1" align="center" component="p" className={classes.heroSubTitle}>
        {t('sub_title')}
        <br />
        <strong>Hardened & Secure</strong>
      </Typography>
    </Container>
  );
}
```

## 5. Recompilación y Despliegue (Fase 5 y 6)

### Configuración Docker Compose
Se creó un archivo docker-compose.yml con configuraciones de seguridad adicionales:

```yaml
version: '3.8'

services:
  hatsh:
    build: .
    ports:
      - "3991:3991"
    environment:
      - NODE_ENV=production
    restart: unless-stopped
    security_opt:
      - no-new-privileges:true
    read_only: true
    tmpfs:
      - /tmp
      - /var/run
      - /var/cache/nginx
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:3991"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 40s
```

## 6. Despliegue en Docker Hub

### Preparación y Construcción
```bash
# Construir la imagen
docker build -t hat.sh-by-loiz1 .

# Etiquetar para Docker Hub
docker tag hat.sh-by-loiz1:latest loizzz/hat.sh-by-loiz1:latest
```

### Autenticación y Push a Docker Hub
```bash
# Login a Docker Hub
docker login

# Subir la imagen a Docker Hub
docker push loizzz/hat.sh-by-loiz1:latest
```

### Verificación en Docker Hub
```bash
# Verificar que la imagen se subió correctamente
docker search loizzz/hat.sh-by_loizzz


```

## 7. Actualizaciones Recientes: Corrección de Bugs y Mejoras

### Corrección de Errores en Código
Se identificaron y corrigieron varios bugs en el código fuente:

1. **Propiedad Duplicada en Traducciones (TypeScript Error 1117)**
   - **Problema**: En `Hat-DepSecOps/translations.ts:76-78`, existía una propiedad duplicada `clientSideFileEncryption` en el objeto de traducciones.
   - **Solución**: Se eliminó la entrada duplicada, dejando solo la versión correcta.
   - **Código corregido**:
     ```typescript
     // Antes (con error)
     clientSideFileEncryption: { en: 'Client-Side File Encryption ', es: 'Cifrado de Archivos.' },
     secureFileEncryptor: { en: 'SecureFile Encryptor ', es: 'Cifrador de Archivos Basado en Hat.sh' },
     clientSideFileEncryption: { en: 'Client-Side File Encryption ', es: 'Cifrado de Archivos' },

     // Después (corregido)
     secureFileEncryptor: { en: 'SecureFile Encryptor ', es: 'Cifrador de Archivos Basado en Hat.sh' },
     clientSideFileEncryption: { en: 'Client-Side File Encryption ', es: 'Cifrado de Archivos' },
     ```

2. **Adición de Nueva Traducción Personalizada**
   - **Cambio**: Se agregó una nueva clave de traducción `redesignedBy` para mostrar "Rediseñada por Loizzz Clase DevSecOps".
   - **Implementación**: Se actualizó el tipo `TranslationKey` y se agregó la entrada en el objeto `translations`.
   - **Uso en UI**: Se integró en el componente `Footer` de `App.tsx` para mostrar el crédito en el footer de la aplicación.

### Mejoras en la Arquitectura
- **Separación de Responsabilidades**: Se mantuvo la estructura modular del código, asegurando que las traducciones y componentes estén bien separados.
- **Compatibilidad TypeScript**: Todas las correcciones mantienen la compatibilidad con TypeScript y evitan errores de compilación.

### Impacto en la Seguridad
- Las correcciones no afectan la seguridad de la aplicación, ya que se limitan a errores de sintaxis y adiciones de texto estático.
- Se verificó que no se introdujeron nuevas vulnerabilidades XSS o inyecciones de código.

---

## 🚀 Guía Paso a Paso para Ejecutar el Contenedor

### Prerrequisitos
- Docker instalado y ejecutándose en tu sistema
- Conexión a internet para descargar la imagen

### Método 1:
#### Paso 1: Descargar la Imagen
```bash
# Descargar la imagen desde Docker Hub
docker pull loizzz/hat.sh-by-loiz1:latest
```
#### Paso 2: Ejecutar el Contenedor
```bash
# Ejecutar la aplicación con configuración de seguridad
docker run -p 80:8080 loizzz/hat.sh-by-loiz1:latest
```

#### Paso 3: Verificar que Funciona
```bash
# Verificar que el contenedor está ejecutándose
docker ps

# Ver logs para confirmar que no hay errores
docker logs hatsh-devsecops


#### Paso 4: Acceder a la Aplicación
- Abre tu navegador web
- Ve a: **http://localhost:80**
- ¡Listo!

#### Paso 5: Limpiar (cuando termines)
```bash
# Detener y remover el contenedor
docker stop hatsh-devsecops
docker rm hatsh-devsecops
```

#### Paso 4: Disfruta encryptando tus archivos con una version renobada! 
#### by loiz1 🦊



# PRIMERA MODIFICACIÓN

# **PROYECTO "HAT.SH REFORGED"**

## **Introducción**

Este proyecto documenta el proceso de ingeniería inversa, hardening de seguridad (DevSecOps) y personalización de la aplicación web de código abierto hat.sh (<https://github.com/sh-dv/hat.sh>).

El objetivo fue tomar la aplicación original, realizar un análisis de vulnerabilidades FOSS, mitigar los riesgos encontrados a nivel de código (SAST) y de imagen (SCA), personalizar el branding y desplegar una nueva imagen Docker pública, reforzada y lista para su consumo.

## **1\. Ingeniería Inversa y Análisis de la Aplicación Original**

Se realizó un análisis exhaustivo de la estructura de hat.sh para comprender su funcionamiento, dependencias y dónde se encontraban los elementos a modificar.

- **Tecnologías Identificadas:**
  - **Frontend:** Next.js v12 (React)
  - **Backend:** Nginx (servido estático desde un contenedor Docker)
  - **Dependencias Clave:** libsodium-wrappers (criptografía en el navegador), @material-ui (componentes de UI).
- **Estructura del Proyecto:**
  - src/: Contiene el código fuente de los componentes de React.
  - public/: Contiene todos los activos estáticos (imágenes, logos, iconos).
  - package.json: Define las dependencias y los scripts de build.
  - Dockerfile: Define el proceso de construcción de la imagen.
- **Localización de Branding:**
  - **Logos:**
    - public/assets/images/logo.png (Reemplazado por public/assets2/images/logo.png)
    - public/assets/images/logo2.png (Reemplazado por public/assets2/images/logo2.png)
  - **Rutas de código modificadas (Logos):**
    - src/components/Footer.js | src: /assets/icons/\${res.alt}-logo.png (Reemplazado por src: /assets2/icons/\${res.alt}-logo.png)
  - **Leyendas:**
    - src/components/Footer.js - "Built and developed by sh-dv" (Reemplazado por Modificado por Mauricio)
    - src/components/Footer.js - "Donations" (Reemplazado por Laboratorio Docker Uniminuto)
    - locales/index.js - "Cifrado de archivos del lado del cliente sencillo…." (Reemplazado por App modificada para Laboratorio Uniminuto...)
  - **Imagen "Donation accept":**
    - public/assets/icons/qr-logo.png (Reemplazado por public/assets2/icons/qr-logo.png)

## **2\. Análisis de Vulnerabilidades con DockerScout y Herramientas FOSS**

Se utilizó un conjunto de herramientas FOSS para escanear la aplicación original, complementado con DockerScout.

### **Hallazgo 1: Falla de Compilación del Dockerfile Original**

El primer paso fue consumir el Dockerfile provisto por el desarrollador. Este intento falló inmediatamente.

- **Causa:** El Dockerfile original usaba FROM node:alpine. Esta es una etiqueta "flotante" que descargó la versión más reciente de Node.js (v25+), la cual es incompatible con las dependencias antiguas del proyecto.
- **Error:** TypeError: Cannot read properties of undefined (reading 'prototype').
- **Mitigación Inicial:** El Dockerfile tuvo que ser modificado para fijar una versión compatible (FROM node:18-alpine) antes de que cualquier análisis pudiera comenzar.

### **Hallazgo 2: Análisis de Dependencias de Código (npm audit)**

- **Hallazgo:** Se reportaron **18 vulnerabilidades** en las dependencias de Node.js.
- **Severidad:** 5 CRÍTICAS, 5 ALTAS, 7 Moderadas, 1 Baja.

### **Hallazgo 3: Análisis de Prácticas del Dockerfile (hadolint)**

- **Hallazgo:** hadolint reportó advertencias sobre el uso de etiquetas flotantes (FROM node:alpine) y formato de ENV (Legacy), validando el "Hallazgo 1".

### **Hallazgo 4: Análisis de Vulnerabilidades de Imagen (Trivy)**

Se escaneó la imagen original (construida con node:18-alpine).

**Comando de Escaneo:**

\# Paso 1: Construir la imagen original (con la corrección de Node.js)  
docker build -t hat-sh-dockerfile .  
<br/>\# Paso 2: Escanear la imagen construida  
trivy image hat-sh-dockerfile:latest  

- **Hallazgo:** Se reportaron **5ulnerabilidades** en la imagen final de Nginx.
- **Severidad:** 2 CRÍTICAS (CVE-2025-49794, CVE-2025-49796) y 2 ALTAS (CVE-2025-49795, CVE-2025-6021).
- **Causa Raíz:** Todas las vulnerabilidades se encontraban en la librería libxml2 y tenían un Status: fixed (arreglable).

### **Hallazgo 5: Análisis Complementario (DockerScout)**

- **Hallazgo:** Como análisis complementario, DockerScout (usando su propia base de datos) reportó 4 vulnerabilidades adicionales de severidad Media y Baja (ej. CVE-2025-48174, CVE-2025-48175) que no fueron marcadas como "Fixable" (Arreglables) por las herramientas FOSS.

## **3\. Corrección de Vulnerabilidades (Hardening)**

Se aplicaron las siguientes mitigaciones para crear una imagen final segura:

- **Mitigación de Código (npm):**
  - **Acción:** Se ejecutó npm audit fix.
  - **Resultado:** Se mitigaron **16 de las 18** vulnerabilidades, eliminando **todas las 5 CRÍTICAS**.
- **Mitigación de Imagen (Dockerfile - Trivy):**
  - **Acción:** Se añadió el comando RUN apk update && apk upgrade && rm -rf /var/cache/apk/\* a la etapa final del Dockerfile (la de nginx).
  - **Resultado:** El re-escaneo con trivy confirmó **0 vulnerabilidades** en la imagen final.
- **Mitigación de Prácticas (Dockerfile - Hadolint):**
  - **Acción:** Se corrigió el Dockerfile (fijando node:18-alpine y ENV NEXT_TELEMETRY_DISABLED=1).
  - **Resultado:** hadolint Dockerfile no reportó **ninguna advertencia**.

## **4\. Personalización del Branding (Fase 4)**

Se utilizó un contenedor de desarrollo (con docker run -v ... node:18-alpine npm run dev) para modificar y verificar los cambios de branding en vivo.

- **Logos:** Se crearon los archivos logo.png, logo2.png y qr-logo.png en la carpeta public/assets2 con activos personalizados.
- **Texto:** Se modificaron las leyendas en src/components/Footer.js y locales/index.js para reflejar la nueva autoría.

## **5\. Recompilación y Despliegue (Fase 5 y 6)**

- **Imagen Final:** Se construyó la imagen final, mauriciovergara/hat-sh-laboratoriouniminuto:latest, usando el Dockerfile endurecido. Esta imagen ahora contiene las mitigaciones de seguridad Y las personalizaciones de branding.
- **Despliegue:** La imagen fue subida (push) exitosamente a Docker Hub.

## **6\. Despliegue en Docker Hub**

La nueva imagen Docker mauriciovergara/hat-sh-laboratoriouniminuto:latest está disponible públicamente en Docker Hub.

- **URL de la Imagen:** <https://hub.docker.com/r/mauriciovergara/hat-sh-laboratoriouniminuto>

### **Pasos para Consumir la Imagen:**

- **Descargar la Imagen:**  
    docker pull mauriciovergara/hat-sh-laboratoriouniminuto:latest  

- **Ejecutar la Aplicación en un Contenedor:**  
    docker run -d -p 8080:80 --name hat-sh-reforged-app mauriciovergara/hat-sh-laboratoriouniminuto:latest  
    <br/>(La aplicación estará disponible en <http://localhost:8080>)

## **7\. Conclusiones**

Este proyecto demostró con éxito el ciclo completo de DevSecOps: tomar una aplicación de código abierto, identificar sus debilidades (tanto en el código como en la infraestructura) a través de un análisis de seguridad, y aplicar mitigaciones correctas para remediar el 100% de las vulnerabilidades críticas.

El resultado es un artefacto final (imagen Docker) que no solo está personalizado, sino que también es verificablemente más seguro que el original, listo para un despliegue confiable en producción.



