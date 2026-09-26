# Guide de la veille de nuit

Ce fichier est le mode d'emploi de l'agent Claude qui tourne chaque nuit sur ce dépôt.
Le lecteur des rapports est un étudiant en master de psychologie clinique (Suisse romande,
francophone), en stage clinique. Il veut, en se réveillant, une page visuelle qui résume
l'état de la recherche sur un sujet, lisible en 1 minute, 5 minutes ou 20 minutes.

## Organisation du dépôt

| Chemin | Rôle |
|---|---|
| `index.html` | Page d'accueil : formulaire de dépôt de sujet, sujets en attente, bibliothèque. Ne pas la réécrire. |
| `sujets/AAAA-MM-JJ-slug.md` | Un fichier par sujet déposé (frontmatter `sujet`, `statut`, `date`, puis précisions libres). |
| `rapports/slug.html` | Un rapport par sujet. `rapports/hpi-tdah.html` est le **modèle de référence**. |
| `data/rapports.json` | Liste des rapports lue par `index.html`. À mettre à jour à chaque rapport. |
| `modele/GUIDE.md` | Ce fichier. |

Un second dépôt **privé**, `valejaqu1/newsletter-pdfs`, reçoit les PDF que l'étudiant
télécharge avec son accès universitaire. Ces PDF sont protégés par le droit d'auteur :
**ne jamais les copier, ni en citer de longs passages, dans ce dépôt public.**

## Déroulé d'une nuit

1. Lire ce guide et `data/rapports.json`.
2. **Nouveaux sujets** : fichiers de `sujets/` avec `statut: nouveau`. En traiter au maximum
   **deux** par nuit, les plus anciens d'abord. Les autres attendent la nuit suivante.
3. **Nouveaux PDF** : lister les PDF du dépôt privé qui ne figurent dans aucun
   `pdfs_recus` de `data/rapports.json`. Pour chacun, identifier l'article **à partir de son
   contenu** (titre et DOI de la première page), jamais à partir du nom de fichier. Attention :
   le DOI de l'article lui-même est sur la première page ; les autres DOI du texte sont ceux
   de sa bibliographie. Rattacher le PDF au rapport qui l'avait demandé (`pdfs_demandes`).
   S'il ne correspond à aucune demande, le rattacher au rapport dont le sujet est le plus
   proche, ou l'ignorer et le signaler dans le message de commit.
4. Pour chaque rapport concerné par un nouveau PDF : lire l'article en entier, mettre à jour
   les dossiers concernés (chiffres détaillés, nuances), passer la fiche de l'étude à
   « texte intégral », incrémenter `version`, déplacer l'entrée de `pdfs_demandes` vers
   `pdfs_recus` (avec le nom du fichier).
5. Mettre le `statut` des sujets traités à `fait`.
6. `git add`, `git commit` (message en français, ex. « Veille : nouveau rapport sur X »),
   `git push` sur `main`. S'il n'y avait rien à faire, ne rien committer.

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

Ajouter en tête de la liste `rapports` :

```json
{
  "slug": "exemple",
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

`numero` = plus grand numéro existant + 1. `date` = date de première publication ; en cas de
mise à jour, ajouter `"maj": "AAAA-MM-JJ"`.
