# CLAUDE.md

This file provides guidance to Claude Code when working with this repository.

## Project Overview

TUNI (Tutoría Universitaria Inteligente) — documentation and specification site built with **Mintlify**. Contains product specs, epic definitions, user stories, and technical docs for an adaptive university tutoring platform powered by an AI tutor called "Ada".

## Spec-Driven Development

This repo is the **source specification** for TUNI. Specs are NOT passive documentation — they are contracts that drive all downstream work (implementation, tests, tooling).

- **Specs MUST reduce ambiguity**: A developer or coding agent reading a spec should be able to implement without guessing. If something can be interpreted two ways, the spec is incomplete.
- **Every spec MUST include**: behavior description, constraints/rules, and acceptance criteria.
- **Spec changes come BEFORE code changes**: Update the spec first, then implement. Never let code drift from its spec.
- **Specs are testable artifacts**: Acceptance criteria in a spec should map directly to test cases.

## Commands

```bash
mint dev            # Preview locally → http://localhost:3000
mint broken-links   # Check for broken links
mint update         # Update Mintlify CLI
```

## Architecture

- **`docs.json`** — Mintlify config and navigation structure. Source of truth for site nav.
- **`epicas/`** — Product epics, each in its own subdirectory:
  - `overview.mdx` — Epic summary and scope
  - `spec.mdx` — Detailed specification (the primary artifact)
  - `historias.mdx` — User stories
  - `decisiones.mdx` — Architecture/design decisions
- **`integraciones/`** — External integrations (Canvas LMS, etc.)
- **`identity.mdx`** — Ada's personality, voice, tone, and pedagogical behavior
- **`mixpanel-tracking.mdx`** — Analytics event tracking plan

### Epics

`clase` (adaptive tutoring session), `onboarding`, `dashboard`, `evaluaciones`, `contenido`, `supervisores`, `perfil`, `comunidad`, `mobile`

## Terminology

MUST use these terms consistently — never substitute synonyms:

| Term | Meaning | NOT |
|------|---------|-----|
| Ada | The AI tutor | "el bot", "la IA", "el asistente" |
| Alumno | The student user | "usuario", "estudiante" (in UI copy) |
| Clase | An adaptive tutoring session | "sesión", "lección" |
| Supervisor | Professor/instructor role | "profesor", "docente" (in specs) |
| Actividad | A learning activity within a clase | "ejercicio", "tarea" |
| Comprobación | Knowledge check phase | "evaluación rápida", "quiz" |

## Writing Conventions

- Language: **Spanish** (Argentine — "vos" form: "conocé", "configurá")
- Active voice, second person
- Sentence case for headings
- Bold for UI elements, code formatting for file names/commands/paths

## Gotchas

- **MUST register new pages in `docs.json`** — Pages not in navigation won't appear on the site
- **`.mintignore`** excludes files from publishing — drafts and AGENTS.md are excluded
- **Deployment is automatic** — Pushes to default branch auto-deploy via Mintlify's GitHub integration
- **`identity.mdx` is a core product reference** — Changes here affect Ada's behavior spec across all epics
