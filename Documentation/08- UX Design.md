# 08 - UI/UX Design

## Philosophie de Design

### Principes Fondamentaux
- **Clarté avant tout** : Information hiérarchisée, navigation évidente, pas de surcharge cognitive
- **Élégance technique** : Design qui reflète l'expertise technologique, interfaces précises et soignées
- **Accessibilité universelle** : Toute personne peut accéder au contenu indépendamment de ses capacités
- **Performance perçue** : Animations fluides, retours instantanés, jamais d'attente sans indication
- **Cohérence absolue** : Même composant = même comportement partout, langage visuel uniforme

### Personnalité Visuelle
- Moderne mais intemporel
- Technique sans être froid
- Professionnel avec une touche personnelle
- Innovant et audacieux par petites touches
- Sobre avec des accents calculés


## Système de Couleurs

### Palette Principale
``````css
:root {
  /* Primaire - Bleu profond technique */
  --color-primary-50: #EFF6FF;
  --color-primary-100: #DBEAFE;
  --color-primary-200: #BFDBFE;
  --color-primary-300: #93C5FD;
  --color-primary-400: #60A5FA;
  --color-primary-500: #3B82F6;
  --color-primary-600: #2563EB;
  --color-primary-700: #1D4ED8;
  --color-primary-800: #1E40AF;
  --color-primary-900: #1E3A8A;
  --color-primary-950: #172554;
}

  {/* Secondaire - Violet innovation */
  --color-secondary-50: #F5F3FF;
  --color-secondary-100: #EDE9FE;
  --color-secondary-200: #DDD6FE;
  --color-secondary-300: #C4B5FD;
  --color-secondary-400: #A78BFA;
  --color-secondary-500: #8B5CF6;
  --color-secondary-600: #7C3AED;
  --color-secondary-700: #6D28D9;
  --color-secondary-800: #5B21B6;
  --color-secondary-900: #4C1D95;
  --color-secondary-950: #2E1065;

    /* Accent - Émeraude actions */
  --color-accent-50: #ECFDF5;
  --color-accent-100: #D1FAE5;
  --color-accent-200: #A7F3D0;
  --color-accent-300: #6EE7B7;
  --color-accent-400: #34D399;
  --color-accent-500: #10B981;
  --color-accent-600: #059669;
  --color-accent-700: #047857;
  --color-accent-800: #065F46;
  --color-accent-900: #064E3B;

  /* Neutres */
  --color-gray-50: #F9FAFB;
  --color-gray-100: #F3F4F6;
  --color-gray-200: #E5E7EB;
  --color-gray-300: #D1D5DB;
  --color-gray-400: #9CA3AF;
  --color-gray-500: #6B7280;
  --color-gray-600: #4B5563;
  --color-gray-700: #374151;
  --color-gray-800: #1F2937;
  --color-gray-900: #111827;
  --color-gray-950: #030712;

  /* Sémantiques */
  --color-success: #10B981;
  --color-warning: #F59E0B;
  --color-error: #EF4444;
  --color-info: #3B82F6;
}
.dark {
  --bg-primary: #0F172A;
  --bg-secondary: #1E293B;
  --bg-tertiary: #334155;
  --text-primary: #F1F5F9;
  --text-secondary: #CBD5E1;
  --text-tertiary: #94A3B8;
  --border-color: #334155;
  --shadow-color: rgba(0, 0, 0, 0.5);
}
:root {
  --bg-primary: #FFFFFF;
  --bg-secondary: #F9FAFB;
  --bg-tertiary: #F3F4F6;
  --text-primary: #111827;
  --text-secondary: #4B5563;
  --text-tertiary: #9CA3AF;
  --border-color: #E5E7EB;
  --shadow-color: rgba(0, 0, 0, 0.1);
} 
```
--gradient-primary: linear-gradient(135deg, #3B82F6 0%, #8B5CF6 100%);
--gradient-hero: linear-gradient(135deg, #1E3A8A 0%, #4C1D95 50%, #064E3B 100%);
--gradient-card: linear-gradient(180deg, rgba(255,255,255,0) 0%, rgba(255,255,255,0.05) 100%);
--gradient-text: linear-gradient(135deg, #60A5FA 0%, #A78BFA 100%);

## Typographie

### Familles de Polices

```css
/* Police principale - Interface et corps de texte */
--font-sans: 'Inter', system-ui, -apple-system, sans-serif;

/* Police secondaire - Titres et accents */
--font-display: 'Plus Jakarta Sans', 'Inter', sans-serif;

/* Police monospace - Code et données techniques */
--font-mono: 'JetBrains Mono', 'Fira Code', monospace;

/* Police scientifique - Équations mathématiques */
--font-math: 'KaTeX_Main', 'Times New Roman', serif;

--text-xs: 0.75rem;    /* 12px - Labels, badges, légendes */
--text-sm: 0.875rem;   /* 14px - Texte secondaire, métadonnées */
--text-base: 1rem;     /* 16px - Corps de texte */
--text-lg: 1.125rem;   /* 18px - Texte accentué */
--text-xl: 1.25rem;    /* 20px - Sous-titres */
--text-2xl: 1.5rem;    /* 24px - Titres sections */
--text-3xl: 1.875rem;  /* 30px - Titres pages */
--text-4xl: 2.25rem;   /* 36px - Grands titres */
--text-5xl: 3rem;      /* 48px - Hero titre */
--text-6xl: 3.75rem;   /* 60px - Hero principal */
--text-7xl: 4.5rem;    /* 72px - Landing pages */

```

## Échelle Typographique
``````css
--text-xs: 0.75rem;    /* 12px - Labels, badges, légendes */
--text-sm: 0.875rem;   /* 14px - Texte secondaire, métadonnées */
--text-base: 1rem;     /* 16px - Corps de texte */
--text-lg: 1.125rem;   /* 18px - Texte accentué */
--text-xl: 1.25rem;    /* 20px - Sous-titres */
--text-2xl: 1.5rem;    /* 24px - Titres sections */
--text-3xl: 1.875rem;  /* 30px - Titres pages */
--text-4xl: 2.25rem;   /* 36px - Grands titres */
--text-5xl: 3rem;      /* 48px - Hero titre */
--text-6xl: 3.75rem;   /* 60px - Hero principal */
--text-7xl: 4.5rem;    /* 72px - Landing pages */
```

## Hauteurs de Ligne
```css
--leading-tight: 1.25;   /* Titres */
--leading-snug: 1.375;   /* Sous-titres */
--leading-normal: 1.6;   /* Corps de texte */
--leading-relaxed: 1.75; /* Texte long */
--leading-loose: 2;      /* Très aéré */

```
## Poids de Police
```css
--font-thin: 100;
--font-extralight: 200;
--font-light: 300;
--font-normal: 400;
--font-medium: 500;
--font-semibold: 600;
--font-bold: 700;
--font-extrabold: 800;
--font-black: 900;
Espacements
Système d'Espacement (8px base)
```css
--space-0: 0;
--space-1: 0.25rem;   /* 4px */
--space-2: 0.5rem;    /* 8px */
--space-3: 0.75rem;   /* 12px */
--space-4: 1rem;      /* 16px */
--space-5: 1.25rem;   /* 20px */
--space-6: 1.5rem;    /* 24px */
--space-8: 2rem;      /* 32px */
--space-10: 2.5rem;   /* 40px */
--space-12: 3rem;     /* 48px */
--space-16: 4rem;     /* 64px */
--space-20: 5rem;     /* 80px */
--space-24: 6rem;     /* 96px */
--space-32: 8rem;     /* 128px */
Container Max-Widths
```css
--container-sm: 640px;
--container-md: 768px;
--container-lg: 1024px;
--container-xl: 1280px;
--container-2xl: 1536px;
--container-content: 720px;  /* Largeur optimale lecture */
Grille
```css
--grid-columns: 12;
--grid-gap: 2rem;
--grid-gap-sm: 1rem;
--grid-gap-lg: 3rem;
Bordures et Ombres
Border Radius
```css
--radius-none: 0;
--radius-sm: 0.125rem;   /* 2px */
--radius-md: 0.375rem;   /* 6px */
--radius-lg: 0.5rem;     /* 8px */
--radius-xl: 0.75rem;    /* 12px */
--radius-2xl: 1rem;      /* 16px */
--radius-3xl: 1.5rem;    /* 24px */
--radius-full: 9999px;   /* Cercles, pills */
Ombres
```css
--shadow-xs: 0 1px 2px rgba(0, 0, 0, 0.05);
--shadow-sm: 0 1px 3px rgba(0, 0, 0, 0.1), 0 1px 2px rgba(0, 0, 0, 0.06);
--shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -2px rgba(0, 0, 0, 0.1);
--shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -4px rgba(0, 0, 0, 0.1);
--shadow-xl: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 8px 10px -6px rgba(0, 0, 0, 0.1);
--shadow-2xl: 0 25px 50px -12px rgba(0, 0, 0, 0.25);
--shadow-inner: inset 0 2px 4px 0 rgba(0, 0, 0, 0.05);
--shadow-glow: 0 0 20px rgba(59, 130, 246, 0.5);
--shadow-glow-purple: 0 0 20px rgba(139, 92, 246, 0.5);
Composants UI
Boutons
```css
/* Tailles */
.btn-sm { padding: 0.375rem 0.75rem; font-size: 0.875rem; }
.btn-md { padding: 0.625rem 1.25rem; font-size: 0.875rem; }
.btn-lg { padding: 0.75rem 1.5rem; font-size: 1rem; }
.btn-xl { padding: 1rem 2rem; font-size: 1.125rem; }

/* Variantes */
.btn-primary { background: var(--color-primary-600); color: white; }
.btn-secondary { background: var(--color-secondary-600); color: white; }
.btn-outline { border: 2px solid var(--color-primary-600); color: var(--color-primary-600); }
.btn-ghost { background: transparent; color: var(--color-primary-600); }
.btn-danger { background: var(--color-error); color: white; }
.btn-success { background: var(--color-success); color: white; }

/* États */
.btn:hover { transform: translateY(-1px); box-shadow: var(--shadow-md); }
.btn:active { transform: translateY(0); }
.btn:focus-visible { outline: 2px solid var(--color-primary-400); outline-offset: 2px; }
.btn:disabled { opacity: 0.5; cursor: not-allowed; transform: none; }

/* Icône boutons */
.btn-icon { padding: 0.5rem; border-radius: var(--radius-full); }
.btn-icon-sm { padding: 0.375rem; }
.btn-icon-lg { padding: 0.75rem; }
Cards
```css
.card {
  background: var(--bg-primary);
  border: 1px solid var(--border-color);
  border-radius: var(--radius-xl);
  padding: var(--space-6);
  transition: all 0.3s ease;
}
.card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-xl);
  border-color: var(--color-primary-300);
}
.card-interactive { cursor: pointer; }
.card-featured { border-left: 4px solid var(--color-primary-500); }

/* Variantes */
.card-compact { padding: var(--space-4); }
.card-horizontal { display: flex; flex-direction: row; }
.card-glass {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.2);
}
Badges
```css
.badge {
  display: inline-flex;
  align-items: center;
  padding: 0.125rem 0.625rem;
  border-radius: var(--radius-full);
  font-size: var(--text-xs);
  font-weight: var(--font-medium);
  line-height: 1.25rem;
}
.badge-primary { background: var(--color-primary-100); color: var(--color-primary-700); }
.badge-success { background: var(--color-accent-100); color: var(--color-accent-700); }
.badge-warning { background: #FEF3C7; color: #92400E; }
.badge-error { background: #FEE2E2; color: #991B1B; }
.badge-outline { background: transparent; border: 1px solid currentColor; }
.badge-with-dot::before {
  content: '';
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: currentColor;
  margin-right: 0.375rem;
}
Inputs
```css
.input {
  width: 100%;
  padding: 0.625rem 0.875rem;
  background: var(--bg-primary);
  border: 1px solid var(--border-color);
  border-radius: var(--radius-lg);
  font-size: var(--text-base);
  color: var(--text-primary);
  transition: border-color 0.15s ease, box-shadow 0.15s ease;
}
.input:focus {
  outline: none;
  border-color: var(--color-primary-400);
  box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.1);
}
.input-error {
  border-color: var(--color-error);
}
.input-error:focus {
  box-shadow: 0 0 0 3px rgba(239, 68, 68, 0.1);
}
.input-disabled {
  background: var(--bg-tertiary);
  cursor: not-allowed;
  opacity: 0.6;
}
.input-with-icon {
  padding-left: 2.5rem;
}
.input-icon {
  position: absolute;
  left: 0.75rem;
  top: 50%;
  transform: translateY(-50%);
  color: var(--text-tertiary);
}
Modales
```css
.modal-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(4px);
  z-index: 50;
  display: flex;
  align-items: center;
  justify-content: center;
  animation: fadeIn 0.2s ease;
}
.modal-content {
  background: var(--bg-primary);
  border-radius: var(--radius-2xl);
  padding: var(--space-8);
  max-width: 90vw;
  max-height: 90vh;
  overflow-y: auto;
  animation: slideUp 0.3s ease;
}
.modal-sm { width: 400px; }
.modal-md { width: 560px; }
.modal-lg { width: 720px; }
.modal-xl { width: 960px; }
Tooltips
```css
.tooltip {
  position: relative;
}
.tooltip::after {
  content: attr(data-tooltip);
  position: absolute;
  bottom: 100%;
  left: 50%;
  transform: translateX(-50%);
  padding: 0.25rem 0.5rem;
  background: var(--color-gray-900);
  color: white;
  border-radius: var(--radius-md);
  font-size: var(--text-xs);
  white-space: nowrap;
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.15s ease;
}
.tooltip:hover::after {
  opacity: 1;
}
Skeleton Loaders
```css
.skeleton {
  background: linear-gradient(90deg, var(--bg-tertiary) 25%, var(--bg-secondary) 50%, var(--bg-tertiary) 75%);
  background-size: 200% 100%;
  animation: shimmer 1.5s infinite;
  border-radius: var(--radius-md);
}
.skeleton-text { height: 1rem; margin-bottom: 0.5rem; }
.skeleton-title { height: 1.5rem; width: 60%; margin-bottom: 1rem; }
.skeleton-avatar { width: 3rem; height: 3rem; border-radius: var(--radius-full); }
.skeleton-card { height: 200px; }
.skeleton-image { height: 300px; }
Animations et Transitions
Durées
```css
--duration-instant: 100ms;
--duration-fast: 200ms;
--duration-normal: 300ms;
--duration-slow: 500ms;
--duration-very-slow: 1000ms;
Courbes d'Animation
```css
--ease-linear: cubic-bezier(0, 0, 1, 1);
--ease-in: cubic-bezier(0.4, 0, 1, 1);
--ease-out: cubic-bezier(0, 0, 0.2, 1);
--ease-in-out: cubic-bezier(0.4, 0, 0.2, 1);
--ease-spring: cubic-bezier(0.68, -0.55, 0.265, 1.55);
--ease-bounce: cubic-bezier(0.68, -0.6, 0.32, 1.6);
Animations Définies
```css
@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}

@keyframes fadeInDown {
  from { opacity: 0; transform: translateY(-20px); }
  to { opacity: 1; transform: translateY(0); }
}

@keyframes slideUp {
  from { transform: translateY(20px); opacity: 0; }
  to { transform: translateY(0); opacity: 1; }
}

@keyframes slideInRight {
  from { transform: translateX(20px); opacity: 0; }
  to { transform: translateX(0); opacity: 1; }
}

@keyframes scaleIn {
  from { transform: scale(0.95); opacity: 0; }
  to { transform: scale(1); opacity: 1; }
}

@keyframes shimmer {
  0% { background-position: -200% 0; }
  100% { background-position: 200% 0; }
}

@keyframes pulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.5; }
}

@keyframes spin {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}

@keyframes float {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-10px); }
}

@keyframes gradientShift {
  0% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }
}
Classes Utilitaires Animation
```css
.animate-fade-in { animation: fadeIn var(--duration-normal) var(--ease-out); }
.animate-fade-in-up { animation: fadeInUp var(--duration-normal) var(--ease-out); }
.animate-slide-up { animation: slideUp var(--duration-normal) var(--ease-out); }
.animate-scale-in { animation: scaleIn var(--duration-fast) var(--ease-out); }
.animate-float { animation: float 3s var(--ease-in-out) infinite; }
.animate-gradient { animation: gradientShift 3s ease infinite; background-size: 200% 200%; }

/* Stagger children */
.stagger > * { opacity: 0; animation: fadeInUp var(--duration-normal) var(--ease-out) forwards; }
.stagger > *:nth-child(1) { animation-delay: 0ms; }
.stagger > *:nth-child(2) { animation-delay: 100ms; }
.stagger > *:nth-child(3) { animation-delay: 200ms; }
.stagger > *:nth-child(4) { animation-delay: 300ms; }
.stagger > *:nth-child(5) { animation-delay: 400ms; }
.stagger > *:nth-child(6) { animation-delay: 500ms; }

/* Réduction animations (préférence utilisateur) */
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
Icônes
Bibliothèques
Heroicons (par Tailwind Labs) : Icônes UI principales

Phosphor Icons : Icônes supplémentaires variées

Simple Icons : Logos technologies et réseaux sociaux

Custom SVG : Icônes spécifiques Sethios (logo, IA, contrat)

Tailles Icônes
```css
.icon-xs { width: 0.75rem; height: 0.75rem; }
.icon-sm { width: 1rem; height: 1rem; }
.icon-md { width: 1.25rem; height: 1.25rem; }
.icon-lg { width: 1.5rem; height: 1.5rem; }
.icon-xl { width: 2rem; height: 2rem; }
.icon-2xl { width: 3rem; height: 3rem; }
Responsive Design
Breakpoints
```css
/* Mobile first */
--screen-sm: 640px;   /* @media (min-width: 640px) */
--screen-md: 768px;   /* @media (min-width: 768px) */
--screen-lg: 1024px;  /* @media (min-width: 1024px) */
--screen-xl: 1280px;  /* @media (min-width: 1280px) */
--screen-2xl: 1536px; /* @media (min-width: 1536px) */
Grilles Responsives
```css
/* Colonnes automatiques */
.grid-auto-fit {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: var(--grid-gap);
}

.grid-auto-fill {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: var(--grid-gap);
}

/* Grilles spécifiques */
.grid-1 { grid-template-columns: repeat(1, 1fr); }
.grid-2 { grid-template-columns: repeat(2, 1fr); }
.grid-3 { grid-template-columns: repeat(3, 1fr); }
.grid-4 { grid-template-columns: repeat(4, 1fr); }

@media (max-width: 1024px) {
  .grid-4 { grid-template-columns: repeat(2, 1fr); }
}
@media (max-width: 640px) {
  .grid-2, .grid-3, .grid-4 { grid-template-columns: 1fr; }
}
Navigation Responsive
```css
/* Desktop */
.nav-desktop { display: flex; }

/* Mobile */
.nav-mobile {
  display: none;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  background: var(--bg-primary);
  z-index: 100;
  padding: var(--space-6);
  overflow-y: auto;
}

@media (max-width: 1024px) {
  .nav-desktop { display: none; }
  .nav-mobile.open { display: block; animation: slideInRight var(--duration-normal) var(--ease-out); }
  .hamburger { display: flex; }
}
Typographie Responsive
```css
/* Fluid typography */
--text-fluid-sm: clamp(0.875rem, 0.8rem + 0.25vw, 1rem);
--text-fluid-base: clamp(1rem, 0.9rem + 0.5vw, 1.125rem);
--text-fluid-lg: clamp(1.125rem, 1rem + 0.5vw, 1.25rem);
--text-fluid-xl: clamp(1.25rem, 1rem + 1vw, 1.5rem);
--text-fluid-2xl: clamp(1.5rem, 1.25rem + 1.5vw, 2rem);
--text-fluid-3xl: clamp(2rem, 1.5rem + 2vw, 3rem);
--text-fluid-4xl: clamp(2.5rem, 2rem + 3vw, 4rem);
--text-fluid-5xl: clamp(3rem, 2.5rem + 4vw, 5rem);
Pages Clés - Layouts
Page d'Accueil
text
┌─────────────────────────────────────────────────┐
│                   NAVIGATION                     │
├─────────────────────────────────────────────────┤
│                                                 │
│              HERO SECTION                        │
│    [Photo] [Titre animé] [CTA] [Particules]     │
│                                                 │
├─────────────────────────────────────────────────┤
│           PROJETS RÉCENTS (grille 3)            │
│   ┌──────┐   ┌──────┐   ┌──────┐              │
│   │ Card │   │ Card │   │ Card │              │
│   └──────┘   └──────┘   └──────┘              │
├─────────────────────────────────────────────────┤
│           DERNIERS ARTICLES (grille 2)          │
│   ┌────────────┐   ┌────────────┐              │
│   │   Card     │   │   Card     │              │
│   └────────────┘   └────────────┘              │
├─────────────────────────────────────────────────┤
│        STATISTIQUES (compteurs animés)          │
│    [Projets]  [Articles]  [Contrats]  [Ans]    │
├─────────────────────────────────────────────────┤
│         TÉMOIGNAGES (carrousel)                 │
├─────────────────────────────────────────────────┤
│         APERÇU IA LAB + BIBLIOTHÈQUE            │
├─────────────────────────────────────────────────┤
│              NEWSLETTER                          │
├─────────────────────────────────────────────────┤
│                   FOOTER                         │
└─────────────────────────────────────────────────┘
Page Article
text
┌─────────────────────────────────────────────────┐
│                   NAVIGATION                     │
├──────────────────┬──────────────────────────────┤
│                  │                              │
│   TABLE DES      │    CONTENU ARTICLE           │
│   MATIÈRES       │                              │
│   (sticky)       │    [Image couverture]        │
│                  │                              │
│   1. Intro       │    [Métadonnées]             │
│   2. Section     │                              │
│   3. Section     │    [Texte Markdown]          │
│   4. Section     │    [Code blocks]             │
│   5. Conclusion  │    [Équations KaTeX]          │
│                  │    [Images]                   │
│   [Progression]  │                              │
│                  │    [Tags]                    │
│   [Partage]      │    [Articles liés]           │
│                  │    [Commentaires]            │
├──────────────────┴──────────────────────────────┤
│                   FOOTER                         │
└─────────────────────────────────────────────────┘
Page Portfolio
text
┌─────────────────────────────────────────────────┐
│                   NAVIGATION                     │
├─────────────────────────────────────────────────┤
│   [Filtres]    [Recherche]    [Tri]  [Vue]      │
├─────────────────────────────────────────────────┤
│   ┌──────┐   ┌──────┐   ┌──────┐              │
│   │Projet│   │Projet│   │Projet│              │
│   │  1   │   │  2   │   │  3   │              │
│   └──────┘   └──────┘   └──────┘              │
│   ┌──────┐   ┌──────┐   ┌──────┐              │
│   │Projet│   │Projet│   │Projet│              │
│   │  4   │   │  5   │   │  6   │              │
│   └──────┘   └──────┘   └──────┘              │
├─────────────────────────────────────────────────┤
│               [Charger plus]                     │
├─────────────────────────────────────────────────┤
│                   FOOTER                         │
└─────────────────────────────────────────────────┘
États UI
État Vide
html
<div class="empty-state">
  <div class="empty-state-icon">
    <!-- Icône illustrative -->
  </div>
  <h3 class="empty-state-title">Titre contexte</h3>
  <p class="empty-state-description">Message encourageant avec action possible</p>
  <button class="btn btn-primary">Action</button>
</div>
État Chargement
html
<div class="loading-state">
  <div class="spinner"></div>
  <p class="loading-text">Chargement en cours...</p>
  <div class="skeleton-grid">
    <div class="skeleton skeleton-card"></div>
    <div class="skeleton skeleton-card"></div>
    <div class="skeleton skeleton-card"></div>
  </div>
</div>
État Erreur
html
<div class="error-state">
  <div class="error-icon">⚠️</div>
  <h3>Une erreur est survenue</h3>
  <p>Message d'erreur spécifique</p>
  <button class="btn btn-primary">Réessayer</button>
  <button class="btn btn-ghost">Retour à l'accueil</button>
</div>
État Succès
html
<div class="success-state">
  <div class="success-icon">✅</div>
  <h3>Opération réussie !</h3>
  <p>Message de confirmation</p>
  <button class="btn btn-primary">Continuer</button>
</div>
Accessibilité UI
Focus Visible
```css
*:focus-visible {
  outline: 2px solid var(--color-primary-500);
  outline-offset: 2px;
  border-radius: var(--radius-sm);
}
Skip Link
html
<a href="#main-content" class="skip-link">
  Aller au contenu principal
</a>
```css
.skip-link {
  position: absolute;
  top: -100%;
  left: 1rem;
  background: var(--color-primary-600);
  color: white;
  padding: 0.5rem 1rem;
  z-index: 200;
  border-radius: var(--radius-md);
}
.skip-link:focus {
  top: 1rem;
}
Contrastes Minimum
Texte normal : ratio 4.5:1 minimum

Grand texte (>18px ou >14px bold) : ratio 3:1 minimum

Composants UI et graphiques : ratio 3:1 minimum

Tailles de Touche
Minimum 44x44px pour toutes les cibles tactiles

Espacement minimum 8px entre éléments interactifs

Dark Mode Spécifique
Images en Dark Mode
```css
/* Réduire luminosité images en dark mode */
.dark img:not(.no-dark-filter) {
  filter: brightness(0.9) contrast(1.1);
}

/* Inverser logos si nécessaire */
.dark .logo-invert {
  filter: invert(1);
}
Code Blocks en Dark Mode
```css
.dark pre {
  background: #1E293B !important;
  border-color: #334155;
}
.dark code {
  color: #E2E8F0;
}
Tableaux en Dark Mode
```css
.dark table {
  border-color: #334155;
}
.dark th {
  background: #1E293B;
}
.dark tr:nth-child(even) {
  background: rgba(255, 255, 255, 0.02);
}
