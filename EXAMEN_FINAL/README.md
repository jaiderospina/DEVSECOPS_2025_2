
## Trabajo Práctico: PROYECTO CENTINELA

**Implementación de un Pipeline DevSecOps de Ciclo Completo para una Plataforma Contenerizada de Análisis de Desinformación y OSINT (Inteligencia de Fuentes Abiertas)**

### 1. Contexto y Justificación

En el panorama digital actual, la desinformación (noticias falsas) y la manipulación de la opinión pública en redes sociales representan amenazas significativas. Combatirlas requiere herramientas ágiles, escalables y, sobre todo, seguras.

Este trabajo propone la creación de una plataforma funcional, llamada **"Proyecto Centinela"**, diseñada para:
1.  **Combatir noticias falsas** mediante web scraping y contrastación de fuentes.
2.  **Evaluar el impacto** de campañas de información/desinformación.
3.  **Gestionar la publicación** de contenido verificado en múltiples plataformas.

El verdadero desafío y núcleo de este trabajo no es solo construir la aplicación, sino **construir, asegurar y automatizar el *ciclo de vida completo* de la aplicación** utilizando exclusivamente herramientas de acceso libre (FOSS) y un enfoque 100% contenerizado, demostrando la maestría en los principios de DevSecOps.

### 2. Objetivos del Trabajo

#### Objetivo General
Diseñar, implementar, desplegar y operar un pipeline de Integración Continua, Entrega Continua y Seguridad Continua (CI/CD/CS) que gestione el ciclo de vida completo de una aplicación de microservicios, integrando la seguridad en cada fase (DevSecOps).

#### Objetivos Específicos
1.  **Desarrollar la Aplicación "Centinela"** con las funcionalidades de web scraping, análisis de sentimiento básico e integración de APIs sociales.
2.  **Contenerizar** todos los componentes de la aplicación (Frontend, Backend, Base de Datos, Workers) usando Docker.
3.  **Implementar un pipeline CI/CD** robusto (ej. GitLab CI, Jenkins) que automatice la construcción, prueba y despliegue. **Opcional**
4.  **Integrar herramientas de seguridad FOSS** en *cada* etapa del pipeline (Shift-Left Security).
5.  **Desplegar** la aplicación en un orquestador de contenedores (ej. K3s o Docker Swarm) utilizando Infraestructura como Código (IaC).
6.  **Establecer un sistema de monitoreo** de seguridad y rendimiento en tiempo real para la aplicación en producción.**Opcional**

---

### 3. Descripción de la Aplicación "Centinela"

La aplicación se construirá sobre una arquitectura de microservicios para facilitar la contenerización y el despliegue independiente.

* **Frontend:** Una SPA (Single Page Application) desarrollada en **Vue.js** o **React**.
* **Backend (API Gateway):** Un servicio principal en **Python (FastAPI)** o **Node.js (Express)** que gestiona las peticiones.
* **Servicio de Scraping:** Un *worker* independiente (Python con **Scrapy** o **BeautifulSoup**) que recibe URLs, extrae contenido y lo contrasta con una lista de fuentes fiables.
* **Servicio de Análisis:** Un microservicio que utiliza librerías de NLP (como **NLTK** en Python) para un análisis de sentimiento básico del impacto en redes.
* **Servicio de Publicación:** Gestiona las conexiones y publicaciones en APIs sociales (ej. **Mastodon**, **Reddit**, y **X/Twitter** en modo *developer*).
* **Base de Datos:** **PostgreSQL** o **MongoDB** para almacenar hallazgos, URLs y métricas.
* **Broker de Mensajes:** **RabbitMQ** o **Redis** para comunicar el API Gateway con los *workers* de scraping de forma asíncrona.

### 4. Metodología: El Ciclo DevSecOps y Herramientas FOSS

Este es el núcleo del trabajo. El alumno deberá implementar y documentar cada una de las siguientes fases, integrando las herramientas de seguridad correspondientes.

#### Fase 1: Plan (Planificación)
* **Actividad:** Definición de requisitos de seguridad y modelado de amenazas.
* **Herramienta FOSS:**
    * **Gestión:** **GitLab Issues** (Community Edition) o **Taiga**.
    * **Modelado de Amenazas:** **OWASP Threat Dragon** para crear diagramas de flujo de datos (DFD) e identificar amenazas (STRIDE).

#### Fase 2: Code (Codificación)
* **Actividad:** Desarrollo de código, análisis de seguridad estático (SAST) y análisis de dependencias (SCA) antes de confirmar (commit).
* **Herramienta FOSS:**
    * **Repositorio:** **GitLab** (CE).
    * **Hooks Pre-commit:** **Gitleaks** o **TruffleHog** (para detectar secretos y claves API hardcodeadas).
    * **SAST (Análisis Estático):** **Semgrep** (para reglas de seguridad personalizadas) y **Bandit** (específico para Python).
    * **SCA (Análisis de Dependencias):** **OWASP Dependency-Check** o **Trivy** (para escanear `requirements.txt`, `package.json`, etc.).

#### Fase 3: Build (Construcción)
* **Actividad:** El servidor CI compila el código, construye las imágenes Docker y las escanea en busca de vulnerabilidades.
* **Herramienta FOSS:**
    * **Servidor CI:** **GitLab CI/CD** (usando `.gitlab-ci.yml`) o **Jenkins**.
    * **Contenerización:** **Docker** y **Dockerfile**.
    * **Escaneo de Imágenes:** **Trivy** o **Grype** (integrado en el pipeline para escanear la imagen Docker base y las capas añadidas en busca de CVEs).

#### Fase 4: Test (Pruebas)
* **Actividad:** Ejecución de pruebas unitarias, de integración y de seguridad dinámicas (DAST) en un entorno de *staging*.
* **Herramienta FOSS:**
    * **Pruebas Unitarias:** **Pytest** (Python) o **Jest** (Node.js/React).
    * **DAST (Análisis Dinámico):** **OWASP ZAP (Zed Attack Proxy)**, ejecutado en modo *baseline scan* o *full scan* automatizado contra el entorno de *staging*.

#### Fase 5: Release & Deploy (Lanzamiento y Despliegue)
* **Actividad:** Aprobación del *build*, versionado y despliegue automatizado en producción usando IaC.
* **Herramienta FOSS:**
    * **IaC:** **Terraform** o **Ansible** para definir la infraestructura (VPS, redes, etc.).
    * **Escaneo de IaC:** **Checkov** o **tfsec** (para escanear los archivos de Terraform en busca de malas configuraciones de seguridad).
    * **Orquestación:** **K3s (Kubernetes ligero)** o **Docker Compose** / **Docker Swarm** para el despliegue de los contenedores.
    * **Registro de Contenedores:** **GitLab Container Registry** (FOSS).

#### Fase 6: Operate & Monitor (Operación y Monitoreo)
* **Actividad:** Observabilidad (logs, métricas) y Detección de Amenazas en Tiempo de Ejecución (Runtime Security).
* **Herramienta FOSS:**
    * **Logs:** Stack **PLG (Promtail, Loki, Grafana)** o el stack **ELK** (Elasticsearch, Logstash, Kibana - Versión Open Source).
    * **Métricas:** **Prometheus** (para métricas de la aplicación y el clúster) y **Grafana** (para dashboards).
    * **Runtime Security:** **Falco** (para detectar comportamiento anómalo a nivel de sistema y contenedores, ej. "Se abrió un shell en un contenedor de frontend").
    * **SIEM (Opcional Avanzado):** **Wazuh** (para correlacionar eventos de seguridad de Falco, ZAP y los logs de la aplicación).

---

### 5. Entregables

1.  **Código Fuente Completo:**
    * Código de la aplicación "Centinela" (Frontend, Backend, Servicios).
    * Todos los `Dockerfiles` y archivos `docker-compose.yml`.
    * Archivos de configuración del pipeline (`.gitlab-ci.yml` o `Jenkinsfile`).
    * Scripts de IaC (Terraform/Ansible) y manifiestos de K3s/Kubernetes.

2.  **Documento Final (Trabajo Práctico):**
    * **Introducción:** Justificación y objetivos.
    * **Arquitectura:** Diagramas de la aplicación y del pipeline DevSecOps.
    * **Modelado de Amenazas:** Salidas de Threat Dragon (DFDs y amenazas identificadas).
    * **Implementación:** Explicación de la elección de cada herramienta FOSS y su integración en el ciclo.
    * **Resultados de Seguridad:** Reportes de ejemplo generados por Gitleaks, Trivy, ZAP y Falco, y cómo se gestionaron (o se gestionarían) los hallazgos.
    * **Monitoreo:** Capturas de pantalla de los dashboards de Grafana/Kibana. **Opcional - No requerido**
    * **Conclusiones:** Desafíos encontrados (ej. limitaciones de APIs sociales) y lecciones aprendidas.

3.  **Video-Demostración (15 min):**Opcional**

    * Una demostración en vivo de un *commit* de código (ej. introduciendo una vulnerabilidad a propósito), mostrando cómo el pipeline lo detecta (falla el *build*), cómo se corrige, y cómo se despliega exitosamente hasta producción, finalizando con la visualización en los dashboards de monitoreo.
  
      **NOTA** Se mencion gitlab, el cual en nuestro curso se reemplazó por github.
