# DOZZLE 

Dozzle est un projet open source sponsorisé par Docker OSS. Il s'agit d'un 
visualiseur de journaux conçu pour simplifier la surveillance et le débogage des
conteneurs.

### Générer un fichier de configuration pour les utilisateurs
```bash
  docker run amir20/dozzle generate admin --password <user-password> --email \
    <user-email> --name <username> > data/users.yml
```

### Configuration de l'application
```bash
  # Créer un fichier .env
  cp dotenv/app.env.example dotenv/app.env
```

### Démarrer Dozzle
```bash
  docker compose --env-file=dotenv/app.env -f docker-compose.yml up -d
```

### Arrêter Dozzle
```bash
  docker compose --env-file=dotenv/app.env -f docker-compose.yml down
```

### Accéder à Dozzle
```
  http://localhost:8080
```

### Configuration de l'application
```bash
  # Créer un fichier .env
  cp dotenv/app.env.example dotenv/app.env
```
