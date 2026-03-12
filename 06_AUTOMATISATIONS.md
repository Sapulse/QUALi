# QUALi — Automatisations Intelligentes

---

## PARTIE 7 — AUTOMATISATIONS INTELLIGENTES

### Vue d'ensemble des scénarios Make.com

| # | Scénario | Déclencheur | Outils impliqués |
|---|----------|-------------|-----------------|
| 1 | Envoi questionnaire positionnement | Session confirmée dans Airtable | Airtable → Make → Brevo |
| 2 | Relance questionnaire positionnement | J-2, statut = Envoyé (non Répondu) | Make → Brevo |
| 3 | Envoi questionnaire satisfaction à chaud | Date fin session atteinte | Airtable → Make → Brevo |
| 4 | Relance questionnaire à chaud | J+3, statut = Envoyé | Make → Brevo |
| 5 | Invitation avis public (post-chaud) | Note chaud ≥ 8/10 | Make → Brevo |
| 6 | Alerte interne note faible (chaud) | Note chaud ≤ 5/10 | Make → Email/Slack |
| 7 | Envoi questionnaire impact à froid | Délai selon axe atteint | Airtable → Make → Brevo |
| 8 | Relance questionnaire à froid | J+5 après envoi froid, statut = Envoyé | Make → Brevo |
| 9 | Invitation avis public (post-froid) | Note froid ≥ 8/10 et invitation non envoyée | Make → Brevo |
| 10 | Alerte interne note faible (froid) | Note froid ≤ 5/10 | Make → Email/Slack |
| 11 | Enregistrement réponses Tally → Airtable | Soumission formulaire Tally | Tally webhook → Make → Airtable |
| 12 | Calcul indicateurs session | Toutes réponses froid reçues | Make → Airtable indicateurs_calcules |

---

## SCÉNARIO 1 — Envoi questionnaire de positionnement

### Déclencheur
Airtable (watcher) : nouvelle ligne dans table `sessions` avec `statut = "Confirmée"`

### Conditions
- `statut` = "Confirmée"
- `date_session` > aujourd'hui + 2 jours
- `nb_participants` > 0
- Au moins un email participant renseigné

### Actions
1. Récupérer la liste des participants liés à cette session
2. Pour chaque participant :
   - Générer l'URL personnalisée :
     ```
     https://tally.so/r/[form-id]?session_id={{id_session}}&participant_id={{id_participant}}&axe={{id_axe}}
     ```
   - Envoyer un email via Brevo avec le template "positionnement_avant"
   - Mettre à jour `statut_questionnaire_avant` → "Envoyé"
   - Enregistrer la date d'envoi

### Règle métier
- **Ne pas envoyer si** `date_session` - aujourd'hui < 1 jour (session imminente)
- **Envoyer à J-5** si possible, sinon envoyer immédiatement si J-3 ou moins

### Template email — Positionnement avant
```
Objet : [Prénom], votre formation approche — 3 minutes pour la personnaliser

Bonjour [Prénom],

Votre formation "[Nom axe]" est prévue le [Date session].
Avant de nous retrouver, j'aimerais mieux comprendre vos attentes et votre niveau de départ.

Ce questionnaire prend 3 à 5 minutes. Il me permettra d'adapter le contenu à vos besoins.

👉 Répondre au questionnaire : [LIEN PERSONNALISÉ]

À très vite,
César Robitzer
```

---

## SCÉNARIO 2 — Relance questionnaire de positionnement

### Déclencheur
Planificateur Make (déclenché chaque matin à 9h00)

### Conditions
- `statut_questionnaire_avant` = "Envoyé"
- `date_envoi_questionnaire_avant` = aujourd'hui - 3 jours
- `date_session` = aujourd'hui + 2 jours (fenêtre de relance pertinente)

### Actions
1. Récupérer les participants correspondants
2. Envoyer email de relance avec template "relance_avant"
3. Mettre à jour `statut_questionnaire_avant` → "Relancé"

### Template email — Relance avant
```
Objet : Dernier rappel — questionnaire avant votre formation [Nom axe]

Bonjour [Prénom],

Votre formation "[Nom axe]" approche ! Je n'ai pas encore reçu votre questionnaire de positionnement.

Cela prend 3 minutes et m'aide vraiment à préparer une session adaptée à vos besoins.

👉 [LIEN PERSONNALISÉ]

À bientôt,
César
```

> **Ne jamais relancer plus d'une fois** sur le questionnaire avant.

---

## SCÉNARIO 3 — Envoi questionnaire satisfaction à chaud

### Déclencheur
Planificateur Make (déclenché chaque soir à 18h30)

### Conditions
- `date_fin_session` = aujourd'hui
- `statut` = "Réalisée"
- `statut_questionnaire_chaud` = "À envoyer"

> *Le formateur met à jour `statut = "Réalisée"` dans Airtable à la fin de la session. C'est le signal.*

### Actions
1. Récupérer les participants de la session
2. Générer les URLs personnalisées (questionnaire à chaud)
3. Envoyer l'email via Brevo avec template "satisfaction_chaud"
4. Mettre à jour `statut_questionnaire_chaud` → "Envoyé"

### Template email — Satisfaction à chaud
```
Objet : [Prénom], comment s'est passée votre journée ? (4 minutes)

Bonjour [Prénom],

Merci d'avoir participé à la formation "[Nom axe]" aujourd'hui.

J'aimerais avoir votre retour pendant que les impressions sont fraîches.
Cela prend 4 à 5 minutes et m'aide directement à améliorer mes formations.

👉 Donner mon avis : [LIEN PERSONNALISÉ]

Merci et bonne soirée,
César
```

---

## SCÉNARIO 4 — Relance questionnaire à chaud

### Déclencheur
Planificateur Make (chaque matin à 9h00)

### Conditions
- `statut_questionnaire_chaud` = "Envoyé"
- `date_envoi_questionnaire_chaud` = aujourd'hui - 3 jours

### Actions
1. Envoyer email de relance courte
2. Mettre à jour statut → "Relancé"

### Template email — Relance à chaud
```
Objet : Votre avis sur la formation [Nom axe] — il n'est pas trop tard

Bonjour [Prénom],

Votre retour sur la formation "[Nom axe]" m'est précieux.
Si vous avez 4 minutes, je suis preneur !

👉 [LIEN PERSONNALISÉ]

Merci beaucoup,
César
```

---

## SCÉNARIO 5 — Invitation avis public (post-chaud)

### Déclencheur
Soumission du questionnaire à chaud (webhook Tally → Make)

### Conditions
- `note_satisfaction_chaud` ≥ 8
- `consentement_recontact` = Oui
- `invitation_envoyee` = Non (dans table statut_avis)

### Délai
Attendre 48h avant d'envoyer (laisser le temps à la satisfaction de "poser")

### Actions
1. Créer une ligne dans `statut_avis` avec `invitation_envoyee = Oui`
2. Envoyer email via Brevo avec template "invitation_avis"
3. Marquer `date_invitation`

### Template email — Invitation avis (chaud)
```
Objet : Votre avis peut aider d'autres professionnels

Bonjour [Prénom],

Merci pour votre retour très positif sur la formation "[Nom axe]".
Ça me fait vraiment plaisir de savoir que ça vous a apporté quelque chose.

Si vous avez 2 minutes, un avis public nous aiderait énormément à développer ces accompagnements et à toucher d'autres professionnels comme vous.

Même une phrase suffit :

👉 Laisser un avis Google : [LIEN GOOGLE]
👉 Témoigner sur LinkedIn : [LIEN LINKEDIN]

Merci d'avance,
César
```

> **Règle** : Ne jamais insister. Un seul envoi, pas de relance sur l'invitation d'avis.

---

## SCÉNARIO 6 — Alerte interne : note faible (chaud)

### Déclencheur
Soumission questionnaire à chaud avec `note_satisfaction_chaud` ≤ 5

### Actions
1. Envoyer un email d'alerte interne à cesar@[domaine] avec :
   - Nom du participant
   - Session concernée
   - Note obtenue
   - Verbatim "point d'amélioration" s'il existe
2. Optionnel : notification Slack si configuré
3. Créer une tâche de suivi dans Airtable : "Prise de contact à faire — J+2"

### Template alerte interne
```
Objet : ⚠️ Alerte satisfaction — [Prénom Nom] | Session [Nom axe] | Note : [X]/10

Un participant a attribué une note faible à sa session.

Participant : [Prénom Nom]
Session : [Nom axe] — [Date]
Note : [X]/10
Point d'amélioration mentionné : [Verbatim ou "Non renseigné"]

👉 Action recommandée : prise de contact dans les 48h
```

---

## SCÉNARIO 7 — Envoi questionnaire impact à froid

### Déclencheur
Planificateur Make (chaque matin à 9h00)

### Conditions
- `date_envoi_questionnaire_froid` = aujourd'hui
- `statut_questionnaire_froid` = "À envoyer"
- `statut_questionnaire_chaud` = "Répondu" (si non répondu, envoyer quand même mais noter)

### Délai par axe (configuré dans table `axes`)
| Axe | Délai | Date envoi calculée |
|-----|-------|---------------------|
| Déclic | 15 jours | date_fin_session + 15 |
| Activation | 21 jours | date_fin_session + 21 |
| Transformation | 30 jours | date_fin_session + 30 |
| Systèmes | 45 jours | date_fin_session + 45 |

### Actions
1. Générer URLs personnalisées (questionnaire froid)
2. Envoyer email via Brevo avec template "impact_froid"
3. Mettre à jour `statut_questionnaire_froid` → "Envoyé"

### Template email — Impact à froid
```
Objet : [Prénom], qu'est-ce que la formation a changé pour vous ?

Bonjour [Prénom],

Votre formation "[Nom axe]" date maintenant de [N] semaines.
J'aimerais savoir ce que ça a changé concrètement dans votre activité.

Ce retour terrain est précieux : il m'aide à mesurer l'impact réel de mes accompagnements
et à continuer de les améliorer.

👉 Partager mon retour terrain (5 minutes) : [LIEN PERSONNALISÉ]

Merci pour votre temps,
César
```

---

## SCÉNARIO 8 — Relance questionnaire à froid

### Déclencheur
Planificateur Make (chaque matin à 9h00)

### Conditions
- `statut_questionnaire_froid` = "Envoyé"
- `date_envoi_questionnaire_froid` = aujourd'hui - 5 jours

### Actions
1. Envoyer version courte du questionnaire (seulement 3 questions clés si possible)
   OU envoyer relance avec le même lien
2. Mettre à jour statut → "Relancé"

### Template email — Relance à froid (version courte)
```
Objet : 2 questions rapides sur ce que vous avez mis en place

Bonjour [Prénom],

Je sais que vous êtes occupé(e). Alors, juste 2 questions :
1. Avez-vous mis en pratique ce que vous avez appris ?
2. Qu'est-ce que ça a changé concrètement ?

👉 Répondre en 2 minutes : [LIEN PERSONNALISÉ]

Merci, c'est précieux,
César
```

---

## SCÉNARIO 9 — Invitation avis public (post-froid)

### Déclencheur
Soumission questionnaire à froid avec `note_satisfaction_froid` ≥ 8

### Conditions supplémentaires
- `consentement_recontact` = Oui
- `invitation_envoyee` = Non (dans statut_avis — pas encore invité)

### Actions
Identiques au Scénario 5 (invitation avis public post-chaud)
Le message peut légèrement différer : mentionner qu'ils ont "confirmé leur satisfaction après plusieurs semaines"

---

## SCÉNARIO 10 — Alerte interne : note faible (froid)

### Conditions
- `note_satisfaction_froid` ≤ 5

### Actions
Identiques au Scénario 6, mais avec contexte "à froid" — signal plus préoccupant car c'est la satisfaction durable.

---

## SCÉNARIO 11 — Enregistrement des réponses Tally → Airtable

### Déclencheur
Webhook Tally : soumission d'un des 3 formulaires

### Logique de routage
Make reçoit les données Tally et identifie quel formulaire a été soumis via le `form_id`.

```
Si form_id = [id_formulaire_avant] → insérer dans reponses_avant
Si form_id = [id_formulaire_chaud] → insérer dans reponses_chaud
Si form_id = [id_formulaire_froid] → insérer dans reponses_froid
```

### Pour chaque réponse reçue
1. Extraire `session_id` et `participant_id` depuis les champs cachés
2. Mapper les réponses Tally vers les champs Airtable correspondants
3. Insérer la ligne dans la bonne table
4. Mettre à jour le `statut_questionnaire_XXX` du participant → "Répondu"
5. Déclencher les scénarios conditionnels (5, 6, 9, 10) si conditions remplies

---

## SCÉNARIO 12 — Calcul des indicateurs de session

### Déclencheur
Mise à jour d'une ligne dans `reponses_froid` (toutes les réponses reçues)
OU planificateur hebdomadaire (chaque lundi matin)

### Calculs effectués par axe/session
```
taux_reponse_avant = (nb_repondu_avant / nb_participants) × 100
taux_reponse_chaud = (nb_repondu_chaud / nb_participants) × 100
taux_reponse_froid = (nb_repondu_froid / nb_participants) × 100
note_satisfaction_chaud_moy = MOYENNE(toutes notes chaud de la session)
note_satisfaction_froid_moy = MOYENNE(toutes notes froid de la session)
taux_recommandation = % participants (Oui certainement + Probablement oui) / total répondants froid
taux_application = % (Régulièrement + Quelques fois) / total répondants froid
taux_gain_temps = % (Oui clairement + Oui un peu) / total répondants froid
score_impact = (note_froid_moy/10 × 0.4) + (taux_application × 0.3) + (taux_gain_temps × 0.2) + (taux_recommandation × 0.1)
```

### Action finale
Écrire ces valeurs dans la table `indicateurs_calcules` pour la session concernée.

---

## Récapitulatif des règles métier critiques

| Règle | Description |
|-------|-------------|
| Invitation avis = 1 seule fois | Vérifier `invitation_envoyee` avant tout envoi |
| Relance avant = 1 seule relance | Ne pas harceler avant la formation |
| Relance chaud = 1 seule relance | 1 relance à J+3 maximum |
| Relance froid = 1 seule relance | Version courte recommandée |
| Alerte interne ≤ 5/10 | Déclencher pour chaud ET froid |
| Consentement obligatoire | Ne jamais envoyer si consentement_recontact = Non |
| Statut "Réalisée" = clé | Le formateur doit mettre à jour manuellement |

---

## Matrice des déclencheurs et conditions

```
EVENT                    CONDITION                 ACTION
─────────────────────────────────────────────────────────────────
Session confirmée        statut=Confirmée          → Envoi avant (J-5)
J-2 sans réponse avant   statut=Envoyé + J-2       → Relance avant
Session réalisée         statut=Réalisée + soir J  → Envoi chaud
J+3 sans réponse chaud   statut=Envoyé + J+3       → Relance chaud
Note chaud ≥ 8           consentement=Oui          → Invitation avis (+48h)
Note chaud ≤ 5           —                         → Alerte interne
Délai axe atteint        statut froid=À envoyer    → Envoi froid
J+5 sans réponse froid   statut=Envoyé + J+5       → Relance froid courte
Note froid ≥ 8           inv. non envoyée          → Invitation avis
Note froid ≤ 5           —                         → Alerte interne
Réponse froid reçue      toutes réponses in        → Calcul indicateurs
```
