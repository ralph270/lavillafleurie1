# Carnet de prospection — sites premium pour restaurants

Playbook autonome. Une nouvelle session Claude lit ce fichier et reprend le travail sans historique. L'utilisateur (Paul Pulard, paulpulard@gmail.com) vend des refontes de sites à des restaurants réputés dont le site actuel est faible.

## La mission en une phrase

Trouver un restaurant français **bien noté** (4+/5 ou emplacement/histoire exceptionnels) dont le site est **catastrophique** (sous-domaine gratuit wixsite.com, e-monsite.com, jimdofree.com, sitew.com, pagesperso-orange.fr, free.fr — ou http sans https), construire une **maquette immersive unique**, la mettre en ligne, préparer l'e-mail de prospection.

## État d'avancement (mettre à jour à chaque ajout !)

Maquettes en ligne sur https://ralph270.github.io/lavillafleurie1/ :

| Fichier | Cible réelle | Thème WebGL | Formation particules |
|---|---|---|---|
| lacasepitey.html | La Case Pitey, La Rivière St-Louis (Réunion) — top 1000 monde, e-monsite en http | Fumée d'encens (fbm+domain warping) | braises montantes |
| pouyaud.html | La Table du Pouyaud, Champcevinel/Périgueux — ex-étoilé 2011-17, Bib Gourmand, wixsite | Aurore dorée | étoile Michelin 5 branches |
| letripot.html | Le Tripot, Chartres — 4,7/5, Maître Restaurateur, maison 1553, wixsite | Vitrail vivant (worley) | rosace 12 pétales |
| kaiku.html | Le Kaïku, St-Jean-de-Luz — 1★ Michelin, maison XVIᵉ (site correct : angle « émotion », prix 3900 €) | Océan de nuit, lune + sillage | rose des vents 8 branches |
| labergerie.html | La Bergerie, Les Prioux, Pralognan-la-Vanoise — 4,5/5, bougies + raclette feu de bois, wixsite | Nuit alpine (crêtes, lune, feu), neige qui tombe | cristal de neige 6 branches |
| cotemer.html | Côté Mer, Étretat — front de mer iconique, ~1800 avis, wixsite | Couchant sur la Manche, falaise+Arche+Aiguille | profil Arche+Aiguille |

Pistes non traitées : Le Malu (gastronomique Vendôme/Villerable, lemalu2.wixsite.com — réputation À VÉRIFIER) · Le Millésime (restolemillesime.wixsite.com, localisation inconnue) · Les Viviers de Nadège (Fouras, wixsite) · L'Embarcadère (Rennes, wixsite). Écartés : Fanfan Paris (devenu Madame FAN, bon site), Umami Strasbourg (bon site), Clos Basque Biarritz (changement de direction), St-Gangolphe (fermé).

Thèmes/formations déjà pris (ne pas répéter) : fumée, aurore, vitrail, océan nuit, montagne neige, couchant falaises · étoile 5, rosace 12, rose des vents 8, cristal 6, arche. Idées libres : vignes+grappe de raisin, jardin à la française+fleur de lys, lavande+cigale, carrelets charentais, volcans d'Auvergne.

## La méthode, étape par étape

1. **Trouver** : WebSearch `restaurant [région] carte menu réservation` avec `allowed_domains:[wixsite.com, e-monsite.com, jimdofree.com, sitew.com, pagesperso-orange.fr]`. Puis vérifier la réputation par une 2ᵉ recherche `"[Nom]" [ville] avis tripadvisor`. WebFetch des sites externes = 403 (proxy), s'appuyer sur les extraits de recherche. Si limite de recherche atteinte : prendre une piste en attente ci-dessus.
2. **Construire** : `cp labergerie.html nouveau.html` (socle le plus complet : favicon, OG, JSON-LD, theme-color) puis UN script python de remplacements exacts (`assert old in s` pour chaque chaîne) : head/OG/JSON-LD, palette (garder les NOMS de variables, changer les valeurs), fonts Google (déjà prises : Manrope/Lora, Cormorant, Fraunces, Playfair, Cinzel, Bodoni Moda, Marcellus, Prata), loader, nav/voile, hero, manifeste, 3 chapitres, titre-fill, 5 plats + 3 menus (prix plausibles), hôtes (JAMAIS de noms de personnes inventés — uniquement des noms publics vérifiés), distinctions (3 compteurs), marquee, final, resa, footer, le fragmentShader du paysage, la fonction de formation (segments + répartition proportionnelle à la longueur).
3. **Pièges connus** : espace insécable `\xa0` obligatoire dans splitChars (sinon les mots se collent) ; quad plein écran → `depthWrite:false,depthTest:false` + `renderer.clearDepth()` entre les 2 scènes (sinon particules invisibles) ; le `<div id="webgl">` doit exister dans le body ; zone claire = UN wrapper `#zoneCreme` (pas 2 sections séparées).
4. **Tester** (obligatoire) : copier dans le scratchpad, `sed` les 4 CDN vers les libs locales du scratchpad (gsap.min.js, ScrollTrigger.min.js, lenis.min.js, three.min.js — déjà téléchargées ; sinon `npm i gsap@3.12.5 lenis@1.1.18 three@0.160.1`), servir `python3 -m http.server 8898`, script Playwright (chromium dans /opt/pw-browsers via `require('/opt/node22/lib/node_modules/playwright')`) : vérifier zéro erreur JS (les fonts Google en échec = normal, sandbox), captures hero/milieu/formation/formulaire, corriger si besoin.
5. **Image OG** : capture 1200×630 du hero → `nouveau-og.jpg` committée.
6. **Publier** : commit sur `claude/claude-md-docs-s37zb8` (jamais main direct), push, PR via mcp github, **merger la PR** (workflow validé par l'utilisateur), envoyer 2-3 captures via SendUserFile, mettre à jour CLAUDE.md + la table ci-dessus.
7. **E-mail** : livrer dans la réponse l'e-mail complet « formule FAQ » (voir modèle ci-dessous).

## Protections non négociables (restaurants réels)

- `<meta name="robots" content="noindex, nofollow">` sur chaque maquette
- Footer : « Maquette de démonstration non officielle — proposition de refonte de site web. Contenus et montants indicatifs, sans valeur contractuelle. Aucune réservation réelle n'est transmise via cette page. »
- Formulaire front-end uniquement. Prix toujours « indicatifs ». À la vente réelle : retirer le noindex, brancher Web3Forms, pages légales.

## Modèle d'e-mail (formule FAQ validée)

Structure : accroche factuelle sur l'écart réputation/site (critiquer LE SITE avec des faits — jamais la personne ; si le site est correct, angle « émotion » à la Kaïku) → lien maquette + « privée, invisible sur Google » + inciter à bouger la souris → 4 questions-réponses : réservations (« directement dans votre boîte mail actuelle, rien ne change dans votre organisation », évolutif Zenchef/TheFork) · gestion après (« vous n'y touchez jamais, SMS→24h, 1 an inclus puis 40 €/mois ») · Google (domaine propre, HTTPS, données structurées, fiche Maps ; « personne ne peut garantir la 1ʳᵉ place ») · modifications (« envoyez vos photos, maquette adaptée sous 48 h sans engagement ») → inclusions noir sur blanc → **prix 2 400 €** (3 900 € si étoilé/prestigieux) sans abonnement → proposition de passage 15 min → « un mot et la maquette sera retirée » → signature Paul Pulard, paulpulard@gmail.com. Conseils récurrents : vérifier les vrais prix des menus avant envoi ; trouver l'e-mail/tél sur la fiche Google Maps ; SMS = un seul message courtois.

## Économie de tokens

- L'utilisateur démarre une session neuve avec : « Lis PROSPECTION.md et c'est reparti » (± la région souhaitée).
- Mettre à jour CE fichier après chaque maquette (table + pistes) — c'est la mémoire.
- Une maquette par échange ; python-remplacements plutôt que réécriture complète ; ne relire les gros fichiers que si nécessaire.
