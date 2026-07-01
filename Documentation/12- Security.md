```markdown
# 12 - Sécurité

## Politique de Sécurité Globale

### Principes
- Défense en profondeur : multiples couches de protection
- Principe du moindre privilège : permissions minimales nécessaires
- Sécurité by design : intégrée dès la conception
- Zero trust : vérification systématique, même en interne
- Audit continu : logs, monitoring, alertes
- Mise à jour proactive : dépendances, correctifs

### Conformité
- RGPD (données personnelles utilisateurs européens)
- OWASP Top 10 (prévention risques web majeurs)
- WCAG 2.1 AA (accessibilité = sécurité pour tous)
- PCI DSS léger (si transactions futures)

---

## Sécurité Applicative

### Authentification

#### Mots de passe
- Hashage : bcrypt avec cost factor 12 minimum (Laravel par défaut)
- Complexité minimum : 12 caractères, mélange minuscules/majuscules/chiffres/caractères spéciaux
- Vérification contre listes de mots de passe compromis (Have I Been Pwned API)
- Expiration : jamais forcée, mais recommandation de changement périodique
- Historique : pas de réutilisation des 5 derniers mots de passe

#### Double authentification (2FA)
- Obligatoire pour rôles Admin et Super Admin
- Optionnelle pour Editor et Client
- Méthode : TOTP (Time-based One-Time Password)
- Applications compatibles : Google Authenticator, Authy, 1Password
- Codes de secours : 8 codes à usage unique générés
- Session 2FA : mémorisation 30 jours sur appareil de confiance

#### Sessions
```php
// config/session.php
'lifetime' => 120, // 2 heures
'expire_on_close' => false,
'encrypt' => true,
'secure' => true, // HTTPS only
'http_only' => true, // Pas d'accès JavaScript
'same_site' => 'lax',
```

#### Protection brute force
- Rate limiting : 5 tentatives par minute par IP + email
- Verrouillage temporaire : 15 minutes après 5 échecs
- Notification email au propriétaire du compte
- Journalisation de toutes les tentatives
- Captcha après 3 échecs (reCAPTCHA v3)

#### Social Login
- Vérification email obligatoire après première connexion sociale
- Association compte : confirmation par mot de passe existant
- Révocation possible depuis chaque provider
- Pas de permissions étendues demandées (read-only profil)

### Autorisation

#### Rôles et Permissions (Spatie)
```php
// Gates dans AuthServiceProvider
Gate::before(function ($user, $ability) {
    if ($user->hasRole('Super Admin')) {
        return true; // Super Admin bypass toutes les vérifications
    }
});

// Policies pour chaque modèle
class ArticlePolicy
{
    public function viewAny(User $user): bool
    {
        return true; // Public
    }
    
    public function view(User $user, Article $article): bool
    {
        return $article->is_published || $user->hasPermission('view_unpublished_articles');
    }
    
    public function create(User $user): bool
    {
        return $user->hasAnyPermission(['create_articles', 'manage_content']);
    }
    
    public function update(User $user, Article $article): bool
    {
        return $user->hasPermission('edit_articles') || 
               ($user->id === $article->author_id && $user->hasPermission('edit_own_articles'));
    }
    
    public function delete(User $user, Article $article): bool
    {
        return $user->hasPermission('delete_articles') && $article->status !== 'published';
    }
}
```

#### Middleware de sécurité
```php
// app/Http/Middleware/SecurityHeaders.php
public function handle(Request $request, Closure $next): Response
{
    $response = $next($request);
    
    $response->headers->set('X-Frame-Options', 'DENY');
    $response->headers->set('X-Content-Type-Options', 'nosniff');
    $response->headers->set('X-XSS-Protection', '1; mode=block');
    $response->headers->set('Referrer-Policy', 'strict-origin-when-cross-origin');
    $response->headers->set('Permissions-Policy', 'camera=(), microphone=(), geolocation=(), interest-cohort=()');
    
    if (app()->environment('production')) {
        $response->headers->set('Strict-Transport-Security', 'max-age=31536000; includeSubDomains; preload');
    }
    
    return $response;
}
```

### Protection CSRF
- Token CSRF sur tous les formulaires (Laravel automatique)
- Exclusion API routes (auth par token Sanctum)
- SameSite cookies : Lax
- Vérification referer/origin pour requêtes sensibles
- Renouvellement token après authentification

### Content Security Policy (CSP)
```php
// CSP Header
"Content-Security-Policy" => implode('; ', [
    "default-src 'self'",
    "script-src 'self' 'unsafe-inline' 'unsafe-eval' https://cdn.jsdelivr.net https://unpkg.com",
    "style-src 'self' 'unsafe-inline' https://fonts.googleapis.com",
    "img-src 'self' data: https: blob:",
    "font-src 'self' https://fonts.gstatic.com",
    "connect-src 'self' https://api.github.com",
    "media-src 'self'",
    "frame-src 'self' https://www.youtube.com https://jupyter.org",
    "object-src 'none'",
    "base-uri 'self'",
    "form-action 'self'",
    "frame-ancestors 'none'",
    "upgrade-insecure-requests",
])
```

---

## Protection des Données

### Entrées Utilisateur
- Validation serveur systématique (Form Requests)
- Échappement automatique Blade (`{{ }}`)
- Strip_tags sur entrées texte simple
- HTML Purifier pour contenu riche (Markdown)
- Limitation taille des uploads
- Validation types MIME stricts

### Base de données
```php
// Protection injections SQL via Eloquent
// Toutes les requêtes utilisent des paramètres liés (prepared statements)
// Pas de DB::raw() avec entrées utilisateur

// Exemple safe
Article::where('title', 'LIKE', '%' . $search . '%')->get(); // Safe, paramétré

// Exemple à éviter
DB::select("SELECT * FROM articles WHERE title LIKE '%{$search}%'"); // DANGEREUX
```

### Upload de fichiers
```php
// Règles validation strictes
'file' => [
    'required',
    'file',
    'mimes:pdf,doc,docx,jpg,jpeg,png,webp',
    'max:10240', // 10MB max
    'mimetypes:application/pdf,image/jpeg,image/png,image/webp',
],

// Stockage hors web root pour fichiers sensibles
Storage::disk('local')->putFileAs('contracts', $file, $safeFilename);

// Scan antivirus optionnel (ClamAV) pour fichiers uploadés
// Vérification type MIME réel, pas seulement extension
```

### Chiffrement
- Données sensibles en base : chiffrées avec Laravel Encryption (AES-256-GCM)
- Fichiers sensibles : chiffrés avec clé dédiée
- Certificats et secrets : hors code source (variables environnement)
- Backup : chiffrés avant stockage cloud
- HTTPS : TLS 1.3 minimum

---

## Headers de Sécurité

### Configuration Nginx
```nginx
# Protection DDoS basique
limit_req_zone $binary_remote_addr zone=api_limit:10m rate=10r/s;
limit_req_zone $binary_remote_addr zone=login_limit:10m rate=5r/m;

# Headers sécurité
add_header X-Frame-Options "DENY" always;
add_header X-Content-Type-Options "nosniff" always;
add_header X-XSS-Protection "1; mode=block" always;
add_header Referrer-Policy "strict-origin-when-cross-origin" always;
add_header Permissions-Policy "camera=(), microphone=(), geolocation=()" always;
add_header Strict-Transport-Security "max-age=31536000; includeSubDomains; preload" always;

# Cacher version Nginx
server_tokens off;

# Protection ressources
location ~ /\.(?!well-known) {
    deny all;
}

# Bloquer accès fichiers sensibles
location ~* \.(env|log|git|yml|yaml|xml|json)$ {
    deny all;
    return 404;
}
```

---

## Monitoring et Détection

### Journalisation (Spatie Activity Log)
```php
// Événements critiques loggés
activity()
    ->causedBy($user)
    ->performedOn($model)
    ->withProperties([
        'ip' => request()->ip(),
        'user_agent' => request()->userAgent(),
        'changes' => $model->getChanges(),
    ])
    ->log('model.updated');
```

### Événements surveillés
- Tentatives de connexion échouées
- Changements de mots de passe
- Modifications de rôles/permissions
- Suppression de contenu
- Export de données
- Accès à des ressources sensibles
- Upload de fichiers
- Modification de configuration

### Alertes
- Connexion depuis nouvelle IP / pays inhabituel
- Multiples échecs d'authentification
- Modification massive de contenu
- Tentative d'accès non autorisé répétée
- Pic anormal de trafic
- Détection de vulnérabilités dépendances
- Certificat SSL proche expiration
- Sauvegarde échouée

### Canaux d'alerte
- Email à l'admin
- Notification in-app (Filament)
- Slack/Discord webhook (optionnel)
- Log dédié

---

## Protection Contre les Attaques Courantes

### XSS (Cross-Site Scripting)
- Échappement automatique Blade
- CSP restrictif
- Validation des entrées
- Pas de `{!! !!}` non sécurisé
- HTML Purifier pour Markdown

### CSRF (Cross-Site Request Forgery)
- Token CSRF sur tous les formulaires
- SameSite cookies
- Vérification Origin/Referer
- Méthode DELETE/PUT/PATCH uniquement avec token

### SQL Injection
- Eloquent ORM systématique
- Pas de requêtes brutes avec entrées utilisateur
- Prepared statements pour cas edge
- Bindings nommés

### File Inclusion
- Pas d'inclusion dynamique de fichiers
- Chemins absolus ou base_path()
- Validation des noms de fichiers uploadés
- Stockage avec noms générés (UUID)

### Clickjacking
- X-Frame-Options: DENY
- CSP frame-ancestors 'none'

### Man-in-the-Middle
- HTTPS obligatoire (HSTS)
- Certificats à jour
- Pas de contenu mixte
- DNSSEC si disponible

### DDoS
- Rate limiting par IP et par route
- Queue pour tâches lourdes
- CDN avec protection DDoS (Cloudflare)
- Monitoring trafic anormal

### Énumération
- Messages d'erreur génériques (login : "Identifiants invalides" sans préciser si email existe)
- Rate limiting sur reset password
- Tokens aléatoires (pas d'incrément prévisible)
- UUIDs publics (pas d'IDs séquentiels)

---

## Sécurité des APIs

### Authentification API
```php
// Sanctum tokens avec abilities
$token = $user->createToken('api-token', [
    'articles:read',
    'projects:read',
    'ai-models:predict'
]);
```

### Rate Limiting API
```php
// config/sanctum.php + middleware
RateLimiter::for('api', function (Request $request) {
    $key = $request->user()?->id ?: $request->ip();
    return Limit::perMinute(60)->by($key);
});
```

### Protection API
- CORS restrictif (origines autorisées explicites)
- Tokens avec expiration
- Révocation tokens possible
- Pas d'exposition de données sensibles
- Validation stricte des entrées
- Réponses d'erreur sans détails internes

---

## Dépendances et Mises à Jour

### Gestion des vulnérabilités
```bash
# Vérification automatique (GitHub Dependabot / CI)
composer audit
npm audit

# Mise à jour sécurité
composer update --dry-run
npm update
```

### Politique de mise à jour
- Correctifs sécurité : sous 24-48h
- Mises à jour mineures : hebdomadaire
- Mises à jour majeures : planifiées avec tests
- Scan automatique des dépendances (CI)
- Alerte si CVE critique

---

## Sauvegarde et Reprise

### Sauvegardes automatiques (Spatie Backup)
```php
// Sauvegardes quotidiennes base de données
// Sauvegardes hebdomadaires complètes (fichiers + DB)
// Rétention configurée
// Stockage multi-disques (local + S3)
// Vérification intégrité automatique
// Alerte si échec
```

### Plan de reprise
- Base de données : restauration < 1 heure
- Fichiers : restauration depuis backup S3
- DNS : configuration secondaire documentée
- Procédure écrite et testée

---

## Conformité RGPD

### Fonctionnalités RGPD
- Consentement explicite newsletter (double opt-in)
- Export données utilisateur (JSON)
- Suppression compte avec données
- Politique de confidentialité accessible
- Cookies : bannière consentement
- Mentions légales
- Droit à l'oubli
- Portabilité des données

### Cookies
- Cookies essentiels : session, CSRF (pas de consentement requis)
- Cookies analytics : consentement requis
- Cookies sociaux : consentement requis
- Documentation des cookies utilisés
- Possibilité de révoquer consentement

---

## Checklist Sécurité Déploiement

### Pré-déploiement
- [ ] .env en production (APP_DEBUG=false)
- [ ] APP_KEY générée et sécurisée
- [ ] Mots de passe forts partout
- [ ] HTTPS configuré et testé
- [ ] Headers sécurité vérifiés
- [ ] CSP configuré sans 'unsafe-inline' si possible
- [ ] Permissions fichiers correctes
- [ ] Dépendances à jour et auditées
- [ ] Tests de sécurité passés

### Post-déploiement
- [ ] Scan sécurité automatisé
- [ ] Vérification certificat SSL
- [ ] Monitoring et alertes actifs
- [ ] Sauvegarde initiale effectuée
- [ ] Documentation sécurité à jour
- [ ] Test intrusion basique

---

## Outils de Sécurité Utilisés

- **Laravel Security** : Middleware, encryption, hashing, CSRF
- **Spatie Activity Log** : Journalisation événements
- **Laravel Sanctum** : Auth API sécurisée
- **Nginx** : Rate limiting, headers, TLS
- **Let's Encrypt** : Certificats SSL gratuits
- **GitHub Dependabot** : Alertes vulnérabilités dépendances
- **Laravel Health** : Monitoring applicatif
- **Laravel Backup** : Sauvegardes automatiques
- **Redis** : Rate limiting distribué

---
