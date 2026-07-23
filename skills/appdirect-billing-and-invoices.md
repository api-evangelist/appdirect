---
name: Report usage and settle invoices
description: Submit metered usage idempotently and read/pay a company's invoices on AppDirect.
api: openapi/appdirect-companies-openapi-original.yml
operations:
- resource_Other_billUsage_POST
- resource_Other_readCompanyInvoices_GET
- resource_Other_readInvoice_GET
- resource_Other_payInvoice_POST
---

# Report usage and settle invoices

Push metered usage into AppDirect billing, then read and pay the resulting invoices.

## Auth
OAuth 2.0 bearer. Usage submission is a system integration (client credentials +
`ROLE_PARTNER`); invoice read/pay may use `ROLE_BILLING_ADMIN`. Base URL `https://{marketplaceURL}/api`.

## Steps
1. **Submit usage** — `resource_Other_billUsage_POST`
   (`POST /integration/v1/billing/usage`). **Set an `idempotencyKey`** in the body so a
   retried submission is de-duplicated (see `conventions/appdirect-conventions.yml`).
2. **List company invoices** — `resource_Other_readCompanyInvoices_GET`
   (`GET /billing/v1/companies/{companyId}/invoices`).
3. **Read one invoice** — `resource_Other_readInvoice_GET`
   (`GET /billing/v1/invoices/{invoiceNumber}`).
4. **Pay it** — `resource_Other_payInvoice_POST`
   (`POST /billing/v1/invoices/{invoiceNumber}/payments`).

## Rules
- Always send `idempotencyKey` on usage POSTs; reuse the same key on retry, never a new one.
- Invoice lists paginate with `page`/`size`/`sort`.
- Treat `429` as throttling (backoff); `402`/`409` on pay indicate a billing state problem.
