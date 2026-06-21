# FD03: Especificación de Requerimientos del Software

## Proyecto: SecOps — Sistema de Enmascaramiento de Datos y Monitoreo de Rendimiento para Entornos Multi-Base de Datos

**Versión:** 1.0  
**Fecha:** Junio 2026  

---

## INTRODUCCIÓN

### I. Generalidades de la Empresa

1. Nombre de la Empresa:

- SecOps Solutions S.A.

2. Visión:

- Ser referentes en soluciones de protección de datos y observabilidad para infraestructuras de bases de datos empresariales, habilitando cumplimiento normativo y optimización de rendimiento.

3. Misión:

- Entregar una plataforma confiable y fácil de integrar que permita proteger información sensible y medir el impacto de las medidas de seguridad en el rendimiento de los sistemas de bases de datos.

4. Organigrama:

- Dirección General
  - CTO / Arquitectura
  - Equipo de Desarrollo
  - Equipo de Seguridad y Compliance
  - Equipo de Operaciones (DBAs)
  - Soporte y Atención al Cliente

---

### II. Visionamiento de la Empresa

1. Descripción del Problema:

- Las organizaciones deben proteger datos sensibles (PII, datos financieros, PHI) sin degradar el rendimiento de sus sistemas transaccionales y analíticos. Las soluciones aisladas carecen de visibilidad conjunta y generan sobrecostos operativos.

2. Objetivos de Negocios:

- Ofrecer una solución integrada que reduzca el riesgo de exposición de datos, facilite el cumplimiento normativo y permita medir el coste operacional del enmascaramiento en tiempo real.

3. Objetivos de Diseño:

- Arquitectura modular y escalable
- Integración con múltiples motores de BD
- Mínima latencia añadida por el enmascaramiento
- Interfaces intuitivas para DBAs y equipos de seguridad

4. Alcance del Proyecto:

- Desarrollo de los módulos de enmascaramiento y monitoreo, API REST, dashboard web, y conectores para MySQL y PostgreSQL en la fase inicial.

5. Análisis de Riesgos (Matriz de Mitigación):

| Riesgo | Probabilidad | Impacto | Mitigación |
|--------|--------------|---------|------------|
| Degradación de rendimiento por enmascaramiento | Media | Alto | Optimización de algoritmos, pruebas de carga, fases de despliegue gradual |
| Acceso no autorizado a claves de cifrado | Baja | Crítico | KMS, rotación de claves, control de acceso estricto |
| Incompatibilidad con versiones de BD | Media | Medio | Definir versiones soportadas, pruebas en CI |
| Falta de adopción por usuarios | Media | Medio | Capacitación, UX centrado en usuario, pilotos |

6. Viabilidad del Sistema:

- Técnica: Alta (tecnologías maduras disponibles)
- Económica: Viable con inversión inicial y plan de soporte
- Operacional: Requiere coordinación con equipos de BD y seguridad

7. Infraestructura Tecnológica:

- Servidores Linux/Windows, contenedores Docker, orquestación Kubernetes (opcional), PostgreSQL para metadatos del sistema, Prometheus/ELK para métricas y logs.

---

## III. Análisis de Procesos

### a) Diagrama del Proceso Actual (Reactivo)

Se incluye diagrama PlantUML en: [Diagrams/process_actual.puml](Diagrams/process_actual.puml)

### b) Diagrama del Proceso Propuesto (Proactivo)

Se incluye diagrama PlantUML en: [Diagrams/process_propuesto.puml](Diagrams/process_propuesto.puml)

---

## IV. Especificación de Requerimientos de Software

### a) Cuadro de Requerimientos Funcionales Final

| ID | Requisito Funcional | Descripción | Prioridad |
|----|---------------------|-------------|-----------|
| RF-01 | Conexión Multi-BD | Permitir conectar y configurar instancias MySQL y PostgreSQL | Alta |
| RF-02 | Enmascaramiento Dinámico | Aplicar políticas de enmascaramiento a resultados de consultas en tiempo real | Alta |
| RF-03 | Monitoreo de Rendimiento | Recolectar métricas de BD y sistema y mostrarlas en dashboard | Alta |
| RF-04 | Gestión de Políticas | CRUD de políticas de enmascaramiento con versionado | Alta |
| RF-05 | Auditoría | Registrar accesos y cambios en políticas | Alta |
| RF-06 | API REST | Exponer endpoints para consultas y administración | Media |
| RF-07 | Alertas | Envío de alertas configurables por umbral | Media |

### b) Cuadro de Requerimientos No Funcionales

| ID | Tipo | Descripción | Meta |
|----|------|-------------|------|
| RNF-01 | Rendimiento | Overhead de enmascaramiento | < 5% promedio |
| RNF-02 | Disponibilidad | Uptime del servicio | >= 99.5% |
| RNF-03 | Seguridad | Comunicaciones cifradas, logs inmutables | TLS/AES-256 |
| RNF-04 | Escalabilidad | Soporte para 1000 conex. concurrentes | Escalable horizontalmente |
| RNF-05 | Usabilidad | Dashboard responsivo y accesible | UX para roles definidos |

### c) Reglas de Negocio

- RN1: Las políticas de enmascaramiento deben aplicarse por rol de usuario y por columna.
- RN2: Cualquier acceso a datos sensibles debe generar una entrada de auditoría con usuario, timestamp y query.
- RN3: Las claves de cifrado deben rotarse cada 12 meses y su uso debe registrarse.
- RN4: Las excepciones temporales de acceso deben expirar automáticamente y quedar registradas.

---

## V. Fase de Desarrollo

### 1. Perfiles de Usuario:

- Administrador (Admin): Gestión completa del sistema
- DBA: Configuración de conectores, revisión de rendimiento
- Analista de Seguridad: Gestión de políticas y auditoría
- Analista de Datos: Consultas sobre datos enmascarados
- Auditor: Generación de reportes y revisión de logs

### 2. Modelo Conceptual:

#### a) Diagrama de Paquetes

- Archivo: [Diagrams/package_diagram.puml](Diagrams/package_diagram.puml)

#### b) Diagrama de Casos de Uso

- Archivo: [Diagrams/usecase_diagram.puml](Diagrams/usecase_diagram.puml)

#### c) Escenarios de Caso de Uso: Generación de Alerta por CPU alta

- Actor: Sistema de Monitoreo / Administrador
- Precondición: Métrica CPU > umbral configurado
- Flujo Principal:
  1. Recolector detecta CPU > umbral
  2. Genera evento y calcula impacto
  3. Envía alerta (email/webhook) y la muestra en dashboard
  4. Registra evento en auditoría
- Postcondición: Alerta creada y notificada

### 3. Modelo Lógico:

#### a) Diagrama de Secuencia: Recolección y Notificación

- Archivo: [Diagrams/sequence_recoleccion_notificacion.puml](Diagrams/sequence_recoleccion_notificacion.puml)

#### b) Diagrama de Clases

- Archivo: [Diagrams/class_diagram.puml](Diagrams/class_diagram.puml)

---

## RECOMENDACIONES

- Implementar inicialmente con MySQL y PostgreSQL como prioridad.
- Realizar pruebas de carga y un piloto en un entorno controlado.
- Usar infraestructura como código y contenedores para despliegues reproducibles.
- Integrar con KMS para gestión de claves.

---

## BIBLIOGRAFÍA / WEBGRAFÍA

- Ver FD02: referencias generales sobre GDPR, HIPAA, PCI-DSS y NIST
- Documentación oficial de MySQL y PostgreSQL
- OWASP Top 10

---

