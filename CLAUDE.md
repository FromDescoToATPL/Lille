# Regards sur Lille — contexte du projet

Guide touristique indépendant de Lille, écrit par un Lillois qui habite le
Vieux-Lille. Public visé : visiteurs français et étrangers. Positionnement :
« Lille, racontée par quelqu'un qui y vit » — les adresses que les guides
officiels ne donnent pas, avec des réserves honnêtes quand il y en a.

L'auteur n'est pas développeur. Explique ce que tu fais en français simple,
et demande avant toute modification importante de structure.

## Hébergement

- Site statique HTML + CSS, sans framework ni étape de compilation.
- Hébergé sur GitHub Pages, branche `main`, publié depuis la racine.
- Dépôt : https://github.com/FromDescoToATPL/Lille
- En ligne : https://fromdescotoatpl.github.io/Lille/
- Google Search Console vérifié, `sitemap.xml` soumis.
- Nom de domaine prévu : `regards-sur-lille.fr`, pas encore acheté.

## Structure

```
/                   pages françaises + style.css + favicon.svg + photos
/en/                pages anglaises (noms de fichiers en anglais)
/evenements/        articles d'événements, en français
/en/events/         articles d'événements, en anglais
/en/evenements/     anciennes adresses anglaises : redirections seulement
sitemap.xml, robots.txt, google4fb4a0a25f12452b.html (vérification Google)
```

Pages : index, evenements, lieux-a-visiter, restaurants, bars, excursions,
carte, itineraires, quand-venir, infos-pratiques, a-propos, mentions-legales.
Le pied de page de chaque page contient aussi « Quand venir » et
« Mentions légales ». Le chinois a été retiré (seul l'accueil existait) ;
les règles de police `html[lang^="zh"]` restent dans le CSS pour plus tard.
Articles : marche-de-noel, braderie, foire-maneges, street-food-festival,
biere-a-lille.

Correspondance des noms anglais (septembre 2026) :
evenements → events, lieux-a-visiter → places-to-see, excursions →
day-trips, carte → map, itineraires → itineraries, quand-venir →
when-to-visit, infos-pratiques → practical-info, a-propos → about,
mentions-legales → legal-notice ; index, restaurants, bars inchangés.
Articles : marche-de-noel → christmas-market, foire-maneges → funfair,
biere-a-lille → beer-in-lille ; braderie et street-food-festival inchangés.

Redirections : les anciennes adresses (noms français dans `/en/`, tout
`/en/evenements/`, `noel-2026.html`, `braderie-2026.html`) sont de petites
pages avec meta refresh + canonical, sans en-tête ni menu. C'est voulu :
ne pas les « compléter » et ne pas les mettre dans le sitemap. Leur
canonical contient l'adresse github.io : à mettre à jour avec le domaine.

## Règles techniques — à respecter à chaque modification

1. **Noms de fichiers en minuscules, sans accent, avec tirets.**
   GitHub Pages est sensible à la casse : `Photo.jpg` ≠ `photo.jpg`.
2. **Chemins relatifs selon la profondeur.** Racine : `style.css`.
   `en/` et `evenements/` : `../style.css`. `en/events/` : `../../style.css`.
   Même logique pour les photos et le favicon. Jamais de chemin absolu `/...`.
3. **Chaque page doit contenir** : l'en-tête (`.topbar` + `.lang-switch`),
   le menu (`.mainnav`), le pied de page (`.footer-links`), les balises
   `hreflang`, les balises Open Graph (og:title, og:description, og:image
   en URL absolue, og:url, og:type, og:locale, twitter:card), le favicon,
   le lien d'évitement `.evitement` en tout début de `<body>`, le contenu
   dans `<main id="contenu">`, et le bouton `.haut` avec son script en fin
   de page. Le lien actif du menu porte la classe `active` et
   `aria-current` (« page » sur la page elle-même, « true » dans un
   article de la rubrique). Les pages françaises ont en plus le bandeau
   `.bandeau-langue` (« This guide is also in English »), affiché par
   script seulement si le navigateur n'est pas en français.
   Menu : Accueil, Événements, Lieux, Restaurants, Bars, Excursions, Carte,
   Infos pratiques. Sélecteur de langue : « FR / EN » (les noms longs
   ont été refusés : ils prennent trop de place).
4. **Toute page ajoutée en français doit exister en anglais**, avec des
   `hreflang` croisés, et être ajoutée au `sitemap.xml`.
5. **Cache CSS** : le lien est `style.css?v=N`. Toutes les pages sont en
   `v=13` (septembre 2026). Pour forcer le rechargement partout, il faut
   incrémenter le numéro dans toutes les pages.
6. **Ne jamais imbriquer de commentaire CSS** (`/*` dans un `/* ... */`) :
   ça casse silencieusement tout le bloc qui suit.
7. Vérifier l'équilibre des balises (`div`, `article`, `figure`) après
   chaque édition.

## Design

Palette inspirée de Lille (brique flamande, crème, dorure de la Déesse) :

```
--cream #f6f1e6   --band #ece2ce   --stone #ddd2bb   --ink #322920
--brick #a13c26   --brick-dark #7e2e1c   --gold #b8862e
--slate #3c4652   --featured-bg #3c4652   --on-brick #fff
```

- Polices : Fraunces (titres) et Public Sans (texte), via Google Fonts.
  Pages chinoises : polices système (règle `html[lang^="zh"]`).
- Mode sombre automatique (`prefers-color-scheme`), jetons redéfinis.
  Le bandeau « À la une » utilise `--featured-bg`, pas `--slate`,
  car `--slate` devient clair en mode sombre. Même principe pour
  `--astuce-bg` et `--frame-bg` (cadre des photos : blanc en mode clair,
  brun foncé en mode sombre, pour que la légende reste lisible).
- Sur PC, `article.post` est limité à 660 px (environ 70 signes par ligne).
- **Mobile d'abord.** L'auteur est très attaché au rendu mobile actuel :
  ne jamais le modifier sans le lui demander. Les adaptations PC sont
  regroupées dans des blocs `@media (min-width: 900px)`.
- Sur PC, l'en-tête tient sur une ligne à partir de 1100 px : nom à
  gauche, menu calé à droite juste avant « FR / EN » (bloc « ESSAI » dans
  le CSS). Avec huit rubriques, le menu centré passait sur le nom du site,
  même sur grand écran : il est donc aligné à droite (`justify-content:
  flex-end` + `padding-right: 8.8rem`). « Français / English » a été
  essayé puis refusé par l'auteur : garder « FR / EN ». Entre 900 et
  1100 px, le menu est sous le nom. Il utilise une
  marge négative ; le `display: flow-root` sur `.mainnav` est
  indispensable, sinon le contenu remonte dans l'en-tête.
- Menu mobile : une seule ligne qui défile horizontalement. L'auteur a
  refusé la version sur deux lignes.
- Transitions entre pages (`@view-transition`), respectant
  `prefers-reduced-motion`.
- Emplacements publicitaires `.ad-slot` présents mais masqués
  (`display: none`) jusqu'à l'approbation AdSense.

## Photos

- Toutes prises par l'auteur (Galaxy S24 Ultra). Ne jamais utiliser de
  photo trouvée en ligne, même créditée : c'est une contrefaçon.
- Calibrage : 1600 px sur le grand côté, JPEG qualité 82, environ 200 à
  300 Ko.
- En place : opera-place, vieille-bourse (bas recadré pour enlever les
  pavés), rue-de-gand, cafe-oz, grande-roue, chalets-noel,
  braderie-terrasse, braderie-rue, citadelle-porte, citadelle-douves
  (lieux-a-visiter), place-aux-oignons, oasis-citadelle (restaurants).
- En réserve : passage-des-arts (pour une future balade dans le
  Vieux-Lille, page itinéraires). Une allée d'arbres de la Citadelle
  existe aussi chez l'auteur.
- Éviter les visages reconnaissables au premier plan (recadrer si besoin).
  Vérifier les EXIF en cas de doute sur l'origine d'une photo.
- Sur PC, traitement **au cas par cas** : proportions d'origine par défaut ;
  rue-de-gand et cafe-oz recadrées en 4/3 ; vieille-bourse entière dans un
  cadre resserré à 340 px. Pas de règle uniforme — l'auteur l'a refusée.
- Placer une photo après le texte qu'elle illustre, jamais juste avant un
  bloc sans rapport (une image se lit comme illustrant ce qui la précède).
- Une photo tous les deux ou trois blocs, pas une par lieu.

## Écriture — le point le plus important

L'auteur veut un site qui sonne humain, pas rédigé par une IA.

- **Aucun tiret long en incise** ( — ). Utiliser un point, une virgule
  ou deux-points.
- Pas de formules en deux temps, pas de deux-points qui annoncent une
  révélation, pas d'adjectifs recherchés (« éclectique », « feutrée »,
  « ludique »). Phrases courtes, mots courants.
- Première personne pour les recommandations (« je vous conseille »).
  Les passages factuels (horaires, accès, prix) restent neutres.
- Quand l'auteur fournit un texte, **ne corriger que les fautes** et le
  garder tel quel. Il tient à sa voix.
- Les articles d'événements sont **intemporels** : « chaque année début
  octobre », avec les dates de l'année en cours en exemple. Pas de
  millésime dans les titres ni les noms de fichiers.
- Vérifier chaque fait (date, adresse, prix, ligne de métro) avant de
  l'écrire. En cas de sources contradictoires, ne pas publier de chiffre
  précis.
- Aucune adresse n'est rémunérée. Tout futur lien d'affiliation devra
  être signalé clairement.

## Méthode de travail

- Livraison hors Claude Code : un zip rangé par dossier de destination
  (`1-racine`, `2-dossier-en`, `3-dossier-evenements`,
  `4-dossier-en-events`). L'auteur envoie un dossier à la fois.
- Le chargement des fichiers par l'interface web de GitHub a causé des
  écrasements (fichiers anglais qui remplacent les français quand
  plusieurs dossiers sont glissés d'un coup). Avec Claude Code, travailler
  en local puis faire un commit et un push.
- Quand l'auteur refuse une modification, l'annuler aussi dans les
  fichiers, pour qu'elle ne revienne pas dans une livraison suivante.
- Vérifier le rendu après publication en navigation privée (cache).

## En attente

- **Delirium Café** : happy hour noté 16h-19h, à confirmer par l'auteur.
  (Le point est déjà sur la carte.)
- **Nom de domaine** : `regards-sur-lille.fr`, à acheter chez OVH. DNS :
  4 enregistrements A et 4 AAAA vers GitHub Pages, CNAME `www` vers
  `fromdescotoatpl.github.io.`. Ensuite mettre à jour les `hreflang`, le
  `sitemap.xml`, `robots.txt`, et créer une propriété « Domaine » dans
  Search Console.
- **Mesure d'audience** : GoatCounter proposé (sans cookie, gratuit pour
  un usage non commercial). En attente de la décision de l'auteur.
- **Carte** : coordonnées des 5 points douteux et du Delirium vérifiées sur
  Google Maps en septembre 2026. Chaque point a un champ `id` :
  `carte.html#citadelle` centre la carte sur ce point et ouvre sa bulle.
  Les pages Lieux et Restaurants ont un lien « voir sur la carte » dans la
  ligne `venue-meta`. La page Bars est rangée par quartier, sans lien.
  Tout nouveau point doit recevoir un `id` identique en FR et en EN.
- **Menu de langues** : passer à un menu déroulant natif seulement à
  partir de quatre ou cinq langues.
- **Plus tard** : AdSense (retirer `display: none` sur `.ad-slot`, activer
  l'outil de consentement certifié de Google, réécrire la partie données
  personnelles des mentions légales), affiliation Booking (nécessite le
  statut d'auto-entrepreneur, SIRET à ajouter aux mentions légales),
  compte Instagram à relier dans le pied de page, page « Où dormir » par
  quartier (Vieux-Lille, gares, Wazemmes) pour l'affiliation hôtels, dont
  l'auteur fournira le contenu.
- **Fiches restaurants** : chaque adresse doit avoir la même ligne
  d'infos : adresse · quartier · type de cuisine · budget (€/€€/€€€) ·
  réservation conseillée ou non. L'auteur fournira les budgets. Ne jamais
  inventer un prix ou un type de cuisine.
- **Fiches bars** : la page Bars est rangée par quartier, sans fiche par
  bar. L'auteur doit dire quels bars mettre en fiche.
- `beffroi.jpg` montre le beffroi de la Chambre de Commerce, pas celui de
  l'Hôtel de Ville : ne pas l'utiliser pour ce dernier.
