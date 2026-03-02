# Cas pratique en droit du travail : licenciement abusif

Ce tutoriel guide pas a pas a travers un dossier juridique complet avec BetterCallClaude -- de la prise en charge initiale du mandat jusqu'a la remise des documents finaux. Il montre comment les commands, agents, skills et MCP servers du framework collaborent sur un cas realiste de droit suisse du travail.

**Public cible** : avocates et avocats suisses evaluant l'outil, utilisateurs de Claude Code explorant le framework de plugins, ou toute personne souhaitant comprendre comment BetterCallClaude traite un dossier juridique reel de bout en bout.

**Temps de lecture** : 20-30 minutes

**Prerequis** : BetterCallClaude installe et fonctionnel. Voir [docs/INSTALL.md](../INSTALL.md) pour les instructions d'installation.

---

## Le cas

> **Maria Keller** (34 ans, ingenieuse en informatique) contre **AlphaFin SA** (startup fintech, Geneve)
>
> Maria etait employee d'AlphaFin SA depuis 2,5 ans en tant que lead backend engineer. Lors d'une migration de systeme, elle a decouvert que le module de traitement des paiements ne disposait pas du monitoring des transactions exige par la FINMA. Le 3 mars 2026, elle a signale cette lacune de conformite par courriel a son superieur direct et au CTO.
>
> Le 10 mars, la direction a prolonge retroactivement sa periode d'essai -- ce qui est nul en vertu de l'art. 335b CO, la periode d'essai initiale etant echue depuis longtemps. Le 24 mars, elle a recu son conge pour le 30 avril, avec un delai de preavis d'un mois (comme si elle etait encore en periode d'essai).
>
> Maria estime que le licenciement est abusif (licenciement abusif, art. 336 let. d CO) et entend reclamer une indemnite pouvant atteindre 6 mois de salaire (art. 336a CO). Son salaire mensuel brut s'eleve a CHF 12'500.

**Questions juridiques en jeu** :
1. Validite de la prolongation retroactive de la periode d'essai (art. 335b CO)
2. Licenciement de represailles apres signalement de non-conformite (art. 336 let. d CO)
3. Delai de conge -- ordinaire vs. periode d'essai (art. 335a/335c CO)
4. Applicabilite du delai de protection (art. 336c CO)
5. Calcul et plafond de l'indemnite (art. 336a al. 2 CO -- max. 6 mois)
6. Competence et procedure (Tribunal des prud'hommes de Geneve, CPC)

---

## Etape 1 : prise en charge du mandat avec session de briefing

Commencez par une session de briefing complete afin que BetterCallClaude comprenne le dossier avant que les agents n'entrent en action.

```
/bettercallclaude:briefing --depth comprehensive

Maria Keller, ingenieuse en informatique, a ete licenciee par AlphaFin SA (Geneve)
apres avoir signale des lacunes de conformite FINMA. La periode d'essai a ete prolongee
retroactivement et le conge a ete donne avec un delai d'un mois. Elle invoque un
licenciement abusif au sens de l'art. 336 CO.
Salaire mensuel CHF 12'500, duree des rapports de travail 2,5 ans.
```

**Ce qui se passe** : le command de briefing active l'agent **swiss-legal-briefing-coordinator**. Celui-ci classe la requete comme hautement complexe (droit du travail + conformite reglementaire + licenciement de represailles), compose un panel de specialistes et pose des questions structurees d'intake.

**Agents actives** : briefing-coordinator, qui constitue un panel parmi : researcher, strategist, procedure-specialist, risk-analyst, compliance-officer

Le coordinateur de briefing pose des questions ciblees -- chacune indiquant quel specialiste a besoin de la reponse :

> **Questions d'intake**
>
> 1. Maria dispose-t-elle d'une trace ecrite de son signalement de conformite (courriel, ticket interne) ? *(Requis par : Researcher -- evaluation des preuves ; Advocate -- preuve documentaire)*
>
> 2. Des problemes de performance documentes existaient-ils avant le signalement de conformite ? *(Requis par : Adversary -- analyse de la defense de l'employeur ; Risk -- calcul de l'exposition)*
>
> 3. AlphaFin SA a-t-elle respecte ses procedures RH internes pour le licenciement ? *(Requis par : Procedure -- validite formelle ; Strategist -- moyens d'attaque proceduraux)*
>
> 4. Maria a-t-elle deja adresse une opposition ecrite au licenciement ? *(Requis par : Procedure -- suivi des delais ; Strategist -- preservation des pretentions)*
>
> 5. AlphaFin SA est-elle un etablissement assujetti a la FINMA ou opere-t-elle sous licence fintech ? *(Requis par : Compliance -- contexte reglementaire ; Researcher -- regles FINMA applicables)*

Apres avoir repondu a ces questions, le systeme elabore un plan d'execution :

> **Plan d'execution**
>
> | Etape | Agent | Tache | Point de controle |
> |-------|-------|-------|-------------------|
> | 1 | Researcher | Recherche de precedents sur art. 335b/336/336a CO, contexte de signalement FINMA | Examen des resultats |
> | 2 | Advocate + Adversary + Judicial Analyst | Analyse contradictoire tripartite | Examen de l'evaluation |
> | 3 | Strategist + Risk Analyst + Procedure Specialist | Strategie contentieuse avec analyse cout-benefice | Examen de la strategie |
> | 4 | Drafter | Demande en justice pour le Tribunal des prud'hommes de Geneve | Examen du projet |
> | 5 | Summarizer | Consolidation de l'ensemble des resultats | Controle final |
>
> **Duree estimee du pipeline** : 15-25 minutes avec points de controle

**Fonctionnalite cle** : BetterCallClaude detecte automatiquement la juridiction (Geneve) a partir des faits et oriente vers le droit federal du travail (CO) ainsi que la procedure cantonale genevoise. Aucune specification manuelle n'est necessaire.

---

## Etape 2 : recherche juridique -- trouver les precedents

Le briefing termine, lancez la recherche juridique ciblee sur les questions centrales.

```
/bettercallclaude:research "licenciement abusif apres signalement interne de non-conformite, art. 336 let. d CO, delai de protection art. 336c CO, prolongation retroactive de la periode d'essai art. 335b CO"
```

**Agents actives** : swiss-legal-researcher

**MCP servers utilises** : bge-search (arrets du Tribunal federal), entscheidsuche (recherche inter-juridictionnelle), fedlex-sparql (textes legislatifs)

Le researcher execute un workflow structure en six etapes : analyser la question, rechercher dans les ATF, rechercher la jurisprudence cantonale, evaluer les sources, identifier la doctrine et compiler un memorandum.

> **Resultats de recherche -- Licenciement abusif (art. 336 CO)**
>
> **Cadre legal**
> - Art. 336 let. d CO : le conge est abusif lorsqu'il est donne parce que le travailleur fait valoir de bonne foi des pretentions resultant du contrat de travail.
> - Art. 336a al. 2 CO : indemnite en cas de licenciement abusif -- max. 6 mois de salaire.
> - Art. 335b CO : la periode d'essai doit etre convenue au debut des rapports de travail ; une prolongation requiert un accord ecrit avant l'expiration de la periode initiale.
>
> **Arrets de principe**
>
> 1. **ATF 136 III 513** -- Elements du licenciement de represailles
>    - Attendu : la proximite temporelle entre l'activite protegee et le licenciement fonde une presomption de motif de represailles. Le fardeau de la preuve est renverse : il incombe a l'employeur de demontrer un motif legitime et independant.
>    - Pertinence : l'intervalle de 21 jours entre le signalement de Maria et le licenciement etaye fortement l'allegation de represailles.
>
> 2. **ATF 132 III 115** -- Methodologie de calcul de l'indemnite
>    - Attendu : le tribunal dispose d'un pouvoir d'appreciation dans la fourchette de 0 a 6 mois. Criteres : gravite de la faute, age et anciennete de la travailleuse, impact economique, comportement de l'employeur.
>    - Pertinence : 2,5 ans d'anciennete, schema de represailles manifeste -> vraisemblablement 3-4 mois.
>
> 3. **ATF 134 III 108** -- Fardeau de la preuve en cas de licenciement abusif
>    - Attendu : la travailleuse doit prouver le lien de causalite entre l'activite protegee et le licenciement. La preuve par indices (proximite temporelle, absence d'autres motifs) est suffisante.
>    - Pertinence : documentation par courriel du signalement de conformite + absence de problemes de performance anterieurs = solide faisceau d'indices.
>
> 4. **ATF 125 III 70** -- Regime de la periode d'essai
>    - Attendu : les prolongations de la periode d'essai doivent etre convenues avant l'expiration de la periode initiale. Les prolongations retroactives sont nulles.
>    - Pertinence : invalide directement la prolongation retroactive de la periode d'essai par AlphaFin SA.
>
> *Sources verifiees via les MCP servers bge-search et legal-citations.*

Chaque citation peut etre verifiee individuellement avec le command cite :

```
/bettercallclaude:cite ATF 136 III 513
```

> **Verification de citation**
>
> **ATF 136 III 513** -- Verifie
> - FR : ATF 136 III 513
> - DE : BGE 136 III 513
> - IT : DTF 136 III 513
> - Objet : Licenciement abusif / Missbrauchliche Kundigung
> - Date : 2010
> - Passage cle : consid. 2.5 (analyse de la proximite temporelle)

**Fonctionnalite cle** : la meme recherche fonctionne dans toutes les langues. Une requete en allemand (`/bettercallclaude:research "missbrauchliche Kundigung nach interner Compliance-Meldung, OR 336 lit. d"`) renvoie des resultats avec des references BGE et la terminologie juridique allemande.

---

## Etape 3 : analyse contradictoire -- mettre le dossier a l'epreuve

Il s'agit desormais de soumettre la position juridique a un test de resistance. Le command adversarial execute trois agents independants en sequence -- un advocate, un adversary et un judicial analyst -- afin de produire une evaluation equilibree.

```
/bettercallclaude:adversarial Maria Keller a ete licenciee de maniere abusive par AlphaFin SA apres avoir signale des lacunes de conformite FINMA. 2,5 ans d'emploi, signalement de l'absence de monitoring des transactions le 3 mars, prolongation retroactive de la periode d'essai le 10 mars, licenciement le 24 mars avec un delai d'un mois. Pretentions fondees sur l'art. 336 let. d CO, indemnite jusqu'a 6 mois de salaire.
```

**Agents actives** : swiss-legal-advocate, swiss-legal-adversary, swiss-judicial-analyst

Les trois agents produisent des analyses independantes. Voici une version condensee de chacune :

### Position de l'Advocate

> **Evaluation de l'Advocate -- Dossier Maria Keller**
>
> **Arguments les plus solides** :
> 1. *Proximite temporelle* : seulement 21 jours entre le signalement de conformite (3 mars) et le licenciement (24 mars). Selon l'ATF 136 III 513, cela fonde une forte presomption de motif de represailles.
> 2. *Prolongation fictive de la periode d'essai* : la prolongation retroactive du 10 mars est nulle selon l'ATF 125 III 70. Elle constitue un indice supplementaire de la mauvaise foi de l'employeur -- celui-ci avait besoin d'un pretexte pour raccourcir le delai de conge.
> 3. *Preuves documentaires* : le courriel signalant les lacunes FINMA est horodate et constitue une trace sans equivoque de l'activite protegee.
> 4. *Absence de problemes de performance anterieurs* : l'absence de critiques documentees sur les performances avant le 3 mars mine toute explication alternative du licenciement.
> 5. *Contexte du signalement FINMA* : la lacune de conformite representait un risque reglementaire reel (absence de monitoring des transactions). Il ne s'agissait pas d'une plainte temeraire, mais de l'exercice legitime de droits decoulant du contrat de travail, exerce de bonne foi.
>
> **Pretention recommandee** : 4-6 mois de salaire (CHF 50'000-75'000) fondes sur l'art. 336a CO, plus le differentiel resultant du delai de conge correct.

### Position de l'Adversary

> **Evaluation de l'Adversary -- Moyens de defense d'AlphaFin SA**
>
> **Arguments potentiels de l'employeur** :
> 1. *Restructuration operationnelle* : AlphaFin SA pourrait soutenir que le licenciement s'inscrivait dans une reorganisation planifiee dans le cadre de la migration de systeme, sans lien avec le signalement de conformite.
> 2. *Problemes de performance* : meme en l'absence de documentation formelle, l'employeur pourrait produire des temoignages sur une insatisfaction preexistante a l'egard du travail de Maria.
> 3. *Signalement de conformite non effectue "de bonne foi"* : l'employeur pourrait arguer que le signalement de Maria etait motive par un conflit personnel plutot que par un souci reel de conformite -- affaiblissant la protection de l'art. 336 let. d CO.
> 4. *Prolongation de la periode d'essai comme erreur administrative* : AlphaFin SA pourrait presenter la prolongation retroactive comme une erreur des ressources humaines plutot que comme un acte delibere de represailles, afin d'en reduire la valeur probante.
>
> **Faiblesses de la position de l'employeur** :
> - La chronologie de 21 jours est extremement difficile a justifier.
> - La prolongation nulle de la periode d'essai nuit activement a la credibilite de l'employeur.
> - L'absence de problemes de performance documentes avant le 3 mars constitue une lacune significative.

### Synthese du Judicial Analyst

> **Evaluation judiciaire -- Analyse equilibree**
>
> **Considerants**
>
> *Consid. 1 -- Licenciement abusif (art. 336 let. d CO)*
> La proximite temporelle (21 jours) entre le signalement de conformite et le licenciement, combinee a l'absence de problemes de performance documentes, etablit un solide cas prima facie de licenciement de represailles. Il incombe a l'employeur de prouver un motif legitime et independant (ATF 134 III 108). La prolongation fictive de la periode d'essai fragilise en outre la credibilite de l'employeur.
> **Probabilite de constatation d'un licenciement abusif : 70-80 %**
>
> *Consid. 2 -- Validite de la prolongation de la periode d'essai (art. 335b CO)*
> La prolongation retroactive est nulle de plein droit (ATF 125 III 70). La periode d'essai initiale avait expire environ deux ans avant la tentative de prolongation. La question ne prete pas a discussion.
> **Probabilite : >95 % que la prolongation est nulle**
>
> *Consid. 3 -- Delai de conge*
> La prolongation de la periode d'essai etant nulle, le delai de conge applicable est de deux mois (art. 335c al. 1 CO, deuxieme annee de service). Le delai d'un mois est insuffisant. Les rapports de travail se poursuivent jusqu'a la prochaine date de resiliation valable.
> **Probabilite : >90 % que le delai de conge etait incorrect**
>
> *Consid. 4 -- Indemnite (art. 336a CO)*
> Compte tenu de la solidite des preuves de represailles et du comportement contraire a la bonne foi de l'employeur (prolongation fictive de la periode d'essai), le tribunal accorderait vraisemblablement 3-4 mois de salaire. Le maximum de 6 mois est reserve aux cas les plus graves.
> **Fourchette d'indemnite attendue : CHF 37'500-50'000 (3-4 mois)**
>
> *Evaluation globale* : **issue favorable probable (65-75 %)**. Transaction recommandee dans la fourchette de CHF 40'000-55'000.

**Fonctionnalite cle** : le Judicial Analyst emploie la structure suisse des considerants -- le meme format que celui utilise par les tribunaux suisses dans leurs motivations. Chaque question juridique fait l'objet d'un considerant numerote assorti d'une estimation de probabilite.

---

## Etape 4 : elaboration de la strategie

La recherche et l'analyse contradictoire etant achevees, la strategie contentieuse est elaboree.

```
/bettercallclaude:strategy Action pour licenciement abusif, Maria Keller c. AlphaFin SA, Geneve. Art. 336 let. d CO, represailles apres signalement de non-conformite FINMA. Prolongation nulle de la periode d'essai (art. 335b CO). Indemnite reclamee jusqu'a 6 mois de salaire. Salaire mensuel CHF 12'500.
```

**Agents actives** : swiss-case-strategist, risk-analyst, procedure-specialist

> **Strategie contentieuse -- Keller c. AlphaFin SA**
>
> **1. Feuille de route procedurale**
>
> | Phase | For | Delai | Duree est. |
> |-------|-----|-------|------------|
> | Opposition ecrite | A l'employeur | Avant l'expiration du delai de conge | Immediat |
> | Conciliation | Autorite de conciliation, Geneve | Dans les 180 jours suivant la fin des rapports de travail | 1-3 mois |
> | En cas d'echec -> demande en justice | Tribunal des prud'hommes de Geneve | 30 jours apres l'autorisation de proceder | 6-12 mois |
>
> Note : les litiges relevant du droit du travail jusqu'a CHF 30'000 sont exemptes de frais judiciaires (art. 114 let. c CPC). La pretention de Maria (jusqu'a CHF 75'000) depasse ce seuil ; les frais judiciaires ordinaires s'appliquent donc.
>
> **2. Estimation des couts**
>
> | Poste | Estimation |
> |-------|-----------|
> | Frais judiciaires | CHF 3'000-6'000 |
> | Representation juridique | CHF 8'000-15'000 |
> | Frais d'expertise (si necessaire) | CHF 2'000-5'000 |
> | **Total des frais de procedure** | **CHF 13'000-26'000** |
>
> **3. Analyse transactionnelle**
>
> - **Meilleur scenario** (indemnite integrale) : CHF 75'000 (6 mois) + dommages-interets pour le differentiel de delai de conge
> - **Resultat attendu** : CHF 37'500-50'000 (3-4 mois) + correction du delai de conge
> - **Objectif transactionnel** : CHF 45'000-55'000 (y compris le differentiel de delai de conge)
> - **Seuil minimum acceptable** : CHF 30'000
>
> Le ratio cout-benefice plaide en faveur d'une transaction : des frais de procedure estimes a ~CHF 20'000 pour un recouvrement attendu de ~CHF 45'000 degagent un gain net de ~CHF 25'000. Une transaction amiable a CHF 45'000 preserve le meme resultat net sans le risque du proces.
>
> **4. Matrice des risques**
>
> | Risque | Probabilite | Impact | Mesure d'attenuation |
> |--------|-------------|--------|----------------------|
> | L'employeur fait valoir des problemes de performance preexistants | 30 % | Eleve | Exiger la production du dossier RH complet en phase d'instruction |
> | Le tribunal accorde moins de 3 mois | 25 % | Moyen | Mettre en avant la prolongation fictive comme facteur aggravant |
> | AlphaFin SA depose une demande reconventionnelle (p. ex. violation de NDA) | 10 % | Moyen | Examiner le contrat de travail quant a la portee de la clause NDA |
> | Procedure prolongee (>18 mois) | 20 % | Moyen-faible | Insister sur un reglement amiable en phase de conciliation |
>
> **5. Approche recommandee**
>
> Adresser immediatement une opposition ecrite formelle au licenciement. Deposer une requete de conciliation aupres de l'Autorite de conciliation de Geneve avec une demande d'ouverture de CHF 65'000 (ancrage haut). Objectif transactionnel de CHF 45'000-55'000. Ne proceder devant le Tribunal des prud'hommes que si la conciliation echoue et que la mandante accepte le risque lie aux couts et aux delais.

**Fonctionnalite cle** : BetterCallClaude calcule l'exposition financiere sur la base des tarifs judiciaires effectifs de Geneve et trace la voie procedurale complete specifique au canton.

---

## Etape 5 : redaction de l'acte judiciaire

Generation d'une demande en justice professionnelle pour le Tribunal des prud'hommes de Geneve.

```
/bettercallclaude:draft Demande en justice pour licenciement abusif, Maria Keller c. AlphaFin SA, Tribunal des prud'hommes de Geneve. Conclusions : (1) constatation du caractere abusif du licenciement au sens de l'art. 336 let. d CO, (2) indemnite de CHF 75'000 fondee sur l'art. 336a CO, (3) correction du delai de conge. Fondee sur des represailles apres signalement de non-conformite FINMA, prolongation nulle de la periode d'essai.
```

**Agents actives** : swiss-legal-drafter

Le drafter produit un acte judiciaire formel conforme aux exigences du CPC. Voici un extrait -- les conclusions et le debut de l'expose des faits :

> **Demande en justice**
> **au Tribunal des prud'hommes de Geneve**
>
> ---
>
> **Demanderesse** : Maria Keller, [adresse], representee par [Me Prenom Nom, avocat/avocate]
>
> **Defenderesse** : AlphaFin SA, [adresse], [numero RC]
>
> ---
>
> ## I. Conclusions
>
> Plaise au Tribunal des prud'hommes :
>
> 1. Constater que le licenciement signifie par la defenderesse le 24 mars 2026 est abusif au sens de l'art. 336 al. 1 let. d CO.
>
> 2. Condamner la defenderesse a verser a la demanderesse une indemnite de CHF 75'000.00 (six mois de salaire) en application de l'art. 336a al. 2 CO, avec interets a 5 % l'an des le 1er mai 2026.
>
> 3. Constater que la prolongation retroactive de la periode d'essai du 10 mars 2026 est nulle et que le delai de conge ordinaire de deux mois prevu a l'art. 335c al. 1 CO s'applique.
>
> 4. Le tout sous suite de frais et depens.
>
> ---
>
> ## II. Faits
>
> 1. La demanderesse etait employee depuis [date] en qualite de lead backend engineer aupres de la defenderesse a Geneve. Son salaire mensuel brut s'elevait a CHF 12'500.00.
>
> 2. Le 3 mars 2026, la demanderesse a signale par courriel a son superieur direct et au CTO de la defenderesse que le module de traitement des paiements n'implementait pas le monitoring des transactions prescrit par la FINMA.
>
> 3. Le 10 mars 2026 -- sept jours apres le signalement de conformite -- la defenderesse a prolonge retroactivement la periode d'essai de la demanderesse. Cette prolongation est nulle (ATF 125 III 70), la periode d'essai initiale etant echue depuis plus de deux ans.
>
> 4. Le 24 mars 2026 -- 21 jours apres le signalement de conformite -- la defenderesse a resilie les rapports de travail pour le 30 avril 2026, moyennant un delai d'un mois.
>
> [...]
>
> ---
>
> *Le present document a ete elabore avec le soutien de BetterCallClaude. L'ensemble du contenu doit etre revu et approuve par l'avocate ou l'avocat en charge du dossier.*

**Fonctionnalite cle** : le drafter structure le document conformement aux exigences du CPC -- conclusions, faits, discussion juridique, moyens de preuve -- avec un format de citation correct et un registre formel. Les champs a completer (adresses, dates) sont signales pour l'avocate ou l'avocat.

---

## Etape 6 : consolidation finale

Une fois toutes les etapes du pipeline achevees, les resultats sont consolides dans un resume executif.

```
/bettercallclaude:summarize --medium
```

**Agents actives** : swiss-legal-summarizer

> **Resume du dossier -- Keller c. AlphaFin SA**
>
> **Objet** : licenciement abusif (art. 336 let. d CO)
> **Juridiction** : Tribunal des prud'hommes de Geneve
> **Evaluation** : issue favorable probable (65-75 %)
>
> **Constatations principales** :
> - L'intervalle de 21 jours entre le signalement de conformite et le licenciement fonde une forte presomption de represailles (ATF 136 III 513).
> - La prolongation retroactive de la periode d'essai est nulle de plein droit (ATF 125 III 70).
> - Indemnite attendue : 3-4 mois de salaire (CHF 37'500-50'000).
> - Objectif transactionnel : CHF 45'000-55'000.
>
> **Actions recommandees** :
> 1. Adresser immediatement une opposition ecrite a AlphaFin SA.
> 2. Deposer une requete de conciliation aupres de l'Autorite de conciliation de Geneve.
> 3. Ouvrir des negociations transactionnelles avec une demande d'ancrage de CHF 65'000.
> 4. Preparer une demande en justice pour le Tribunal des prud'hommes de Geneve en tant que solution subsidiaire.
>
> **Documents produits** :
> - Memorandum de recherche avec 4 arrets de principe ATF
> - Analyse contradictoire avec evaluation des probabilites
> - Strategie contentieuse avec analyse cout-benefice
> - Projet de demande en justice (Tribunal des prud'hommes de Geneve)
>
> *Avertissement professionnel : cette analyse a ete produite par un outil de recherche juridique fonde sur l'intelligence artificielle. L'ensemble des resultats doit etre revu et valide par une avocate ou un avocat qualifie(e) inscrit(e) au barreau suisse avant toute utilisation. Le present document ne constitue pas un avis juridique.*

**Fonctionnalite cle** : le summarizer deduplique les avertissements, les definitions terminologiques et les citations qui apparaissent dans les resultats de plusieurs agents, produisant un resume consolide et epure.

---

## Conseils et prochaines etapes

### Personnaliser le workflow

Il n'est pas necessaire d'executer chaque etape. Quelques variantes :

- **Recherche seule** : executez uniquement `/bettercallclaude:research` pour obtenir les precedents sans l'ensemble du pipeline.
- **Passer directement a la strategie** : si les questions juridiques sont deja connues, passez directement a `/bettercallclaude:strategy`.
- **Analyse contradictoire rapide** : utilisez `/bettercallclaude:adversarial --short` pour une evaluation condensee.
- **Pipeline automatise** : `/bettercallclaude:workflow litigation-prep` execute l'ensemble du pipeline (Researcher -> Strategist -> Adversarial -> Drafter) avec des transferts automatiques.

### Variations cantonales

Le meme cas a Zurich impliquerait des regles procedurales differentes :

```
/bettercallclaude:strategy Missbrauchliche Kundigung, Maria Keller gegen AlphaFin AG, Zurich.
Art. 336 Abs. 1 lit. d OR, Vergeltung nach FINMA-Compliance-Meldung.
```

BetterCallClaude bascule automatiquement vers la terminologie juridique allemande (OR au lieu de CO, BGE au lieu d'ATF) et oriente vers la procedure cantonale zurichoise (Arbeitsgericht) de maniere automatique.

### Analyse multilingue

Chaque command peut etre execute en francais, en allemand ou en italien :

```
# Francais (standard pour Geneve)
/bettercallclaude:research "Licenciement abusif art. 336 CO"

# Allemand
/bettercallclaude:research "Missbrauchliche Kundigung OR 336"

# Italien
/bettercallclaude:research "Disdetta abusiva art. 336 CO"
```

### Documentation complementaire

- [Reference des commands](../command-reference.md) -- Documentation complete des 18 commands
- [Workflow de recherche juridique](../workflows/research-precedents.md) -- Approfondissement de la methodologie de recherche ATF
- [Workflow de strategie de cas](../workflows/case-strategy.md) -- Guide detaille d'elaboration de strategie
- [Guide d'installation](../INSTALL.md) -- Installation, configuration des MCP servers, depannage

---

## Fonctionnalites de BCC demontrees dans ce tutoriel

| Fonctionnalite | Etape | Composants utilises |
|----------------|-------|---------------------|
| Session de briefing | Etape 1 | agent briefing-coordinator, skill de detection de juridiction |
| Recherche juridique | Etape 2 | agent researcher, MCP bge-search, MCP entscheidsuche, MCP fedlex-sparql |
| Verification de citation | Etape 2 | agent citation-specialist, MCP legal-citations |
| Analyse contradictoire | Etape 3 | agents advocate + adversary + judicial-analyst, skill adversarial-analysis |
| Strategie contentieuse | Etape 4 | agents case-strategist + risk-analyst + procedure-specialist, skill strategy |
| Redaction d'acte judiciaire | Etape 5 | agent drafter, skill swiss-legal-drafting |
| Consolidation des resultats | Etape 6 | agent summarizer |
| Support multilingue | Tout au long | detection de la langue, terminologie juridique DE/FR/IT/EN |
| Routage juridictionnel | Tout au long | skill swiss-jurisdictions, detection cantonale |

**Total** : 10 agents, 5 commands, 4 skills, 4 MCP servers demontres dans un seul dossier.
