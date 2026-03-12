# QUALi — Système Autonome de Suivi des Formations

> Système indépendant de collecte, mesure et pilotage de l'impact des formations.
> Conçu pour un organisme de formation pilotant 4 axes d'accompagnement.

---

## Structure du dépôt

| Fichier | Contenu |
|---------|---------|
| `01_VISION_ET_ARCHITECTURE.md` | Vision produit + Architecture fonctionnelle MVP (Parties 1 & 2) |
| `02_PARCOURS_UTILISATEUR.md` | Parcours complet avant / chaud / froid + logique de relances (Partie 3) |
| `03_QUESTIONNAIRES.md` | 3 questionnaires améliorés avec tronc commun + questions par axe (Partie 4) |
| `04_MODELE_DE_DONNEES.md` | Structure des 10 tables + relations + champs filtres dashboard (Partie 5) |
| `05_DASHBOARD.md` | 7 vues dashboard + KPIs + graphiques + indicateurs prioritaires (Partie 6) |
| `06_AUTOMATISATIONS.md` | 12 scénarios Make.com avec règles métier et templates email (Partie 7) |
| `07_PLAN_DEPLOIEMENT.md` | Plan en 3 phases + risques + décisions + checklist finale (Partie 8) |

---

## Les 4 axes d'accompagnement

| Axe | Description | Durée typique | Délai questionnaire froid |
|-----|-------------|---------------|--------------------------|
| **Déclic** | Sensibilisation, découverte, compréhension initiale | 1 journée | J+15 |
| **Activation** | Premiers usages concrets, mise en mouvement | 1 journée | J+21 |
| **Transformation** | Changement de pratiques, intégration dans l'activité | 2-3 jours | J+30 |
| **Systèmes** | Création d'outils, tableaux de bord, agents IA | Variable | J+45 |

---

## Stack recommandée

| Module | Outil | Rôle |
|--------|-------|------|
| Formulaires | **Tally.so** | Collecte des réponses, logique conditionnelle par axe |
| Base de données | **Airtable** | Stockage relationnel, vues filtrées |
| Automatisations | **Make.com** | Envois, relances, alertes, calculs |
| Emails | **Brevo** | Emails transactionnels avec variables |
| Dashboard | **Looker Studio** | Visualisation dynamique, gratuit |

---

## Les 3 questionnaires

| Questionnaire | Moment | Durée | Déclenche |
|---------------|--------|-------|-----------|
| **Positionnement avant** | J-5 avant la formation | 3-5 min | Inscription confirmée |
| **Satisfaction à chaud** | Soir du dernier jour | 4-6 min | Session réalisée |
| **Impact à froid** | J+15 à J+45 selon axe | 5-7 min | Délai axe atteint |

Chaque questionnaire comporte :
- Un **tronc commun** identique pour les 4 axes (comparaison possible)
- Un **bloc spécifique par axe** (3 à 5 questions adaptées)

---

## Démarrer en 3 étapes

1. **Jour 1** — Créer la base Airtable (modèle dans `04_MODELE_DE_DONNEES.md`)
2. **Jour 2** — Créer le questionnaire à chaud sur Tally.so (questions dans `03_QUESTIONNAIRES.md`)
3. **Jour 3** — Configurer Make.com pour enregistrer les réponses Tally → Airtable (Scénario 11 dans `06_AUTOMATISATIONS.md`)

---

## Indicateurs clés à surveiller

| KPI | Source | Seuil d'alerte |
|-----|--------|----------------|
| Satisfaction moyenne à chaud | reponses_chaud | < 7/10 |
| Satisfaction moyenne à froid | reponses_froid | < 7/10 |
| Taux de mise en application | reponses_froid | < 60% |
| Taux de recommandation | reponses_froid | < 70% |
| Taux de réponse froid | statut_questionnaire_froid | < 40% |

---

*Système conçu comme MVP autonome — indépendant de l'outil existant.*
*Objectif : déployer en moins de 3 semaines, apprendre vite, améliorer ensuite.*
