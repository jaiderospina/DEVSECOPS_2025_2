## LABORATORIO: PROYECTO "HAT.SH REFORGED"

**Ingeniería Inversa, Hardening y Personalización de una Aplicación Web Open Source (hat.sh)**

### 1\. Contexto y Justificación

La aplicación `hat.sh` es una herramienta web sencilla y útil para compartir secretos de forma segura. Sin embargo, como cualquier aplicación, puede contener vulnerabilidades. Este proyecto se centrará en tomar esta aplicación pública, realizar ingeniería inversa, identificar y corregir vulnerabilidades con herramientas DevSecOps FOSS, personalizar su branding y desplegar una nueva versión endurecida y personalizada. Esto simula un escenario real donde una organización podría adoptar y adaptar un proyecto open source, asegurando su postura de seguridad y alineándolo con su identidad corporativa.

### 2\. Objetivos del Trabajo

#### Objetivo General

Realizar un ciclo completo de ingeniería inversa, análisis de seguridad, corrección de vulnerabilidades, personalización de branding y despliegue de una nueva imagen Docker pública de la aplicación `hat.sh`, documentando cada paso y demostrando la aplicación de principios DevSecOps.

#### Objetivos Específicos

1.  **Ingeniería Inversa:** Comprender la estructura de código, dependencias y flujo de la aplicación `hat.sh`.
2.  **Análisis de Vulnerabilidades:** Utilizar **DockerScout** (u otras herramientas de escaneo de contenedores FOSS) para identificar vulnerabilidades en la imagen original de `hat.sh`.
3.  **Corrección de Vulnerabilidades:** Implementar las correcciones necesarias en el código o en el `Dockerfile` para mitigar las vulnerabilidades encontradas.
4.  **Personalización de Branding:**
      * Cambiar y documentar el cambio de todos los logos presentes en la aplicación.
      * Modificar leyendas como "Built and developed by sh-dv" por una leyenda personalizada.
      * Reemplazar la imagen "Donation accept" y su funcionalidad asociada si es relevante.
5.  **Recompilación y Reconstrucción:** Compilar la aplicación modificada y crear una nueva imagen Docker.
6.  **Despliegue Público:** Subir la nueva imagen Docker a Docker Hub, con un `Overview` bien estructurado.
7.  **Documentación Exhaustiva:** Crear un `README.md` detallado en GitHub que cubra todos los aspectos del proyecto.

### 3\. Fases del Proyecto y Herramientas a Utilizar

#### Fase 1: Ingeniería Inversa y Entendimiento

  * **Actividad:** Descargar el código fuente de `hat.sh`, revisar la estructura de directorios, `package.json` (o similar), `Dockerfile`, scripts de *build*, y comprender cómo funciona la aplicación (Frontend, Backend, etc.). Identificar dónde se encuentran los activos gráficos y las cadenas de texto.
  * **Herramientas:** Editor de texto/IDE (**VS Code**), explorador de archivos, **navegador web** para interactuar con la aplicación original.

#### Fase 2: Análisis de Vulnerabilidades con DockerScout y FOSS Alternativas

  * **Actividad:** Escanear la imagen Docker original de `hat.sh` (si está disponible en Docker Hub) con DockerScout. Si no hay una imagen oficial, construir una imagen a partir del `Dockerfile` provisto y escanearla.
  * **DockerScout:** Realizar el escaneo de vulnerabilidades.
  * **Alternativas FOSS (si DockerScout no es viable o como complemento):**
      * **Trivy:** Para escanear imágenes Docker en busca de CVEs.
      * **OWASP Dependency-Check:** Para analizar las dependencias de proyectos (ej. `package.json` para JS, `composer.json` para PHP) en busca de vulnerabilidades conocidas.
      * **Hadolint:** Para *linting* de `Dockerfiles` y buenas prácticas de seguridad.
  * **Resultado:** Un informe de vulnerabilidades identificadas que servirá como base para la corrección.

#### Fase 3: Corrección de Vulnerabilidades

  * **Actividad:** Basándose en el informe de la Fase 2, modificar el código o, más probablemente, el `Dockerfile` para:
      * Actualizar dependencias a versiones seguras.
      * Remover paquetes innecesarios del contenedor.
      * Implementar principios de seguridad en el `Dockerfile` (ej. ejecutar como usuario no-root, multistage builds para reducir el tamaño y la superficie de ataque, añadir etiquetas de seguridad).
      * Aplicar *patches* si fuera necesario (aunque en este tipo de proyecto es más común actualizar dependencias o configurar el entorno de forma más segura).
  * **Herramientas:** Editor de código, **Docker**.

#### Fase 4: Personalización del Branding y Contenido

  * **Actividad:**
      * **Logos:** Localizar los archivos de imagen de los logos (SVG, PNG, JPG) en el código fuente. Crear nuevos logos personalizados.
      * **Leyendas:** Buscar las cadenas de texto a modificar en los archivos HTML, JS o de configuración de la aplicación.
      * **Imagen "Donation accept":** Localizar esta imagen y el código que la muestra. Sustituirla por una imagen personalizada o eliminarla si la funcionalidad de donación ya no es relevante.
      * **Importante:** Documentar en el `README.md` la ubicación original, el nuevo nombre del archivo, y la línea de código o configuración modificada para cada cambio.
  * **Herramientas:** Editor de código, editor de imágenes (**GIMP** o **Inkscape** si es FOSS).

#### Fase 5: Recompilación y Creación de Nueva Imagen Docker

  * **Actividad:**
      * Recompilar la aplicación con los cambios de código y branding.
      * Crear un nuevo `Dockerfile` (posiblemente basado en el original pero mejorado en la Fase 3) para construir la imagen.
      * Construir la nueva imagen Docker.
      * Verificar localmente que la aplicación modificada funciona correctamente y que los cambios de branding son visibles.
  * **Herramientas:** **Node.js/npm** (para la compilación del frontend), **Docker**.

#### Fase 6: Despliegue Público en Docker Hub

  * **Actividad:**
      * Crear una cuenta en Docker Hub si no se tiene.
      * Etiquetar la nueva imagen Docker con un nombre apropiado (`tu_usuario/hat.sh-hardened:v1.0`).
      * Subir la imagen a Docker Hub.
      * **Crear el `Overview` de Docker Hub:** Debe describir la imagen, su propósito (versión hardened y personalizada de hat.sh), las mejoras de seguridad implementadas, los cambios de branding, y **los pasos claros para su consumo** (ej. `docker pull tu_usuario/hat.sh-hardened:v1.0`, `docker run -p 80:80 tu_usuario/hat.sh-hardened:v1.0`).
  * **Herramientas:** **Docker CLI**, **Navegador web** (para Docker Hub).

### 7\. Entregables

1.  **Repositorio GitHub Completo:**

      * El código fuente de `hat.sh` modificado.
      * Los nuevos logos, imágenes y activos personalizados.
      * El `Dockerfile` final y optimizado.
      * Cualquier script de compilación modificado.
      * **`README.md` Detallado (Ver Estructura a continuación).**

2.  **Imagen Docker Pública en Docker Hub:**

      * La nueva imagen disponible públicamente.
      * El `Overview` de Docker Hub estructurado y descriptivo.

### 8\. Estructura del `README.md` en GitHub

El `README.md` es un entregable clave y debe ser exhaustivo.

````markdown
# PROYECTO "HAT.SH REFORGED"

## Introducción

Este proyecto documenta el proceso de ingeniería inversa, hardening de seguridad y personalización de la aplicación web de código abierto `hat.sh` (https://github.com/sh-dv/hat.sh). El objetivo fue crear una versión más segura, actualizada y con branding personalizado, empaquetada como una nueva imagen Docker y disponible públicamente.

## 1. Ingeniería Inversa y Análisis de la Aplicación Original

Se realizó un análisis exhaustivo de la estructura de `hat.sh` para comprender su funcionamiento, dependencias y dónde se encontraban los elementos a modificar.

* **Tecnologías Identificadas:** [Mencionar Frontend (ej. Vue.js, Webpack), Backend (ej. Nginx, Static HTML/JS), dependencias clave].
* **Estructura del Proyecto:** Breve descripción de los directorios clave (`src`, `dist`, `public`, `assets`).
* **Localización de Branding:** Se identificaron los siguientes archivos y líneas de código relevantes para la personalización:
    * **Logos:**
        * `path/to/original-logo.svg` (ej. `public/img/logo.svg`)
        * `path/to/favicon.ico`
    * **Leyendas:**
        * `src/components/Footer.vue` - Línea X: "Built and developed by sh-dv"
        * `public/index.html` - Título de la página
    * **Imagen "Donation accept":**
        * `path/to/donation-image.png` (ej. `public/img/donation.png`)
        * `src/components/DonationPrompt.vue` - Lógica de visualización.

## 2. Análisis de Vulnerabilidades con DockerScout y Herramientas FOSS

Se utilizó DockerScout para escanear la imagen Docker base de `hat.sh` (o una construida a partir del `Dockerfile` original).

**Comando de Escaneo (Ejemplo):**
```bash
docker build -t hat.sh-original . # Si no hay imagen oficial
docker scout cves hat.sh-original:latest
# O si hay imagen oficial
docker scout cves shdv/hat.sh:latest
````

**Resumen de Vulnerabilidades Identificadas:**

  * **CVE-YYYY-XXXXX:** [Descripción breve de la vulnerabilidad], Afecta a [paquete/dependencia], Severidad [CRITICAL/HIGH/MEDIUM].
  * **CVE-ZZZZ-WWWWW:** [Descripción breve], Afecta a [paquete/dependencia], Severidad [HIGH].
  * *(Añadir más según los resultados de DockerScout)*

**Alternativas FOSS Utilizadas (si aplica):**

  * **Trivy:** `trivy image hat.sh-original:latest` - [Resultados clave o hallazgos adicionales].
  * **Hadolint:** `hadolint Dockerfile` - [Reglas incumplidas y sugerencias].

## 3\. Corrección de Vulnerabilidades

Basándose en los informes de seguridad, se implementaron las siguientes correcciones:

  * **Actualización de Dependencias:** Se actualizó `[nombre-dependencia]` de la versión `X.Y.Z` a la versión `A.B.C` en `package.json` (o similar) para mitigar [CVE-YYYY-XXXXX].
  * **Optimización del Dockerfile:**
      * **Base Image:** Se cambió la imagen base de `node:XX` a `node:XX-alpine` para reducir el tamaño y la superficie de ataque.
      * **Multi-Stage Build:** Se implementó un *multi-stage build* para asegurar que solo los artefactos necesarios se incluyan en la imagen final, descartando herramientas de desarrollo.
      * **Usuario No-Root:** Se configuró el contenedor para ejecutarse con un usuario no-root (`USER node` o `USER appuser`).
      * **Eliminación de Paquetes Innecesarios:** Se eliminaron paquetes de desarrollo o innecesarios tras la compilación.
  * **Otras Correcciones:** [Mencionar cualquier otra corrección específica de código o configuración].

## 4\. Personalización del Branding y Contenido

Se realizaron los siguientes cambios para personalizar la aplicación:

  * **Nuevo Logo Principal:**
      * **Original:** `public/img/logo.svg`
      * **Nuevo Archivo:** `public/img/your-company-logo.svg`
      * **Modificación:** Se actualizó la referencia en `public/index.html` (o el componente Vue.js/React relevante) a `<img src="/img/your-company-logo.svg" alt="Your Company Logo">`.
  * **Nuevo Favicon:**
      * **Original:** `public/favicon.ico`
      * **Nuevo Archivo:** `public/your-company-favicon.ico`
      * **Modificación:** Se actualizó la referencia en `public/index.html`.
  * **Leyenda del Footer:**
      * **Original:** "Built and developed by sh-dv"
      * **Nueva Leyenda:** "Desarrollado y mantenido por [Tu Nombre/Empresa]"
      * **Modificación:** Archivo `src/components/Footer.vue`, línea XX: `<span>{{ new_legend_text }}</span>`.
  * **Imagen y Funcionalidad de Donación:**
      * **Original:** Imagen `public/img/donation.png` y lógica en `src/components/DonationPrompt.vue`.
      * **Cambio:** La imagen de donación fue reemplazada por `public/img/custom-call-to-action.png` y la leyenda modificada a "Apoya nuestro trabajo" (o eliminada si no es relevante). La lógica del componente `DonationPrompt.vue` se ajustó para reflejar este cambio.

## 5\. Compilación de la Aplicación y Creación de la Imagen Docker

### Pre-requisitos

  * Node.js (LTS recomendado)
  * npm
  * Docker

### Pasos para Compilar y Construir la Imagen:

1.  **Clonar el Repositorio Modificado:**

    ```bash
    git clone [https://github.com/tu-usuario/hat.sh-reforged.git](https://github.com/tu-usuario/hat.sh-reforged.git)
    cd hat.sh-reforged
    ```

2.  **Instalar Dependencias de Desarrollo (si se necesita recompilar el frontend):**

    ```bash
    npm install
    ```

3.  **Compilar la Aplicación (Generar los artefactos estáticos):**

    ```bash
    npm run build
    ```

    *Esto generará la carpeta `dist` con los archivos estáticos listos para ser servidos.*

4.  **Crear la Nueva Imagen Docker:**

    ```bash
    docker build -t tu-usuario/hat.sh-reforged:v1.0 .
    ```

## 6\. Despliegue en Docker Hub

La nueva imagen Docker `tu-usuario/hat.sh-reforged:v1.0` ha sido publicada en Docker Hub.

  * **URL de la Imagen en Docker Hub:** `https://hub.docker.com/r/tu-usuario/hat.sh-reforged`

El `Overview` en Docker Hub está estructurado para detallar el propósito de la imagen, sus características de seguridad y los pasos para su consumo.

### Pasos para Consumir la Imagen Docker:

1.  **Descargar la Imagen:**

    ```bash
    docker pull tu-usuario/hat.sh-reforged:v1.0
    ```

2.  **Ejecutar la Aplicación en un Contenedor:**

    ```bash
    docker run -d -p 8080:80 --name hat.sh-reforged-app tu-usuario/hat.sh-reforged:v1.0
    ```

    *La aplicación estará disponible en `http://localhost:8080`*

## Conclusiones

Este proyecto ha demostrado la capacidad de realizar ingeniería inversa en una aplicación existente, aplicar principios de DevSecOps para identificar y mitigar vulnerabilidades, personalizar la identidad de marca, y gestionar el ciclo de vida de una imagen Docker. La documentación exhaustiva asegura la reproducibilidad y el entendimiento de todas las modificaciones realizadas.

-----

