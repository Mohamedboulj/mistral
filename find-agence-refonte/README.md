# Refonte de la section `.find` — « Trouver une agence »

Refonte du HTML/CSS de la section `class="find"` de `old/trouver-agence-filahicash.php?category=1312`, alignée sur le composant **« Agency finder »** du design Figma (nœud `93:8771` : panneau 1280×592, fond `#fbfaf6`, bordure `#e9e5d8`, rayon 32px, colonne recherche 560px + carte 718px).

## Fichiers

| Fichier | Rôle |
|---|---|
| `find-section.html` | Nouveau markup de `<section class="find">…</section>` (prêt à copier, avec toutes les `<option>` ville) |
| `find-refonte.css` | Bloc CSS à ajouter **en fin** de `css/style.css` |
| `preview-find.html` | Page de prévisualisation autonome (vrai `style.css` + bootstrap + nouvelle section + hooks JS) — ouvrir dans un navigateur |
| `shots/find-1440.png`, `shots/find-390.png` | Captures de vérification (headless Chromium, 1440px et 390px) |

## Application

1. Dans `old/trouver-agence-filahicash.php`, remplacer tout le contenu de `<section class="find">…</section>` par `find-section.html`. Dans la boucle PHP des agences, faire correspondre chaque agence à une carte `.find-result` (titre « Ville — nom », seconde ligne adresse/horaires/tél.), en gardant la classe `agencyPanel` sur le `<div>` englobant.
2. Ajouter le contenu de `find-refonte.css` en fin de `css/style.css` (dernier = prioritaire sur les anciennes règles `.find`, `.thumbnail.find`, `.findHeader`, etc.).
3. Ajouter dans le `<head>` :
   ```html
   <link href="https://fonts.googleapis.com/css2?family=Geist:wght@400;500;600&family=Geist+Mono&family=Instrument+Serif:ital@1&family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
   ```
4. Supprimer l'iframe Google Maps de la section `.sliderFind` au-dessus : la carte est désormais intégrée au panneau `.find` (évite le doublon).

## Compatibilité JS (aucun changement de script nécessaire)

- `id="inputState"` conservé → `$('#inputState [value=1312]').attr('selected','true')` présélectionne toujours la ville de `?category=`.
- Classes `agencyPanel` et `load-more` conservées → l'affichage paginé « Plus d'agences » fonctionne toujours (le lien affiche désormais « Voir toutes les agences » avec flèche, conforme au design).
- Handler `$('select').on('change')` → `?category=` toujours actif ; le `name` du select passe à `category` pour que le bouton **Rechercher** soumette le formulaire même sans JS.
- `iframe{margin-top:-50px}` global neutralisé dans le panneau via `.find .find-map iframe{margin:0}`.

## Point d'attention

L'ancienne règle `.load-more{ color:#fff !important }` écrrasait la couleur du lien : le nouveau bloc déclare `color:#007731 !important` sur `.find .load-more` (vérifié au rendu : `rgb(0,119,49)`).

## Vérification effectuée

Rendu en Chromium headless à 1440px et 390px avec le `style.css` réel de la page : panneau 1280×592 `#fbfaf6`/radius 32, recherche 560px, carte 718px radius `0 32 32 0`, titre Geist 40/44 ls −1.2px avec « agence » en Instrument Serif italique 45px `#007731`, champ `#fffdf8` h50 radius 12, bouton pilule `#007731` h52 ombre `0 10px 24px -14px rgba(0,119,49,.75)`, pilules h36 bord `#e9e5d8` Inter 12.5, cartes `#f3f0e8` radius 12 (titre Inter 700 13.3 `#0d0f0c`, adresse 11.5 `#003d1f`), carte « Agence la plus proche » `#eff6f0` radius 16 (label Geist Mono 12 ls .96 uppercase), carte Google Maps grisée + teinte dégradée 30 % + 4 épingles + légende mono. Mobile : empilement vertical, aucun débordement.
