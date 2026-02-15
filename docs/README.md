# Learning Lab (Isolated)

This folder is an isolated mini project for learning Sceptre + CI/CD without changing the main stack layout.

## Structure

- `learning-lab/config`: Sceptre configs
- `learning-lab/templates`: CloudFormation templates
- `learning-lab/data/buildspecs`: CodeBuild buildspec files

## Stacks in this lab

- `infra/learning-service.yaml`: deploys a private S3 bucket and SNS topic
- `infra/cicd/learning/pr.yaml`: PR validation pipeline
- `infra/cicd/learning/deploy.yaml`: main branch deploy pipeline
- `infra/cicd/codestar-connection.yaml`: CodeStar dependency for pipeline source/webhooks

## Commands (safe start)

```bash
sceptre --dir learning-lab/config generate infra/learning-service.yaml
```

## Deploy lab app stack manually

```bash
sceptre --dir learning-lab/config launch infra/learning-service.yaml
```

## Deploy lab CI/CD stacks

```bash
sceptre --dir learning-lab/config launch infra/cicd/codestar-connection.yaml
sceptre --dir learning-lab/config launch infra/cicd/learning/pr.yaml --var GitHubAccount=<your-github-user-or-org>
sceptre --dir learning-lab/config launch infra/cicd/learning/deploy.yaml --var GitHubAccount=<your-github-user-or-org>
```

## Flow to observe

1. Open/update PR -> PR pipeline runs `learning-lab/data/buildspecs/learning-validate.yaml`
2. Push to `main` -> deploy pipeline runs `learning-lab/data/buildspecs/learning-deploy.yaml`
