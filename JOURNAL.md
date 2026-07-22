# Setup Journal

## 2026-07-22 — Environment setup

Confirmed local toolchain meets the prerequisites in [docs/SETUP.md](docs/SETUP.md):

| Requirement | Installed |
|---|---|
| Git | 2.54.0 |
| Python | 3.12.13 |
| Node.js | 26.0.0 |
| npm | 11.12.1 |
| Docker | 29.4.3 |
| Docker Compose | v5.1.3 |
| Platform | macOS (Darwin arm64) |

Followed the setup steps in docs/SETUP.md: cloned the repo, copied `.env.example` to `.env`, and reviewed `docker-compose.yml` and the Makefile targets (`make setup`, `make run`) ahead of running the full stack.

This branch (`chore/153-environment-setup`) exists to confirm the dev environment is ready per issue #153, following the branch naming convention in [docs/CONTRIBUTING.md](docs/CONTRIBUTING.md).
