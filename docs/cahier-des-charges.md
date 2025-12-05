# Cahier des charges — Plateforme de suivi adaptatif des compétences

## 1. Contexte et objectifs
- **Public cible** : écoles, collèges, lycées, centres de formation et universités.
- **Objectif général** : analyser le besoin, personnaliser les parcours et assurer un suivi continu des compétences, afin de proposer un accompagnement individualisé et des indicateurs clairs pour l'ensemble des acteurs (apprenants, formateurs, responsables).
- **Enjeux** : fiabiliser la collecte des évaluations, automatiser le calcul des niveaux de maîtrise, fournir des alertes précoces et des tableaux de bord lisibles pour guider les décisions pédagogiques.

## 2. Périmètre fonctionnel
1. Gestion des référentiels de compétences (import, versionnement, alignement avec des cadres existants).
2. Gestion des profils (apprenants, formateurs, responsables, administrateurs) et des rôles/permissions.
3. Analyse du besoin initial (positionnement, tests diagnostiques, auto-évaluation).
4. Personnalisation des parcours (objectifs, modules, remédiations recommandées).
5. **Suivi précis et continu de l'acquisition des compétences** (évaluations formatives/sommatives, journalisation, alertes).
6. **Indicateurs clairs et tableaux de bord** (progression, lacunes, KPIs, exports).
7. Notifications et communication (email, in-app, option SMS/Push selon contexte).
8. Intégration et interopérabilité (LMS, SSO, LTI, import/export CSV/Excel, API REST).
9. Administration (paramétrages, gestion des sessions, sécurité, conformité RGPD).

## 3. Acteurs et parcours utilisateur
- **Apprenant** : réalise un positionnement, suit ses objectifs, reçoit des feedbacks, consulte son tableau de bord (radar, progression, plan personnalisé d'amélioration).
- **Formateur** : crée ou importe des référentiels, saisit les évaluations, suit les groupes/classes via des vues agrégées (heatmaps), reçoit des alertes de stagnation ou baisse de niveau.
- **Responsable / Manager** : suit les indicateurs globaux (taux de complétion, atteinte des niveaux cibles, efficacité des modules), planifie des actions correctives.
- **Administrateur** : configure les accès, gère les référentiels globaux, supervise la conformité et les sauvegardes.

## 4. Exigences fonctionnelles détaillées
### 4.1 Référentiels et parcours
- Import de référentiels (CSV/Excel, LOM/SCORM/LTI si applicable), versionnement, archivage.
- Mapping des référentiels aux parcours et aux objectifs par promotion/groupe.
- Définition d'objectifs par compétence (niveaux cibles, échéances, critères de validation).

### 4.2 Évaluations formatives et sommatives (Point 3)
| Étape | Description | Exigences techniques |
|---|---|---|
| A. Saisie des évaluations formatives | Saisie rapide après TP, devoir, observation (mobile/web). | Interfaces rapides (échelle, binaire Oui/Non, score), offline-friendly si possible, validation des champs, gestion des brouillons. |
| B. Mise à jour du niveau | Chaque saisie actualise le niveau de maîtrise de l'apprenant. | Moteur de scoring paramétrable (moyenne pondérée, dernière note majorée, décroissance dans le temps), recalcul asynchrone ou synchrone, gestion des exceptions. |
| C. Journal de compétence | Historique horodaté et traçable. | Tables dédiées liées apprenant/compétence ; audit log (qui a saisi, quand, source). |
| D. Déclenchement d'alertes | Alerter en cas de stagnation ou baisse. | Triggers ou tâches planifiées (cron/queue), seuils configurables, canaux (email, notification in-app), possibilité d'accusé de lecture. |

### 4.3 Tableaux de bord et indicateurs (Point 4)
- **Apprenant** :
  - Radar de compétences montrant forces/lacunes ; trajectoire temporelle par compétence.
  - Chemin restant (PPI), recommandations de modules/ressources, rappels d'échéances.
- **Formateur** :
  - Vue agrégée par classe/groupe : heatmap ou tableau comparatif des niveaux par compétence et par élève.
  - Filtrage par session, module, compétence ; tri des élèves en difficulté ; génération de plans de remédiation.
- **Responsable** :
  - KPIs : taux de complétion, % d'atteinte des niveaux cibles, progression moyenne, efficacité des modules (gain moyen par module), taux d'alertes résolues.
  - Suivi des promotions et comparaison inter-groupes ; indicateurs de qualité (délai de feedback, taux d'engagement).
- **Export de données** :
  - Exports CSV/Excel/PDF pour reporting ou audit ; API pour extraction automatisée ; respect des règles de confidentialité et d'anonymisation.

### 4.4 Notifications et communication
- Règles d'alerte configurables par compétence, groupe, promotion.
- Templates d'email/notification multilingues ; personnalisation par établissement.
- Centre de notifications in-app avec filtres (non lues, critiques, informatives).

### 4.5 Personnalisation et recommandations
- Adaptation dynamique du parcours en fonction du scoring et des objectifs.
- Suggestion de ressources (modules, exercices, tutoriels) en cas de lacunes détectées.
- Prise en charge des accommodations (handicap, rythme personnalisé, temps supplémentaire).

### 4.6 Administration et conformité
- Gestion des rôles et permissions fines (RBAC).
- Journalisation des accès et actions (audit). Sauvegardes et restauration planifiées.
- Conformité RGPD : consentement, droit à l'oubli, portabilité des données, minimisation.

## 5. Exigences techniques
- **Architecture** : API RESTful (ou GraphQL en option), frontend web responsive + app mobile ou PWA, séparation front/back, support multitenant (établissements).
- **Interopérabilité** : LTI 1.3/Advantage, OAuth2/SSO (SAML/OIDC), webhooks, connecteurs LMS (Moodle, Canvas, etc.), import/export standards.
- **Base de données** : tables dédiées aux référentiels, évaluations, journaux, notifications ; gestion des versions et des statuts ; contraintes d'intégrité.
- **Moteur de scoring** : algorithmes paramétrables, gestion du poids des évaluations, prise en compte de la recence, gestion des évaluations invalidées ou corrigées.
- **Scalabilité et performance** : queues de traitement pour recalcul/alertes, cache pour les tableaux de bord, pagination et filtrage serveur, tests de charge.
- **Sécurité** : chiffrage en transit (TLS), hachage des mots de passe, gestion des tokens, protection contre CSRF/XSS/SQLi, limitation des taux (rate limiting).

## 6. Parcours critiques (UX)
- Saisie ultra-rapide côté formateur (mobile-first, raccourcis, formulaires optimisés, mode hors-ligne optionnel avec synchronisation).
- Visualisations claires et accessibles (contrastes, alternatives textuelles, compatibilité lecteurs d'écran).
- Navigation par rôle avec onboarding guidé et contextualisation par promotion/groupe.

## 7. Reporting et audits
- Traçabilité complète des évaluations (horodatage, auteur, source, version du référentiel).
- Historique des alertes (émission, réception, résolution) et SLA de traitement.
- Journal d'export et téléchargements, avec justification et périmètre des données exportées.

## 8. Tests et qualité
- Tests unitaires et d'intégration (API, moteur de scoring, règles d'alerte).
- Tests d'acceptation sur les parcours clés (saisie formateur, radar apprenant, heatmap formateur, KPIs responsable).
- Tests de charge sur le recalcul des scores et la génération des tableaux de bord.

## 9. Déploiement et exploitation
- Environnements Dev/Recette/Prod, CI/CD avec automatisation des migrations et des tests.
- Monitoring (APM, logs centralisés, métriques), alerting sur les erreurs et performances.
- Politique de sauvegarde/restauration, plan de continuité et reprise après incident.

## 10. Livrables
- Documentation fonctionnelle (guides par rôle), documentation technique (API, schémas de données, scoring, alertes).
- Jeux de données de test (référentiels types, scénarios d'évaluation).
- Tableau de traçabilité des exigences et protocole de recette.

## 11. Contraintes et risques
- Hétérogénéité des référentiels selon les établissements ; nécessité de normalisation.
- Sensibilité des données personnelles (RGPD) et dépendance aux systèmes externes (LMS/SSO).
- Adoption par les formateurs : simplicité et rapidité de saisie comme facteur clé de succès.

## 12. Planning indicatif (macro)
1. Cadrage & conception (2-4 semaines) : ateliers besoins, maquettes UX, modèles de données, choix techniques.
2. Implémentation itérative (8-16 semaines) : modules référentiels, scoring, tableaux de bord, notifications, intégrations SSO/LMS.
3. Pilote (4-6 semaines) : déploiement sur un groupe restreint, ajustements UX/scoring.
4. Généralisation & montée en charge (4-8 semaines) : optimisations, formation des utilisateurs, gouvernance des données.

## 13. Indicateurs de succès
- Temps moyen de saisie d'une évaluation par compétence < 10 secondes en situation réelle.
- Délai moyen de recalcul des niveaux < 1 minute après saisie (objectif asynchrone).
- Taux d'adoption des tableaux de bord (>80% des formateurs et responsables actifs).
- Réduction mesurée des compétences non maîtrisées après remédiation ciblée (>15% sur un trimestre pilote).
