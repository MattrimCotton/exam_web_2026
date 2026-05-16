# CLAUDE.md — exam_web_2026

## Description du projet

Site statique HTML/CSS : bibliographie de l'imaginaire (SF, Fantasy, Fantastique).  
Projet académique 2026 — Simon Desseille, 1ère bachelier informatique, EAFC Namur-Cadet.

## Ouvrir le site

Ouvrir `index.html` directement dans un navigateur (aucun serveur requis).  
Ou avec Live Server (VS Code) pour le rechargement automatique.  
Preview Claude Code : `.claude/launch.json` configure `npx serve` sur le port 3333.

## Structure

```
exam_web_2026/
├── index.html                  ← Page d'accueil (hero, genre-intro, accordéon 3 genres)
├── science-fiction/            ← asimov, clarke, dick, heinlein, herbert
├── fantasy/                    ← tolkien, hobb, martin, pratchett, sanderson
├── fantastique/                ← king, lovecraft, poe, shelley, stoker
├── assets/
│   ├── css/style.css           ← Feuille de style (15 sections + variables CSS)
│   ├── fonts/                  ← Vide (polices système utilisées)
│   ├── img/                    ← 16 images : header-library.jpg + 15 couvertures
│   ├── js/                     ← Vide (JS inline suffisant)
│   └── vendor/bootstrap/       ← Vide (fallback local non finalisé)
└── .claude/
    └── launch.json             ← Config preview server (npx serve)
```

## Stack technique

- **HTML5** sémantique (`<header>`, `<nav>`, `<main>`, `<footer>`)
- **Bootstrap 5.3.3** via CDN (accordéon, tableaux responsifs)
- **CSS personnalisé** dans `assets/css/style.css` (chargé après Bootstrap)
- Pas de build, pas de bundler, pas de framework JS

## Palette de couleurs

Toutes les couleurs sont déclarées comme variables CSS dans `:root` de `style.css` :

| Variable | Hex | Rôle |
|----------|-----|------|
| `--color-primary` | `#003580` | Header, titres, bordures, boutons |
| `--color-nav` | `#004fa3` | Barre de navigation |
| `--color-accent` | `#ffd700` | Focus, lien actif, ligne dorée hero |
| `--color-surface` | `#f0f4ff` | Fond bio, alternance tableau, cartes genre |
| `--color-surface-hover` | `#d9e4ff` | Hover lignes tableau |
| `--color-text` | `#1a1a1a` | Texte principal, fond footer |
| `--color-border` | `#cccccc` | Séparateurs tableau, texte footer |
| `--color-white` | `#ffffff` | Fond page, texte sur fond sombre |
| `--color-muted` | `#555555` | Métadonnées, figcaption, book-count |

Ne jamais écrire ces couleurs en dur dans le CSS — utiliser les variables.

## Structure du CSS (`assets/css/style.css`)

16 sections dans cet ordre :

1. Variables (`:root`)
2. Reset / base
3. Layout global
4. Accessibilité (skip-link, focus global)
5. Header et hero
6. Navigation
7. Breadcrumb
8. Titres et contenus
9. Page d'accueil (`.genre-intro`, `.author-grid`, `.stats-box`)
10. Pages auteurs et couvertures (`.author-bio`, `.featured-book-cover`)
11. Tableaux
12. Liens / boutons (`.back-link`)
13. Footer
14. Utilitaires (`.sr-only`, `.tag-*`)
15. Responsive (un seul bloc `@media (max-width: 700px)`)
16. Réduction de mouvement (`@media (prefers-reduced-motion: reduce)`)

## Transitions et micro-interactions

Toutes les transitions sont définies dans `style.css` et désactivées via la section 16 (`prefers-reduced-motion: reduce`).

| Élément | Effet hover | Durée |
|---------|-------------|-------|
| `nav.genre-nav ul li a` | `background-color` + `color` | 0.2s |
| `nav.breadcrumb a` | `color` | 0.15s |
| `.genre-intro-card` | `translateY(-2px)` + ombre renforcée | 0.2s |
| `.author-card` | `translateY(-3px)` + ombre + `border-color` → `--genre-accent` | 0.2s |
| `.stat-card` | `translateY(-2px)` + ombre renforcée | 0.2s |
| `.featured-book-cover img` | `translateY(-3px)` + ombre renforcée | 0.25s |
| `.back-link` | `background-color` + `color` + ombre | 0.2s |

Ombres de base (statiques) : `.genre-intro-card`, `.author-card`, `.stat-card`, `.author-bio`.

**Pas de `border` sur `.featured-book-cover img`** — supprimé car il créait un trait noir visible sur les couvertures à bords clairs. L'ombre portée (`box-shadow`) assure seule la séparation visuelle.

## Hero header (`index.html` uniquement)

`<header class="hero">` sur la page d'accueil :
- Image de fond : `assets/img/header-library.jpg` (bibliothèque de Strahov, Prague)
- Overlay gradient vignette (plus sombre en haut/bas, ouvert au centre) — pas un overlay plat
- `min-height: 340px` (240px sur mobile)
- `background-position: center 30%` pour cadrer la voûte
- `<h1>` : `2.75rem`, ligne dorée (`border-bottom: 3px solid var(--color-accent)`) en `display: inline-block`
- `<p>` : tagline en petites majuscules espacées (`text-transform: uppercase`, `letter-spacing: 0.12em`)
- Tagline actuelle : *"Trois genres — quinze auteurs — une bibliothèque de l'imaginaire"*

Les pages auteurs ont un `<header>` (sans classe `hero`) : fond bleu uni, sans image.

## Bloc genre-intro (`index.html` uniquement)

Trois cartes placées entre la section `#apropos` et l'accordéon :

```html
<div class="genre-intro" aria-label="Présentation des genres">
  <div class="genre-intro-card"><h3>Science-fiction</h3><p>…</p></div>
  <div class="genre-intro-card"><h3>Fantasy</h3><p>…</p></div>
  <div class="genre-intro-card"><h3>Fantastique</h3><p>…</p></div>
</div>
```

Affichage en ligne sur desktop (flex-wrap), empilé en colonne sur mobile. Bordure bleue à gauche, titre en small-caps, texte compact.

## Architecture des pages auteurs

Toutes les 15 pages partagent la même structure :
1. `<head>` — chemin CSS `../assets/css/style.css`
2. Header bleu uni (sans classe `hero`)
3. `<nav>` avec `aria-current="true"` sur le genre de l'auteur
4. Fil d'Ariane (breadcrumb)
5. `<h1>` nom de l'auteur
6. `<figure class="featured-book-cover">` avec couverture (`../assets/img/`)
7. `<div class="author-bio">`
8. Un `<h2>` + tableau par cycle/groupe d'œuvres
9. Lien `.back-link` vers `../index.html#<genre>`
10. Footer identique
11. **Pas de JS** sur les pages auteurs

## JavaScript (`index.html` uniquement)

Un seul bloc IIFE en bas de `index.html` : lit `window.location.hash`, ouvre le panneau d'accordéon correspondant (`#sf`, `#fantasy`, `#fantastique`) via `bootstrap.Collapse.getOrCreateInstance(panel).show()`. Nécessaire pour que les liens de retour des pages auteurs (`../index.html#sf`) fonctionnent.

## Accessibilité — règles invariantes

- Skip link `<a href="#main" class="skip-link">` présent sur chaque page
- Tout lien de retour genre pointe vers `../index.html#<genre>`
- `aria-current="true"` sur le lien actif dans `<nav>`
- `aria-hidden="true"` sur les flèches décoratives de breadcrumb
- `<th scope="col">` sur tous les en-têtes de tableau
- Focus clavier visible : contour jaune `var(--color-accent)`, 3px, sur tous les éléments interactifs
- `.sr-only` pour le texte réservé aux lecteurs d'écran
- Contraste WCAG 2.1 AA : blanc sur `#003580`, `#1a1a1a` sur fond blanc

## Git

```bash
git status
git add <fichier>
git commit -m "message"
```

Dépôt local uniquement (pas de remote configuré).
