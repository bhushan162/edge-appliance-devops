# Decision log

Short records of why the project is built the way it is.

## 001: Jenkins as the main CI
- **Decision:** Jenkins runs the primary pipeline, defined as code in a `Jenkinsfile`.
- **Why:** Widely used in embedded teams, supports self-hosted agents with attached hardware. GitHub Actions is used only for lightweight PR checks.

## 002: LocalStack instead of real AWS
- **Decision:** S3 and related AWS services run locally via LocalStack.
- **Why:** Zero cost, safe to break and rebuild. Code targets AWS APIs, so moving to real AWS means changing an endpoint.

## 003: Pipeline order
- **Decision:** Docker build image, then Jenkins pipeline, then quality gates, then Renode emulation, then hardware tests, then OTA.
- **Why:** Each stage depends on the previous one being reproducible.

## 004: Emulation and hardware use different targets
- **Decision:** Renode tests run on an STM32F4-family board. Hardware tests run on the Nucleo-C092RC.
- **Why:** Renode does not support STM32C0. The same Robot Framework suites check the same behavior contract on both.
