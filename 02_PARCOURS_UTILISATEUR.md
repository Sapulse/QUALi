# QUALi — Parcours Utilisateur Complet

---

## PARTIE 3 — PARCOURS UTILISATEUR COMPLET

### Vue d'ensemble du parcours

```
INSCRIPTION / CONFIRMATION
         │
    [J-5 à J-2]
         │
         ▼
┌─────────────────────┐
│  EMAIL #1           │
│  Questionnaire de   │
│  positionnement     │
│  (avant formation)  │
└──────────┬──────────┘
           │ Non-réponse à J-2
           ▼
┌─────────────────────┐
│  RELANCE #1         │
│  Email de rappel    │
│  "Dernier rappel"   │
└──────────┬──────────┘
           │
    [Jour J]
    La formation a lieu
           │
    [J+0 à J+1]
           ▼
┌─────────────────────┐
│  EMAIL #2           │
│  Questionnaire de   │
│  satisfaction       │
│  à chaud            │
└──────────┬──────────┘
           │ Non-réponse à J+3
           ▼
┌─────────────────────┐
│  RELANCE #2         │
│  "Votre avis compte"│
└──────────┬──────────┘
           │ Note ≥ 8/10 → EMAIL AVIS
           │
    [J+15 ou J+30 selon axe]
           ▼
┌─────────────────────┐
│  EMAIL #3           │
│  Questionnaire      │
│  d'impact à froid   │
└──────────┬──────────┘
           │ Non-réponse à J+5
           ▼
┌─────────────────────┐
│  RELANCE #3         │
│  "2 minutes pour    │
│  partager votre     │
│  retour terrain"    │
└──────────┬──────────┘
           │ Note ≥ 8/10 → EMAIL AVIS (si pas encore fait)
           │ Note < 6/10 → ALERTE INTERNE
           ▼
        FIN DU PARCOURS
```

---

### Détail complet par étape

---

#### ÉTAPE 0 — Déclencheur : Confirmation de session

**Qui** : Toi (le formateur) ou ton assistante
**Quand** : Dès qu'une session est confirmée (client signé, date posée)
**Action** : Créer une ligne dans la table "Sessions" d'Airtable avec :
- Nom du client / de l'entreprise
- Email(s) du/des participant(s)
- Axe (Déclic / Activation / Transformation / Systèmes)
- Date de la session
- Formateur
- Statut = "Confirmée"

> Ce seul enregistrement déclenche toute la séquence automatisée via Make.com.

---

#### ÉTAPE 1 — Questionnaire de positionnement (avant formation)

| Paramètre | Valeur |
|-----------|--------|
| **Qui reçoit** | Chaque participant inscrit à la session |
| **Quand** | J-5 avant la formation (ou J-2 si session courte) |
| **Canal** | Email personnalisé (Brevo) |
| **Lien** | Tally.so avec ?session_id=XXX&participant_id=YYY en paramètre URL |
| **Durée estimée** | 3 à 5 minutes |
| **Relance** | J-2 si non-réponse |
| **Obligatoire** | Oui (pour Qualiopi) |

**Objet de l'email :**
> *"[Prénom], votre formation approche — 3 minutes pour nous aider à la personnaliser"*

**Corps de l'email :**
> Bonjour [Prénom],
>
> Votre formation [Nom de l'axe] est prévue le [Date]. Avant de nous retrouver, j'aimerais mieux comprendre où vous en êtes et ce que vous souhaitez en retirer.
>
> Ce questionnaire prend 3 à 5 minutes. Vos réponses me permettront d'adapter le contenu à vos besoins réels.
>
> 👉 [Répondre au questionnaire]
>
> À très vite,
> César

**Relance J-2 :**
> Objet : *"Dernier rappel avant votre formation — questionnaire de positionnement"*

**Cas particuliers à anticiper :**
- Participant inscrit tardivement (J-1 ou J) : envoyer le questionnaire mais ne pas bloquer la session
- Groupe de plusieurs participants : envoyer un email individuel à chaque adresse
- Client B2B avec un seul contact RH qui répond pour l'équipe : prévoir une version "dirigeant répond pour son équipe" (voir question profil)

---

#### ÉTAPE 2 — Questionnaire de satisfaction à chaud

| Paramètre | Valeur |
|-----------|--------|
| **Qui reçoit** | Chaque participant ayant suivi la formation |
| **Quand** | J+0 (envoi en fin de journée ou le soir) ou J+1 matin |
| **Canal** | Email personnalisé |
| **Durée estimée** | 4 à 6 minutes |
| **Relance** | J+3 si non-réponse |
| **Logique conditionnelle** | Si note ≥ 8/10 → envoi automatique d'un email avec lien d'avis public |

**Objet de l'email :**
> *"[Prénom], comment s'est passée votre formation ? (2 minutes)"*

**Logique après soumission :**
- Note globale ≥ 8/10 : envoi automatique d'un email "Votre avis nous aiderait énormément" avec lien Google / Trustpilot / Page LinkedIn
- Note globale ≤ 5/10 : alerte interne (email ou notification Slack) pour prise de contact rapide
- Note entre 6 et 7/10 : aucune action immédiate, suivi classique

**Cas particuliers :**
- Formation sur 2 jours (axe Transformation) : envoyer le questionnaire à chaud à la fin du dernier jour, pas après le premier
- Participant absent une partie de la formation : prévoir une note dans Airtable "présence partielle" pour pondérer l'analyse

---

#### ÉTAPE 3 — Questionnaire d'impact à froid

| Axe | Délai recommandé | Justification |
|-----|-----------------|---------------|
| Déclic | J+15 | Formation courte, impact visible rapidement |
| Activation | J+21 | Premiers usages attendus à 3 semaines |
| Transformation | J+30 | Changements de pratiques nécessitent du temps |
| Systèmes | J+45 | Outils à mettre en place, délai plus long |

| Paramètre | Valeur |
|-----------|--------|
| **Qui reçoit** | Chaque participant ayant répondu au questionnaire à chaud |
| **Canal** | Email personnalisé |
| **Durée estimée** | 5 à 7 minutes |
| **Relance** | J+5 après envoi si non-réponse |
| **Logique conditionnelle** | Si note ≥ 8/10 et pas encore laissé d'avis → relance invitation avis |

**Objet de l'email :**
> *"[Prénom], qu'est-ce que la formation a changé pour vous ?"*

**Corps de l'email :**
> Bonjour [Prénom],
>
> Votre formation [Nom de l'axe] a eu lieu il y a quelques semaines. J'aimerais savoir ce que ça a changé concrètement pour vous.
>
> Ce retour terrain est précieux : il m'aide à améliorer mes accompagnements et à prouver leur impact réel.
>
> 👉 [Partager mon retour terrain — 5 minutes]
>
> Merci pour votre temps,
> César

**Relance J+5 :**
> Objet : *"Juste 2 questions sur ce que vous avez mis en place"*
> (version ultra-courte avec 2-3 questions seulement en cas de non-réponse)

---

#### ÉTAPE 4 — Redirection vers un avis public (conditionnel)

**Déclencheur :** Note de satisfaction (chaud ou froid) ≥ 8/10

**Email envoyé :**

> Objet : *"Votre avis peut aider d'autres professionnels comme vous"*
>
> Bonjour [Prénom],
>
> Vous avez partagé un retour très positif sur votre formation — merci sincèrement.
>
> Si vous avez 2 minutes, un avis public serait une aide précieuse pour que d'autres professionnels puissent découvrir ces accompagnements.
>
> 👉 [Laisser un avis Google]
> 👉 [Témoigner sur LinkedIn]
>
> Même une phrase suffit. Merci d'avance !
> César

**Règles importantes :**
- Ne jamais envoyer cet email si le participant n'a pas explicitement consenti à être recontacté
- Envoyer cet email une seule fois (ne pas relancer)
- Marquer dans Airtable : "Invitation avis envoyée = Oui" pour éviter les doublons

---

### Résumé du calendrier par session

```
J-5   → Email questionnaire positionnement
J-2   → Relance positionnement (si non-réponse)
J     → Formation
J+0   → Email questionnaire satisfaction à chaud
J+1   → Alternative si envoi le matin suivant
J+3   → Relance à chaud (si non-réponse)
J+3   → Email invitation avis (si note ≥ 8)
J+15 à J+45 → Email questionnaire impact à froid (selon axe)
J+20 à J+50 → Relance à froid (si non-réponse)
J+20 à J+50 → Email invitation avis si pas encore fait (si note ≥ 8)
```
