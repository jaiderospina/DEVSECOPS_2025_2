📘 Presentación del Proyecto
El proyecto Hat.sh Reforged tiene como finalidad analizar, fortalecer y personalizar la aplicación web de cifrado de archivos Hat.sh, una herramienta FOSS (Free and Open Source Software) desarrollada con tecnologías modernas como Next.js y libsodium.js.
El proceso incluyó prácticas de ingeniería inversa, evaluación de vulnerabilidades, ajustes de seguridad (hardening) y la creación de una imagen Docker personalizada y reforzada lista para despliegue.

⚙️ 1. Análisis e Ingeniería Inversa
Se inició con la revisión de la aplicación original desde su repositorio oficial:
🔗 https://github.com/sh-dv/hat.sh
El propósito fue comprender su arquitectura y componentes principales:


Frontend: Next.js (React v12)


Dependencias clave: libsodium-wrappers, @material-ui/core


Servidor: Contenedor Nginx que sirve archivos estáticos


Archivos importantes:


/src/: componentes y lógica de la interfaz.


/public/: recursos estáticos (logos, íconos, etc.).


Dockerfile: proceso de construcción de la imagen.


package.json: dependencias y scripts de ejecución.




Durante esta fase se identificaron las rutas donde sería necesario modificar elementos visuales (branding) y textos institucionales.

🔍 2. Análisis de Vulnerabilidades
Para evaluar el nivel de seguridad del proyecto, se realizaron análisis con distintas herramientas FOSS:


npm audit: para detectar vulnerabilidades en dependencias JavaScript.


hadolint: para revisar buenas prácticas del Dockerfile.


Trivy: para analizar vulnerabilidades en la imagen base de Docker.


Docker Scout: para complementar los resultados y comparar versiones más seguras de las dependencias.


Resultados iniciales:


Vulnerabilidades críticas relacionadas con dependencias antiguas de Node.js.


Uso de etiquetas flotantes en Docker (FROM node:alpine), lo cual generó fallas de compatibilidad.


Librerías del sistema con fallas conocidas en la imagen de Nginx.



🛠️ 3. Fortalecimiento (Hardening)
A partir de los hallazgos, se implementaron las siguientes acciones:


Compatibilidad: fijar la versión de Node.js en node:18-alpine para asegurar estabilidad.


Actualización de dependencias: ejecución de npm audit fix para corregir vulnerabilidades.


Optimización del Dockerfile:
RUN apk update && apk upgrade && rm -rf /var/cache/apk/*
ENV NEXT_TELEMETRY_DISABLED=1



Verificación final: se confirmó que no existían vulnerabilidades críticas tras aplicar los parches.



🎨 4. Personalización del Branding
Una vez la aplicación fue estabilizada y segura, se procedió a personalizar los elementos visuales:


Reemplazo de logos y colores en la carpeta public/assets2/.


Modificación de textos en los archivos:


src/components/Footer.js → leyendas personalizadas.


locales/index.js → mensaje institucional propio.




Estas modificaciones se probaron en un entorno de desarrollo local con:
docker run -v $(pwd):/app node:18-alpine npm run dev


🧱 5. Construcción y Despliegue
Se creó una nueva imagen Docker endurecida y personalizada.
Pasos:
# Construir la imagen
docker build -t hat-sh-reforged .

# Ejecutar localmente
docker run -d -p 8080:80 --name hatsh-app hat-sh-reforged

La aplicación quedó disponible en http://localhost:8080 y lista para subir a un repositorio público (Docker Hub).

🧩 6. Conclusiones
El desarrollo de Hat.sh Reforged permitió aplicar los principios de DevSecOps de forma práctica, combinando ingeniería inversa, análisis de vulnerabilidades, mitigación y despliegue seguro.
El resultado fue una imagen Docker funcional, sin vulnerabilidades críticas y con branding propio, demostrando el valor de integrar la seguridad dentro del ciclo de desarrollo desde las primeras etapas.

¿Quieres que te ayude a agregar una sección final de referencias y herramientas utilizadas (por ejemplo, Visual Studio Code, Docker Desktop, Trivy, Hadolint, npm audit, etc.) para completar el formato académico?
