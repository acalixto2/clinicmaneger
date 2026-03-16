# Workspace

## Overview

pnpm workspace monorepo using TypeScript. Each package manages its own dependencies.

## Stack

- **Monorepo tool**: pnpm workspaces
- **Node.js version**: 24
- **Package manager**: pnpm
- **TypeScript version**: 5.9
- **API framework**: Express 5
- **Database**: PostgreSQL + Drizzle ORM
- **Validation**: Zod (`zod/v4`), `drizzle-zod`
- **API codegen**: Orval (from OpenAPI spec)
- **Build**: esbuild (CJS bundle)
- **Frontend**: React + Vite, TailwindCSS, shadcn/ui, React Query, Wouter, Recharts, react-hook-form

## Project: Gestão Clínica Médica

A complete Brazilian medical clinic management system with laboratory.

### Features
- **Dashboard**: Today's appointments, exams, and revenue overview
- **Patients**: CRUD with CPF, birthdate, insurance, full history
- **Professionals**: CRUD with specialty, CRM, schedule days/hours
- **Rooms**: CRUD with types (consultório, coleta, ultrassom, laboratório)
- **Appointments**: Scheduling with conflict prevention (same professional/room/time)
- **Exam Catalog**: Lab exams with categories and pricing
- **Exam Orders**: Order exams for patients, link to appointments
- **Financial**: Payment records (PIX, cartão, dinheiro, convênio)
- **Cash Closing**: Daily report with revenue breakdown by payment method

## Structure

```text
artifacts-monorepo/
├── artifacts/
│   ├── api-server/         # Express API server (all routes)
│   └── clinica/            # React+Vite frontend (clinic management UI)
├── lib/
│   ├── api-spec/           # OpenAPI spec + Orval codegen config
│   ├── api-client-react/   # Generated React Query hooks
│   ├── api-zod/            # Generated Zod schemas from OpenAPI
│   └── db/                 # Drizzle ORM schema + DB connection
├── scripts/
├── pnpm-workspace.yaml
├── tsconfig.base.json
├── tsconfig.json
└── package.json
```

## Database Schema (PostgreSQL via Drizzle)

Tables:
- `patients` — patient records
- `professionals` — healthcare staff
- `rooms` — clinic rooms/offices
- `appointments` — medical appointments (conflict-checked)
- `exams` — lab exam catalog
- `exam_orders` — exam orders per patient
- `financial_records` — payment/financial records

## API Routes (all under /api)

- `GET/POST /api/patients` — list/create patients
- `GET/PUT/DELETE /api/patients/:id` — get/update/delete patient (with history)
- `GET/POST /api/professionals` — list/create professionals
- `GET/PUT/DELETE /api/professionals/:id`
- `GET/POST /api/rooms` — list/create rooms
- `PUT/DELETE /api/rooms/:id`
- `GET/POST /api/appointments` — list/create appointments (conflict-checked)
- `GET/PUT/DELETE /api/appointments/:id`
- `GET/POST /api/exams` — list/create exam catalog
- `PUT/DELETE /api/exams/:id`
- `GET/POST /api/exam-orders` — list/create exam orders
- `PUT /api/exam-orders/:id` — update status
- `GET/POST /api/financial` — list/create financial records
- `PUT /api/financial/:id`
- `GET /api/dashboard` — dashboard summary data
- `GET /api/cash-closing` — daily cash closing report

## TypeScript & Composite Projects

Every package extends `tsconfig.base.json` which sets `composite: true`. The root `tsconfig.json` lists all packages as project references.

## Root Scripts

- `pnpm run build` — runs `typecheck` first, then recursively runs `build` in all packages that define it
- `pnpm run typecheck` — runs `tsc --build --emitDeclarationOnly` using project references

## Key Commands

- `pnpm --filter @workspace/api-spec run codegen` — regenerate API client/Zod from OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes
- `pnpm --filter @workspace/api-server run dev` — start API server
- `pnpm --filter @workspace/clinica run dev` — start frontend
