# Bibliographie de l'imaginaire

Site statique consacré aux grands auteurs de SF, Fantasy et Fantastique. Seize pages HTML, zéro dépendance côté serveur.

---

## Fonctionnalités

- Page d'accueil avec accordéon Bootstrap (un panneau par genre, un seul ouvert à la fois)
- 15 pages auteurs — 5 par genre — toutes construites sur le même gabarit
- Navigation avec `aria-current` dynamique et lien d'évitement (*skip link*)
- Ouverture automatique du bon panneau d'accordéon via l'ancre URL (`#sf`, `#fantasy`, `#fantastique`)
- Tableaux d'œuvres responsifs (`table-responsive`) avec zébrage et survol
- Design entièrement responsive (breakpoint à 700 px)

---

## Stack technique

| Couche | Choix |
|--------|-------|
| Balisage | HTML5 sémantique (`<header>`, `<nav>`, `<main>`, `<footer>`) |
| Style | Bootstrap 5.3.3 (CDN) + `assets/css/style.css` |
| Scripts | Un seul IIFE dans `index.html` (accordéon + ancre) |
| Polices | Polices système — `Arial, Helvetica, sans-serif` |
| Build | Aucun — fichiers ouverts directement dans le navigateur |

---

## Palette

| Rôle | Hex |
|------|-----|
| Bleu principal (header, titres, bordures) | `#003580` |
| Bleu navigation | `#004fa3` |
| Jaune focus / actif | `#ffd700` |
| Fond alternance tableau & bio | `#f0f4ff` |
| Texte principal / footer | `#1a1a1a` |

---

## Architecture

```
exam_web_2026/
├── index.html                  ← Accueil : accordéon 3 genres + stats
├── science-fiction/
│   ├── asimov.html
│   ├── clarke.html
│   ├── dick.html
│   ├── heinlein.html
│   └── herbert.html
├── fantasy/
│   ├── hobb.html
│   ├── martin.html
│   ├── pratchett.html
│   ├── sanderson.html
│   └── tolkien.html
├── fantastique/
│   ├── king.html
│   ├── lovecraft.html
│   ├── poe.html
│   ├── shelley.html
│   └── stoker.html
└── assets/
    ├── css/style.css           ← Styles personnalisés (chargés après Bootstrap)
    ├── img/                    ← 16 images : header + 15 couvertures
    ├── fonts/                  ← Vide (polices système retenues)
    ├── js/                     ← Vide (JS inline suffisant)
    └── vendor/bootstrap/       ← Vide (fallback local non finalisé)
```

---

## Structure commune des pages auteurs

Chaque page auteur suit le même gabarit dans cet ordre :

1. En-tête identique à l'accueil (header bleu uni, sans image de fond)
2. `<nav>` avec `aria-current="true"` sur le genre concerné
3. Fil d'Ariane (`Accueil › Genre › Auteur`)
4. `<h1>` + bloc `.author-bio`
5. Un `<h2>` et un tableau Bootstrap par cycle ou groupe d'œuvres
6. Lien `.back-link` pointant vers `../index.html#<genre>`
7. Footer identique

Aucun script JS sur les pages auteurs.

---

## JavaScript

Un seul bloc IIFE en bas de `index.html`. Il lit `window.location.hash` et, si l'ancre correspond à un panneau d'accordéon (`#sf`, `#fantasy`, `#fantastique`), appelle `bootstrap.Collapse.getOrCreateInstance(panel).show()` pour l'ouvrir immédiatement. Cela rend les liens de retour (`../index.html#sf`) fonctionnels sans rechargement ni état côté serveur.

---

## Accessibilité

- Skip link (`.skip-link`) présent sur chaque page, visible au focus clavier
- `aria-label`, `aria-current`, `aria-expanded`, `aria-controls`, `aria-labelledby` sur tous les éléments interactifs et les sections
- Flèches de fil d'Ariane masquées aux technologies d'assistance via `aria-hidden="true"`
- `<th scope="col">` sur tous les en-têtes de tableau
- Contour focus jaune (`#ffd700`, 3 px) systématiquement préservé sur les liens et boutons
- Contraste WCAG 2.1 AA : blanc sur `#003580`, `#1a1a1a` sur fond blanc

---

## Installation / Utilisation

```bash
# Ouvrir directement dans le navigateur
start index.html          # Windows
open index.html           # macOS
```

Aucun serveur, aucune dépendance à installer. Le rechargement automatique est possible avec l'extension *Live Server* (VS Code).

---

## Choix techniques

- **Polices système** : pas de requête réseau supplémentaire, rendu natif, pas de FOUT.
- **Bootstrap via CDN** : mise en cache navigateur partagée, pas de bundle à maintenir.
- **CSS personnalisé après Bootstrap** : surcharge ciblée sans modifier la bibliothèque.
- **JS minimal** : un seul IIFE, aucune dépendance, aucun état global.
- **Gabarit unique** : les 15 pages auteurs partagent exactement la même structure, ce qui facilite la maintenance et garantit la cohérence.

---

## Améliorations futures

- Ajouter les portraits d'auteurs dans `assets/img/` et les intégrer dans les pages
- Mettre en place un fallback local Bootstrap dans `assets/vendor/bootstrap/`
- Ajouter des polices locales dans `assets/fonts/` (ex. : Inter ou Source Serif)
- Envisager une page de recherche ou un index alphabétique

---

*Projet académique 2026 — Simon Desseille, 1ère bachelier informatique, EAFC Namur-Cadet.*
