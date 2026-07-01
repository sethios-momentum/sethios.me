```markdown
# 10 - API

## Vue d'ensemble
API RESTful versionnée pour Sethios.me. Permet l'accès programmatique aux contenus publics, aux services IA, et aux fonctionnalités privées (contrats, messages) via authentification par tokens. Documentation OpenAPI/Swagger disponible. Rate limiting appliqué. Versioning par préfixe d'URL (/api/v1/).

## Structure des réponses

### Format standard
```json
{
    "success": true,
    "data": {},
    "message": "string",
    "meta": {
        "current_page": 1,
        "last_page": 10,
        "per_page": 15,
        "total": 150
    },
    "links": {
        "first": "https://sethios.me/api/v1/ressource?page=1",
        "last": "https://sethios.me/api/v1/ressource?page=10",
        "prev": null,
        "next": "https://sethios.me/api/v1/ressource?page=2"
    }
}
```

### Format erreur
```json
{
    "success": false,
    "message": "Description de l'erreur",
    "errors": {
        "field": ["Message d'erreur spécifique"]
    },
    "code": "ERROR_CODE"
}
```

### Codes HTTP utilisés
- 200 : Succès
- 201 : Créé avec succès
- 204 : Succès sans contenu (suppression)
- 400 : Requête invalide
- 401 : Non authentifié
- 403 : Non autorisé
- 404 : Ressource non trouvée
- 422 : Erreur de validation
- 429 : Trop de requêtes (rate limit)
- 500 : Erreur serveur

---

## Authentification

### Obtenir un token
```
POST /api/v1/auth/token
Content-Type: application/json

{
    "email": "user@example.com",
    "password": "password123",
    "device_name": "iPhone 15"
}
```

**Réponse** :
```json
{
    "success": true,
    "data": {
        "token": "1|sethios_abc123def456...",
        "type": "Bearer",
        "expires_at": "2027-01-01T00:00:00Z"
    }
}
```

### Révocation du token
```
DELETE /api/v1/auth/token
Authorization: Bearer 1|sethios_abc123def456...
```

### Informations utilisateur connecté
```
GET /api/v1/auth/user
Authorization: Bearer 1|sethios_abc123def456...
```

---

## Endpoints Publics

### Articles

#### Liste des articles
```
GET /api/v1/articles
```

**Paramètres** :
| Paramètre | Type | Défaut | Description |
|-----------|------|--------|-------------|
| page | integer | 1 | Numéro de page |
| per_page | integer | 15 | Articles par page (max 50) |
| category | string | - | Filtrer par slug catégorie |
| tag | string | - | Filtrer par slug tag |
| search | string | - | Recherche textuelle |
| sort | string | published_at | Tri (published_at, views_count, reading_time) |
| order | string | desc | Ordre (asc, desc) |
| locale | string | fr | Langue (fr, en) |
| year | integer | - | Filtrer par année |

**Réponse** :
```json
{
    "success": true,
    "data": [
        {
            "id": 1,
            "uuid": "550e8400-e29b-41d4-a716-446655440000",
            "title": "Introduction au Machine Learning",
            "slug": "introduction-machine-learning",
            "excerpt": "Un guide complet pour débuter...",
            "reading_time": 12,
            "published_at": "2026-06-15T10:00:00Z",
            "author": {
                "name": "Sethios",
                "avatar": "https://sethios.me/storage/avatars/sethios.jpg"
            },
            "categories": [
                {"name": "Intelligence Artificielle", "slug": "intelligence-artificielle"}
            ],
            "tags": [
                {"name": "Machine Learning", "slug": "machine-learning"},
                {"name": "Python", "slug": "python"}
            ],
            "image": "https://sethios.me/storage/articles/ml-intro.jpg",
            "url": "https://sethios.me/blog/introduction-machine-learning",
            "views_count": 1234
        }
    ],
    "meta": {
        "current_page": 1,
        "last_page": 5,
        "per_page": 15,
        "total": 72
    }
}
```

#### Article spécifique
```
GET /api/v1/articles/{slug}
```

**Réponse** :
```json
{
    "success": true,
    "data": {
        "id": 1,
        "uuid": "550e8400-e29b-41d4-a716-446655440000",
        "title": "Introduction au Machine Learning",
        "slug": "introduction-machine-learning",
        "content": "# Introduction\n\nLe Machine Learning est...",
        "content_html": "<h1>Introduction</h1><p>Le Machine Learning est...</p>",
        "excerpt": "Un guide complet pour débuter...",
        "reading_time": 12,
        "published_at": "2026-06-15T10:00:00Z",
        "updated_at": "2026-06-20T14:30:00Z",
        "author": {
            "name": "Sethios",
            "bio": "Chercheur en IA...",
            "avatar": "https://sethios.me/storage/avatars/sethios.jpg",
            "github": "https://github.com/sethios",
            "linkedin": "https://linkedin.com/in/sethios"
        },
        "categories": [
            {"name": "Intelligence Artificielle", "slug": "intelligence-artificielle"}
        ],
        "tags": [
            {"name": "Machine Learning", "slug": "machine-learning"},
            {"name": "Python", "slug": "python"}
        ],
        "image": "https://sethios.me/storage/articles/ml-intro.jpg",
        "seo": {
            "meta_title": "Introduction au Machine Learning - Guide Complet",
            "meta_description": "Découvrez les fondamentaux du Machine Learning..."
        },
        "related_articles": [
            {
                "title": "Deep Learning avancé",
                "slug": "deep-learning-avance",
                "reading_time": 15,
                "published_at": "2026-05-20T10:00:00Z"
            }
        ],
        "comments_count": 8,
        "views_count": 1234,
        "shares_count": 56,
        "url": "https://sethios.me/blog/introduction-machine-learning"
    }
}
```

### Projets

#### Liste des projets
```
GET /api/v1/projects
```

**Paramètres** :
| Paramètre | Type | Défaut | Description |
|-----------|------|--------|-------------|
| page | integer | 1 | Numéro de page |
| per_page | integer | 12 | Projets par page |
| category | string | - | Filtrer par slug catégorie |
| technology | string | - | Filtrer par slug technologie |
| difficulty | integer | - | Niveau difficulté (1-5) |
| status | string | - | Statut (in_progress, completed, maintained) |
| type | string | - | Type (personal, professional, open_source) |
| featured | boolean | - | Projets mis en avant |
| search | string | - | Recherche textuelle |
| sort | string | published_at | Tri |

**Réponse** :
```json
{
    "success": true,
    "data": [
        {
            "id": 1,
            "uuid": "660e8400-e29b-41d4-a716-446655440001",
            "title": "Système de Reconnaissance d'Images",
            "slug": "systeme-reconnaissance-images",
            "excerpt": "Système de classification d'images par deep learning...",
            "type": "professional",
            "status": "completed",
            "difficulty_level": 4,
            "technologies": [
                {"name": "Python", "slug": "python", "icon": "python"},
                {"name": "TensorFlow", "slug": "tensorflow", "icon": "tensorflow"}
            ],
            "image": "https://sethios.me/storage/projects/image-recognition.jpg",
            "github_url": "https://github.com/sethios/image-recognition",
            "demo_url": "https://demo.sethios.me/image-recognition",
            "published_at": "2026-04-10T10:00:00Z",
            "url": "https://sethios.me/portfolio/systeme-reconnaissance-images"
        }
    ]
}
```

#### Projet spécifique
```
GET /api/v1/projects/{slug}
```

### Publications

#### Liste des publications
```
GET /api/v1/publications
```

**Paramètres** :
| Paramètre | Type | Défaut | Description |
|-----------|------|--------|-------------|
| page | integer | 1 | Numéro de page |
| per_page | integer | 20 | Publications par page |
| type | string | - | Type (journal_article, conference_paper, etc.) |
| year | integer | - | Année de publication |
| language | string | - | Langue (fr, en) |
| search | string | - | Recherche |
| sort | string | publication_date | Tri |

#### Publication spécifique
```
GET /api/v1/publications/{slug}
```

### Bibliothèque

#### Liste des livres
```
GET /api/v1/books
```

**Paramètres** :
| Paramètre | Type | Défaut | Description |
|-----------|------|--------|-------------|
| page | integer | 1 | Numéro de page |
| per_page | integer | 12 | Livres par page |
| category | string | - | Filtrer par slug catégorie |
| language | string | - | Langue |
| search | string | - | Recherche |
| downloadable | boolean | - | Livres téléchargeables |

#### Livre spécifique
```
GET /api/v1/books/{slug}
```

### CV

#### Données du CV
```
GET /api/v1/cv
```

**Réponse** :
```json
{
    "success": true,
    "data": {
        "personal": {
            "name": "Sethios",
            "title": "Chercheur & Développeur IA",
            "email": "contact@sethios.me",
            "website": "https://sethios.me",
            "location": "Paris, France",
            "avatar": "https://sethios.me/storage/avatars/sethios.jpg"
        },
        "summary": "Expert en intelligence artificielle avec 10 ans d'expérience...",
        "skills": [
            {"name": "Python", "level": 5, "years": 8, "category": "technical"},
            {"name": "Machine Learning", "level": 5, "years": 7, "category": "technical"}
        ],
        "experiences": [
            {
                "company": "TechCorp",
                "position": "Lead AI Engineer",
                "start_date": "2022-01",
                "end_date": null,
                "is_current": true,
                "description": "Direction de l'équipe IA..."
            }
        ],
        "education": [
            {
                "institution": "Université de Paris",
                "degree": "Doctorat en Intelligence Artificielle",
                "field": "Machine Learning",
                "year": 2018
            }
        ],
        "languages": [
            {"name": "Français", "level": "Natif"},
            {"name": "Anglais", "level": "C2"}
        ],
        "certifications": [
            {"name": "TensorFlow Developer", "organization": "Google", "year": 2023}
        ],
        "social_links": {
            "github": "https://github.com/sethios",
            "linkedin": "https://linkedin.com/in/sethios",
            "twitter": "https://twitter.com/sethios",
            "google_scholar": "https://scholar.google.com/citations?user=sethios"
        }
    }
}
```

### IA Lab

#### Liste des modèles IA
```
GET /api/v1/ai-models
```

**Paramètres** :
| Paramètre | Type | Défaut | Description |
|-----------|------|--------|-------------|
| page | integer | 1 | Numéro de page |
| type | string | - | Type (nlp, computer_vision, etc.) |
| framework | string | - | Framework (tensorflow, pytorch, etc.) |
| status | string | - | Statut (development, deployed) |
| search | string | - | Recherche |

#### Modèle spécifique
```
GET /api/v1/ai-models/{slug}
```

#### Prédiction (si API disponible)
```
POST /api/v1/ai-models/{slug}/predict
Content-Type: application/json

{
    "input": "données d'entrée spécifiques au modèle"
}
```

**Réponse** :
```json
{
    "success": true,
    "data": {
        "model": "sentiment-analysis-v1",
        "version": "v1.2.0",
        "input": "Ce produit est excellent !",
        "prediction": "positive",
        "confidence": 0.95,
        "processing_time_ms": 45.2,
        "created_at": "2026-07-01T12:00:00Z"
    }
}
```

### Témoignages

#### Liste des témoignages
```
GET /api/v1/testimonials
```

### Contact

#### Envoi message
```
POST /api/v1/contact
Content-Type: application/json

{
    "name": "Jean Dupont",
    "email": "jean@example.com",
    "subject": "Proposition de collaboration",
    "message": "Bonjour, je souhaiterais...",
    "type": "collaboration",
    "company": "TechCorp",
    "phone": "+33612345678"
}
```

### Newsletter

#### Inscription
```
POST /api/v1/newsletter/subscribe
Content-Type: application/json

{
    "email": "subscriber@example.com",
    "name": "Marie Martin",
    "locale": "fr"
}
```

---

## Endpoints Authentifiés

### Contrats

#### Liste des contrats (client connecté)
```
GET /api/v1/contracts
Authorization: Bearer 1|sethios_abc123...
```

#### Contrat spécifique
```
GET /api/v1/contracts/{uuid}
Authorization: Bearer 1|sethios_abc123...
```

#### Messages d'un contrat
```
GET /api/v1/contracts/{uuid}/messages
Authorization: Bearer 1|sethios_abc123...
```

#### Envoyer un message
```
POST /api/v1/contracts/{uuid}/messages
Authorization: Bearer 1|sethios_abc123...
Content-Type: application/json

{
    "message": "Voici les documents demandés..."
}
```

#### Upload fichier contrat
```
POST /api/v1/contracts/{uuid}/files
Authorization: Bearer 1|sethios_abc123...
Content-Type: multipart/form-data

file: document.pdf
```

### Utilisateur

#### Mise à jour profil
```
PUT /api/v1/user/profile
Authorization: Bearer 1|sethios_abc123...
Content-Type: application/json

{
    "name": "Sethios",
    "bio": "Nouvelle bio...",
    "website": "https://sethios.me"
}
```

#### Upload avatar
```
POST /api/v1/user/avatar
Authorization: Bearer 1|sethios_abc123...
Content-Type: multipart/form-data

avatar: photo.jpg
```

---

## Rate Limiting

### Limites par type d'accès
| Type | Requêtes/minute | Requêtes/jour |
|------|-----------------|---------------|
| Public non authentifié | 60 | 10 000 |
| Utilisateur authentifié | 300 | 50 000 |
| API IA Lab (prédictions) | 30 | 1 000 |
| Admin | 1000 | Illimité |

### Headers de rate limit
```
X-RateLimit-Limit: 60
X-RateLimit-Remaining: 45
X-RateLimit-Reset: 1690000000
Retry-After: 60
```

---

## Versioning

### Stratégie
- Version dans l'URL : `/api/v1/`, `/api/v2/`
- Changements majeurs = nouvelle version
- Anciennes versions maintenues 12 mois après sortie nouvelle version
- Dépréciation annoncée par header : `Sunset: Sat, 31 Dec 2027 23:59:59 GMT`
- Documentation versionnée : `/api/docs/v1`, `/api/docs/v2`

---

## Documentation OpenAPI

### Spécification
- Format : OpenAPI 3.1
- Disponible : `/api/docs/v1/openapi.json`
- Interface Swagger UI : `/api/docs/v1`
- Interface ReDoc : `/api/docs/v1/redoc`

### Structure documentation
```yaml
openapi: 3.1.0
info:
  title: Sethios.me API
  version: 1.0.0
  description: API publique de la plateforme Sethios.me
  contact:
    name: Sethios
    email: api@sethios.me
    url: https://sethios.me
servers:
  - url: https://sethios.me/api/v1
    description: Serveur de production
  - url: https://staging.sethios.me/api/v1
    description: Serveur de staging
paths:
  /articles:
    get:
      summary: Liste des articles
      parameters:
        - name: page
          in: query
          schema:
            type: integer
      responses:
        '200':
          description: Liste paginée d'articles
  /articles/{slug}:
    get:
      summary: Article spécifique
      parameters:
        - name: slug
          in: path
          required: true
          schema:
            type: string
      responses:
        '200':
          description: Article complet
        '404':
          description: Article non trouvé
```

---

## Exemples de Code Client

### JavaScript (Fetch)
```javascript
// Récupérer les articles
fetch('https://sethios.me/api/v1/articles?category=intelligence-artificielle&per_page=5')
  .then(response => response.json())
  .then(data => {
    console.log(data.data);
    console.log(`Page ${data.meta.current_page}/${data.meta.last_page}`);
  });

// Prédiction IA
fetch('https://sethios.me/api/v1/ai-models/sentiment-analysis/predict', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer 1|sethios_your_token_here'
  },
  body: JSON.stringify({
    input: "Ce produit est excellent !"
  })
})
.then(response => response.json())
.then(data => console.log(data.data.prediction));
```

### Python (requests)
```python
import requests

# Récupérer les projets
response = requests.get(
    'https://sethios.me/api/v1/projects',
    params={'technology': 'python', 'difficulty': 4}
)
data = response.json()
for project in data['data']:
    print(f"{project['title']} - {project['status']}")

# Prédiction IA
headers = {
    'Authorization': 'Bearer 1|sethios_your_token_here',
    'Content-Type': 'application/json'
}
response = requests.post(
    'https://sethios.me/api/v1/ai-models/sentiment-analysis/predict',
    headers=headers,
    json={'input': 'Ce produit est excellent !'}
)
print(response.json()['data']['prediction'])
```

### PHP (Laravel HTTP Client)
```php
use Illuminate\Support\Facades\Http;

// Récupérer les articles
$response = Http::get('https://sethios.me/api/v1/articles', [
    'category' => 'intelligence-artificielle',
    'per_page' => 10
]);
$articles = $response->json('data');

// Prédiction IA avec token
$response = Http::withToken('1|sethios_your_token_here')
    ->post('https://sethios.me/api/v1/ai-models/sentiment-analysis/predict', [
        'input' => 'Ce produit est excellent !'
    ]);
$prediction = $response->json('data.prediction');
```

### cURL
```bash
# Liste articles
curl -X GET "https://sethios.me/api/v1/articles?per_page=5&sort=views_count" \
  -H "Accept: application/json"

# Article spécifique
curl -X GET "https://sethios.me/api/v1/articles/introduction-machine-learning" \
  -H "Accept: application/json"

# Prédiction IA
curl -X POST "https://sethios.me/api/v1/ai-models/sentiment-analysis/predict" \
  -H "Authorization: Bearer 1|sethios_your_token_here" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -d '{"input": "Ce produit est excellent !"}'
```

---

## Webhooks (Futur)

### Événements disponibles (v1.1)
- `article.published` : Nouvel article publié
- `project.completed` : Projet marqué comme terminé
- `contract.signed` : Contrat signé par les deux parties
- `ai_model.deployed` : Nouveau modèle IA déployé

### Configuration webhook
```
POST /api/v1/webhooks
Authorization: Bearer 1|sethios_abc123...
Content-Type: application/json

{
    "url": "https://example.com/webhook",
    "events": ["article.published", "project.completed"],
    "secret": "webhook_secret_key"
}
```

---

## Erreurs communes

### Rate limit dépassé
```json
{
    "success": false,
    "message": "Trop de requêtes. Veuillez réessayer dans 45 secondes.",
    "code": "RATE_LIMIT_EXCEEDED",
    "retry_after": 45
}
```

### Token invalide
```json
{
    "success": false,
    "message": "Token d'authentification invalide ou expiré.",
    "code": "INVALID_TOKEN"
}
```

### Ressource non trouvée
```json
{
    "success": false,
    "message": "L'article demandé n'existe pas.",
    "code": "NOT_FOUND"
}
```

### Validation échouée
```json
{
    "success": false,
    "message": "Données invalides.",
    "errors": {
        "email": ["Le champ email est obligatoire."],
        "message": ["Le message doit contenir au moins 10 caractères."]
    },
    "code": "VALIDATION_ERROR"
}
```

---

## Monitoring API

### Métriques suivies
- Nombre de requêtes par endpoint
- Temps de réponse moyen
- Taux d'erreur
- Répartition par statut HTTP
- Utilisateurs les plus actifs
- Endpoints les plus utilisés
- Prédictions IA (volume, latence)

### Logs
- Toutes les requêtes API loguées (sauf body pour raisons sécurité)
- Erreurs détaillées avec stack trace
- Alertes si taux d'erreur > 5%
- Dashboard dans Filament

---
