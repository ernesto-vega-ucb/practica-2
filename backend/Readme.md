## Pipeline DevSecOps
- Esta práctica tiene como objetivo implementar y comprender un flujo DevSecOps completo, integrando diseño, desarrollo, seguridad y operaciones en una arquitectura basada en microservicios, utilizando Node.js, Docker, Docker Compose y GitHub Actions.

# Arquitectura del Sistema
El sistema está compuesto por tres servicios principales:
## API Gateway
- Punto de entrada único al sistema.
- Encargado del enrutamiento hacia los microservicios.
- Punto central para aplicar seguridad y control.

## Users Service
- Microservicio encargado de la gestión de usuarios.
- Expone endpoints relacionados con información de usuarios.

## Academic Service
- Microservicio encargado de la gestión académica.
- Expone endpoints relacionados con cursos y datos académicos.

La comunicación entre servicios se realiza a través de una red interna Docker, evitando el uso de localhost entre contenedores.

# Backlog y priorización

El backlog se construyó a partir del análisis del dominio (DDD) y la definición de una arquitectura basada en microservicios, identificando los servicios centrales del sistema y sus responsabilidades.
## 📋 Backlog y Priorización (Enfoque DevSecOps)

### 🎯 Objetivo
Definir un **backlog inicial priorizado** que oriente el desarrollo del sistema, considerando no solo funcionalidades, sino también **aspectos de calidad, seguridad y operación**, alineados con el enfoque **DevSecOps**.

El backlog se construyó a partir del **análisis del dominio (DDD)** y la definición de una **arquitectura basada en microservicios**, identificando los servicios centrales del sistema y sus responsabilidades.

---

### 🧩 Backlog Inicial Priorizado

| Prioridad | Ítem de Backlog | Dominio / Servicio | Justificación DevSecOps |
|----------|----------------|--------------------|-------------------------|
| Alta | Gestión de usuarios (listar) | `users-service` | Servicio base para autenticación, autorización y control de acceso |
| Alta | Exposición unificada de endpoints | `api-gateway` | Punto central de entrada y control del sistema |
| Media | Gestión académica (cursos) | `academic-service` | Funcionalidad de dominio dependiente de usuarios |
| Alta | Tests automatizados por microservicio | Todos | Detección temprana de errores y aseguramiento de calidad |
| Alta | Pipeline CI/CD automatizado | Todos | Automatización de build, test, seguridad y despliegue |
| Alta | Escaneo de seguridad de imágenes | Todos | Prevención de vulnerabilidades en dependencias |
| Media | Smoke tests post-despliegue | `api-gateway` | Verificación básica de disponibilidad |
| Baja | Optimización y mejoras evolutivas | Servicios de dominio | Iteraciones posteriores al MVP |

---

### 🔐 Criterios de Priorización

La priorización del backlog se definió considerando los siguientes criterios:

- **Criticidad del dominio**
- **Reducción temprana de riesgos**
- **Seguridad desde el inicio (Shift-Left Security)**
- **Automatización y reproducibilidad**
- **Viabilidad de despliegue continuo**

Este enfoque permite que el sistema evolucione de manera incremental, segura y controlada.

---

### 🏗️ Relación con la Arquitectura

Cada ítem del backlog se encuentra directamente vinculado a un **microservicio específico**, favoreciendo:

- Bajo acoplamiento
- Escalabilidad
- Análisis de impacto localizado
- Integración natural con pipelines DevSecOps

---

### 🧠 Nota Metodológica
El backlog se concibe como un **artefacto vivo**, susceptible de ser refinado y re-priorizado a medida que evolucionan los requerimientos funcionales, no funcionales y de seguridad.


## Justificación de los cambios realizados en el nuevo archivo yml de actions

Instalación con Caché: Se añadió cache 'npm' en el setup de Node para que las ejecuciones subsiguientes sean mucho más rápidas.

Linting Integrado: Se añadió la ejecución de npx eslint, esto garantiza que si hay mal código o malas prácticas de estilo, el pipeline se detenga antes de gastar recursos en el build.

Versionado Dinámico: Se utiliza ${{ github.sha }} para etiquetar las imágenes Docker, esto es fundamental para la trazabilidad  en el sentido de saber exactamente qué código está dentro de cada imagen.

SCA Robusto: Trivy ahora está configurado con exit-code: 1 y severity: 'CRITICAL,HIGH', esto significa que si una librería externa tiene una vulnerabilidad grave sin agregar un parche, el despliegue se bloquea automáticamente.

Validación de Salud (Smoke Test): El comando curl --fail asegura que si el servicio devuelve un error (404, 500), de manera que el paso se marque como fallido.