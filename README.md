# DOZZLE

Dozzle est un projet open source sponsorisé par Docker OSS. Il s'agit d'un visualiseur de journaux en temps réel pour les
conteneurs Docker, conçu pour simplifier la surveillance et le débogage.

### 1. Configuration de l'application

Avant de lancer le service, vous devez configurer les variables d'environnement.

Créez un fichier `.env` à la racine du projet en copiant l'exemple :

```bash
cp dotenv/app.env.example dotenv/app.env
```

Modifiez le fichier `dotenv/app.env` et assurez-vous que les variables suivantes sont définies pour correspondre à votre
configuration de reverse proxy :

- `DOZZLE_HOSTNAME`
- `DOZZLE_VIRTUAL_HOST`
- `DOZZLE_LETSENCRYPT_HOST`
- `DOZZLE_LETSENCRYPT_EMAIL`

### 2. Générer un fichier de configuration pour les utilisateurs

Pour l'authentification, Dozzle utilise un fichier `users.yml`. Vous pouvez en générer un pour l'utilisateur `admin` avec la
commande suivante:

```bash
docker run --rm -v "$(pwd)/data:/data" amir20/dozzle generate-users
```
*Suivez les instructions pour créer les utilisateurs.*

*Note : Cette commande montera le répertoire `data` local dans le conteneur et y créera le fichier `users.yml`.*

### 3. Démarrer et Arrêter Dozzle

Pour démarrer le service en arrière-plan :

```bash
docker compose --env-file=dotenv/app.env -f docker-compose.yml up -d
```

Pour arrêter le service :

```bash
docker compose --env-file=dotenv/app.env -f docker-compose.yml down
```

### 4. Accéder à Dozzle

Une fois le conteneur démarré, vous pouvez accéder à Dozzle via l'URL que vous avez configurée dans la variable d'environnement 
`DOZZLE_VIRTUAL_HOST`.

Par exemple : `http://dozzle.votredomaine.com`
