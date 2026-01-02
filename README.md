# Dozzle - Visualiseur de Logs Docker

Dozzle est une interface web légère et performante permettant de visualiser les journaux de vos conteneurs Docker en
temps réel. Ce projet fournit une configuration de déploiement prête pour la production, incluant la gestion SSL
(via Reverse Proxy) et une couche d'authentification sécurisée.

## Fonctionnalités
- **Visualisation Temps Réel** : Suivi des logs sans rechargement de page.
- **Support Multi-Nœuds** : Compatible avec les agents Docker.
- **Authentification Intégrée** : Protection via `simple` provider et gestion des utilisateurs.
- **Support Proxy** : Configuration optimisée pour `nginx-proxy` et `acme-companion` (SSL/TLS).
- **Persistance** : Configuration et profils utilisateurs persistants.

## Prérequis
- [Docker](https://docs.docker.com/get-docker/) & [Docker Compose](https://docs.docker.com/compose/install/)
- Un réseau Docker externe nommé `proxy` (pour le reverse proxy).
- Un Reverse Proxy actif (ex: `nginx-proxy`) écoutant sur le réseau `proxy`.

## Installation & Configuration

### 1. Structure du Projet
```bash
.
├── docker-compose.yml    # Définition des services
├── .env                  # Variables d'environnement (non versionné)
├── .env.example          # Modèle de configuration
└── data/
    ├── users.yml         # Base de données utilisateurs (YAML)
    └── admin/            # Profils et configurations spécifiques
```

### 2. Configuration de l'environnement
Copiez le fichier d'exemple pour créer votre configuration locale :
```bash
cp .env.example .env
```

Modifiez ensuite le fichier `.env` avec vos paramètres :

| Variable                   | Description                             | Exemple                  |
|----------------------------|-----------------------------------------|--------------------------|
| `DOZZLE_VERSION`           | Version de l'image Docker à utiliser    | `latest` ou `v4.x.x`     |
| `DOZZLE_HOSTNAME`          | Nom d'affichage de l'instance           | `dozzle-prod`            |
| `DOZZLE_VIRTUAL_HOST`      | Nom de domaine (pour le proxy)          | `logs.votredomaine.com`  |
| `DOZZLE_VIRTUAL_PORT`      | Port interne (défaut Dozzle: 8080)      | `8080`                   |
| `DOZZLE_LETSENCRYPT_HOST`  | Domaine pour le certificat SSL          | `logs.votredomaine.com`  |
| `DOZZLE_LETSENCRYPT_EMAIL` | Email pour l'enregistrement SSL         | `admin@votredomaine.com` |
| `DOZZLE_AUTH_PROVIDER`     | Méthode d'auth (`simple`, `none`, etc.) | `simple`                 |

### 3. Gestion des Utilisateurs
L'authentification est gérée via le fichier `data/users.yml`.
Pour ajouter ou modifier un utilisateur, vous devez générer un hash de mot de passe (bcrypt).

**Générer un nouvel utilisateur :**
```bash
docker run --rm -v "$(pwd)/data:/data" amir20/dozzle generate-users
```
Suivez les instructions interactives. Copiez ensuite le bloc YAML généré dans `data/users.yml`.

Exemple de `data/users.yml` :
```yaml
users:
  admin:
    name: "Administrateur"
    email: "admin@unsta.dev"
    password: "$2a$10$..." # Hash généré
```

## Démarrage

Lancez le service en mode détaché :
```bash
docker compose --env-file=dotenv/app.env -f docker-compose.yml up -d
```

Vérifiez les logs pour confirmer le bon démarrage :
```bash
docker compose logs -f
```

Accédez ensuite à votre instance via l'URL définie (ex: `https://logs.votredomaine.com`).

## Maintenance

**Mettre à jour Dozzle :**
1. Modifiez `DOZZLE_VERSION` dans `.env` si nécessaire (ou gardez `latest`).
2. Tirez la nouvelle image et redémarrez :
```bash
docker compose pull
docker compose --env-file=dotenv/app.env -f docker-compose.yml up -d
```

**Arrêter le service :**
```bash
docker compose --env-file=dotenv/app.env -f docker-compose.yml down
```
