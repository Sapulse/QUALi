# QUALi — Plan de Déploiement MVP

---

## PARTIE 8 — PLAN DE DÉPLOIEMENT MVP

### Principe directeur

> **"Deployé imparfait vaut mieux que parfait non lancé."**
> La V1 doit être opérationnelle en moins de 3 semaines. Elle sera imparfaite.
> C'est voulu. L'objectif est d'apprendre vite, pas de construire parfait.

---

## Phase 0 — Décisions à prendre avant de commencer

**Ces décisions bloquent tout le reste. Les prendre en 1 séance de travail (1h max) :**

| # | Décision | Options | Recommandation |
|---|----------|---------|----------------|
| D1 | Outil de formulaires | Tally.so / Google Forms / Typeform | **Tally.so** (meilleur équilibre UX/fonctionnalités/prix) |
| D2 | Base de données | Airtable / Notion / Google Sheets | **Airtable** (relations + API + automations intégrées) |
| D3 | Outil d'automatisation | Make.com / Zapier / n8n | **Make.com** (meilleur rapport puissance/coût) |
| D4 | Outil d'email | Brevo / Mailchimp / Gmail direct | **Brevo** (gratuit jusqu'à 300/j, API propre) |
| D5 | Dashboard | Looker Studio / Airtable vues / Notion | **Looker Studio** (gratuit, puissant, connecté à GSheets) |
| D6 | Plateforme d'avis cible | Google / Trustpilot / LinkedIn | **Google + LinkedIn** (les deux principaux pour une OF) |
| D7 | Qui gère les statuts sessions ? | Toi seul / assistante / partagé | Décider qui met à jour Airtable après chaque session |

---

## Phase 1 — Fondation (Semaine 1 : jours 1 à 5)

### Étape 1.1 — Créer la base Airtable (Jour 1)

**Durée estimée : 3h**

1. Créer un workspace Airtable dédié : "QUALi - Suivi formations"
2. Créer les tables dans cet ordre :
   - `axes` (4 lignes : Déclic, Activation, Transformation, Systèmes)
   - `entreprises`
   - `clients`
   - `participants`
   - `sessions`
   - `reponses_avant`
   - `reponses_chaud`
   - `reponses_froid`
   - `statut_avis`
   - `indicateurs_calcules`
3. Créer les liaisons entre tables (champs Linked Record)
4. Créer les vues filtrées utiles dans sessions : "Sessions ce mois", "Sessions à venir", "Sessions Déclic", etc.

> **Piège à éviter** : Ne pas créer des champs pour tout ce qui pourrait être utile un jour. Seulement ce qui est dans le modèle de données V1.

### Étape 1.2 — Créer les 3 formulaires Tally (Jours 2-3)

**Durée estimée : 4h pour les 3 formulaires**

Pour chaque formulaire :
1. Créer le formulaire avec tronc commun + logique conditionnelle par axe
2. Activer les champs cachés : `session_id`, `participant_id`, `axe`
3. Configurer le webhook Tally vers Make.com (URL à créer à l'étape suivante)
4. Tester avec des réponses factices

**Ordre de priorité :**
1. Questionnaire à chaud (le plus utilisé immédiatement)
2. Questionnaire de positionnement avant
3. Questionnaire à froid (pas encore utilisé en semaine 1, mais à créer)

### Étape 1.3 — Configurer Make.com (Jours 3-4)

**Durée estimée : 4h**

Créer dans cet ordre :
1. **Scénario 11** (Tally → Airtable) : le plus critique, à valider en premier
2. **Scénario 1** (Envoi avant) : à tester avec une session test
3. **Scénario 3** (Envoi chaud) : à tester avec la même session test
4. Les autres scénarios peuvent attendre la semaine 2

### Étape 1.4 — Configurer Brevo (Jour 4)

**Durée estimée : 2h**

1. Créer un compte Brevo (gratuit)
2. Créer les templates d'email (copier-coller depuis ce document)
3. Configurer les variables dynamiques : `{{prenom}}`, `{{nom_axe}}`, `{{date_session}}`, `{{lien_formulaire}}`
4. Tester l'envoi depuis Make.com

### Étape 1.5 — Test bout en bout (Jour 5)

**Durée estimée : 2h**

1. Créer une session test dans Airtable (une vraie, avec ta propre adresse email comme participant)
2. Vérifier que l'email d'avant arrivé
3. Remplir le formulaire de positionnement → vérifier que la réponse arrive dans Airtable
4. Passer la session en statut "Réalisée" → vérifier que l'email à chaud part
5. Remplir le formulaire à chaud avec une note ≥ 8 → vérifier que l'invitation avis part
6. Remplir avec une note ≤ 5 → vérifier que l'alerte interne part

> **Critère de succès de la phase 1** : Le cycle complet fonctionne de bout en bout sur une session test.

---

## Phase 2 — Lancement réel & Dashboard (Semaine 2 : jours 6 à 10)

### Étape 2.1 — Premier vrai déploiement (Jour 6)

1. Identifier la prochaine session réelle
2. Créer la session dans Airtable (vrais participants, vraie date)
3. Vérifier que les emails partent automatiquement
4. Suivre manuellement les statuts pendant quelques jours

### Étape 2.2 — Créer le dashboard Looker Studio (Jours 7-8)

**Durée estimée : 4h**

1. Exporter les tables Airtable vers Google Sheets (via Make ou Airtable Sync)
2. Connecter Google Sheets à Looker Studio
3. Créer les vues dans l'ordre de priorité :
   - Vue 1 : Dashboard Global (KPIs en cartes + 2 graphiques principaux)
   - Vue 3 : Satisfaction & Qualité
   - Vue 4 : Impact & Application
   - Les autres vues en V1.1

**À éviter en phase 2 :** Les graphiques trop complexes. 5 KPIs qui fonctionnent valent mieux que 20 KPIs approximatifs.

### Étape 2.3 — Compléter les scénarios Make (Jour 8-9)

Les scénarios restants à créer :
- Scénario 2 (relance avant)
- Scénario 4 (relance chaud)
- Scénarios 6 et 10 (alertes internes)
- Scénario 7 (envoi froid)

### Étape 2.4 — Documentation interne (Jour 10)

Créer une fiche opérationnelle (1 page max) :
> **"Comment enregistrer une nouvelle session dans le système"**
> 1. Ouvrir Airtable → Table Sessions
> 2. Créer une nouvelle ligne avec : client, axe, date, participants, formateur
> 3. Passer le statut à "Confirmée"
> 4. ✓ Le reste est automatique

---

## Phase 3 — Consolidation et apprentissage (Semaines 3-4)

### Étape 3.1 — Déployer sur 3 à 5 sessions réelles

Objectif : avoir des vraies données avant d'investir dans des améliorations.

**Ce qu'il faut surveiller :**
- Taux de réponse aux questionnaires (objectif : > 60% à chaud)
- Erreurs dans les automatisations (Make.com logs)
- Problèmes d'identifiants (session_id mal transmis)

### Étape 3.2 — Premier bilan à 30 jours

**Questions à se poser après 30 jours :**

| Question | Indicateur à regarder |
|---------|----------------------|
| Le système tourne-t-il en autonomie ? | Nombre d'interventions manuelles nécessaires |
| Les questionnaires sont-ils remplis ? | Taux de réponse chaud (objectif > 60%) |
| Les données arrivent-elles correctement dans Airtable ? | Taux d'erreur de routage |
| Le dashboard affiche-t-il des données exploitables ? | Oui / Non |
| Y a-t-il des doublons ou des incohérences ? | Audit visuel des tables |
| La séquence à froid fonctionne-t-elle ? | Vérifier les premières réponses froid reçues |

### Étape 3.3 — Ajustements de contenu

Après les premières vraies réponses :
- Reformuler les questions qui génèrent trop de "Ne sait pas" ou de réponses vides
- Raccourcir les questionnaires si le taux de completion est < 50%
- Ajouter une question si un insight manque

---

## Critères de succès du MVP

### Le MVP est concluant si, à 60 jours :

| Critère | Seuil minimal |
|---------|--------------|
| Sessions enregistrées dans le système | ≥ 5 |
| Taux de réponse questionnaire à chaud | ≥ 60% |
| Taux de réponse questionnaire à froid | ≥ 35% |
| Automatisations fonctionnant sans intervention manuelle | ≥ 80% des cas |
| Dashboard affichant des données exploitables | Oui |
| Au moins 1 insight actionné (changement de contenu, process, comm) | Oui |
| Aucun incident sur l'outil existant | Oui (indépendance confirmée) |

---

## Pièges et risques à anticiper

### Risques techniques

| Risque | Probabilité | Impact | Mitigation |
|--------|------------|--------|-----------|
| `session_id` mal transmis dans Tally | Moyen | Élevé | Tester systématiquement sur chaque formulaire |
| Doublons de réponses dans Airtable | Faible | Moyen | Vérifier la logique de déduplication dans Make |
| Emails qui atterrissent en spam | Moyen | Élevé | Configurer DKIM/SPF dans Brevo, utiliser ton domaine |
| Limites du plan gratuit Make.com | Faible | Moyen | Surveiller le quota d'opérations (1000/mois gratuit) |
| Perte de connexion Tally → Make | Faible | Élevé | Activer les alertes d'erreur Make.com |

### Risques humains

| Risque | Probabilité | Impact | Mitigation |
|--------|------------|--------|-----------|
| Oublier de mettre la session en "Réalisée" | Élevé | Moyen | Créer une checklist de fin de session |
| Participants qui ne répondent pas | Élevé | Faible | Normaliser (taux 50-60% est déjà bien) |
| Formulaire trop long = abandon | Moyen | Élevé | Respecter les durées cibles (voir questionnaires) |
| Mauvaise saisie des emails participants | Moyen | Élevé | Valider les emails à la création de la session |

### Risques produit

| Risque | Probabilité | Impact | Mitigation |
|--------|------------|--------|-----------|
| Les données ne sont pas suffisantes pour un dashboard utile | Moyen | Moyen | Lancer vite, accepter 60 jours de "chauffe" |
| Questions trop génériques = données peu actionnables | Moyen | Élevé | Suivre les questionnaires proposés dans ce document |
| Trop de questionnaires = irritation des participants | Faible | Élevé | Jamais plus de 3 contacts par parcours |

---

## Ce qu'il faut éviter absolument en V1

| À éviter | Pourquoi |
|----------|---------|
| Intégrer le système à l'outil existant | Risque, complexité, perte de temps |
| Créer plus de 3 questionnaires | Surcharge pour les participants |
| Chercher à automatiser 100% dès le départ | 80% d'automatisation fonctionnelle est un succès |
| Concevoir un dashboard parfait avant d'avoir des données | Perte de temps, données insuffisantes |
| Ajouter des questions "au cas où" dans les formulaires | Questionnaires trop longs = abandon |
| Utiliser plus de 5 outils différents | Complexité de maintenance |

---

## Quand passer à la V2 et envisager l'intégration ?

**Critères pour passer à la V2 :**

1. Le MVP tourne en autonomie depuis ≥ 2 mois sans intervention hebdomadaire
2. Tu as ≥ 10 sessions enregistrées avec des réponses à froid
3. Tu as identifié au moins 3 insights actionnables grâce aux données
4. Les 4 axes sont représentés avec au moins 2 sessions chacun
5. Le taux de réponse à froid est ≥ 40%
6. Le dashboard te donne envie de le consulter chaque semaine

**À ce stade, tu peux envisager :**
- Connecter le système à ton outil existant (via API ou export)
- Remplacer Looker Studio par un dashboard plus avancé (Metabase, Retool)
- Ajouter une couche d'analyse IA (analyse automatique des verbatims)
- Automatiser les relances en multi-séquences
- Créer des rapports PDF automatiques par session

---

## Recommandation finale : par où commencer demain matin

**En 3 étapes prioritaires, dans cet ordre :**

### Priorité 1 — Créer la base Airtable (Jour 1, 3h)
C'est le cœur du système. Sans ça, rien ne fonctionne.
Copier exactement le modèle de données de ce document. Ne rien inventer.

### Priorité 2 — Créer le questionnaire à chaud sur Tally (Jour 2, 2h)
C'est le questionnaire le plus immédiatement utile et le plus simple à déployer.
Tu peux l'envoyer manuellement par lien lors de tes 3 prochaines sessions pendant que le reste du système se met en place.

### Priorité 3 — Configurer Make pour enregistrer les réponses Tally dans Airtable (Jour 3, 2h)
C'est le scénario 11. Sans lui, les données arrivent dans Tally mais n'alimentent pas le dashboard.

> **Le reste (automatisations d'envoi, dashboard, emails) peut attendre la semaine 2.**
> L'important est d'avoir de vraies données qui entrent dans Airtable dès la première semaine.

---

## Résumé visuel du plan

```
SEMAINE 1                    SEMAINE 2                    SEMAINES 3-4
─────────────────────────    ─────────────────────────    ──────────────────────
J1 : Créer Airtable          J6 : 1ère vraie session      Déployer sur 5 sessions
J2-3 : Formulaires Tally     J7-8 : Dashboard Looker      Ajuster les formulaires
J3-4 : Make + Brevo          J8-9 : Scénarios restants    Bilan à 30 jours
J5 : Test bout en bout       J10 : Doc interne            Décision V1.1
```

---

## Liste de contrôle finale avant déploiement

- [ ] Toutes les tables Airtable créées et reliées
- [ ] Champs cachés Tally configurés (session_id, participant_id, axe)
- [ ] Webhook Tally → Make configuré et testé
- [ ] Make → Airtable : mapping des champs validé
- [ ] Brevo : templates créés avec variables dynamiques
- [ ] Test bout en bout avec une session fictive (ta propre adresse email)
- [ ] Alerte interne (note ≤ 5) testée et fonctionnelle
- [ ] Invitation avis (note ≥ 8) testée et fonctionnelle
- [ ] Checklist opérationnelle "créer une session" rédigée
- [ ] Première vraie session enregistrée dans Airtable
