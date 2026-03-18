# Sprint 32 — Planning de historias

> Fecha: 2026-03-18

> Estado: Draft — en definición
---

## Norte del sprint

> "Lo más prioritario es que el usuario que entre tenga una buena experiencia, le guste y haga muchas clases. Para que use mucho necesitamos que tenga más satisfacción. No está teniendo satisfacción en terminar una clase, está costando terminar una clase."

### KPIs — Primeros 15 días en producción (baseline pre-Sprint 32)

**Chat sessions (= clases)**

| Métrica | Valor | % |
|---------|-------|---|
| Total creadas | 175 | — |
| Llegaron al modo enseñar | 101 | ~58% |
| Con al menos una study session | 66 | ~38% |
| Con al menos una study session completa | 48 | ~27% |
| Completadas | 13 | **~7.5%** |

**Usuarios en chat sessions**

| Métrica | Valor | % |
|---------|-------|---|
| Usuarios que crearon una chat session | 92 | — |
| Usuarios que completaron una chat session | 8 | **~8.7%** |

**Study sessions (= set de actividades dentro de una clase)**

| Métrica | Valor | % |
|---------|-------|---|
| Study sessions creadas | 89 | — |
| Study sessions completadas | 61 | **~69%** |
| Usuarios que realizaron una study session | 52 | — |
| Usuarios que completaron una study session | 40 | **~77%** |

**Lectura clave:** Los alumnos completan las actividades (69-77% de study sessions) pero no completan la clase (7.5% de chat sessions). El problema no es la actividad individual — es que la clase no cierra. Esto valida directamente la Prioridad 1: reestructurar la clase en pasos con duración predecible.

### Prioridades (en orden)

| # | Prioridad | Épica Jira | Por qué |
|---|-----------|------------|---------|
| 1 | **Mejorar la clase** | [TUNI-1007](https://aula-educabot.atlassian.net/browse/TUNI-1007) | Los alumnos no terminan las clases. Si es necesario, todo el equipo se dedica a esto. |
| 2 | **Nivelación / diagnóstico rápido** | [TUNI-1085](https://aula-educabot.atlassian.net/browse/TUNI-1085) | El sistema necesita adaptarse rápido al nivel del alumno. "Como un personal trainer que te hace un test de fuerza." |
| 3 | **Modo examen** | [TUNI-1017](https://aula-educabot.atlassian.net/browse/TUNI-1017) | "Llegó el examen, quiero estudiar para el examen." Muy peleado con nivelación en prioridad. |
| 4 | **Memoria** | [TUNI-1019](https://aula-educabot.atlassian.net/browse/TUNI-1019) | Importante pero sacrificable. Buscar forma simple de aplicar rápido. |

---

## Carryover del Sprint 31

### En curso (arrastran)

| Ticket | Historia | Épica | Asignado | Relación Sprint 32 |
|--------|----------|-------|----------|---------------------|
| [TUNI-1043](https://aula-educabot.atlassian.net/browse/TUNI-1043) | Botón "Ahora no": retomar contexto | Mejoras Clase | Seba | Prioridad 1 — Mejorar la clase |
| [TUNI-1001](https://aula-educabot.atlassian.net/browse/TUNI-1001) | Tutor que sepa de mí — preferencias e historial | Memoria | Leo | Prioridad 4 — Memoria |
| [TUNI-1070](https://aula-educabot.atlassian.net/browse/TUNI-1070) | Bug: redirect a chat-session da error | Estabilización | Alejo | Bloqueante UX |
| [TUNI-1073](https://aula-educabot.atlassian.net/browse/TUNI-1073) | Herramienta QA: respuestas en consola | QA | Leo | Herramienta interna |

### Por subir (liberar primero)

| Ticket | Historia | Asignado |
|--------|----------|----------|
| [TUNI-1063](https://aula-educabot.atlassian.net/browse/TUNI-1063) | Chat vacío al ingresar primera vez | Joaquin |

### Bugs pendientes (afectan experiencia de clase)

| Ticket | Historia | Prioridad | Impacto en Sprint 32 |
|--------|----------|-----------|----------------------|
| [TUNI-1067](https://aula-educabot.atlassian.net/browse/TUNI-1067) | Widget redirige a materia equivocada | High | Bloquea integración |
| [TUNI-1045](https://aula-educabot.atlassian.net/browse/TUNI-1045) | Complete phrase no matchea expected result | High | **Afecta Prioridad 1** — Actividad rota = clase mala |
| [TUNI-1047](https://aula-educabot.atlassian.net/browse/TUNI-1047) | Feedback en inglés para MC | Medium | **Afecta Prioridad 1** — Experiencia inconsistente |
| [TUNI-1044](https://aula-educabot.atlassian.net/browse/TUNI-1044) | Historial de chats múltiples materias | Medium | Confusión de contexto |
| [TUNI-960](https://aula-educabot.atlassian.net/browse/TUNI-960) | LLM en compare answers no procesa bien | Medium | **Afecta Prioridad 1** — Actividad rota |
| [TUNI-950](https://aula-educabot.atlassian.net/browse/TUNI-950) | Propiedad finished no capturada | Medium | **Afecta Prioridad 1** — Actividades que no corresponden |
| [TUNI-947](https://aula-educabot.atlassian.net/browse/TUNI-947) | Al llegar a nivel 4 no se marcó check | Medium | **Afecta Prioridad 1** — Progresión rota |

### Tareas pendientes (no son bugs)

| Ticket | Historia | Épica | Asignado |
|--------|----------|-------|----------|
| [TUNI-1083](https://aula-educabot.atlassian.net/browse/TUNI-1083) | Mejora UI Onboarding + Product Tour | Mobile | Joaquin |
| [TUNI-1080](https://aula-educabot.atlassian.net/browse/TUNI-1080) | Mejora UI Dashboard materia | Mobile | Joaquin |
| [TUNI-1079](https://aula-educabot.atlassian.net/browse/TUNI-1079) | Mejora UI Home Mobile | Mobile | Joaquin |
| [TUNI-1078](https://aula-educabot.atlassian.net/browse/TUNI-1078) | Conectar front con endpoints | Dashboard | Joaquin |
| [TUNI-1058](https://aula-educabot.atlassian.net/browse/TUNI-1058) | Front — historial chats | Estabilización | Joaquin |
| [TUNI-1057](https://aula-educabot.atlassian.net/browse/TUNI-1057) | Back — historial chats | Estabilización | Alejo |
| [TUNI-1000](https://aula-educabot.atlassian.net/browse/TUNI-1000) | Tutor flotante para consultas generales | Mejoras Clase | Alejo |
| [TUNI-1002](https://aula-educabot.atlassian.net/browse/TUNI-1002) | Widget se acuerde de mí | Memoria | Leo |
| [TUNI-956](https://aula-educabot.atlassian.net/browse/TUNI-956) | Implementar getConceptInfo con contenido indexado | Mejoras Clase | Leo |
| [TUNI-1029](https://aula-educabot.atlassian.net/browse/TUNI-1029) | Persistir estado del product tour | Perfil | Alejo , e
---

## Prioridad 1: Mejorar la clase

> "Lo principal es que terminen una clase. Ahí vamos a estar trabajando fuerte, si es necesario vamos a meter todo el equipo para lograr una experiencia mucho mejor, más cómoda, más rápida, más clara."

**Diagnóstico:** Los alumnos no completan las clases. Los puntos de fricción principales son:
- Actividades con errores que frustran (bugs en complete phrase, compare answers, feedback en inglés)
- Sesiones demasiado largas o poco claras en su progreso
- Falta de sensación de cierre y recompensa
- El alumno no entiende cuánto falta para terminar

### TUNI-1087 · Arreglo integral de actividades rotas

**Épica:** Mejoras Clase · **Prioridad:** crítica · **Estado:** to-do
**Jira:** [TUNI-1087](https://aula-educabot.atlassian.net/browse/TUNI-1087)

**Como** alumno,
**quiero** que todas las actividades funcionen correctamente
**para** no frustrarme durante la clase con errores que no son míos.

**Descripción**

Ticket paraguas que agrupa todos los bugs de actividades que degradan la experiencia de clase. Es la base para que cualquier mejora posterior tenga sentido — no sirve mejorar el flujo si las actividades están rotas.

**Bugs incluidos:**
- [TUNI-1045](https://aula-educabot.atlassian.net/browse/TUNI-1045): Complete phrase no matchea expected result
- [TUNI-1047](https://aula-educabot.atlassian.net/browse/TUNI-1047): Feedback en inglés para MC
- [TUNI-960](https://aula-educabot.atlassian.net/browse/TUNI-960): LLM en compare answers no procesa bien respuestas
- [TUNI-950](https://aula-educabot.atlassian.net/browse/TUNI-950): Propiedad finished no capturada — genera actividades que no corresponden
- [TUNI-947](https://aula-educabot.atlassian.net/browse/TUNI-947): Al llegar a nivel 4 no se marcó check en plan

**Criterios de aceptación**
- [ ] Complete phrase: la respuesta del alumno matchea correctamente con el expected result
- [ ] Feedback de MC se muestra en español, nunca en inglés
- [ ] Compare answers: el LLM procesa y evalúa correctamente las respuestas del alumno
- [ ] Propiedad `finished` de la study session se captura para evitar actividades que no corresponden
- [ ] Al llegar a nivel 4, el plan se marca como completado visualmente (check)

**Notas técnicas**
- Ver [Comprobación spec](/epicas/clase/comprobacion) para tipos de actividad y reglas
- Ver [Knowledge Tracking](/epicas/clase/knowledge-tracking) para sistema de niveles
- Los bugs de TUNI-960 y TUNI-1045 son de procesamiento LLM en backend (tich-cronos)
- TUNI-1047 es probablemente un problema de locale en el prompt o en la generación

---

### TUNI-1088 · Clase basada en pasos con duración predecible

**Épica:** Mejoras Clase · **Prioridad:** alta · **Estado:** to-do
**Jira:** [TUNI-1088](https://aula-educabot.atlassian.net/browse/TUNI-1088)
**Fuente:** Reuniones Juan + Rocío (16/03/2026, dos sesiones)
**Referencia UX:** Escuela app (step-by-step chat con progreso visual)

**Como** alumno,
**quiero** que la clase tenga pasos claros y una duración predecible,
**para** saber en qué momento estoy, cuánto me falta y sentir que la terminé.

**Descripción**

**Problema detectado (Mixpanel + observación):** Los alumnos no están terminando las clases. Un alumno estuvo 40 minutos, hizo 2 sesiones de actividades y aún así no completó la clase. Hay usuarios que vuelven (señal de que el producto les sirve), pero la experiencia de clase genera fricción por ser impredecible.

**Causa raíz:** Hoy la clase se completa cuando el alumno **sube de nivel** (objetivo de resultado). Esto significa que la duración es completamente variable — un alumno puede terminar en 10 minutos y otro puede estar 3 horas en la misma clase. El alumno no sabe cuánto le falta, no tiene visibilidad de progreso, y la sensación es "esto no termina más".

> "Hay que reducir el costo de terminar una clase." — Juan
> "Lo que no funciona es que sea dinámico y que no se termine en algún momento." — Rocío
> "En Duolingo empiezan muy fácil para que la sesión sea fácil y te venga la serotonina de las estrellitas. Terminaste rápido, encima te subió la autoestima. Y hacés otra." — Rocío

**Cambio fundamental: de resultado a esfuerzo**

| Aspecto | Hoy (resultado) | Propuesta (esfuerzo) |
|---------|-----------------|----------------------|
| Completar clase | Subir N niveles de dominio | Completar N pasos |
| Duración | Variable (10 min a 3+ horas) | Predecible (~10 min) |
| Visibilidad | Plan de 3 pastillas (conceptos) | Steps visibles: "Paso 2 de 4" |
| Si no subís de nivel | La clase no termina | La clase termina igual; la próxima retoma donde quedaste |
| Progresión de niveles | Dentro de la clase | **Entre clases** (si no subiste, la próxima clase sigue con el mismo tema) |

**Estructura de la clase por pasos (propuesta de Rocío)**

La clase se compone de pasos secuenciales. La cantidad varía según el tiempo disponible:

| Paso | Nombre | Qué pasa | Bloom |
|------|--------|----------|-------|
| 1 | **Elección** | "¿Conocés este tema?" + "¿Cuánto tiempo tenés?" → define cantidad de pasos | — |
| 2 | **Lección** | Introducción breve al concepto. Ada explica, muestra ejemplos. El alumno puede pedir más ejemplos o hacer consultas. Se skipea si el alumno dice que ya sabe. | Recordar/Comprender |
| 3 | **Ejercicio** | Actividades de práctica: MC, fill blanks. Verificar comprensión de lo recién visto. | Recordar/Comprender |
| 4 | **Refuerzo adaptativo** | Basado en lo que salió mal en el ejercicio: re-explicar, más ejemplos, tips específicos. Si todo salió bien, se skipea o es breve. | Comprender/Aplicar |
| 5 | **Desafío** | Actividades de mayor nivel: preguntas abiertas, comparar respuestas. Solo en clase "larga". | Analizar/Evaluar |
| 6 | **Resumen** | Cierre: qué vimos, qué aprendimos, resumen descargable. CTAs para continuar. | — |

**Adaptación según tiempo disponible:**

| Tiempo | Pasos | Descripción |
|--------|-------|-------------|
| "Poco tiempo" (~5 min) | 3 | Lección breve → Ejercicio rápido → Resumen |
| "Un rato" (~10 min) | 4 | Lección → Ejercicio → Refuerzo adaptativo → Resumen |
| "Para profundizar" (~15-20 min) | 5-6 | Todos los pasos incluyendo Desafío |

**Progresión visual (referencia: Escuela app)**

El chat muestra steps visibles que se van completando (check) a medida que el alumno avanza. El alumno sabe en todo momento en qué paso está y cuántos le faltan. Referencia directa de Escuela app donde el chat marca "Paso 1 completado ✓" y muestra "Paso 2: Ejercicio".

**Qué pasa con los niveles de dominio**

Los niveles (Knowledge Tracking) **no desaparecen** — siguen siendo el motor adaptativo del sistema. Pero dejan de ser la condición de completado de la clase:
- Si el alumno subió de nivel durante la clase → genial, se refleja en el cierre
- Si no subió de nivel → la clase termina igual, y la próxima clase retoma el mismo tema con el mismo nivel
- El sistema sigue trackeando dominio, pero el alumno siente que "terminó" su sesión de estudio

> "Termino la clase, pero no subí de nivel, y la próxima clase va a ser de lo que todavía no entendiste." — Rocío

**Criterios de aceptación**
- [ ] La clase se estructura en pasos visibles (steps) que el alumno ve en el chat
- [ ] Cada paso se marca como completado visualmente cuando termina
- [ ] El alumno elige tiempo disponible al inicio ("poco tiempo" / "un rato" / "para profundizar")
- [ ] La cantidad de pasos se adapta al tiempo elegido (3 / 4 / 5-6)
- [ ] La clase se completa al terminar los pasos, independientemente de si subió de nivel
- [ ] Si el alumno no subió de nivel, la próxima clase retoma el mismo concepto
- [ ] La sesión promedio dura ~10 minutos para "un rato"
- [ ] El finish rate de clases sube significativamente respecto al baseline actual
- [ ] El paso de "Refuerzo adaptativo" se basa en los errores del ejercicio previo
- [ ] (Futuro) Resumen descargable al final de la clase (PDF/texto)

**Notas técnicas**
- **Cambio de modelo en backend:** El umbral de completado hoy es "subir N niveles" con un hardcode de 6 actividades (`register_activities_in_session_study.go:234`). Reemplazar por un modelo de pasos fijos cuya cantidad depende del tiempo elegido.
- **buildPlan:** Hoy genera plan basado en dominio de conceptos. Debe generar plan basado en pasos, donde cada paso tiene un tipo (lección, ejercicio, refuerzo, desafío, resumen).
- **Progreso visual:** Frontend necesita renderizar steps con estado (pendiente / actual / completado). Referencia: Escuela app.
- **Lógica de "Elección" (Paso 1):** Ya existe la pregunta "¿Cómo venís con este tema?" (M1 en spec actual). Agregar selección de tiempo. Si dice que sabe → skipear Lección → ir directo a Ejercicio.
- **Refuerzo adaptativo (Paso 4):** Analizar errores del Paso 3 y generar explicación focalizada. Ya existe lógica similar en M3 (fin comprobación) del spec actual.
- **Niveles siguen trackeándose:** `KnowledgeTracking.DomainLevel` sigue actualizándose durante la clase. Lo que cambia es que la clase no bloquea su cierre por no alcanzar un nivel.
- **Resumen descargable:** Usuarios ya piden PDFs y resúmenes (dato de Mixpanel — un alumno pidió resumen). Para V1 puede ser texto; para V2, PDF descargable.
- Contexto: [Clase spec](/epicas/clase/spec), [Knowledge Tracking](/epicas/clase/knowledge-tracking), [Comprobación](/epicas/clase/comprobacion)

**Insights adicionales de las reuniones**
- Hay ~54 usuarios activos, algunos vuelven (señal positiva de product-market fit)
- Un usuario estudió a las 4am, otro a las 7am — hay engagement real
- Rocío está diseñando prototipo de esta propuesta para validar con Pau (pedagoga)
- Hay deuda técnica entre diseño y frontend: inconsistencias Figma → código que ralentizan la iteración
- Rocío propone sesión de alineación con Joaquin y Juli para mejorar el flujo diseño → implementación

---

## Prioridad 2: Nivelación / diagnóstico rápido

> "Que podamos hacer de alguna manera un test rápido, como lo hace un personal trainer, que te hace un test de fuerza. Para que vos puedas saber cómo estás y en base a eso armar un plan personalizado. No te voy a dar dificultad básica si sos bueno y no te voy a poner cosas superdifíciles si no."

**Contexto:** La nivelación fue planteada en el [Onboarding spec](/epicas/onboarding/spec) como un modal post-onboarding con opción de "Hacer nivelación" o "Comenzar ya". Hasta ahora la opción de nivelación no se implementó. El flujo de nivelación completo está "pendiente de definición" según la spec. Este sprint es el momento de definirlo e implementarlo.

### TUNI-1089 · Diagnóstico rápido de nivel por materia

**Épica:** Nivelación Inicial ([TUNI-1085](https://aula-educabot.atlassian.net/browse/TUNI-1085)) · **Prioridad:** alta · **Estado:** to-do
**Jira:** [TUNI-1089](https://aula-educabot.atlassian.net/browse/TUNI-1089)

**Como** alumno,
**quiero** hacer un test rápido al empezar una materia para que el sistema sepa mi nivel,
**para** que las clases se adapten a lo que ya sé y no pierda tiempo en cosas básicas.

**Descripción**

Test de diagnóstico corto (~5-8 preguntas) que permite al sistema calibrar el nivel inicial del alumno en una materia. Inspirado en el modelo de "placement test" o "test de fuerza" del personal trainer.

**Flujo propuesto:**
1. **Trigger**: primera vez que el alumno entra a una materia (o explícitamente desde el modal de nivelación del onboarding)
2. **Presentación**: Ada explica brevemente: "Antes de empezar, vamos a hacer unas preguntas rápidas para conocer tu nivel y armar un plan a tu medida."
3. **Test adaptativo**: 5-8 preguntas de dificultad creciente (empieza en nivel 2, si acierta sube a nivel 3, si falla baja a nivel 1). Se detiene cuando converge.
4. **Resultado**: Ada muestra resumen visual: "En esta materia arrancás en nivel X. Tus puntos fuertes son A y B, y vamos a trabajar en C."
5. **Efecto en sistema**: Se setean los `DomainLevel` iniciales por concepto/unidad según resultado del test. El plan de clase posterior se genera con estos niveles como base.

**Criterios de aceptación**
- [ ] Al entrar por primera vez a una materia, se ofrece diagnóstico rápido (no obligatorio)
- [ ] El test tiene 5-8 preguntas adaptativas (corta duración, < 5 minutos)
- [ ] Las preguntas se adaptan en dificultad según respuestas previas
- [ ] Al finalizar, se muestra resumen visual del nivel detectado
- [ ] Los niveles de dominio iniciales se setean en el sistema basándose en el resultado
- [ ] Las clases posteriores arrancan desde el nivel detectado, no desde cero
- [ ] Si el alumno elige saltear el diagnóstico, arranca desde nivel 1 (comportamiento actual)

**Diseño del test adaptativo (propuesta):**

```
Pregunta 1 → Nivel 2 (Comprender)
  ├── Acierta → Pregunta 2 → Nivel 3 (Aplicar)
  │     ├── Acierta → Pregunta 3 → Nivel 4 (Evaluar)
  │     │     ├── Acierta → Nivel inicial: 4 (dominio alto)
  │     │     └── Falla  → Nivel inicial: 3 (comprensión aplicada)
  │     └── Falla  → Pregunta 3b → Nivel 2 (confirmar)
  │           ├── Acierta → Nivel inicial: 2 (básico confirmado)
  │           └── Falla  → Nivel inicial: 1 (sin conocimiento)
  └── Falla  → Pregunta 2b → Nivel 1 (Recordar)
        ├── Acierta → Nivel inicial: 1 (conocimiento mínimo)
        └── Falla  → Nivel inicial: 1 (sin conocimiento)
```

La estructura se repite por cada unidad clave de la materia (2-3 unidades muestreadas), totalizando 5-8 preguntas.

**Notas técnicas**
- Las preguntas del diagnóstico pueden venir del pool de actividades existentes (por nivel de Bloom), filtradas por unidad
- Nuevo tipo de sesión: `session_type: "diagnostic"` o `session_type: "placement"`
- Resultado alimenta `KnowledgeTracking.DomainLevel` por concepto — setear niveles iniciales en batch
- No genera puntos ni afecta racha (es diagnóstico, no clase)
- El modal de nivelación del onboarding ([Onboarding spec](/epicas/onboarding/spec)) ya existe — conectar con este flujo
- Ver [Knowledge Tracking](/epicas/clase/knowledge-tracking) para sistema de niveles
- Ver [Comprobación](/epicas/clase/comprobacion) para tipos de actividad por nivel de Bloom

---

### TUNI-1090 · Adaptación de dificultad según nivel detectado

**Épica:** Modelo Estudiante ([TUNI-905](https://aula-educabot.atlassian.net/browse/TUNI-905)) · **Prioridad:** alta · **Estado:** to-do
**Jira:** [TUNI-1090](https://aula-educabot.atlassian.net/browse/TUNI-1090)

**Como** sistema,
**quiero** que las clases se adapten al nivel detectado por el diagnóstico,
**para** no aburrir a alumnos avanzados ni frustrar a principiantes.

**Descripción**

Hoy el sistema empieza siempre desde nivel 1 para un concepto nuevo, independientemente del conocimiento previo del alumno. Con el diagnóstico, el alumno puede arrancar en nivel 2 o 3, y las actividades deben reflejar eso.

**Cambios necesarios:**
1. `buildPlan` debe respetar el `DomainLevel` inicial (si viene del diagnóstico, no empieza en 1)
2. `getActivities` debe filtrar actividades por nivel de Bloom correspondiente al `DomainLevel` del alumno
3. Ada debe adaptar su discurso al nivel: con un alumno nivel 3, no explica desde cero

**Criterios de aceptación**
- [ ] Si el alumno tiene nivel inicial 3 en un concepto, las actividades empiezan en Aplicar/Analizar (no en Recordar)
- [ ] El plan de sesión refleja los niveles iniciales correctamente
- [ ] Ada adapta su tono y profundidad según el nivel del alumno
- [ ] Un alumno que hizo diagnóstico tiene una experiencia notablemente distinta a uno que no lo hizo

**Notas técnicas**
- `buildPlan` (tool call) ya recibe contexto del estudiante — agregar niveles iniciales al contexto
- `getActivities(filter: "session")` debe considerar `DomainLevel` para seleccionar nivel de Bloom adecuado
- Prompt de Ada en M1 (inicio): si `DomainLevel > 1`, saltear relevamiento básico y proponer comprobación directa
- Ver [Clase spec — Matriz de Momentos](/epicas/clase/spec#consideraciones-técnicas)

---

## Prioridad 3: Modo examen

> "Che, llegó el examen, quiero estudiar para el examen. El modo examen es clave."

**Contexto:** Ya existe un overview en [Modo desempeño](/epicas/modo-desempeno/overview) con la visión general. Este sprint es para definir las historias concretas e iniciar implementación.

### TUNI-1091 · Modo examen: sesión de práctica intensiva

**Épica:** Modo Desempeño ([TUNI-1017](https://aula-educabot.atlassian.net/browse/TUNI-1017)) · **Prioridad:** alta · **Estado:** to-do
**Jira:** [TUNI-1091](https://aula-educabot.atlassian.net/browse/TUNI-1091)
**Historia Jira previa:** [TUNI-996](https://aula-educabot.atlassian.net/browse/TUNI-996)

**Como** alumno,
**quiero** un modo de estudio intensivo para preparar un parcial,
**para** practicar los temas del examen y saber si estoy listo para rendir.

**Descripción**

Modo diferenciado de estudio que se activa cuando el alumno quiere prepararse para un examen. A diferencia de la clase normal (que avanza progresivamente), el modo examen es pragmático: evalúa rápido, identifica debilidades y drilla los temas flojos.

**Flujo propuesto:**
1. **Entry point**: CTA "Preparar examen" en el dashboard de materia (nuevo) o en el side sheet de módulo
2. **Selección de alcance**: "¿Qué módulos entran en el examen?" — multi-select de módulos
3. **Diagnóstico express**: 3-4 preguntas rápidas por módulo para identificar nivel actual (reutiliza lógica de NIV-001)
4. **Plan de estudio**: Ada arma plan priorizado: "Tenés 3 temas débiles. Empezamos por el más importante."
5. **Loop intensivo**: Por cada tema débil: explicación condensada → 2-3 actividades de nivel examen (Analizar/Evaluar) → feedback directo
6. **Cierre**: "Nota estimada: X/10. Temas fuertes: A, B. Seguí practicando: C."

**Diferencias con clase normal:**

| Aspecto | Clase normal | Modo examen |
|---------|-------------|-------------|
| Selección de temas | Por dominio (sistema decide) | Por temario de examen (alumno elige) |
| Profundidad | Completa (nivel 1→4) | Foco en debilidades, skip lo dominado |
| Actividades | Progresivas por Bloom | Nivel alto (Analizar, Evaluar) — tipo examen |
| Tono de Ada | Paciente, exploratoria | Directa, "modo coach", enfocada |
| Duración | ~10 min, 4 actividades | ~15-20 min, cubre múltiples temas |
| Feedback | Formativo, 60/40 | Orientado a rendimiento, "en un examen esto costaría X puntos" |

**Criterios de aceptación**
- [ ] Existe entry point "Preparar examen" accesible desde dashboard de materia
- [ ] El alumno puede seleccionar qué módulos entran en el examen
- [ ] Se ejecuta diagnóstico express para detectar nivel por módulo
- [ ] Ada arma plan de estudio priorizado por debilidades
- [ ] Las actividades son de nivel Analizar/Evaluar (tipo examen)
- [ ] El tono de Ada es directo y enfocado ("modo coach")
- [ ] Al cierre se muestra estimación de rendimiento + recomendaciones
- [ ] El modo examen genera puntos y afecta racha (es estudio real)

**Notas técnicas**
- Nuevo `session_type: "exam_prep"` en study session
- Reutilizar lógica de diagnóstico de NIV-001 para fase de detección
- Filtrar actividades por nivel de Bloom >= Analizar (niveles 4-6)
- Prompt de Ada específico para modo coach (nuevo prompt, no reutilizar M1-M4 tal cual)
- Entry point: nuevo CTA en dashboard, posiblemente vinculado a "Próximo parcial" de las `MateriaCard`
- Ver [Modo Desempeño overview](/epicas/modo-desempeno/overview), [Comprobación](/epicas/clase/comprobacion), [Contenido spec](/epicas/contenido/spec)

---

## Prioridad 4: Memoria (forma simple — MVP / paso 0)

> "Me gustaría encontrar una forma simple de aplicar memoria rápido."

**Contexto:** Ya existen dos historias en Jira para esta épica. La directiva del sprint es: encontrar la versión más simple que aporte valor. No es la prioridad principal, pero si se puede hacer algo rápido, sumar.

### TUNI-1001 · Tutor que sepa de mí — Preferencias e historial (MVP)

**Épica:** Memoria ([TUNI-1019](https://aula-educabot.atlassian.net/browse/TUNI-1019)) · **Prioridad:** media-baja · **Estado:** en curso (Leo)
**Jira:** [TUNI-1001](https://aula-educabot.atlassian.net/browse/TUNI-1001)

**Como** alumno,
**quiero** que Ada se acuerde de mis errores frecuentes y mis intereses,
**para** que las clases se sientan personalizadas y no tenga que repetir contexto.

**Scope Sprint 32 (paso 0 — lo más simple posible):**

Inyectar en cada llamada al LLM un bloque de contexto del alumno construido con datos que **ya existen** en el sistema. Sin tablas nuevas, sin migraciones, sin persistencia adicional.

**Datos ya disponibles (sin desarrollo nuevo):**
- Nombre y hobbies (del onboarding — tabla `users`, campos `nickname`, `hobbies`)
- Errores frecuentes por concepto (de `KnowledgeTracking` + historial de actividades)
- Nivel de dominio por unidad/concepto (de `KnowledgeTracking.DomainLevel`)
- Materias y unidades cursadas (de inscripciones)
- Cantidad de clases realizadas (de `ChatSession` con status `Finished`)

**Implementación (la más simple):**
1. Al iniciar sesión de chat, el backend genera un bloque de contexto "perfil del alumno":
   ```
   Alumno: {nombre}, intereses: {hobbies}.
   Nivel en esta unidad: {domainLevel}/4.
   Errores frecuentes en esta unidad: {top 3 conceptos con más fallos}.
   Clases realizadas en esta materia: {count}.
   ```
2. Este bloque se inyecta en el system prompt antes del contexto de la sesión actual.
3. Ada usa esta info para personalizar saludos, ejemplos y explicaciones.

**Criterios de aceptación**
- [ ] Ada saluda al alumno por nombre y hace referencia a intereses cuando es natural
- [ ] Ada menciona errores previos al explicar un tema: "La última vez te costó X, vamos a enfocarnos en eso"
- [ ] Ada adapta ejemplos usando los hobbies del alumno cuando es posible
- [ ] El contexto inyectado no supera 200 tokens para no impactar latencia ni costo
- [ ] No se persiste nada nuevo — solo se lee lo que ya existe

**Notas técnicas**
- 100% backend: construir bloque de contexto en `CreateChatSession` o en el armado del prompt
- Sin nuevas tablas ni migraciones — todo se lee de entidades existentes
- Ver [Memoria overview](/epicas/memoria/overview), [Identity de Ada](/identity) para tono

---

### TUNI-1002 · Widget se acuerde de mí (diferido)

**Épica:** Memoria ([TUNI-1019](https://aula-educabot.atlassian.net/browse/TUNI-1019)) · **Prioridad:** baja · **Estado:** por hacer (Leo)
**Jira:** [TUNI-1002](https://aula-educabot.atlassian.net/browse/TUNI-1002)

**Como** alumno,
**quiero** que el widget embebido en Canvas recuerde mi conversación anterior,
**para** no tener que repetir contexto cuando navego entre páginas.

**Scope Sprint 32:** Diferido. Este ticket depende de que TUNI-1001 funcione bien primero. Si TUNI-1001 se completa y queda margen en el sprint, evaluar si se puede hacer una versión mínima (ej: persistir el último `chatSessionId` del widget para retomarlo). Sino, queda para Sprint 33.

---

## Resumen de historias y asignación propuesta

### Prioridad 1 — Mejorar la clase (todo el equipo si es necesario)

| Ticket | Historia | Tipo | Asignación sugerida |
|--------|----------|------|---------------------|
| [TUNI-1087](https://aula-educabot.atlassian.net/browse/TUNI-1087) | Arreglo integral de actividades rotas | Bug fix | Leo (backend), Joaquin (frontend) |
| [TUNI-1088](https://aula-educabot.atlassian.net/browse/TUNI-1088) | Clase más corta y con progreso visible | Feature | Backend + Frontend |
| [TUNI-1043](https://aula-educabot.atlassian.net/browse/TUNI-1043) | Botón "Ahora no" (continuación) | Feature | Seba (en curso) |

### Prioridad 2 — Nivelación

| Ticket | Historia | Tipo | Asignación sugerida |
|--------|----------|------|---------------------|
| [TUNI-1089](https://aula-educabot.atlassian.net/browse/TUNI-1089) | Diagnóstico rápido de nivel por materia | Feature | Backend + Frontend |
| [TUNI-1090](https://aula-educabot.atlassian.net/browse/TUNI-1090) | Adaptación de dificultad según nivel | Feature | Backend (LLM/prompts) |

### Prioridad 3 — Modo examen

| Ticket | Historia | Tipo | Asignación sugerida |
|--------|----------|------|---------------------|
| [TUNI-1091](https://aula-educabot.atlassian.net/browse/TUNI-1091) | Modo examen: sesión de práctica intensiva | Feature | Backend + Frontend + UX |

### Prioridad 4 — Memoria (MVP / paso 0)

| Ticket | Historia | Tipo | Asignación |
|--------|----------|------|------------|
| [TUNI-1001](https://aula-educabot.atlassian.net/browse/TUNI-1001) | Tutor que sepa de mí — inyectar contexto en prompt | Feature (MVP) | Leo (en curso) |
| [TUNI-1002](https://aula-educabot.atlassian.net/browse/TUNI-1002) | Widget se acuerde de mí | Feature (diferido) | Leo (si queda margen) |

### Bugs / carryover

| Ticket | Impacto |
|--------|---------|
| TUNI-1063 | Chat vacío primera vez — por subir, liberar |
| TUNI-1070 | Redirect chat-session — bloqueante, en curso |
| TUNI-1067 | Widget materia equivocada — integración |
| TUNI-1044 + 1058 + 1057 | Historial chats múltiples materias (bug + subtareas front/back) |
| TUNI-1083, 1080, 1079 | Mejoras UI mobile (onboarding, dashboard, home) — Joaquin |

---

## Dependencias y riesgos

| Riesgo | Mitigación |
|--------|-----------|
| Prioridad 1 consume todo el sprint | Aceptable según directiva de Juan: "si es necesario metemos todo el equipo" |
| Nivelación y modo examen compiten por prioridad | Juan dice "muy peleado" — definir en sprint planning cuál arranca primero |
| Diagnóstico necesita actividades por nivel de Bloom | Las actividades ya existen por tipo, pero no todas las materias tienen cobertura completa |
| Modo examen necesita UX nueva | Coordinar con Ro para diseño antes de implementar |
| Memoria depende de que el backend tenga datos limpios | Usar solo datos del onboarding + KnowledgeTracking — ya validados |

---

## Referencias

| Recurso | Link |
|---------|------|
| Clase — Spec | [/epicas/clase/spec](/epicas/clase/spec) |
| Clase — Comprobación | [/epicas/clase/comprobacion](/epicas/clase/comprobacion) |
| Knowledge Tracking | [/epicas/clase/knowledge-tracking](/epicas/clase/knowledge-tracking) |
| Onboarding — Spec (nivelación) | [/epicas/onboarding/spec](/epicas/onboarding/spec) |
| Modo Desempeño — Overview | [/epicas/modo-desempeno/overview](/epicas/modo-desempeno/overview) |
| Memoria — Overview | [/epicas/memoria/overview](/epicas/memoria/overview) |
| Identity de Ada | [/identity](/identity) |
| Contenido — Spec | [/epicas/contenido/spec](/epicas/contenido/spec) |
| Sprint 31 — Planning | [/sprint31-planning.md](/sprint31-planning.md) |
| Roadmap | [/producto/roadmap](/producto/roadmap) |
