# Guide de la veille de nuit

Ce fichier est le mode d'emploi des agents Claude qui travaillent sur ce dépôt.
Le lecteur des rapports est un étudiant en master de psychologie clinique (Suisse romande,
francophone), en stage clinique. Il veut, en se réveillant, une page visuelle qui résume
l'état de la recherche sur un sujet, lisible en 1 minute, 5 minutes ou 20 minutes.

## Population d'intérêt

Par défaut, l'étudiant s'intéresse aux **adultes et jeunes adultes**, pas aux enfants.
Chercher en priorité des études sur ces populations. Ne présenter des données sur l'enfant
que si elles éclairent le sujet (développement, persistance à l'âge adulte) ou si les données
adultes manquent, et le dire explicitement. Si le sujet déposé précise une autre population,
suivre le sujet.

## Organisation du dépôt

| Chemin | Rôle |
|---|---|
| `index.html` | Page d'accueil : formulaire de dépôt de sujet, sujets en attente, bibliothèque. Ne pas la réécrire. |
| `sujets/AAAA-MM-JJ-slug.md` | Un fichier par sujet déposé (frontmatter `sujet`, `statut`, `date`, puis précisions libres). |
| `rapports/slug.html` | Un rapport par sujet. `rapports/hpi-tdah.html` est le **modèle de référence**. |
| `data/rapports.json` | Rapports publiés et sujets en préparation (avec leurs PDF demandés), lus par `index.html`. |
| `modele/GUIDE.md` | Ce fichier. |

Un second dépôt **privé**, `valejaqu1/newsletter-pdfs`, reçoit les PDF que l'étudiant
télécharge avec son accès universitaire. Ces PDF sont protégés par le droit d'auteur :
**ne jamais les copier, ni en citer de longs passages, dans ce dépôt public.**

## Deux passages par sujet

Un sujet passe par deux étapes, pour que l'étudiant ait le temps de fournir les PDF avant la
rédaction. Statuts dans le frontmatter du sujet : `nouveau` → `attente-pdf` → `fait`.

### Passage 1 · Pré-recherche (tâche « Veille · pré-recherche », dans les minutes qui suivent le dépôt)

1. Traiter les sujets `statut: nouveau`. S'il n'y en a aucun, s'arrêter sans rien committer.
2. Recherche rapide et économe (Consensus d'abord, une dizaine de recherches au plus) :
   cadrer le sujet et repérer les **3 à 6 articles clés** (revues, méta-analyses, grandes
   études, en privilégiant le récent) dont le texte intégral **n'est pas** en libre accès.
   Ne jamais demander un article déjà lisible gratuitement. Vérifier le DOI de chaque
   article demandé dans Crossref ou Europe PMC.
3. Ajouter en tête de `data/rapports.json` une entrée avec `"etat": "en-preparation"`,
   `slug`, `titre`, `accroche` (une phrase : ce que le rapport couvrira), `date` du jour et
   `pdfs_demandes` (référence, DOI, raison précise). `numero`, `sources`, `periode`,
   `version` peuvent rester vides à ce stade.
4. Passer le sujet à `statut: attente-pdf` et ajouter `prerecherche: AAAA-MM-JJTHH:MMZ`
   (heure UTC) dans son frontmatter.
5. Commit « Veille : PDF demandés pour … », push. **Ne pas rédiger le rapport.**

### Passage 2 · Rédaction (tâche « Veille de nuit », chaque nuit à 3 h)

1. Lire ce guide et `data/rapports.json`.
2. **Rapports à rédiger** : sujets `statut: attente-pdf` dont la `prerecherche` date d'au
   moins 6 heures (sinon ils attendent la nuit suivante). Au maximum **deux** par nuit, les
   plus anciens d'abord. Rédiger le rapport complet en utilisant les PDF déposés et les
   sources en libre accès. Mettre l'entrée de `data/rapports.json` à `"etat": "publie"` et
   compléter tous ses champs ; les PDF utilisés passent dans `pdfs_recus`, ceux qui manquent
   restent dans `pdfs_demandes` (ils pourront compléter le rapport plus tard).
3. **Sujets jamais pré-recherchés** (`statut: nouveau`, si la pré-recherche n'a pas tourné) :
   faire le passage 1 pour eux, pas le rapport, pour que l'étudiant puisse fournir les PDF
   dans la journée.
4. **PDF arrivés après publication** : PDF du dépôt privé absents de tous les `pdfs_recus`
   et rattachés à un rapport déjà publié : lire l'article en entier, mettre à jour les
   dossiers concernés, passer la fiche à « texte intégral », incrémenter `version`, ajouter
   `"maj"`, déplacer l'entrée vers `pdfs_recus` (avec le nom du fichier).
5. Mettre le `statut` des sujets rédigés à `fait`.
6. `git add`, `git commit` (message en français), `git push` sur `main`. S'il n'y avait rien
   à faire, ne rien committer.

### Identifier un PDF

Toujours identifier l'article **à partir de son contenu** (titre et DOI de la première page),
jamais à partir du nom de fichier. Le DOI de l'article est sur sa première page ; les autres
DOI du texte sont ceux de sa bibliographie. Rattacher le PDF à l'entrée qui l'avait demandé
(`pdfs_demandes`) ; s'il ne correspond à aucune demande, le rattacher au sujet le plus proche
ou l'ignorer et le signaler dans le message de commit.

## Méthode de recherche

1. **Cadrer** : reformuler le sujet en 4 à 6 questions précises qu'un clinicien se pose.
2. **Chercher dans cet ordre** : revues systématiques et méta-analyses récentes, grandes
   cohortes ou registres, puis les études qui contredisent le consensus. Viser **12 à 20
   sources**. Commencer par l'outil **Consensus** (connecteur MCP) s'il est disponible : il
   cible directement les articles scientifiques et économise des recherches web.
   Autres sources utiles : Europe PMC, PubMed Central, Semantic Scholar, Crossref, sites des
   revues, PsyArXiv. Privilégier le texte intégral en libre accès.
3. **Privilégier le récent, sans exclusive.** L'étudiant veut se tenir à jour et voir les
   points de vue nouveaux : donner la priorité aux 5 dernières années, et chercher
   activement ce qui est paru dans les 2 dernières années (au moins 3 sources si elles
   existent). Ce n'est pas une règle absolue : garder les études plus anciennes quand elles
   restent la référence ou qu'elles fondent le débat, et dire quand une nouvelle étude
   confirme, nuance ou contredit ce qu'on savait.
4. **Vérifier chaque référence** : auteurs, année, revue, volume, pages et DOI, un par un, dans
   Europe PMC (`https://www.ebi.ac.uk/europepmc/webservices/rest/search?query=DOI:"..."&format=json&resultType=core`)
   ou Crossref (`https://api.crossref.org/works?rows=1&query.bibliographic=...`). Ne jamais
   reprendre une année ou un volume depuis un résumé de page web : ces résumés se trompent.
   Si `curl` est bloqué dans l'environnement, utiliser WebFetch sur ces mêmes URL.
5. **Noter pour chaque étude** si elle a été lue en texte intégral ou en résumé seulement.
6. **Chiffres** : n'afficher que des chiffres publiés. Si seuls des qualificatifs sont
   disponibles (« effet faible », « moyen »), représenter des catégories, pas des nombres.
   Tout calcul fait par l'agent (ex. proportion attendue sous la loi normale) est signalé
   comme tel dans la légende.
7. **Articles clés non accessibles** : les lister (3 à 5 maximum) dans `pdfs_demandes`, avec
   la raison précise pour laquelle le texte intégral changerait le rapport.

## Structure d'un rapport

Partir d'une copie de `rapports/hpi-tdah.html` : garder tout le CSS, la structure HTML et les
fonctions JavaScript de dessin, et remplacer le contenu et les données. Ne pas inventer un
nouveau design.

1. **En-tête** : « Veille n° NN », date, nombre de sources, période couverte, titre court,
   une phrase d'accroche. Lien de retour vers `../index.html`.
2. **Niveau 1, l'essentiel** : 3 phrases de synthèse, puis 4 à 6 lignes
   « question / réponse courte / solidité des preuves » (3 points : solide, modérée, faible).
   Quand la recherche récente apporte un éclairage nouveau, le dire dans la synthèse et le
   signaler dans le dossier concerné (par ex. « Nouveau en 2026 : … »).
3. **Carte des études** : tableau `S` du script (une entrée par étude : année, thème, type,
   effectif, fiche). Les couloirs `LANES` correspondent aux dossiers. Types : population,
   clinique, revue, auto-déclaré. Ajuster `dy` si deux bulles se chevauchent.
4. **Niveau 2, les dossiers** (4 à 6) : titre-affirmation, paragraphe, encadré « À garder
   en tête », **une figure tirée de vrais chiffres** quand il y en a, puis « Approfondir »
   avec 2 à 4 blocs et des boutons `cite` vers les fiches.
5. **En pratique** : 4 à 6 pistes concrètes pour un stagiaire, puis la phrase rappelant d'en
   discuter avec son ou sa superviseur·e.
6. **Débats et questions ouvertes.**
7. **PDF demandés** : pour chaque article, référence, DOI et raison ; bouton vers
   `https://github.com/valejaqu1/newsletter-pdfs/upload/main`.
8. **Glossaire** (6 à 10 termes) et **Références** (générées depuis `S`).

## Style d'écriture

- Français, tutoiement, phrases courtes et directes, voix active.
- Pas de jargon non défini : chaque sigle va dans le glossaire.
- Pas d'apartés entre tirets, pas de formules toutes faites.
- Nuancer selon la solidité des preuves ; ne jamais présenter une étude isolée comme un consensus.
- Citations directes : 15 mots maximum, entre guillemets, avec la source. Sinon, reformuler.
- Aucun conseil médical individuel ; les pistes cliniques restent générales.

## Mise à jour de `data/rapports.json`

Forme d'une entrée publiée (l'entrée existe déjà depuis la pré-recherche, en tête de liste) :

```json
{
  "slug": "exemple",
  "etat": "publie",
  "numero": 2,
  "titre": "Titre court",
  "accroche": "Une phrase qui donne la conclusion principale.",
  "date": "AAAA-MM-JJ",
  "sources": 18,
  "periode": "2015–2026",
  "version": 1,
  "pdfs_demandes": [
    {"ref": "Auteur et al. (2024)", "doi": "10.xxxx/yyyy", "pourquoi": "Raison précise."}
  ],
  "pdfs_recus": []
}
```

`numero` = plus grand numéro des rapports publiés + 1, attribué à la publication. `date` = date de première publication ; en cas de
mise à jour, ajouter `"maj": "AAAA-MM-JJ"`.
