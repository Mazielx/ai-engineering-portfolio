# Bitácora de Desarrollo — Ian Maciel

> Registro de avance, decisiones y habilidades adquiridas en cada proyecto.
> Actualizada: 2026-09-15

---

## Índice de Proyectos

| Proyecto | Repo | Stack principal | Estado |
|---|---|---|---|
| [Moirauder](#moirauder) | `Mazielx/moirauder` | Next.js 15, Turborepo, Prisma, PostgreSQL | En desarrollo |
| [Inkora (ex-Grafito)](#inkora-ex-grafito) | `Mazielx/inkora` | Next.js 16, Prisma, OpenCV, PDF | En desarrollo |
| [Grydex / Kapta (Facturas)](#grydex--kapta-facturas) | `Mazielx/Facturas` | Next.js 16, SQLite/Turso, multi-tenant | MVP completo |
| [Shyftex](#shyftex) | `Mazielx/shyftex` | React Native/Expo, Prisma, PostgreSQL | En desarrollo |
| [Tenzi](#tenzi) | `Mazielx/tenzi` | Next.js 16, Prisma, Vercel | MVP completo |
| [Ghork](#ghork) | `Mazielx/ghork` | Next.js 15, Prisma, Stripe | MVP + hardening |
| [Agent Factory](#agent-factory) | — (sin repo) | OpenCode agents | Base del ecosistema |

---

## Moirauder

**Repo:** `github.com/Mazielx/moirauder` · **Local:** `~/moirauder` · **Branch:** `main`

### Qué es
Plataforma de modelado de probabilidades. Filosofía: *"No predecimos el futuro. Lo modelamos."* Producto nuevo desarrollado desde cero con mentalidad de producto real, no demo.

### Stack
Next.js 15, React 19, Turborepo (monorepo), Prisma, PostgreSQL, Tailwind, shadcn/ui, pnpm.

### Avances
- Monorepo con Turborepo: `apps/web` + paquetes `@moirauder/core-domain` y `@moirauder/ui`.
- Landing page con visualización de probabilidad: strip de probabilidad, gauge de incertidumbre, barras divergentes, pipeline stepper, guardian note.
- Lazy loading de visualizaciones (`LazyVisualization`) para performance.
- Modelo de dominio con `Probability` (punto, intervalo, grado de incertidumbre).
- Reglas de producto documentadas (HD-05: sin promesa de ganancias, LE-01/02/03).

### Habilidades adquiridas
- Arquitectura monorepo con Turborepo y workspaces de pnpm.
- Diseño de dominio (domain modeling) con tipos de probabilidad e intervalos.
- Componentes de visualización de datos con declaraciones de riesgo.
- Lazy loading y optimización de render en Next.js App Router.

---

## Inkora (ex-Grafito)

**Repo:** `github.com/Mazielx/inkora` (renombrado desde `grafito` el 2026-09-15) · **Local:** `~/proyectos/grafito` · **Branch:** `main`

### Qué es
Aplicación web que convierte documentos digitales en páginas que parecen escritas a mano con lápiz real, usando muestras de escritura del usuario. Motor de renderizado de documentos manuscritos personalizados.

### Stack
Next.js 16, React 19, Prisma, PostgreSQL, OpenCV (procesamiento de imágenes), PDF, Docker, Tailwind, shadcn/ui.

### Avances
- Motor de render de lápiz (`src/engine/`): parser, layout, selección de glifos, variación contextual, trazos, render de papel, exportador.
- Textura de grafito real: ancho de trazo variable con ruido, capas con alpha bajo.
- Perfiles de escritura: subir muestras, revisar glifos, calibración.
- API de muestras (samples) con validación, revisión de glifos mejorada.
- Hardening de auth: CSRF, rate limiting, registro/login robustos.
- Pruebas E2E reales: `grafito-flujo-check.mjs` 14/14 checks (registro→login→onboarding→perfil→muestra→glifos→documento→render→PDF→borrado).
- Rebranding completo Grafito → Inkora (README, docs, email, paquetes `@inkora/*`).

### Habilidades adquiridas
- Procesamiento de imágenes con OpenCV (detección de páginas, vectorización).
- Renderizado vectorial de trazos (no fuentes genéricas).
- Exportación PDF vectorial multi-página.
- Pruebas E2E de flujos completos de usuario.
- Rebranding de proyecto (nombre, paquetes, docs, email).

---

## Grydex / Kapta (Facturas)

**Repo:** `github.com/Mazielx/Facturas` · **Local:** `~/proyectos/facturas` · **Branch:** `main`

### Qué es
Sistema de gestión de facturas con extracción automática desde Gmail. Multi-tenant: cada negocio con su propia base de datos aislada. También conocido como Kapta (antes "En Regla").

### Stack
Next.js 16 (App Router, Turbopack), React 19, Tailwind v4, SQLite vía Turso (`@libsql/client`), TypeScript, Vitest + Testing Library, ESLint 9, Recharts, Stripe, pdf-parse, fast-xml-parser, googleapis, bcrypt, nodemailer, xlsx-js-style.

### Avances
- Arquitectura multi-tenant: DB principal (`data/main.db`) con negocios/usuarios/sesiones/api_keys + DBs tenant (`data/negocios/{slug}/facturas.db`) con aislamiento completo.
- Motor de extracción PDF/XML con scoring de confianza.
- Detección de duplicados, etiquetas, exportación Excel.
- Integración Gmail API, Stripe checkout, webhooks.
- API pública versionada (`/api/v1`).
- 11 módulos de API routes, panel de administración, dashboard con charts.
- 8 sesiones de pentest, 67 vulnerabilidades corregidas.
- ~15 deploys a Vercel.

### Habilidades adquiridas
- Arquitectura multi-tenant con SQLite (aislamiento por tenant).
- Extracción de datos de PDF/XML con scoring de confianza.
- Integración con Gmail API y Stripe.
- Seguridad: auditorías de pentest y remediación sistemática.
- Deploy continuo a Vercel.

---

## Shyftex

**Repo:** `github.com/Mazielx/shyftex` · **Local:** `~/proyectos/shopping-optimizer` · **Branch:** `master`

### Qué es
Intelligent Shopping Optimizer: aplicación móvil de optimización de compras (comparador de precios, cupones, ofertas, supermercados) con recomendaciones explicadas.

### Stack
React Native/Expo, Prisma, PostgreSQL, Docker, Vercel, Supabase.

### Avances
- Especificación maestra de producto + ingeniería (fases 0-20).
- Arquitectura de proveedores/adapters correctamente abstraída (datos mock solo como proveedor sustituible identificado).
- Regla anti-mock: nunca presentar datos simulados como reales.
- Docker para PostgreSQL local, Prisma con migraciones.
- Rediseño de identidad visual.

### Habilidades adquiridas
- Desarrollo móvil con React Native/Expo.
- Arquitectura de adapters/proveedores para integraciones externas.
- Reglas de honestidad de producto (no-fake-functionality).
- Trabajo por fases con criterios de aceptación (FASE 0-20).

---

## Tenzi

**Repo:** `github.com/Mazielx/tenzi` · **Local:** `~/tenzi` · **Branch:** `main`

### Qué es
Plataforma SaaS para gestión integral de arrendamientos inmobiliarios. Conecta administradores, inmobiliarias, asesores, propietarios, inquilinos y personal jurídico.

### Stack
Next.js 16, React 19, Prisma, SQLite, Tailwind, Vercel, Stripe.

### Avances
- Maqueta frontend de alta fidelidad → MVP completo por fases (A-H).
- Schema de Prisma de 816 líneas (diseñado con Oracle).
- Flujo de registro completo (con subagentes JARVIS).
- Orquestación multi-agente extensiva: brief en archivo → lanzar agente → verificación independiente → checkpoint git.
- Deploys a Vercel.

### Habilidades adquiridas
- Diseño de modelos de datos complejos con Prisma.
- Orquestación de agentes especializados (JARVIS, Oracle, Vendetta, Koro).
- Trabajo por fases con exit criteria.
- Recuperación de dev server (zombies de `next-server`, SQLITE_READONLY).

---

## Ghork

**Repo:** `github.com/Mazielx/ghork` · **Local:** `~/ghork` · **Branch:** `master`

### Qué es
Plataforma SaaS B2B de descubrimiento de procesos y automatización de negocio. Detecta el "ghost work" (trabajo operativo repetitivo), reconstruye los procesos, cuantifica su costo e impacto y los transforma en automatizaciones ejecutables. Flujo: OBSERVE → UNDERSTAND → DISCOVER → EXPLAIN → PROPOSE → VALIDATE → APPROVE → AUTOMATE → EXECUTE.

### Stack
Next.js 15, React 19, Prisma, PostgreSQL, Stripe, Docker, Tailwind, shadcn/ui, Vercel.

### Avances
- MVP completo: process discovery & automation platform.
- Hardening de seguridad, billing y confiabilidad tras auditoría:
  - Billing: cancelar revoca acceso inmediato (CANCELED+canceledAt), ENTERPRISE no auto-provisionable, race de doble suscripción resuelto con advisory lock.
  - Rate limiting anti-spoofing (`x-vercel-forwarded-for`).
  - Límites de uso atómicos (updateMany condicional).
  - Worker con reclaim de jobs RUNNING huérfanos por lease timeout.
  - scrypt N=2^16 + maxmem; dummy hash anti-enumeración en login.
  - CSP headers, validación de uploads (10 MB), error boundaries.
- 240 tests (rbac, billing/access, policies/engine, usage-limits).
- Config de Prisma migrada a `prisma.config.ts`, postinstall para builds en Vercel.

### Habilidades adquiridas
- Hardening de seguridad en producción (billing, rate limiting, auth).
- Manejo de races con advisory locks y operaciones atómicas.
- Tests de seguridad y políticas (RBAC, billing, usage limits).
- Deploy a Vercel con config de Prisma correcta.

---

## Agent Factory

**Repo:** — (sin repo, no requiere push) · **Local:** `~/.config/opencode/`

### Qué es
La sesión fundacional donde se creó el ecosistema de agentes de OpenCode. El usuario (vibecoder) pidió un agente que combinara desarrollador senior, pentester, ingeniero IA y DevOps para compensar su falta de conocimiento técnico.

### Avances
- Creación del agente `mega-expert` global.
- Evolución al ecosistema actual: Computadora (Master Intelligence) + JARVIS (orquestador) + 12 especialistas (Pickle Rick, Vendetta, Morty, Koro, Morpheus, Zoldyck, House, Smith, Oracle, Sheldon, Higuruma, Diane).
- Configuración global en `~/.config/opencode/`.
- Skills de orquestación y procesos repetibles (ver sección de Skills).

### Habilidades adquiridas
- Configuración de OpenCode: agentes, skills, plugins, harness.
- Ingeniería de prompts para agentes especializados.
- Orquestación multi-agente.
- **Harness engineering**: convertir procesos repetidos en skills reutilizables.

---

## Habilidades Transversales Adquiridas

1. **Desarrollo fullstack con Next.js** (App Router, Server Components, API routes) — en todos los proyectos.
2. **Modelado de datos con Prisma** — schemas de 800+ líneas, migraciones, multi-tenant.
3. **Bases de datos** — PostgreSQL (Docker), SQLite/Turso, aislamiento multi-tenant.
4. **Seguridad ofensiva y defensiva** — pentest con Vendetta, hardening, rate limiting, CSP, auth.
5. **Testing** — Vitest, Testing Library, tests E2E de flujos completos, tests de políticas.
6. **Deploy y DevOps** — Vercel (deploy manual y auto), Docker, variables de entorno.
7. **Orquestación de agentes IA** — briefs, verificación independiente, checkpoints git.
8. **Documentación-driven development** — specs como fuente de verdad, docs que reflejan la realidad.
9. **Ingeniería de prompts** — prompts efectivos para generación de código, debugging y arquitectura.
10. **Harness engineering** — procesos repetibles como skills del ecosistema.

---

## Procesos Repetidos → Skills del Harness

Los procesos que se repitieron en múltiples proyectos se convirtieron en skills reutilizables en `~/.config/opencode/skills/`:

| Skill | Proceso que encapsula |
|---|---|
| `superagent-orchestration` | Delegación obligatoria a superagentes en cada tarea |
| `fullstack-app-scaffold` | Bootstrap de app fullstack (Next.js/Expo + Prisma + Docker) |
| `phase-based-workflow` | Trabajo por fases con exit criteria |
| `verify-before-finish` | Puerta de verificación (tsc + test + lint + build) |
| `security-audit` | Auditoría de seguridad estilo Vendetta |
| `multi-agent-orchestration` | Orquestación de agentes especialistas |
| `vercel-deploy` | Deploy manual a Vercel |
| `no-fake-functionality` | Regla anti-mock / honestidad de producto |
| `local-postgres-docker` | PostgreSQL local en Docker (puerto 5433) |
| `e2e-http-smoke-test` | Smoke tests por HTTP/curl con sesión real |
| `secrets-hygiene` | Higiene de secretos (.env, git log -S) |
| `prisma-data-model` | Diseño de modelo de datos Prisma |
| `branding-design-overhaul` | Rediseño de identidad visual |
| `dev-server-recovery` | Recuperación de dev server zombi |
| `production-readme-docs` | Documentación de entrega (README + docs) |
| `session-context-transfer` | Migración de contexto entre sesiones |

---

*Bitácora mantenida por Computadora + especialistas. Los procesos repetidos se convierten en skills para hacer el desarrollo repetible y profesional.*