# FD04: Informe de Arquitectura de Software

## Proyecto: SecOps — Sistema de Enmascaramiento de Datos y Monitoreo de Rendimiento para Entornos Multi-Base de Datos

**Versión:** 1.0  
**Fecha:** Junio 2026

---

## 1. INTRODUCCIÓN

### 1.1 Propósito 

Se presenta una visión global y resumida de la arquitectura del sistema SecOps, utilizando el modelo 4+1 (vistas: lógica, procesos, desarrollo, despliegue y casos de uso). Este documento describe las decisiones arquitectónicas clave, su relación con los requisitos funcionales y no funcionales, y las prioridades establecidas (eficiencia vs portabilidad, seguridad vs latencia).

### 1.2 Alcance

El documento se centrará en la vista lógica del framework y en el mapeo hacia las vistas de implementación y despliegue. Se incluirán diagramas y descripciones de las vistas más relevantes; se omiten detalles de procesos de negocio no directamente relacionados con la arquitectura técnica.

### 1.3 Definiciones, Siglas y Abreviaturas

- API: Interfaz de Programación de Aplicaciones
- BD: Base de Datos
- DBA: Administrador de Base de Datos
- ELK: Elasticsearch, Logstash, Kibana
- KPI: Indicador Clave de Rendimiento
- MFA: Autenticación Multifactor
- PII: Información de Identificación Personal
- QAs: Atributos de Calidad (Quality Attributes)
- REST: Representational State Transfer
- TLS: Transport Layer Security

### 1.4 Organización del Documento

- Sección 1: Introducción
- Sección 2: Objetivos y restricciones arquitectónicas
- Sección 3: Representación de la arquitectura (vistas y diagramas)
- Sección 4: Atributos de calidad
- Sección 5: Conclusiones y recomendaciones

---

## 2. OBJETIVOS Y RESTRICCIONES ARQUITECTÓNICAS

### 2.1 Prioritización de requerimientos

Se priorizan los requerimientos que impactan directamente en seguridad y rendimiento. La siguiente tabla resume prioridades iniciales:

| ID | Descripción | Prioridad |
|----|-------------|-----------|
| RF-01 | Enmascaramiento dinámico en consultas | Crítica |
| RF-03 | Monitoreo y recolección de métricas | Crítica |
| RNF-01 | Overhead de enmascaramiento < 5% | Alta |
| RNF-02 | Disponibilidad >= 99.5% | Alta |
| RF-05 | Auditoría detallada | Alta |

### 2.2 Requerimientos Funcionales (priorizados)

| ID | Descripción | Prioridad |
|----|-------------|-----------|
| RF-01 | Conexión a MySQL/PostgreSQL | Crítica |
| RF-02 | Aplicar políticas de enmascaramiento | Crítica |
| RF-06 | API REST para administración | Media |
| RF-07 | Alertas por umbrales | Media |

### 2.3 Requerimientos No Funcionales – Atributos de Calidad

| ID | Descripción | Prioridad |
|----|-------------|-----------|
| QA-Perf | Rendimiento y latencia | Crítica |
| QA-Sec | Seguridad y confidencialidad | Crítica |
| QA-Scal | Escalabilidad | Alta |
| QA-Avail | Disponibilidad | Alta |
| QA-Maint | Mantenibilidad | Media |

### 2.4 Restricciones

- Lenguajes: Backend en Node.js o Python; Frontend en React/Vue
- Entorno de despliegue: Linux containers (Docker), orquestación Kubernetes opcional
- Versiones BD: MySQL >=5.7, PostgreSQL >=10
- Cifrado: TLS 1.2+, AES-256 para datos en reposo

---

## 3. REPRESENTACIÓN DE LA ARQUITECTURA DEL SISTEMA

### 3.1 Vista de Casos de Uso

Se describen los casos de uso principales: configurar conectores, definir políticas, ejecutar consultas enmascaradas, monitorear rendimiento y generar reportes. Véase el diagrama de casos de uso: `Diagrams/FD03/usecase_diagram.puml`.

#### Flujos de eventos - Ejemplo: Ejecutar Consulta Enmascarada

- Actor: Usuario / Aplicación cliente
- Precondición: Conector configurado y usuario autenticado
- Flujo principal:
  1. Usuario envía consulta vía UI o API
  2. API Gateway recibe la petición y la enruta al motor de enmascaramiento
  3. Motor de Enmascaramiento solicita datos al conector de BD
  4. BD devuelve datos originales
  5. Motor aplica políticas y devuelve resultados enmascarados
  6. Resultados son presentados al usuario
- Requisitos derivados: Latencia máxima por consulta, políticas por rol, logging de auditoría

### 3.2 Diagramas de Casos de Uso

- Archivo: `Diagrams/FD03/usecase_diagram.puml`

### 3.3 Vista Lógica

La vista lógica muestra los subsistemas principales: UI/Dashboard, API Gateway, Motor de Enmascaramiento, Motor de Monitoreo, Auditoría y Conectores de BD.

#### Diagrama de Subsistemas (paquetes)

- Archivo: `Diagrams/FD03/package_diagram.puml`

### 3.4 Diagrama de Secuencia (vista de diseño)

- Archivo: `Diagrams/FD03/sequence_recoleccion_notificacion.puml`

### 3.5 Diagrama de Colaboración (vista de diseño)

- Archivo: `Diagrams/FD04/collaboration_diagram.puml` (ver carpeta FD04)

### 3.6 Diagrama de Objetos

- Archivo: `Diagrams/FD04/objects_diagram.puml`

### 3.7 Diagrama de Clases

- Archivo: `Diagrams/FD03/class_diagram.puml`

### 3.8 Diagrama de Base de Datos

- Archivo: `Diagrams/FD04/db_diagram.puml`

---

## 4. VISTA DE IMPLEMENTACIÓN (DESARROLLO)

### 4.1 Diagrama de arquitectura software (paquetes)

- Archivo: `Diagrams/FD04/package_impl_diagram.puml`

### 4.2 Diagrama de componentes (arquitectura del sistema)

- Archivo: `Diagrams/FD04/components_diagram.puml`

---

## 5. VISTA DE PROCESOS

### 5.1 Diagrama de Procesos del sistema (actividad)

- Archivo: `Diagrams/FD04/activity_process_diagram.puml`

---

## 6. VISTA DE DESPLIEGUE (FÍSICA)

### 6.1 Diagrama de despliegue

- Archivo: `Diagrams/FD04/deployment_diagram.puml`

Ejemplo de escenario: despliegue en Kubernetes con pods para API Gateway, Enmascaramiento, Monitoreo, y Conectores, con servicios de backend de BD en subred protegida.

---

## 7. ATRIBUTOS DE CALIDAD DEL SOFTWARE

Se reutilizan y amplían los QAs definidos en FD03. Se incluyen escenarios concretos para validar:

- Funcionalidad: pruebas de aceptación para casos de uso principales
- Usabilidad: tiempo de aprendizaje, eficiencia de tareas comunes
- Confiabilidad: MTTR, MTBF, integridad de logs
- Rendimiento: latencias p50/p95/p99, throughput
- Mantenibilidad: cobertura de pruebas, modularidad

---

## CONCLUSIONES Y RECOMENDACIONES

- Implementar MVP con contenedores Docker y despliegue en entorno de pruebas
- Priorizar seguridad y rendimiento en las primeras iteraciones
- Mantener documentación y diagramas actualizados en `Documentacion/Diagrams/FD03` y `FD04`

---

**Fin FD04**
