```markdown
# 11 - SEO

## Stratégie SEO Globale

### Objectifs
- Positionnement top 3 sur les mots-clés cibles liés à l'IA, au développement et à la recherche
- Trafic organique qualifié (recruteurs, clients, collaborateurs)
- Autorité de domaine (Domain Authority) croissante via contenu expert
- Rich snippets dans les SERP (articles, projets, CV, livres)
- Indexation complète et rapide du contenu

### Mots-clés prioritaires
- **Brand** : Sethios, Sethios.me
- **Expertise** : chercheur IA, développeur IA, consultant machine learning, expert en intelligence artificielle
- **Techniques** : deep learning, NLP, computer vision, transformer models, LLM, RAG
- **Services** : freelance IA, consulting IA, formation IA, développement IA sur mesure
- **Longue traîne** : articles scientifiques accessibles, bibliothèque numérique IA, tutoriels machine learning avancé

---

## SEO On-Page

### Structure des URLs
```
https://sethios.me/
https://sethios.me/blog/
https://sethios.me/blog/{slug}/
https://sethios.me/portfolio/
https://sethios.me/portfolio/{slug}/
https://sethios.me/publications/
https://sethios.me/publications/{slug}/
https://sethios.me/library/
https://sethios.me/library/{slug}/
https://sethios.me/cv/
https://sethios.me/ia-lab/
https://sethios.me/ia-lab/{slug}/
https://sethios.me/contact/
https://sethios.me/about/
https://sethios.me/opportunities/
```

**Règles** :
- URLs en minuscules
- Sans accents (slug translittéré)
- Mots séparés par tirets
- Pas de paramètres inutiles
- Trailing slash cohérent
- Canonical URL absolue sur chaque page

### Balises Meta

#### Configuration par type de page

**Page d'accueil** :
```html
<title>Sethios - Chercheur & Développeur en Intelligence Artificielle</title>
<meta name="description" content="Portfolio, blog scientifique et services en IA de Sethios. Découvrez mes projets, publications et expertises en machine learning, deep learning et NLP.">
<meta name="keywords" content="Sethios, IA, intelligence artificielle, machine learning, deep learning, NLP, chercheur, développeur, freelance, consultant">
<link rel="canonical" href="https://sethios.me/">
```

**Article de blog** :
```html
<title>{Titre article} - Blog Sethios</title>
<meta name="description" content="{Extrait article 150-160 caractères}">
<meta name="keywords" content="{Tags article}">
<meta name="author" content="Sethios">
<link rel="canonical" href="https://sethios.me/blog/{slug}/">
```

**Projet portfolio** :
```html
<title>{Titre projet} - Portfolio Sethios</title>
<meta name="description" content="{Description projet 150-160 caractères}">
<meta name="keywords" content="{Technologies utilisées}">
<link rel="canonical" href="https://sethios.me/portfolio/{slug}/">
```

### Hiérarchie des titres (H1-H6)
- **H1** : Un seul par page, contient le mot-clé principal
- **H2** : Sections principales, mots-clés secondaires
- **H3** : Sous-sections
- **H4-H6** : Détails si nécessaire
- Structure pyramidale logique
- Pas de saut de niveau

### Optimisation des images
- Noms de fichiers descriptifs : `introduction-machine-learning-illustration.jpg`
- Attributs alt obligatoires et descriptifs
- Images compressées (WebP prioritaire, JPEG fallback)
- Dimensions adaptées (srcset pour responsive)
- Lazy loading natif (`loading="lazy"`)
- Images Open Graph dédiées (1200x630px)
- Sitemap image inclus

### Maillage interne
- Articles liés automatiquement (mêmes catégories, tags)
- Projets liés depuis articles
- Publications liées depuis projets
- Fil d'Ariane structuré sur toutes les pages
- Menu principal avec liens vers pages clés
- Footer avec liens importants
- Nuage de tags dans sidebar blog

### Contenu
- Articles > 1000 mots pour sujets approfondis
- Densité mots-clés naturelle (1-3%)
- Paragraphes courts (2-3 phrases)
- Listes à puces pour lisibilité
- Tableaux pour données structurées
- Citations et sources externes
- Mise à jour régulière du contenu existant

---

## Open Graph et Twitter Cards

### Configuration Open Graph
```html
<!-- Toutes les pages -->
<meta property="og:site_name" content="Sethios">
<meta property="og:locale" content="fr_FR">
<meta property="og:type" content="website">

<!-- Page d'accueil -->
<meta property="og:title" content="Sethios - Chercheur & Développeur en IA">
<meta property="og:description" content="Portfolio, blog scientifique et services en IA.">
<meta property="og:url" content="https://sethios.me/">
<meta property="og:image" content="https://sethios.me/images/og-home.jpg">
<meta property="og:image:width" content="1200">
<meta property="og:image:height" content="630">

<!-- Article -->
<meta property="og:type" content="article">
<meta property="og:title" content="{Titre article}">
<meta property="og:description" content="{Extrait}">
<meta property="og:url" content="https://sethios.me/blog/{slug}/">
<meta property="og:image" content="https://sethios.me/storage/articles/{slug}-og.jpg">
<meta property="article:published_time" content="{Date publication ISO 8601}">
<meta property="article:author" content="Sethios">
<meta property="article:tag" content="{Tag 1}">
```

### Configuration Twitter Cards
```html
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:site" content="@sethios">
<meta name="twitter:creator" content="@sethios">
<meta name="twitter:title" content="{Titre}">
<meta name="twitter:description" content="{Description}">
<meta name="twitter:image" content="https://sethios.me/storage/twitter/{slug}.jpg">
```

---

## Données Structurées (Schema.org)

### Organisation
```json
{
    "@context": "https://schema.org",
    "@type": "Organization",
    "name": "Sethios",
    "url": "https://sethios.me",
    "logo": "https://sethios.me/images/logo.png",
    "sameAs": [
        "https://github.com/sethios",
        "https://linkedin.com/in/sethios",
        "https://twitter.com/sethios",
        "https://scholar.google.com/citations?user=sethios"
    ]
}
```

### Person (CV)
```json
{
    "@context": "https://schema.org",
    "@type": "Person",
    "name": "Sethios",
    "givenName": "Sethios",
    "url": "https://sethios.me",
    "image": "https://sethios.me/storage/avatars/sethios.jpg",
    "jobTitle": "Chercheur & Développeur en Intelligence Artificielle",
    "description": "Expert en IA avec X années d'expérience...",
    "email": "contact@sethios.me",
    "sameAs": [
        "https://github.com/sethios",
        "https://linkedin.com/in/sethios"
    ],
    "knowsAbout": [
        "Artificial Intelligence",
        "Machine Learning",
        "Deep Learning",
        "Natural Language Processing"
    ],
    "alumniOf": {
        "@type": "EducationalOrganization",
        "name": "Université de Paris"
    }
}
```

### Article (Blog)
```json
{
    "@context": "https://schema.org",
    "@type": "Article",
    "headline": "{Titre article}",
    "description": "{Extrait 150-160 caractères}",
    "image": "https://sethios.me/storage/articles/{slug}.jpg",
    "author": {
        "@type": "Person",
        "name": "Sethios",
        "url": "https://sethios.me"
    },
    "publisher": {
        "@type": "Organization",
        "name": "Sethios",
        "logo": {
            "@type": "ImageObject",
            "url": "https://sethios.me/images/logo.png"
        }
    },
    "datePublished": "{Date ISO 8601}",
    "dateModified": "{Date modification ISO 8601}",
    "mainEntityOfPage": {
        "@type": "WebPage",
        "@id": "https://sethios.me/blog/{slug}/"
    },
    "keywords": ["tag1", "tag2", "tag3"]
}
```

### Projet (Portfolio)
```json
{
    "@context": "https://schema.org",
    "@type": "SoftwareApplication",
    "name": "{Titre projet}",
    "description": "{Description}",
    "url": "https://sethios.me/portfolio/{slug}/",
    "applicationCategory": "DeveloperApplication",
    "operatingSystem": "Web",
    "author": {
        "@type": "Person",
        "name": "Sethios"
    },
    "offers": {
        "@type": "Offer",
        "price": "0",
        "priceCurrency": "EUR"
    }
}
```

### Publication Scientifique
```json
{
    "@context": "https://schema.org",
    "@type": "ScholarlyArticle",
    "headline": "{Titre publication}",
    "description": "{Abstract}",
    "author": [
        {
            "@type": "Person",
            "name": "Sethios"
        }
    ],
    "datePublished": "{Date}",
    "publisher": {
        "@type": "Organization",
        "name": "{Journal/Conférence}"
    },
    "identifier": {
        "@type": "PropertyValue",
        "propertyID": "DOI",
        "value": "{DOI}"
    }
}
```

### Livre (Bibliothèque)
```json
{
    "@context": "https://schema.org",
    "@type": "Book",
    "name": "{Titre}",
    "author": {
        "@type": "Person",
        "name": "{Auteur}"
    },
    "isbn": "{ISBN}",
    "publisher": "{Éditeur}",
    "datePublished": "{Année}",
    "inLanguage": "fr",
    "description": "{Description}"
}
```

### Fil d'Ariane
```json
{
    "@context": "https://schema.org",
    "@type": "BreadcrumbList",
    "itemListElement": [
        {
            "@type": "ListItem",
            "position": 1,
            "name": "Accueil",
            "item": "https://sethios.me/"
        },
        {
            "@type": "ListItem",
            "position": 2,
            "name": "Blog",
            "item": "https://sethios.me/blog/"
        },
        {
            "@type": "ListItem",
            "position": 3,
            "name": "{Titre article}",
            "item": "https://sethios.me/blog/{slug}/"
        }
    ]
}
```

### FAQ (Page Contact)
```json
{
    "@context": "https://schema.org",
    "@type": "FAQPage",
    "mainEntity": [
        {
            "@type": "Question",
            "name": "Comment proposer une collaboration ?",
            "acceptedAnswer": {
                "@type": "Answer",
                "text": "Utilisez le formulaire de contact..."
            }
        }
    ]
}
```

---

## Sitemap

### Sitemap XML principal
```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9"
        xmlns:image="http://www.google.com/schemas/sitemap-image/1.1"
        xmlns:news="http://www.google.com/schemas/sitemap-news/0.9">
    
    <!-- Pages statiques -->
    <url>
        <loc>https://sethios.me/</loc>
        <changefreq>weekly</changefreq>
        <priority>1.0</priority>
        <lastmod>2026-07-01</lastmod>
    </url>
    <url>
        <loc>https://sethios.me/blog/</loc>
        <changefreq>daily</changefreq>
        <priority>0.9</priority>
    </url>
    <url>
        <loc>https://sethios.me/portfolio/</loc>
        <changefreq>weekly</changefreq>
        <priority>0.9</priority>
    </url>
    <url>
        <loc>https://sethios.me/publications/</loc>
        <changefreq>monthly</changefreq>
        <priority>0.8</priority>
    </url>
    <url>
        <loc>https://sethios.me/library/</loc>
        <changefreq>monthly</changefreq>
        <priority>0.7</priority>
    </url>
    <url>
        <loc>https://sethios.me/cv/</loc>
        <changefreq>monthly</changefreq>
        <priority>0.8</priority>
    </url>
    <url>
        <loc>https://sethios.me/ia-lab/</loc>
        <changefreq>weekly</changefreq>
        <priority>0.8</priority>
    </url>
    <url>
        <loc>https://sethios.me/about/</loc>
        <changefreq>monthly</changefreq>
        <priority>0.7</priority>
    </url>
    <url>
        <loc>https://sethios.me/contact/</loc>
        <changefreq>monthly</changefreq>
        <priority>0.6</priority>
    </url>
    
    <!-- Articles -->
    <url>
        <loc>https://sethios.me/blog/introduction-machine-learning/</loc>
        <changefreq>monthly</changefreq>
        <priority>0.8</priority>
        <lastmod>2026-06-15</lastmod>
        <image:image>
            <image:loc>https://sethios.me/storage/articles/ml-intro.jpg</image:loc>
            <image:title>Introduction au Machine Learning</image:title>
        </image:image>
    </url>
</urlset>
```

### Sitemaps secondaires
- `sitemap-articles.xml` : Tous les articles (mise à jour quotidienne)
- `sitemap-projects.xml` : Tous les projets
- `sitemap-publications.xml` : Toutes les publications
- `sitemap-books.xml` : Tous les livres
- `sitemap-ai-models.xml` : Tous les modèles IA

### Index sitemap
```xml
<?xml version="1.0" encoding="UTF-8"?>
<sitemapindex xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
    <sitemap>
        <loc>https://sethios.me/sitemap-main.xml</loc>
        <lastmod>2026-07-01</lastmod>
    </sitemap>
    <sitemap>
        <loc>https://sethios.me/sitemap-articles.xml</loc>
        <lastmod>2026-07-01</lastmod>
    </sitemap>
    <sitemap>
        <loc>https://sethios.me/sitemap-projects.xml</loc>
        <lastmod>2026-07-01</lastmod>
    </sitemap>
    <sitemap>
        <loc>https://sethios.me/sitemap-publications.xml</loc>
        <lastmod>2026-07-01</lastmod>
    </sitemap>
</sitemapindex>
```

---

## Robots.txt

```text
User-agent: *
Allow: /
Disallow: /admin/
Disallow: /api/
Disallow: /horizon/
Disallow: /pulse/
Disallow: /telescope/

Sitemap: https://sethios.me/sitemap-index.xml

# Crawl-delay respectueux
Crawl-delay: 5

# Spécifique Googlebot
User-agent: Googlebot
Allow: /
Crawl-delay: 2

# Blocage IA crawlers (optionnel)
User-agent: GPTBot
Disallow: /

User-agent: CCBot
Disallow: /
```

---

## SEO Technique

### Performance
- Core Web Vitals optimisés :
  - LCP (Largest Contentful Paint) < 2.5s
  - FID (First Input Delay) < 100ms
  - CLS (Cumulative Layout Shift) < 0.1
- Score Lighthouse > 95
- Temps de chargement < 2 secondes
- TTFB (Time To First Byte) < 200ms

### HTTPS
- Certificat SSL/TLS à jour
- Redirection 301 HTTP vers HTTPS
- HSTS activé (max-age=31536000; includeSubDomains)
- Pas de contenu mixte

### Mobile-Friendly
- Design responsive validé
- Test Mobile-Friendly Google réussi
- Vitesse mobile optimisée
- Touch targets > 48px
- Pas de pop-ups intrusifs mobiles

### Balises hreflang
```html
<link rel="alternate" hreflang="fr" href="https://sethios.me/fr/blog/{slug}/">
<link rel="alternate" hreflang="en" href="https://sethios.me/en/blog/{slug}/">
<link rel="alternate" hreflang="x-default" href="https://sethios.me/blog/{slug}/">
```

### Pagination
```html
<link rel="prev" href="https://sethios.me/blog/?page=2">
<link rel="next" href="https://sethios.me/blog/?page=4">
```

### Redirections
- 301 pour les URLs modifiées (changement de slug)
- Redirections accumulées dans un fichier dédié
- Maximum 1 redirection par URL (pas de chaînes)
- Anciens slugs d'articles redirigés automatiquement

---

## Analyse et Suivi

### Google Search Console
- Vérification propriété
- Soumission sitemaps
- Surveillance indexation
- Analyse requêtes et positions
- Corrections erreurs d'exploration
- Core Web Vitals monitoring

### Outils SEO intégrés
- **Spatie Sitemap** : Génération automatique
- **Spatie Health** : Monitoring uptime et erreurs
- **Analytics dashboard** : Suivi trafic organique
- **SEO audit Filament** : Vérifications automatiques :
  - Balises title/meta manquantes
  - Images sans alt
  - URLs trop longues
  - Contenu dupliqué
  - Liens brisés

### KPIs SEO
| Métrique | Objectif | Fréquence |
|----------|---------|-----------|
| Trafic organique | +20%/mois | Hebdomadaire |
| Mots-clés top 10 | 50+ | Mensuel |
| Pages indexées | 95%+ | Mensuel |
| Taux de clic (CTR) | > 5% | Mensuel |
| Taux de rebond | < 50% | Mensuel |
| Backlinks | +10/mois | Mensuel |
| Domain Authority | > 30 an 1 | Trimestriel |

---

## Stratégie de Contenu SEO

### Calendrier éditorial
- 2 articles de blog minimum par mois
- Articles piliers (> 2000 mots) sur mots-clés stratégiques
- Articles secondaires ciblant longue traîne
- Mise à jour contenu ancien trimestriellement

### Types de contenu SEO-friendly
- Tutoriels techniques détaillés
- Études de cas projets IA
- Comparaisons frameworks/outils
- Guides débutant à expert
- Actualités IA commentées
- Glossaire technique

### Optimisation sémantique
- Recherche sémantique des entités liées
- Utilisation synonymes et termes connexes
- Réponse aux questions "People Also Ask"
- Contenu structuré pour featured snippets
- Paragraphes définitions optimisés

---

## Netlinking

### Stratégie backlinks
- Articles invités sur blogs tech reconnus
- Participation conférences avec pages dédiées
- Open source : liens depuis GitHub/NPM/Packagist
- Publications scientifiques citées
- Interviews et podcasts
- Partenariats académiques

### Liens sortants
- NoFollow sur liens sponsorisés
- Liens vers sources autoritaires (.edu, .gov, Wikipedia)
- Vérification périodique liens cassés
- Outbound link strategy mesurée

---

