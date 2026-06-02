# ⛺ Safe-Shelter Connect
### Plateforme de Logistique Post-Séisme — Système d'Information de Gestion des Urgences

<p align="center">
  <img src="https://img.shields.io/badge/Python-Flask-blue?logo=flask" />
  <img src="https://img.shields.io/badge/Frontend-React%2018%20%2F%20Vite-61DAFB?logo=react" />
  <img src="https://img.shields.io/badge/Database-MySQL-orange?logo=mysql" />
  <img src="https://img.shields.io/badge/Auth-JWT-yellow" />
</p>

---

## 📌 Description du Projet

**Safe-Shelter Connect** est un système d'information complet conçu pour gérer les opérations d'urgence après un séisme. Il permet de gérer les zones de regroupement, d'affecter des abris aux sinistrés, et d'assurer une traçabilité stricte (via des **Triggers** et **Procédures Stockées SQL**) des ressources vitales (eau, tentes, kits médicaux).

Le projet est développé en deux versions :
- **Version Humaine** (`sans_ia`) — Stack Python/Flask + React (version principale documentée ici)
- **Version IA** — Stack Node.js/Express + React (architecture alternative)

---

## 🎥 Démo du Projet

> 🔗 **[Voir la démo vidéo](https://drive.google.com/file/d/1X_Tk-gu4q68HUKOnzaEohg2Zlt0x34Vs/view?usp=sharing)**

---

## 📄 Documentation & Rapports

| Document | Fichier |
|---|---|
| Architecture & Implémentation | `Rapport_Safe_Shelter_Connect.pdf` |
| Rapport Sécurité (Red Team) | `Rapport_Security.pdf` |
| Rapport  Sécurité (Blue Team) | `BlueTeam_PostFix_Report.pdf` |
| Rapport Équipe de Test | `rapport équipe de test.pdf` |
| Rapport Augmenté | `rapport.pdf` |
| Présentation Architecture | `presentation_Architectes.pdf` |

---

## 👥 Membres de l'Équipe (Groupe 4)

Filières **SDBDIA & SITCN** :

| SDBDIA | SITCN |
|---|---|
| CHITACHNI Doha | EL AOUTMANI Moncef |
| EL BAGHDADI Wafae | CHOKRANI R. |
| EL BOUKAA Ghait | DAOUI Abdessamad |
| EL HAFIANE Ahmed | DARBALI A. YASSER |
| EL HART Hamza | DREUICHE Mohamed |
| EL KAZDIR Fatima | EDDRIA Aya |

---

## 🚀 Fonctionnalités Clés

### Logique Métier Automatisée (Business Logic)

1. **Gestion des Capacités (Automatisée)** — Procédure stockée `sp_refresh_capacity` qui recalcule en temps réel les places disponibles dans les abris selon les points d'affectation occupés.
2. **Auto-Deduct Stock** — Trigger `fn_deduct_stock` qui déduit automatiquement les stocks de survie lors d'une distribution.
3. **Sécurité** — Authentification par Token JWT avec gestion des rôles (`super_admin`, `admin`, `sinistre`).
4. **Gestion des Missions** — Affectation d'équipes de secours aux zones critiques avec suivi des statuts (`Pending`, `In Progress`, `Completed`, `Cancelled`).

### Pour les Sinistrés (Victimes)

- Consultation publique des zones actives et leurs capacités disponibles (sans connexion)
- Inscription et connexion sécurisées
- Soumission d'une demande d'abri dans une zone disponible
- Tableau de bord personnel pour suivre l'état de la réservation (`Pending`, `Confirmed`, `Rejected`)

### Pour les Administrateurs

- **Contrôle d'accès par rôle (RBAC)** — Super Admin (accès global) vs Admin Zone (accès restreint à sa zone)
- **Gestion des Zones** — Création, modification, suppression de zones avec coordonnées GPS
- **Logistique & Inventaire** — Distribution et réapprovisionnement des ressources avec déduction automatique des stocks
- **Gestion des Réservations** — Validation ou rejet des demandes de sinistrés avec mise à jour automatique de la capacité
- **Gestion des Équipes** — Affectation d'équipes de secours et suivi des missions
- **Annuaire des Sinistrés** — Vue d'ensemble de tous les sinistrés enregistrés
- **Analytiques en Temps Réel** — Suivi des niveaux critiques de stock et des capacités par zone (Recharts + React Query polling)

---

## 🛠️ Technologies Utilisées

### Backend

| Composant | Technologie |
|---|---|
| Framework | Python / Flask |
| ORM | Flask-SQLAlchemy |
| Authentification | Flask-JWT-Extended (tokens 1h) |
| Base de données | MySQL (via PyMySQL) |
| CORS | Flask-CORS |
| Variables d'environnement | python-dotenv |

### Frontend

| Composant | Technologie |
|---|---|
| Framework | React 18 / Vite |
| Styling | Tailwind CSS |
| Routing | React Router DOM |
| State & Fetching | TanStack React Query (avec polling) |
| Cartographie | React Leaflet |
| Graphiques | Recharts |
| Composants UI | Radix UI / Shadcn |
| Icônes | Lucide React |
| Requêtes HTTP | Axios |

---

## 📂 Structure du Projet

```text
📦 safe-shelter-connect
 ┣ 📂 safe-shelter-app-main (Human version)    ← Version principale
 ┃ ┣ 📂 backend
 ┃ ┃ ┣ 📂 routes
 ┃ ┃ ┃ ┣ 📜 auth.py              # Inscription / Connexion JWT
 ┃ ┃ ┃ ┣ 📜 admin.py             # Gestion admin (users, stock, équipes)
 ┃ ┃ ┃ ┣ 📜 zones.py             # CRUD zones de regroupement
 ┃ ┃ ┃ ┣ 📜 reservations.py      # Gestion des demandes de sinistrés
 ┃ ┃ ┃ ┣ 📜 victim.py            # Routes espace sinistré
 ┃ ┃ ┃ ┗ 📜 misc.py              # Routes utilitaires
 ┃ ┃ ┣ 📂 utils
 ┃ ┃ ┃ ┗ 📜 decorators.py        # @super_admin_required, @admin_required
 ┃ ┃ ┣ 📜 app.py                 # Point d'entrée Flask
 ┃ ┃ ┣ 📜 extensions.py          # Initialisation db, jwt, cors
 ┃ ┃ ┣ 📜 models.py              # Modèles SQLAlchemy (10 tables)
 ┃ ┃ ┗ 📜 .env.example           # Variables d'environnement (exemple)
 ┃ ┣ 📂 frontend
 ┃ ┃ ┗ 📂 src
 ┃ ┃   ┣ 📂 api                  # Services Axios (auth, admin, zone)
 ┃ ┃   ┣ 📂 components           # Composants partagés (Navbar, Badge...)
 ┃ ┃   ┣ 📂 context              # AuthContext (état global auth)
 ┃ ┃   ┣ 📂 lib                  # Utilitaires (Tailwind cn merge)
 ┃ ┃   ┗ 📂 pages
 ┃ ┃     ┣ 📂 Admin              # Dashboard, Logistique, Réservations,
 ┃ ┃     ┃                       # Zones, Utilisateurs, Équipes, Sinistrés
 ┃ ┃     ┣ 📂 Auth               # Login, Register
 ┃ ┃     ┣ 📂 Public             # Carte publique des zones
 ┃ ┃     ┗ 📂 Victim             # Dashboard sinistré, portail demande
 ┃ ┗ 📜 soussmassaresilience.sql  # Script SQL complet (schéma + données)
 ┣ 📂 safe-shelter-app-main (AI version)       ← Version alternative Node.js
 ┗ 📄 *.pdf (Rapports de l'équipe)
```

---

## ⚙️ Installation & Lancement

### Prérequis

- [Node.js](https://nodejs.org/) v16+
- [Python](https://www.python.org/) v3.9+
- [MySQL Server](https://dev.mysql.com/downloads/)

---

### 1. Base de données

Créer une base MySQL et importer le schéma complet :

```bash
mysql -u your_username -p -e "CREATE DATABASE safe_shelter_db;"
mysql -u your_username -p safe_shelter_db < soussmassaresilience.sql
```

---

### 2. Backend (Flask)

```bash
cd "safe-shelter-app-main(Human version)/backend"

# Créer et activer l'environnement virtuel
python -m venv venv

# Windows
venv\Scripts\activate
# Mac/Linux
source venv/bin/activate

# Installer les dépendances
pip install Flask flask-sqlalchemy flask-jwt-extended flask-cors pymysql python-dotenv

# Configurer les variables d'environnement
cp .env.example .env
# Éditer .env avec vos paramètres MySQL

# Lancer le serveur
python app.py
```

> Le serveur backend sera disponible sur `http://127.0.0.1:5000`

---

### 3. Frontend (React)

```bash
cd "safe-shelter-app-main(Human version)/frontend"

npm install

npm run dev
```

> L'application sera disponible sur `http://localhost:5173`

---

## ⚙️ Configuration

Créer le fichier `.env` dans le dossier `backend/` à partir de `.env.example` :

```env
DB_USER=your_mysql_username
DB_PASSWORD=your_mysql_password
DB_HOST=localhost
DB_NAME=safe_shelter_db
JWT_SECRET_KEY=your_secure_jwt_secret
```

> Les tokens JWT expirent après **1 heure**.

---

## 💻 Utilisation

1. **Consultation publique** — Accéder à la page d'accueil pour voir les zones actives et leurs capacités sans connexion.
2. **Inscription sinistré** — Créer un compte pour soumettre une demande d'abri dans une zone disponible.
3. **Tableau de bord sinistré** — Suivre l'état de sa réservation après connexion.
4. **Accès administrateur** — Se connecter avec des identifiants admin pour accéder au portail interne :
   - Onglet **Zones** — Gérer les zones physiques d'hébergement.
   - Onglet **Logistique** — Distribuer et réapprovisionner les stocks.
   - Onglet **Réservations** — Valider ou rejeter les demandes de sinistrés.
   - Onglet **Équipes** — Affecter des équipes de secours aux missions.

---

## 🗄️ Modèle de Données

| Table | Description |
|---|---|
| `ZoneRegroupement` | Zones d'hébergement (nom, GPS, capacité max, capacité restante) |
| `PointAffectation` | Points d'hébergement individuels dans une zone (`Libre` / `Occupé`) |
| `User` | Utilisateurs avec rôles : `super_admin`, `admin`, `sinistre` |
| `Sinistre` | Profil détaillé du sinistré (CIN, nom, statut réservation, point assigné) |
| `Ressource` | Types de ressources disponibles (eau, tentes, kits médicaux…) |
| `Stocker` | Quantité de chaque ressource par zone |
| `Distribuer` | Journal des distributions de ressources aux sinistrés |
| `Equipe` | Équipes de secours affectées aux zones |
| `Mission` | Missions assignées aux équipes (`Pending`, `In Progress`, `Completed`, `Cancelled`) |

### Automatisations SQL

- **`sp_refresh_capacity`** — Procédure stockée appelée à chaque réservation pour recalculer la capacité restante d'une zone.
- **`fn_deduct_stock`** — Trigger qui décrémente automatiquement le stock d'une ressource lors d'une distribution.

---

## 🔐 Sécurité

- Authentification JWT (Bearer Token, expiration 1h)
- RBAC : accès différencié selon le rôle (`super_admin` > `admin` > `sinistre`)
- Décorateurs personnalisés `@super_admin_required` et `@admin_required` sur les routes sensibles
- Variables d'environnement pour les secrets (jamais en dur dans le code)
- Rapports de sécurité complets disponibles : `Rapport_Security.pdf` (Red Team) et `BlueTeam_PostFix_Report.pdf` (Blue Team)

---

## 🌐 API — Endpoints Principaux

Tous les endpoints sont préfixés par `/api/v1/`.

| Préfixe | Module | Description |
|---|---|---|
| `/auth` | Authentication | Inscription, connexion, refresh token |
| `/zones` | Zones | Liste, détail, CRUD des zones (admin) |
| `/reservations` | Réservations | Créer, valider, rejeter une réservation |
| `/admin` | Administration | Gestion utilisateurs, stocks, équipes |
| `/victim` | Sinistré | Profil, tableau de bord sinistré |

---

> 📌 Projet académique — **Safe-Shelter Connect** — Groupe 4 | Filières SDBDIA & SITCN
