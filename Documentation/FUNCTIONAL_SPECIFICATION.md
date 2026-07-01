# 02 - Spécification Fonctionnelle

## 1. Vision du projet
Sethios.me est la plateforme numérique centrale de Sethios, conçue comme un écosystème intégré Laravel regroupant portfolio, blog scientifique, bibliothèque numérique, CV interactif, système de contrats, laboratoire d'IA et opportunités professionnelles.

## 2. Objectifs
- Centraliser la présence numérique professionnelle
- Démontrer l'expertise technique via un portfolio interactif
- Diffuser du contenu scientifique de qualité
- Générer des opportunités professionnelles qualifiées
- Expérimenter et présenter des projets d'IA innovants
- Offrir une expérience utilisateur exceptionnelle

## 3. Public cible
- Recruteurs techniques et CTOs
- Entreprises cherchant des prestations freelance
- Chercheurs et communauté scientifique
- Développeurs et passionnés de tech
- Étudiants en informatique/IA

## 4. Architecture générale
- **Backend** : Laravel 12 avec PHP 8.3+
- **Frontend** : Livewire 3 + Volt + Alpine.js
- **Admin** : Filament 4
- **Base de données** : PostgreSQL 16
- **Cache** : Redis
- **Filesystem** : Local + S3-compatible
- **Queue** : Redis + Horizon
- **WebSocket** : Reverb
- **Search** : Meilisearch via Scout
- **Container** : Docker

## 5. Technologies
- PHP 8.3+, Laravel 12, Livewire 3, Volt, Alpine.js
- Filament 4, Tailwind CSS 4
- PostgreSQL 16, Redis 7
- Meilisearch, Docker
- Pest PHP (tests), Laravel Pint (formatage)
- Laravel Octane (performance)

## 6. Modules

### 6.1 Accueil
- Hero section dynamique avec animation
- Présentation rapide (titre, sous-titre, CTA)
- Derniers projets mis en avant
- Derniers articles de blog
- Statistiques en temps réel (projets, articles, contrats)
- Section témoignages
- Newsletter subscription

### 6.2 À propos
- Biographie détaillée
- Timeline de carrière
- Compétences techniques avec jauges/pourcentages
- Soft skills
- Certifications et formations
- Téléchargement CV PDF
- Liens réseaux sociaux professionnels

### 6.3 Portfolio
- Grille de projets filtrable (catégorie, technologie, difficulté)
- Fiche projet détaillée :
  - Description, objectifs, résultats
  - Technologies utilisées (avec icônes)
  - Liens GitHub/démo
  - Galerie d'images avec lightbox
  - Niveau de difficulté
  - Statut (en cours, terminé, maintenu)
  - Impact/ROI
  - Témoignage client
- Mode vue rapide (modal)
- Pagination ou infinite scroll

### 6.4 Projets
- Distinction projets personnels vs professionnels
- Roadmap publique des projets en cours
- Open source : liens, étoiles GitHub, contributeurs
- Projets collaboratifs avec rôles

### 6.5 Publications
- Liste des publications scientifiques
- Filtrage par année, domaine, type (article, conférence, journal)
- DOI et liens vers éditeurs
- Citations et métriques
- PDF téléchargeables
- BibTeX export

### 6.6 Blog scientifique
- Articles en Markdown
- Catégories et tags
- Recherche plein texte (Meilisearch)
- Métadonnées SEO complètes
- Temps de lecture estimé
- Articles liés
- Commentaires (modérés)
- Partage social
- Flux RSS
- Newsletter : notification nouveaux articles

### 6.7 Bibliothèque numérique
- Collection de ressources documentaires
- Filtrage par catégorie, auteur, date
- Upload/download de documents
- Prévisualisation PDF intégrée
- Compteur de téléchargements
- Citations formatées
- Recherche dans les documents

### 6.8 CV numérique
- Version interactive du CV
- Sections : expérience, formation, compétences, langues, certifications
- Export PDF personnalisable
- Timeline interactive
- Compétences avec niveaux visuels
- QR code vers la version en ligne

### 6.9 Opportunités
- Types : emploi, freelance, consulting, collaboration, conférence
- Formulaire de soumission (entreprise, description, budget, deadline)
- Dashboard de suivi
- Statuts : reçu, en discussion, accepté, refusé
- Notifications email
- Pipeline visuel Kanban

### 6.10 Contrats
- Modèles de contrats types (freelance, consulting, formation)
- Génération dynamique avec variables
- Signature électronique
- Suivi des étapes (brouillon, envoyé, signé, en cours, terminé, archivé)
- Facturation associée
- Espace client dédié

### 6.11 IA Lab
- Projets expérimentaux d'IA
- Démos interactives (chatbot, génération, analyse)
- Documentation technique
- Notebooks Jupyter intégrés
- Datasets téléchargeables
- Modèles pré-entraînés
- Benchmarks et performances
- API publique (rate limited)

### 6.12 Contact
- Formulaire de contact intelligent
- Upload de pièces jointes
- Détection automatique du sujet
- Réponse automatique personnalisée
- Intégration CRM (suivi conversations)
- FAQ dynamique avant soumission
- Anti-spam (honeypot + rate limit)

## 7. Fonctionnalités transverses

### Authentification et autorisation
- Multi-rôles (admin, editor, client, user, visitor)
- Social login (GitHub, Google, LinkedIn)
- 2FA optionnel
- Session management

### Administration (Filament)
- Dashboard avec statistiques
- CRUD pour toutes les entités
- Gestion des médias
- Modération des commentaires
- File d'attente et logs
- Health monitoring

### Notifications
- Email (Laravel Notification)
- In-app (database)
- Web push optionnel

### Analytics
- Visiteurs, pages vues, temps passé
- Taux de conversion
- Popularité du contenu
- Origine géographique
- Appareils et navigateurs

### Multilingue
- Français (principal)
- Anglais (secondaire)
- Possibilité d'extension

## 8. Tableau des rôles

| Rôle | Description | Accès |
|------|-------------|-------|
| Super Admin | Sethios | Tout |
| Admin | Gestion complète sauf critique | Admin panel complet |
| Editor | Gestion contenu | Blog, publications, bibliothèque |
| Client | Accès contrats et espace client | Dashboard client |
| User | Utilisateur enregistré | Commentaires, téléchargements |
| Visitor | Visiteur anonyme | Contenu public |

## 9. Tableau des permissions

| Permission | Super Admin | Admin | Editor | Client | User | Visitor |
|------------|-------------|-------|--------|--------|------|---------|
| Gérer utilisateurs | ✓ | ✓ | ✗ | ✗ | ✗ | ✗ |
| Créer contenu | ✓ | ✓ | ✓ | ✗ | ✗ | ✗ |
| Modérer commentaires | ✓ | ✓ | ✓ | ✗ | ✗ | ✗ |
| Voir contrats | ✓ | ✓ | ✗ | ✓ (siens) | ✗ | ✗ |
| Télécharger ressources | ✓ | ✓ | ✓ | ✓ | ✓ | ✗ |
| Commenter | ✓ | ✓ | ✓ | ✓ | ✓ | ✗ |
| Voir contenu public | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |

## 10. Base de données

### Tables principales
- users
- projects
- articles
- publications
- books
- skills
- experiences
- education
- contracts
- opportunities
- media
- tags
- categories
- messages
- subscribers
- visitors
- analytics
- comments
- testimonials
- ai_projects
- ai_models

## 11. Workflow éditorial

```mermaid
graph LR
    A[Brouillon] --> B[Relecture]
    B --> C[Prêt à publier]
    C --> D[Publié]
    D --> E[Archivé]
    C --> F[Programmé]
    F --> D

12. SEO
Techniques
URLs propres et descriptives

Balises title/meta dynamiques

Open Graph et Twitter Cards

Schema.org (Article, Person, Project, Book)

Sitemap.xml automatique (Spatie)

robots.txt configuré

Canonical URLs

Breadcrumbs structurés

Contenu
Recherche de mots-clés par article

Structure H1-H6 hiérarchique

Maillage interne automatique

Alt text sur toutes les images

Rich snippets (articles, CV, projets)

13. Performance
Objectifs Lighthouse
Performance : > 95

Accessibility : > 95

Best Practices : > 90

SEO : 100

Techniques
Cache Redis multi-niveaux

Lazy loading images

Asset minification et bundling

CDN pour les assets statiques

Database indexing optimisé

Queue pour les tâches lourdes

Octane (RoadRunner/Swoole)

HTTP/2 et Brotli compression

14. Sécurité
CSP headers stricts

Rate limiting sur API et formulaires

Honeypot anti-spam

Validation serveur stricte

SQL injection prevention (Eloquent)

XSS protection (Blade escaping)

CSRF tokens

File upload validation (type, size, scan)

2FA pour admin

Audit logs (Spatie Activity Log)

Backup automatique quotidien (Spatie)

Dépendances à jour (Dependabot/GitHub)

15. Accessibilité
WCAG 2.1 AA minimum

Navigation au clavier

ARIA labels

Contrastes suffisants

Focus indicators visibles

Skip links

Alt text obligatoires

Form labels et error messages

Responsive police (rem/em)

16. Responsive
Mobile-first design

Breakpoints : 375, 768, 1024, 1440, 1920

Touch-friendly (tailles de cibles ≥ 44px)

Images responsives (srcset)

Tableaux scrollables horizontalement

Menus adaptatifs (off-canvas mobile)

17. Internationalisation (i18n)
Fichiers de traduction Laravel

Détection automatique de la langue (navigateur)

Sélecteur de langue manuel

URLs localisées (/fr/, /en/)

Contenu traduisible (Spatie Translatable)

Dates et nombres formatés

18. API
Publique
Articles (liste, détail)

Projets (liste, détail)

IA Lab (liste modèles)

Privée (API tokens)
Contrats (CRUD client)

Messages

Upload documents

Documentation
OpenAPI/Swagger

Rate limiting documenté

Versioning (v1, v2)

19. Tableau des packages Laravel
Package	Usage
filament/filament	Admin panel
livewire/livewire	UI réactive
livewire/volt	Components single-file
spatie/laravel-permission	Rôles et permissions
spatie/laravel-medialibrary	Gestion médias
spatie/laravel-backup	Sauvegardes
spatie/laravel-activitylog	Logs d'activité
spatie/laravel-sitemap	Sitemap
spatie/laravel-translatable	Traductions
spatie/laravel-tags	Tags polymorphes
spatie/laravel-health	Monitoring
laravel/pulse	Performance dashboard
laravel/reverb	WebSockets
laravel/scout	Full-text search
laravel/horizon	Queue monitoring
laravel/sanctum	API tokens
laravel/socialite	Social login
laravel/octane	Performance
barryvdh/laravel-debugbar	Debug (dev)
20. Évolutions futures
Assistant IA personnel sur le site

Recommandation de contenu personnalisée

Espace membres premium

Marketplace de formations

Version SaaS multi-utilisateurs

Intégration visioconférence pour consulting

Application mobile (PWA puis native)

Document de référence pour tout le développement. Toute modification fonctionnelle doit être répercutée ici.

text

---

Spécification fonctionnelle terminée. Souhaitez-vous passer au document suivant : **03-Architecture.md** ?