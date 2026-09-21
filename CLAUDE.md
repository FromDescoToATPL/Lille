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
/en/                pages anglaises (mêmes noms de fichiers)
/zh/                chinois simplifié — SEULE index.html existe pour l'instant
/evenements/        articles d'événements, en français
/en/evenements/     articles d'événements, en anglais
sitemap.xml, robots.txt, google4fb4a0a25f12452b-2.html (vérification Google)
```

Pages : index, evenements, lieux-a-visiter, restaurants, bars, excursions,
carte, itineraires, quand-venir, infos-pratiques, a-propos.
Articles : noel-2026, braderie-2026, foire-maneges, street-food-festival,
biere-a-lille.

## Règles techniques — à respecter à chaque modification

1. **Noms de fichiers en minuscules, sans accent, avec tirets.**
   GitHub Pages est sensible à la casse : `Photo.jpg` ≠ `photo.jpg`.
2. **Chemins relatifs selon la profondeur.** Racine : `style.css`.
   `en/` et `evenements/` : `../style.css`. `en/evenements/` : `../../style.css`.
   Même logique pour les photos et le favicon. Jamais de chemin absolu `/...`.
3. **Chaque page doit contenir** : l'en-tête (`.topbar` + `.lang-switch`),
   le menu (`.mainnav`), le pied de page (`.footer-links`), les balises
   `hreflang`, le favicon, et le bouton `.haut` avec son script en fin de page.
   Le lien actif du menu porte la classe `active`.
4. **Toute page ajoutée en français doit exister en anglais**, avec des
   `hreflang` croisés, et être ajoutée au `sitemap.xml`.
5. **Cache CSS** : le lien est `style.css?v=N`. `index.html` et `en/index.html`
   sont en `v=10`, les autres pages en `v=9`. Pour forcer le rechargement
   partout, il faut incrémenter le numéro dans toutes les pages.
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
  car `--slate` devient clair en mode sombre.
- **Mobile d'abord.** L'auteur est très attaché au rendu mobile actuel :
  ne jamais le modifier sans le lui demander. Les adaptations PC sont
  regroupées dans des blocs `@media (min-width: 900px)`.
- Sur PC, l'en-tête tient sur une ligne : nom à gauche, menu au centre,
  langues à droite (bloc « ESSAI » dans le CSS, validé). Il utilise une
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
- En place : opera-place, vieille-bourse, rue-de-gand, cafe-oz,
  grande-roue, chalets-noel, braderie-terrasse, braderie-rue.
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

- Le chargement des fichiers par l'interface web de GitHub a causé des
  écrasements (fichiers anglais qui remplacent les français quand
  plusieurs dossiers sont glissés d'un coup). Avec Claude Code, travailler
  en local puis faire un commit et un push.
- Quand l'auteur refuse une modification, l'annuler aussi dans les
  fichiers, pour qu'elle ne revienne pas dans une livraison suivante.
- Vérifier le rendu après publication en navigation privée (cache).

## En attente

- **Delirium Café** : happy hour noté 16h-19h, à confirmer par l'auteur.
  Coordonnées GPS à obtenir pour l'ajouter à la carte.
- **Chinois** : traduire toutes les pages (seul l'accueil existe, ses
  liens internes pointent vers des pages absentes). Mettre aussi à jour
  l'accueil chinois, qui annonce encore trois villes d'excursion au lieu
  de quatre.
- **Nom de domaine** : une fois acheté, mettre à jour les `hreflang`, le
  `sitemap.xml`, `robots.txt` et la vérification Search Console.
- **Menu de langues** : passer à un menu déroulant natif seulement à
  partir de quatre ou cinq langues.
- **Nettoyage** : `Photo-Opera.jpg` et `photo-opera.jpg` ne sont plus
  utilisées. `beffroi.jpg` montre le beffroi de la Chambre de Commerce,
  pas celui de l'Hôtel de Ville : ne pas l'utiliser pour ce dernier.
- **Plus tard** : AdSense (retirer `display: none` sur `.ad-slot`),
  affiliation Booking (nécessite le statut d'auto-entrepreneur), mentions
  légales, compte Instagram à relier dans le pied de page.
- **Contenu** : l'auteur relit tout pour y remettre sa voix. Priorité aux
  nouveaux articles sur des sujets intemporels.
