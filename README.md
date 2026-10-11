# OpsMind AI

Telegram-based AI operations assistant built with **n8n + Google Gemini + Google Sheets + Gmail**.
Employees send natural-language requests (Arabic/English); an AI agent extracts the intent, role rules gate the action, and dedicated workflows handle invoices, customers, orders, tickets, reports and approvals — with full audit logging and a global error handler.

## Architecture
Telegram → user lookup (Users sheet) → authorization gate → AI intent parsing → role gate → router → WF-01..06 → WF-08 audit → reply

| Workflow | Role |
|---|---|
| WF-00 Telegram Router | Entry point, auth, AI agent, routing |
| WF-01 Invoice Processing | Invoices CRUD |
| WF-02 Approval Management | Human approval (invoice > 1000, deletes) via inline buttons |
| WF-03 Customer Management | Customers CRUD |
| WF-04 Order Management | Orders CRUD |
| WF-05 Support Tickets | Tickets CRUD |
| WF-06 Business Reports | On-demand report |
| WF-07 Daily AI Report | Scheduled daily AI summary (09:30) |
| WF-08 Audit Logging | Writes every operation to AuditLog |
| WF-09 Error Handler | Telegram alert + audit log on any failure |

## Setup
1. Create a Google Sheet with tabs: Users, Customers, Products, Invoices, Orders, Tickets, Approvals, AuditLog.
2. Import all JSON files into n8n; connect your own credentials (Telegram, Google Sheets, Gmail, Gemini).
3. Replace placeholders: `YOUR_MANAGER_CHAT_ID` (WF-02 ×2, WF-07 ×2, WF-09), `manager@example.com`, `@YOUR_USERNAME`.
4. Re-link the "Call WF-xx" sub-workflow nodes and set WF-09 as the Error Workflow in each workflow's settings.
5. Add yourself to Users (role: Manager/Admin/Employee, status: active) and activate.

No secrets are stored in these files; credentials are referenced by placeholder only.

## License
MIT
