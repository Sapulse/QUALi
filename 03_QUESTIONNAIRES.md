# QUALi — Questionnaires Améliorés

---

## PARTIE 4 — REFONTE ET AMÉLIORATION DES QUESTIONNAIRES

### Analyse critique des questionnaires actuels

#### Ce qui fonctionne bien
- La grille d'auto-évaluation du niveau actuel est pertinente
- La liste des attentes (checkboxes) est bien construite
- La distinction dirigeant / salarié est utile

#### Ce qui pose problème

| Problème identifié | Impact |
|---|---|
| Le questionnaire avant est trop générique : il ne change pas selon l'axe | On ne capte pas les besoins spécifiques à chaque niveau d'accompagnement |
| Le questionnaire à chaud contient des formulations scolaires ("enseignant/démonstrateur", "élèves") | Inapproprié pour un public de professionnels adultes |
| La double section "Contenu de la formation" (avec deux échelles différentes) est redondante et confuse | Surcharge cognitive, risque d'abandon |
| "Niveau de compétences en début / fin de formation" dans le questionnaire à chaud est prématuré | L'impact réel n'est pas mesurable juste après la formation |
| Aucune question sur le gain de temps anticipé ou le ROI perçu | Manque total pour le pilotage commercial |
| Pas de question de satisfaction globale en format NPS (0-10) sur le questionnaire avant | Impossible de détecter les attentes très élevées ou les réticences |
| Aucun identifiant technique (session_id) prévu | Impossible de relier les réponses entre elles automatiquement |
| Les questions "Handicap" sont posées sans délicatesse suffisante | Risque d'inconfort |
| Le questionnaire à froid est absent | C'est le manque le plus critique |

#### Principes de refonte
1. **Tronc commun** pour les 3 questionnaires : les questions clés sont identiques pour tous les axes → comparaison possible
2. **Bloc spécifique par axe** : 3 à 5 questions adaptées selon Déclic / Activation / Transformation / Systèmes
3. **Durée cible** : avant = 4 min, chaud = 5 min, froid = 6 min
4. **Logique progressive** : questions les plus importantes d'abord (en cas d'abandon)
5. **Formulations adultes** : on parle de "participants", pas d'"élèves"

---

## QUESTIONNAIRE 1 — POSITIONNEMENT AVANT FORMATION

### Objectif
Comprendre le niveau de départ, les attentes précises et les besoins spécifiques du participant avant la session. Utilisé pour adapter le contenu, constituer un dossier Qualiopi, et créer la ligne de base pour mesurer la progression.

---

### En-tête du formulaire (masqué du participant, transmis via URL)
```
Champs cachés (pré-remplis via paramètres URL) :
- session_id
- axe (Déclic / Activation / Transformation / Systèmes)
- formateur
- date_session
```

---

### SECTION A — Identification (obligatoire)

| # | Question | Type | Obligatoire |
|---|----------|------|-------------|
| A1 | Votre prénom et nom | Texte court | Oui |
| A2 | Votre adresse email professionnelle | Email | Oui |
| A3 | Le nom de votre entreprise ou organisation | Texte court | Oui |
| A4 | Votre fonction / poste | Texte court | Oui |
| A5 | Vous participez à cette formation... | Choix unique | Oui |

**A5 — Options :**
- En tant que participant individuel (salarié, indépendant, dirigeant qui se forme lui-même)
- En tant que dirigeant ou responsable : je représente des membres de mon équipe
- Dans le cadre d'un groupe constitué (plusieurs personnes de ma structure)

> *Si vous répondez pour votre équipe, pensez aux personnes concernées lorsque vous répondez aux questions suivantes.*

---

### SECTION B — Niveau de départ (obligatoire — tronc commun)

> *Évaluez honnêtement votre niveau actuel. Il n'y a pas de bonne ou mauvaise réponse.*

| # | Affirmation | Échelle |
|---|-------------|---------|
| B1 | J'ai déjà entendu parler de l'intelligence artificielle | Jamais / Vaguement / Bien / Très bien |
| B2 | J'ai déjà utilisé un outil d'IA (ChatGPT, Copilot, Gemini…) | Jamais / 1 à 2 fois / Régulièrement / Quotidiennement |
| B3 | Je me sens à l'aise avec les outils numériques en général | Pas du tout / Un peu / Assez bien / Très à l'aise |
| B4 | J'utilise déjà des outils numériques pour gagner du temps au travail | Pas du tout / Rarement / Souvent / Tous les jours |

*Format recommandé : tableau avec 4 niveaux par ligne (matrice de sélection)*

---

### SECTION C — Attentes et objectifs (obligatoire — tronc commun)

| # | Question | Type | Obligatoire |
|---|----------|------|-------------|
| C1 | Qu'est-ce qui vous a motivé à rejoindre cette formation ? | Choix multiple (max 3) | Oui |
| C2 | Si vous aviez un seul objectif à atteindre à l'issue de cette formation, quel serait-il ? | Texte libre | Oui |
| C3 | Sur quels types de tâches passez-vous le plus de temps aujourd'hui (et que vous aimeriez accélérer) ? | Choix multiple (max 3) | Oui |
| C4 | Avez-vous déjà tenté d'utiliser l'IA dans votre travail ? Si oui, qu'est-ce qui vous a bloqué ? | Texte libre | Non |

**C1 — Options :**
- Découvrir ce qu'est l'IA et comprendre ses possibilités
- Gagner du temps sur des tâches répétitives
- Améliorer la qualité de mes productions (textes, supports, analyses)
- Créer du contenu plus rapidement
- Mieux m'organiser et structurer mes idées
- Automatiser certains processus dans mon activité
- Rester à jour et ne pas prendre de retard sur le marché
- Me différencier ou innover dans mon activité
- Comprendre pour mieux conseiller mes clients ou collaborateurs
- Autre (préciser)

**C3 — Options :**
- Rédaction d'emails, comptes rendus, rapports
- Recherche d'informations et veille
- Création de contenus (posts, présentations, descriptifs)
- Gestion et organisation des priorités
- Communication client
- Préparation de réunions ou de formations
- Administration et traitement de documents
- Analyse de données ou de résultats
- Autre (préciser)

---

### SECTION D — Questions spécifiques par axe

> *Ces questions s'affichent uniquement selon l'axe de la session (logique conditionnelle Tally)*

#### Axe DÉCLIC uniquement
| # | Question | Type |
|---|----------|------|
| D-DEC-1 | Avant cette session, quel mot décrit le mieux votre rapport à l'IA ? | Choix unique : Curiosité / Méfiance / Indifférence / Enthousiasme / Inquiétude |
| D-DEC-2 | Y a-t-il quelque chose que vous avez entendu sur l'IA qui vous interpelle ou vous préoccupe ? | Texte libre |

#### Axe ACTIVATION uniquement
| # | Question | Type |
|---|----------|------|
| D-ACT-1 | Avez-vous déjà essayé de créer un prompt ou d'utiliser ChatGPT pour une tâche précise ? | Oui / Non / J'ai essayé mais ça n'a pas fonctionné comme attendu |
| D-ACT-2 | Quelle est la tâche concrète que vous souhaitez réussir à réaliser avec l'IA à l'issue de cette formation ? | Texte libre |

#### Axe TRANSFORMATION uniquement
| # | Question | Type |
|---|----------|------|
| D-TRA-1 | Sur une échelle de 0 à 10, à quel point l'IA est-elle déjà intégrée dans votre façon de travailler ? | Note 0-10 |
| D-TRA-2 | Quels sont les principaux freins à une utilisation plus systématique dans votre activité ? | Choix multiple : Manque de temps pour tester / Incertitude sur la fiabilité / Manque de méthode / Résistances dans l'équipe / Contraintes réglementaires / Autre |
| D-TRA-3 | Quel processus ou domaine de votre activité souhaitez-vous transformer en priorité ? | Texte libre |

#### Axe SYSTÈMES uniquement
| # | Question | Type |
|---|----------|------|
| D-SYS-1 | Quel est le projet ou le besoin concret qui vous amène à cet accompagnement ? | Texte libre |
| D-SYS-2 | Avez-vous déjà utilisé des outils d'automatisation, de création de tableaux de bord ou d'agents IA ? | Jamais / J'ai exploré / J'utilise déjà des outils basiques / J'ai des projets avancés |
| D-SYS-3 | Quels outils utilisez-vous actuellement dans votre activité (CRM, tableur, outil de gestion, etc.) ? | Texte libre |

---

### SECTION E — Accessibilité (obligatoire — tronc commun)

| # | Question | Type | Obligatoire |
|---|----------|------|-------------|
| E1 | Avez-vous des besoins particuliers pour participer dans les meilleures conditions (accessibilité, langue, rythme, situation de handicap) ? | Oui / Non | Oui |
| E2 | Si oui, pouvez-vous nous en dire plus afin que nous puissions vous accompagner au mieux ? | Texte libre | Non (affiché si E1 = Oui) |

---

### SECTION F — Consentement et attentes finales

| # | Question | Type | Obligatoire |
|---|----------|------|-------------|
| F1 | Si vous le souhaitez, partagez un commentaire libre ou une question avant la formation | Texte libre | Non |
| F2 | Acceptez-vous d'être recontacté après la formation pour un retour d'expérience ? | Oui / Non | Oui |

---

## QUESTIONNAIRE 2 — SATISFACTION À CHAUD

### Objectif
Mesurer la satisfaction immédiate, la qualité perçue de la session et du formateur, recueillir les premiers verbatims. Utilisé pour Qualiopi, amélioration continue, et déclenchement des invitations d'avis.

---

### En-tête (champs cachés via URL)
```
- session_id
- participant_id
- axe
- formateur
- date_session
```

---

### SECTION A — Identification rapide (pré-remplie si possible)

| # | Question | Type | Obligatoire |
|---|----------|------|-------------|
| A1 | Votre prénom et nom | Texte court | Oui |
| A2 | Formateur de votre session | Choix unique : César Robitzer / Mickael Mège / Autre | Oui |

---

### SECTION B — Satisfaction globale (obligatoire — tronc commun)

| # | Question | Type | Obligatoire |
|---|----------|------|-------------|
| B1 | Sur 10, quelle est votre satisfaction globale sur cette session ? | Note 0-10 | Oui |
| B2 | Recommanderiez-vous cette formation à un collègue ou un proche ? | Oui certainement / Probablement oui / Je ne sais pas / Probablement pas / Non | Oui |
| B3 | En une phrase, qu'est-ce que vous retenez de cette journée ? | Texte libre | Oui |

---

### SECTION C — Évaluation de la session (tronc commun)

*Échelle : 1 = Pas du tout d'accord → 5 = Tout à fait d'accord*

| # | Affirmation |
|---|-------------|
| C1 | Le contenu était adapté à mon niveau de départ |
| C2 | Les explications étaient claires et bien illustrées |
| C3 | La session correspondait à ce que j'attendais |
| C4 | Le rythme de la session était adapté |
| C5 | J'ai eu suffisamment de temps pour pratiquer et expérimenter |
| C6 | Le formateur était à l'écoute et disponible |
| C7 | L'ambiance et la dynamique de groupe étaient bonnes |

---

### SECTION D — Apport perçu (obligatoire — tronc commun)

| # | Question | Type |
|---|----------|------|
| D1 | Comparez votre niveau par rapport à avant la formation | Nettement progressé / Un peu progressé / Stable / Je ne sais pas encore |
| D2 | Citez 1 à 3 choses concrètes que vous allez faire différemment ou tester dès maintenant | Texte libre |
| D3 | Y a-t-il quelque chose que vous auriez souhaité approfondir davantage ? | Texte libre |

---

### SECTION E — Questions spécifiques par axe

#### Axe DÉCLIC uniquement
| # | Question | Type |
|---|----------|------|
| E-DEC-1 | Est-ce que cette session a changé votre regard sur l'IA ? | Oui complètement / Oui un peu / Pas vraiment / Non |
| E-DEC-2 | Quel outil ou usage vous a le plus intrigué ou donné envie d'explorer ? | Texte libre |

#### Axe ACTIVATION uniquement
| # | Question | Type |
|---|----------|------|
| E-ACT-1 | Avez-vous réussi à créer quelque chose de concret pendant la session ? | Oui, et ça a bien fonctionné / Oui, avec de l'aide / Pas vraiment / Non |
| E-ACT-2 | Quel est le premier usage que vous allez tester dans les 7 prochains jours ? | Texte libre |

#### Axe TRANSFORMATION uniquement
| # | Question | Type |
|---|----------|------|
| E-TRA-1 | Sur quels processus ou domaines pensez-vous appliquer ce que vous avez vu ? | Texte libre |
| E-TRA-2 | Avez-vous identifié des obstacles ou résistances dans votre organisation à l'adoption de ces pratiques ? | Oui / Non / Peut-être — précisez si souhaité |

#### Axe SYSTÈMES uniquement
| # | Question | Type |
|---|----------|------|
| E-SYS-1 | L'accompagnement a-t-il correspondu à votre besoin initial ? | Oui parfaitement / Oui globalement / Partiellement / Non |
| E-SYS-2 | Quelles fonctionnalités ou aspects du système mis en place vous semblent les plus utiles ? | Texte libre |
| E-SYS-3 | Y a-t-il des ajustements ou compléments à prévoir ? | Texte libre |

---

### SECTION F — Verbatims et finalisation

| # | Question | Type | Obligatoire |
|---|----------|------|-------------|
| F1 | Qu'est-ce qui a été le plus utile ou marquant dans cette session ? | Texte libre | Non |
| F2 | Qu'est-ce qui pourrait être amélioré ? | Texte libre | Non |
| F3 | Un dernier commentaire libre ? | Texte libre | Non |

---

## QUESTIONNAIRE 3 — IMPACT À FROID

### Objectif
Mesurer l'impact réel de la formation plusieurs semaines après. Recueillir des données concrètes sur la mise en application, les gains de temps, les bénéfices mesurables. Aliment principal pour le pilotage, les cas clients et les arguments commerciaux.

---

### En-tête (champs cachés via URL)
```
- session_id
- participant_id
- axe
- formateur
- date_session
- semaines_depuis_formation (calculé automatiquement)
```

---

### SECTION A — Identification rapide

| # | Question | Type | Obligatoire |
|---|----------|------|-------------|
| A1 | Votre prénom et nom (pour relier vos réponses) | Texte court | Oui |

---

### SECTION B — Mise en application (obligatoire — tronc commun)

| # | Question | Type | Obligatoire |
|---|----------|------|-------------|
| B1 | Depuis la formation, avez-vous mis en pratique ce que vous avez appris ? | Oui régulièrement / Oui quelques fois / Une seule fois / Pas encore / Non | Oui |
| B2 | Si vous avez mis en pratique : sur quelles tâches ou situations ? | Texte libre | Non |
| B3 | Si vous n'avez pas encore mis en pratique : qu'est-ce qui vous en a empêché ? | Texte libre | Non (affiché si B1 = Pas encore ou Non) |
| B4 | Sur une échelle de 0 à 10, à quel point l'IA fait-elle maintenant partie de votre façon de travailler ? | Note 0-10 | Oui |

---

### SECTION C — Gains et bénéfices concrets (obligatoire — tronc commun)

| # | Question | Type | Obligatoire |
|---|----------|------|-------------|
| C1 | Estimez-vous avoir gagné du temps grâce à ce que vous avez appris ? | Oui clairement / Oui un peu / Pas encore / Non | Oui |
| C2 | Si oui, combien de temps estimez-vous gagner par semaine ? | Moins de 30 min / 30 min à 1h / 1h à 2h / Plus de 2h / Je ne sais pas | Non |
| C3 | Quels bénéfices concrets avez-vous observés depuis la formation ? | Choix multiple | Oui |
| C4 | Y a-t-il un exemple précis de situation où vous avez utilisé ce que vous avez appris avec succès ? | Texte libre | Non |

**C3 — Options :**
- Je rédige plus vite (emails, comptes rendus, contenus)
- Je cherche des informations plus efficacement
- Je produis des contenus de meilleure qualité
- Je m'organise mieux ou structure mieux mes idées
- J'automatise des tâches répétitives
- Je me sens plus à l'aise avec les outils numériques
- Je prends de meilleures décisions grâce à une meilleure analyse
- Je gagne en confiance sur les outils IA
- J'ai créé un outil ou un système que j'utilise régulièrement
- Je n'observe pas encore de bénéfice tangible
- Autre (préciser)

---

### SECTION D — Satisfaction durable (tronc commun)

| # | Question | Type | Obligatoire |
|---|----------|------|-------------|
| D1 | Avec le recul, quel est votre niveau de satisfaction global sur cette formation ? | Note 0-10 | Oui |
| D2 | Si vous deviez refaire ce choix, recommanderiez-vous cette formation ? | Oui absolument / Probablement oui / Je ne sais pas / Probablement pas / Non | Oui |
| D3 | Qu'est-ce qui a eu le plus d'impact sur votre activité ou votre façon de travailler ? | Texte libre | Non |

---

### SECTION E — Questions spécifiques par axe

#### Axe DÉCLIC uniquement
| # | Question | Type |
|---|----------|------|
| E-DEC-1 | Avez-vous parlé de l'IA autour de vous (collègues, clients, réseau) depuis la formation ? | Oui souvent / Oui parfois / Non |
| E-DEC-2 | Avez-vous essayé d'autres outils IA depuis la session ? Si oui, lesquels ? | Texte libre |

#### Axe ACTIVATION uniquement
| # | Question | Type |
|---|----------|------|
| E-ACT-1 | Avez-vous intégré l'IA dans votre routine de travail quotidienne ou hebdomadaire ? | Oui / En cours / Non |
| E-ACT-2 | Quel a été votre usage le plus fréquent ou le plus efficace depuis la formation ? | Texte libre |

#### Axe TRANSFORMATION uniquement
| # | Question | Type |
|---|----------|------|
| E-TRA-1 | Avez-vous impliqué votre équipe ou vos collaborateurs dans ces nouvelles pratiques ? | Oui / En cours / Non / Je suis seul(e) |
| E-TRA-2 | Avez-vous modifié ou créé de nouveaux processus dans votre activité grâce à ce que vous avez appris ? | Oui, décrivez : [texte libre] / Non / En cours |
| E-TRA-3 | Estimez-vous que cette formation a eu un impact sur votre chiffre d'affaires ou votre productivité globale ? | Oui significatif / Oui modeste / Trop tôt pour le dire / Non |

#### Axe SYSTÈMES uniquement
| # | Question | Type |
|---|----------|------|
| E-SYS-1 | Le système, outil ou tableau de bord mis en place est-il opérationnel ? | Oui et utilisé quotidiennement / Oui mais sous-utilisé / Partiellement / Non |
| E-SYS-2 | Avez-vous rencontré des difficultés techniques ou d'adoption depuis la mise en place ? | Texte libre |
| E-SYS-3 | Quel est l'impact estimé sur votre productivité ou votre pilotage depuis la mise en place ? | Texte libre |
| E-SYS-4 | Avez-vous des besoins complémentaires ou des évolutions à prévoir ? | Texte libre |

---

### SECTION F — Besoins futurs et finalisation

| # | Question | Type | Obligatoire |
|---|----------|------|-------------|
| F1 | Avez-vous des besoins en formation ou en accompagnement pour la suite ? | Oui / Non / Peut-être |
| F2 | Si oui, dans quel domaine ? | Choix multiple (reprendre la liste des axes + domaines spécifiques) | Non |
| F3 | Un dernier commentaire libre ou témoignage que vous souhaitez partager ? | Texte libre | Non |

---

### Tableau récapitulatif des 3 questionnaires

| Critère | Positionnement avant | Satisfaction à chaud | Impact à froid |
|---------|----------------------|----------------------|----------------|
| Durée estimée | 3-5 min | 4-6 min | 5-7 min |
| Questions obligatoires | 8 | 7 | 7 |
| Questions tronc commun | 100% | 100% | 100% |
| Questions spécifiques axe | 2-3 | 1-3 | 2-4 |
| Utile Qualiopi | Oui (niveau départ, besoins) | Oui (satisfaction, acquis) | Oui (mise en application) |
| Utile pilotage ROI | Non | Partiel | Oui (central) |
| Déclenche invitation avis | Non | Si note ≥ 8 | Si note ≥ 8 et pas encore fait |
| Déclenche alerte interne | Non | Si note ≤ 5 | Si note ≤ 5 |
