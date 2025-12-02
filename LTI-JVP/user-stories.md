# User Stories - LTI ATS

## Plantilla Utilizada

Todas las User Stories siguen esta plantilla estándar:

```
## US-[ID]: [Título]

**Como** [rol del usuario]
**Quiero** [acción/funcionalidad deseada]
**Para** [beneficio/valor que obtiene]

### Descripción
[Contexto adicional y detalles de la historia]

### Criterios de Aceptación
- [ ] [Criterio verificable 1]
- [ ] [Criterio verificable 2]
- [ ] [Criterio verificable N]

### Notas Técnicas
[Consideraciones de implementación, dependencias, etc.]

### Definición de Done (DoD)
- [ ] Código implementado y revisado (code review)
- [ ] Tests unitarios escritos y pasando (>80% cobertura)
- [ ] Tests de integración pasando
- [ ] Documentación actualizada
- [ ] Desplegado en ambiente de staging
- [ ] Aprobación del Product Owner

### Mockups/Wireframes
[Referencias visuales si aplica]

### Dependencias
[Otras US o componentes necesarios]

### Prioridad: [Alta/Media/Baja]
### Estimación: [Puntos de historia]
```

---

## User Stories del Sistema LTI

---

## US-001: Creación de Ofertas de Empleo con Asistencia de IA

**Como** Recruiter
**Quiero** crear ofertas de empleo con asistencia de inteligencia artificial
**Para** generar descripciones optimizadas en menos tiempo y con mejor calidad

### Descripción
El recruiter necesita poder crear nuevas ofertas de empleo de manera eficiente. El sistema debe proporcionar asistencia de IA que genere automáticamente la descripción del puesto, requisitos y beneficios basándose en el título del puesto y algunos parámetros básicos. El recruiter debe poder editar y personalizar el contenido generado antes de publicar.

### Criterios de Aceptación
- [ ] El usuario puede acceder al formulario de creación de oferta desde el dashboard principal
- [ ] Al ingresar el título del puesto, el sistema sugiere una categoría/departamento
- [ ] Existe un botón "Generar con IA" que activa la generación automática
- [ ] La IA genera: descripción del puesto, requisitos (mínimos y deseables), beneficios
- [ ] El contenido generado se muestra en campos editables
- [ ] El usuario puede regenerar secciones individuales si no está satisfecho
- [ ] El sistema guarda borradores automáticamente cada 30 segundos
- [ ] La oferta puede guardarse como borrador o publicarse directamente
- [ ] El tiempo de generación de IA no excede 10 segundos
- [ ] Se muestra indicador de carga durante la generación

### Notas Técnicas
- Integración con AI Service (Content Generator component)
- Uso de LangChain para orquestación de prompts
- Almacenamiento en PostgreSQL (tabla `job_posting`)
- Endpoint: `POST /api/v1/jobs` y `POST /api/v1/jobs/{id}/generate`
- Cache de respuestas frecuentes en Redis para optimizar costos LLM

### Definición de Done (DoD)
- [ ] Código implementado y revisado (code review)
- [ ] Tests unitarios escritos y pasando (>80% cobertura)
- [ ] Tests de integración pasando
- [ ] Documentación de API actualizada (OpenAPI)
- [ ] Desplegado en ambiente de staging
- [ ] Aprobación del Product Owner
- [ ] Métricas de uso de IA configuradas en Prometheus

### Mockups/Wireframes
- Wireframe del formulario de creación con botón de IA
- Estado de loading durante generación
- Vista de edición post-generación

### Dependencias
- Infrastructure: AI Service desplegado
- Data: Prompts de generación configurados en Prompt Manager
- Auth: Sistema de autenticación funcional

### Prioridad: Alta
### Estimación: 13 puntos de historia

---

## US-002: Visualización de Pipeline de Candidatos en Kanban

**Como** Recruiter
**Quiero** visualizar y gestionar candidatos en un tablero Kanban
**Para** tener una vista clara del estado de cada candidato y moverlos entre etapas fácilmente

### Descripción
El sistema debe proporcionar una vista tipo Kanban donde el recruiter pueda ver todos los candidatos de una oferta organizados por etapas del pipeline (Applied, Screening, Interview, Offer, Hired/Rejected). Los candidatos deben poder moverse entre columnas mediante drag & drop, y cada tarjeta debe mostrar información resumida del candidato incluyendo su AI Score.

### Criterios de Aceptación
- [ ] El tablero Kanban muestra todas las etapas del pipeline configuradas para la oferta
- [ ] Cada candidato se representa como una tarjeta con: nombre, foto, AI Score, fecha de aplicación
- [ ] El AI Score se muestra con código de colores (verde >70, amarillo 40-70, rojo <40)
- [ ] Las tarjetas pueden arrastrarse y soltarse entre columnas
- [ ] Al mover una tarjeta, se actualiza el estado en tiempo real
- [ ] Se muestra un contador de candidatos por columna
- [ ] Existe filtro por AI Score (rango), fecha de aplicación y fuente
- [ ] El tablero se actualiza en tiempo real cuando otro usuario hace cambios
- [ ] Se puede cambiar entre vista Kanban, Lista y Tabla
- [ ] Al hacer clic en una tarjeta, se abre el perfil detallado del candidato

### Notas Técnicas
- Frontend: React con react-beautiful-dnd para drag & drop
- WebSocket (Socket.io) para actualizaciones en tiempo real
- Estado optimista para mejor UX al mover tarjetas
- Endpoint: `GET /api/v1/jobs/{id}/pipeline`, `PATCH /api/v1/applications/{id}/stage`
- Collaboration Service para sincronización entre usuarios

### Definición de Done (DoD)
- [ ] Código implementado y revisado (code review)
- [ ] Tests unitarios escritos y pasando (>80% cobertura)
- [ ] Tests E2E para flujo de drag & drop
- [ ] Performance: renderizado <100ms para 100 candidatos
- [ ] Documentación actualizada
- [ ] Desplegado en ambiente de staging
- [ ] Aprobación del Product Owner

### Mockups/Wireframes
- Diseño del tablero Kanban con columnas
- Diseño de tarjeta de candidato
- Estados de hover y dragging

### Dependencias
- US-005: Sistema de scoring de candidatos
- US-003: Gestión básica de candidatos
- Collaboration Service operativo

### Prioridad: Alta
### Estimación: 8 puntos de historia

---

## US-003: Aplicación de Candidatos y Parsing de CV con IA

**Como** Candidato
**Quiero** aplicar a una oferta subiendo mi CV
**Para** que mis datos sean procesados automáticamente sin tener que llenar formularios extensos

### Descripción
Los candidatos deben poder aplicar a ofertas de empleo subiendo su CV. El sistema utilizará IA para extraer automáticamente la información relevante (datos de contacto, experiencia laboral, educación, habilidades) y completar el perfil del candidato. Esto reduce la fricción en el proceso de aplicación y mejora la experiencia del candidato.

### Criterios de Aceptación
- [ ] El candidato puede acceder a la página de aplicación desde la oferta pública
- [ ] Puede subir CV en formatos: PDF, DOCX, DOC (máx. 5MB)
- [ ] El sistema muestra barra de progreso durante el procesamiento
- [ ] El CV es parseado y se extraen: nombre, email, teléfono, LinkedIn, experiencia, educación, habilidades
- [ ] Los datos extraídos se muestran al candidato para confirmación/edición
- [ ] El candidato puede corregir datos incorrectos antes de enviar
- [ ] Se solicita consentimiento GDPR explícito antes de completar la aplicación
- [ ] El candidato recibe email de confirmación al completar la aplicación
- [ ] Si el parsing falla, se muestra formulario manual como fallback
- [ ] El tiempo de parsing no excede 15 segundos

### Notas Técnicas
- AI Service: CV Parser component (spaCy + LLM)
- Almacenamiento de documentos en S3 con cifrado
- Endpoint: `POST /api/v1/public/applications`
- Generación de embeddings para búsqueda semántica posterior
- Evento Kafka `cv.uploaded` para procesamiento asíncrono
- Tabla `candidate_profile` para datos estructurados

### Definición de Done (DoD)
- [ ] Código implementado y revisado (code review)
- [ ] Tests unitarios escritos y pasando (>80% cobertura)
- [ ] Tests de integración con AI Service
- [ ] Validación de seguridad (sanitización de archivos)
- [ ] Documentación actualizada
- [ ] Desplegado en ambiente de staging
- [ ] Aprobación del Product Owner

### Mockups/Wireframes
- Página de aplicación con upload de CV
- Vista de confirmación de datos extraídos
- Email de confirmación

### Dependencias
- AI Service: CV Parser operativo
- Storage: S3 bucket configurado
- Email Service: Templates configurados

### Prioridad: Alta
### Estimación: 13 puntos de historia

---

## US-004: Programación Automática de Entrevistas

**Como** Recruiter
**Quiero** que el sistema coordine automáticamente la programación de entrevistas
**Para** eliminar el trabajo manual de encontrar horarios compatibles entre candidato y entrevistadores

### Descripción
El sistema debe integrar los calendarios de los entrevistadores (Google Calendar, Outlook) y proponer automáticamente horarios disponibles al candidato. El candidato selecciona su horario preferido y el sistema crea el evento en todos los calendarios, envía confirmaciones y configura recordatorios automáticos.

### Criterios de Aceptación
- [ ] El recruiter puede seleccionar candidato y tipo de entrevista (phone, video, onsite)
- [ ] El recruiter asigna entrevistadores al proceso
- [ ] El sistema consulta disponibilidad de calendarios conectados
- [ ] Se calculan slots óptimos considerando zonas horarias de todos los participantes
- [ ] Se presentan al recruiter los 5 mejores slots disponibles
- [ ] El recruiter aprueba el envío de propuesta al candidato
- [ ] El candidato recibe link con opciones de horario
- [ ] El candidato puede seleccionar su preferencia
- [ ] Al confirmar, se crean eventos en todos los calendarios
- [ ] Se envían confirmaciones con link de videoconferencia (Zoom/Teams/Meet)
- [ ] Se programan recordatorios automáticos (24h y 1h antes)
- [ ] Si el candidato no responde en 48h, se envía reminder automático

### Notas Técnicas
- Scheduling Service con Temporal para workflows
- Integración OAuth con Google Calendar y Microsoft Graph API
- Endpoint: `POST /api/v1/interviews/schedule`
- Manejo de zonas horarias con moment-timezone
- Generación de links de videoconferencia vía API (Zoom/Teams)
- Eventos Kafka para notificaciones

### Definición de Done (DoD)
- [ ] Código implementado y revisado (code review)
- [ ] Tests unitarios escritos y pasando (>80% cobertura)
- [ ] Tests de integración con APIs de calendario
- [ ] Manejo de errores para calendarios desconectados
- [ ] Documentación actualizada
- [ ] Desplegado en ambiente de staging
- [ ] Aprobación del Product Owner

### Mockups/Wireframes
- Modal de programación de entrevista
- Email de propuesta al candidato
- Página de selección de horario para candidato
- Evento de calendario creado

### Dependencias
- Integraciones: Google Calendar OAuth, Microsoft Graph API
- Notification Service operativo
- Configuración de Zoom/Teams/Meet API

### Prioridad: Alta
### Estimación: 21 puntos de historia

---

## US-005: Scoring Automático de Candidatos con IA

**Como** Recruiter
**Quiero** que el sistema calcule automáticamente un score de match para cada candidato
**Para** priorizar mi tiempo en los candidatos más relevantes para el puesto

### Descripción
Cuando un candidato aplica a una oferta, el sistema debe calcular automáticamente un score de 0-100 que represente qué tan bien encaja el candidato con los requisitos del puesto. El score debe considerar skills, experiencia, educación y fit cultural. El recruiter debe poder ver el desglose del score y entender por qué el candidato recibió esa puntuación.

### Criterios de Aceptación
- [ ] El score se calcula automáticamente tras el parsing del CV
- [ ] El score general es un número de 0-100
- [ ] Se muestra desglose en 4 categorías: Skills Match, Experience Match, Education Match, Cultural Fit
- [ ] Cada categoría tiene score individual y explicación en texto
- [ ] Se listan skills coincidentes, faltantes y adicionales
- [ ] El score se actualiza si se modifica el CV o los requisitos del puesto
- [ ] Los candidatos se pueden ordenar por score en cualquier vista
- [ ] El cálculo de score no excede 30 segundos tras la aplicación
- [ ] Se genera un resumen ejecutivo de 2-3 oraciones sobre el candidato
- [ ] El recruiter puede dar feedback sobre la precisión del score (👍/👎)

### Notas Técnicas
- AI Service: Candidate Scorer component
- Uso de embeddings (OpenAI/Cohere) para matching semántico
- Vector DB (Pinecone) para búsqueda de similitud
- Almacenamiento en tabla `ai_score`
- Evento Kafka `score.calculated` para notificaciones
- Endpoint: `GET /api/v1/applications/{id}/score`
- Métricas de feedback para mejora continua del modelo

### Definición de Done (DoD)
- [ ] Código implementado y revisado (code review)
- [ ] Tests unitarios escritos y pasando (>80% cobertura)
- [ ] Tests de integración con AI Service
- [ ] Benchmarks de precisión vs evaluación manual
- [ ] Documentación actualizada
- [ ] Desplegado en ambiente de staging
- [ ] Aprobación del Product Owner

### Mockups/Wireframes
- Tarjeta de score en perfil de candidato
- Vista expandida con desglose de categorías
- Iconografía para skills match/missing

### Dependencias
- US-003: Parsing de CV implementado
- AI Service: Embeddings y Vector DB operativos
- Prompts de scoring configurados

### Prioridad: Alta
### Estimación: 21 puntos de historia

---

## US-006: Evaluación Colaborativa con Scorecards

**Como** Hiring Manager
**Quiero** completar scorecards de evaluación estructuradas después de entrevistar candidatos
**Para** proporcionar feedback consistente y facilitar la comparación entre candidatos

### Descripción
Después de cada entrevista, los evaluadores deben poder completar una scorecard con criterios predefinidos. Cada criterio tiene una escala de puntuación y espacio para notas. Las scorecards permiten evaluar de manera consistente y generar una puntuación agregada para comparar candidatos.

### Criterios de Aceptación
- [ ] Existe un catálogo de scorecards configurables por tipo de entrevista
- [ ] Cada scorecard tiene criterios con nombre, descripción y peso
- [ ] Los criterios se puntúan en escala 1-5 con etiquetas descriptivas
- [ ] Cada criterio tiene campo opcional para notas
- [ ] El evaluador puede añadir una recomendación final: Strong Hire, Hire, No Hire, Strong No Hire
- [ ] Se puede añadir un comentario general sobre la entrevista
- [ ] La scorecard se guarda automáticamente (autosave)
- [ ] Al completar, se notifica al recruiter responsable
- [ ] El recruiter puede ver todas las scorecards de un candidato en un panel consolidado
- [ ] Se calcula score promedio ponderado automáticamente

### Notas Técnicas
- Tablas: `scorecard`, `scorecard_criteria`, `evaluation`, `evaluation_score`
- Endpoint: `POST /api/v1/evaluations`, `GET /api/v1/applications/{id}/evaluations`
- WebSocket para notificaciones en tiempo real
- Cálculo de promedio ponderado en el frontend y backend

### Definición de Done (DoD)
- [ ] Código implementado y revisado (code review)
- [ ] Tests unitarios escritos y pasando (>80% cobertura)
- [ ] Tests de integración
- [ ] Documentación actualizada
- [ ] Desplegado en ambiente de staging
- [ ] Aprobación del Product Owner

### Mockups/Wireframes
- Formulario de scorecard
- Vista de panel consolidado de evaluaciones
- Comparativa visual entre candidatos

### Dependencias
- US-004: Sistema de entrevistas
- Notification Service operativo

### Prioridad: Media
### Estimación: 13 puntos de historia

---

## US-007: Colaboración en Tiempo Real en Perfiles de Candidatos

**Como** Miembro del equipo de contratación
**Quiero** ver en tiempo real quién está viendo el perfil de un candidato y sus comentarios
**Para** colaborar eficientemente y evitar duplicar esfuerzos o tomar decisiones desinformadas

### Descripción
Inspirado en herramientas como Figma y Notion, el sistema debe mostrar presencia en tiempo real (avatares de quién está viendo el mismo perfil), comentarios contextuales con menciones, y actualizaciones instantáneas sin necesidad de refrescar la página.

### Criterios de Aceptación
- [ ] Al abrir un perfil de candidato, se muestran avatares de otros usuarios viéndolo
- [ ] Los avatares muestran nombre al hacer hover
- [ ] Se pueden añadir comentarios en cualquier sección del perfil
- [ ] Los comentarios soportan @menciones a otros usuarios del equipo
- [ ] Las menciones generan notificaciones al usuario mencionado
- [ ] Los comentarios aparecen en tiempo real sin refrescar
- [ ] Se puede responder a comentarios (threads)
- [ ] Existe un feed de actividad con el historial de acciones
- [ ] Las acciones (mover de etapa, añadir evaluación) se reflejan instantáneamente
- [ ] El sistema funciona correctamente con hasta 10 usuarios simultáneos en el mismo perfil

### Notas Técnicas
- Collaboration Service con Socket.io
- Redis para estado de presencia
- Tabla `comment` con soporte para threads (parent_id)
- Eventos: user.joined, user.left, comment.added, action.performed
- Endpoint: `POST /api/v1/applications/{id}/comments`

### Definición de Done (DoD)
- [ ] Código implementado y revisado (code review)
- [ ] Tests unitarios escritos y pasando (>80% cobertura)
- [ ] Tests de integración con WebSocket
- [ ] Load testing con 10 usuarios simultáneos
- [ ] Documentación actualizada
- [ ] Desplegado en ambiente de staging
- [ ] Aprobación del Product Owner

### Mockups/Wireframes
- Indicadores de presencia en perfil
- Componente de comentarios
- Notificación de mención

### Dependencias
- Collaboration Service desplegado
- Notification Service operativo
- WebSocket infrastructure

### Prioridad: Media
### Estimación: 21 puntos de historia

---

## US-008: Panel de Decisión Colaborativa

**Como** Hiring Manager
**Quiero** facilitar una sesión de decisión estructurada con todo el equipo
**Para** tomar decisiones de contratación informadas y documentadas basadas en consenso

### Descripción
Cuando un candidato ha completado todas las entrevistas, el sistema debe proporcionar un panel de decisión donde todos los evaluadores puedan votar simultáneamente, ver un resumen de todas las evaluaciones, discutir puntos importantes y llegar a una decisión final documentada.

### Criterios de Aceptación
- [ ] El Hiring Manager puede iniciar una sesión de decisión para un candidato
- [ ] Se invita automáticamente a todos los que han evaluado al candidato
- [ ] El panel muestra resumen de: AI Score, scores de cada evaluador, recomendaciones
- [ ] Se presenta comparativa visual con otros candidatos finalistas (si aplica)
- [ ] Cada participante puede votar: Hire / No Hire / Need More Info
- [ ] Los votos se muestran en tiempo real (anónimos hasta que todos voten, opcionalmente)
- [ ] Existe chat integrado para discusión
- [ ] El Hiring Manager puede registrar la decisión final y justificación
- [ ] La decisión se guarda en el historial del candidato
- [ ] Se generan automáticamente las notificaciones/emails según la decisión

### Notas Técnicas
- Collaboration Service para sincronización en tiempo real
- Nuevo endpoint: `POST /api/v1/applications/{id}/decision-session`
- WebSocket events para votos y chat
- Almacenamiento de decisión en tabla `application` (status) y nuevo registro en audit

### Definición de Done (DoD)
- [ ] Código implementado y revisado (code review)
- [ ] Tests unitarios escritos y pasando (>80% cobertura)
- [ ] Tests de integración
- [ ] Documentación actualizada
- [ ] Desplegado en ambiente de staging
- [ ] Aprobación del Product Owner

### Mockups/Wireframes
- Panel de decisión con resumen de evaluaciones
- Sistema de votación
- Chat integrado
- Registro de decisión final

### Dependencias
- US-006: Scorecards implementadas
- US-007: Colaboración en tiempo real
- Notification Service operativo

### Prioridad: Media
### Estimación: 13 puntos de historia

---

## US-009: Multi-posting Automático en Job Boards

**Como** Recruiter
**Quiero** publicar una oferta en múltiples job boards con un solo clic
**Para** maximizar el alcance sin tener que publicar manualmente en cada plataforma

### Descripción
Una vez creada una oferta, el recruiter debe poder seleccionar en qué job boards publicarla (LinkedIn, Indeed, Glassdoor, InfoJobs, etc.) y el sistema debe encargarse de publicarla automáticamente en todas las plataformas seleccionadas, rastreando el rendimiento de cada canal.

### Criterios de Aceptación
- [ ] Al publicar una oferta, se muestra lista de job boards disponibles (mínimo 10)
- [ ] Cada job board muestra: logo, costo (si aplica), tiempo estimado de publicación
- [ ] El recruiter puede seleccionar múltiples job boards
- [ ] El sistema adapta el formato de la oferta a los requisitos de cada plataforma
- [ ] Se muestra progreso de publicación en cada canal
- [ ] El estado de publicación se actualiza en tiempo real (pending, published, failed)
- [ ] Si falla en algún canal, se muestra error específico y opción de reintentar
- [ ] Se trackea la fuente de cada aplicación recibida
- [ ] El recruiter puede ver métricas de rendimiento por canal (aplicaciones, views)
- [ ] Se puede despublicar de todos los canales con un clic

### Notas Técnicas
- Integration Service con conectores a APIs de job boards
- Uso de n8n o similar para workflows de integración
- Tabla `job_posting_channel` para trackeo
- Endpoint: `POST /api/v1/jobs/{id}/publish`
- Eventos Kafka para procesamiento asíncrono

### Definición de Done (DoD)
- [ ] Código implementado y revisado (code review)
- [ ] Tests unitarios escritos y pasando (>80% cobertura)
- [ ] Tests de integración con al menos 3 job boards
- [ ] Documentación de integraciones
- [ ] Desplegado en ambiente de staging
- [ ] Aprobación del Product Owner

### Mockups/Wireframes
- Selector de job boards con checkboxes
- Estado de publicación por canal
- Dashboard de métricas por canal

### Dependencias
- Integration Service desplegado
- Credenciales API de job boards configuradas
- Analytics Service para métricas

### Prioridad: Media
### Estimación: 21 puntos de historia

---

## US-010: Dashboard de Analytics en Tiempo Real

**Como** Head of Talent Acquisition
**Quiero** ver un dashboard con métricas clave de recruiting en tiempo real
**Para** tomar decisiones informadas y optimizar los procesos de contratación

### Descripción
El sistema debe proporcionar un dashboard ejecutivo con KPIs de recruiting incluyendo: time-to-hire, candidatos por etapa, fuentes más efectivas, ratio de conversión del pipeline, y comparativas históricas. Las métricas deben actualizarse en tiempo real.

### Criterios de Aceptación
- [ ] El dashboard muestra métricas globales y puede filtrarse por: departamento, recruiter, período
- [ ] Se visualiza: Time-to-hire promedio (con tendencia), candidatos activos por etapa, ofertas abiertas
- [ ] Gráfico de funnel con ratios de conversión entre etapas
- [ ] Top 5 fuentes de candidatos por efectividad (aplicaciones → contrataciones)
- [ ] Comparativa mes actual vs mes anterior
- [ ] Los datos se actualizan cada 5 minutos automáticamente
- [ ] Se pueden exportar reportes a PDF/Excel
- [ ] El usuario puede personalizar qué widgets ver
- [ ] Existe versión móvil responsive del dashboard
- [ ] Se pueden configurar alertas para umbrales (ej: time-to-hire > 30 días)

### Notas Técnicas
- Analytics Service con Apache Spark para agregaciones
- Materialización de métricas en PostgreSQL (CQRS read model)
- Frontend: Chart.js o Recharts para visualizaciones
- Endpoint: `GET /api/v1/analytics/dashboard`
- WebSocket para actualizaciones incrementales
- Export: generación de PDF con Puppeteer

### Definición de Done (DoD)
- [ ] Código implementado y revisado (code review)
- [ ] Tests unitarios escritos y pasando (>80% cobertura)
- [ ] Performance: carga inicial < 3 segundos
- [ ] Documentación actualizada
- [ ] Desplegado en ambiente de staging
- [ ] Aprobación del Product Owner

### Mockups/Wireframes
- Layout del dashboard con widgets
- Gráficos de funnel y tendencias
- Versión móvil

### Dependencias
- Datos históricos disponibles
- Analytics Service desplegado
- Todas las US anteriores para generar datos

### Prioridad: Baja
### Estimación: 21 puntos de historia

---

## Resumen de User Stories

| ID | Título | Prioridad | Estimación |
|----|--------|-----------|------------|
| US-001 | Creación de Ofertas con IA | Alta | 13 pts |
| US-002 | Pipeline Kanban de Candidatos | Alta | 8 pts |
| US-003 | Aplicación y Parsing de CV con IA | Alta | 13 pts |
| US-004 | Programación Automática de Entrevistas | Alta | 21 pts |
| US-005 | Scoring Automático de Candidatos | Alta | 21 pts |
| US-006 | Evaluación con Scorecards | Media | 13 pts |
| US-007 | Colaboración en Tiempo Real | Media | 21 pts |
| US-008 | Panel de Decisión Colaborativa | Media | 13 pts |
| US-009 | Multi-posting en Job Boards | Media | 21 pts |
| US-010 | Dashboard de Analytics | Baja | 21 pts |

**Total: 165 puntos de historia**
