# Le Moulin — Contexte pour continuer le site web

> **À utiliser au début d'une nouvelle conversation.** Téléverse aussi ton `index.html` courant (celui de ton GitHub, qui contient déjà tout ce qui est listé plus bas), puis décris les modifications voulues.

---

## Le projet

- **Restaurant Le Moulin** — restaurant familial québécois, 59 rue Angus Nord, East Angus, QC.
- Site : **lemoulineastangus.com**, un **seul fichier `index.html`** (~5900+ lignes), déployé via **GitHub → Netlify**.
- **Trilingue FR / EN / ES** via les attributs `data-fr` / `data-en` / `data-es` sur les éléments. Un sélecteur de langue (vers la ligne ~2760) applique `innerHTML` si la valeur contient `<`, sinon `textContent`.

## Comment travailler avec moi (préférences)

- **Réponds en français québécois informel**, concis et direct.
- **Donne-moi les options avant d'implémenter** un gros changement, et **pose des questions de clarification** plutôt que de présumer — surtout pour : chemins d'images, placement/ordre des sections, et portée du contenu.

## Pile technique & design

- **Couleurs** : `--gold #C9993A`, `--dark #16130F`, `--dark2 #1F1B16`, `--text #F2EDE4`.
- **Polices** : Cormorant Garamond (titres/display), Outfit (corps) — auto-hébergées (woff2).
- Données structurées **Schema.org** (Restaurant + Menu JSON-LD), `_headers` Netlify pour le cache.
- Bons scores PageSpeed (96 Perf, 100 Accessibilité/Best Practices/SEO).

## Règles d'édition (qualité)

- **Modifications chirurgicales sur le texte brut** (str_replace / manipulation de chaînes uniques).
- **NE PAS** faire de réécriture complète via BeautifulSoup : un round-trip bs4 reformate tout et **perd ~81 lignes**. bs4 sert seulement à lire/analyser.
- **Valider après chaque changement** : comptes d'éléments inchangés, accolades `{ }` équilibrées, balises `<div>`/`</div>` équilibrées, présence de `dishModal`, parse bs4 sans erreur, taille cohérente.
- **Ancrer sur des chaînes UNIQUES** (chemin d'image complet, balise d'ouverture complète) — voir pièges plus bas.

## Pièges connus ⚠️

- **Noms `data-fr` dupliqués** entre les items du menu **et la galerie photos** (ex. « Entrée pour deux » existe dans la galerie ET dans les Entrées). Toujours ancrer sur l'ouverture unique du menu-item (`<div class="menu-item has-thumb" data-image="...">`) ou un chemin d'image, et vérifier que le compte = 1.
- L'entité `&amp;` apparaît dans certains `data-fr` (ex. « Eaux, Limonades &amp; Sans Alcool »).
- Les sections de menu sont délimitées par `<div class="menu-cat">` (titres de section) ; les boissons par `<div class="bois-card">` / `bois-card bois-card-table` (attention à ne pas confondre avec `bois-card-header`).
- Le menu « Spéciaux du midi » (Menu Midi) a un style séparé du menu continu — les cadres dorés ne s'y appliquent pas.

---

## État actuel du site

### Structure : 4 menus
**Déjeuner** · **Dîner & Souper** · **Boissons** · **Desserts**

### Ordre actuel des 17 sections — Dîner & Souper
1. Entrées
2. Salades
3. Pizzas
4. Combos Pizza
5. Poutines
6. Spécialités Grecques
7. Pâtes
8. Fruits de Mer
9. Poissons
10. Grillades
11. Sandwichs
12. Poulets
13. Côtes Levées
14. Sous-Marins
15. Hamburgers
16. Riz Frits
17. Menu enfant

*(Fin des Entrées : … Entrée pour deux → Pain à l'ail gratiné → Soupe/Crème du jour.)*

### Ordre actuel des 11 sections — Boissons
1. Bières en Fût
2. Bières en Bouteille
3. Vins Maison
4. Vins Blancs
5. Vins Rouges
6. Apéritifs
7. Sangria
8. Digestifs
9. Boissons Chaudes
10. Boissons Gazeuses
11. Eaux, Limonades & Sans Alcool

### Éléments visuels en place
- **Cadre doré « double filet »** (bordure double) + **bande dorée** derrière chaque en-tête de section, sur les 4 menus.
- **Prix à deux montants empilés sur deux lignes** (Demi/Complet, Petite/Grande, etc.) — corrige l'écrasement des noms sur mobile.
- **Galerie photos** : 2 boutons dorés (« Voir les photos déjeuners → » et « Voir les photos dîners et soupers → »), sans aperçu photo ni titre, qui ouvrent les galeries (lightbox).
- **Table des pizzas** : indice doré « Défiler pour voir tous les formats et prix → » qui apparaît seulement si la table déborde (mobile) et disparaît au premier scroll.
- **Vins Maison mobile** : colonnes de prix rétrécies + marge à droite pour que le 1 L ne touche pas le cadre doré.
- Hero avec diaporama animé (Ken Burns), navigation par catégories dans les panneaux, sous-onglets Dîner/Souper collants, boutons flottants (téléphone, retour galerie).

### Patterns techniques réutilisables
- **`mclose(s, i)`** : matcher qui trouve le `</div>` fermant un `<div>` donné en suivant la profondeur, en sautant les commentaires `<!-- -->`.
- **Réordonnancement de sections** : découper le contenu de la grille aux délimiteurs (`<div class="menu-cat">` ou `<div class="bois-card">`), réassembler dans le nouvel ordre, vérifier que la taille ne change pas (diff 0 = aucune perte).
- Images converties en **WebP qualité 85**, rangées en sous-dossiers : `images/plats/`, `images/entrees/`, `images/sandwichs/`, `images/dejeuners/`, `images/poutines/`, `images/salades/`, `images/boissons/`, `images/desserts/`.

---

## Emplacements clés dans le fichier (approximatifs)
- Panneaux : `id="panel-dejeuner"`, `id="panel-diner"` (`menu-continuous`), `id="panel-boissons"`, `id="panel-desserts"`.
- Cartes galerie : `<div class="gallery-cards" id="gallery-cards">`.
- Bloc `<style>` principal pour le CSS ; scripts en bas avant `</body>`.

## Pour démarrer la prochaine conversation
1. **Téléverse** ton `index.html` courant (la version GitHub à jour).
2. Colle ou mentionne ce document.
3. **Liste les modifications voulues** (je te proposerai des options / poserai des questions au besoin, puis j'exécuterai de façon chirurgicale avec validation).
