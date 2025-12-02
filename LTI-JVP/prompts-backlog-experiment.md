# Experimento de Prompts para Generación de Backlog

Este documento detalla los diferentes prompts utilizados para generar el Product Backlog de LTI a partir de la documentación de diseño, junto con el análisis de resultados y conclusiones.

---

## Prompt 1: Enfoque Directo Simple

```
Basándote en el documento de diseño de LTI (un ATS con IA), genera un Product Backlog
con las User Stories priorizadas.
```

### Resultado
- Generó una lista básica de funcionalidades sin formato de User Story
- No incluyó criterios de aceptación
- Priorización vaga ("Alta", "Media", "Baja") sin metodología
- Sin estimaciones
- Sin dependencias entre historias

### Evaluación: ⭐ (1/5)
**Problema**: Demasiado genérico, no aprovecha el contexto del documento de diseño.

---

## Prompt 2: Enfoque con Rol + Estructura Explícita

```
Actúa como Product Owner de un equipo Scrum. Basándote en el siguiente documento de
diseño de un ATS llamado LTI, genera User Stories para un Product Backlog.

Para cada User Story incluye:
- Título descriptivo
- Formato "Como [rol] quiero [funcionalidad] para [beneficio]"
- Criterios de aceptación (al menos 5)
- Estimación en puntos de historia
- Prioridad

Genera al menos 10 User Stories.

[Pegar documento de diseño]
```

### Resultado
- Formato correcto de User Stories
- Criterios de aceptación básicos pero funcionales
- Estimaciones presentes pero sin justificación
- Priorización sin metodología clara
- No consideró dependencias técnicas
- No relacionó las historias con el roadmap del documento

### Evaluación: ⭐⭐⭐ (3/5)
**Mejora**: La estructura explícita mejoró significativamente el output.
**Problema**: Falta conexión con la arquitectura técnica del documento.

---

## Prompt 3: Enfoque Multi-paso con Contexto Técnico

```
Eres un Product Owner senior con experiencia en productos HR Tech. Vas a generar
el Product Backlog para LTI, un ATS de nueva generación.

PASO 1: Primero, analiza el documento de diseño adjunto identificando:
- Casos de uso principales
- Componentes técnicos clave (AI Service, Collaboration Service, etc.)
- Roadmap propuesto (MVP, v1.5, v2.0)
- Actores del sistema

PASO 2: Para cada funcionalidad identificada, crea una User Story siguiendo
esta plantilla exacta:

---
## US-XXX: [Título]

**Como** [rol del sistema: Recruiter/Hiring Manager/Candidato/Admin]
**Quiero** [funcionalidad específica]
**Para** [beneficio medible]

### Descripción
[2-3 párrafos de contexto]

### Criterios de Aceptación
- [ ] [Criterio específico y verificable]
(mínimo 8 criterios)

### Notas Técnicas
[Referencias a componentes del documento: AI Service, tablas DB, endpoints]

### Dependencias
[Otras US necesarias]

### Prioridad: [Alta/Media/Baja]
### Estimación: [Fibonacci: 1,2,3,5,8,13,21] puntos
---

PASO 3: Organiza las User Stories en un backlog priorizado usando la metodología
MoSCoW, justificando cada clasificación.

[Documento de diseño adjunto]
```

### Resultado
- Excelente estructura y formato
- User Stories alineadas con el roadmap del documento
- Referencias técnicas precisas (tablas, servicios, endpoints)
- Dependencias bien identificadas
- MoSCoW aplicado correctamente
- Conexión clara entre US y arquitectura

### Evaluación: ⭐⭐⭐⭐⭐ (5/5)
**Éxito**: El enfoque multi-paso forzó análisis previo antes de generar contenido.

---

## Prompt 4: Enfoque con WSJF + Matrix

```
Como Product Owner, genera un backlog priorizado para LTI ATS usando la metodología
WSJF (Weighted Shortest Job First).

Para cada User Story calcula:
- Business Value (1-10)
- User Value (1-10)
- Risk Reduction (1-10)
- Effort (puntos de historia)
- WSJF Score = (BV + UV + RR) / Effort

Presenta el backlog ordenado por WSJF Score descendente.

[Documento de diseño]
```

### Resultado
- Priorización matemáticamente sólida
- Justificaciones claras para cada valor
- Fácil de explicar a stakeholders
- Sin embargo, faltó contexto de User Stories completas
- Se enfocó demasiado en la priorización y poco en el contenido

### Evaluación: ⭐⭐⭐⭐ (4/5)
**Fortaleza**: Metodología de priorización robusta.
**Debilidad**: User Stories menos detalladas.

---

## Prompt 5 (GANADOR): Enfoque Combinado

```
Eres un Product Owner senior con 10 años de experiencia en HR Tech, específicamente
en ATS (Applicant Tracking Systems). Tu tarea es crear la documentación completa
de Product Backlog para LTI.

CONTEXTO:
- LTI es un ATS de nueva generación con IA integrada
- Compite con Greenhouse, Lever, Workable
- Diferenciadores: IA nativa, colaboración real-time, UX moderna
- Arquitectura: microservicios, event-driven, multi-tenant

ENTREGABLES REQUERIDOS:

1. USER STORIES (mínimo 10)
   Usa esta plantilla para CADA historia:

   ## US-XXX: [Título descriptivo]

   **Como** [Recruiter | Hiring Manager | Candidato | Admin]
   **Quiero** [acción específica]
   **Para** [beneficio cuantificable cuando sea posible]

   ### Descripción
   [Contexto de 2-3 párrafos explicando el problema y la solución]

   ### Criterios de Aceptación
   - [ ] [Criterio 1 - específico, medible, verificable]
   - [ ] [Criterio 2]
   ... (mínimo 8, máximo 12)

   ### Notas Técnicas
   - Servicio responsable: [Job Service | AI Service | etc.]
   - Tablas afectadas: [nombre de tablas del modelo de datos]
   - Endpoints: [método + ruta]
   - Eventos: [eventos Kafka si aplica]

   ### Definición de Done
   - [ ] Code review aprobado
   - [ ] Tests >80% cobertura
   - [ ] Documentación actualizada
   - [ ] Deploy en staging
   - [ ] PO sign-off

   ### Dependencias
   [Lista de US-XXX que deben completarse antes]

   ### Prioridad: [Alta | Media | Baja]
   ### Estimación: [Fibonacci] puntos de historia

2. BACKLOG PRIORIZADO
   - Aplica metodología MoSCoW (Must/Should/Could/Won't)
   - Dentro de cada categoría, ordena por WSJF
   - Incluye tabla con: Business Value, User Value, Risk Reduction, Effort, WSJF
   - Agrupa por releases según el roadmap del documento (MVP, v1.5, v2.0)

3. VISUALIZACIÓN
   - Diagrama ASCII del backlog
   - Diagrama mermaid de dependencias entre US

4. MÉTRICAS
   - Estimación de velocidad del equipo
   - Proyección de sprints por release

DOCUMENTO DE DISEÑO A ANALIZAR:
[Contenido completo del documento LTI-JVP.md]
```

### Resultado
- Documentación completa y profesional
- User Stories con todos los campos necesarios
- Conexión directa con arquitectura técnica del documento
- Metodología de priorización híbrida (MoSCoW + WSJF)
- Visualizaciones útiles
- Métricas de planificación incluidas
- Dependencias claras con diagrama mermaid

### Evaluación: ⭐⭐⭐⭐⭐ (5/5)
**Mejor resultado global**

---

## Análisis Comparativo

| Aspecto | P1 | P2 | P3 | P4 | P5 |
|---------|----|----|----|----|-----|
| Formato User Stories | ❌ | ✅ | ✅ | 🟡 | ✅ |
| Criterios de Aceptación | ❌ | 🟡 | ✅ | ❌ | ✅ |
| Notas Técnicas | ❌ | ❌ | ✅ | ❌ | ✅ |
| Dependencias | ❌ | ❌ | ✅ | ❌ | ✅ |
| Metodología Priorización | ❌ | ❌ | ✅ | ✅ | ✅ |
| Visualización | ❌ | ❌ | 🟡 | ❌ | ✅ |
| Conexión con Roadmap | ❌ | ❌ | ✅ | 🟡 | ✅ |
| Estimaciones Justificadas | ❌ | ❌ | 🟡 | ✅ | ✅ |

**Leyenda**: ✅ Cumple | 🟡 Parcial | ❌ No cumple

---

## Conclusiones

### ¿Por qué el Prompt 5 fue el más efectivo?

1. **Contexto Rico**: Establecer el contexto de HR Tech y competencia ayudó a generar contenido más relevante y realista.

2. **Rol Específico con Experiencia**: "Product Owner senior con 10 años de experiencia" produce respuestas más maduras que simplemente "Product Owner".

3. **Estructura de Plantilla Explícita**: Proporcionar la plantilla exacta con todos los campos elimina la ambigüedad sobre qué incluir.

4. **Entregables Numerados**: Dividir la solicitud en entregables claros (1. User Stories, 2. Backlog, 3. Visualización, 4. Métricas) asegura que ningún aspecto se omita.

5. **Metodología Especificada**: Indicar explícitamente MoSCoW + WSJF evita priorizaciones arbitrarias.

6. **Conexión con Documento**: Mencionar que el output debe conectarse con la arquitectura (servicios, tablas, endpoints) del documento fuente produce resultados técnicamente sólidos.

7. **Ejemplos de Valores**: Proporcionar rangos (ej: "Business Value 1-10", "Fibonacci para estimaciones") estandariza el output.

### Patrones Identificados para Prompts Efectivos de Backlog

```
ESTRUCTURA DE PROMPT EFECTIVO:

1. IDENTIDAD
   "Eres [rol específico] con [años experiencia] en [dominio]"

2. CONTEXTO
   - Qué es el producto
   - Competencia
   - Diferenciadores
   - Restricciones técnicas

3. ENTREGABLES (numerados)
   - User Stories (con plantilla completa)
   - Priorización (metodología específica)
   - Visualización (formatos específicos)
   - Métricas adicionales

4. PLANTILLAS
   - Proporcionar plantilla exacta para cada entregable
   - Incluir campos obligatorios y opcionales

5. DOCUMENTO FUENTE
   - Anexar el documento de diseño completo
   - Mencionar qué elementos del documento deben referenciarse
```

### Recomendaciones para Futuros Prompts

1. **Siempre proporcionar plantilla explícita** - Reduce variabilidad en el output
2. **Especificar metodología de priorización** - Evita priorizaciones arbitrarias
3. **Incluir contexto de dominio** - Mejora la relevancia del contenido
4. **Solicitar múltiples formatos de visualización** - ASCII, Mermaid, tablas
5. **Pedir conexión explícita con arquitectura** - Produce US técnicamente viables
6. **Dividir en entregables numerados** - Asegura completitud

---

## Aplicación al Proyecto LTI

El backlog generado para LTI utilizando el Prompt 5 resultó en:

- **10 User Stories** completas y técnicamente viables
- **Priorización MoSCoW + WSJF** con justificaciones
- **Mapa de dependencias** que evita bloqueos en desarrollo
- **Proyección de sprints** para planificación de recursos
- **Conexión directa** con la arquitectura de microservicios propuesta

Este enfoque permitirá al equipo de desarrollo comenzar la implementación con claridad sobre el orden de trabajo y las interdependencias técnicas.
