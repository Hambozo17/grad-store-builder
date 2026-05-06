# Auth, Users, And Tenancy Spec

## Goal

Version One must support real login, user profiles, role-based access, and strict tenant isolation so every approved user can generate and manage only their own stores.

## Required Capabilities

- Supabase Auth signup, login, logout, and protected routes.
- `profiles` table for custom user details: full name, display name, role, approval status, avatar URL, preferred language/region, and timestamps.
- User statuses: `pending_approval`, `approved`, `suspended`.
- Roles: `user`, `super_admin`.
- Tenant model with `organizations` and `organization_members`.
- Each approved user gets or joins an organization before creating stores.
- Every store, generation run, catalog item, cart, mock checkout session, artifact, and verification report belongs to an organization.
- Super Admin access must be checked server-side and audited.

## Data Model

- `profiles`: one row per Supabase Auth user.
- `organizations`: tenant/workspace identity.
- `organization_members`: user-to-organization role and status.
- `admin_actions`: Super Admin actions such as approval, suspension, credit grant, store approval, and rejection.

## RLS Principles

- Enable RLS on all tenant-owned tables.
- Normal users may read/write only rows tied to their organization membership.
- Super Admin reads should go through server-side privileged functions or carefully reviewed policies.
- Never expose Supabase service-role keys to browser code.
- Do not modify protected `auth` schema tables directly.

## UX Requirements

- Login/signup must feel like part of the product, not a generic auth screen.
- Pending users see a calm waiting state explaining that admin approval is required.
- Suspended users see a blocked state with no tenant data leakage.
- Approved users land in their builder workspace.

## Tests

- Pending user cannot generate stores.
- Approved user can access only own organization data.
- User A cannot access User B stores by URL guessing.
- Suspended user cannot access builder routes.
- Super Admin can approve users and audit actions are recorded.

## Out Of Scope

- Enterprise SSO.
- Password policy customization beyond Supabase defaults.
- Production user billing.
- Vendor payout roles.
