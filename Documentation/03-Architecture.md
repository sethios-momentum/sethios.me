Voici le document suivant : **03-Architecture.md**

---

```markdown
# 03 - Architecture Technique

## 1. Vue d'ensemble

Sethios.me suit une architecture **monolithique modulaire** basée sur Laravel 12, structurée pour être maintenable, testable et évolutive vers une architecture orientée services si nécessaire.

## 2. Stack technique

### Backend
```
PHP 8.3+
Laravel 12
PostgreSQL 16
Redis 7
Meilisearch
RoadRunner (Octane)
```

### Frontend
```
Livewire 3 + Volt
Alpine.js
Tailwind CSS 4
Vite
```

### Infrastructure
```
Docker + Docker Compose
Nginx
GitHub Actions (CI/CD)
S3-compatible storage
```

## 3. Structure du projet Laravel

```
sethios.me/
├── app/
│   ├── Console/
│   │   └── Commands/
│   ├── Enums/
│   ├── Exceptions/
│   ├── Filament/
│   │   ├── Resources/
│   │   ├── Pages/
│   │   └── Widgets/
│   ├── Http/
│   │   ├── Controllers/
│   │   │   ├── Api/
│   │   │   ├── Web/
│   │   │   └── Admin/
│   │   ├── Livewire/
│   │   │   ├── Blog/
│   │   │   ├── Portfolio/
│   │   │   ├── Library/
│   │   │   ├── Contracts/
│   │   │   └── Ia/
│   │   ├── Middleware/
│   │   └── Requests/
│   ├── Models/
│   ├── Notifications/
│   ├── Policies/
│   ├── Providers/
│   ├── Services/
│   │   ├── Blog/
│   │   ├── Contracts/
│   │   ├── Ia/
│   │   ├── Library/
│   │   ├── Portfolio/
│   │   └── Seo/
│   └── View/
│       └── Components/
├── bootstrap/
├── config/
├── database/
│   ├── factories/
│   ├── migrations/
│   └── seeders/
├── docs/
├── lang/
│   ├── fr/
│   └── en/
├── modules/
│   ├── Blog/
│   │   ├── Database/
│   │   ├── Http/
│   │   ├── Models/
│   │   ├── Providers/
│   │   ├── Resources/
│   │   ├── Routes/
│   │   └── Services/
│   ├── Portfolio/
│   ├── Library/
│   ├── Contracts/
│   ├── Ia/
│   └── Cv/
├── public/
├── resources/
│   ├── css/
│   ├── js/
│   ├── views/
│   │   ├── components/
│   │   ├── layouts/
│   │   ├── livewire/
│   │   └── pages/
│   └── images/
├── routes/
│   ├── api.php
│   ├── web.php
│   ├── admin.php
│   └── channels.php
├── storage/
├── tests/
│   ├── Feature/
│   ├── Unit/
│   └── Pest.php
├── docker-compose.yml
├── Dockerfile
├── composer.json
├── package.json
├── vite.config.js
└── README.md
```

## 4. Architecture modulaire

### Modules métier

```
Modules/
├── Blog/
│   ├── Controllers/
│   ├── Livewire/
│   ├── Models/
│   ├── Services/
│   ├── Policies/
│   ├── Tests/
│   └── Routes/
├── Portfolio/
├── Publications/
├── Library/
├── Contracts/
├── Opportunities/
├── Cv/
├── Ia/
└── Core/
    ├── Media/
    ├── Seo/
    ├── Analytics/
    └── Notifications/
```

### Communication inter-modules
- **Services** : Injection de dépendance
- **Events/Listeners** : Communication asynchrone
- **Contracts/Interfaces** : Contrats d'interface entre modules

## 5. Base de données

### Moteur : PostgreSQL 16

#### Avantages
- Support JSON/JSONB natif
- Full-text search intégré
- Indexation avancée (GIN, GiST, BRIN)
- Partitionnement de tables
- Transactions ACID robustes

### Connexions
```php
// config/database.php
'pgsql' => [
    'driver' => 'pgsql',
    'host' => env('DB_HOST'),
    'port' => env('DB_PORT', 5432),
    'database' => env('DB_DATABASE'),
    'username' => env('DB_USERNAME'),
    'password' => env('DB_PASSWORD'),
    'charset' => 'utf8',
    'prefix' => '',
    'schema' => 'public',
    'sslmode' => 'prefer',
],
```

### Stratégie de cache
- **Redis** comme cache primaire
- Tags de cache par module
- Invalidation intelligente via observers

```php
// Exemple de clé de cache
Cache::tags(['portfolio', 'projects'])->remember(
    'portfolio.project.' . $id,
    now()->addHours(6),
    fn() => $project->load('media', 'technologies')
);
```

## 6. File d'attente et Jobs

### Configuration Horizon
```php
// config/horizon.php
'environments' => [
    'production' => [
        'supervisor-1' => [
            'maxProcesses' => 10,
            'balanceMaxShift' => 1,
            'balanceCooldown' => 3,
        ],
    ],
],
```

### Queues dédiées
- `default` : Jobs généraux
- `notifications` : Emails et notifications
- `media` : Traitement d'images/vidéos
- `ia` : Inférences IA
- `exports` : Générations PDF/rapports
- `analytics` : Traitement données visiteurs

### Jobs principaux
- `ProcessMediaJob` : Redimensionnement, optimisation
- `GenerateSitemapJob` : Régénération sitemap
- `SendNewsletterJob` : Envoi newsletter
- `TrainModelJob` : Entraînement modèle IA
- `GenerateContractPdfJob` : Génération contrat PDF
- `ProcessAnalyticsJob` : Agrégation analytics

## 7. Cache multi-niveaux

```
Niveau 1 : Cache applicatif (Redis)
    ↓ Miss
Niveau 2 : Cache HTTP (CDN)
    ↓ Miss
Niveau 3 : Base de données (PostgreSQL)
```

### Stratégie
- **Cache warming** : Pré-chargement périodique
- **Stale-while-revalidate** pour contenu non critique
- **Cache tags** pour invalidation ciblée

## 8. Search Architecture

### Meilisearch via Laravel Scout

```php
// config/scout.php
'driver' => env('SCOUT_DRIVER', 'meilisearch'),
'meilisearch' => [
    'host' => env('MEILISEARCH_HOST'),
    'key' => env('MEILISEARCH_KEY'),
    'index-settings' => [
        'articles' => [
            'filterableAttributes' => ['category', 'tags', 'status'],
            'sortableAttributes' => ['created_at', 'reading_time'],
            'searchableAttributes' => ['title', 'content', 'excerpt'],
        ],
    ],
],
```

### Modèles searchables
- Article
- Project
- Publication
- Book
- AiModel

## 9. Architecture Frontend

### Livewire 3 + Volt
```
Livewire Components/
├── Layout/
│   ├── Navigation
│   ├── Footer
│   └── Sidebar
├── Blog/
│   ├── ArticleList
│   ├── ArticleShow
│   ├── ArticleCard
│   └── SearchBlog
├── Portfolio/
│   ├── ProjectGrid
│   ├── ProjectShow
│   └── ProjectFilter
├── Library/
│   ├── BookList
│   ├── BookReader
│   └── SearchLibrary
├── Cv/
│   ├── InteractiveCv
│   └── SkillChart
├── Ia/
│   ├── DemoInterface
│   ├── ModelExplorer
│   └── ApiPlayground
└── Contracts/
    ├── ContractWizard
    ├── ContractStatus
    └── ClientDashboard
```

### État global (Alpine.js)
```javascript
// app.js
Alpine.store('app', {
    darkMode: false,
    locale: 'fr',
    notifications: [],
    
    toggleDarkMode() {
        this.darkMode = !this.darkMode
        localStorage.setItem('darkMode', this.darkMode)
    }
})
```

## 10. API Design

### RESTful versionnée
```
/api/v1/
├── articles
│   ├── GET /
│   └── GET /{id}
├── projects
│   ├── GET /
│   └── GET /{id}
├── models
│   ├── GET /
│   ├── GET /{id}
│   └── POST /{id}/predict
└── contracts
    ├── GET / (auth)
    └── GET /{id} (auth)
```

### Authentification API
- **Sanctum** tokens pour SPA
- **Personal access tokens** pour API publique
- Rate limiting : 60 req/min (public), 300 req/min (auth)

## 11. WebSockets (Reverb)

### Canaux
```php
// routes/channels.php
Broadcast::channel('contract.{contractId}', function ($user, $contractId) {
    return $user->can('view', Contract::find($contractId));
});

Broadcast::channel('ai.progress.{modelId}', function ($user, $modelId) {
    return true; // Canal public
});
```

### Événements broadcast
- `ContractStatusChanged`
- `NewCommentPosted`
- `ModelTrainingProgress`
- `RealTimeVisitorCount`

## 12. File Storage

### Architecture de stockage
```
storage/
├── app/
│   ├── public/
│   │   ├── media/
│   │   │   ├── projects/
│   │   │   ├── articles/
│   │   │   └── books/
│   │   └── downloads/
│   ├── contracts/
│   ├── exports/
│   └── backups/
└── framework/
```

### Disques Laravel
```php
'disks' => [
    'public' => [
        'driver' => 'local',
        'root' => storage_path('app/public'),
        'url' => env('APP_URL').'/storage',
    ],
    'media' => [
        'driver' => 's3',
        'key' => env('AWS_ACCESS_KEY_ID'),
        'secret' => env('AWS_SECRET_ACCESS_KEY'),
        'region' => env('AWS_DEFAULT_REGION'),
        'bucket' => env('AWS_BUCKET'),
        'url' => env('AWS_URL'),
        'cache' => true,
    ],
    'backups' => [
        'driver' => 's3',
        'bucket' => 'sethios-backups',
    ],
],
```

## 13. Sécurité applicative

### Middleware pipeline
```php
// bootstrap/app.php
->withMiddleware(function (Middleware $middleware) {
    $middleware->web([
        \App\Http\Middleware\EncryptCookies::class,
        \Illuminate\Cookie\Middleware\AddQueuedCookiesToResponse::class,
        \Illuminate\Session\Middleware\StartSession::class,
        \Illuminate\View\Middleware\ShareErrorsFromSession::class,
        \App\Http\Middleware\VerifyCsrfToken::class,
        \App\Http\Middleware\SetLocale::class,
        \App\Http\Middleware\SecurityHeaders::class,
    ]);
    
    $middleware->api([
        \Laravel\Sanctum\Http\Middleware\EnsureFrontendRequestsAreStateful::class,
        'throttle:api',
        \Illuminate\Routing\Middleware\SubstituteBindings::class,
    ]);
})
```

### Security Headers
```php
// SecurityHeaders middleware
'Content-Security-Policy' => "default-src 'self'; script-src 'self' 'unsafe-inline' 'unsafe-eval'; style-src 'self' 'unsafe-inline';",
'X-Frame-Options' => 'DENY',
'X-Content-Type-Options' => 'nosniff',
'Referrer-Policy' => 'strict-origin-when-cross-origin',
'Permissions-Policy' => 'camera=(), microphone=(), geolocation=()',
'Strict-Transport-Security' => 'max-age=31536000; includeSubDomains',
```

## 14. Performance Optimization

### Laravel Octane (RoadRunner)
```php
// config/octane.php
'server' => 'roadrunner',
'https' => true,
'max_requests' => 500,
'workers' => 4,
```

### Asset Building (Vite)
```javascript
// vite.config.js
export default defineConfig({
    plugins: [
        laravel({
            input: [
                'resources/css/app.css',
                'resources/js/app.js',
            ],
            refresh: true,
        }),
    ],
    build: {
        rollupOptions: {
            output: {
                manualChunks: {
                    alpine: ['alpinejs'],
                    livewire: ['livewire'],
                    charts: ['chart.js'],
                }
            }
        },
        chunkSizeWarningLimit: 1000,
    },
});
```

### Stratégies d'optimisation
- **Eager loading** systématique des relations
- **Chunking** pour les grosses collections
- **Lazy loading** des composants Livewire
- **Debouncing** des requêtes search
- **Pagination** avec curseurs pour grandes tables

## 15. Monitoring et Observabilité

### Laravel Pulse
```php
// Dashboard Pulse
'components' => [
    'slow_queries' => \Laravel\Pulse\Recorders\SlowQueries::class,
    'slow_requests' => \Laravel\Pulse\Recorders\SlowRequests::class,
    'exceptions' => \Laravel\Pulse\Recorders\Exceptions::class,
    'queues' => \Laravel\Pulse\Recorders\Queues::class,
    'cache' => \Laravel\Pulse\Recorders\CacheInteractions::class,
],
```

### Spatie Health
```php
// Health checks
Health::checks([
    DatabaseCheck::new(),
    RedisCheck::new(),
    MeiliSearchCheck::new(),
    DebugModeCheck::new(),
    EnvironmentCheck::new(),
    HorizonCheck::new(),
    QueueCheck::new(),
    SchedulerCheck::new(),
]);
```

### Logging structuré
```php
// config/logging.php
'channels' => [
    'stack' => [
        'driver' => 'stack',
        'channels' => ['daily', 'flare'],
    ],
    'analytics' => [
        'driver' => 'daily',
        'path' => storage_path('logs/analytics.log'),
        'level' => 'info',
        'days' => 90,
    ],
],
```

## 16. Tests Architecture

### Pest PHP
```
tests/
├── Unit/
│   ├── Models/
│   ├── Services/
│   └── Helpers/
├── Feature/
│   ├── Blog/
│   ├── Portfolio/
│   ├── Library/
│   ├── Contracts/
│   ├── Ia/
│   └── Api/
├── Browser/
│   └── Pages/
└── Datasets/
```

### Couverture cible
- Unit tests : > 80%
- Feature tests : > 70%
- Browser tests : flux critiques
- API tests : > 90%

## 17. CI/CD Pipeline

### GitHub Actions
```yaml
# .github/workflows/deploy.yml
name: Deploy
on:
  push:
    branches: [main]
jobs:
  test:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:16
      redis:
        image: redis:7
      meilisearch:
        image: getmeili/meilisearch:latest
    steps:
      - uses: actions/checkout@v4
      - name: Setup PHP
        uses: shivammathur/setup-php@v2
      - name: Install dependencies
        run: composer install --no-interaction
      - name: Run tests
        run: php artisan test --parallel
      - name: Build assets
        run: npm ci && npm run build
  deploy:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to production
        run: deploy-script
```

## 18. Docker Configuration

### Services
```yaml
# docker-compose.yml
services:
  app:
    build: .
    volumes:
      - .:/var/www/html
    depends_on:
      - postgres
      - redis
      - meilisearch
  
  postgres:
    image: postgres:16-alpine
    environment:
      POSTGRES_DB: sethios
      POSTGRES_USER: sethios
      POSTGRES_PASSWORD: secret
    volumes:
      - postgres_data:/var/lib/postgresql/data
  
  redis:
    image: redis:7-alpine
    
  meilisearch:
    image: getmeili/meilisearch:v1.7
    environment:
      MEILI_MASTER_KEY: master_key
    
  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./docker/nginx/default.conf:/etc/nginx/conf.d/default.conf

volumes:
  postgres_data:
```

