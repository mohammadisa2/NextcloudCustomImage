# Nextcloud Custom Install

Documentation project for setting up Nextcloud on CasaOS with Docker, PostgreSQL, Redis, and Cloudflare Tunnel.

This repository provides a structured GitHub Pages documentation site with:

- step-by-step installation guides
- separated troubleshooting pages for common issues
- operational notes for logs, backups, audits, and updates
- bilingual documentation in Indonesian and English

## What this covers

- Nextcloud deployment on CasaOS using custom Docker Compose
- PostgreSQL instead of SQLite for long-term NAS usage
- Redis for cache and file locking
- Cloudflare Tunnel for secure public HTTPS access without router port forwarding
- SSD/HDD permanent mount via UUID for stable storage paths
- operational practices such as `nas-check`, backups, and safer update flow

## Documentation

- GitHub Pages: [Nextcloud Custom Install Docs](https://mohammadisa2.github.io/NextcloudCustomImage/)
- Indonesian docs: [docs/](docs/)
- English docs: [docs/en/](docs/en/)

Main documentation sections:

- Tutorial
- Issues
- Operations

## Repository Structure

```txt
docs/
  index.md
  tutorial/
  issues/
  ops/
  en/
.github/workflows/
  pages.yml
LICENSE
README.md
```

## Publishing

This repository is configured for GitHub Pages deployment through GitHub Actions.

Key files:

- GitHub Pages workflow: [.github/workflows/pages.yml](.github/workflows/pages.yml)
- Jekyll config: [docs/_config.yml](docs/_config.yml)

## License

This project is licensed under the MIT License.

See [LICENSE](LICENSE) for details.

