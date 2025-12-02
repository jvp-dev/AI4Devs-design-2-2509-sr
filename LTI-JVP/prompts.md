# Prompts Utilizados para el Diseño de LTI ATS

Este documento contiene los prompts internos que se ejecutaron para generar cada sección del documento de diseño LTI-JVP.md.

---

## Prompt 1: Descripción del Software y Ventajas Competitivas

```
Actúa como un Product Manager senior de una startup de HR Tech. Necesito que definas
el producto LTI, un ATS (Applicant Tracking System) del futuro que compita con
Greenhouse, Lever, Workday Recruiting y otros líderes del mercado.

Define:
1. Descripción concisa del software (qué es y para quién)
2. Valor añadido comparado con ATS tradicionales (tabla comparativa)
3. Las 5 ventajas competitivas principales que harán brillar a LTI

Enfócate en:
- Eficiencia para departamentos de HR
- Colaboración en tiempo real entre reclutadores y managers
- Automatizaciones inteligentes
- Asistencia de IA en diversas tareas

El tono debe ser profesional pero accesible, orientado a convencer a inversores y
early adopters del potencial disruptivo del producto.
```

---

## Prompt 2: Funciones Principales del Sistema

```
Como arquitecto de producto para un ATS de nueva generación, lista y describe
las funciones principales que debe tener el sistema LTI.

Organiza las funciones en módulos/categorías:
- Gestión de ofertas
- Pipeline de candidatos
- Screening inteligente
- Evaluación y entrevistas
- Colaboración
- Comunicación
- Analytics
- Administración

Para cada función principal, incluye 3-5 subfunciones o características específicas.
Destaca aquellas que involucren IA o automatización.
```

---

## Prompt 3: Lean Canvas

```
Genera un Lean Canvas completo para LTI, un ATS SaaS B2B con IA integrada.

El canvas debe incluir los 9 bloques:
1. Problema (3 problemas principales del mercado)
2. Segmentos de clientes (específicos y priorizados)
3. Propuesta de valor única (una frase memorable)
4. Solución (3 características principales)
5. Canales (cómo llegar a clientes)
6. Fuentes de ingresos (modelo de pricing)
7. Estructura de costes (principales partidas)
8. Métricas clave (KPIs de negocio)
9. Ventaja especial (unfair advantage)

Formatea el canvas de forma visual en ASCII art y también en formato Mermaid
para visualización en markdown.
```

---

## Prompt 4: Casos de Uso Principales

```
Diseña 3 casos de uso principales para el sistema ATS LTI que representen
los flujos más críticos del producto.

Para cada caso de uso incluye:
- Nombre descriptivo
- Descripción del escenario (2-3 párrafos)
- Actores involucrados
- Precondiciones
- Flujo principal (10-12 pasos)
- Flujos alternativos (si aplica)
- Postcondiciones

Los casos de uso deben ser:
1. Uno relacionado con publicación de ofertas y screening con IA
2. Uno relacionado con colaboración y evaluación de candidatos
3. Uno relacionado con automatización (ej: scheduling de entrevistas)

Genera también el diagrama UML de cada caso de uso en formato Mermaid.
```

---

## Prompt 5: Modelo de Datos

```
Diseña el modelo de datos completo para el ATS LTI considerando:

Entidades principales:
- Organization, User, Department
- JobPosting, PipelineStage
- Candidate, CandidateProfile, Document
- Application, ApplicationStage
- Interview, Scorecard, Evaluation
- Comment, Notification
- AIScore (para el scoring de IA)

Para cada entidad incluye:
- Nombre de atributos
- Tipo de dato (UUID, VARCHAR, TEXT, ENUM, JSONB, TIMESTAMP, etc.)
- Descripción breve del propósito

Indica las relaciones entre entidades:
- 1:1, 1:N, N:M
- Foreign keys

Genera el diagrama ER en formato Mermaid usando la sintaxis erDiagram.
Incluye también una tabla descriptiva para las 6 entidades más importantes.
```

---

## Prompt 6: Diseño de Alto Nivel

```
Diseña la arquitectura de alto nivel para LTI ATS considerando:

Requisitos:
- Escalabilidad para miles de organizaciones (multi-tenant)
- Colaboración en tiempo real (WebSockets)
- Procesamiento de IA asíncrono
- Integraciones con servicios externos (job boards, calendars, LLMs)
- Alta disponibilidad y compliance (GDPR)

Define:
1. Tipo de arquitectura (monolito, microservicios, serverless, híbrida)
2. Lista de componentes/servicios principales con su responsabilidad
3. Stack tecnológico recomendado para cada componente
4. Patrones arquitectónicos utilizados (event-driven, CQRS, etc.)
5. Flujos de datos principales

Genera un diagrama de arquitectura en Mermaid usando flowchart TB.
Organiza visualmente por capas: Clients, Edge, Core Services, AI Layer,
Supporting Services, Data Layer, Messaging.
```

---

## Prompt 7: Diagrama C4 del AI Service

```
Genera el diagrama C4 completo (4 niveles) para el componente AI Service de LTI.

El AI Service es responsable de:
- Parsing de CVs y extracción de datos estructurados
- Scoring de candidatos (match con ofertas)
- Generación de contenido (descripciones, emails)
- Embeddings para búsqueda semántica
- Análisis de sentimiento

Para cada nivel C4:

Nivel 1 (Contexto):
- Sistema LTI y sus actores externos
- Sistemas externos con los que interactúa

Nivel 2 (Contenedores):
- Principales contenedores del sistema
- Bases de datos, message brokers, etc.
- Resalta el AI Service

Nivel 3 (Componentes):
- Componentes internos del AI Service
- API, consumers, orquestadores
- Componentes de IA específicos (parser, scorer, generator)
- Conexiones con LLM, vector DB, cache

Nivel 4 (Código):
- Diseño de clases para el componente Scorer
- Clases principales, métodos y relaciones
- Objetos de dominio (AIScore, SkillMatch, etc.)

Genera cada nivel en formato Mermaid (C4Context, C4Container, C4Component, classDiagram).
Incluye tabla descriptiva de componentes del AI Service.
```

---

## Prompt 8: Consideraciones Técnicas

```
Lista las consideraciones técnicas adicionales para el ATS LTI en las áreas de:

1. Seguridad
   - Autenticación y autorización
   - Encriptación
   - Compliance

2. Escalabilidad
   - Estrategias de scaling
   - Manejo de datos
   - Caching

3. Observabilidad
   - Logging
   - Métricas
   - Tracing
   - Alerting

4. Disaster Recovery
   - RTO/RPO
   - Backups
   - Alta disponibilidad

Sé conciso, usa bullet points y menciona tecnologías específicas recomendadas.
```

---

## Prompt 9: Roadmap de Desarrollo

```
Define un roadmap de desarrollo de alto nivel para LTI ATS dividido en versiones:

- MVP (v1.0): Funcionalidad mínima para primeros clientes
- v1.5: Mejoras de colaboración
- v2.0: IA avanzada
- v2.5: Features enterprise

Para cada versión, lista 4-5 features principales sin estimaciones de tiempo.
```

---

## Notas sobre el Proceso

### Enfoque de Diseño

1. **Investigación previa**: Se consideraron las características de ATS líderes del mercado (Greenhouse, Lever, Workable, Ashby) para identificar gaps y oportunidades de diferenciación.

2. **User-centric**: El diseño prioriza la experiencia tanto del recruiter como del candidato, reconociendo que ambos son "usuarios" del sistema.

3. **AI-native**: A diferencia de añadir IA como feature adicional, se diseñó el sistema con IA como componente central desde el inicio.

4. **Escalabilidad**: La arquitectura de microservicios permite escalar componentes independientemente según demanda.

5. **Extensibilidad**: El diseño API-first y event-driven facilita integraciones futuras sin modificar el core.

### Herramientas Utilizadas

- **Diagramas**: Mermaid para compatibilidad con Markdown y GitHub
- **Notación**: UML para casos de uso, ER para modelo de datos, C4 para arquitectura
- **Formato**: Markdown para documentación técnica legible
