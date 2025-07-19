# Herramientas Usadas

* Producto: ChatGPT
* LLM: o3

# Resultado del Ejercicio

## 📍 Roadmap de Producto LTI – MVPs Prioritarios

| Prioridad | MVP                                           | Objetivo de Negocio (valor ↗)                                                                                               | Épicas / Hitos clave                                                                                                                                                                                                                                              |
| --------- | --------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1**     | **MVP 1 – “Aplicar & Pipeline Básico”**       | Permitir a candidatos aplicar a vacantes y a reclutadores gestionar un pipeline visual en < 30 días → *primer valor usable* | 1. Autenticación & perfiles básicos<br>2. Publicación de vacante (template)<br>3. Aplicación en 3 pasos + carga de CV<br>4. Dashboard candidato con tracking en tiempo real<br>5. Kanban pipeline reclutador (drag-&-drop)<br>6. Email de confirmación automática |
| **2**     | **MVP 2 – “Comunicación & Multiposting”**     | Aumentar alcance de vacantes y engagement                                                                                   | 1. Multiposting a LinkedIn, Indeed, Computrabajo<br>2. Mensajes masivos (plantillas)<br>3. Notificaciones push & recordatorios                                                                                                                                    |
| **3**     | **MVP 3 – “IA Pre-filtrado”**                 | Reducir tiempo de triage 50 %                                                                                               | 1. Parsing de CV (AI-Service)<br>2. Scoring automático + ranking<br>3. Shortlist auto-enviada al reclutador                                                                                                                                                       |
| **4**     | **MVP 4 – “Evaluación & Selección Avanzada”** | Decisiones objetivas y colaborativas                                                                                        | 1. Programar entrevistas<br>2. Formularios de feedback técnico<br>3. Panel Hiring Manager                                                                                                                                                                         |
| **5**     | **MVP 5 – “Analytics & Reporting”**           | Métricas para ROI y optimización                                                                                            | 1. KPIs de tiempo-a-contratación, calidad y costos<br>2. Exportes & dashboards BI                                                                                                                                                                                 |

---

## ✍️ Historias de Usuario (MVP 1 – “Aplicar & Pipeline Básico”)

### US-1 | Aplicar a Vacante desde Móvil

**Como** candidato,
**quiero** aplicar a una vacante en máximo 3 pasos desde mi teléfono,
**para que** pueda postularme rápidamente sin usar un computador.

#### Criterios de Aceptación

1. **Carga de CV**

   * **Dado que** estoy autenticado en el portal móvil
   * **Cuando** selecciono una vacante y adjunto mi CV (PDF/DOC)
   * **Entonces** el sistema confirma la subida y pasa al siguiente paso
2. **Formulario mínimo**

   * **Dado que** he cargado mi CV
   * **Cuando** completo nombre, email y salario deseado
   * **Entonces** puedo enviar la candidatura sin más campos obligatorios
3. **Confirmación instantánea**

   * **Dado que** envío la candidatura
   * **Cuando** el backend la registra
   * **Entonces** recibo un email y veo un mensaje “¡Aplicación recibida!” en < 5 s

#### Criterios Técnicos

1. Endpoint `POST /candidatures` devuelve **201** y body con `id_candidatura`
2. Validaciones de tamaño < 5 MB y formatos PDF/DOC
3. CV se guarda en S3 con clave `{id_candidatura}.pdf/doc`

#### Notas Adicionales

* Usar presigned-URL para subida directa a S3

#### Historias de Usuario Relacionadas

* US-2 (Seguimiento de estado)

### US-2 | Ver Estado de mi Candidatura

**Como** candidato,
**quiero** consultar el estado de cada postulación en un dashboard,
**para que** sepa en qué etapa voy y no tenga que preguntar al reclutador.

#### Criterios de Aceptación

1. **Listado de candidaturas**

   * **Dado que** estoy en mi panel
   * **Cuando** ingreso a “Mis postulaciones”
   * **Entonces** veo una tarjeta por candidatura con estado y fecha de actualización
2. **Estados claros**

   * **Dado que** la candidatura cambia a `preseleccionado`
   * **Cuando** actualizo el panel
   * **Entonces** el estado se muestra en máximo 60 s
3. **Alertas automáticas**

   * **Dado que** mi candidatura es rechazada
   * **Cuando** se produce el cambio de estado
   * **Entonces** recibo email con motivo configurado por el reclutador

#### Criterios Técnicos

1. WS/API `GET /candidatures?candidateId=…` paginado
2. Websocket/SSE para push de cambios de estado
3. Frontend React PWA offline-ready

#### Notas Adicionales

* Mostrar barra de progreso (% etapas superadas)

#### Historias de Usuario Relacionadas

* US-1, US-3

### US-3 | Crear Vacante con Plantilla

**Como** reclutador,
**quiero** generar una oferta usando plantillas inteligentes,
**para que** publique vacantes consistentes en menos de 2 minutos.

#### Criterios de Aceptación

1. **Selector de plantilla**

   * **Dado que** estoy en “Crear vacante”
   * **Cuando** elijo la plantilla “Backend Java”
   * **Entonces** se precargan título, requisitos y rango salarial
2. **Validación de campos**

   * **Dado que** completo los datos requeridos
   * **Cuando** presiono “Guardar”
   * **Entonces** el sistema crea la vacante en estado `borrador`
3. **Publicar inmediata**

   * **Dado que** edito la vacante y selecciono “Publicar ahora”
   * **Cuando** confirmo la acción
   * **Entonces** la vacante cambia a estado `activa` y es visible para candidatos

#### Criterios Técnicos

1. Endpoint `POST /vacancies` con esquema validado vía Zod
2. Tiempo de respuesta < 200 ms (P95)
3. Job en background para indexación full-text

#### Notas Adicionales

* Plantillas almacenadas en tabla `templates` editable por Admin

#### Historias de Usuario Relacionadas

* US-4

### US-4 | Pipeline Kanban Drag-&-Drop

**Como** reclutador,
**quiero** arrastrar candidatos entre columnas de un pipeline,
**para que** gestione el flujo de selección de forma visual y rápida.

#### Criterios de Aceptación

1. **Renderizar columnas**

   * **Dado que** tengo una vacante activa
   * **Cuando** abro la pestaña “Pipeline”
   * **Entonces** veo columnas por etapa (Aplicada, En Revisión, etc.) con conteos
2. **Mover tarjeta**

   * **Dado que** arrastro una tarjeta de “Aplicada” a “Preseleccionada”
   * **Cuando** suelto el candidato en la nueva columna
   * **Entonces** el backend actualiza el estado y notifica al candidato
3. **Reglas de negocio**

   * **Dado que** intento mover un candidato a “Contratado” sin oferta enviada
   * **Cuando** realizo el drag-&-drop
   * **Entonces** el sistema muestra un mensaje “Acción no permitida”

#### Criterios Técnicos

1. API `PATCH /candidatures/{id}/status` con validación de transición
2. Frontend usa `react-beautiful-dnd` + optimistic-UI
3. Eventos SNS “PipelineUpdated” para auditoría

#### Notas Adicionales

* Historial de movimientos en tabla `audit`

#### Historias de Usuario Relacionadas

* US-2

## 📌 Backlog de Producto

| ID   | MVP   | Historia de Usuario           | Impacto/Valor                                  | Urgencia                               | Complejidad                                            | Riesgos / Dependencias                     |
| ---- | ----- | ----------------------------- | ---------------------------------------------- | -------------------------------------- | ------------------------------------------------------ | ------------------------------------------ |
| US-1 | MVP 1 | Aplicar a Vacante desde Móvil | **Alto** – core flujo de ingreso de candidatos | **Alta** – early adopters esperan esto | Media                                                  | Dependencia: File Service, Auth listo      |
| US-2 | MVP 1 | Ver Estado de mi Candidatura  | Alto – mejora NPS temprano                     | Media                                  | Media-Alta – websockets/actualizaciones en tiempo real | Dep. US-1 (datos), Comm-Service para mails |
| US-3 | MVP 1 | Crear Vacante con Plantilla   | Alto – sin vacantes no hay pipeline            | Alta                                   | Media                                                  | Dep. User-Service (roles), Template data   |
| US-4 | MVP 1 | Pipeline Kanban Drag-&-Drop   | Alto – valor para recruiter                    | Media                                  | Alta – drag-drop + reglas                              | Dep. US-3 (vacantes), WS events            |
| ---  | MVP 2 | Email/SMS plantillas masivas  | Medio-Alto                                     | Alta (competencia)                     | Media                                                  | Requiere Comm-Service, templates           |
| ---  | MVP 2 | Multiposting LinkedIn/Indeed  | Alto – expone oferta                           | Media-Alta                             | Alta – API externas                                    | JobBoard creds; US-3 datos vacante         |
| ---  | MVP 3 | CV Parsing & Scoring          | Muy Alto – ventaja competitiva                 | Media                                  | Alta+                                                  | Modelo IA-Service, data labeling           |
| ---  | MVP 4 | Entrevistas programadas       | Medio                                          | Media                                  | Media                                                  | Google Calendar API, permisos              |
| ---  | MVP 5 | Dashboard KPIs                | Medio                                          | Baja                                   | Alta                                                   | Dep. de eventos y datos históricos         |

> Escala cualitativa: **Alta / Media / Baja**

---

## 🗂️ Tickets de Trabajo – User Story US-1 “Aplicar a Vacante”

| Ticket ID   | Puntos (Fib) | Descripción Técnica                                                                                        |
| ----------- | ------------ | ---------------------------------------------------------------------------------------------------------- |
| **LTS-A1**  | 3            | **Diseñar UI móvil “ApplyVacancy.vue/tsx”** – formulario 3 pasos + dropzone CV                             |
| **LTS-A2**  | 5            | **Endpoint `POST /candidatures` (Node.js)** – valida payload, persiste en `CANDIDATURA`, estado `aplicada` |
| **LTS-A3**  | 2            | **Presigned-URL handler (`/files/presign`)** para S3, expira 15 min                                        |
| **LTS-A4**  | 5            | **Lambda/File-Service** – recibe upload-completed webhook, guarda metadatos en DB                          |
| **LTS-A5**  | 3            | **Email Template “Confirmación de Aplicación”** en Comm-Service + trigger SNS                              |
| **LTS-A6**  | 8            | **E2E Tests Cypress** – flujo completo móvil (subida CV + confirmación)                                    |
| **LTS-A7**  | 1            | **Actualizar OpenAPI Spec** y publicar en portal de devs                                                   |
| **LTS-A8**  | 3            | **IaC (Terraform)** – policies S3 bucket privado + role `candidature-uploader`                             |
| **LTS-A9**  | 2            | **Monitoreo CloudWatch métricas** – `POST /candidatures` latency & errors                                  |
| **LTS-A10** | 1            | **Checklist de Deploy** – variables ENV, revisiones de seguridad                                           |

> *Total estimado*: **33 puntos** (≈ 1 sprint de 2 semanas para squad 5-7 devs)

---
