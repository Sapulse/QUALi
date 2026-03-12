# QUALi — Système de Suivi des Formations
## Vision Produit & Architecture Fonctionnelle MVP

---

## PARTIE 1 — VISION PRODUIT

### Pourquoi séparer avant / à chaud / à froid ?

La plupart des organismes de formation commettent la même erreur : ils ne collectent qu'un seul retour, juste après la formation, quand les participants sont encore dans l'émotion du moment. Ce retour est biaisé, incomplet, et surtout inutilisable pour mesurer l'impact réel.

**La logique des 3 temps répond à 3 questions fondamentalement différentes :**

| Moment       | Question posée                              | Ce que ça mesure                            |
|--------------|---------------------------------------------|---------------------------------------------|
| **Avant**    | Où en es-tu et qu'est-ce que tu cherches ?  | Niveau de départ, attentes, besoins réels   |
| **À chaud**  | Est-ce que ça t'a plu et apporté quelque chose ? | Satisfaction immédiate, qualité perçue  |
| **À froid**  | Est-ce que tu l'as mis en pratique ?        | Impact réel, ROI perçu, changements durables |

Sans le "avant", tu ne peux pas mesurer la progression.
Sans le "à froid", tu ne mesures que le plaisir sur le moment, pas la valeur réelle.
Ce sont trois instruments distincts, chacun irremplaçable.

### Pourquoi commencer en version autonome ?

Trois raisons concrètes :

1. **Zéro risque** : tu ne touches à rien dans ton outil existant. Si quelque chose ne fonctionne pas comme prévu, tu corriges sans impact.
2. **Vitesse de test** : tu peux déployer, tester, ajuster en quelques jours, pas en plusieurs semaines.
3. **Clarté d'apprentissage** : tu comprends ce qui fonctionne vraiment avant d'investir dans une intégration plus lourde.

Un MVP autonome, c'est un laboratoire. Pas un chantier permanent.

### Comment penser le parcours de collecte ?

Le parcours doit être pensé comme une séquence automatisée, déclenchée par un événement : l'inscription ou la confirmation d'une session.

```
Inscription confirmée
       │
       ▼
[J-2 à J-5] Questionnaire de positionnement
       │
       ▼
[Jour J] La formation se déroule
       │
       ▼
[J+0 à J+1] Questionnaire de satisfaction à chaud
       │
       ▼
[J+15 à J+45] Questionnaire d'impact à froid
       │
       ▼
[Conditionnel] Redirection vers avis public si note ≥ 8/10
```

Chaque étape doit être simple, rapide à remplir, et apporter une valeur perçue au participant (sentiment d'être écouté, suivi, accompagné).

### Bénéfices pour un organisme de formation

- **Qualiopi / conformité** : traces écrites des besoins pré-formation et de la mesure des acquis.
- **Amélioration continue** : données actionnables pour faire évoluer les contenus.
- **Argumentaire commercial** : verbatims, notes moyennes, gains de temps déclarés = preuves sociales.
- **Pilotage par les 4 axes** : savoir quel axe (Déclic / Activation / Transformation / Systèmes) produit le plus de valeur.
- **Détection des signaux faibles** : un participant insatisfait détecté à froid, c'est une relation sauvée avant qu'elle ne devienne un problème public.

---

## PARTIE 2 — ARCHITECTURE FONCTIONNELLE MVP

### Stack recommandée (pragmatique, sans surcharge)

| Module            | Outil recommandé       | Rôle                                              | Coût approximatif |
|-------------------|------------------------|---------------------------------------------------|-------------------|
| Formulaires       | **Tally.so**           | Collecte des réponses, logique conditionnelle     | Gratuit (ou 29€/mois Pro) |
| Base de données   | **Airtable**           | Stockage, relations entre tables, vues filtrées   | Gratuit jusqu'à 1 200 lignes/table |
| Automatisations   | **Make.com**           | Déclenchements, envois d'emails, relances         | Gratuit (1 000 ops/mois) ou 9€/mois |
| Dashboard         | **Looker Studio**      | Visualisation dynamique, graphiques, KPIs         | Gratuit (Google) |
| Emails            | **Brevo (ex-Sendinblue)** | Envoi d'emails transactionnels avec variables  | Gratuit jusqu'à 300 emails/jour |

> **Hypothèse** : tu gères moins de 50 sessions/an et moins de 10 participants par session en moyenne. Si ce n'est pas le cas, adapte les quotas ou passe sur les tiers payants, qui restent très accessibles.

### Les 5 modules et leur rôle

```
┌─────────────────────────────────────────────────────────────────┐
│  MODULE 1 : DÉCLENCHEUR                                         │
│  Airtable — Table "Sessions"                                    │
│  → Enregistrement d'une nouvelle session = événement déclencheur│
└───────────────────────┬─────────────────────────────────────────┘
                        │
                        ▼
┌─────────────────────────────────────────────────────────────────┐
│  MODULE 2 : COLLECTE                                            │
│  Tally.so — 3 formulaires distincts                             │
│  → Positionnement (avant) / Satisfaction (chaud) / Impact (froid)│
│  → Chaque formulaire reçoit un paramètre ?session_id=XXX        │
└───────────────────────┬─────────────────────────────────────────┘
                        │
                        ▼
┌─────────────────────────────────────────────────────────────────┐
│  MODULE 3 : STOCKAGE                                            │
│  Airtable — Tables reliées                                      │
│  → Toutes les réponses atterrissent dans Airtable via webhook   │
│  → Reliées automatiquement à la session, l'axe, le participant  │
└───────────────────────┬─────────────────────────────────────────┘
                        │
                        ▼
┌─────────────────────────────────────────────────────────────────┐
│  MODULE 4 : AUTOMATISATIONS                                     │
│  Make.com — Scénarios                                           │
│  → Envoi des liens formulaires au bon moment                    │
│  → Relances si non-réponse à J+3                                │
│  → Alerte interne si note < 6/10                                │
│  → Invitation avis public si note ≥ 8/10                        │
└───────────────────────┬─────────────────────────────────────────┘
                        │
                        ▼
┌─────────────────────────────────────────────────────────────────┐
│  MODULE 5 : DASHBOARD                                           │
│  Looker Studio connecté à Airtable (via Google Sheets export)   │
│  → KPIs par axe, par période, satisfaction, impact, verbatims   │
└─────────────────────────────────────────────────────────────────┘
```

### Communication entre modules

1. **Airtable → Make.com** : un watcher Make surveille les nouvelles lignes dans la table Sessions. Dès qu'une session est créée avec statut "Confirmée", Make déclenche la séquence.
2. **Make.com → Brevo** : Make envoie les emails avec les liens Tally personnalisés (session_id en paramètre URL).
3. **Tally.so → Make.com (webhook)** : chaque soumission de formulaire envoie les données vers Make via webhook.
4. **Make.com → Airtable** : Make reçoit les réponses et les insère dans les bonnes tables Airtable.
5. **Airtable → Google Sheets** : export automatique ou synchronisation via Make vers Google Sheets.
6. **Google Sheets → Looker Studio** : source de données native, actualisée automatiquement.

### Niveau de complexité recommandé pour la V1

| Élément                      | V1 (MVP)                     | V2 (évolution)               |
|------------------------------|------------------------------|------------------------------|
| Formulaires                  | 3 formulaires Tally          | Formulaires dynamiques multi-axes |
| Stockage                     | Airtable simple              | Base de données dédiée (Supabase) |
| Automatisations              | Make.com basique             | Make.com avancé + logique métier |
| Dashboard                    | Looker Studio                | Dashboard custom (Metabase/Retool) |
| Relances                     | 1 relance par questionnaire  | Séquences multi-relances     |
| Avis publics                 | Lien manuel dans l'email     | Redirection automatisée      |

**En V1, la règle d'or : si ça prend plus de 2 semaines à mettre en place, c'est trop complexe.**
