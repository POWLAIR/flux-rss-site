# flux-rss-site — Étape 3/3 : Affichage

Site de veille technologique généré avec **Astro**, déployé sur **Vercel**. Consomme le `feed-enriched.json` produit par `flux-rss-enricher` au moment du build et génère un site statique entièrement client-side, sans serveur.

## Pipeline global

```
[1] flux-rss-framework  →  feed.xml           (GitHub Pages)
         ↓
[2] flux-rss-enricher   →  feed-enriched.json (GitHub Pages)
         ↓
[3] flux-rss-site       →  Site de veille     (Vercel)  ← vous êtes ici
```

## Fonctionnalités

- Grille de cartes articles : titre, source (badge coloré), date relative, auteur
- Tags par article (React, TypeScript, Security…) avec filtres cliquables
- Indicateur "trending" pour les articles à fort score de signal
- Recherche texte sur le titre
- Filtres combinables : tag + source + recherche (ET logique)
- Design dark mode, responsive

## Installation & développement local

```bash
# Installer les dépendances
npm install

# Lancer le serveur de développement
npm run dev
```

> Le build récupère le feed live depuis `https://powlair.github.io/flux-rss-enricher/feed-enriched.json`. Une connexion internet est nécessaire.

```bash
# Build de production
npm run build

# Prévisualiser le build
npm run preview
```

## Déploiement sur Vercel

### 1. Import du dépôt

1. Aller sur [vercel.com](https://vercel.com) → **Add New Project**
2. Importer le dépôt `POWLAIR/flux-rss-site`
3. Vercel détecte Astro automatiquement — aucune configuration de build à modifier
4. Cliquer **Deploy**

### 2. Rebuild automatique toutes les heures

Le rebuild est déclenché depuis GitHub Actions via un **Deploy Hook** Vercel :

**Côté Vercel :**
1. Settings du projet → **Git** → **Deploy Hooks**
2. Créer un hook nommé `hourly-rebuild` sur la branche `main`
3. Copier l'URL générée

**Côté GitHub :**
1. Settings du dépôt → **Secrets and variables** → **Actions**
2. Créer un secret `VERCEL_DEPLOY_HOOK` avec l'URL copiée

Le workflow `.github/workflows/deploy.yml` se déclenche alors à `cron: 10 * * * *` (10 minutes après la mise à jour du feed enrichi).

## Stack technique

| Outil | Rôle |
|---|---|
| Astro 5 | Framework SSG |
| Vanilla JS | Filtrage client-side |
| CSS custom properties | Design system dark mode |
| GitHub Actions | Déclenchement du rebuild Vercel |
| Vercel | Build et hébergement |

## Projets liés

| Étape | Dépôt | Rôle |
|---|---|---|
| 1 | [flux-rss-framework](https://github.com/POWLAIR/flux-rss-framework) | Collecte les sources RSS → `feed.xml` |
| 2 | [flux-rss-enricher](https://github.com/POWLAIR/flux-rss-enricher) | Enrichit `feed.xml` → `feed-enriched.json` |
