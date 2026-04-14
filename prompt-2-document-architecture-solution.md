# Prompt 2 — Génération d'un document d'architecture de solution (rendu RAG)

## Contexte d'utilisation

Ce prompt simule le **rendu produit par le système RAG** d'assistance à l'architecture. Il prend en entrée une description de solution fournie par un architecte et génère un document d'architecture complet, structuré selon les standards BNSL, aligné sur les guides du corpus de référence. Ce document est le **premier livrable du pipeline agentique** — il sera ensuite consommé par des agents aval pour produire de la documentation Word, des vues Archimate, etc.

---

## Prompt

```
Tu es le système « Architecture Supportée par l'IA » (ASAI) de la Banque Nordique du Saint-Laurent (BNSL). Tu as été alimenté par les guides de référence d'architecture de la BNSL, notamment :
- BNSL-ARCH-SAAS-001 — Guide d'évaluation et d'adoption des solutions SaaS
- BNSL-ARCH-SAAS-002 — Patron d'intégration SaaS ↔ Systèmes internes
- BNSL-ARCH-SAAS-003 — Standard IAM pour les solutions SaaS
- BNSL-ARCH-SAAS-004 — Résidence et souveraineté des données dans les SaaS
- BNSL-ARCH-SAAS-005 — Continuité et résilience des SaaS critiques

Ces guides sont disponibles dans le dossier source-synth du projet

Un architecte de solution vient de te soumettre la description suivante :

---

**Description de la solution soumise par l'architecte :**

> Nous souhaitons adopter **Salesforce Financial Services Cloud (FSC)** comme CRM centralisé pour les conseillers du réseau de succursales (environ 1 200 utilisateurs). La solution remplacera notre CRM maison actuel (application .NET hébergée on-prem). Salesforce FSC devra se synchroniser avec notre système de core banking (Temenos Transact, on-prem), notre annuaire d'entreprise (Azure AD), et notre plateforme de gestion documentaire (SharePoint Online). Les données clients (profils, comptes, opportunités) seront hébergées dans Salesforce. Les conseillers accéderont à Salesforce via le navigateur web et l'application mobile Salesforce. Le projet est en phase d'initiation, aucune décision d'architecture n'a encore été prise.

---

À partir de cette description et de ta base de connaissances BNSL, génère un **document d'architecture de solution complet**, en format Markdown, respectant la structure corporative BNSL définie ci-dessous.

---

### Structure corporative obligatoire du document

Le document doit suivre exactement cette structure de sections. Chaque section doit être substantielle et alignée sur les guides de référence BNSL.

---

#### En-tête du document

Tableau de métadonnées :
- **Identifiant** : `BNSL-ARCH-SOL-[AAAA-NNN]` (génère un identifiant fictif cohérent)
- **Titre** : titre descriptif de la solution
- **Version** : 0.1 — Ébauche initiale (générée par ASAI)
- **Statut** : En révision
- **Architecte responsable** : [À compléter par l'architecte]
- **Date de génération** : [date fictive réaliste]
- **Domaine** : Distribution / Relation client

---

#### Section 1 — Résumé exécutif

En 150 à 250 mots : contexte d'affaires, objectif de la solution, bénéfices attendus, et mention des principaux enjeux d'architecture identifiés.

---

#### Section 2 — Contexte et objectifs

2.1 Contexte d'affaires (problème à résoudre, état actuel)
2.2 Objectifs architecturaux (alignés sur les capacités visées)
2.3 Hors-portée explicite (ce que cette solution ne couvre pas)

---

#### Section 3 — Vue de contexte (diagramme haut niveau)

Inclus **obligatoirement** un diagramme Mermaid de type `C4Context` ou, si C4 n'est pas disponible, un `graph TD` ou `graph LR` qui représente :
- Le système central (Salesforce FSC)
- Les systèmes externes qui interagissent avec lui (Temenos, Azure AD, SharePoint Online, CRM actuel en phase de transition)
- Les acteurs utilisateurs (Conseillers, Administrateurs, Clients — si applicable)
- Les flux d'information principaux (sens des échanges)

Le diagramme doit ressembler à une vue de contexte de style Archimate / C4 Level 1. Utilise des labels descriptifs sur les flèches pour indiquer la nature des échanges (ex. : « Synchronisation des comptes clients », « Authentification SSO »).

---

#### Section 4 — Décisions d'architecture

Pour chaque décision, utilise le format ADR (Architecture Decision Record) simplifié :

**ADR-001 : [Titre de la décision]**
- Contexte : …
- Options évaluées : …
- Décision : …
- Justification (alignement avec guides BNSL) : …
- Conséquences / risques : …

Génère **au moins 4 ADR** couvrant :
1. Le patron d'intégration avec Temenos (référence BNSL-ARCH-SAAS-002)
2. La stratégie IAM / SSO (référence BNSL-ARCH-SAAS-003)
3. La résidence des données clients dans Salesforce (référence BNSL-ARCH-SAAS-004)
4. Le niveau de criticité et les exigences de résilience (référence BNSL-ARCH-SAAS-005)

---

#### Section 5 — Vue d'intégration

5.1 Carte des intégrations (tableau Markdown) : système source, système cible, direction, patron d'intégration retenu, fréquence, volume estimé, données échangées

5.2 Diagramme Mermaid des flux d'intégration (sequenceDiagram ou flowchart) illustrant le flux principal : mise à jour d'un profil client dans Temenos → synchronisation vers Salesforce FSC

---

#### Section 6 — Exigences de sécurité et de conformité

6.1 Classification des données traitées
6.2 Exigences IAM (SSO, MFA, provisionnement, comptes techniques) — référence BNSL-ARCH-SAAS-003
6.3 Résidence des données — zone applicable selon BNSL-ARCH-SAAS-004 et justification
6.4 Conformité réglementaire (OSFI B-10, Loi 25, PIPEDA) — impacts identifiés
6.5 Points ouverts en sécurité (éléments à valider avec le CISO)

---

#### Section 7 — Continuité et résilience

7.1 Niveau de criticité retenu et justification
7.2 Exigences RTO/RPO
7.3 Mode dégradé et procédures de repli (si Salesforce indisponible)
7.4 Exigences contractuelles envers Salesforce (SLA, droit d'audit, notification d'incident)

---

#### Section 8 — Risques et points ouverts

Tableau Markdown avec colonnes : ID, Description du risque, Probabilité (H/M/F), Impact (H/M/F), Mitigation proposée, Statut

Inclus au moins 5 risques réalistes (ex. : concentration de risque chez Salesforce, synchronisation des données en cas de panne, gestion des comptes orphelins, dépassement de licence, résidence effective des sauvegardes).

---

#### Section 9 — Feuille de route d'architecture (phases)

Tableau des phases recommandées : Phase, Objectif, Livrables clés, Prérequis, Durée estimée

Couvre au moins 3 phases : Fondations (IAM, intégration de base), Déploiement pilote, Déploiement étendu.

---

#### Section 10 — Références

- Références aux guides BNSL utilisés (SAAS-001 à SAAS-005)
- Références externes : documentation Salesforce FSC, OSFI B-10, Loi 25
- Documents connexes à produire (ex. : PIA, Plan de migration, Plan de tests de résilience)

---

### Directives de génération

- Le document doit être **autonome et directement exploitable** par un architecte de solution comme point de départ.
- Là où des informations manquent (ex. : volumes de données, contraintes budgétaires), indique-le explicitement avec la mention `[À PRÉCISER]` plutôt que d'inventer.
- Toutes les recommandations doivent **citer explicitement le guide BNSL de référence** (ex. : « Conformément à BNSL-ARCH-SAAS-003… »).
- Le ton est professionnel, concis, orienté décision.
- La longueur totale du document (hors diagrammes) doit être d'environ 2 000 à 3 000 mots.
- Produis le document en un seul bloc Markdown continu.
```
```
