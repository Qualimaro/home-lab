# 🏠 Home Lab

Infrastructure personnelle basée sur **Proxmox, Docker, Portainer, Keycloak, Cloudflare, Prometheus et Grafana**.

Ce dépôt contient les configurations nécessaires au déploiement et au versionnement des principaux services du homelab.

## 🎯 Objectifs

- versionner les stacks Docker ;
- centraliser les configurations ;
- déployer les services depuis Git ;
- séparer configuration, données et secrets ;
- automatiser progressivement les déploiements ;
- expérimenter Docker, GitOps, CI/CD et Kubernetes.

## 🐳 Services principaux

| Service | Usage |
|---|---|
| Portainer | Gestion des stacks Docker |
| Keycloak | Authentification centralisée / SSO |
| Guacamole | Accès distant |
| Jellyfin | Serveur multimédia |
| Sonarr / Radarr | Automatisation média |
| Prowlarr / Jackett | Gestion des indexers |
| Audiobookshelf | Livres audio |
| Vaultwarden | Gestionnaire de mots de passe |
| Prometheus | Collecte des métriques |
| Grafana | Dashboards et monitoring |
| Cloudflared | Publication sécurisée des services |

## 🔐 Authentification

Keycloak est utilisé comme fournisseur d'identité central via **OpenID Connect / OAuth2** lorsque les applications le permettent.

```text
Utilisateur
    │
    ▼
 Keycloak
    │
    ├── Portainer
    ├── Guacamole
    ├── Grafana
    └── Applications compatibles
```

## 🌐 Accès distant

Les services externes sont principalement publiés via **Cloudflare Tunnel**, afin de limiter l'exposition directe de l'infrastructure.

```text
Internet
   │
Cloudflare
   │
Tunnel
   │
Docker
```

## 📊 Monitoring

La supervision repose principalement sur :

- Prometheus
- Grafana
- Node Exporter
- cAdvisor
- PVE Exporter

```text
Proxmox ─┐
Linux ───┼──► Prometheus ───► Grafana
Docker ──┘
```

## 🗃️ GitOps

Git constitue progressivement la source de vérité de l'infrastructure.

```text
Modification
    ↓
Git
    ↓
GitHub
    ↓
Portainer
    ↓
Docker
```

Les stacks sont récupérées directement depuis le dépôt par Portainer.

## 🔑 Secrets

Aucun secret réel ne doit être stocké dans Git.

Les fichiers versionnés utilisent uniquement des variables :

```yaml
environment:
  DB_PASSWORD: "${DB_PASSWORD}"
```

Les valeurs réelles sont injectées lors du déploiement.

Avant un commit :

```bash
git diff --cached
```

et éventuellement :

```bash
grep -RniE 'password|secret|token|apikey|api_key' . --exclude-dir=.git
```

## 💾 Persistance

Les conteneurs sont considérés comme remplaçables.

Les données importantes sont conservées séparément via :

- volumes Docker ;
- bind mounts ;
- bases de données persistantes ;
- stockage local ou NAS.

Principe :

```text
Git      = configuration
Portainer = secrets
Volumes   = données
```

## 🚀 Évolutions prévues

- GitHub Actions
- validation automatique des fichiers Compose
- scan de secrets
- images Docker personnalisées
- GitHub Container Registry
- automatisation des déploiements
- Kubernetes
- GitOps avancé
- amélioration des sauvegardes

## 🧪 Philosophie

Ce homelab sert à héberger des services, mais surtout à expérimenter autour de :

`Linux` · `Docker` · `Portainer` · `Proxmox` · `Git` · `CI/CD` · `Keycloak` · `OAuth2` · `OIDC` · `Prometheus` · `Grafana` · `Cloudflare` · `Kubernetes`

---

> **Configuration dans Git. Secrets hors de Git. Données persistantes séparées des conteneurs.**
