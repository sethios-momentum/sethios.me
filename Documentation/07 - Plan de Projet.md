# 07 - Plan de Projet

## Méthodologie
Développement itératif avec des cycles quotidiens documentés. Chaque journée inclut objectifs, tâches détaillées, tests et livrables. Approche modulaire : chaque module est autonome avant de passer au suivant. Tests continus, commits fréquents, documentation mise à jour en temps réel.

## Phase Préparatoire

### Jour 0 - Préparation et Setup
**Objectif** : Environnement de développement prêt

**Matin (4h)**
- Installation Docker Desktop / OrbStack
- Clonage du dépôt Git
- Configuration Docker Compose (PHP 8.3, PostgreSQL 16, Redis 7, Meilisearch, Nginx)
- Build des conteneurs Docker
- Vérification connexions inter-conteneurs
- Création projet Laravel 12 dans le conteneur
- Configuration .env (DB, Redis, Meilisearch, Mail)
- Premier test : page d'accueil Laravel

**Après-midi (4h)**
- Installation dépendances Composer :
  - livewire/livewire
  - livewire/volt
  - filament/filament
  - spatie/laravel-permission
  - spatie/laravel-medialibrary
  - spatie/laravel-activitylog
  - spatie/laravel-health
  - laravel/sanctum
  - laravel/horizon
  - laravel/scout (meilisearch)
  - laravel/socialite
  - barryvdh/laravel-debugbar
- Installation dépendances NPM :
  - tailwindcss @tailwindcss/typography @tailwindcss/forms
  - alpinejs
  - chart.js
  - glightbox
  - typed.js
  - particles.js
  - prismjs (ou shiki)
  - katex
- Configuration Vite avec Tailwind CSS 4
- Premier build assets
- Initialisation Git, premier commit
- Push sur GitHub
- Configuration GitHub Actions (lint + build test)

**Livrables** : Projet Laravel fonctionnel, toutes dépendances installées, CI en place

---

## Phase 1 - Fondations (Jours 1-5)

### Jour 1 - Base de données et Modèles
**Objectif** : Structure de données complète

**Matin (4h)**
- Création migrations tables principales :
  - users (avec champs additionnels)
  - categories
  - tags (tables Spatie)
  - technologies
  - skills
- Exécution migrations
- Vérification structure base de données
- Création modèles Eloquent avec relations :
  - User
  - Category
  - Tag
  - Technology
  - Skill
- Tests unitaires modèles (Pest)

**Après-midi (4h)**
- Migrations modules métier :
  - projects + project_technology
  - articles + article_category
  - publications + publication_category
  - books + book_category
  - experiences
  - education
  - certifications
- Modèles avec relations complètes
- Factories pour toutes les entités
- Seeders de base :
  - RolesAndPermissionsSeeder
  - AdminUserSeeder
  - CategoriesSeeder
  - TechnologiesSeeder
  - SkillsSeeder
- Exécution seeders
- Tests unitaires relations

**Livrables** : Base de données complète, modèles avec relations, factories, seeders

### Jour 2 - Authentification et Autorisation
**Objectif** : Système auth complet avec rôles

**Matin (4h)**
- Installation Laravel Breeze avec Livewire
- Personnalisation vues auth (design Sethios)
- Configuration email verification
- Tests register, login, logout
- Password reset fonctionnel
- Email verification fonctionnelle
- Page profil basique
- Tests auth complets

**Après-midi (4h)**
- Configuration Spatie Permission
- Création rôles : Super Admin, Admin, Editor, Client, User
- Création permissions par module :
  - view, create, edit, delete pour chaque entité
  - publish, unpublish pour contenu
  - manage_users, manage_roles
- Attribution rôles aux utilisateurs
- Middleware permissions
- Gates et Policies pour chaque modèle :
  - ProjectPolicy
  - ArticlePolicy
  - PublicationPolicy
  - BookPolicy
  - ContractPolicy
- Tests permissions et policies

**Soirée (2h optionnel)**
- Socialite : GitHub login
- Socialite : Google login
- Socialite : LinkedIn login
- Tests social login

**Livrables** : Auth complète, rôles, permissions, policies, social login

### Jour 3 - Layout et Design System
**Objectif** : Interface utilisateur cohérente

**Matin (4h)**
- Création layout principal (app.blade.php) :
  - Navigation responsive
  - Footer complet
  - Meta tags dynamiques
- Composants Blade réutilisables :
  - x-button (primaire, secondaire, outline)
  - x-card
  - x-badge
  - x-modal
  - x-input
  - x-select
  - x-textarea
  - x-dropdown
  - x-tabs
  - x-accordion
  - x-tooltip
  - x-skeleton
  - x-alert
- Tests composants

**Après-midi (4h)**
- Configuration Tailwind complète :
  - Couleurs Sethios (palette)
  - Typographie (polices, échelles)
  - Espacements standards
  - Breakpoints responsive
  - Animations personnalisées
  - Classes utilitaires custom
- Design System documentation page
- Dark mode :
  - Variables CSS
  - Toggle dark mode
  - Tests tous composants dark
- Responsive tests (mobile, tablet, desktop)
- Tests accessibilité composants

**Livrables** : Design system complet, layout responsive, dark mode, composants réutilisables

### Jour 4 - Filament Admin Panel
**Objectif** : Administration complète

**Matin (4h)**
- Installation et configuration Filament 4
- Personnalisation thème (couleurs, logo, favicon)
- Branding panel admin
- Configuration auth Filament (utilise auth existante)
- Dashboard widgets :
  - StatsOverview (users, articles, projects, messages)
  - LatestUsers
  - LatestArticles
  - SystemHealth (Spatie Health)
- Ressource User (CRUD complet)
- Ressource Role et Permission (Spatie Plugin)
- Tests Filament resources

**Après-midi (4h)**
- Ressources Filament :
  - CategoryResource
  - TagResource
  - TechnologyResource
  - SkillResource
- Configuration Spatie Media Library
- Media Library integration dans Filament
- Médiathèque dans Filament
- Activité log integration
- Health monitoring dashboard
- Personnalisation navigation admin
- Groupes de navigation
- Icônes navigation
- Tests admin panel

**Livrables** : Panel admin complet, CRUD entités de base, médiathèque, monitoring

### Jour 5 - Page d'Accueil
**Objectif** : Homepage complète et animée

**Matin (4h)**
- Route "/" et controller
- Composant Livewire HomePage
- Hero section :
  - Typed.js pour titre animé
  - Mots-clés défilants
  - CTA buttons
  - Photo profil parallaxe
  - Particles.js arrière-plan
  - Scroll indicator
- Section projets récents :
  - Récupération 6 derniers projets featured
  - Cards avec images, technologies
  - Animation hover
- Tests hero et sections

**Après-midi (4h)**
- Section derniers articles (3-5 articles)
- Section bibliothèque (carrousel horizontal)
- Statistiques animées :
  - Compteur projets
  - Compteur articles
  - Compteur contrats
  - Années expérience
- Section témoignages (carrousel)
- Section IA Lab aperçu
- Newsletter form (Livewire component)
- Footer complet
- Optimisation images
- Lazy loading sections
- Tests page complète
- Test responsive
- Test performance (Lighthouse)

**Livrables** : Homepage complète, animée, responsive, performante

---

## Phase 2 - Contenu Principal (Jours 6-10)

### Jour 6 - Module Portfolio
**Objectif** : Portfolio complet fonctionnel

**Matin (4h)**
- Migration Project (si pas déjà faite)
- Ressource Filament Project complète :
  - Formulaire avec sections
  - Upload images multiple (Spatie Media Library)
  - Galerie avec tri drag-drop
  - Association technologies (select multiple)
  - Association catégories
  - Éditeur Markdown contenu
  - SEO metadata fields
  - Preview avant publication
- Tests Filament Project

**Après-midi (4h)**
- Route "/portfolio" et controller
- Composant Livewire ProjectIndex :
  - Grille projets responsive
  - Filtres (catégorie, technologie, difficulté, statut)
  - Recherche avec debounce
  - Tri (date, popularité, difficulté)
  - Vue grille/liste toggle
  - Pagination infinie
  - Compteur résultats
  - Animations apparition
- Route "/portfolio/{slug}"
- Composant Livewire ProjectShow :
  - Hero image/galerie
  - Informations projet
  - Technologies avec badges
  - GitHub et démo liens
  - Galerie lightbox (GLightbox)
  - Résultats et impact
  - Projets liés
  - Partage social
  - Compteur vues
- Tests pages portfolio

**Livrables** : Portfolio admin + public complet

### Jour 7 - Module Blog
**Objectif** : Blog scientifique complet

**Matin (4h)**
- Migration Article (si pas déjà faite)
- Ressource Filament Article complète :
  - Éditeur Markdown avec preview
  - Image couverture upload
  - Catégories et tags
  - Calcul automatique temps lecture
  - Planification publication
  - Statuts workflow
  - SEO metadata
- Tests Filament Article

**Après-midi (4h)**
- Route "/blog" et controller
- Composant Livewire BlogIndex :
  - Grille articles
  - Filtres (catégorie, tag, année)
  - Recherche Meilisearch
  - Pagination
  - Articles épinglés
  - Sidebar catégories/tags
- Route "/blog/{slug}"
- Composant Livewire BlogShow :
  - Article header
  - Rendu Markdown (Prism.js/Shiki)
  - Support KaTeX
  - Table des matières auto
  - Barre progression lecture
  - Articles liés
  - Partage social
  - Micro-données Schema.org
- Système commentaires :
  - Composant Livewire Comments
  - Formulaire commentaire
  - Réponses imbriquées
  - Modération admin
  - Notification email
  - Anti-spam
- Flux RSS
- Tests blog complets

**Livrables** : Blog admin + public, commentaires, RSS

### Jour 8 - Publications et Bibliothèque
**Objectif** : Publications et bibliothèque fonctionnelles

**Matin (4h)**
- Ressource Filament Publication :
  - CRUD publications
  - Import DOI (CrossRef API)
  - Import/Export BibTeX
  - Upload PDF
  - Gestion auteurs
  - Catégorisation
- Route "/publications"
- Composant Livewire PublicationIndex :
  - Liste formatée académique
  - Filtres (année, type, domaine)
  - Vue détaillée
  - Citation formatée
  - Copie presse-papier
  - Téléchargement PDF
- Tests publications

**Après-midi (4h)**
- Ressource Filament Book :
  - CRUD livres
  - Upload PDF + extraction metadata
  - Upload couverture
  - Catégorisation
- Route "/library"
- Composant Livewire LibraryIndex :
  - Grille livres
  - Filtres et recherche
  - Vue couvertures/liste
- Route "/library/{slug}"
- Composant Livewire BookShow :
  - Fiche livre détaillée
  - Lecteur PDF intégré (PDF.js)
  - Téléchargement compteur
  - Citation formatée
  - Livres liés
- Tests bibliothèque

**Livrables** : Publications et bibliothèque admin + public

### Jour 9 - CV Numérique et Contact
**Objectif** : CV interactif et formulaire contact

**Matin (4h)**
- Ressources Filament :
  - ExperienceResource
  - EducationResource
  - CertificationResource
- CRUD expériences, formations, certifications
- Gestion compétences (skill_user pivot)
- Gestion langues
- Route "/cv"
- Composant Livewire InteractiveCv :
  - Photo profil, infos personnelles
  - Résumé professionnel
  - Timeline expériences
  - Timeline formations
  - Barres compétences
  - Langues avec niveaux
  - Certifications logos
  - Projets sélectionnés
  - Publications sélectionnées
- Export PDF personnalisable :
  - Sélection sections
  - Thèmes couleur
  - Police taille
- QR code version en ligne
- Micro-données Schema.org Person
- Tests CV

**Après-midi (4h)**
- Migration Message
- Composant Livewire ContactForm :
  - Champs nom, email, sujet, message
  - Détection automatique type demande
  - Upload pièces jointes (max 10MB)
  - Validation temps réel
  - Honeypot anti-spam
  - Rate limiting
  - FAQ dynamique avant soumission
  - Confirmation visuelle
  - Email confirmation auto
- Ressource Filament Message :
  - Inbox style interface
  - Statuts (unread, read, replied, archived)
  - Réponse depuis interface
  - Templates réponses
  - Détection spam
- Tests contact

**Livrables** : CV numérique, formulaire contact, gestion messages

### Jour 10 - IA Lab (Base)
**Objectif** : Laboratoire IA fonctionnel

**Matin (4h)**
- Migrations AiProject, AiModelVersion
- Ressource Filament AiProject :
  - CRUD projets IA
  - Types (NLP, Computer Vision, etc.)
  - Frameworks
  - Métriques (accuracy, F1, etc.)
  - Upload modèle
  - Upload notebook Jupyter
  - Gestion versions modèles
- Ressource AiModelVersion :
  - Gestion versions
  - Changelog
  - Métriques par version
  - Fichiers modèle
- Tests admin IA

**Après-midi (4h)**
- Route "/ia-lab"
- Composant Livewire IaLabIndex :
  - Catalogue modèles
  - Cartes avec métriques clés
  - Filtres (type, framework, statut)
  - Recherche
- Route "/ia-lab/{slug}"
- Composant Livewire IaProjectShow :
  - Description complète
  - Métriques détaillées
  - Graphiques (Chart.js)
  - Notebook Jupyter intégré (JupyterLite)
  - Comparaison modèles
- Démos interactives basiques :
  - Upload image/texte
  - Affichage résultats
  - Temps inférence
  - Feedback utilisateur
- API IA (routes, controllers, documentation)
- Tests IA Lab

**Livrables** : IA Lab admin + public, démos, API

---

## Phase 3 - Avancé (Jours 11-15)

### Jour 11 - Opportunités et Contrats
**Objectif** : Système complet opportunités/contrats

**Matin (4h)**
- Migration Opportunity
- Ressource Filament Opportunity :
  - Pipeline Kanban
  - Drag-drop statuts
  - Fiches détaillées
  - Notes internes
- Formulaire public soumission opportunité
- Workflow traitement
- Notifications email
- Tests opportunités

**Après-midi (4h)**
- Migration Contract + tables liées
- Ressource Filament Contract :
  - Assistant création (étapes)
  - Templates contrats
  - Génération PDF (Laravel PDF)
  - Suivi jalons
  - Paiements
  - Signature électronique
- Dashboard client :
  - Liste contrats
  - Vue détaillée
  - Messagerie contrat
  - Upload fichiers
- Notifications contrats
- Tests contrats

**Livrables** : Opportunités et contrats complets

### Jour 12 - SEO et Performance
**Objectif** : Optimisation complète

**Matin (4h)**
- Configuration Spatie Sitemap :
  - Génération automatique
  - Tous types contenus
  - Fréquences mise à jour
  - Priorités
- robots.txt dynamique
- Métadonnées SEO toutes pages :
  - Title, description dynamiques
  - Open Graph (titre, description, image)
  - Twitter Cards
- Schema.org structuré :
  - Article, Project, Book, Person
  - BreadcrumbList
  - WebSite, Organization
- URLs canoniques
- Fils d'Ariane
- Balises hreflang (fr/en)
- Redirections 301 (fichier routes)
- Analyse SEO Lighthouse
- Tests SEO

**Après-midi (4h)**
- Configuration Laravel Octane (RoadRunner)
- Optimisation cache :
  - Redis tags par module
  - Cache warming scripts
  - Invalidation intelligente
- Images optimisation :
  - Conversion WebP auto
  - Lazy loading
  - Responsive images (srcset)
  - CDN configuration
- Assets optimisation :
  - Vite build config avancée
  - Code splitting
  - Compression Brotli
  - Cache headers
- Database :
  - Indexes check et optimisation
  - Query optimization (Eager loading)
  - N+1 detection et correction
- Audit performance (Lighthouse > 95)
- Tests performance

**Livrables** : SEO complet, performance optimisée

### Jour 13 - Accessibilité et i18n
**Objectif** : Accessible et multilingue

**Matin (4h)**
- Audit accessibilité WCAG 2.1 AA :
  - Navigation clavier
  - ARIA labels manquants
  - Contrastes couleurs
  - Skip links
  - Alt texts images
  - Form labels
  - Error messages
  - Focus indicators
  - Landmarks
- Corrections accessibilité
- Tests navigation clavier
- Tests lecteur écran (VoiceOver/NVDA)
- Tests zoom 200%
- Rapport accessibilité

**Après-midi (4h)**
- Configuration i18n Laravel :
  - Fichiers lang/fr/
  - Fichiers lang/en/
  - Traductions UI complètes
- Détection langue navigateur
- Sélecteur langue
- URLs localisées (/fr/..., /en/...)
- Middleware SetLocale
- Spatie Translatable :
  - Articles contenu
  - Projets contenu
  - Publications
  - Tags (noms)
  - Catégories (noms)
- Dates et nombres localisés
- Tests i18n
- Tests switch langue

**Livrables** : Site accessible WCAG AA, bilingue fr/en

### Jour 14 - Tests Complets
**Objectif** : Couverture tests exhaustive

**Matin (4h)**
- Tests unitaires :
  - Tous les modèles
  - Tous les services
  - Tous les helpers
  - Toutes les policies
  - Toutes les règles validation
- Objectif couverture > 85%
- Tests feature :
  - Authentification (register, login, logout, reset)
  - Pages publiques (accueil, blog, portfolio, etc.)
  - Formulaires (contact, newsletter, commentaires)
  - Filtres et recherche
  - Pagination
  - Upload fichiers
  - Permissions et accès
- Objectif couverture > 75%

**Après-midi (4h)**
- Tests Filament :
  - Resources CRUD
  - Permissions admin
  - Workflows (publication, statuts)
- Tests API :
  - Endpoints publics
  - Authentification API
  - Rate limiting
  - Validation
  - Réponses format
- Tests Browser (Pest Browser) :
  - Flux critiques (visite → contact)
  - Navigation complète
  - Dark mode toggle
  - Langue switch
  - Responsive behavior
- Rapport couverture global
- Corrections bugs trouvés

**Livrables** : Suite tests complète, couverture > 80%

### Jour 15 - Documentation et Déploiement
**Objectif** : Documentation finale et mise en production

**Matin (4h)**
- Finalisation docs/ :
  - Vérification cohérence tous documents
  - Mise à jour dates et versions
  - README.md présentation projet
  - CONTRIBUTING.md conventions
  - CHANGELOG.md v0.1 à v1.0
- Documentation API (OpenAPI/Swagger)
- Documentation utilisateur (guide admin)
- Documentation déploiement
- Capture écrans pour docs
- Revue documentation complète

**Après-midi (4h)**
- Préparation déploiement :
  - Configuration serveur production
  - Variables environnement
  - SSL certificats
  - DNS configuration
  - CDN setup
- CI/CD pipeline :
  - Tests automatiques
  - Build assets
  - Déploiement automatique
  - Rollback strategy
- Sauvegardes automatiques (Spatie Backup)
- Monitoring setup :
  - Laravel Pulse
  - Spatie Health
  - Error tracking (Flare/Sentry)
  - Uptime monitoring
- Déploiement production
- Tests post-déploiement :
  - Pages principales
  - Formulaires
  - Emails
  - Performance
  - SSL
- Annonce lancement

**Livrables** : Documentation complète, déploiement production réussi

---

## Suivi Quotidien

### Rituel matinal (30 min)
- Revue objectifs du jour
- Vérification environnement (Docker up, tests passent)
- Pull latest changes
- Revue rapide code jour précédent

### Rituel fin de journée (30 min)
- Tests passent tous
- Commit et push
- Mise à jour TODO.md
- Note dans CHANGELOG.md
- Revue rapide journée suivante

### Règles de développement
- Un commit par fonctionnalité
- Messages conventionnels (feat:, fix:, docs:, test:, refactor:)
- Tests avant chaque commit
- Pas de code commenté sans explication
- Pas de console.log ou dd() dans le code
- Revue personnelle avant push
- Branche feature par module
- Pull request vers main avec description

### Gestion des risques
- Bloqueur technique : demander aide, documenter solution
- Sous-estimation temps : ajuster planning, prioriser
- Bug critique : corriger avant de continuer
- Fatigue : faire pause, qualité > quantité

---
