# conditional-access-as-code


I work with Entra ID and Intune every day, and Conditional Access is the part I'm most
careful with. One wrong exclusion can lock people out or quietly leave a gap, and most
changes are still made by hand in the portal with nobody reviewing them. I wanted to
build a safer way to do it.

## How it works

- Each policy is a JSON file in this repo. To change one, you open a pull request.
- CI checks the policy and runs it against a set of test "personas". For example, a
  contractor on an unmanaged laptop shouldn't be able to open Exchange Online.
- If the tests pass, the policy is deployed through Microsoft Graph in report-only mode.
- I check the sign-in logs for anything unexpected before turning it on.
- A nightly job compares the tenant with what's in Git and opens an issue if they differ.
- Break-glass accounts are left out of every policy, and any sign-in from them raises an alert.

## Status

Still a work in progress. I'm aiming for a first version in January 2027. Everything runs
in my own lab tenant, with nothing from work.

- [ ] Policy format and linting
- [ ] Persona tests (pytest)
- [ ] GitHub Actions with OIDC, so no secrets are stored in the repo
- [ ] Report-only deployment through Graph
- [ ] Drift check
- [ ] A table mapping each policy to Zero Trust and the Essential Eight

Design decisions and the threat model will go in `docs/adr` as I write them up.
