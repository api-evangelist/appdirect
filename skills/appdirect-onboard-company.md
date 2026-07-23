---
name: Onboard a company and invite its users
description: Create a marketplace company on AppDirect, invite users into it, and confirm memberships.
api: openapi/appdirect-companies-openapi-original.yml
operations:
- resource_Company_createCompany_POST
- resource_Company_readCompany_GET
- resource_Company_inviteUsers_POST
- resource_Company_getInvitedUsers_GET
- resource_Company_readCompanyMemberships_GET
---

# Onboard a company and invite its users

Use the AppDirect Companies API to stand up a new company and bring its users on board.

## Auth
OAuth 2.0 bearer token against `https://{marketplaceURL}/oauth2/token`. Company-admin
flows need a user-level grant with `ROLE_SYS_ADMIN`; system integrations use the client
credentials grant with `ROLE_PARTNER`. See `authentication/appdirect-authentication.yml`.
Base URL: `https://{marketplaceURL}/api`.

## Steps
1. **Create the company** — `resource_Company_createCompany_POST`
   (`POST /account/v2/companies`). Capture the returned `companyUuid`.
2. **Read it back** — `resource_Company_readCompany_GET`
   (`GET /account/v2/companies/{companyUuid}`) to confirm it exists.
3. **Invite users** — `resource_Company_inviteUsers_POST`
   (`POST /account/v2/companies/{companyUuid}/invitations`) with the invitee emails/roles.
4. **List pending invitations** — `resource_Company_getInvitedUsers_GET`
   (`GET /account/v2/companies/{companyUuid}/invitations`).
5. **Confirm memberships** — `resource_Company_readCompanyMemberships_GET`
   (`GET /account/v2/companies/{companyUuid}/memberships`) once invitees accept.

## Rules
- Pass tenant context with the `AD-Tenant` header when calling across marketplaces.
- List endpoints paginate with `page`/`size`/`sort` (see `conventions/appdirect-conventions.yml`).
- Handle `429` (rate limited) with backoff; `409` indicates a conflicting company/user.
