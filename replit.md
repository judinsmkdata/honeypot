# Honeypot Monitor - Security Dashboard

## Overview

A real-time security monitoring dashboard for detecting and logging scanning attempts, intrusion attempts, and suspicious activity targeting smkdata.sch.id. The system acts as a honeypot by intercepting suspicious requests, categorizing threats by severity, and sending instant notifications via Telegram when threats are detected.

**Core Features:**
- Real-time threat detection and logging
- IP blocking and management
- Telegram notifications for security events
- Material Design-inspired monitoring interface
- Live activity feed and threat analysis

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture

**Technology Stack:**
- React with TypeScript
- Vite as build tool and dev server
- Wouter for client-side routing
- TanStack Query (React Query) for server state management
- Tailwind CSS for styling with shadcn/ui component library

**Design System:**
- Custom theme system with dark/light mode support
- Material Design principles for data-heavy monitoring applications
- Design references: Datadog, Grafana, Splunk monitoring interfaces
- Typography: Inter font family with monospace (Fira Code) for technical data
- Component library: shadcn/ui (Radix UI primitives)

**Key Pages:**
- Dashboard: Overview statistics and recent activity
- Live Monitor: Real-time threat feed with auto-refresh
- Threat Logs: Searchable/filterable table of all detected threats
- Blocked IPs: IP blocking management interface
- Settings: Telegram notification configuration

**State Management:**
- React Query for API data fetching with automatic refetching (5-10 second intervals)
- Local component state for UI interactions
- Theme persistence via localStorage

### Backend Architecture

**Technology Stack:**
- Node.js with Express
- TypeScript for type safety
- Drizzle ORM for database operations
- In-memory storage (MemStorage class) for development/simple deployments
- PostgreSQL support configured via Drizzle (NeonDB serverless)

**Honeypot Middleware:**
- Express middleware that intercepts ALL requests before API routes
- Pattern matching against suspicious paths, SQL injection, XSS, command injection, path traversal
- Detects scanner tools via user-agent analysis
- Categorizes threats by type and severity (low, medium, high, critical)
- Returns fake "success" responses to confuse attackers

**Threat Detection Patterns:**
- Admin panel paths (/wp-admin, /phpmyadmin, /cpanel)
- Configuration files (.env, .git, web.config)
- Backup files (.bak, .backup)
- Shell/backdoor attempts (c99, r57, wso)
- Path traversal (../, encoded variants)
- SQL injection keywords
- XSS patterns
- Command injection attempts

**API Routes:**
- GET /api/logs - Retrieve all threat logs
- POST /api/blocked-ips - Block an IP address
- DELETE /api/blocked-ips/:id - Unblock an IP
- GET /api/blocked-ips - List blocked IPs
- GET /api/telegram/settings - Get notification settings
- POST /api/telegram/settings - Update notification settings
- POST /api/telegram/test - Send test notification
- GET /api/stats - Dashboard statistics

**Rate Limiting:**
- Telegram notifications have configurable rate limits (per minute)
- Severity threshold filtering to prevent notification spam

### Data Storage

**Storage Interface (IStorage):**
- Abstract interface supporting multiple storage backends
- Current implementation: MemStorage (in-memory Map-based storage)
- Designed to support database persistence (Drizzle ORM configured for PostgreSQL/NeonDB)

**Data Models:**
- ThreatLog: Complete request details, threat classification, timestamps
- BlockedIp: IP address, reason, timestamps, optional expiry
- TelegramSettings: Notification preferences and rate limits
- DashboardStats: Aggregated metrics for overview

**Schema Design:**
- Zod schemas for runtime validation
- Drizzle schema definitions in shared/schema.ts
- Type-safe models shared between frontend and backend

### External Dependencies

**Telegram Bot API:**
- Bot token and chat ID configured via environment variables
- Sends formatted threat notifications with severity levels
- Rate-limited to prevent spam
- Configurable minimum severity threshold
- Test notification endpoint for verification

**Database (Optional):**
- NeonDB Serverless PostgreSQL configured
- Drizzle ORM as database abstraction
- Currently using in-memory storage but migration-ready
- Connection via DATABASE_URL environment variable

**Font CDN:**
- Google Fonts for Inter, DM Sans, Fira Code, Geist Mono, Architects Daughter
- Self-hosted alternative available

**Build Dependencies:**
- esbuild for server bundling (optimized cold starts)
- Vite for client bundling
- HMR support for development
- Production build outputs to dist/ directory

**Development Tools:**
- Replit-specific plugins for error overlay and dev banner
- TypeScript for compile-time type checking
- ESLint/Prettier configuration implied by shadcn/ui setup

**Environment Variables:**
- DATABASE_URL: PostgreSQL connection string (optional, falls back to in-memory)
- TELEGRAM_BOT_TOKEN: Telegram bot authentication
- TELEGRAM_CHAT_ID: Target chat for notifications
- NODE_ENV: Production/development mode switching