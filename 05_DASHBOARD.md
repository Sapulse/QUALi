# QUALi — Dashboard & Indicateurs

---

## PARTIE 6 — DASHBOARD ET INDICATEURS

### Principes de conception du dashboard

1. **Pas de KPI sans action possible** : chaque indicateur doit répondre à "et donc, qu'est-ce que je fais ?"
2. **Les 4 axes toujours comparables** : toutes les vues principales incluent un filtre ou une décomposition par axe
3. **Deux lectures** : quantitative (notes, %) + qualitative (verbatims)
4. **3 niveaux de zoom** : vision globale → par axe → par session

---

### Architecture des vues du dashboard

```
┌─────────────────────────────────────────────────────────┐
│  VUE 1 : TABLEAU DE BORD GLOBAL                         │
│  Vue synthétique tous axes confondus                    │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│  VUE 2 : COMPARAISON PAR AXE                            │
│  Déclic / Activation / Transformation / Systèmes        │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│  VUE 3 : SATISFACTION & QUALITÉ                         │
│  Notes, NPS, formateur, verbatims satisfaction          │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│  VUE 4 : IMPACT & MISE EN APPLICATION                   │
│  Application, gain de temps, bénéfices, ROI perçu       │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│  VUE 5 : BESOINS & ATTENTES                             │
│  Récurrence des besoins, attentes, profils              │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│  VUE 6 : VERBATIMS & CITATIONS                          │
│  Extraits qualitatifs par axe et catégorie              │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│  VUE 7 : AVIS & RECOMMANDATIONS                         │
│  Suivi des invitations, avis laissés, NPS global        │
└─────────────────────────────────────────────────────────┘
```

---

### Filtres globaux disponibles sur toutes les vues

| Filtre | Options |
|--------|---------|
| Période | Ce mois / Ce trimestre / Cette année / Personnalisé |
| Axe | Tous / Déclic / Activation / Transformation / Systèmes |
| Formateur | Tous / César / Mickael |
| Modalité | Tous / Présentiel / Distanciel |
| Profil participant | Tous / Dirigeant / Salarié / Indépendant |

---

## VUE 1 — TABLEAU DE BORD GLOBAL

### KPIs en cartes (bandeau supérieur)

| KPI | Calcul | Format |
|-----|--------|--------|
| Sessions réalisées | Comptage sessions statut "Réalisée" | Nombre |
| Participants formés | Somme nb_participants | Nombre |
| Satisfaction moyenne (chaud) | Moyenne notes_satisfaction_chaud | Note /10 + tendance ▲▼ |
| Satisfaction moyenne (froid) | Moyenne notes_satisfaction_froid | Note /10 + tendance ▲▼ |
| Taux de recommandation | % "Oui certainement" + "Probablement oui" (froid) | % |
| Taux de mise en application | % ayant répondu "Régulièrement" ou "Quelques fois" (froid) | % |
| Taux de réponse moyen | Moyenne des 3 taux de réponse | % |
| Avis publics récoltés | Comptage avis_laisse = Oui | Nombre |

### Graphiques principaux

**Graphique 1 : Évolution de la satisfaction dans le temps**
- Type : Courbe double (satisfaction chaud + froid)
- Axe X : Mois
- Axe Y : Note /10
- Objectif : Voir la tendance et l'écart chaud/froid

**Graphique 2 : Sessions par axe (camembert ou barres)**
- Type : Graphique en secteurs
- Données : Nombre de sessions par axe
- Objectif : Voir la répartition de l'activité

**Graphique 3 : Taux de réponse par questionnaire**
- Type : Barres horizontales
- Données : Avant / Chaud / Froid — taux de réponse %
- Objectif : Identifier si un questionnaire est sous-rempli

---

## VUE 2 — COMPARAISON PAR AXE

> *C'est la vue la plus stratégique. Elle permet de comparer les 4 axes sur les mêmes métriques.*

### Tableau comparatif des 4 axes

| Métrique | Déclic | Activation | Transformation | Systèmes |
|----------|--------|------------|----------------|----------|
| Nb sessions | ■ | ■ | ■ | ■ |
| Nb participants | ■ | ■ | ■ | ■ |
| Note satisfaction chaud (moy) | ■/10 | ■/10 | ■/10 | ■/10 |
| Note satisfaction froid (moy) | ■/10 | ■/10 | ■/10 | ■/10 |
| Taux recommandation | ■% | ■% | ■% | ■% |
| Taux mise en application | ■% | ■% | ■% | ■% |
| Taux gain de temps déclaré | ■% | ■% | ■% | ■% |
| Taux réponse froid | ■% | ■% | ■% | ■% |

### Graphique radar par axe
- Type : Spider chart (radar)
- Dimensions : Satisfaction / Application / Gain de temps / Recommandation / Taux réponse
- Objectif : Portrait visuel de chaque axe sur 5 dimensions

### Graphique : Score impact global par axe
- **Formule du score impact global** (V1 simple) :
  ```
  Score impact = (note_froid × 0.4) + (taux_application × 0.3) + (taux_gain_temps × 0.2) + (taux_recommandation × 0.1)
  ```
- Type : Barres verticales comparant les 4 axes
- Objectif : Réponse directe à "quel axe produit le plus de valeur ?"

---

## VUE 3 — SATISFACTION & QUALITÉ

### KPIs spécifiques

| KPI | Calcul | Seuil d'alerte |
|-----|--------|----------------|
| Note satisfaction chaud globale | Moyenne toutes sessions | < 7/10 = attention |
| Note satisfaction froid globale | Moyenne toutes sessions | < 7/10 = attention |
| Écart chaud - froid | note_chaud_moy - note_froid_moy | > 1 point = signal négatif |
| % très satisfaits (≥ 8/10) | % participants note ≥ 8 (froid) | Objectif > 70% |
| % insatisfaits (≤ 5/10) | % participants note ≤ 5 | Objectif < 5% |
| % promoteurs NPS | % "Oui certainement" (froid) | Objectif > 60% |

### Graphiques satisfaction

**Histogramme de distribution des notes à chaud**
- Type : Histogramme (notes 0-10)
- Couleurs : Rouge (0-5) / Orange (6-7) / Vert (8-10)
- Objectif : Visualiser la distribution

**Histogramme de distribution des notes à froid**
- Même format que chaud — comparaison visuelle

**Évolution par formateur**
- Type : Barres groupées
- Données : Note moyenne par formateur par trimestre

### Tableau des évaluations détaillées (moyenne par item)

| Critère évalué | Note moy /5 | Tendance |
|----------------|-------------|---------|
| Adéquation au niveau | ■ | ▲▼ |
| Clarté des explications | ■ | ▲▼ |
| Adéquation aux attentes | ■ | ▲▼ |
| Rythme adapté | ■ | ▲▼ |
| Temps de pratique | ■ | ▲▼ |
| Écoute du formateur | ■ | ▲▼ |

> *Cible : identifier les critères sous 3.5/5 pour action corrective.*

---

## VUE 4 — IMPACT & MISE EN APPLICATION

### KPIs principaux

| KPI | Calcul | Objectif V1 |
|-----|--------|-------------|
| Taux de mise en application | % fréquence "Régulièrement" ou "Quelques fois" | > 70% |
| Taux de gain de temps déclaré | % C1 = "Oui clairement" + "Oui un peu" | > 60% |
| Estimation moyenne du gain hebdomadaire | Distribution de C2 | — |
| Score d'intégration IA moyen | Moyenne note B4 (0-10) | — |
| Taux "impact transformationnel" | % E-TRA-3 "Oui significatif" (axe Transformation) | — |

### Graphiques impact

**Graphique : Taux d'application par axe**
- Type : Barres empilées (Régulièrement / Quelques fois / Une fois / Pas encore / Non)
- Segmenté par axe
- Objectif : Comparer les axes sur l'adoption

**Graphique : Distribution du gain de temps déclaré**
- Type : Barres horizontales
- Données : Options C2 (Moins de 30 min / 30 min-1h / 1h-2h / Plus de 2h / NSP)
- Objectif : Quantifier le ROI déclaré

**Graphique : Bénéfices les plus fréquemment cités**
- Type : Nuage de mots ou barres triées
- Données : Multi-sélection C3 — comptage des occurrences par option
- Objectif : Identifier les bénéfices qui reviennent le plus

**Évolution du niveau d'intégration IA (avant → froid)**
- Type : Barres groupées ou courbes
- Données : B4 (niveau_avant, 0-10) vs B4 (niveau_integration_froid, 0-10)
- Objectif : Visualiser la progression mesurée

---

## VUE 5 — BESOINS & ATTENTES

### KPIs

| KPI | Description |
|-----|-------------|
| Top 5 motivations | Classement des options C1 (avant) par fréquence |
| Top 5 tâches à accélérer | Classement des options C3 (avant) par fréquence |
| Niveau de départ moyen par axe | Moyenne des 4 items de niveau (B1-B4) par axe |
| % participants ayant un objectif précis | % répondants ayant rempli C2 |
| Besoins futurs identifiés | % F1 = "Oui" (froid) + répartition F2 |

### Graphiques besoins

**Top motivations à rejoindre une formation (C1)**
- Type : Barres horizontales triées par fréquence
- Objectif : Savoir ce qui drive les inscriptions

**Carte des niveaux de départ par axe**
- Type : Tableau de chaleur (heatmap)
- Axes : Items B1-B4 en lignes / Axes en colonnes
- Objectif : Identifier si les niveaux de départ varient selon l'axe

**Besoins futurs remontés à froid**
- Type : Barres horizontales
- Données : Options F2 (froid)
- Objectif : Identifier les prochaines opportunités commerciales

---

## VUE 6 — VERBATIMS & CITATIONS

> *Cette vue est qualitative. Elle ne se lit pas avec des graphiques mais avec des extraits triés.*

### Organisation des verbatims

**Section 1 : Ce qui a eu le plus d'impact**
- Source : reponses_froid.impact_principal_activite (D3)
- Filtrable par axe, période
- Affichage : liste de citations avec nom (ou anonymisé), axe, date

**Section 2 : Exemples concrets de succès**
- Source : reponses_froid.exemple_concret_succes (C4)
- Très utile pour les arguments commerciaux et cas clients

**Section 3 : Points forts de la session**
- Source : reponses_chaud.point_fort (F1)
- Affichage par axe — utile pour savoir ce qui fonctionne

**Section 4 : Pistes d'amélioration**
- Source : reponses_chaud.point_amelioration (F2)
- À lire avec attention — signaux d'amélioration produit

**Section 5 : Témoignages publics potentiels**
- Source : reponses_froid.temoignage_libre (F3)
- Marqué "à valider" — utile pour extraire des témoignages pour le site/LinkedIn

**Tableau de tri des verbatims**
| Champ | Valeur affichée |
|-------|----------------|
| Axe | Badge coloré |
| Date | Date de soumission |
| Note froid | Note /10 associée |
| Extrait | 150 premiers caractères + lire plus |
| Usage | Usage commercial possible ? (case à cocher manuelle) |

---

## VUE 7 — AVIS & RECOMMANDATIONS

### KPIs avis

| KPI | Calcul |
|-----|--------|
| % éligibles à une invitation | % note ≥ 8/10 (chaud ou froid) |
| % invitations envoyées | invitation_envoyee = Oui / total éligibles |
| % avis laissés | avis_laisse = Oui / invitation_envoyee |
| NPS global (chaud) | % promoteurs (Oui certainement) - % détracteurs (≤ 5) |
| NPS à froid | Idem sur questionnaire froid |

### Graphique : Entonnoir avis
```
Participants formés
        │ 100%
Répondants questionnaire froid
        │ XX%
Éligibles invitation (note ≥ 8)
        │ XX%
Invitations envoyées
        │ XX%
Avis effectivement laissés
        │ XX%
```

---

## Récapitulatif des indicateurs prioritaires

### Indicateurs de niveau 1 (à surveiller chaque mois)

| Indicateur | Source | Seuil d'alerte |
|------------|--------|----------------|
| Satisfaction moyenne à chaud | reponses_chaud | < 7/10 |
| Satisfaction moyenne à froid | reponses_froid | < 7/10 |
| Taux de mise en application | reponses_froid | < 60% |
| Taux de recommandation | reponses_froid | < 70% |
| Taux de réponse froid | statut_questionnaire_froid | < 40% |

### Indicateurs de niveau 2 (à analyser chaque trimestre)

| Indicateur | Source | Usage |
|------------|--------|-------|
| Score impact par axe | indicateurs_calcules | Comparaison axes |
| Top 5 bénéfices déclarés | reponses_froid C3 | Argumentaire |
| Top 5 motivations | reponses_avant C1 | Ciblage marketing |
| Gain de temps moyen déclaré | reponses_froid C2 | Preuve ROI |
| Besoins futurs | reponses_froid F2 | Plan commercial |

### Indicateurs de niveau 3 (à analyser ponctuellement)

| Indicateur | Usage |
|------------|-------|
| Verbatims d'impact | Cas clients, témoignages |
| Écart satisfaction chaud/froid | Qualité perçue dans le temps |
| Niveaux de départ moyens par axe | Adapter le ciblage |
| Progression déclarée (avant → froid) | Mesure pédagogique |
