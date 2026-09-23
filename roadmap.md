# 🔥 DATAFORGE: LA CAMPAÑA DEFINITIVA
### Data Platform Engineer + IA Aplicada — Versión Fusionada

Un solo proyecto que evoluciona nivel a nivel. Al final tenés un sistema real, documentado, con capa de IA — no seis repos sueltos.

---

## 🧠 REGLAS DE INTERACCIÓN CON IA

**1. Copiloto, no autopiloto.** Nunca copies código que no puedas explicar línea por línea.

**2. Usá la IA para 3 cosas específicas, no para todo:**
- *Desbloqueo puntual:* "Mi consumer falla con error de concurrencia, dame 3 causas posibles" (no "arreglame esto").
- *Crítica de diseño:* "Acá está mi diagrama de arquitectura. Actuá como Staff Engineer y destruí mi diseño — decime qué falla en escala." Esto es oro: te entrena a pensar en fallas antes de que un entrevistador te las señale.
- *Boilerplate:* configuración inicial, mocks de test, Dockerfiles base — lo repetitivo, no lo que requiere criterio.

**3. Intento previo de 15-20 min** antes de consultar IA en cualquier problema nuevo.

**4. Reimplementación sin IA:** una vez por nivel, reescribí una pieza pequeña (una función, un consumer) desde cero, sin mirar tu código anterior ni usar IA. Es tu chequeo real de retención.

**5. Explicación Feynman al cierre de cada nivel:** explicá en voz alta cómo funciona todo el sistema hasta ese punto, como si se lo enseñaras a alguien. Donde te trabás, ahí tenés el hueco.

**6. Quiz semanal generado por IA** sobre lo que construiste esa semana, respondido sin mirar el código.

---

## 📜 REGLAS DE EMPRESA TIER 1 (mundo real)

- **ADRs por decisión importante:** contexto → opciones consideradas → decisión → consecuencias. Los escribís vos, sin IA.
- **TDD pragmático:** toda lógica de negocio con tests unitarios + integración. Cobertura mínima 80%. Si la IA generó el código, vos escribís los tests (o al revés) — nunca los dos hechos por IA.
- **Infraestructura como código:** nada se instala a mano. Todo vía Docker/Docker Compose.
- **Git flow real:** nunca directo a `main`. Rama por feature, Conventional Commits (`feat:`, `fix:`, `refactor:`, `docs:`), y Pull Request — usá la IA como revisor estricto de ese PR pidiéndole que exija estándares de Clean Code.
- **Idempotencia:** toda operación de API o pipeline debe poder reintentarse sin duplicar efectos. Es lo primero que un entrevistador senior te va a preguntar sobre tu diseño.
- **Observabilidad desde el día 1:** logs estructurados, métricas básicas, trazabilidad entre servicios. No es un "extra" al final, es una capa que crece con cada nivel.

---

## 🗺️ NIVEL 0 — Cimientos
**Tiempo: 3-4 días**

- Repo con estructura de Clean Architecture (capas: dominio / aplicación / infraestructura).
- Docker Compose base (Postgres levantado).
- CI básico en GitHub Actions (corre tests en cada push, aunque estén vacíos todavía).
- Primer ADR: "Por qué esta estructura de carpetas y no otra."

**Condición de victoria:** `docker compose up` levanta todo sin tocar nada a mano.

---

## 🗺️ NIVEL 1 — El Despertar del Monolito (Ingesta Crítica)
**Tiempo: 2-2.5 semanas**

**Conceptos:** SOLID, Clean Architecture, TDD, Estructuras de Datos y Big O, **Idempotencia**.

**Misión:** API robusta que recibe eventos de telemetría de dispositivos IoT (simulados) a alto volumen.

**Desafíos:**
- Diseño con capa web, lógica de negocio y acceso a datos completamente desacoplados (aplicá al menos 3 principios SOLID de forma justificada, no forzada).
- Validación estricta de payloads.
- **Cada evento debe ser idempotente** — si el mismo evento llega dos veces, no duplica efectos (usá un ID único + verificación).
- Persistencia en PostgreSQL.

**Condición de victoria:** API soporta prueba de carga básica (ej. con `k6` o `locust`), cobertura de tests ≥80% en lógica de negocio, ADR de la estructura del proyecto.

---

## 🗺️ NIVEL 2 — El Laberinto Asíncrono (Desacoplamiento)
**Tiempo: 2.5-3 semanas**

**Conceptos:** Mensajería (Kafka/RabbitMQ), Inyección de Dependencias, **Saga Pattern**, **CQRS**, Dead Letter Queues.

**Misión:** Postgres empieza a colapsar bajo presión. Desacoplás con mensajería.

**Desafíos:**
- Refactorizá con **Inyección de Dependencias** para poder cambiar el destino de guardado sin tocar la lógica central.
- Productor publica eventos en una cola (Kafka o RabbitMQ).
- Consumidor independiente (script en Python) procesa y transforma los eventos.
- Implementá **Dead Letter Queue** para mensajes que fallan repetidamente.
- Si tu flujo involucra más de un paso que puede fallar a mitad de camino (ej. "guardar evento" + "notificar"), diseñalo como una **Saga** simple con compensación.
- Empezá a separar comandos de consultas (**CQRS** básico): un modelo para escribir, otro optimizado para leer.

**Condición de victoria:** procesamiento asíncrono demostrable, manejo robusto de fallos (podés matar el consumer a mitad de proceso y el sistema se recupera sin perder ni duplicar datos), ADR explicando la decisión Kafka vs RabbitMQ.

---

## 🗺️ NIVEL 3 — El Leviatán de Datos (Analytics + Observabilidad)
**Tiempo: 2.5-3 semanas**

**Conceptos:** Modelado de datos (Star Schema/Data Vault), almacenamiento columnar, orquestación, **Observabilidad** (logs, métricas, trazabilidad distribuida).

**Misión:** Los datos limpios del Nivel 2 se consolidan para análisis de negocio.

**Desafíos:**
- Almacenamiento columnar (DuckDB, ClickHouse, o Parquet local) con un esquema justificado (Star Schema o Data Vault — ADR explicando por qué).
- Pipeline que aplique estructuras algorítmicas reales (diccionarios, sets, grafos) para detectar anomalías o agrupar eventos similares — no solo un `SELECT`.
- Orquestación automatizada (cron nativo o Airflow en Docker).
- **Observabilidad real:** logs estructurados en todos los servicios (API, consumer, pipeline), métricas básicas expuestas (Prometheus-style o simples endpoints `/metrics`), y trazabilidad de un evento desde que entra a la API hasta que aparece en el reporte final.

**Condición de victoria:** un dashboard o script que genera reporte estadístico leyendo millones de filas simuladas en segundos, todo en contenedores, y podés trazar un evento específico de punta a punta usando tus logs/métricas.

---

## 🗺️ NIVEL 4 — La Capa de IA (tu diferencial real)
**Tiempo: 2-2.5 semanas**

**Conceptos:** RAG, vector databases, function calling/agentes, MLOps básico.

**Misión:** Agregale inteligencia al sistema que ya construiste. Esto es lo que te separa de un Data Platform Engineer "clásico".

**Desafíos:**
- Construí un sistema RAG: documentá tu propio proyecto (los ADRs que ya escribiste, por ejemplo) y armá un chatbot que responda preguntas sobre TU arquitectura basándose en esos documentos.
- Agregale una herramienta vía function calling que consulte directamente los datos del Nivel 3 ("¿cuántas anomalías detectamos esta semana?" → el agente consulta tu warehouse y responde).
- Versioná el pipeline de embeddings como código (no algo que corriste una vez a mano).

**Condición de victoria:** el agente responde correctamente preguntas sobre tu propio sistema Y sobre los datos que procesa. Es la demo más impactante de toda la campaña.

---

## 👑 BOSS FINAL — Integración y Producción
**Tiempo: 1.5-2 semanas**

- Todo el sistema (Niveles 1-4) corriendo con `docker compose up`, con CI/CD corriendo tests en cada push.
- README general con diagrama de arquitectura completo (todos los componentes y cómo se comunican).
- Los ADRs de cada nivel consolidados.
- Deploy real en un servicio gratuito (Railway, Fly.io, Render) — aunque sea una versión reducida.

**Condición de victoria:** alguien que nunca vio el proyecto puede clonarlo, levantarlo, y entenderlo leyendo solo el README.

---

## ⏱️ TIEMPO TOTAL — versión honesta

| Ritmo | Horas/semana | Duración |
|---|---|---|
| Sostenible (con facultad) | 12-15h | **11-13 semanas (~3 meses)** |
| Intenso (dedicación fuerte) | 25-30h | **8-9 semanas** |

No prometo menos porque los temas de Nivel 2 y 3 (Saga, CQRS, DLQ, observabilidad distribuida) son genuinamente densos — la IA te acelera la implementación, no el tiempo que tu cerebro necesita para que estos conceptos queden fijados. Si comprimís de más saltándote las reglas de reimplementación y explicación Feynman, terminás con el repo pero sin poder defenderlo en la entrevista, que es el punto que falla más gente.

---

## 🎯 AL TERMINAR

Cuando completes el Boss Final (o querés ir nivel por nivel, avisame igual), decime "terminé DataForge" y hago de **líder técnico en una entrevista de diseño de sistemas completa**: preguntas de arquitectura sobre tus propias decisiones, código en vivo, y preguntas comportamentales. Si la pasás, armamos la estrategia real de búsqueda de empleo con este proyecto como pieza central del portafolio.

---

## 📌 Nota: proyecto de práctica anterior

Antes de DataForge, se completó un proyecto de práctica en un repo separado (`.../dataforge`, no este): una Task Manager API (FastAPI, TDD, refactor SOLID — Dependency Inversion y Single Responsibility — con Repository pattern), siguiendo un roadmap anterior ("The Engineer Quest"). Ese trabajo queda cerrado y documentado en su propio repo. DataForge es un proyecto nuevo desde Nivel 0, en este repo.
