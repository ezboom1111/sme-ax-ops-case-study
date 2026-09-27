# SME AI-transformation (AX) operations platform: case study (code is private)

**What it is.** One application that runs a two-site small business in Korea (leisure retail), built as its AI-transformation (AX) backbone: ERP, marketing CRM, staff operations, and customer check-in on one data plane. Staff, managers, marketing, and customers each get their own surface, and the numbers that drive daily decisions come from three different POS systems.

**Why it exists.** Before this, staff tasks, payroll and contracts, the marketing CRM, and customer check-in lived in separate tools, and the sales dashboard was a report rather than a tool for daily decisions. The goal was a single loop: today's recommendation, the evidence behind it, who acts, and what happened.

## Architecture

| Surface | Users | Device | Notes |
| --- | --- | --- | --- |
| Staff app | floor staff, part-timers | mobile | tasks, attendance, daily logs |
| ERP console | owner, executives, site managers | PC | sales, payroll, contracts, role-scoped views |
| Marketing CRM | marketing team | PC | campaigns, tasks, effect attribution |
| Customer check-in | guests | mobile QR | membership and visit flow |

- Single Next.js 16 / React 19 app with route groups per surface; Supabase (Postgres, Auth, RLS) as the data plane; deployed on Vercel.
- Python data pipeline for sales and external-signal analysis, feeding the same database.
- Three POS integrations (API and collected data) unified by a server-to-server data contract into the sales, P&L, and review ingest endpoints.
- Role model: executives, site managers, team leads, staff, part-timers, and customers are different principals with different auth, enforced at the API and RLS layers.

## Decisions that mattered

- **Operating loop, not reporting wall.** Every dashboard section carries a next action and an owner. Charts without a decision were removed.
- **No unwired backend gets merged.** After an audit found duplicated navigation, overlapping task entry points, and dead URL constants, the rule became: a new API constant ships only with at least one consumer.
- **Privacy boundary in code.** Customer identifiers, raw sales rows, and campaign numbers never leave the business system. This case study shows structure, not figures, on purpose.

## Scale (private repositories)

- Ops platform: 1,000+ files, 300+ commits, TypeScript and Python test suites.
- Sales analysis (Streamlit, the pipeline this platform absorbed): 150+ files, 150+ commits, Python test suite.

## Built with Claude Code

Specs and audits were written first; Claude Code and Codex workers implemented against them, and every worker report was checked against the diff and the test output before merge. The UI redesign was driven by a written design spec and an adversarial UX audit, not by adding pages.

## Status

Built during my employment (May to September 2026) and handed over to the business when I left. Code, data, and screenshots belong to the business and stay private. Ask me about the design.
