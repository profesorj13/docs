# Sprint 31 — Planning de historias

> Fecha: 2026-03-02
> Estado: Draft — en definición

---

## Historias existentes (revisión)

### DASH-007 · Métricas semanales ("Esta semana")

**Épica:** Dashboard · **Prioridad:** media · **Estado:** to-do
**Figma:** [node 786-179020](https://www.figma.com/design/LBojaTcYAAiFbfE9G6wb7F/TICH-AI--%3E-Tuni-V3?node-id=786-179020)

**Como** estudiante,
**quiero** ver cuántas clases hice, horas estudié y cuánto avancé esta semana,
**para** tener noción de mi ritmo de estudio y mantener la motivación.

**Descripción**

Card `WeeklyMetricsCard` con tres métricas con iconografía:
- Clases realizadas (ícono birrete) — ej. 5
- Horas estudiadas (ícono reloj) — ej. 3
- Avance semanal (ícono tendencia) — ej. 10%

**Criterios de aceptación**
- [ ] Se muestran las tres métricas con valores de la semana en curso
- [ ] Cada métrica tiene su ícono correspondiente (birrete, reloj, tendencia)
- [ ] Si no hay actividad en la semana, mostrar "0" (sin empty state aparte)
- [ ] La semana se calcula lunes a domingo
- [ ] Los datos se obtienen del backend (endpoint de métricas del estudiante)

**Revisión vs. versión anterior:** Sin cambios significativos. Figma confirma el diseño con 3 métricas + iconos. Lista para desarrollo.

**Technical Enrichment (Backend)**
- **Entidades relevantes:** `ChatSession` (`src/core/entities/chat_session.go`) con `Status`, `StartedAt`, `UpdatedAt` — base para calcular clases y horas. `Point` (`src/core/entities/point.go`) para puntos.
- **Estados de sesión:** `Created`, `InProgress`, `Finished`, `Closed`, `Expired`. "Clases realizadas" = sesiones `Finished` en la semana.
- **Cálculo de horas:** Derivable de `StartedAt`/`UpdatedAt` de sesiones `Finished` en rango lunes-domingo.
- **Avance semanal:** Requiere delta de `DomainLevel` del `KnowledgeTracking` entre inicio y fin de semana.
- **Endpoint existente:** `GetStudentDashboard` no incluye métricas semanales — necesita nuevo endpoint o extensión.
- **A crear:** Use case `GetWeeklyMetrics(studentID, weekStart)` que agregue sesiones, horas y dominio del período.

**Technical Enrichment (Frontend)**
- **Archivos relevantes:** `src/modules/app/pages/dashboard.page.tsx` (donde se monta), `src/shared/components/ui/card.tsx` (Card shadcn disponible).
- **Estado actual:** No existe `WeeklyMetricsCard` ni visualización de métricas semanales. Infraestructura de TanStack Query lista — solo crear query + componente.
- **Componentes reutilizables:** `Card` shadcn para estructura. Lucide Icons ya integrado: `GraduationCap` (birrete), `Clock` (reloj), `TrendingUp` (tendencia).
- **Complejidad:** Baja. Componente nuevo con building blocks disponibles. Usar skeleton loading con TanStack Query `isLoading`.

---

### DASH-008 · Racha diaria (gamificación mínima)

**Épica:** Dashboard · **Prioridad:** media · **Estado:** to-do
**Figma:** [node 38-8690](https://www.figma.com/design/LBojaTcYAAiFbfE9G6wb7F/TICH-AI--%3E-Tuni-V3?node-id=38-8690)

**Como** estudiante,
**quiero** visualizar y acumular mi racha diaria de estudio,
**para** sentir motivación por mantener mi hábito de estudio.

**Descripción**

Widget de racha diaria con **dos puntos de aparición**:

**1. Header (persistente)**
- Visible en todas las pantallas autenticadas
- Muestra número de días consecutivos + ícono fuego

**2. Pantalla de celebración (cierre de clase)**
- Ícono de fuego grande
- Contador: "X días de racha"
- Copy: "La racha indica cuántos días seguidos estudiaste. ¡No cortes tu racha!"
- Vista semanal: círculos L-M-M-J-V con check en los días cumplidos
- Dos CTAs: "Ir a la materia" (secundario) / "Continuar estudiando" (primario)

**3. NO se muestra** durante el chat de clase ni durante evaluaciones.

**Criterios de aceptación**
- [ ] Widget en header visible post-login con ícono fuego + número de días
- [ ] Al cerrar una clase, se muestra pantalla de celebración con racha actualizada
- [ ] La pantalla muestra vista semanal (L-M-M-J-V) con checks en días activos
- [ ] CTAs: "Ir a la materia" vuelve al dashboard, "Continuar estudiando" inicia nueva clase
- [ ] Si la racha se rompe (día sin actividad), vuelve a 0 o 1 según regla definida
- [ ] No se muestra durante evaluaciones ni dentro del chat
- [ ] Se aumenta la racha al realizar una clase (chat session)
- [ ] Al perder la racha, es decir 24 horas sin realizar una clase completa, la racha se muestra en cero. Al terminar la proxima clase, vuelve a encenderse a uno.

**Revisión vs. versión anterior:** El Figma revela una pantalla de celebración completa con vista semanal (L-M-M-J-V) y dos CTAs que no estaban en la historia original. Actualizar historia con estos elementos y los nuevos criterios de aceptación.

**Technical Enrichment (Backend)**
- **Entidad existente:** `Streak` (`src/core/entities/streak.go`) con `CurrentStreak`, `MaxStreak`, `LastActivityDate`.
- **Use cases existentes:** `GetStreak` y `UpdateStreak` ya implementados — la racha se incrementa al completar sesión (`Finished`).
- **Lógica de reset:** Si `LastActivityDate` > 24h, reset a 0. Al completar siguiente clase, vuelve a 1. Ya implementado.
- **Pantalla de celebración — a crear:** Nuevo endpoint `GET /streak/celebration` que retorne `currentStreak`, `maxStreak`, y `weeklyView` (array L-V con booleano de actividad por día).
- **Weekly view:** Calcular desde `ChatSession` con status `Finished` agrupadas por día de la semana en curso.
- **Condición "no mostrar en chat/evaluaciones":** Es lógica 100% frontend (condicionar render según ruta activa).

**Technical Enrichment (Frontend)**
- **Archivos relevantes:** `src/modules/app/components/header.tsx` (donde va el badge), `src/modules/study-session/pages/study-session.page.tsx` (trigger celebración al cerrar clase), `src/modules/study-session/stores/study-session.store.ts` (ciclo de vida de sesión), `src/shared/components/ui/dialog.tsx` (Dialog shadcn).
- **Estado actual:** No existe nada de racha — ni componentes, ni store, ni API calls. Completamente nuevo.
- **Dos componentes nuevos:** (1) `StreakBadge` en header (Lucide `Flame` + número), (2) `StreakCelebrationDialog` (modal post-clase con vista semanal L-M-M-J-V y CTAs).
- **Ocultamiento:** Detectar ruta actual — si incluye `/study-session` o `/evaluation`, ocultar del header.
- **Trigger celebración:** Cuando `sessionStatus` cambia a `completed` en el store, mostrar celebración antes de navegar al dashboard.
- **Inconsistencia detectada:** Figma muestra vista semanal L-M-M-J-V (5 días), pero la historia dice "la semana se calcula lunes a domingo" (7 días). **Validar si la vista visual es L-V pero el cálculo incluye S-D.**

---

### DASH-010 · Side sheet detalle de unidad

**Épica:** Dashboard · **Prioridad:** alta · **Estado:** to-do
**Figma:** [node 205-10726](https://www.figma.com/design/LBojaTcYAAiFbfE9G6wb7F/TICH-AI--%3E-Tuni-V3?node-id=205-10726)

**Como** estudiante,
**quiero** ver el detalle de una unidad (dominio por temas, descripción),
**para** entender mi nivel en esa unidad y decidir si estudiarla.

**Descripción**

Side sheet que se despliega de derecha a izquierda con:
- Contexto de módulo (ej. "1. Fundamentos y metodología de l...")
- Título de unidad: "Conceptos básicos de la disciplina"
- Descripción breve de la unidad
- Donut "Dominio de la unidad" (ej. 30%)
- Sección colapsable "Dominio de temas": lista de temas con barra de progreso + porcentaje individual, por default abierto.
- CTA primario: "Estudiar esta unidad"

**Criterios de aceptación**
- [ ] Se abre como overlay de derecha a izquierda
- [ ] Muestra módulo padre, título, descripción y donut de dominio
- [ ] Sección "Dominio de temas" es colapsable (accordion) con barra + % por tema, por default abierto.
- [ ] CTA "Estudiar esta unidad" inicia clase en esa unidad
- [ ] Se cierra con click fuera o ícono X
- [ ] Botón X en esquina superior derecha

**Revisión vs. versión anterior:** Figma confirma el diseño. Se agrega detalle de que "Dominio de temas" es colapsable (accordion). Sin otros cambios significativos.

**Technical Enrichment (Backend)**
- **Entidades existentes:** `Unit` (`src/core/entities/unit.go`) con `Name`, `Description`, `ModuleID`. `Topic` (`src/core/entities/topic.go`) con `UnitID`. `Module` para contexto padre.
- **Dominio por tema:** `KnowledgeTracking` (`src/core/entities/knowledge_tracking.go`) con `DomainLevel` por topic por estudiante — ya disponible.
- **Dominio de unidad (donut):** Promedio ponderado de `DomainLevel` de todos los topics de la unidad. No existe como cálculo directo — a implementar.
- **A crear:** Use case `GetUnitDetail(studentID, unitID)` que retorne: info de unidad + módulo padre, % dominio agregado (donut), lista de topics con `DomainLevel` individual (barras).
- **Endpoint:** `GET /units/{unitID}/detail?student_id=X` o similar.

**Technical Enrichment (Frontend)**
- **Archivos relevantes:** `src/shared/components/ui/sheet.tsx` (Sheet shadcn — **ya existe**, soporta `side="right"`), `src/shared/components/ui/accordion.tsx` (Accordion — **ya existe**), `src/shared/components/ui/progress.tsx` (Progress bar — **ya existe**), `src/modules/courses/pages/course-detail.page.tsx` (donde se abriría el sheet).
- **Estado actual:** Los building blocks de shadcn existen (Sheet, Accordion, Progress). No existe componente de side sheet de unidad — es ensamblaje de piezas disponibles.
- **Componentes reutilizables:** `Sheet` con `side="right"`, `Accordion`/`AccordionItem` para sección colapsable (abierto por default con `defaultValue`), `Progress` para barras por tema, `Button` para CTA.
- **Falta donut chart:** No hay componente de donut/ring en el proyecto. Recomendación: SVG custom con `stroke-dasharray` (ligero, sin dependencias) dado que es un solo donut simple. Evitar instalar `recharts` para un solo uso.
- **Vínculo con CLS-220:** El CTA "Estudiar esta unidad" es entry point de CLS-220. Ambas historias están acopladas.

---

## Historias nuevas

### CLS-220 · Estudiar específicamente una unidad

**Épica:** Clase · **Prioridad:** alta · **Estado:** to-do

**Como** estudiante,
**quiero** poder elegir estudiar una unidad específica (no solo la recomendada),
**para** prepararme para un tema particular que necesito o me interesa.

**Descripción**

Hoy el flujo principal lleva al alumno a la "clase recomendada" (DASH-001), que elige la unidad según dominio. Esta historia habilita el camino alternativo: el alumno elige manualmente qué unidad estudiar.

**Entry points:**
1. Desde DASH-005 (lista de contenidos): botón "Estudiar" en hover de una unidad
2. Desde DASH-010 (side sheet): CTA "Estudiar esta unidad"

**Flujo:**
1. Alumno selecciona una unidad desde cualquiera de los entry points
2. El sistema inicia una clase con Ada sobre esa unidad específica
3. Ada adapta el plan de enseñanza al nivel de dominio del alumno en esa unidad
4. El flujo de clase (momentos pedagógicos 1-4) funciona igual que con la clase recomendada

**Criterios de aceptación**
- [ ] Click en "Estudiar" (DASH-005) o "Estudiar esta unidad" (DASH-010) inicia clase en la unidad seleccionada
- [ ] El backend recibe el `unit_id` elegido por el alumno (no el recomendado por sistema)
- [ ] Se crea una chat session para esa unidad, con un plan de clase adaptado al nivel de dominio del alumno en esa unidad
- [ ] El flujo de clase es idéntico al de la clase recomendada (momentos 1-4)
- [ ] Al cerrar la clase, el alumno vuelve al dashboard de la materia
- [ ] Las métricas (puntos, dominio, racha) se registran normalmente

**Notas técnicas**
- Requiere que el endpoint de inicio de clase acepte `unit_id` como parámetro opcional (Discutir con backend y confirmar)
- Contexto: ver [Clase spec](/epicas/clase/spec), [Dashboard spec](/epicas/dashboard/spec)

**Technical Enrichment (Backend)**
- **StudySession ya recibe UnitID:** `CreateStudySessionRequest` (`src/core/usecases/study_sessions/create.go:19`) ya tiene campo `UnitID uuid.UUID`. Se almacena en `StudySession.IncludeUnits` (`wrappers.UUIDSlice`). El endpoint `POST /v1/study-sessions/:id` ya acepta `unit_id`.
- **Pero NO se usa para filtrar actividades:** `listConceptsConfig` no pasa filtro de `UnitID`. `getActivitiesFromAllocation` lo ignora. El SQL de `ListForStudySession` (`src/repositories/activities/list_for_study_session.go:11`) **no filtra por unidad**.
- **ChatSession sin unit awareness:** `CreateChatSession` no tiene campo `UnitID` — siempre elige "next unlocked concept" globalmente. `CreateChatSessionRequest` tiene `SubjectID` pero no `UnitID`.
- **Implementación:** (1) Agregar filtro `UnitID` a `providers.ListConceptsConfig`, (2) wirear en el repository de conceptos, (3) pasar `UnitID` desde `CreateChatSession` si clases se inician via chat.
- **Validación:** Verificar que la unidad pertenezca a una materia inscripta del alumno.
- **Gotcha:** `StudySessionTypeLesson` existe como constante pero está excluido de `IsValid()` — incluirlo si clases por unidad crean sesiones de ese tipo.

**Technical Enrichment (Frontend)**
- **Archivos relevantes:** `src/modules/study-session/actions/start-study-session.action.ts` (acción que inicia sesión — llama `cronos.createStudySession()`), `src/modules/study-session/stores/study-session.store.ts` (store de sesión activa), `src/modules/courses/components/unit-card.tsx` (card de unidad, entry point DASH-005), `src/modules/courses/pages/course-detail.page.tsx` (lista unidades).
- **Estado actual:** `startStudySession` solo envía `course_id`, no `unit_id`. Entry points existen parcialmente: lista de unidades en `course-detail.page.tsx` sin botón "Estudiar" en hover. Side sheet DASH-010 aún no existe.
- **Cambio mínimo en frontend:** Pasar `unit_id` como parámetro opcional en `startStudySession.action.ts` y propagarlo desde los entry points. El flujo completo de `study-session` se reutiliza.
- **Dependencia:** Confirmar que `POST /study-sessions` acepta `unit_id` opcional en backend. Navegación usa `navigate('/study-session')` con state.

---

### CLS-221 · Cerrar clase sin terminar (alert dialog)

**Épica:** Clase · **Prioridad:** alta · **Estado:** to-do
**Figma:** [node 2283-13180](https://www.figma.com/design/LBojaTcYAAiFbfE9G6wb7F/TICH-AI--%3E-Tuni-V3?node-id=2283-13180)

**Como** sistema,
**quiero** permitir que el alumno cierre una clase sin terminarla, con una confirmación,
**para** evitar abandonos accidentales y preservar el progreso parcial.

**Descripción**

Alert dialog que se activa **únicamente** cuando el alumno intenta abandonar una clase en curso que no fue finalizada.

**Condiciones de activación:**
- La clase ya comenzó
- El alumno está resolviendo contenido (chat + actividades)
- Aún no llegó a la pantalla de cierre/finalización
- El progreso es parcial

**Trigger:** El alumno hace click en el botón "X" (cerrar) de la ventana de clase.

**Contenido del dialog:**
- Ícono de advertencia
- Título: "¡No frenes ahora!"
- Copy: mensaje sobre que perderá el progreso de esta sesión si sale
- CTA primario: "Seguir con la clase" (cierra el dialog, vuelve a la clase)
- CTA secundario: "Terminar clase" (cierra la clase, redirige al dashboard de materia)

**Criterios de aceptación**
- [ ] El dialog se muestra al hacer click en X durante una clase en curso no finalizada
- [ ] No se muestra si la clase ya llegó a la pantalla de cierre
- [ ] "Seguir con la clase" cierra el dialog y retoma donde estaba
- [ ] "Terminar clase" cierra la clase y redirige al dashboard de la materia
- [ ] El progreso parcial se persiste (actividades respondidas, dominio parcial)
- [ ] El dialog tiene overlay oscuro y no se puede interactuar con la clase detrás
- [ ] Si se clickea afuera del dialog, se cierra y se retoma la clase

**Notas técnicas**
- Definir con backend qué datos se persisten al cerrar parcialmente (¿se guarda la chat session como incompleta?)
- Contexto: ver [Clase spec](/epicas/clase/spec), pantalla de cierre (CLS-056)

**Technical Enrichment (Backend)**
- **Modelo de sesión:** `StudySession` (`src/core/entities/study_sessions.go:38`) tiene `Status` con valores `active`, `completed`, `canceled`.
- **Status `canceled` existe pero nunca se usa:** `StudySessionStatusCanceled` está definido (`study_sessions.go:35`) pero **ningún código lo setea**. La migración 000027 ya incluye el valor en la DB.
- **No hay endpoint de cancelación:** Crear `PATCH /v1/study-sessions/:id/cancel` que setee `Status = "canceled"` y `EndTime = now()`.
- **Nuevo use case necesario:** `CancelStudySession` con **locking via advisory lock** (`studySessionProvider.Lock`) para evitar race conditions — mismo patrón que el endpoint de evaluate.
- **Persistencia parcial ya funciona:** `UserActivity` records con `FinishedAt != nil` ya están guardados — cancelar no pierde progreso parcial.
- **Umbral de completado:** Sesión se marca `completed` automáticamente cuando `completedCount >= 6` (hardcodeado en `register_activities_in_session_study.go:234`). El cancel es para salir antes de ese umbral.
- **Sin migración adicional:** El status `canceled` ya existe en el enum de BD.

**Technical Enrichment (Frontend)**
- **Archivos relevantes:** `src/modules/study-session/components/study-session-header.tsx` (header con botón X — actualmente navega sin confirmación), `src/modules/study-session/stores/study-session.store.ts` (estado de sesión con `sessionStatus`), `src/shared/components/ui/alert-dialog.tsx` (AlertDialog shadcn — **ya existe**).
- **Estado actual:** El botón X navega al dashboard **sin ninguna confirmación**. No hay dialog de intercepción. Las actividades respondidas se envían en tiempo real — progreso parcial ya se persiste implícitamente.
- **Implementación directa:** Interceptar click del X, verificar `sessionStatus !== 'completed'`, mostrar `AlertDialog`. Overlay oscuro y cierre son nativos del componente.
- **Corrección al planning:** El doc dice "Si se clickea afuera del dialog, se cierra y se retoma la clase". Sin embargo, shadcn `AlertDialog` **no se cierra al click afuera** (a diferencia del `Dialog` regular). Si se quiere ese comportamiento, usar `Dialog` en vez de `AlertDialog`, o customizar. **Decisión de UX a confirmar con Figma.**
- **Redirección:** "Al dashboard de la materia" necesita `course_id` — ya disponible en el store.

---

### CLS-222 · Ocultar actividades reportadas según criterio

**Épica:** Clase / Comprobación · **Prioridad:** media · **Estado:** to-do

**Como** sistema,
**quiero** ocultar automáticamente actividades que acumulen reportes de alumnos,
**para** evitar que actividades defectuosas sigan apareciendo en las sesiones de comprobación.

**Descripción**

Continuación de la funcionalidad de [Reportar Actividad](/epicas/clase/reportar-actividad). Hoy los alumnos pueden reportar actividades con 3 causas (errores, no pertinente, contenido incorrecto), pero los reportes no generan ninguna acción automática. Esta historia implementa la lógica de ocultamiento.

**Criterio propuesto:**
- Una actividad se oculta automáticamente cuando acumula **N reportes** (umbral configurable, Hoy: 2).
- El ocultamiento es "soft delete": la actividad no se elimina, se marca como `hidden` (o lo que corresponda en db) y deja de aparecer en nuevas sesiones.
- Las sesiones en curso que ya cargaron la actividad no se ven afectadas.
- Un supervisor puede revertir el ocultamiento desde su panel (futuro).


**Causas con peso diferenciado (propuesta):** (Contemplar un posible desarrollo asi, hoy no forma parte del alcance)
| Causa | Peso | Umbral efectivo |
|-------|------|-----------------|
| Contenido incorrecto | 2 | 2 reportes = oculta |
| No pertinente a la materia | 1.5 | 2 reportes = oculta |
| Actividad con errores | 1 | 3 reportes = oculta |

**Criterios de aceptación**
- [ ] Al recibir un reporte, el sistema evalúa si la actividad alcanzó el umbral de ocultamiento
- [ ] Si se alcanza el umbral, la actividad se marca como `hidden`
- [ ] Actividades `hidden` no aparecen en nuevas sesiones de comprobación
- [ ] Sesiones en curso no se ven afectadas por el ocultamiento
- [ ] El umbral y los pesos son configurables (no hardcodeados)
- [ ] Se registra log/evento cuando una actividad se oculta automáticamente
- [ ] Un alumno no puede reportar la misma actividad dos veces (ya implementado)

**Notas técnicas**
- Modelo ActivityReport ya existe con campos: `activity_id`, `student_id`, `reason`, `created_at`
- Agregar campo `hidden` (boolean) y `hidden_at` (timestamp) a la tabla de actividades
- Agregar campo `hidden_reason` (ej. "auto_reported") para distinguir de ocultamientos manuales
- Contexto: ver [Reportar Actividad](/epicas/clase/reportar-actividad), [Contenido spec](/epicas/contenido/spec)
- Para evitar que un loco nos rompa la app crearemos una atuomatizacion a slack a un canal especifico donde veremos las actividades reportadas y podremos decidir si ocultarlas o no.

**Technical Enrichment (Backend)**
- **Entidad existente:** `ActivityReport` (`src/core/entities/activity_report.go:9`) con `id`, `activity_id`, `user_id`, `reason`, `created_at`. Tabla creada en migración 000045.
- **Entidad Activity:** (`src/core/entities/activities.go:18`) tiene `Status` con valores `pending | approved | rejected`. **NO tiene campo `hidden`**.
- **Fast path de supervisor ya existe:** En `report.go`, si quien reporta es **supervisor**, la actividad se setea `status = "rejected"` inmediatamente (se remueve de sesiones). **Reportes de alumnos no tienen ningún efecto** actualmente.
- **Query de actividades:** `ListForStudySession` (`src/repositories/activities/list_for_study_session.go:11`) solo filtra por `status = 'approved'` — no hace join con `activity_reports`.
- **Dos opciones de implementación:** (1) **Threshold auto-rejection:** Contar reportes, si `count >= N` → `status = 'rejected'`. Más simple, afecta a todos. (2) **Campo `hidden` separado:** Agregar `hidden BOOLEAN`, `hidden_at TIMESTAMP`, `hidden_reason VARCHAR`. Permite diferenciar rejection manual de auto-reportada.
- **Provider a extender:** `ActivityReports` (`src/core/providers/activity_reports.go`) solo tiene `Save` — agregar método `Count(activityID)` para lógica de umbral.
- **Queries a modificar:** `ListForStudySession` y `providers.Activities.List()` (`src/repositories/activities/list.go`) — ambos necesitan filtrar actividades ocultas.
- **Pesos diferenciados (futuro):** `activity_reports.reason` ya permite mapear peso por causa sin cambios de schema.

**Technical Enrichment (Frontend)**
- **Archivos relevantes:** `src/modules/study-session/components/activity-view.tsx` (renderiza actividad con opción de reportar), `src/modules/study-session/components/report-activity-dialog.tsx` (dialog de reporte con 3 causas — **ya implementado**), `src/modules/study-session/actions/report-activity.action.ts` (envía reporte vía `cronos.reportActivity()`).
- **Estado actual:** Flujo de reporte frontend **ya completo** — reporta con 3 causas, no puede reportar la misma actividad dos veces (validación local implementada).
- **Impacto frontend mínimo a nulo.** Si el backend filtra actividades `hidden` antes de enviarlas, el frontend no necesita ningún cambio. Esta historia es **95% backend**.
- **Único cambio posible:** Si se quiere mostrar estado visual "actividad ocultada" (para supervisores futuros), se necesitaría trabajo frontend. Para alumnos, no.

---

### CLS-223 · [Producto] Tutor ayuda a repasar temas

**Épica:** Clase · **Prioridad:** media · **Estado:** Backlog

**Como** estudiante,
**quiero** que el tutor me ayude a repasar temas que ya estudié,
**para** reforzar mi conocimiento antes de un parcial o evaluación.

**Descripción**

Modo "Repaso" diferenciado del flujo de clase normal. Hoy el flujo de clase tiene dos modos: Comprobación (verificar conocimiento con actividades) y Enseñar (explicar conceptos nuevos). El repaso es un tercer modo orientado a consolidar lo ya aprendido.

**Diferencias con clase normal:**
| Aspecto | Clase normal | Modo repaso |
|---------|-------------|-------------|
| Objetivo | Avanzar en dominio (niveles 1→4) | Consolidar conocimiento existente |
| Contenido | Temas nuevos o con bajo dominio | Temas ya vistos (dominio >= 2) |
| Actividades | Progresivas por nivel | Variadas, priorizando errores previos |
| Duración | Sesión completa (plan de 3 ítems) | Más corta, enfocada |
| Ada | Guía descubrimiento | Refuerza y conecta conceptos |

**Flujo propuesto:**
1. Alumno selecciona "Repasar" desde el dashboard de unidad (nuevo CTA)
2. Ada genera un plan de repaso basado en: temas con dominio medio (2-3), errores frecuentes del alumno, tiempo desde último estudio
3. Loop de repaso: resumen rápido del tema → pregunta de verificación → feedback → siguiente tema
4. Al finalizar: resumen de temas repasados + sugerencia de próximos pasos

**Criterios de aceptación**
- [ ] Existe un entry point "Repasar" accesible desde el dashboard (ubicación a definir)
- [ ] Ada genera plan de repaso basado en historial del alumno
- [ ] El repaso prioriza temas con dominio medio y errores previos
- [ ] Las sesiones de repaso son más cortas que una clase completa
- [ ] El dominio del alumno puede aumentar como resultado del repaso
- [ ] Al finalizar se muestra resumen de temas repasados

**Notas técnicas**
- Requiere nuevo tipo de sesión (`session_type: "review"`) o flag en study session
- Ada necesita acceso al historial de errores del alumno por unidad (ya existe en knowledge tracking)
- Prompt de Ada debe diferenciarse del Momento 4 (Enseñar): aquí no re-explica desde cero, sino que refuerza
- Contexto: ver [Clase overview](/epicas/clase/overview), [Knowledge Tracking](/epicas/clase/knowledge-tracking), [Identity](/identity)

**Pendientes de definición:**
- ¿Desde dónde se accede? (side sheet de unidad, dashboard, o ambos)
- ¿Tiene efecto en puntos/racha?
- Duración objetivo de una sesión de repaso

---

### CLS-224 · [Producto] Modo desempeño (estudio intensivo)

**Épica:** Clase · **Prioridad:** media · **Estado:** backlog

**Como** estudiante,
**quiero** un "modo desempeño" para incorporar los temas más importantes en poco tiempo,
**para** prepararme rápidamente cuando tengo un parcial cerca o poco tiempo disponible.

**Descripción**

Modo de estudio intensivo/acelerado que prioriza los temas de mayor impacto en el menor tiempo posible. A diferencia de la clase normal (que sigue una progresión completa por niveles), el modo desempeño es pragmático: va directo a lo esencial.

**Características diferenciadoras:**
| Aspecto | Clase normal | Modo desempeño |
|---------|-------------|----------------|
| Selección de temas | Por nivel de dominio | Por importancia/peso en evaluaciones |
| Profundidad | Completa (4 niveles) | Esencial (conceptos clave + aplicación) |
| Duración | Sin límite de re-explicaciones | Acotada, orientada a eficiencia |
| Actividades | Progresivas | Directas, tipo examen |
| Tono de Ada | Paciente, exploratoria | Directa, enfocada, "modo coach" |

**Flujo propuesto:**
1. Alumno selecciona "Modo desempeño" (entry point a definir — posible CTA en dashboard cuando hay parcial cerca)
2. El sistema identifica los temas prioritarios: bajo dominio + alto peso en evaluaciones
3. Ada presenta un plan compacto: "Tenés 30 min, vamos a cubrir los 5 temas más importantes"
4. Por cada tema: explicación condensada → actividad de verificación rápida → siguiente
5. Al finalizar: resumen ejecutivo de lo cubierto + recomendaciones de qué seguir estudiando

**Criterios de aceptación**
- [ ] Existe un entry point para el modo desempeño (ubicación a definir)
- [ ] El sistema prioriza temas por impacto: bajo dominio + alto peso en evaluaciones
- [ ] Ada adapta su tono a "modo coach" (más directa, menos exploratoria)
- [ ] Las sesiones son más cortas y enfocadas que una clase normal
- [ ] Al finalizar se muestra resumen de temas cubiertos y recomendaciones
- [ ] Las métricas de dominio se actualizan normalmente

**Notas técnicas**
- Requiere metadata de "peso/importancia" por tema (¿viene de la indexación de contenido?)
- Nuevo tipo de sesión (`session_type: "performance"`) o flag
- El prompt de Ada debe diferenciarse: menos socrático, más directo y eficiente
- Contexto: ver [Clase overview](/epicas/clase/overview), [Contenido spec](/epicas/contenido/spec), [Identity](/identity)

**Pendientes de definición:**
- ¿Cómo se determina la "importancia" de un tema? (frecuencia en evaluaciones, peso en programa, input del docente)
- ¿Duración objetivo? (15 min, 30 min, configurable)
- ¿Se activa automáticamente cuando hay parcial cerca, o siempre es manual?
- ¿Qué pasa con la identidad de Ada? El modo coach debe respetar las reglas base (nunca revelar respuestas, etc.)

---

### PLAT-001 · Ambiente con identidad de marca del cliente

**Épica:** Plataforma · **Prioridad:** alta · **Estado:** to-do

**Como** cliente (universidad/institución),
**quiero** que TUNI refleje mi identidad de marca (logo, colores, nombre),
**para** que los alumnos lo perciban como parte de mi oferta educativa.

**Descripción**

White-labeling básico que permite a cada cliente institucional personalizar la apariencia de TUNI. El alumno ve la plataforma con la identidad visual de su universidad, no con la marca genérica TUNI.

**Elementos personalizables (MVP):**
| Elemento | Ejemplo |
|----------|---------|
| Logo | Logo de la universidad en header/sidebar |
| Colores primarios | Paleta institucional (primary, secondary, accent) |
| Nombre de plataforma | "Siglo 21 Tutorías" en vez de "TUNI" |
| Favicon | Ícono de la institución |

**Elementos NO personalizables (MVP):**
- Personalidad y reglas pedagógicas de Ada (se mantienen)
- Estructura de navegación
- Lógica de gamificación
- Flujos de clase/evaluación

**Criterios de aceptación**
- [ ] Existe una configuración por cliente (¿tenant?¿Cosmos?) con: logo, colores, nombre, favicon
- [ ] El frontend carga la configuración del tenant al iniciar sesión
- [ ] Los colores del tema se aplican globalmente (header, botones, acentos)
- [ ] El logo del cliente reemplaza al logo por defecto en header/sidebar
- [ ] El nombre de la plataforma se muestra según configuración del tenant
- [ ] Si no hay configuración custom, se usa el branding por defecto de TUNI
- [ ] El nombre del tutor es configurable por tenant

**Notas técnicas**
- Arquitectura multi-tenant: cada cliente tiene su configuración de branding
- Los colores se pueden implementar con CSS variables / design tokens por tenant
- La configuración puede vivir en BD o en un archivo de configuración por tenant
- Considerar: ¿subdominios por cliente (siglo21.tuni.app) o path-based?

**Pendientes de definición:**
- ¿Modelo de tenancy? / cosmos (subdominio, path, o basado en login)
- ¿Quién carga la configuración de marca? (admin TUNI o self-service del cliente)
- ¿El tutor puede cambiar de nombre pero mantener la misma personalidad?

**Technical Enrichment (Backend)**
- **Modelo de tenant real:** `University` (`src/core/entities/universities.go:10`) con `university_id`, `display_name`, `timezone`, `country`. Users vinculados via `User.UniversityID`.
- **Branding ya existe parcialmente:** `UniversityPreference` (`src/core/entities/universities.go:27`) ya tiene `LogoURL string`, `Colors datatypes.JSON`, `ColorsVersion uint`. Tabla: `university_preferences` (migración 000001).
- **Campos a agregar (migración):** Extender `university_preferences` con: `favicon_url`, `platform_name`, `tutor_name` (default: "Ada"). Los colores ya se almacenan en JSONB (`colors_json`).
- **Endpoint:** Extender el endpoint existente de universidad o crear `GET /universities/{id}/branding`. Ya hay patrón GORM de preload.
- **Tutor name:** Inyectar `tutor_name` de `UniversityPreference` en el prompt del LLM en lugar de hardcodear "Ada". La personalidad y reglas pedagógicas no cambian.
- **Pattern `datatypes.JSON`:** Ya usado para colores — mismo patrón reutilizable para config adicional.

**Technical Enrichment (Frontend)**
- **Archivos relevantes:** `src/app.css` (CSS con `@theme inline` — define `--color-primary`, `--color-secondary`, `--color-accent`, etc.), `index.html` (favicon y título), `src/modules/app/components/sidebar.tsx` (logo estático), `src/modules/app/components/header.tsx`, `src/modules/auth/stores/auth.store.ts` (extender para datos de tenant).
- **Estado actual:** **La base técnica ya existe.** Tailwind v4 con CSS custom properties es la arquitectura ideal para white-labeling. Logo y favicon son estáticos. No hay concepto de tenant en frontend.
- **La infraestructura es perfecta:** CSS variables del proyecto se sobrescriben en runtime por tenant. No se necesita lib de theming externa.
- **Implementación sugerida:** (1) Endpoint backend de config de tenant al login, (2) `TenantProvider` que aplique CSS variables al `<html>`, (3) Logo y favicon dinámicos (favicon via `document.querySelector('link[rel="icon"]').href`).
- **Subdominios:** Si se usa `siglo21.tuni.app`, se detecta tenant desde `window.location.hostname` antes del login.
- **Nombre del tutor:** Buscar todas las referencias hardcodeadas a "Ada" en el código y hacerlas dinámicas (`tenant.tutorName ?? 'Ada'`).

---

### PLAT-002 · Ambiente con funcionalidades a medida del cliente

**Épica:** Plataforma · **Prioridad:** alta · **Estado:** to-do

**Como** cliente (universidad/institución),
**quiero** elegir qué funcionalidades están activas en mi ambiente,
**para** adaptar la plataforma a mis necesidades y modelo pedagógico.

**Descripción**

Sistema de feature flags por tenant que permite activar/desactivar módulos de TUNI según las necesidades del cliente. No todos los clientes necesitan todas las funcionalidades.

**Funcionalidades togglables (propuesta):**
| Feature | Default | Descripción |
|---------|---------|-------------|
| Clase adaptativa | ON | Flujo de tutoría con Ada (core) |
| Evaluaciones | ON | Evaluaciones por módulo |
| Racha diaria | ON | Gamificación mínima |
| Consultas | ON | Side panel "Tengo una consulta" |
| Widget Canvas | OFF | Integración con Canvas LMS |
| Widget flotante | OFF | Tutor flotante fuera de clase |
| Reportar actividad | ON | Alumnos pueden reportar actividades |
| Modo desempeño | OFF | Estudio intensivo pre-parcial |
| Modo repaso | OFF | Sesiones de refuerzo |

**Criterios de aceptación**
- [ ] Existe una configuración de features por tenant con flags on/off
- [ ] El frontend consulta los feature flags al cargar y oculta/muestra módulos
- [ ] Features desactivadas no aparecen en navegación, dashboard ni entry points
- [ ] El backend respeta los flags (no expone endpoints de features desactivadas)
- [ ] Existe un panel admin (o config) para gestionar los flags por tenant
- [ ] Cambios en flags se reflejan sin necesidad de redeploy

**Notas técnicas**
- Puede implementarse con: tabla de configuración en BD, servicio de feature flags (LaunchDarkly, Unleash), o config file por tenant
- Los flags deben ser consultables tanto desde frontend como backend
- Considerar granularidad: ¿solo on/off o también configuración por feature? (ej. umbral de racha, cantidad de mensajes anónimos)
- Relación directa con PLAT-001 (mismo modelo de tenancy)

**Pendientes de definición:**
- ¿Qué herramienta de feature flags usar?
- ¿Quién gestiona los flags? (admin TUNI, o self-service del cliente)
- ¿Features que dependen de otras? (ej. widget Canvas requiere configuración LTI)

**Technical Enrichment (Backend)**
- **Estado actual:** No existe sistema de feature flags en backend. Ninguna de las 45+ migraciones crea tabla de feature toggles. Zero infraestructura de on/off por tenant.
- **Modelo de tenant:** `University` + `UniversityPreference` (misma base que PLAT-001). `UniversityPreference` ya usa `datatypes.JSON` para colores — mismo patrón aplicable.
- **Dos opciones de implementación:** (1) Columna `features_json JSONB` en `university_preferences` (simple, flexible, sin tabla extra). (2) Tabla relacional `university_features(university_id FK, feature_key VARCHAR, enabled BOOL, config JSONB)` (queryable, type-safe).
- **Recomendación:** Opción 1 para MVP (JSONB blob), con constantes en Go para feature keys (`FeatureEmbeddedChat = "embedded_chat"`, etc.).
- **Endpoint:** `GET /universities/{id}/features` retorna mapa de features. Cacheable.
- **Backend enforcement:** Helper `IsFeatureEnabled(ctx, universityID, featureName)` llamado en handlers o como middleware.
- **Frontend ya tiene LaunchDarkly:** `package.json` incluye `launchdarkly-react-client-sdk`. Decisión de herramienta frontend ya tomada — backend debe sincronizar flags con LD o exponer su propio endpoint. **Eliminar la pregunta "¿Qué herramienta usar?" del doc.**
- **Dependencia con PLAT-001:** Comparten modelo `University`. Implementar juntos.

**Technical Enrichment (Frontend)**
- **Archivos relevantes:** `package.json` (contiene `launchdarkly-react-client-sdk` — **ya integrado**), `src/modules/app/components/sidebar.tsx` (navegación principal), rutas/router config.
- **Estado actual:** **LaunchDarkly ya está integrado.** Hooks `useFlags()` y `useLDClient()` disponibles. Uso en etapa inicial — no hay uso extensivo en componentes. Las rutas no están condicionadas por flags.
- **La decisión de herramienta ya está tomada.** El planning pregunta "¿Qué herramienta de feature flags usar?" pero **ya se eligió LaunchDarkly**. Eliminar esta pregunta del doc de pendientes.
- **Implementación:** (1) Definir flags en dashboard LaunchDarkly (`enable-streak`, `enable-evaluations`, etc.), (2) Segmentar por `tenant_id`, (3) `useFlags()` en sidebar para mostrar/ocultar secciones, (4) `FeatureFlagGuard` component para proteger rutas.
- **Cambio sin redeploy:** LaunchDarkly cumple nativamente — flags se evalúan en tiempo real.
- **Relación con PLAT-001:** El `tenant_id` de PLAT-001 es la targeting key de LaunchDarkly.

---

### DASH-012 · [Producto] TUNI en mobile

**Épica:** Dashboard · **Prioridad:** alta · **Estado:** to-do
**Figma:** [node 2287-3221](https://www.figma.com/design/LBojaTcYAAiFbfE9G6wb7F/TICH-AI--%3E-Tuni-V3?node-id=2287-3221)

**Como** estudiante,
**quiero** contar con TUNI en mobile,
**para** poder estudiar desde mi celular en cualquier momento y lugar.

**Descripción**

Versión mobile-responsive de TUNI. El Figma muestra la pantalla de selección de materias adaptada a mobile:

**Header mobile:**
- Ícono sidebar (hamburguesa)
- Racha diaria (ej. "1")
- Puntos (ej. "440 pts")
- Avatar del usuario

**Contenido:**
- Saludo: "¡Hola, nombre!"
- Pregunta: "¿Con qué materia comenzamos?"
- Cards de materia en formato carrusel (swipeable, con dots de paginación)
- Cada card muestra: pill "Parcial en X días", badge "Recomendada", nombre de materia, barra de progreso con % dominio, unidad actual

**Alcance propuesto (MVP mobile):**
| Pantalla | Prioridad |
|----------|-----------|
| Selección de materias (home) | Alta — diseño disponible |
| Dashboard de materia | Alta — adaptar layout desktop |
| Chat de clase con Ada | Alta — ya es vertical por naturaleza |
| Side panels (unidad, consulta) | Media — convertir a full-screen mobile |
| Sesión de actividades | Alta — definir la transición entre actividades y chat. |
| Pantalla de cierre + racha | Media — ya tiene diseño vertical |

**Criterios de aceptación**
- [ ] La pantalla de materias se muestra correctamente en viewports mobile (< 768px)
- [ ] Las cards de materia se muestran en carrusel horizontal con swipe y dots de paginación
- [ ] El header se adapta: sidebar se convierte en menú hamburguesa
- [ ] Racha, puntos y avatar visibles en header mobile
- [ ] El chat de clase funciona correctamente en mobile (input fijo abajo, mensajes scrolleables)
- [ ] Los side panels se convierten en vistas full-screen en mobile, que surgen desde abajo.
- [ ] Todas las interacciones táctiles funcionan (swipe, tap, scroll)
- [ ] No hay scroll horizontal accidental en ninguna pantalla

**Notas técnicas**
- Enfoque: responsive web (no app nativa). TUNI ya tiene DASH-011 (modal de instalación PWA)
- Breakpoints sugeridos: mobile (< 768px), tablet (768-1024px), desktop (> 1024px)
- Priorizar mobile-first en nuevos componentes
- El chat de Ada ya debería ser mayormente compatible (interfaz vertical)
- Contexto: ver [DASH-011 (PWA)](/epicas/dashboard/historias), [Dashboard spec](/epicas/dashboard/spec)

**Pendientes de definición:**
- ¿Se abordan todas las pantallas en un sprint o se divide por prioridad?
- ¿Tablet es un breakpoint intermedio o se trata como desktop?
- Comportamiento offline / PWA (cache de última sesión, notificaciones push)

---

### DASH-013 · Tutor flotante para consultas generales

MEJORA DASH 03; Reescribirla incorporando esto.

**Épica:** Dashboard · **Prioridad:** media · **Estado:** to-do

**Como** estudiante,
**quiero** contar con un tutor flotante para hacer consultas de mi materia o generales,
**para** obtener ayuda rápida sin necesidad de iniciar una clase.

**Descripción**

Evolución del side panel de consultas (DASH-003) hacia un widget flotante persistente accesible desde cualquier pantalla. A diferencia de DASH-003 (que solo atiende consultas de la materia activa), este tutor flotante maneja tres tipos de consultas:

**1. Consultas de la materia (RAG)**
- Ada responde preguntas académicas usando el contenido indexado de la bibliografía
- Usa Retrieval-Augmented Generation para fundamentar respuestas en las fuentes
- Ejemplo: "¿Cuáles son los principios del derecho constitucional?"

**2. Consultas administrativas**
- Ada responde preguntas sobre la institución, trámites, fechas, etc.
- Requiere una base de conocimiento administrativa por tenant
- Ejemplo: "¿Cuándo es la fecha de inscripción a finales?"

**3. Consultas sobre el sistema**
- Ada explica cómo usar TUNI: funcionalidades, navegación, tips
- Conocimiento embebido en el prompt (no requiere RAG)
- Ejemplo: "¿Cómo funciona la racha diaria?" "¿Cómo me puedo preparar para el parcial?" (Organización + Features del sistema)

**Criterios de aceptación**
- [ ] Widget flotante visible en las pantallas autenticadas (ícono en esquina inferior, según figma) 
- [ ] Al expandir, muestra un chat con Ada
- [ ] Ada puede responder consultas académicas basadas en contenido indexado (RAG)
- [ ] Ada puede responder consultas administrativas si hay base de conocimiento configurada
- [ ] Ada puede explicar funcionalidades de TUNI (ayuda del sistema)
- [ ] Ada detecta el tipo de consulta y adapta su fuente de información
- [ ] Si la consulta amerita profundizar, Ada sugiere "¿Querés hacer una clase sobre esto?"
- [ ] El widget no interfiere con el flujo de clase (se oculta o minimiza durante clase activa)

**Notas técnicas**
- RAG requiere integración con el contenido indexado (épica Contenido)
- Base de conocimiento administrativa: por tenant, formato y carga a definir
- El widget flotante es conceptualmente similar al widget de Canvas (INT-001) pero dentro de TUNI
- Relación con DASH-003: este widget podría reemplazar el side panel de consultas o coexistir
- Contexto: ver [DASH-003](/epicas/dashboard/historias), [Contenido spec](/epicas/contenido/spec), [Identity](/identity)

**Definiciones:**
- Reemplaza a DASH-003!

**Pendiente de definiciones:**
- ¿Cómo se carga la base de conocimiento administrativa por tenant?
- ¿Límite de mensajes por sesión de consulta? ¿Cuando se resetea automaticamente el contexto?
- ¿Se persiste el historial de consultas? si; Según figma.

**Technical Enrichment (Backend)**
- **EL TUTOR FLOTANTE YA EXISTE como embedded chat.** Entidad `EmbeddedChatSession` (`src/core/entities/embedded_chat.go:10`) con: `user_id` opcional, `course_code`/`module`/`lecture` opcional, TTL de 12h, historial completo de mensajes.
- **Endpoint existente:** `POST /cronos/v1/embedded/chat` con `optionalTokenMiddleware` — ya maneja usuarios anónimos + autenticados.
- **Use case completo:** `src/core/usecases/embedded_chat/chat.go` — flujo: rate limit → session create/resume → concept resolution → readings lookup → LLM call → tool handling.
- **Prompt existente:** `EMBEDDED_TUTOR_V2` en `src/repositories/llm/prompts/embedded_tutor.go` — persona "Ada", ya diseñado para manejar consultas generales cuando no hay contexto de lecture.
- **RAG existente:** Tabla `readings` (migración 000043+044) con `url`, `course_code`, `concepts text[]` (GIN indexed), `content text`. Búsqueda por URL exacta, NO vector search.
- **ÚNICO CAMBIO BACKEND NECESARIO:** Actualmente `EmbeddedChatRequest.SessionID = nil && Context = nil` retorna `ErrContextRequired` (400) en `chat.go:94`. **Relajar esta validación** para permitir modo "general" sin contexto de lecture. `resolveConcepts()` y readings lookup ya retornan nil/empty gracefully.
- **Opcional:** Agregar campo `mode` ("general" vs "lecture") a `EmbeddedChatSession` para analytics.
- **Rate limiting:** Ya funciona por IP (`embedded_chat_rate_limits`). `suggestAuth` tool call ya nudgea a usuarios anónimos a loguearse.
- **Consultas administrativas:** Requiere nueva base de conocimiento por tenant — no existe aún. Formato y carga a definir.

**Technical Enrichment (Frontend)**
- **Archivos relevantes:** `src/modules/study-session/components/` (componentes de chat: message list, bubble, input — reutilizables), `src/modules/study-session/stores/study-session.store.ts` (patrón de gestión de mensajes), `src/modules/study-session/actions/send-message.action.ts` (estructura API reutilizable).
- **Estado actual:** **No existe DASH-003 implementado.** No hay side panel de consultas en el código. DASH-013 es la primera implementación de consultas fuera de clase. Se necesita módulo nuevo (`src/modules/consultation/` o `src/modules/floating-tutor/`).
- **Feature significativo — nuevo módulo completo.** Requiere: (1) componente flotante `fixed bottom-4 right-4`, (2) panel expandible con chat, (3) nuevo Zustand store para estado del chat de consultas, (4) nuevas acciones/API.
- **Refactor sugerido:** Extraer componentes de chat de study-session a `src/shared/components/chat/` para reutilizar en ambos contextos (study-session y tutor flotante).
- **Ocultar durante clase:** Detectar sesión activa con el store de study-session. Los 3 tipos de consulta se diferencian en backend — frontend solo envía mensaje.

---

### CLS-225 · Tutor que sepa de mí — Preferencias e historial

**Épica:** Clase · **Prioridad:** media · **Estado:** to-do

**Como** estudiante,
**quiero** que el tutor sepa de mí (mis preferencias, historial, estilo de aprendizaje),
**para** recibir una experiencia cada vez más personalizada.

**Descripción**

Hoy Ada adapta su comportamiento según el perfil del estudiante (procrastinación, autoeficacia, estrategias de estudio — ver [Identity](/identity)), pero este perfil se construye implícitamente desde las sesiones. Esta historia busca enriquecer el conocimiento que Ada tiene del alumno con datos explícitos e históricos.

**Dimensiones del "conocer al alumno":**

**1. Preferencias explícitas**
- Estilo de explicación preferido (ejemplos prácticos vs. teoría, visual vs. textual) (sale del onboarding)
- Ritmo preferido (sesiones cortas frecuentes vs. sesiones largas esporádicas)
- Temas de interés o conexión (para que Ada use analogías relevantes) (onboarding)

**2. Historial acumulado**
- Temas estudiados, dominio por unidad, evolución temporal
- Errores frecuentes y patrones de dificultad
- Tiempo promedio por sesión, frecuencia de estudio
- Consultas realizadas (temas recurrentes)

**3. Memoria conversacional**
- Ada recuerda lo que hablaron en sesiones anteriores
- Puede referenciar: "La última vez trabajamos en [tema], ¿cómo te fue en el parcial?"
- Continuidad emocional: si el alumno expresó frustración, Ada lo tiene en cuenta, le pregunta como está.
- Información adicional, personal. Estamos creando un tutor que **se relaciona** con el alumno, no solo un motor de preguntas y respuestas.

**Criterios de aceptación**
- [ ] Ada accede a un resumen del historial de sesiones del alumno al iniciar una clase
- [ ] Ada puede referenciar temas estudiados y errores previos en la conversación
- [ ] Existe un mecanismo para capturar preferencias del alumno (onboarding y progresivo)
- [ ] Ada adapta analogías y ejemplos según el contexto conocido del alumno y sus intereses.
- [ ] El alumno puede ver/editar sus preferencias (pantalla de perfil, a definir)
- [ ] Luego de una conversación, se evalúa el contenido para definir si hay nueva información que persistir para futuras sesiones.

**Notas técnicas**
- Requiere un "student profile" enriquecido más allá del knowledge tracking actual
- La memoria conversacional puede implementarse con resúmenes (summarization) de sesiones previas inyectados en el prompt
- Cuidado con el tamaño del contexto; Hay que ser muy cuidadosos en que inyectar.
- Contexto: ver [Identity](/identity), [Knowledge Tracking](/epicas/clase/knowledge-tracking), [Onboarding](/epicas/onboarding/overview)

**Pendientes de definición:**
- ¿Captura de preferencias en onboarding o de forma progresiva? -> Ambas
- ¿Cuántas sesiones previas resume Ada? (últimas 3, últimas 5, por unidad) -> 5
- ¿El alumno puede pedir que Ada "olvide" algo? -> No
- Límite de tokens para el contexto de personalización -> 1000

**Technical Enrichment (Backend)**
- **Datos de onboarding YA existen en User:** `User` (`src/core/entities/users.go:59`) ya tiene: `Nickname`, `Hobbies []string`, `AgeRange`, `Objective`, `Personality`, `FirstName`, `Sex`. Guardados via `SaveOnboardingData` (`src/core/usecases/users/save_onboarding_data.go`). **No se necesita nueva entidad de preferencias para MVP.**
- **Problema: el LLM no los usa.** `TutorContext` (`src/core/providers/llm.go:120`) solo tiene `StudentName`. `buildTutorContext` en `chatting.go:400` solo lee `conv.User.Nickname`/`FirstName` — ignora `Hobbies`, `Objective`, `Personality`, `AgeRange`.
- **Fix inmediato:** Agregar campos a `TutorContext`: `StudentHobbies []string`, `StudentObjective string`, `StudentPersonality string`, `StudentAgeRange string`. Actualizar `buildTutorContext` para leerlos de `conv.User`. Actualizar prompt templates en `src/repositories/llm/` para interpolarlos.
- **Sin migración:** Todos los campos ya existen en tabla `users` (migración 000039).
- **Gotcha preload:** `conv.User` se preloada en `ChatSession` — verificar que el preload esté activo en `ListChatSessionsConfig`. `buildTutorContext` ya hace `conv.User != nil` check, lo que implica que puede ser nil en algunos casos.
- **Memoria conversacional (nuevo):** Entidad `SessionSummary` (`session_id`, `student_id`, `summary_text`, `created_at`). Post-sesión `Finished`, trigger async genera summary via LLM (~200 tokens) y lo persiste.
- **Presupuesto de tokens (definido: 1000):** ~500 resúmenes de sesiones + ~300 knowledge tracking + ~200 datos de usuario (hobbies, personality, etc.).
- **Evaluación post-chat:** Al cierre, segundo call al LLM evalúa si hay nueva info personal para persistir (intereses detectados, preferencias implícitas).

**Technical Enrichment (Frontend)**
- **Archivos relevantes:** `src/modules/onboarding/` (flujo de onboarding con captura de preferencias — **ya existe**), `src/modules/onboarding/stores/onboarding.store.ts` (respuestas del onboarding), `src/modules/study-session/actions/send-message.action.ts` (construcción de contexto del chat), `src/modules/auth/stores/auth.store.ts` (datos del usuario).
- **Estado actual:** **Onboarding ya captura preferencias** (estilo de aprendizaje, intereses). No hay "student profile" enriquecido en frontend ni pantalla de perfil editable. No hay memory/summarization en frontend — es responsabilidad de backend/prompt.
- **Mayormente backend/prompt:** Frontend necesita: (1) pantalla de perfil/preferencias editable (nuevo módulo `src/modules/profile/`), (2) potencialmente enviar metadata adicional en requests al chat.
- **Reutilizable:** Componentes del onboarding adaptables para pantalla de edición de preferencias. Los componentes de chat no necesitan cambios — la personalización viene del backend.

---

### INT-014 · Widget se acuerde de mí

**Épica:** Integraciones / Canvas · **Prioridad:** media · **Estado:** to-do

**Como** estudiante usando el widget de Ada en Canvas,
**quiero** que el widget se acuerde de mí entre sesiones,
**para** no tener que empezar de cero cada vez que abro el chat.

**Descripción**

Hoy el widget de Canvas (INT-001 a INT-013) tiene un flujo de chat anónimo → registro → chat autenticado. Pero luego de la autenticación, el widget no persiste memoria del usuario (materias, conversaciones anteriores y demás). Esta historia agrega persistencia.

**Niveles de memoria:**

**1. Sin cuenta (anónimo)**
- Persistir en localStorage: últimos mensajes de la sesión anónima
- Al volver a la misma página, Ada retoma el hilo (hasta el límite de mensajes anónimos)

**2. Con cuenta (autenticado)**
- Persistir en backend: historial de conversaciones por materia
- Al abrir el widget luego de cierto tiempo (Igual a la definición que se tomen en Dash-013) se debe reiniciar la conversación, inyectando memoria del usuario.
- Saludo personalizado: "Hola de nuevo, [nombre]" (ya contemplado en INT-011)


**Criterios de aceptación**
- [ ] Usuario autenticado: al reabrir el widget, con menos tiempo del definido para la última interacción, visualiza el último chat de esa materia. Pasado ese tiempo, nuevo chat.
- [ ] Ada adapta su saludo según si hay historial previo o no
- [ ] El historial se asocia a la materia (no se mezclan conversaciones de distintas materias)


**Notas técnicas**
- Anónimo: localStorage con TTL (ej. 24h)
- Autenticado: endpoint para persistir/recuperar historial de chat del widget
- Considerar límite de historial (últimos N mensajes o último chat completo)
- Contexto: ver [INT-003 (chat anónimo)](/integraciones/canvas/historias), [INT-011 (nivel curso)](/integraciones/canvas/historias)

**Technical Enrichment (Backend)**
- **Resumption ya funciona parcialmente:** `EmbeddedChatSession.ID` se retorna en cada response como `session_id` (`EmbeddedChatResponse.SessionID`). El request acepta `SessionID *uuid.UUID` para resumir sesión existente. El backend carga historial completo de mensajes automáticamente.
- **TTL existente:** `sessionTTL = 12 * time.Hour` (constante en `src/core/usecases/embedded_chat/chat.go:18`) — rolling reset en cada mensaje. `user_id` almacenado en `embedded_chat_sessions` (migración 000042).
- **Lo que FALTA — endpoint de recovery:** No hay lookup por `user_id + course_code`. Solo existe `GetByIDWithMessages(id)`. Si el frontend pierde el `session_id` (localStorage borrado, otro browser), la sesión se pierde.
- **A crear:** (1) `GET /cronos/v1/embedded/chat/sessions?course_code=X` — recuperar última sesión activa por user + course. (2) Provider `GetActiveByUserAndCourse(ctx, userID, courseCode)`. (3) Index DB: `(user_id, course_code, deleted_at)`.
- **TTL para autenticados:** Considerar `authenticatedSessionTTL = 7 * 24h` (1 semana) vs 12h actual cuando `user_id != nil`.
- **"Nueva conversación":** Cerrar sesión actual + crear nueva. Reutilizar flow existente.
- **Locking:** `Lock()` es per-session-ID — recovery solo fetch, no lockea. Sin race conditions.
- **Dependencia con DASH-013:** Compartir TTL de reinicio. El `matched_concept_id` en sesión permite deep-link Canvas→Tich — preservar en recovery.

**Technical Enrichment (Frontend)**
- **Archivos relevantes:** No se encontró módulo de widget Canvas en `src/modules/`. El widget **probablemente vive en un repo separado** (integrado via iframe en Canvas LMS). Como referencia de patrones: `src/modules/study-session/stores/study-session.store.ts` (Zustand con `persist` middleware), `src/modules/auth/stores/auth.store.ts` (persist con localStorage).
- **Estado actual:** No hay código de widget Canvas en este repo. El patrón de persistencia Zustand con localStorage ya existe como referencia.
- **Aclaración importante:** Esta historia aplica a un widget que **vive fuera de tuni-ai-webapp**. El enriquecimiento técnico frontend debería validarse contra el repo del widget.
- **localStorage con TTL:** Zustand `persist` no tiene TTL nativo — se necesita middleware custom o check de timestamp al rehidratar.
- **Consistencia con DASH-013:** El timeout de reinicio de conversación debe ser consistente entre widget Canvas y tutor flotante. Definir constante compartida o config por tenant.

---

### INT-015 · [Producto] Tutor disponible en WhatsApp

**Épica:** Integraciones · **Prioridad:** baja · **Estado:** to-do

**Como** estudiante,
**quiero** tener a mi tutor disponible en WhatsApp,
**para** hacer consultas rápidas desde la app que ya uso todos los días.

**Descripción**

Canal de WhatsApp para interactuar con Ada. El alumno puede enviar mensajes a un número de WhatsApp y recibir respuestas del tutor, manteniendo la misma personalidad y conocimiento que en la plataforma web.

**Alcance propuesto (MVP):**
- Consultas rápidas (similar a DASH-013 tipo 1 y 3: materia + sistema)
- Respuestas basadas en RAG (contenido indexado)
- Identificación del alumno por número de teléfono vinculado a su cuenta TUNI
- Ada mantiene su personalidad (tono, voseo, reglas pedagógicas)

**Fuera de alcance (MVP):**
- Clases completas por WhatsApp (solo consultas)
- Actividades de comprobación (requieren UI interactiva)
- Evaluaciones
- Envío de archivos/imágenes

**Flujo propuesto:**
1. Alumno vincula su WhatsApp desde su perfil en TUNI (verificación por código)
2. Alumno envía mensaje al número de TUNI
3. Backend identifica al alumno, carga su contexto (materias, dominio, historial)
4. Ada responde con su personalidad habitual
5. Si la consulta amerita profundizar: "Para esto te conviene hacer una clase en TUNI: [link]"

**Criterios de aceptación**
- [ ] Existe un número de WhatsApp asociado a TUNI (WhatsApp Business API)
- [ ] El alumno puede vincular su número de teléfono a su cuenta TUNI
- [ ] Al enviar mensaje, Ada responde con su personalidad habitual
- [ ] Ada tiene contexto del alumno (materias inscriptas, nivel de dominio)
- [ ] Ada puede responder consultas académicas (RAG) y sobre el sistema
- [ ] Ada deriva a la plataforma web cuando la consulta requiere una clase o evaluación
- [ ] Mensajes del alumno se persisten como historial de consultas

**Notas técnicas**
- Requiere integración con WhatsApp Business API (Twilio, Meta Business API, o similar)
- El backend de chat es compartido con el tutor web (misma lógica de Ada, distinto canal)
- Rate limiting por usuario para controlar costos de API/LLM
- Considerar costos: cada mensaje de WhatsApp Business tiene costo por conversación
- Contexto: ver [Identity](/identity), [Contenido spec](/epicas/contenido/spec)

**Pendientes de definición:**
- ¿Proveedor de WhatsApp Business API? (Twilio, 360dialog, Meta directo)
- ¿Modelo de costos? (quién paga los mensajes, límite por alumno)
- ¿Vinculación de número: obligatoria o el alumno puede escribir sin cuenta?
- ¿Horario de disponibilidad o 24/7?

---

## Resumen de historias

| ID | Historia | Épica | Estado | Figma |
|----|----------|-------|--------|-------|
| DASH-007 | Métricas semanales | Dashboard | Existente — sin cambios | Si |
| DASH-008 | Racha diaria | Dashboard | Existente — **actualizar** con pantalla celebración | Si |
| DASH-010 | Side sheet detalle unidad | Dashboard | Existente — sin cambios | Si |
| CLS-220 | Estudiar una unidad específica | Clase | Nueva | — |
| CLS-221 | Cerrar clase sin terminar | Clase | Nueva | Si |
| CLS-222 | Ocultar actividades reportadas | Clase | Nueva | — |
| CLS-223 | Tutor ayuda a repasar | Clase | Nueva | — |
| CLS-224 | Modo desempeño | Clase | Nueva | — |
| PLAT-001 | Ambiente identidad de marca | Plataforma | Nueva | — |
| PLAT-002 | Funcionalidades a medida | Plataforma | Nueva | — |
| DASH-012 | TUNI en mobile | Dashboard | Nueva | Si |
| DASH-013 | Tutor flotante consultas | Dashboard | Nueva | — |
| CLS-225 | Tutor sepa de mí | Clase | Nueva | — |
| INT-014 | Widget se acuerde de mí | Integraciones | Nueva | — |
| INT-015 | Tutor en WhatsApp | Integraciones | Nueva | — |

**Total: 15 historias** (3 existentes a revisar + 12 nuevas)
