```markdown
# 09 - Packages Laravel

## Liste complète des packages avec versions, usage et configuration

### 1. filament/filament
- **Version** : ^4.0
- **Usage** : Panneau d'administration complet
- **Pourquoi** : Interface admin moderne, rapide à développer, intégration native Livewire
- **Ressources créées** : UserResource, ProjectResource, ArticleResource, PublicationResource, BookResource, CategoryResource, TagResource, TechnologyResource, SkillResource, ExperienceResource, EducationResource, CertificationResource, ContractResource, OpportunityResource, AiProjectResource, MessageResource, SubscriberResource
- **Pages personnalisées** : Dashboard avec widgets statistiques
- **Widgets** : StatsOverview, LatestUsers, LatestArticles, LatestProjects, SystemHealth, QueueMonitor
- **Thème** : Personnalisé avec couleurs Sethios

### 2. livewire/livewire
- **Version** : ^3.0
- **Usage** : Composants réactifs sans JavaScript
- **Pourquoi** : Réactivité temps réel, pas d'API nécessaire pour l'UI, intégré à Filament
- **Composants principaux** :
  - HomePage (Hero, projets récents, articles récents)
  - ProjectIndex (filtres, recherche, pagination)
  - ProjectShow (galerie, technologies, projets liés)
  - BlogIndex (liste, filtres, recherche Meilisearch)
  - BlogShow (article, commentaires, table des matières)
  - PublicationIndex (liste académique, filtres)
  - LibraryIndex (grille livres, filtres)
  - BookShow (lecteur PDF, détails)
  - InteractiveCv (CV interactif complet)
  - ContactForm (formulaire intelligent)
  - IaLabIndex (catalogue modèles)
  - IaProjectShow (détails modèle, démo)
  - ContractWizard (assistant création contrat)
  - NewsletterForm (inscription)
  - SearchGlobal (recherche globale)
  - Comments (système commentaires)
  - ThemeToggle (dark mode)

### 3. livewire/volt
- **Version** : ^1.0
- **Usage** : Composants Livewire single-file
- **Pourquoi** : Code plus concis, composants dans un seul fichier, meilleure organisation
- **Utilisation** : Tous les composants Livewire en syntaxe Volt

### 4. spatie/laravel-permission
- **Version** : ^6.0
- **Usage** : Gestion des rôles et permissions
- **Pourquoi** : Package le plus maintenu, intégration Filament native, cache efficace
- **Rôles** : Super Admin, Admin, Editor, Client, User, Visitor
- **Permissions** :
  - view, create, edit, delete pour chaque entité
  - publish, unpublish pour contenu
  - manage_users, manage_roles
  - export_data, import_data
  - access_horizon, access_pulse, access_telescope
- **Configuration** :
```php
// config/permission.php
'models' => [
    'permission' => Spatie\Permission\Models\Permission::class,
    'role' => Spatie\Permission\Models\Role::class,
],
'cache' => [
    'expiration_time' => \DateInterval::createFromDateString('24 hours'),
    'key' => 'spatie.permission.cache',
],
```

### 5. spatie/laravel-medialibrary
- **Version** : ^11.0
- **Usage** : Gestion des médias et fichiers uploadés
- **Pourquoi** : Conversions automatiques, génération responsive images, intégration Filament
- **Collections** :
  - projects : images portfolio
  - articles : images de couverture
  - books : couvertures et PDFs
  - publications : PDFs
  - users : avatars
  - contracts : documents signés
  - ai_projects : images et notebooks
- **Conversions** :
```php
// Dans les modèles
public function registerMediaConversions(): void
{
    $this->addMediaConversion('thumb')
        ->width(400)
        ->height(300)
        ->sharpen(10)
        ->format('webp');
    
    $this->addMediaConversion('medium')
        ->width(800)
        ->height(600)
        ->format('webp');
    
    $this->addMediaConversion('responsive')
        ->withResponsiveImages();
}
```
- **Configuration** :
```php
// config/media-library.php
'driver' => 'gd',
'queue_connection' => 'media',
'queue_name' => 'media',
'media_model' => Spatie\MediaLibrary\MediaCollections\Models\Media::class,
```

### 6. spatie/laravel-backup
- **Version** : ^9.0
- **Usage** : Sauvegardes automatiques base de données et fichiers
- **Pourquoi** : Sécurité données, plan de reprise, conformité RGPD
- **Configuration** :
```php
// config/backup.php
'backup' => [
    'name' => 'sethios-backup',
    'source' => [
        'files' => [
            'include' => [
                base_path('storage/app'),
                base_path('storage/app/public'),
            ],
            'exclude' => [
                base_path('storage/logs'),
            ],
        ],
        'databases' => ['pgsql'],
    ],
    'destination' => [
        'disks' => ['s3', 'local'],
    ],
],
'cleanup' => [
    'strategy' => \Spatie\Backup\Tasks\Cleanup\Strategies\DefaultStrategy::class,
    'defaultStrategy' => [
        'keep_all_backups_for_days' => 7,
        'keep_daily_backups_for_days' => 30,
        'keep_weekly_backups_for_weeks' => 8,
        'keep_monthly_backups_for_months' => 4,
        'keep_yearly_backups_for_years' => 2,
    ],
],
```
- **Planification** :
```php
// routes/console.php
Schedule::command('backup:clean')->daily()->at('01:00');
Schedule::command('backup:run --only-db')->daily()->at('02:00');
Schedule::command('backup:run')->weekly()->sundays()->at('03:00');
Schedule::command('backup:monitor')->daily()->at('08:00');
```

### 7. spatie/laravel-activitylog
- **Version** : ^4.0
- **Usage** : Journalisation de toutes les activités
- **Pourquoi** : Traçabilité, audit, debugging, sécurité
- **Modèles tracés** : Tous les modèles principaux
- **Configuration** :
```php
// config/activitylog.php
'enabled' => true,
'delete_records_older_than_days' => 365,
'default_log_name' => 'default',
'activity_model' => Spatie\Activitylog\Models\Activity::class,
```

### 8. spatie/laravel-sitemap
- **Version** : ^7.0
- **Usage** : Génération automatique du sitemap XML
- **Pourquoi** : SEO, indexation Google, mise à jour automatique
- **Contenus inclus** :
  - Pages statiques (accueil, à propos, contact, CV, portfolio, blog, bibliothèque, IA Lab)
  - Articles (tous les publiés)
  - Projets (tous les publiés)
  - Publications
  - Livres
  - Projets IA
- **Configuration** :
```php
// app/Console/Commands/GenerateSitemap.php
use Spatie\Sitemap\SitemapGenerator;
use Spatie\Sitemap\Tags\Url;

SitemapGenerator::create(config('app.url'))
    ->getSitemap()
    ->add(Url::create('/blog')->setChangeFrequency('daily')->setPriority(0.9))
    ->add(Url::create('/portfolio')->setChangeFrequency('weekly')->setPriority(0.9))
    ->add(Article::all())
    ->add(Project::all())
    ->writeToFile(public_path('sitemap.xml'));
```
- **Planification** :
```php
Schedule::command('sitemap:generate')->daily()->at('04:00');
```

### 9. spatie/laravel-translatable
- **Version** : ^6.0
- **Usage** : Contenu multilingue dans la base de données
- **Pourquoi** : Traduction articles, projets, tags, catégories sans duplication de tables
- **Modèles multilingues** :
  - Article (title, content, excerpt)
  - Project (title, description, content)
  - Book (title, description)
  - Publication (title, abstract)
  - Tag (name, slug)
  - Category (name, description)
- **Configuration** :
```php
// config/translatable.php
'locales' => ['fr', 'en'],
'fallback_locale' => 'fr',
```

### 10. spatie/laravel-tags
- **Version** : ^4.0
- **Usage** : Système de tags polymorphe
- **Pourquoi** : Tags communs entre articles, projets, publications, modèles IA
- **Modèles taggables** : Article, Project, Publication, Book, AiProject
- **Configuration** :
```php
// config/tags.php
'model' => Spatie\Tags\Tag::class,
'type_model' => Spatie\Tags\Type::class,
```

### 11. spatie/laravel-health
- **Version** : ^1.0
- **Usage** : Monitoring santé de l'application
- **Pourquoi** : Détection proactive des problèmes, tableau de bord admin
- **Checks configurés** :
  - DatabaseCheck
  - RedisCheck
  - MeiliSearchCheck
  - DebugModeCheck (production)
  - EnvironmentCheck
  - HorizonCheck
  - QueueCheck
  - SchedulerCheck
  - DiskSpaceCheck
  - CertificateCheck
  - PingCheck
- **Configuration** :
```php
// config/health.php
'notifications' => [
    'enabled' => true,
    'notify_on' => ['fails', 'recovers'],
    'channels' => ['mail'],
],
```

### 12. laravel/pulse
- **Version** : ^1.0
- **Usage** : Dashboard performance temps réel
- **Pourquoi** : Monitoring performances, requêtes lentes, jobs, cache
- **Composants dashboard** :
  - Slow Queries
  - Slow Requests
  - Slow Jobs
  - Exceptions
  - Queues
  - Cache Interactions
  - Server Stats
- **Configuration** :
```php
// config/pulse.php
'enabled' => env('PULSE_ENABLED', true),
'path' => 'admin/pulse',
'ingest' => [
    'driver' => 'redis',
    'trim' => [
        'lottery' => [1, 1000],
    ],
],
```

### 13. laravel/reverb
- **Version** : ^1.0
- **Usage** : Serveur WebSocket natif Laravel
- **Pourquoi** : Temps réel (notifications, contrats, commentaires), auto-hébergé
- **Canaux** :
  - contracts.{id} : mises à jour contrats
  - projects.{id} : commentaires projets
  - ai.predictions.{id} : progression entraînement IA
  - admin.notifications : alertes admin
- **Configuration** :
```php
// config/reverb.php
'apps' => [
    'provider' => 'config',
    'apps' => [
        [
            'key' => env('REVERB_APP_KEY'),
            'secret' => env('REVERB_APP_SECRET'),
            'app_id' => env('REVERB_APP_ID'),
            'options' => [
                'host' => 'localhost',
                'port' => 8080,
                'scheme' => 'https',
            ],
        ],
    ],
],
```

### 14. laravel/scout
- **Version** : ^10.0
- **Usage** : Recherche full-text avec Meilisearch
- **Pourquoi** : Recherche rapide, pertinente, typo-tolérante
- **Modèles searchables** :
  - Article (title, content, excerpt)
  - Project (title, description, content)
  - Book (title, author, description)
  - Publication (title, abstract, authors)
  - AiProject (name, description)
- **Configuration Meilisearch** :
```php
// config/scout.php
'driver' => 'meilisearch',
'meilisearch' => [
    'host' => env('MEILISEARCH_HOST'),
    'key' => env('MEILISEARCH_KEY'),
    'index-settings' => [
        'articles' => [
            'filterableAttributes' => ['category', 'tags', 'status', 'published_at'],
            'sortableAttributes' => ['published_at', 'reading_time', 'views_count'],
            'searchableAttributes' => ['title', 'content', 'excerpt'],
            'typoTolerance' => ['enabled' => true],
        ],
        'projects' => [
            'filterableAttributes' => ['category', 'technologies', 'difficulty', 'status'],
            'sortableAttributes' => ['published_at', 'views_count', 'difficulty_level'],
            'searchableAttributes' => ['title', 'description', 'content'],
        ],
    ],
],
```

### 15. laravel/horizon
- **Version** : ^5.0
- **Usage** : Monitoring et gestion des files d'attente Redis
- **Pourquoi** : Dashboard queues, métriques jobs, gestion échecs
- **Queues configurées** :
  - default (jobs généraux)
  - notifications (emails)
  - media (traitement images)
  - ia (inférences IA)
  - exports (génération PDF)
  - analytics (traitement données)
- **Configuration** :
```php
// config/horizon.php
'environments' => [
    'production' => [
        'supervisor-default' => [
            'connection' => 'redis',
            'queue' => ['default', 'notifications'],
            'balance' => 'auto',
            'maxProcesses' => 10,
            'memory' => 128,
            'tries' => 3,
            'timeout' => 60,
        ],
        'supervisor-media' => [
            'connection' => 'redis',
            'queue' => ['media'],
            'balance' => 'simple',
            'maxProcesses' => 5,
            'memory' => 256,
            'tries' => 2,
            'timeout' => 300,
        ],
        'supervisor-ia' => [
            'connection' => 'redis',
            'queue' => ['ia'],
            'balance' => 'simple',
            'maxProcesses' => 3,
            'memory' => 512,
            'tries' => 1,
            'timeout' => 600,
        ],
    ],
],
```

### 16. laravel/sanctum
- **Version** : ^4.0
- **Usage** : Authentification API par tokens
- **Pourquoi** : API IA Lab, accès programmatique, SPA auth
- **API Tokens** : Personnels et par abilities
- **Configuration** :
```php
// config/sanctum.php
'stateful' => explode(',', env('SANCTUM_STATEFUL_DOMAINS', 'localhost,localhost:3000,127.0.0.1')),
'expiration' => null, // Pas d'expiration par défaut
'token_prefix' => 'sethios_',
```

### 17. laravel/socialite
- **Version** : ^5.0
- **Usage** : Authentification via réseaux sociaux
- **Pourquoi** : Connexion simplifiée, profils enrichis
- **Providers** :
  - GitHub
  - Google
  - LinkedIn
- **Configuration** :
```php
// config/services.php
'github' => [
    'client_id' => env('GITHUB_CLIENT_ID'),
    'client_secret' => env('GITHUB_CLIENT_SECRET'),
    'redirect' => env('GITHUB_REDIRECT_URI'),
],
'google' => [
    'client_id' => env('GOOGLE_CLIENT_ID'),
    'client_secret' => env('GOOGLE_CLIENT_SECRET'),
    'redirect' => env('GOOGLE_REDIRECT_URI'),
],
'linkedin' => [
    'client_id' => env('LINKEDIN_CLIENT_ID'),
    'client_secret' => env('LINKEDIN_CLIENT_SECRET'),
    'redirect' => env('LINKEDIN_REDIRECT_URI'),
],
```

### 18. laravel/octane
- **Version** : ^2.0
- **Usage** : Serveur applicatif haute performance
- **Pourquoi** : Temps de réponse réduits, concurrence élevée
- **Serveur** : RoadRunner
- **Configuration** :
```php
// config/octane.php
'server' => 'roadrunner',
'https' => true,
'max_requests' => 500,
'max_execution_time' => 30,
'workers' => 4,
'task_workers' => 2,
'watch' => [
    'app',
    'config',
    'database',
    'resources/views',
    'routes',
],
```

### 19. barryvdh/laravel-debugbar
- **Version** : ^3.0
- **Usage** : Barre de débogage développement
- **Pourquoi** : Debugging requêtes, vues, performances
- **Environnement** : Uniquement local et staging
- **Configuration** :
```php
// config/debugbar.php
'enabled' => env('DEBUGBAR_ENABLED', false),
'except' => ['api/*', 'admin/*', 'horizon/*', 'pulse/*'],
```

### 20. barryvdh/laravel-dompdf (ou autre PDF)
- **Version** : ^3.0
- **Usage** : Génération de PDF
- **Pourquoi** : Contrats PDF, CV PDF, exports
- **Utilisations** :
  - Contrats signés
  - CV téléchargeable
  - Export articles
  - Factures
- **Configuration** :
```php
// config/dompdf.php
'default_paper_size' => 'a4',
'default_font' => 'inter',
'enable_remote' => true,
```

### 21. meilisearch/meilisearch-php
- **Version** : ^1.0
- **Usage** : Client PHP Meilisearch (utilisé par Scout)
- **Pourquoi** : Recherche full-text puissante

### 22. predis/predis
- **Version** : ^2.0
- **Usage** : Client Redis PHP
- **Pourquoi** : Cache, sessions, queues, broadcasting

### 23. intervention/image
- **Version** : ^3.0
- **Usage** : Manipulation d'images
- **Pourquoi** : Redimensionnement, recadrage, optimisation
- **Utilisation** : Avec Spatie Media Library pour les conversions

## Packages NPM (Frontend)

### 24. tailwindcss
- **Version** : ^4.0
- **Usage** : Framework CSS utilitaire
- **Pourquoi** : Design system, responsive, dark mode intégré

### 25. @tailwindcss/typography
- **Version** : ^0.5
- **Usage** : Styles typographiques pour contenu riche
- **Pourquoi** : Rendu Markdown élégant, articles lisibles

### 26. @tailwindcss/forms
- **Version** : ^0.5
- **Usage** : Reset styles formulaires
- **Pourquoi** : Formulaires cohérents cross-browser

### 27. alpinejs
- **Version** : ^3.0
- **Usage** : Framework JavaScript léger
- **Pourquoi** : Interactions UI, état local, intégré Livewire

### 28. chart.js
- **Version** : ^4.0
- **Usage** : Graphiques et visualisations
- **Pourquoi** : Métriques IA, analytics, statistiques

### 29. glightbox
- **Version** : ^3.0
- **Usage** : Lightbox pour galeries images
- **Pourquoi** : Galeries portfolio, pas de dépendance jQuery

### 30. typed.js
- **Version** : ^2.0
- **Usage** : Animation texte dactylographié
- **Pourquoi** : Hero section dynamique

### 31. particles.js
- **Version** : ^2.0
- **Usage** : Particules animées arrière-plan
- **Pourquoi** : Hero section visuellement impressionnante

### 32. prismjs (ou shiki)
- **Version** : ^1.0
- **Usage** : Coloration syntaxique code
- **Pourquoi** : Articles techniques avec extraits code

### 33. katex
- **Version** : ^0.16
- **Usage** : Rendu équations mathématiques LaTeX
- **Pourquoi** : Articles scientifiques avec formules

### 34. pdfjs-dist
- **Version** : ^4.0
- **Usage** : Lecteur PDF dans le navigateur
- **Pourquoi** : Consultation livres dans bibliothèque

### 35. vite
- **Version** : ^6.0
- **Usage** : Bundler assets
- **Pourquoi** : Rapide, intégré Laravel

### 36. @vitejs/plugin-vue (si nécessaire)
- **Usage** : Support Vue si composants spécifiques

## Packages de Développement (dev)

### 37. pestphp/pest
- **Version** : ^3.0
- **Usage** : Framework de test PHP élégant
- **Pourquoi** : Syntaxe expressive, parallélisation, coverage

### 38. pestphp/pest-plugin-laravel
- **Version** : ^3.0
- **Usage** : Intégration Pest avec Laravel
- **Pourquoi** : Helpers Laravel dans les tests

### 39. pestphp/pest-plugin-browser
- **Version** : ^2.0
- **Usage** : Tests navigateur avec Playwright
- **Pourquoi** : Tests end-to-end

### 40. laravel/pint
- **Version** : ^1.0
- **Usage** : Formatage de code PHP
- **Pourquoi** : Style cohérent, PSR-12, automatique
- **Configuration** :
```json
// pint.json
{
    "preset": "laravel",
    "rules": {
        "declare_strict_types": true,
        "strict_comparison": true,
        "ordered_imports": { "sort_algorithm": "alpha" }
    }
}
```

### 41. larastan/larastan
- **Version** : ^3.0
- **Usage** : Analyse statique PHPStan pour Laravel
- **Pourquoi** : Détection bugs sans exécution, types stricts
- **Configuration** :
```neon
# phpstan.neon
includes:
    - vendor/larastan/larastan/extension.neon
parameters:
    level: 8
    paths:
        - app/
        - modules/
```

### 42. rector/rector
- **Version** : ^1.0
- **Usage** : Refactoring automatique PHP
- **Pourquoi** : Mises à jour code automatiques, upgrade versions

## Installation Complète

```bash
# Packages Composer principaux
composer require \
    filament/filament:^4.0 \
    livewire/livewire:^3.0 \
    livewire/volt:^1.0 \
    spatie/laravel-permission:^6.0 \
    spatie/laravel-medialibrary:^11.0 \
    spatie/laravel-backup:^9.0 \
    spatie/laravel-activitylog:^4.0 \
    spatie/laravel-sitemap:^7.0 \
    spatie/laravel-translatable:^6.0 \
    spatie/laravel-tags:^4.0 \
    spatie/laravel-health:^1.0 \
    laravel/pulse:^1.0 \
    laravel/reverb:^1.0 \
    laravel/scout:^10.0 \
    laravel/horizon:^5.0 \
    laravel/sanctum:^4.0 \
    laravel/socialite:^5.0 \
    laravel/octane:^2.0 \
    barryvdh/laravel-dompdf:^3.0 \
    intervention/image:^3.0 \
    meilisearch/meilisearch-php:^1.0 \
    predis/predis:^2.0

# Packages développement
composer require --dev \
    pestphp/pest:^3.0 \
    pestphp/pest-plugin-laravel:^3.0 \
    pestphp/pest-plugin-browser:^2.0 \
    laravel/pint:^1.0 \
    barryvdh/laravel-debugbar:^3.0 \
    larastan/larastan:^3.0 \
    rector/rector:^1.0

# Packages NPM
npm install \
    tailwindcss@^4.0 \
    @tailwindcss/typography \
    @tailwindcss/forms \
    alpinejs@^3.0 \
    chart.js@^4.0 \
    glightbox@^3.0 \
    typed.js@^2.0 \
    particles.js@^2.0 \
    prismjs@^1.0 \
    katex@^0.16 \
    pdfjs-dist@^4.0
```

---

