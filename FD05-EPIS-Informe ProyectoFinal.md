# FD05: Informe Proyecto Final

## Proyecto: SecOps — Sistema de Enmascaramiento de Datos y Monitoreo de Rendimiento para Entornos Multi-Base de Datos

**Versión:** 1.0  
**Fecha:** Junio 2026

---

## 1. ANTECEDENTES

En las organizaciones actuales, la protección de datos sensibles (PII, financieros, PHI) y la necesidad de mantener un alto rendimiento en sistemas de bases de datos son prioridades estratégicas. Proyectos anteriores y análisis de factibilidad (ver Anexo 01) concluyen que las soluciones aisladas de seguridad o monitoreo no proporcionan visibilidad conjunta ni permiten cuantificar el impacto operativo del enmascaramiento sobre el rendimiento. El presente proyecto fusiona dos iniciativas: un módulo de enmascaramiento de datos y un módulo de monitoreo de rendimiento para ofrecer una plataforma integrada.

Referencias relavantes: [FD01-Informe de Factibilidad](FD01-EPIS-Informe%20de%20Factibilidad_Secops.docx), [FD02-Documento de Visión](FD02-EPIS-Informe%20Vision.md), [FD03-Especificación de Requerimientos](FD03-EPIS-Informe%20Especificacion%20Requerimientos.md), [FD04-Arquitectura de Software](FD04-EPIS-Informe%20Arquitectura%20de%20Software.md)

---

## 2. PLANTEAMIENTO DEL PROBLEMA

### 2.1. Problema

Las organizaciones necesitan proteger datos sensibles a la vez que mantienen niveles de servicio y rendimiento en bases de datos. Las técnicas de enmascaramiento pueden introducir overhead y afectar SLAs, pero muchas organizaciones carecen de herramientas que cuantifiquen ese impacto y ofrezcan controles centralizados y auditables.

### 2.2. Justificación

Implementar una plataforma integrada permitirá:
- Cumplir requisitos regulatorios y de privacidad
- Cuantificar el impacto de las medidas de protección en tiempo real
- Reducir riesgo de exposición mediante enmascaramiento consistente y auditado
- Facilitar operaciones y respuesta a incidentes mediante alertas y dashboards

### 2.3. Alcance

El proyecto abordará:
- Desarrollo e integración del módulo de enmascaramiento y del módulo de monitoreo
- Soporte inicial para MySQL y PostgreSQL
- Dashboard web, API REST, y auditoría centralizada
- Piloto en entorno controlado y despliegue en producción tras validación

No se incluye:
- Migración masiva de datos legados fuera de procesos de prueba
- Integraciones con todos los motores de BD desde el inicio (se incorporarán en fases posteriores)

---

## 3. OBJETIVOS

### 3.1. Objetivo General

Desarrollar e implementar la plataforma SecOps que aplique técnicas de enmascaramiento de datos y mida el impacto de estas técnicas en el rendimiento de entornos multi-base de datos, garantizando trazabilidad y cumplimiento.

### 3.2. Objetivos Específicos

- Diseñar e implementar mecanismos de enmascaramiento dinámico aplicables por rol y columna.
- Implementar recolección de métricas y visualización en tiempo real del rendimiento.
- Proveer auditoría y reportes para cumplimiento normativo (GDPR, HIPAA, PCI-DSS).
- Validar el impacto de enmascaramiento mediante pruebas de carga y métricas p50/p95/p99.
- Desplegar un piloto y documentar procedimientos operativos.

---

## 4. MARCO TEÓRICO

El marco teórico incluye conceptos de protección de datos (enmascaramiento, tokenización, cifrado y hashing), métricas de rendimiento (latencia, throughput, p50/p95/p99), arquitectura de observabilidad (logs, métricas, trazas distribuidas) y normas de seguridad (OWASP, NIST, GDPR). Se recomienda revisar las secciones de referencia en FD02 (Visión) y bibliografía adjunta.

---

## 5. DESARROLLO DE LA SOLUCIÓN

### 5.1. Análisis de Factibilidad

#### 5.1.1. Factibilidad Técnica

Las tecnologías propuestas (Node.js/Python para backend, React/Vue para frontend, Prometheus/ELK para métricas y logging, PostgreSQL para metadatos) son maduras y compatibles con despliegues en contenedores Docker y Kubernetes. Conectores para MySQL/PostgreSQL pueden implementarse mediante drivers nativos.

#### 5.1.2. Factibilidad Económica

Se estiman costos iniciales (ver sección 7). El proyecto es económicamente viable con un ROI proyectado en 12-18 meses al reducir riesgos de sanciones y horas de trabajo manual.

#### 5.1.3. Factibilidad Operativa

Requiere coordinación con equipos de DBA y seguridad para acceso a instancias y credenciales. Las operaciones de mantenimiento deben planificarse para ventanas no disruptivas.

#### 5.1.4. Factibilidad Social

Impacto positivo en confianza del cliente y cumplimiento normativo. Requiere programas de concienciación y formación para usuarios.

#### 5.1.5. Factibilidad Legal

Debe asegurarse que el proyecto cumple con normativas locales e internacionales (GDPR, HIPAA, PCI). Incluir acuerdos legales sobre manejo de datos y BAA cuando aplique.

#### 5.1.6. Factibilidad Ambiental

Impacto ambiental mínimo; se recomienda el uso de infraestructura eficiente y consolidación de cargas en nube para eficiencia energética.

### 5.2. Tecnología de Desarrollo

- Backend: Node.js (Express/Fastify) o Python (FastAPI)
- Frontend: React o Vue.js
- Base de datos de metadatos: PostgreSQL
- Observabilidad: Prometheus (métricas) + ELK (logs) o alternativa gestionada
- Contenerización: Docker, orquestación con Kubernetes
- CI/CD: GitHub Actions / GitLab CI
- Gestión de claves: AWS KMS / Azure Key Vault / HashiCorp Vault

### 5.3. Metodología de Implementación

Se propone metodología ágil (Scrum):
- Sprint 0: Preparación (infraestructura, repositorios, CI)
- Fase MVP (3-4 sprints): Conectores MySQL/Postgres, enmascaramiento básico, dashboard esencial
- Fase 2 (siguientes sprints): Enmascaramiento avanzado, auditoría detallada, alertas y reportes
- Piloto, pruebas de carga y despliegue progresivo a producción

### 5.4. Documentación del Proyecto

#### 5.4.1. Documento de Visión
- Archivo: `FD02-EPIS-Informe Vision.md`

#### 5.4.2. Especificación de Requerimientos de Software (SRS)
- Archivo: `FD03-EPIS-Informe Especificacion Requerimientos.md`

#### 5.4.3. Documento de Arquitectura de Software (SAD)
- Archivo: `FD04-EPIS-Informe Arquitectura de Software.md`

---

## 6. CRONOGRAMA

Tabla resumen (6 meses estimados para MVP):

| Fase | Actividades | Duración |
|------|-------------|----------|
| Preparación | Infraestructura, repositorios, CI | 2 semanas |
| Desarrollo MVP | Conectores, enmascaramiento básico, API, dashboard | 8 semanas |
| Monitoreo y Auditoría | Integración métricas, logs, alertas | 4 semanas |
| Pruebas y Ajustes | Pruebas de carga, optimización | 3 semanas |
| Piloto | Despliegue en entorno controlado | 2 semanas |
| Despliegue | Puesta en producción y estabilización | 3 semanas |
| Total | | ~22 semanas (~5.5 meses) |

Incluye sprints de 2 semanas y revisiones con stakeholders al final de cada sprint.

---

## 7. PRESUPUESTO

Estimación simplificada (valores referenciales):

| Concepto | Costo Estimado (USD) |
|----------|----------------------|
| Desarrollo (equipo 4 personas, 6 meses) | 160,000 |
| Infraestructura (servidores, licencias) | 12,000 |
| Herramientas (monitoring/licencias) | 8,000 |
| Formación y documentación | 5,000 |
| Soporte y mantenimiento 1er año | 20,000 |
| Reservas y contingencias (10%) | 20,500 |
| **Total Estimado** | **225,500** |

---

## 8. CONCLUSIONES

SecOps aborda una necesidad real y crítica en las organizaciones: proteger datos sensibles sin perder visibilidad sobre el impacto en el rendimiento. La integración de enmascaramiento dinámico y monitoreo permite operar con mayor seguridad, cumplir normativas y tomar decisiones basadas en métricas objetivas.

---

## 9. RECOMENDACIONES

- Priorizar la implementación del MVP para validar supuestos de rendimiento.
- Implementar CI/CD y pruebas automáticas desde etapas tempranas.
- Incorporar gestión centralizada de claves (KMS) y auditoría inmutable desde el inicio.
- Realizar pruebas de penetración y revisión de seguridad antes de producción.

---

## 10. BIBLIOGRAFÍA

- IEEE Std 830-1998
- ISO/IEC 27001:2013
- OWASP Top 10
- NIST SP 800 series

---

## 11. ANEXOS

### Anexo 01. Informe de Factibilidad
- Archivo: `FD01-EPIS-Informe de Factibilidad_Secops.docx`

### Anexo 02. Documento de Visión
- Archivo: `FD02-EPIS-Informe Vision.md`

### Anexo 03. Documento SRS
- Archivo: `FD03-EPIS-Informe Especificacion Requerimientos.md`

### Anexo 04. Documento SAD
- Archivo: `FD04-EPIS-Informe Arquitectura de Software.md`

### Anexo 05. Manuales y Otros Documentos
- Documentación técnica, guías de usuario y manuales en la carpeta `Documentacion`.

---

