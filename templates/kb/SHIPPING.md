# How we ship

> **Source of truth:** [link to canonical page]
>
> **Last synced:** [YYYY-MM-DD]
>
> Read by `how-we-ship-aura` when the user is writing, testing, reviewing, deploying, or operating production code.

## Code conventions

_(linting, formatting, language style guides — one bullet each, linked to canonical config)_

## Testing

_(test framework, where tests live, coverage targets, what kinds of tests are required vs optional)_

## Pull requests

_(PR title conventions, required reviewers, required CI checks, merge strategy)_

## CI / CD

_(GitHub Actions setup, where workflows live, secrets management, deploy environments)_

## Deploys

_(deploy targets, deploy automation, rollback procedure, blue/green vs rolling, release versioning)_

## Infrastructure as code

_(Terraform / Pulumi conventions, where state buckets live, what belongs in `infrastructure-global` vs service repo, label conventions)_

## Repo settings

_(branch protection rules, rulesets, autolinks, collaborator policy — typically managed by IaC; flag what's hands-off)_

## DNS / CDN

_(where DNS lives, how Fastly / CloudFront / etc. are configured, hands-off rules)_

## Observability

_(logging conventions, metrics, error reporting, alerting expectations)_
