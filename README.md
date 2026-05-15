# Bibliographie de l'imaginaire — Documentation technique

Ce document explique **comment ce site est construit**, fichier par fichier, ligne par ligne. Il s'adresse à quelqu'un qui sait lire du HTML mais qui n'a pas forcément de culture technique approfondie.

---

## Table des matières

1. [Structure des fichiers](#1-structure-des-fichiers)
2. [Ce qu'est le HTML et comment une page est organisée](#2-ce-quest-le-html-et-comment-une-page-est-organisée)
3. [Bootstrap — la boîte à outils visuelle](#3-bootstrap--la-boîte-à-outils-visuelle)
4. [La feuille de style personnalisée (style.css)](#4-la-feuille-de-style-personnalisée-stylecss)
5. [La typographie — les polices utilisées](#5-la-typographie--les-polices-utilisées)
6. [JavaScript — le comportement dynamique](#6-javascript--le-comportement-dynamique)
7. [Accessibilité — pourquoi et comment](#7-accessibilité--pourquoi-et-comment)
8. [Les pages auteurs — structure commune](#8-les-pages-auteurs--structure-commune)
9. [Responsive design — adaptation aux petits écrans](#9-responsive-design--adaptation-aux-petits-écrans)
10. [Ce qui ne sert pas encore (dossiers vides)](#10-ce-qui-ne-sert-pas-encore-dossiers-vides)

---

## 1. Structure des fichiers

```
exam_web_2026/
│
├── index.html                  ← Page d'accueil du site
│
├── science-fiction/            ← Dossier contenant les 5 pages SF
│   ├── asimov.html
│   ├── clarke.html
│   ├── dick.html
│   ├── heinlein.html
│   └── herbert.html
│
├── fantasy/                    ← Dossier contenant les 5 pages Fantasy
│   ├── hobb.html
│   ├── martin.html
│   ├── pratchett.html
│   ├── sanderson.html
│   └── tolkien.html
│
├── fantastique/                ← Dossier contenant les 5 pages Fantastique
│   ├── king.html
│   ├── lovecraft.html
│   ├── poe.html
│   ├── shelley.html
│   └── stoker.html
│
└── assets/                     ← Ressources partagées (style, images, etc.)
    ├── css/
    │   └── style.css           ← Feuille de style personnalisée
    ├── fonts/                  ← Dossier prévu pour les polices locales (vide)
    ├── img/                    ← Dossier prévu pour les images (vide)
    ├── js/                     ← Dossier prévu pour le JavaScript (vide)
    └── vendor/bootstrap/       ← Copie locale de Bootstrap (non utilisée)
```

Le site comporte **17 fichiers HTML** au total : 1 accueil + 15 pages auteurs (5 par genre).

---

## 2. Ce qu'est le HTML et comment une page est organisée

### Le DOCTYPE et la langue

Chaque page commence par :

```html
<!DOCTYPE html>
<html lang="fr">
```

- `<!DOCTYPE html>` : indique au navigateur que c'est du HTML5, le standard actuel.
- `lang="fr"` : précise que la langue du contenu est le français. C'est important pour les lecteurs d'écran (logiciels utilisés par les personnes malvoyantes) qui adaptent la prononciation en conséquence.

### Le `<head>` — la tête invisible

```html
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Bibliographie de l'imaginaire – SF, Fantasy, Fantastique</title>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/.../bootstrap.min.css">
  <link rel="stylesheet" href="assets/css/style.css">
</head>
```

Le `<head>` contient des informations **sur** la page, pas du contenu visible :

- `charset="UTF-8"` : définit l'encodage des caractères. UTF-8 permet d'afficher correctement les accents français (é, è, à…), les guillemets spéciaux et tous les caractères non-ASCII.
- `viewport` : contrôle comment la page s'affiche sur mobile. Sans cette ligne, un téléphone afficherait la page comme un écran d'ordinateur miniaturisé, illisible.
- `<title>` : le texte qui apparaît dans l'onglet du navigateur et dans les résultats de moteurs de recherche.
- Les deux `<link>` chargent les feuilles de style CSS (voir section 3 et 4).

### Le `<body>` — ce que voit l'utilisateur

Le corps de chaque page suit toujours le même ordre logique :

```
<body>
  lien d'évitement (accessibilité)
  <header>     ← Bandeau bleu en haut avec le titre du site
  <nav>        ← Barre de navigation (liens vers les genres)
  <main>       ← Contenu principal de la page
  <footer>     ← Pied de page
  <script>     ← JavaScript (chargé en dernier, exprès)
</body>
```

Cet ordre n'est pas anodin : les balises `<header>`, `<nav>`, `<main>`, `<footer>` sont dites **sémantiques**. Elles n'ont pas d'apparence par défaut, mais elles donnent du sens à la structure. Un lecteur d'écran peut ainsi annoncer "navigation principale" ou "contenu principal" à l'utilisateur.

---

## 3. Bootstrap — la boîte à outils visuelle

### Qu'est-ce que Bootstrap ?

Bootstrap est une **bibliothèque CSS et JavaScript** créée par Twitter. Elle fournit des centaines de classes CSS prêtes à l'emploi pour construire rapidement des interfaces. On peut comparer ça à un jeu de Lego : au lieu de fabriquer chaque pièce soi-même, on assemble des briques existantes.

La version utilisée ici est **Bootstrap 5.3.3**.

### Comment il est chargé

Bootstrap est chargé depuis un **CDN** (Content Delivery Network), c'est-à-dire un serveur externe spécialisé dans la distribution rapide de fichiers :

```html
<!-- Dans le <head> : la partie visuelle (CSS) -->
<link rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css"
      crossorigin="anonymous">

<!-- Juste avant </body> : la partie interactive (JS) -->
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"
        crossorigin="anonymous"></script>
```

- Le CSS est chargé dans le `<head>` pour que les styles s'appliquent avant que la page ne soit affichée.
- Le JS est chargé **tout en bas** du `<body>`, juste avant `</body>`. C'est une bonne pratique : cela évite que le chargement du script bloque l'affichage du contenu visible.
- `crossorigin="anonymous"` : mesure de sécurité qui empêche le serveur CDN de lire les cookies de l'utilisateur.
- `.min.css` / `.min.js` : versions *minifiées* (compressées), où tous les espaces et commentaires ont été supprimés pour que le fichier soit plus léger.
- Un dossier `assets/vendor/bootstrap/` est présent mais non utilisé — il était prévu pour une version locale de secours.

### Les classes Bootstrap utilisées dans ce projet

#### `container`

```html
<div class="container">...</div>
```

Centre le contenu horizontalement et lui fixe une largeur maximale. Sans ça, un texte sur un grand écran s'étirerait jusqu'aux bords et deviendrait difficile à lire.

#### `accordion` — accordéon dépliable

L'accordéon est le composant interactif principal de la page d'accueil. Il permet d'afficher les trois genres (SF, Fantasy, Fantastique) comme des tiroirs que l'on peut ouvrir ou fermer.

```html
<div class="accordion" id="accordionGenres">
  <div class="accordion-item" id="sf">
    <h2 class="accordion-header">
      <button class="accordion-button collapsed"
              data-bs-toggle="collapse"
              data-bs-target="#collapse-sf">
        Science-fiction
      </button>
    </h2>
    <div id="collapse-sf" class="accordion-collapse collapse">
      <div class="accordion-body">
        <!-- contenu caché/affiché -->
      </div>
    </div>
  </div>
</div>
```

- `data-bs-toggle="collapse"` : indique à Bootstrap que ce bouton contrôle un élément qui se déplie/replie.
- `data-bs-target="#collapse-sf"` : pointe vers l'élément à montrer/cacher (identifié par son `id`).
- `class="accordion-button collapsed"` : `collapsed` indique que le panneau est fermé par défaut.
- `data-bs-parent="#accordionGenres"` : garantit qu'un seul panneau est ouvert à la fois.

#### `table` et `table-responsive`

```html
<div class="table-wrapper table-responsive">
  <table class="table">...</table>
</div>
```

- `table` (Bootstrap) : applique un style propre au tableau (espacements, lisibilité).
- `table-responsive` : sur petit écran, ajoute un défilement horizontal plutôt que d'écraser le tableau.

---

## 4. La feuille de style personnalisée (style.css)

Le fichier `assets/css/style.css` vient **après** Bootstrap dans l'ordre de chargement. Cela signifie qu'il peut écraser ou compléter les styles de Bootstrap. C'est intentionnel : Bootstrap pose les bases, et le CSS personnalisé affine l'identité visuelle.

### Reset universel

```css
*, *::before, *::after {
  box-sizing: border-box;
}
```

Par défaut en CSS, quand on dit "cet élément fait 200px de large", ce calcul **exclut** la bordure et le padding (espacement interne). Avec `box-sizing: border-box`, le calcul les **inclut** — beaucoup plus intuitif. Cette règle s'applique à tous les éléments (`*`) et à leurs pseudo-éléments (`::before`, `::after`).

### Palette de couleurs

Tout le site utilise trois couleurs principales, définies directement dans le CSS :

| Couleur            | Code hexadécimal | Utilisation                               |
|--------------------|------------------|-------------------------------------------|
| Bleu foncé         | `#003580`        | Header, titres, bordures, boutons         |
| Bleu moyen         | `#004fa3`        | Barre de navigation                       |
| Jaune doré         | `#ffd700`        | Lien actif, focus clavier, survol nav     |
| Blanc cassé        | `#f0f4ff`        | Fond des lignes paires de tableau, bio    |
| Gris foncé         | `#1a1a1a`        | Couleur du texte, fond du footer          |
| Gris moyen         | `#cccccc`        | Séparateurs de tableau                    |

Un code hexadécimal comme `#003580` est une façon d'exprimer une couleur en mélangeant Rouge, Vert et Bleu. `#003580` = 0 rouge, 53 vert, 128 bleu → un bleu roi soutenu.

### Corps de la page (`body`)

```css
body {
  font-family: Arial, Helvetica, sans-serif;
  font-size: 1rem;
  line-height: 1.6;
  background-color: #ffffff;
  color: #1a1a1a;
  margin: 0;
  padding: 0;
}
```

- `font-family` : liste de polices dans l'ordre de préférence (voir section 5).
- `font-size: 1rem` : taille de base. `rem` est une unité relative à la taille définie par le navigateur (généralement 16px). Utiliser `rem` plutôt que `px` respecte les préférences d'accessibilité de l'utilisateur.
- `line-height: 1.6` : interligne à 1.6 fois la taille de la police, ce qui améliore la lisibilité.
- `margin: 0; padding: 0;` : supprime les marges par défaut que certains navigateurs ajoutent autour du `<body>`.

### Header

```css
header {
  background-color: #003580;
  color: #ffffff;
  padding: 1.5rem 2rem;
}
```

- `padding: 1.5rem 2rem` : ajoute un espace intérieur. Le premier chiffre (1.5rem) s'applique en haut et en bas, le second (2rem) à gauche et à droite.

### Barre de navigation

```css
nav.genre-nav ul {
  display: flex;
  flex-wrap: wrap;
  gap: 0.25rem;
}
```

- `display: flex` : active le *Flexbox*, un système de mise en page CSS qui aligne les éléments en ligne ou en colonne.
- `flex-wrap: wrap` : si les liens ne tiennent pas tous sur une ligne, ils passent automatiquement à la ligne suivante.
- `gap: 0.25rem` : espace entre chaque lien.

### Cartes auteurs (`.author-card`)

```css
.author-card {
  border: 2px solid #003580;
  border-radius: 6px;
  padding: 1rem 1.25rem;
  flex: 1 1 180px;
  max-width: 220px;
}
```

- `border-radius: 6px` : arrondit les coins.
- `flex: 1 1 180px` : chaque carte peut grandir (`1`), rétrécir (`1`), et sa taille de départ est 180px. Cela crée une grille flexible qui s'adapte à l'espace disponible.

### Bloc biographie (`.author-bio`)

```css
.author-bio {
  background: #f0f4ff;
  border-left: 4px solid #003580;
  padding: 1rem 1.25rem;
  border-radius: 0 4px 4px 0;
}
```

Le `border-left` crée une barre bleue verticale sur le côté gauche, effet visuel classique pour les citations ou encadrés informatifs.

### Tableaux

```css
tbody tr:nth-child(even) {
  background-color: #f0f4ff;
}
tbody tr:hover {
  background-color: #d9e4ff;
}
```

- `nth-child(even)` : colore une ligne sur deux en bleu très pâle (zébrage), ce qui facilite la lecture.
- `:hover` : change la couleur quand la souris passe sur une ligne.

### Utilitaires `.sr-only`

```css
.sr-only {
  position: absolute;
  width: 1px; height: 1px;
  overflow: hidden;
  clip: rect(0,0,0,0);
}
```

Cette classe rend un élément **invisible visuellement** mais **présent pour les lecteurs d'écran**. Elle sert à ajouter des descriptions textuelles que les voyants n'ont pas besoin de lire (car ils voient l'image ou le contexte visuel).

---

## 5. La typographie — les polices utilisées

```css
font-family: Arial, Helvetica, sans-serif;
```

Il n'y a **pas de police importée** depuis Google Fonts ou un fichier externe. Le site utilise des **polices système** :

1. **Arial** : police sans-sérif très répandue sur Windows et macOS.
2. **Helvetica** : alternative préférée sur macOS et iOS (quasi-identique visuellement).
3. **sans-serif** : mot-clé générique de secours — si aucune des deux précédentes n'est disponible, le navigateur choisit n'importe quelle police sans-sérif installée sur le système.

*Sans-sérif* signifie sans les petits empattements au bout des lettres. Ces polices sont considérées plus lisibles à l'écran que les polices avec sérifs (comme Times New Roman).

Le dossier `assets/fonts/` est vide : il avait été prévu pour héberger des polices locales, mais ce choix a été abandonné au profit des polices système, plus rapides et sans dépendance réseau.

---

## 6. JavaScript — le comportement dynamique

Il y a **un seul bloc JavaScript** dans tout le projet, situé dans `index.html` :

```html
<script>
  (function () {
    var hash = window.location.hash;
    if (!hash) return;
    var panel = document.querySelector(hash + ' .accordion-collapse');
    if (panel) bootstrap.Collapse.getOrCreateInstance(panel).show();
  })();
</script>
```

### Pourquoi ce script ?

L'accordéon Bootstrap ferme tous ses panneaux par défaut. Or, depuis les pages auteurs, les liens de navigation pointent vers des ancres comme `../index.html#sf`. Sans ce script, retourner à l'accueil via ce lien afficherait la page mais le panneau SF resterait fermé.

### Comment ça marche, étape par étape

1. `(function () { ... })()` : **IIFE** (Immediately Invoked Function Expression) — une fonction qui s'exécute immédiatement au chargement de la page, sans polluer l'espace global.
2. `window.location.hash` : récupère la partie de l'URL après le `#`. Par exemple, pour `index.html#sf`, la valeur est `"#sf"`.
3. `if (!hash) return;` : si l'URL n'a pas d'ancre, on arrête là (rien à faire).
4. `document.querySelector(hash + ' .accordion-collapse')` : cherche dans le DOM l'élément `.accordion-collapse` qui se trouve à l'intérieur de l'élément `#sf` (ou `#fantasy`, etc.).
5. `bootstrap.Collapse.getOrCreateInstance(panel).show()` : appelle l'API JavaScript de Bootstrap pour ouvrir ce panneau d'accordéon.

Le JS est placé **en bas du `<body>`**, après le script Bootstrap. C'est indispensable : le code appelle `bootstrap.Collapse`, qui ne serait pas disponible si Bootstrap n'était pas déjà chargé.

---

## 7. Accessibilité — pourquoi et comment

L'accessibilité web (souvent abrégée *a11y*) vise à rendre les sites utilisables par tout le monde, y compris les personnes qui utilisent un lecteur d'écran, naviguent au clavier sans souris, ou ont une faible vision des couleurs.

### Lien d'évitement (*skip link*)

```html
<a href="#main" class="skip-link">Aller au contenu principal</a>
```

```css
.skip-link {
  position: absolute;
  top: -3rem;    /* caché par défaut */
}
.skip-link:focus {
  top: 0;        /* visible quand on appuie sur Tab */
}
```

Ce lien est **invisible** jusqu'à ce qu'on appuie sur la touche Tab. Il permet aux utilisateurs du clavier ou d'un lecteur d'écran de sauter directement au contenu principal sans devoir traverser tout le menu à chaque page.

### Attributs ARIA

ARIA (*Accessible Rich Internet Applications*) est un ensemble d'attributs HTML qui enrichissent la description d'éléments pour les technologies d'assistance.

- `aria-label="Navigation principale"` : donne un nom explicite à la balise `<nav>` pour le lecteur d'écran.
- `aria-current="page"` : indique au lecteur d'écran quel est le lien de la page actuellement active.
- `aria-expanded="false"` : communique l'état ouvert/fermé d'un bouton d'accordéon.
- `aria-controls="collapse-sf"` : lie un bouton à l'élément qu'il contrôle.
- `aria-labelledby="titre-stats"` : lie une section à son titre pour que le lecteur d'écran annonce "Section : Statistiques".
- `aria-hidden="true"` : sur les flèches décoratives (›), indique qu'elles doivent être ignorées par le lecteur d'écran.

### Fil d'Ariane (*breadcrumb*)

```html
<nav class="breadcrumb" aria-label="Fil d'Ariane">
  <a href="../index.html">Accueil</a>
  <span aria-hidden="true">&rsaquo;</span>
  <a href="../index.html#sf">Science-fiction</a>
  <span aria-hidden="true">&rsaquo;</span>
  Isaac Asimov
</nav>
```

Le fil d'Ariane indique à l'utilisateur où il se trouve dans la hiérarchie du site. Les flèches `›` sont masquées au lecteur d'écran (`aria-hidden="true"`) car elles sont purement décoratives.

### Focus clavier

```css
a:focus,
button:focus,
[tabindex]:focus {
  outline: 3px solid #ffd700;
  outline-offset: 2px;
}
```

Quand un utilisateur navigue au clavier (touche Tab), le contour jaune doré indique clairement quel élément est sélectionné. Beaucoup de sites suppriment cet outline par erreur ou par souci esthétique, ce qui rend la navigation clavier très difficile.

### Contrastes de couleurs

La combinaison `#ffffff` (blanc) sur `#003580` (bleu foncé) offre un ratio de contraste élevé, conforme aux recommandations WCAG 2.1 niveau AA. Le texte noir `#1a1a1a` sur fond blanc est également très lisible.

### Structure des tableaux

```html
<thead>
  <tr>
    <th scope="col">Date de publication</th>
    ...
  </tr>
</thead>
```

`scope="col"` indique que cette cellule d'en-tête s'applique à toute la colonne. Un lecteur d'écran peut ainsi annoncer "Date de publication : 1951" en lisant chaque cellule du tableau.

---

## 8. Les pages auteurs — structure commune

Toutes les 15 pages auteurs (dans `science-fiction/`, `fantasy/`, `fantastique/`) partagent exactement la même architecture :

```
1. <head>   → même contenu que l'index, sauf :
             - <title> avec le nom de l'auteur
             - chemins vers style.css en "../assets/css/style.css"
               (deux points + slash = remonter d'un dossier)

2. Header   → identique à l'accueil

3. <nav>    → liens vers les genres, avec aria-current sur le genre de l'auteur

4. <main>
   ├── Fil d'Ariane (breadcrumb)
   ├── <h1> Nom de l'auteur
   ├── <div class="author-bio"> Présentation biographique
   └── Pour chaque cycle ou groupe d'œuvres :
       ├── <h2> Nom du cycle
       └── <div class="table-wrapper table-responsive">
               <table class="table"> avec caption, thead, tbody

5. Lien retour → <a href="../index.html" class="back-link">

6. Footer   → identique à l'accueil

7. (Pas de script JS sur les pages auteurs — inutile)
```

**Note sur les chemins relatifs** : depuis un fichier dans `science-fiction/`, le chemin `../assets/css/style.css` signifie : "remonte d'un dossier (`..`), puis descends dans `assets/css/`". C'est la manière standard de naviguer dans une arborescence de fichiers en HTML.

---

## 9. Responsive design — adaptation aux petits écrans

```css
@media (max-width: 700px) {
  .container { padding: 1rem; }
  header { padding: 1rem; }
  nav.genre-nav { padding: 0.5rem 1rem; }
  .stats-box { flex-direction: column; }
  .author-grid { flex-direction: column; }
  .author-card { max-width: 100%; }
}
```

Une `@media query` est une règle CSS conditionnelle : les styles à l'intérieur ne s'appliquent que si la condition est vraie. Ici, `max-width: 700px` signifie "si l'écran fait 700px de large ou moins (téléphone)".

Sur mobile :
- Les paddings sont réduits pour gagner de la place.
- Les grilles en ligne (`flex`) basculent en colonne (`flex-direction: column`) : les cartes auteurs s'empilent verticalement au lieu d'être côte à côte.
- Les cartes prennent toute la largeur disponible (`max-width: 100%`).

---

## 10. Ce qui ne sert pas encore (dossiers vides)

| Dossier                   | Prévu pour                              | État         |
|---------------------------|-----------------------------------------|--------------|
| `assets/fonts/`           | Polices locales (.woff2, .ttf…)         | Vide         |
| `assets/img/`             | Portraits des auteurs, illustrations    | Vide         |
| `assets/js/`              | Scripts JavaScript externes             | Vide         |
| `assets/vendor/bootstrap/`| Version locale de Bootstrap (hors CDN) | Vide         |

Ces dossiers témoignent de l'architecture planifiée mais non finalisée. Si le projet évolue, on pourrait y ajouter des images d'auteurs dans `img/` et les référencer dans les pages avec `<img src="../assets/img/asimov.jpg" alt="Portrait d'Isaac Asimov">`.

---

*Documentation rédigée pour le projet académique 2026 — Simon Desseille.*
