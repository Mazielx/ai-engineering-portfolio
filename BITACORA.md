# Bitácora de Desarrollo — Ian Maciel

> Registro de avance, decisiones de arquitectura y habilidades técnicas adquiridas en cada proyecto.
> Actualizada: 2026-09-15

---

## Resumen Ejecutivo

Desarrollador fullstack especializado en la construcción de productos SaaS de extremo a extremo: desde la especificación de producto y el diseño de arquitectura hasta la implementación, el hardening de seguridad y el deploy en producción.

**Stack principal:** Next.js (App Router) · React · TypeScript · Prisma · PostgreSQL · SQLite/Turso · React Native/Expo · Tailwind CSS · Docker · Vercel

**Lo que construyo:**
- Productos SaaS multi-tenant con aislamiento de datos por diseño
- Motores de renderizado y procesamiento propios (PDF, imágenes, optimización)
- Sistemas con billing, autenticación endurecida y APIs públicas versionadas
- Aplicaciones móviles con arquitectura hexagonal y motores de optimización puros

**Resultados verificables:**
- 6 productos SaaS construidos de extremo a extremo
- 240+ tests automatizados en el proyecto más maduro (RBAC, billing, políticas)
- Auditorías de seguridad con 67 vulnerabilidades corregidas
- Pruebas E2E de flujos completos de usuario (14/14 checks)
- Deploys continuos a producción en Vercel

> **Nota sobre repositorios:** el código fuente de los proyectos es privado para proteger la propiedad intelectual. Los proyectos se presentan mediante demos, documentación de arquitectura y case studies — disponibles bajo solicitud o NDA.

---

## Índice de Proyectos

| Proyecto | Repo | Stack principal | Estado |
|---|---|---|---|
| [Moirauder](#moirauder) | `Mazielx/moirauder` (privado) | Next.js 15, Turborepo, Prisma, PostgreSQL | En desarrollo |
| [Inkora](#inkora) | `Mazielx/inkora` (privado) | Next.js 16, Prisma, OpenCV, PDF | MVP funcional |
| [Facturas](#facturas) | `Mazielx/Facturas` (privado) | Next.js 16, SQLite/Turso, multi-tenant | MVP completo |
| [Shyftex](#shyftex) | `Mazielx/shyftex` (privado) | React Native/Expo, Prisma, PostgreSQL | En desarrollo |
| [Tenzi](#tenzi) | `Mazielx/tenzi` (privado) | Next.js 16, Prisma, Tailwind | Frontend + modelo de datos |
| [Ghork](#ghork) | `Mazielx/ghork` (privado) | Next.js 15, Prisma, Stripe | MVP + hardening |

---

## Moirauder

**Repo:** `github.com/Mazielx/moirauder` · **Branch:** `main`

### Concepto
Plataforma de modelado de probabilidades. Filosofía de producto: *"No predecimos el futuro. Lo modelamos."* El producto se diferencia de la predicción tradicional al exponer explícitamente el grado de incertidumbre de cada estimación.

### Stack
Next.js 15, React 19, Turborepo (monorepo con pnpm workspaces), Prisma, PostgreSQL, Tailwind, shadcn/ui.

### Decisiones de arquitectura
- **Monorepo con Turborepo**: separación clara entre `@moirauder/core-domain` (lógica de dominio pura, sin dependencias de UI) y `@moirauder/ui` (componentes de visualización).
- **Modelo de dominio de probabilidad**: `Probability` como tipo central con punto, intervalo y grado de incertidumbre — la incertidumbre es parte del modelo, no una nota al pie.
- **Visualizaciones con declaración de riesgo**: cada componente (probability strip, uncertainty gauge, diverging bars, pipeline stepper) incluye su grado de confianza y evidencia a favor/en contra.
- **Lazy loading de visualizaciones** para mantener el rendimiento de la landing page.

### Reglas de producto definidas
- HD-05: sin promesa de ganancias — el producto nunca sugiere resultados garantizados.
- LE-01/02/03: política de lenguaje diferida — el tono de las afirmaciones se calibra según la evidencia disponible.

### Habilidades
- Arquitectura monorepo (Turborepo + workspaces).
- Domain modeling con tipos de incertidumbre explícita.
- Diseño de componentes de visualización de datos con semántica de riesgo.
- Optimización de render en Next.js App Router.

---

## Inkora

**Repo:** `github.com/Mazielx/inkora` · **Branch:** `main`

### Concepto
Aplicación web que convierte documentos digitales en páginas que parecen escritas a mano con lápiz real, utilizando muestras de la escritura del usuario. No es una fuente genérica: es un motor de renderizado que reproduce la textura, presión y variación del trazo humano.

### Stack
Next.js 16, React 19, TypeScript, Prisma, PostgreSQL, OpenCV.js (procesamiento de imágenes), Tailwind, shadcn/ui.

### Decisiones de arquitectura
- **Motor de renderizado propio** (`src/engine/`): pipeline de parser → layout → selección de glifos → variación contextual → trazos → render de papel → exportador PDF.
- **Textura de grafito real**: ancho de trazo variable con ruido, capas superpuestas con alpha bajo — no una fuente digital disfrazada.
- **Perfiles de escritura**: el usuario sube muestras, revisa glifos y calibra su perfil; el motor usa esos glifos reales.
- **Exportación PDF vectorial multi-página** (no rasterización).
- **Hardening de autenticación**: CSRF, rate limiting, validación estricta en registro/login.

### Verificación
- Prueba E2E de flujo completo: registro → login → onboarding → perfil → muestra → glifos → documento → render → PDF → borrado (14/14 checks).
- Auditoría de aislamiento de datos (19/19 checks).

### Habilidades
- Procesamiento de imágenes con OpenCV (detección de páginas, vectorización).
- Renderizado vectorial de trazos y texturas.
- Exportación PDF programática.
- Pruebas E2E de flujos completos de usuario.

---

## Facturas

**Repo:** `github.com/Mazielx/Facturas` · **Branch:** `main`

### Concepto
Sistema de gestión de facturas con extracción automática desde Gmail. Arquitectura multi-tenant donde cada negocio tiene su propia base de datos aislada.

### Stack
Next.js 16 (App Router, Turbopack), React 19, TypeScript, Tailwind v4, SQLite vía Turso (`@libsql/client`), Vitest + Testing Library, ESLint 9, Recharts, Stripe, pdf-parse, fast-xml-parser, googleapis, bcrypt, nodemailer, xlsx-js-style.

### Decisiones de arquitectura
- **Multi-tenant con aislamiento físico**: DB principal (`data/main.db`) para negocios/usuarios/sesiones/api_keys + una DB por tenant (`data/negocios/{slug}/facturas.db`). Imposible el cruce de datos entre negocios por diseño.
- **Motor de extracción con scoring de confianza**: PDF y XML (formato español) con puntuación de confianza por campo extraído.
- **Detección de duplicados** y etiquetado automático.
- **API pública versionada** (`/api/v1`) para integraciones externas.
- **Monetización con Stripe**: checkout, webhooks, control de acceso por plan.
- **Exportación a Excel** (xlsx-js-style) con formato.

### Seguridad
- Auditoría de seguridad integral: 67 vulnerabilidades identificadas y corregidas (auth, IDOR, rate limiting, validación de entrada).
- Higiene de secretos: `.env` fuera de git, sin credenciales en historial.

### Habilidades
- Arquitectura multi-tenant con SQLite (aislamiento por tenant).
- Extracción de datos de PDF/XML con scoring de confianza.
- Integración Gmail API y Stripe.
- Auditoría y remediación de seguridad.
- Deploy continuo a Vercel.

---

## Shyftex

**Repo:** `github.com/Mazielx/shyftex` · **Branch:** `master`

### Concepto
Intelligent Shopping Optimizer: aplicación móvil que transforma una lista de compras en la estrategia de compra óptima — comparando precios entre tiendas, promociones, disponibilidad, ubicación, costos de transporte y presupuesto.

### Stack
React Native + Expo (TypeScript), Expo Router, Zustand, Fastify (backend), PostgreSQL + Prisma, Jest, Docker.

### Decisiones de arquitectura
- **Arquitectura hexagonal**: `domain/` (lógica de negocio pura sin dependencias externas), `infrastructure/` (implementaciones de proveedores), `optimization/` (motor de optimización en TypeScript puro).
- **Patrón de adapters/proveedores**: las integraciones externas (precios, inventario) se abstraen detrás de interfaces de puerto; los datos mock son proveedores sustituibles claramente identificados.
- **Motor de optimización puro**: cálculo de estrategia de compra considerando precios, promociones, disponibilidad, transporte y restricciones del usuario.
- **Regla de honestidad de datos**: nunca presentar datos simulados como reales — cada proveedor mock se marca explícitamente.

### Habilidades
- Desarrollo móvil con React Native/Expo.
- Arquitectura hexagonal y patrón de adapters.
- Diseño de motores de optimización en TypeScript.
- Docker para entornos de desarrollo reproducibles.

---

## Tenzi

**Repo:** `github.com/Mazielx/tenzi` · **Branch:** `main`

### Concepto
Plataforma SaaS para la gestión integral de arrendamientos inmobiliarios. Centraliza la operación de arrendamiento de inicio a fin, conectando administradores, inmobiliarias, asesores, propietarios, inquilinos y personal jurídico.

### Stack
Next.js 16 (App Router), TypeScript, Tailwind CSS v4, Radix UI, Prisma, SQLite, NextAuth.

### Decisiones de arquitectura
- **Modelo de dominio centrado en la Operación**: `PROPIEDAD → PROPIETARIO → INQUILINO → SOLICITUD → INVESTIGACIÓN → REVISIÓN → APROBACIÓN → CONTRATO → FIRMA → ARRENDAMIENTO ACTIVO → SEGUIMIENTO → FINALIZACIÓN`.
- **Máquina de estados en Prisma**: el ciclo de vida del expediente se modela con estados válidos y transiciones restringidas en el schema.
- **Frontend completo de alta fidelidad**: dashboard con métricas, wizard de operaciones (5 pasos), portafolio de propiedades, perfiles de clientes, investigación de inquilinos con score visual, gestor documental, contratos, cobranza, panel jurídico, reportes y portales para arrendador/arrendatario/fiador.
- **Tema claro/oscuro** con tokens de diseño semánticos y gráficos SVG propios (sin librerías de charts).

### Estado
Frontend completo y modelo de datos diseñado (schema Prisma + migraciones + seed). Backend API en desarrollo.

### Habilidades
- Diseño de modelos de datos complejos con Prisma (máquinas de estado, restricciones).
- Diseño de sistemas con múltiples roles de usuario (6+ perfiles).
- Componentes UI accesibles con Radix + shadcn patterns.
- Diseño de sistemas de diseño (tokens, temas).

---

## Ghork

**Repo:** `github.com/Mazielx/ghork` · **Branch:** `master`

### Concepto
Plataforma SaaS B2B de descubrimiento de procesos y automatización de negocio. Detecta el "ghost work" (trabajo operativo repetitivo invisible), reconstruye los procesos que lo generan, cuantifica su costo e impacto y los transforma en automatizaciones seguras y medibles.

Flujo del producto: **OBSERVE → UNDERSTAND → DISCOVER → EXPLAIN → PROPOSE → VALIDATE → APPROVE → AUTOMATE → EXECUTE → MEASURE**

### Stack
Next.js 15 (App Router, Turbopack), React 19, TypeScript, Prisma 6, PostgreSQL (Neon serverless), Tailwind, shadcn-style UI, Stripe, Vitest, PWA.

### Decisiones de arquitectura
- **Pipeline de descubrimiento de procesos**: del trabajo observado a la automatización ejecutable, con validación y aprobación humana en cada etapa.
- **Billing robusto**: cancelación revoca acceso inmediato (CANCELED + canceledAt, sin período de gracia), plan ENTERPRISE no auto-provisionable, race de doble suscripción resuelto con advisory lock.
- **Rate limiting anti-spoofing**: `clientIp()` usa `x-vercel-forwarded-for` / última entrada de XFF.
- **Límites de uso atómicos**: `updateMany` condicional — sin race check-then-act.
- **Worker resiliente**: reclaim de jobs RUNNING huérfanos por lease timeout.
- **Auth endurecida**: scrypt N=2^16 + maxmem (4x más fuerte), dummy hash anti-enumeración en login, rate limit de registro.
- **Seguridad de aplicación**: CSP headers, validación de uploads (10 MB), error boundaries y loading states, sidebar filtrado por rol.

### Verificación
- 240 tests: RBAC, billing/access, policies/engine, usage-limits.
- Config de Prisma migrada a `prisma.config.ts` con postinstall para builds en Vercel.

### Habilidades
- Hardening de seguridad en producción (billing, rate limiting, auth).
- Manejo de condiciones de carrera con advisory locks y operaciones atómicas.
- Testing de políticas de negocio (RBAC, billing, usage limits).
- Deploy a Vercel con config de Prisma correcta.

---

## Habilidades Transversales

1. **Desarrollo fullstack con Next.js** — App Router, Server Components, API routes, Turbopack.
2. **Modelado de datos con Prisma** — schemas complejos, máquinas de estado, migraciones, multi-tenant.
3. **Bases de datos** — PostgreSQL, SQLite/Turso, aislamiento multi-tenant, FTS5.
4. **Seguridad** — auditorías integrales, hardening de auth, rate limiting, CSP, manejo de secretos.
5. **Testing** — Vitest, Testing Library, tests E2E de flujos completos, tests de políticas de negocio.
6. **Deploy y DevOps** — Vercel, Docker, variables de entorno, CI de calidad.
7. **Arquitectura de software** — monorepos, hexagonal, adapters/proveedores, domain modeling.
8. **Diseño de producto** — especificaciones, reglas de negocio, criterios de aceptación, honestidad de datos.
9. **Desarrollo móvil** — React Native/Expo, Expo Router, Zustand.
10. **Documentación técnica** — READMEs profesionales, docs de arquitectura, guías de desarrollo.

---

*Bitácora de desarrollo profesional. Cada proyecto documenta decisiones de arquitectura, resultados y habilidades técnicas adquiridas.*