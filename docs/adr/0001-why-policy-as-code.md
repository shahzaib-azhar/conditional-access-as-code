# 1. Manage Conditional Access policies as code

Date: 2026-11-08
Status: Accepted

## Context

Conditional Access decides who can sign in, from where and on what device. In most
tenants, policies are created and changed by hand in the Entra admin centre. There's
usually no review, no record of why a change was made, and no easy way to see what a
change will do before it hits real users. One mistake, like excluding the wrong group,
can lock people out or leave a gap nobody notices until something goes wrong.

## Decision

I'll keep every Conditional Access policy as a JSON file in this repo and treat the
repo as the source of truth. Changes go through pull requests. CI checks each policy
and runs persona tests before anything is deployed. New or changed policies go out
in report-only mode first, through Microsoft Graph, and only get switched on after
I've checked the sign-in logs.

## Consequences

- Every change has a review, a reason and a history I can roll back to.
- Tests catch some mistakes before they reach users, but only the ones I've written
  tests for.
- Someone can still change a policy in the portal, so I need a drift check to catch it.
- Small changes take longer than clicking in the portal. That's a fair trade for the
  most sensitive control in the tenant.
- The pipeline needs Graph permissions to manage policies, so its own identity has to
  be locked down: OIDC, no stored secrets, least privilege.
