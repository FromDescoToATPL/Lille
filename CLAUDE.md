# Regards sur Lille — contexte du projet

Guide touristique indépendant de Lille, écrit par un Lillois qui habite le
Vieux-Lille. Public visé : visiteurs français et étrangers. Positionnement :
« Lille, racontée par quelqu'un qui y vit » — les adresses que les guides
officiels ne donnent pas, avec des réserves honnêtes quand il y en a.

L'auteur n'est pas développeur. Explique ce que tu fais en français simple,
et demande avant toute modification importante de structure.

## Hébergement

- Site statique HTML + CSS, sans framework ni étape de compilation.
- Dépôt : https://github.com/FromDescoToATPL/Lille (branche `main`).
- Hébergé sur **Cloudflare Pages** (projet `regards-sur-lille`, compte
  de l'auteur) depuis septembre 2026 : chaque push sur `main` est publié
  tout seul en une minute. Build : aucun (pas de commande, dossier de
  sortie = racine). Adresse technique : https://regards-sur-lille.pages.dev
- Adresse officielle : **https://www.regards-sur-lille.com**. Domaines
  `regards-sur-lille.com` et `.fr` achetés chez OVH le 27/09/2026
  (particulier, protection des données activée). Le DNS est géré par
  Cloudflare (serveurs julio / kay.ns.cloudflare.com) ; seuls les MX et
  le SPF d'OVH y sont gardés pour l'email. `regards-sur-lille.com` sans
  www et le `.fr` redirigent vers l'adresse officielle.
- Choix de Cloudflare : gratuit ET pub autorisée. Vercel gratuit interdit
  la pub (AdSense cité noir sur blanc dans ses conditions).
- Cloudflare Pages enlève le `.html` des adresses : `carte.html` est
  servie en `/carte` (redirection automatique). Les liens du site sont
  donc écrits sans `.html` (voir règle 2).
- Ancienne adresse : https://fromdescotoatpl.github.io/Lille/ (GitHub
  Pages). Chaque page déclare l'adresse officielle en `canonical`, ce qui
  renvoie Google vers le nouveau domaine. Search Console était vérifié
  sur l'ancienne adresse : à refaire pour le domaine (propriété
  « Domaine », vérification par TXT dans le DNS Cloudflare).

## Structure

```
/                   pages françaises + style.css + favicon.svg + photos
/en/                pages anglaises (noms de fichiers en anglais)
/evenements/        articles d'événements, en français
/en/events/         articles d'événements, en anglais
sitemap.xml, robots.txt, google4fb4a0a25f12452b.html (vérification Google)
_redirects          anciennes adresses -> nouvelles (301), lu par Cloudflare
_headers            « noindex » sur les fichiers techniques (CLAUDE.md...)
404.html            page « introuvable » FR + EN, servie par Cloudflare
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
`/en/evenements/`, `noel-2026`, `braderie-2026`) sont dans le fichier
`_redirects` (redirections permanentes 301, avec et sans `.html`). Elles
remplacent depuis septembre 2026 les petites pages HTML à meta refresh.
Pour renommer une page un jour : ajouter une ligne `ancienne nouvelle 301`.

## Règles techniques — à respecter à chaque modification

1. **Noms de fichiers en minuscules, sans accent, avec tirets.**
   L'hébergement est sensible à la casse : `Photo.jpg` ≠ `photo.jpg`.
2. **Chemins relatifs selon la profondeur.** Racine : `style.css`.
   `en/` et `evenements/` : `../style.css`. `en/events/` : `../../style.css`.
   Même logique pour les photos et le favicon. Jamais de chemin absolu `/...`
   (seule exception : `404.html`, affichée à n'importe quelle adresse).
   **Liens entre pages sans `.html`** : `href="carte"`, `href="../restaurants"`,
   `href="evenements/braderie"`, `carte#citadelle`. L'accueil s'écrit
   `href="./"` à la racine, `href="../"` depuis un sous-dossier, `href="en/"`
   pour l'accueil anglais. Adresses absolues (hreflang, og:url, og:image,
   canonical, sitemap, données structurées) : `https://www.regards-sur-lille.com/...`,
   sans `.html`, avec `/` pour l'accueil et `/en/` pour l'accueil anglais.
3. **Chaque page doit contenir** : l'en-tête (`.topbar` + `.lang-switch`),
   le menu (`.mainnav`), le pied de page (`.footer-links`), les balises
   `hreflang`, la balise `<link rel="canonical">` (même adresse que
   og:url, juste après elle), les balises Open Graph (og:title, og:description, og:image
   en URL absolue, og:url, og:type, og:locale, twitter:card), le favicon,
   le lien d'évitement `.evitement` en tout début de `<body>`, le contenu
   dans `<main id="contenu">`, et le bouton `.haut` avec son script en fin
   de page. Le lien actif du menu porte la classe `active` et
   `aria-current` (« page » sur la page elle-même, « true » dans un
   article de la rubrique). Les pages françaises ont en plus le bandeau
   `.bandeau-langue` (« This guide is also in English »), affiché par
   script seulement si le navigateur n'est pas en français.
   Menu : Accueil, Événements, Lieux, Restaurants, Bars, Excursions, Carte
   (sept rubriques, pas plus). Sélecteur de langue : « FR / EN ».
   Les deux `<nav>` ont un nom pour les lecteurs d'écran : `.lang-switch`
   porte `aria-label="Langue"` (EN : « Language »), `.mainnav` porte
   `aria-label="Menu principal"` (EN : « Main menu »). Juste après le
   `</nav>` du menu vient le petit script du menu mobile (fondu sur le
   bord, onglet actif ramené à l'écran) : le copier tel quel depuis une
   page existante.
4. **Toute page ajoutée en français doit exister en anglais**, avec des
   `hreflang` croisés, et être ajoutée au `sitemap.xml`.
5. **Cache CSS** : le lien est `style.css?v=N`. Toutes les pages sont en
   `v=15` (septembre 2026). Pour forcer le rechargement partout, il faut
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
--gold-text #876220   --featured-tag #dcb05a   --box-bg #fbf8f1
```

- **Petits textes dorés** (étiquettes, adresses `venue-meta`, « En savoir
  plus », survol des liens) : toujours `--gold-text`, jamais `--gold`.
  `--gold` sur le crème ne donne qu'un contraste de 2,9 (4,5 demandé) ;
  `--gold-text` donne 4,9. `--gold` reste pour les filets, bordures et
  soulignés. « À la une » sur l'ardoise : `--featured-tag`. En mode
  sombre, les deux valent `#d1a24a`. Validé par l'auteur (septembre 2026).
- Fond des encadrés `.infos-pratiques` et `.affiliate-box` : `--box-bg`.

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
- Sur PC, l'en-tête tient sur une ligne à partir de 900 px : nom à
  gauche, menu centré, « FR / EN » à droite (bloc « ESSAI » dans le CSS,
  validé). L'auteur tient à ce menu centré à sept rubriques.
  Essayés puis refusés en septembre 2026, à ne pas reproposer :
  « Infos pratiques » dans le menu (il ne tient plus au centre), menu
  calé à droite, « Français / English » à la place de « FR / EN ». Il utilise une
  marge négative ; le `display: flow-root` sur `.mainnav` est
  indispensable, sinon le contenu remonte dans l'en-tête.
- Menu mobile : une seule ligne qui défile horizontalement. L'auteur a
  refusé la version sur deux lignes. Depuis septembre 2026 (validé) : un
  fondu sur le bord signale les rubriques cachées (classes `.suite-gauche`
  / `.suite-droite` posées par le script du menu), et sur les pages du
  bout du menu (Bars, Excursions, Carte) le menu défile tout seul jusqu'à
  l'onglet actif. Sur PC le menu tient : pas de fondu.
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
- Chaque `<img>` porte `width` et `height` (les vraies dimensions du
  fichier, pour que la page ne saute pas au chargement) et
  `loading="lazy" decoding="async"`, sauf la photo d'accueil
  (opera-place), qui porte `fetchpriority="high"`.
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
- Le marché de Noël (FR et EN) a dans son `<head>` un bloc de données
  structurées pour Google (`application/ld+json`, type Event) avec ses
  dates : **les mettre à jour chaque année en même temps que le texte**.
  N'en ajouter à un autre événement que si la page donne des dates
  précises (pas pour « début octobre »).
  Le bloc contient `offers` (prix 0 €, entrée gratuite) et `organizer`
  (Fédération lilloise du commerce, de l'artisanat et des services, en
  partenariat avec la Ville de Lille : donné par l'auteur, confirmé par
  la presse, aussi affiché dans l'encadré « En bref »). `performer` est
  volontairement absent : pas d'artiste pour un marché. Search Console le
  signale en « non critique », c'est sans effet sur l'affichage.
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
- **Suite du passage au domaine** (septembre 2026) : propriété
  « Domaine » dans Search Console + envoi du sitemap ; email
  `contact@regards-sur-lille.com` (Zimbra OVH gratuit) à créer par
  l'auteur puis à ajouter aux mentions légales FR + EN ; réactiver le
  DNSSEC depuis Cloudflare (il a été coupé chez OVH pour le changement de
  serveurs DNS) ; arrêter plus tard la copie GitHub Pages.
- **Mesure d'audience** : Cloudflare Web Analytics, activé le 28/09/2026
  (projet Pages > Metrics). Cloudflare ajoute lui-même son script à chaque
  déploiement : ne pas l'écrire dans les pages. Signalé dans la partie
  données personnelles des mentions légales FR + EN. Pas d'autre outil
  de statistiques.
- **Carte** : coordonnées des 5 points douteux et du Delirium vérifiées sur
  Google Maps en septembre 2026. Chaque point a un champ `id` :
  `carte.html#citadelle` centre la carte sur ce point et ouvre sa bulle.
  Les pages Lieux et Restaurants ont un lien « voir sur la carte » dans la
  ligne `venue-meta`. La page Bars est rangée par quartier, sans lien.
  Tout nouveau point doit recevoir un `id` identique en FR et en EN.
  Points de 24 px avec un pictogramme par catégorie (monument, couverts,
  verre, P) : les couleurs sont dans `style.css` (`.pin-lieu`,
  `.pin-resto`, `.pin-bar`, `.pin-parking`), les pictogrammes dans le
  script de `carte.html` et `en/map.html`. La légende utilise les mêmes.
- **Menu de langues** : passer à un menu déroulant natif seulement à
  partir de quatre ou cinq langues.
- **Plus tard** : AdSense (retirer `display: none` sur `.ad-slot`, activer
  l'outil de consentement certifié de Google, réécrire la partie données
  personnelles des mentions légales), affiliation Booking (nécessite le
  statut d'auto-entrepreneur, SIRET à ajouter aux mentions légales),
  compte Instagram à relier dans le pied de page, page « Où dormir » par
  quartier (Vieux-Lille, gares, Wazemmes) pour l'affiliation hôtels, dont
  l'auteur fournira le contenu.
- **Accueil** : les cartes des rubriques gardent « En savoir plus » /
  « Read more ». Une simple flèche a été essayée et refusée.
- **Fiches restaurants** : chaque adresse doit avoir la même ligne
  d'infos : adresse · quartier · type de cuisine · budget (€/€€/€€€) ·
  réservation conseillée ou non. L'auteur fournira les budgets. Ne jamais
  inventer un prix ou un type de cuisine.
- **Fiches bars** : la page Bars est rangée par quartier, sans fiche par
  bar. L'auteur doit dire quels bars mettre en fiche.
- `beffroi.jpg` montre le beffroi de la Chambre de Commerce, pas celui de
  l'Hôtel de Ville : ne pas l'utiliser pour ce dernier.
