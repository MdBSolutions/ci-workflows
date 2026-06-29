# CLAUDE.md - ci-workflows

## Doel
Gedeelde GitHub Actions reusable workflows voor het platform.
Alle microservices gebruiken deze workflows voor bouwen, testen, security-scans,
Docker-images, deployments en releases.

## Technische eisen
- GitHub Actions reusable workflows (workflow_call trigger)
- Ondersteuning voor PHP, Node.js en database-projecten
- Docker Buildx voor image builds
- Push naar eigen Docker Registry
- Integratie met Vault voor secrets
- Integratie met Platform API voor deployments
- Omgevingen: Development, Staging, Production
- Security scans via Trivy
- Automatisch versiebeheer (Semantic Versioning)
- Volledig container-gebaseerde deployments

## Richtlijnen
- Geen applicatiecode
- Geen secrets opslaan
- Alleen generieke workflows
- Alle workflows moeten door andere repositories herbruikbaar zijn (workflow_call)
- Elke wijziging moet backwards compatible zijn
- Een verantwoordelijkheid: CI/CD

## Mapstructuur
```
.github/workflows/
  build-php.yml          # Build en test PHP-projecten
  build-node.yml         # Build en test Node.js-projecten
  build-db.yml           # Database migrations
  docker-build.yml       # Docker image bouwen en pushen
  docker-deploy.yml      # Docker build + deploy via platform Ansible
  security-scan.yml      # Trivy security scan
  deploy.yml             # Deployment via Platform API
  migrate.yml            # Database migraties uitvoeren via db_migrations service
  release.yml            # Semantic versioning + release
```

## Standaard pipeline per PHP microservice
```
build -> security -> deploy -> migrate
```
Elke PHP microservice met een database voegt migrate toe na deploy.

## Standaard richtlijnen (globaal)
- Idempotent en declaratief
- Nooit direct naar main pushen
- Encoding: altijd ASCII
