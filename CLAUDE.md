# CLAUDE.md — exam_web_2026

## Description du projet

Site statique HTML/CSS : bibliographie de l'imaginaire (SF, Fantasy, Fantastique).  
Projet académique 2026 — Simon Desseille, 1ère bachelier informatique, EAFC Namur-Cadet.

## Ouvrir le site

Ouvrir `index.html` directement dans un navigateur (aucun serveur requis).  
Ou avec Live Server (VS Code) pour le rechargement automatique.

## Structure

```
exam_web_2026/
├── index.html                  ← Page d'accueil (accordéon 3 genres)
├── science-fiction/            ← asimov, clarke, dick, heinlein, herbert
├── fantasy/                    ← tolkien, hobb, martin, pratchett, sanderson
├── fantastique/                ← king, lovecraft, poe, shelley, stoker
└── assets/
    ├── css/style.css           ← Feuille de style personnalisée
    ├── fonts/                  ← Vide (polices système utilisées)
    └── img/                    ← 16 images : header-library.jpg + 15 couvertures de livres
```

## Stack technique

- **HTML5** sémantique (`<header>`, `<nav>`, `<main>`, `<footer>`)
- **Bootstrap 5.3.3** via CDN (accordéon, tableaux responsifs)
- **CSS personnalisé** dans `assets/css/style.css` (chargé après Bootstrap)
- Pas de build, pas de bundler, pas de framework JS

## Palette de couleurs

| Rôle | Hex |
|------|-----|
| Bleu principal (header, titres, bordures) | `#003580` |
| Bleu nav | `#004fa3` |
| Jaune focus/actif | `#ffd700` |
| Fond alternance tableau | `#f0f4ff` |
| Texte / footer fond | `#1a1a1a` |

## Architecture des pages auteurs

Toutes les 15 pages partagent la même structure :
1. `<head>` — même que l'index, chemin CSS `../assets/css/style.css`
2. Header identique
3. `<nav>` avec `aria-current="true"` sur le genre de l'auteur
4. Fil d'Ariane (breadcrumb)
5. `<h1>` nom de l'auteur
6. `<figure class="featured-book-cover">` avec couverture du livre emblématique (`../assets/img/`)
7. `<div class="author-bio">`
8. Un `<h2>` + tableau par cycle/groupe d'œuvres
9. Lien `.back-link` vers `../index.html`
10. Footer identique
11. **Pas de JS** sur les pages auteurs

## Hero header (index.html uniquement)

Le `<header>` de l'accueil porte la classe `hero` : il affiche `assets/img/header-library.jpg` (bibliothèque de Strahov, Prague, 1920×1280 px) en fond avec un overlay `rgba(0,0,0,0.48)` pour garder le texte lisible. Les pages auteurs ont un header bleu uni sans image.

## JavaScript (index.html uniquement)

Un seul bloc IIFE en bas de `index.html` : ouvre automatiquement le panneau d'accordéon correspondant à l'ancre de l'URL (`#sf`, `#fantasy`, `#fantastique`). Nécessaire car les pages auteurs renvoient vers `../index.html#sf`.

## Accessibilité — règles invariantes

- Tout lien de retour genre pointe vers `../index.html#<genre>` (pas juste `../index.html`)
- Le lien actif dans `<nav>` porte `aria-current="true"` (ou `"page"` pour l'accueil)
- Les flèches de fil d'Ariane ont `aria-hidden="true"`
- Les `<th>` de tableau ont `scope="col"`
- Le skip-link `<a href="#main" class="skip-link">` est présent sur chaque page
- `.sr-only` pour le texte lisible uniquement par les lecteurs d'écran

## Git

```bash
git status
git add <fichier>
git commit -m "message"
```

Dépôt local uniquement (pas de remote configuré à ce stade).
