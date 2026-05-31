# QuickCare – Hospital Appointment & Queue Management System

## Overview

Full-stack hospital management system with real-time queue management, multi-role auth, appointment booking, and doctor queue control.

## Stack

- **Monorepo tool**: pnpm workspaces
- **Node.js version**: 24
- **Package manager**: pnpm
- **TypeScript version**: 5.9
- **Frontend**: React + Vite + Tailwind CSS + Wouter + React Query
- **API framework**: Express 5
- **Database**: PostgreSQL + Drizzle ORM
- **Auth**: JWT (jsonwebtoken) + bcryptjs for password hashing
- **Validation**: Zod (`zod/v4`), `drizzle-zod`
- **API codegen**: Orval (from OpenAPI spec)
- **Build**: esbuild (CJS bundle)

## User Roles

- **Patient**: Register, book appointments, view queue status, cancel/reschedule
- **Doctor**: View today's queue, call next patient, complete/skip appointments
- **Admin**: Manage doctors, view all queues/patients, set emergency priority

## Demo Accounts

- Admin: admin@quickcare.com / admin123
- Patient: patient@quickcare.com / patient123
- Doctors (created by admin via UI): sarah.johnson@quickcare.com / doctor123
- More doctors: michael.chen@quickcare.com / doctor123, emily.rodriguez@quickcare.com / doctor123

## Key Features

- JWT authentication with role-based route protection
- Auto-increment token system per doctor per day (format: D{doctorId}-{date}-{token})
- Real-time queue status polling (5s interval)
- Emergency priority elevation (moves patient to top of queue)
- Cancel and reschedule appointments
- Doctor queue control: call next, complete, skip

## Key Commands

- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- `pnpm --filter @workspace/api-server run dev` — run API server locally
- `pnpm --filter @workspace/quickcare run dev` — run frontend locally

## Architecture

- `artifacts/quickcare/` — React frontend
- `artifacts/api-server/` — Express API server
- `lib/db/` — PostgreSQL schema (users, doctors, appointments)
- `lib/api-spec/openapi.yaml` — API contract (OpenAPI 3.1)
- `lib/api-client-react/` — Generated React Query hooks
- `lib/api-zod/` — Generated Zod validation schemas

## API Routes

### Auth
- POST /api/auth/register
- POST /api/auth/login
- GET /api/auth/me

### Doctors (public GET, admin POST/PUT/DELETE)
- GET/POST /api/doctors
- GET/PUT/DELETE /api/doctors/:id

### Patient Appointments
- POST /api/appointments (book)
- GET /api/appointments/my
- PUT /api/appointments/:id/cancel
- PUT /api/appointments/:id/reschedule

### Doctor Queue
- GET /api/doctor/appointments
- GET /api/doctor/queue
- PUT /api/doctor/next (call next patient)
- PUT /api/doctor/complete/:appointmentId
- PUT /api/doctor/skip/:appointmentId

### Admin
- GET /api/admin/queue
- GET /api/admin/appointments
- GET /api/admin/patients
- PUT /api/admin/emergency/:appointmentId

### Queue (public)
- GET /api/queue/status/:doctorId

### Dashboards
- GET /api/dashboard/patient
- GET /api/dashboard/doctor
- GET /api/dashboard/admin
