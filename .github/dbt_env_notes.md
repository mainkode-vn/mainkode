# 📘 DBT CI/CD Environment Variables & Secrets Notes

This document outlines the usage and categorization of environment variables and secrets used in GitHub Actions workflows for DBT deployments.

---

## 🔐 Environment Secrets (Sensitive – must be protected)

| Variable                         | Description                                 | Why it's sensitive                  |
|----------------------------------|---------------------------------------------|-------------------------------------|
| AWS_ACCESS_KEY_ID               | AWS access key                              | Used for accessing S3               |
| AWS_SECRET_ACCESS_KEY           | AWS secret key                              | Used for accessing S3               |
| SNOWFLAKE_ACCOUNT               | Snowflake account identifier                | Credential for Snowflake login      |
| SNOWFLAKE_USER                  | Snowflake transform user                    | User credential                     |
| SNOWFLAKE_PASSWORD              | Password for Snowflake transform user       | Sensitive credential                |
| SNOWFLAKE_ROLE                  | Snowflake role name                         | Controls access level in Snowflake  |
| DOCKER_USERNAME (optional)     | Docker Hub username (if pushing images)     | Used for Docker authentication      |
| DOCKER_PASSWORD (optional)     | Docker Hub password                         | Used for Docker authentication      |

> 📌 **Where to store**: `Settings > Environments > [dbt-pre-prod / dbt-dev] > Secrets`

---

## 🌐 Environment Variables (Non-sensitive – config-specific)

| Variable                               | Description                                        | Notes                                |
|----------------------------------------|----------------------------------------------------|--------------------------------------|
| SNOWFLAKE_TRANSFORM_DATABASE           | Transform database in Snowflake                    | Varies across dev/pre-prod/CI        |
| SNOWFLAKE_TRANSFORM_WAREHOUSE          | Warehouse used for running dbt models              | Per environment                      |
| SNOWFLAKE_TRANSFORM_DATABASE_DEV       | Dev DB used in linting jobs                        | Used in `dbt-dev` environment         |
| SNOWFLAKE_TRANSFORM_DATABASE_CLONE     | Clone DB for PR builds                             | Used in `dbt-dev` environment         |
| AWS_REGION                             | AWS region for S3                                 | Example: `us-west-1`                 |
| DBT_PROFILES_DIR                       | Path to `profiles.yml` in DBT project              | Not sensitive                        |
| DBT_PROJECT_DIR                        | Path to DBT project root                           | Not sensitive                        |
| DBT_ARTIFACT_PATH                      | Local path to dbt's `manifest.json` file           | Not sensitive                        |
| S3_ARTIFACT_PATH                       | S3 path where `manifest.json` is stored            | Not sensitive                        |

> 📌 **Where to store**: `Settings > Environments > [dbt-pre-prod / dbt-dev] > Variables`

---

## ✅ Best Practices

- Store **Secrets** in GitHub **Environment Secrets**.
- Store **non-sensitive configuration values** in **Environment Variables**.
- **Never hardcode secrets in the workflow YAML file.**
- Avoid printing any secret value in logs.

---

## 📁 Suggested GitHub Environments

| Environment     | Purpose                             |
|------------------|-------------------------------------|
| `dbt-pre-prod`   | For deploying production-ready builds |
| `dbt-dev`        | For CI jobs, linting, PR model builds |

---

## 💡 Quick Reminder

| Type     | Use Case                                    |
|----------|---------------------------------------------|
| 🔐 Secrets | Anything that could compromise security if exposed |
| 🌐 Variables | Useful config settings that aren’t sensitive         |
