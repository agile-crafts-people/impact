# R100 – Update files after architecture.yaml changes

**Status**: Shipped 
**Task Type**: Updates
**Run Mode**: Run as needed

## Goal

Update the Developer Edition docker-compose.yaml, and the welcome page index.html with changes after the architecture.yaml file has been updated with new services.

## Context / Input files

These files must be treated as **inputs** and read before implementation:

- `Specifications/architecture.yaml`

**Target files** (to be updated):

- `DeveloperEdition/docker-compose.yaml`
- `index.html` (welcome page)

## Requirements

Update `DeveloperEdition/docker-compose.yaml` and the welcome page `index.html` to match the services defined in `Specifications/architecture.yaml`.

### docker-compose.yaml

- Add or update service definitions for each new domain in the architecture.
- Do not change the existing welcome, runbook, or schema/mongodb services. 
- Do not create services or links for the common_code domain.
- **For each new microservice domain, define two profiles:**
  - `{domain}-api` – API service only (e.g. `profile-api` → profile_api)
  - `{domain}` – API + SPA (e.g. `profile` → profile_api + profile_spa)
- Ensure backing services (e.g. mongodb) are included in the profiles of any new services.
- Ensure all new services are included in the all profile.

### index.html

- Add links for each service SPA (with correct ports from the architecture).
- Add new domains to the top of the list
- Add an API Explorer link for each backing API at `/docs/explorer.html`.
- Do not create services or links for the common_code domain.
- There is no need to adjust the schema or runbook links

## Testing expectations

- **None**

## Packaging / build checks

Before marking this task as completed:
- Run ``make container`` and ensure that the container builds cleanly.

## Dependencies / Ordering

- Should run **after**:
  - **None**
- Should run **before**:
  - **None**

## Implementation notes (to be updated by the agent)

**Summary of changes**

- **docker-compose.yaml**: Added four new microservice domains from architecture.yaml (profile, evaluator, dashboard, classifier). Each domain has:
  - `{domain}-api` profile: API service only (e.g. `profile-api` → profile_api)
  - `{domain}` profile: API + SPA (e.g. `profile` → profile_api + profile_spa)
  - Services use architecture ports (8186–8193)
  - All new services depend on mongodb and are in the `all` profile
  - Extended mongodb profiles to include profile, evaluator, dashboard, classifier
- **index.html**: Added links for profile (8187), evaluator (8189), dashboard (8191), classifier (8193) SPAs at top of list; added API Explorer links at `/docs/explorer.html` for each backing API. Schema and runbook links unchanged.

**Testing results**: `make container` completed successfully.
