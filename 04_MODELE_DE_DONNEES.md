# QUALi — Modèle de Données

---

## PARTIE 5 — MODÈLE DE DONNÉES À ENREGISTRER

### Vue d'ensemble des tables

```
┌─────────────┐     ┌─────────────┐     ┌─────────────────┐
│  entreprises │◄────│  clients    │◄────│  participants   │
└─────────────┘     └─────────────┘     └────────┬────────┘
                                                  │
                    ┌─────────────┐               │
                    │  axes       │               │
                    └──────┬──────┘               │
                           │                      │
                    ┌──────▼──────┐               │
                    │  sessions   │◄──────────────┘
                    └──────┬──────┘
                           │
               ┌───────────┼────────────┐
               ▼           ▼            ▼
        ┌──────────┐ ┌──────────┐ ┌──────────┐
        │ rep_avant│ │ rep_chaud│ │ rep_froid│
        └──────────┘ └──────────┘ └──────────┘
               │           │            │
               └───────────┼────────────┘
                           ▼
                  ┌──────────────────┐
                  │ statut_avis      │
                  └──────────────────┘
                           │
                           ▼
                  ┌──────────────────┐
                  │ indicateurs_calc │
                  └──────────────────┘
```

---

### TABLE 1 — `entreprises`

| Champ | Type | Obligatoire | Description |
|-------|------|-------------|-------------|
| `id_entreprise` | ID unique (ex: ENT-001) | Oui | Identifiant interne |
| `nom_entreprise` | Texte | Oui | Raison sociale |
| `secteur_activite` | Texte / Liste | Non | Secteur d'activité |
| `taille_entreprise` | Choix : 1 / 2-10 / 11-50 / 50+ | Non | Taille de l'entreprise |
| `ville` | Texte | Non | Ville principale |
| `date_premier_contact` | Date | Non | Pour calculer l'ancienneté client |
| `notes` | Texte long | Non | Notes libres |

**Relations :** 1 entreprise → N clients

---

### TABLE 2 — `clients`

> *Le client est le commanditaire (celui qui achète). Le participant est celui qui suit la formation.*

| Champ | Type | Obligatoire | Description |
|-------|------|-------------|-------------|
| `id_client` | ID unique (ex: CLI-001) | Oui | Identifiant interne |
| `id_entreprise` | Référence → entreprises | Non | Lien vers l'entreprise |
| `prenom` | Texte | Oui | Prénom |
| `nom` | Texte | Oui | Nom |
| `email` | Email | Oui | Email principal |
| `telephone` | Texte | Non | Téléphone |
| `fonction` | Texte | Non | Poste occupé |
| `profil` | Choix : Dirigeant / Salarié / Indépendant / RH / Autre | Non | |
| `date_creation` | Date | Automatique | Date d'ajout au système |
| `consentement_recontact` | Booléen | Oui | A accepté d'être recontacté |

**Relations :** 1 client → N participants / N sessions

---

### TABLE 3 — `participants`

> *Un participant peut être différent du client (ex: le dirigeant achète pour son équipe).*

| Champ | Type | Obligatoire | Description |
|-------|------|-------------|-------------|
| `id_participant` | ID unique (ex: PAR-001) | Oui | Identifiant interne |
| `id_client` | Référence → clients | Oui | Client rattaché |
| `prenom` | Texte | Oui | Prénom du participant |
| `nom` | Texte | Oui | Nom du participant |
| `email` | Email | Oui | Email pour les envois |
| `fonction` | Texte | Non | Poste du participant |
| `consentement_recontact` | Booléen | Oui | Consentement RGPD |
| `statut_questionnaire_avant` | Choix : À envoyer / Envoyé / Répondu / Relancé | Automatique | |
| `statut_questionnaire_chaud` | Choix : À envoyer / Envoyé / Répondu / Relancé | Automatique | |
| `statut_questionnaire_froid` | Choix : À envoyer / Envoyé / Répondu / Relancé | Automatique | |

**Relations :** 1 participant → N sessions (possible) / 3 tables de réponses

---

### TABLE 4 — `axes`

| Champ | Type | Obligatoire | Description |
|-------|------|-------------|-------------|
| `id_axe` | ID unique | Oui | |
| `nom_axe` | Choix : Déclic / Activation / Transformation / Systèmes | Oui | |
| `description` | Texte | Non | Description de l'axe |
| `duree_typique` | Texte | Non | Ex : "1 journée", "2-3 jours" |
| `delai_questionnaire_froid` | Nombre (jours) | Oui | 15 / 21 / 30 / 45 selon l'axe |

---

### TABLE 5 — `sessions`

> *Table centrale. Créer une ligne = déclencheur de toute la séquence.*

| Champ | Type | Obligatoire | Description |
|-------|------|-------------|-------------|
| `id_session` | ID unique (ex: SES-2025-001) | Oui | Identifiant de session |
| `id_axe` | Référence → axes | Oui | Axe concerné |
| `id_client` | Référence → clients | Oui | Client commanditaire |
| `formateur` | Choix : César Robitzer / Mickael Mège / Autre | Oui | |
| `date_session` | Date | Oui | Date (ou première date si multi-jours) |
| `date_fin_session` | Date | Non | Pour les axes multi-jours |
| `nb_participants` | Nombre | Oui | Nombre de participants |
| `statut` | Choix : Planifiée / Confirmée / Réalisée / Annulée | Oui | Statut de la session |
| `modalite` | Choix : Présentiel / Distanciel / Hybride | Oui | |
| `lieu` | Texte | Non | Lieu si présentiel |
| `notes_internes` | Texte long | Non | Notes pour le formateur |
| `date_envoi_questionnaire_avant` | Date | Automatique | Calculée = date_session - 5 jours |
| `date_envoi_questionnaire_chaud` | Date | Automatique | Calculée = date_fin_session |
| `date_envoi_questionnaire_froid` | Date | Automatique | Selon délai de l'axe |

**Relations :** 1 session → N participants → N réponses

---

### TABLE 6 — `reponses_avant`

| Champ | Type | Obligatoire | Notes |
|-------|------|-------------|-------|
| `id_reponse_avant` | ID unique | Automatique | |
| `id_session` | Référence → sessions | Oui | Clé de liaison principale |
| `id_participant` | Référence → participants | Oui | |
| `date_soumission` | Date/Heure | Automatique | |
| `profil_repondant` | Choix | Oui | Individuel / Dirigeant équipe / Groupe |
| **Niveau de départ** | | | |
| `niveau_notoriete_ia` | Choix (4 niveaux) | Oui | B1 |
| `niveau_usage_ia` | Choix (4 niveaux) | Oui | B2 |
| `niveau_aise_numerique` | Choix (4 niveaux) | Oui | B3 |
| `niveau_outils_existants` | Choix (4 niveaux) | Oui | B4 |
| **Attentes** | | | |
| `motivations` | Tableau de choix | Oui | C1 — multi-sélection |
| `objectif_principal` | Texte libre | Oui | C2 |
| `taches_a_accelerer` | Tableau de choix | Oui | C3 — multi-sélection |
| `frein_usage_ia` | Texte libre | Non | C4 |
| **Spécifique axe** | | | |
| `axe_question_1` | Variable selon axe | Non | D-xxx-1 |
| `axe_question_2` | Variable selon axe | Non | D-xxx-2 |
| `axe_question_3` | Variable selon axe | Non | D-xxx-3 |
| **Accessibilité** | | | |
| `besoin_accessibilite` | Booléen | Oui | E1 |
| `detail_accessibilite` | Texte libre | Non | E2 |
| **Consentement** | | | |
| `consentement_recontact` | Booléen | Oui | F2 |
| `commentaire_libre_avant` | Texte libre | Non | F1 |

---

### TABLE 7 — `reponses_chaud`

| Champ | Type | Obligatoire | Notes |
|-------|------|-------------|-------|
| `id_reponse_chaud` | ID unique | Automatique | |
| `id_session` | Référence → sessions | Oui | |
| `id_participant` | Référence → participants | Oui | |
| `date_soumission` | Date/Heure | Automatique | |
| **Satisfaction globale** | | | |
| `note_satisfaction_chaud` | Note 0-10 | Oui | B1 — KPI principal |
| `recommandation_nps` | Choix (5 niveaux) | Oui | B2 |
| `phrase_retenue` | Texte libre | Oui | B3 |
| **Évaluation session** | | | |
| `adequation_niveau` | Note 1-5 | Oui | C1 |
| `clarte_explications` | Note 1-5 | Oui | C2 |
| `adequation_attentes` | Note 1-5 | Oui | C3 |
| `rythme_adapte` | Note 1-5 | Oui | C4 |
| `temps_pratique_suffisant` | Note 1-5 | Oui | C5 |
| `ecoute_formateur` | Note 1-5 | Oui | C6 |
| `dynamique_groupe` | Note 1-5 | Non | C7 |
| **Apport perçu** | | | |
| `progression_percue` | Choix (4 niveaux) | Oui | D1 |
| `actions_concrets` | Texte libre | Oui | D2 |
| `points_a_approfondir` | Texte libre | Non | D3 |
| **Spécifique axe** | | | |
| `axe_question_chaud_1` | Variable | Non | E-xxx-1 |
| `axe_question_chaud_2` | Variable | Non | E-xxx-2 |
| `axe_question_chaud_3` | Variable | Non | E-xxx-3 |
| **Verbatims** | | | |
| `point_fort` | Texte libre | Non | F1 |
| `point_amelioration` | Texte libre | Non | F2 |
| `commentaire_libre_chaud` | Texte libre | Non | F3 |

---

### TABLE 8 — `reponses_froid`

| Champ | Type | Obligatoire | Notes |
|-------|------|-------------|-------|
| `id_reponse_froid` | ID unique | Automatique | |
| `id_session` | Référence → sessions | Oui | |
| `id_participant` | Référence → participants | Oui | |
| `date_soumission` | Date/Heure | Automatique | |
| `semaines_depuis_formation` | Nombre calculé | Automatique | |
| **Mise en application** | | | |
| `frequence_application` | Choix (5 niveaux) | Oui | B1 |
| `taches_appliquees` | Texte libre | Non | B2 |
| `frein_non_application` | Texte libre | Non | B3 |
| `niveau_integration_ia` | Note 0-10 | Oui | B4 |
| **Gains concrets** | | | |
| `gain_temps_declare` | Choix (4 niveaux) | Oui | C1 |
| `estimation_temps_gagne_semaine` | Choix (5 niveaux) | Non | C2 |
| `benefices_observes` | Tableau de choix | Oui | C3 — multi-sélection |
| `exemple_concret_succes` | Texte libre | Non | C4 |
| **Satisfaction durable** | | | |
| `note_satisfaction_froid` | Note 0-10 | Oui | D1 — KPI principal |
| `recommandation_retro` | Choix (5 niveaux) | Oui | D2 |
| `impact_principal_activite` | Texte libre | Non | D3 |
| **Spécifique axe** | | | |
| `axe_question_froid_1` | Variable | Non | E-xxx-1 |
| `axe_question_froid_2` | Variable | Non | E-xxx-2 |
| `axe_question_froid_3` | Variable | Non | E-xxx-3 |
| `axe_question_froid_4` | Variable | Non | E-xxx-4 (Systèmes seulement) |
| **Besoins futurs** | | | |
| `besoin_futur` | Booléen / Choix | Non | F1 |
| `domaine_besoin_futur` | Tableau de choix | Non | F2 |
| `temoignage_libre` | Texte libre | Non | F3 — VERBATIM CLÉ |

---

### TABLE 9 — `statut_avis`

| Champ | Type | Obligatoire | Description |
|-------|------|-------------|-------------|
| `id_statut_avis` | ID unique | Automatique | |
| `id_participant` | Référence → participants | Oui | |
| `id_session` | Référence → sessions | Oui | |
| `invitation_envoyee` | Booléen | Automatique | Évite les doublons |
| `date_invitation` | Date | Automatique | |
| `declencheur` | Choix : Chaud / Froid | Non | Quel questionnaire a déclenché |
| `note_declenchement` | Nombre | Non | Note qui a déclenché l'invitation |
| `lien_avis_clique` | Booléen | Non | Si tracking possible |
| `avis_laisse` | Booléen | Non | À mettre à jour manuellement |
| `plateforme_avis` | Texte | Non | Google / LinkedIn / Trustpilot |
| `url_avis` | URL | Non | Lien vers l'avis public |

---

### TABLE 10 — `indicateurs_calcules`

> *Table de synthèse, mise à jour automatiquement par Make.com ou calculée dans Airtable.*

| Champ | Type | Description |
|-------|------|-------------|
| `id_indicateur` | ID unique | |
| `id_session` | Référence → sessions | |
| `id_axe` | Référence → axes | |
| `taux_reponse_avant` | % calculé | Nb répondu / Nb envoyé |
| `taux_reponse_chaud` | % calculé | |
| `taux_reponse_froid` | % calculé | |
| `note_satisfaction_chaud_moy` | Décimal | Moyenne des notes à chaud |
| `note_satisfaction_froid_moy` | Décimal | Moyenne des notes à froid |
| `taux_recommandation_chaud` | % calculé | % "Oui certainement" + "Probablement oui" |
| `taux_application_froid` | % calculé | % ayant mis en pratique |
| `taux_gain_temps_declare` | % calculé | % déclarant un gain de temps |
| `nb_verbatims_positifs` | Nombre | Comptage manuel ou IA |
| `nb_verbatims_amelioration` | Nombre | |
| `score_impact_global` | Score calculé | Formule composite (voir dashboard) |
| `periode` | Date | Mois/trimestre de la session |

---

### Récapitulatif des relations

```
entreprises (1) ──── (N) clients
clients (1) ──── (N) participants
clients (1) ──── (N) sessions
axes (1) ──── (N) sessions
sessions (1) ──── (N) participants  [via table de jointure sessions_participants]
participants (1) ──── (1) reponses_avant
participants (1) ──── (1) reponses_chaud    [par session]
participants (1) ──── (1) reponses_froid    [par session]
participants (1) ──── (N) statut_avis
sessions (1) ──── (1) indicateurs_calcules
```

---

### Champs recommandés pour les filtres du dashboard

| Champ | Filtre utile | Localisation |
|-------|-------------|--------------|
| `id_axe` / `nom_axe` | Filtrer par Déclic/Activation/Transformation/Systèmes | sessions, toutes les réponses |
| `date_session` | Filtrer par période (mois, trimestre, année) | sessions |
| `formateur` | Comparer les formateurs | sessions |
| `profil_repondant` | Dirigeant vs. salarié vs. indépendant | reponses_avant |
| `note_satisfaction_chaud` | Identifier les insatisfaits | reponses_chaud |
| `note_satisfaction_froid` | Idem à froid | reponses_froid |
| `frequence_application` | Filtrer sur la mise en pratique | reponses_froid |
| `gain_temps_declare` | Filtrer sur les gains déclarés | reponses_froid |
| `taille_entreprise` | Segmenter par taille | entreprises |
| `modalite` | Présentiel vs. distanciel | sessions |
