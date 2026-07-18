# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Vue d'ensemble

Ce dépôt contient un site web statique pour « La Villa Fleurie », un restaurant gastronomique créole étoilé (fictif) situé à Saint-Gilles-les-Bains, à l'île de La Réunion. Il n'y a ni système de build, ni gestionnaire de paquets, ni suite de tests, ni linter. Deux pages autonomes :

- `index.html` — le site vitrine classique, entièrement autonome (CSS et JavaScript inclus en ligne, aucune dépendance JS externe).
- `lacasepitey.html`, `pouyaud.html`, `letripot.html`, `kaiku.html` et `labergerie.html` (+ `labergerie-og.jpg`, image Open Graph) — maquettes de prospection pour des restaurants réels (La Case Pitey à La Réunion, La Table du Pouyaud à Périgueux, Le Tripot à Chartres, Le Kaïku à Saint-Jean-de-Luz, La Bergerie à Pralognan-la-Vanoise), sur le même socle technique que `premium.html` avec des effets propres à chacune (fumée en fragment shader, particules qui se métamorphosent en formes, thème clair/sombre au scroll, menu plein écran, aperçu suivant le curseur). Elles portent une balise `noindex` et une mention « maquette de démonstration non officielle » dans le footer — préserver ces deux protections.
- `premium.html` — la démo « Édition Immersive » : mêmes contenus et même identité visuelle, mais avec animations au scroll (GSAP + ScrollTrigger), smooth scroll (Lenis), fond WebGL global (Three.js) dont la sphère change de position, de taille et de couleur au fil du scroll (état partagé `visu` piloté par ScrollTrigger, lu par la boucle de rendu), typographie révélée lettre par lettre, curseur personnalisé, boutons magnétiques, tilt 3D des cartes et inclinaison des titres selon la vélocité du scroll. Les librairies sont chargées via CDN (jsDelivr) ; tout le code applicatif reste inline. Le script détecte l'absence de GSAP ou `prefers-reduced-motion` et laisse alors le contenu entièrement visible sans animations — préserver ce mécanisme de secours lors de toute modification.

## Développement

- Aucune étape de build ou d'installation. Ouvrir `index.html` directement dans un navigateur, ou le servir localement (par ex. `python3 -m http.server`) pour prévisualiser les modifications.
- Les seules dépendances externes sont Google Fonts (Manrope + Lora) et une iframe Google Maps intégrée ; tout le reste est autonome, y compris les illustrations SVG intégrées en data URI.

## Architecture de index.html

Le fichier est organisé en blocs clairement délimités (marqués par des commentaires `/* ============ NOM ============ */` dans le CSS et `<!-- ============ NOM ============ -->` dans le HTML) :

- **`<head>`** : balises meta SEO en français et un bloc JSON-LD Schema.org de type `Restaurant`. Les garder synchronisés lors de toute modification des coordonnées du restaurant (adresse, téléphone, horaires).
- **Tokens de design CSS** : toutes les couleurs et polices sont des propriétés personnalisées CSS sur `:root` (par ex. `--ivoire`, `--vert`, `--or`, `--font-titre`, `--font-corps`). Utiliser ces tokens plutôt que des couleurs codées en dur. Les noms de classes et de variables sont en français dans tout le fichier (`.plat`, `.avis-card`, `--or-clair`).
- **Sections** : nav, hero, histoire, menu, chef, galerie, réservation, avis/presse, événements, contact, footer. Les sections sombres utilisent la classe `.dark` ; l'animation d'apparition au défilement utilise `.reveal` (avec les modificateurs de délai `.d1`/`.d2`/`.d3`), pilotée par un IntersectionObserver.
- **Images de substitution** : il n'y a aucune photo. La classe `.ph` (avec les variantes `ph-a` à `ph-d`) affiche des dégradés de marque avec un motif d'orchidée en SVG inline à la place des photos.
- **Le `<script>` inline en bas de page** gère tout le comportement :
  - État de la nav au défilement et menu burger mobile.
  - Apparition au défilement via IntersectionObserver.
  - Le menu (« La Carte ») est piloté par les données : les plats sont définis dans l'objet `menuData` (catégories `entrees`, `plats`, `desserts`, `accords`) et rendus dans `#menuGrid` par `renderMenu()`. Pour modifier les plats ou les prix, éditer `menuData`, pas le HTML.
  - Le formulaire de réservation est purement front-end : après une soumission valide, il masque le formulaire et affiche la confirmation `#resaOk`. Aucune donnée n'est envoyée nulle part.

## Conventions

- Tout le contenu destiné aux visiteurs est en français ; rédiger les nouveaux textes en français en respectant le ton élégant et gastronomique existant.
- Préserver les pratiques d'accessibilité en place : `aria-label`/`aria-expanded` sur les éléments interactifs, `role="tab"`/`aria-selected` sur les onglets du menu, le style `:focus-visible` et la media query `prefers-reduced-motion`.
- Les points de rupture responsive sont à 960px et 820px (nav mobile) ; tester tout changement de mise en page sur les deux.
