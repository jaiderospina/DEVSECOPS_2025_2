# 🧩 Proyecto: Hat.sh Reforged  
### 👥 Grupo 2  

---

## 💡 Presentación del Proyecto  

**Hat.sh Reforged** es una versión reforzada y personalizada de la aplicación web de código abierto [Hat.sh](https://github.com/sh-dv/hat.sh), centrada en la implementación de prácticas **DevSecOps** y en la mejora tanto de la **seguridad** como de la **interfaz gráfica**.  

El objetivo del proyecto es crear una versión más segura, optimizada y profesional, lista para ser desplegada en entornos de producción mediante Docker.  

---

## ⚙️ Objetivo General  

Fortalecer la aplicación original **Hat.sh** mediante un proceso integral que incluye:  

- Análisis de vulnerabilidades en el código fuente (**SAST**) y dependencias (**SCA**).  
- Mitigación de riesgos detectados.  
- Incorporación de buenas prácticas DevSecOps.  
- Rediseño visual de la interfaz, manteniendo una experiencia moderna y segura.  
- Generación de una **imagen Docker** reforzada y disponible públicamente.  

---

## 🧱 Tecnologías Utilizadas  

- **HTML5 / CSS3**  
- **Node.js**  
- **Docker**  
- **DevSecOps Tools (SAST & SCA)**  
- **GitHub Actions**  
- **Docker Hub**
- **Docker Scout**

---

## 🧰 Refuerzo de Seguridad  

Durante el análisis y endurecimiento de seguridad, se implementaron las siguientes mejoras:  

| Tipo de vulnerabilidad | Riesgo | Acción correctiva aplicada |
|--------------------------|--------|----------------------------|
| Dependencias desactualizadas | Medio | Actualización controlada mediante `npm ci` y revisión de `package-lock.json`. |
| Ejecución como root en contenedor | Alto | Creación de usuario no root (`hat`) y aplicación de permisos mínimos. |
| Contexto de build innecesario | Bajo | Implementación de `.dockerignore` para reducir superficie de ataque. |
| Falta de healthcheck | Medio | Incorporación de `HEALTHCHECK` en el Dockerfile para monitoreo activo. |
| Falta de limpieza post-build | Bajo | Eliminación de paquetes temporales y reducción del tamaño de imagen. |
| Falta de pipeline de verificación | Medio | Configuración de GitHub Actions para auditar dependencias y validar el build. |

---

## 🎨 Interfaz Gráfica (Nueva versión UI)

La interfaz de **Hat.sh Reforged – Grupo 2** fue rediseñada completamente para ofrecer una experiencia visual moderna y coherente con el enfoque **cyber-secure** del proyecto.  

**Principales mejoras:**
- Tema **oscuro tipo terminal**, con acentos **neón verde-azulados**.  
- Tipografía **Roboto Mono** para un aspecto técnico y legible.  
- Botones animados con brillo sutil y efectos de transición.  
- Diseño **responsive** centrado, adaptado a escritorio y móvil.  
- Fondo con degradado oscuro y contraste optimizado para accesibilidad.  
- **Logo textual:** *“Hat.sh Reforged – Grupo 2”*.  
- **Footer:** `© 2025 Grupo 2 – Hat.sh Reforged | DevSecOps Project`.  

---

## 🔗 Repositorios del Proyecto  

🐳 **Docker Hub:** [https://hub.docker.com/r/jonateven12/hat2.sh](https://hub.docker.com/r/jonateven12/hat2.sh)  
💻 **GitHub:** [(https://github.com/javierprias/hat.sh-grupo2.git)  ](https://github.com/javierprias/hat.sh-grupo2.git)

---

## 📜 Licencia  

Este proyecto se distribuye bajo los términos de la **licencia MIT**, respetando los derechos del repositorio original [Hat.sh](https://github.com/sh-dv/hat.sh).

---

## 👥 Créditos  

Desarrollado por el **Grupo 2** como parte del proceso de análisis, refuerzo y despliegue seguro de aplicaciones FOSS bajo un enfoque **DevSecOps**.


