# 🔐 Configuration des variables d'environnement

## Installation initiale

Pour configurer l'application sur une nouvelle machine :

### 1. Copier le fichier d'exemple
```bash
cp .env.example .env
```

### 2. Éditer le fichier .env avec vos valeurs

**Ouvrir le fichier `.env` et remplacer les valeurs d'exemple :**

```bash
# JWT - Générer des clés fortes
JWT_SECRET=votre_cle_secrete_access_token_changez_moi
REFRESH_TOKEN_SECRET=votre_cle_secrete_refresh_token_changez_moi

# Base de données - Remplacer par vos vraies valeurs
DB_HOST=localhost
DB_PORT=5432
DB_USER=votre_user_postgres
DB_PASSWORD=votre_mot_de_passe_postgres
DB_NAME=nom_de_votre_base
```

### 3. Générer des clés JWT fortes (recommandé)

**Option 1 : Avec OpenSSL (Linux/Mac)**
```bash
openssl rand -base64 32
```

**Option 2 : Avec Node.js**
```bash
node -e "console.log(require('crypto').randomBytes(32).toString('base64'))"
```

Utilise ces clés générées pour `JWT_SECRET` et `REFRESH_TOKEN_SECRET`.

---

## Variables disponibles

### JWT_SECRET
- **Description :** Clé secrète pour signer les access tokens
- **Durée de vie :** 30 minutes
- **Importance :** ⚠️ CRITIQUE - Ne jamais partager
- **Exemple :** `cnam_si_web_tp07_jwt_secret_key_2026_elkaid_karim_secure_token`

### REFRESH_TOKEN_SECRET
- **Description :** Clé secrète pour signer les refresh tokens
- **Durée de vie :** 7 jours
- **Importance :** ⚠️ CRITIQUE - Ne jamais partager
- **Note :** Doit être différente de JWT_SECRET
- **Exemple :** `cnam_si_web_tp07_refresh_token_secret_key_2026_elkaid_karim_long_term`

### DB_HOST
- **Description :** Adresse du serveur PostgreSQL
- **Local :** `localhost`
- **Production (Render/Heroku) :** URL fournie par le service
- **Exemple :** `dpg-xxxxx-a.oregon-postgres.render.com`

### DB_PORT
- **Description :** Port de connexion PostgreSQL
- **Valeur par défaut :** `5432`

### DB_USER
- **Description :** Nom d'utilisateur de la base de données
- **Exemple :** `cnam_a1_web_pollution_v2_user`

### DB_PASSWORD
- **Description :** Mot de passe de la base de données
- **Importance :** ⚠️ CRITIQUE - Ne jamais partager

### DB_NAME
- **Description :** Nom de la base de données
- **Exemple :** `cnam_a1_web_pollution_v2`

---

## Sécurité

### ⚠️ Points importants

1. **Ne JAMAIS commiter le fichier `.env`**
   - Le fichier est déjà dans `.gitignore`
   - Vérifier avec : `git status` (ne doit pas apparaître)

2. **Utiliser des clés fortes**
   - Minimum 32 caractères
   - Mélange de lettres, chiffres, caractères spéciaux
   - Générer avec les commandes ci-dessus

3. **Différencier les secrets**
   - `JWT_SECRET` ≠ `REFRESH_TOKEN_SECRET`
   - Si un token fuit, l'autre reste sécurisé

4. **En production (Render/Heroku)**
   - Utiliser les variables d'environnement du service
   - Ne pas mettre le `.env` dans le déploiement

---

## Vérification

Pour vérifier que le `.env` est bien chargé :

```bash
npm run dev
```

**Logs attendus :**
```
Synced db.
Server is running on port 3000.
```

**Si erreur de connexion à la base :**
- Vérifier les valeurs dans `.env`
- Vérifier que PostgreSQL est démarré (local)
- Vérifier les credentials (production)

---

## En cas de problème

### Erreur : "Cannot find module 'dotenv'"
```bash
npm install
```

### Erreur : "JWT_SECRET is not defined"
- Vérifier que le fichier `.env` existe
- Vérifier qu'il contient `JWT_SECRET=...`
- Redémarrer le serveur

### Erreur : "Connection refused" (base de données)
- Vérifier `DB_HOST`, `DB_PORT`, `DB_USER`, `DB_PASSWORD`, `DB_NAME`
- Vérifier que PostgreSQL est démarré
- Tester la connexion : `psql -h DB_HOST -U DB_USER -d DB_NAME`
