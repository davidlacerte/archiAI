# Prompt 1 — Génération des documents de référence d'architecture SaaS

## Contexte d'utilisation

Ce prompt est destiné à générer **5 documents de référence d'architecture** synthétiques simulant ceux d'une institution financière canadienne. Ces documents constituent le corpus source d'une POC de système RAG d'assistance à l'architecture de solution. Chaque document couvre un **domaine technique distinct et précis** de l'architecture SaaS. Ils doivent être réalistes, cohérents entre eux, et refléter la profondeur attendue d'un guide de pratique dans un contexte bancaire réglementé.

---

## Prompt

```
Tu es un architecte d'entreprise senior dans une grande institution financière canadienne. Tu dois produire 5 documents de référence d'architecture — guides, patrons ou standards — chacun couvrant un domaine technique précis et distinct lié aux solutions SaaS. Ces documents constituent le corpus de référence de la pratique d'architecture. Ils sont destinés à être ingérés par un système RAG pour assister les architectes de solution dans la génération de documentation d'architecture.

---

### Contraintes générales

- Chaque document est produit en format Markdown (.md), avec titres, sous-titres, listes et, si pertinent, des diagrammes Mermaid.
- Les documents ont un style professionnel mais accessible : ce sont des guides pratiques à l'usage des architectes de solution, pas des normes ISO formelles.
- Ils peuvent se référencer mutuellement par identifiant (ex. : « voir BNSL-ARCH-SAAS-002… »).
- Ils peuvent contenir des zones intentionnellement incomplètes (TODO, renvois à des documents non encore publiés) — c'est réaliste.
- Ils peuvent se recouper ponctuellement sur des thèmes transversaux (ex. : chiffrement mentionné dans plusieurs guides sous des angles différents).
- Le contexte institutionnel fictif est : **Banque Nordique du Saint-Laurent (BNSL)**, institution financière à charte fédérale, ~8 000 employés, présence au Québec, Ontario et Alberta.
- Les références réglementaires doivent être réalistes : OSFI (ligne directrice B-10), Loi 25 (Québec), PIPEDA, FINTRAC, etc.
- Les technologies mentionnées doivent être réelles et représentatives du marché (Azure AD / Entra ID, MuleSoft, Salesforce, AWS KMS, HashiCorp Vault, Splunk, Apigee, Kong, etc.).

---

### Format de chaque document

Pour chaque document :
1. Commence par un titre `# [Identifiant] — [Titre]`
2. Inclus un bloc de métadonnées en tableau Markdown (version, statut, propriétaire, date de révision, domaine)
3. Corps du document en Markdown structuré, entre 700 et 1 300 mots (hors diagrammes)
4. Diagrammes Mermaid intégrés dans des blocs ```mermaid là où indiqué ou pertinent
5. Termine par une section `## Références` (documents BNSL connexes et sources externes)

---

### Documents à produire

Produis les 5 documents suivants, séquentiellement et en intégralité. Chaque document couvre un domaine technique précis.

---

#### Document 1 — Gestion des identités et des accès (IAM) dans les solutions SaaS

**Identifiant** : `BNSL-ARCH-SAAS-001`
**Titre** : Standard IAM pour les solutions SaaS — Fédération, provisionnement et gouvernance des accès

**Contenu attendu** :
- Objectif et portée : encadrer la gestion des identités dans tous les SaaS adoptés par la BNSL afin d'éliminer les silos d'identité et de réduire la surface d'attaque
- **Exigence de fédération SSO** : Azure AD (Entra ID) comme Identity Provider (IdP) unique, protocoles acceptés (SAML 2.0, OIDC), cas d'exception documenté
- **Provisionnement automatisé** : SCIM 2.0 comme standard, comportement attendu en création/désactivation/suppression, délai maximal de déprovisionnement après départ d'un employé
- **MFA** : obligatoire sans exception, politiques de force d'authentification selon la sensibilité des données du SaaS (Conditional Access Azure AD)
- **Comptes non-humains** (comptes de service, intégrations, API keys) : règles de gestion, rotation des secrets, interdiction des comptes partagés
- **Comptes d'urgence locaux** (break glass) : conditions d'usage, journalisation obligatoire, revue trimestrielle
- **Revue des accès** : fréquence selon criticité, outillage recommandé (Entra ID Access Reviews)
- Tableau : matrice de configuration IAM selon la classification des données traitées par le SaaS (Publique / Interne / Confidentiel / Restreint)
- Section incomplète intentionnelle : gouvernance des accès privilégiés (PAM) dans les SaaS — cadre à développer, renvoi vers document BNSL-SEC-PAM-001 (non encore publié)
- Diagramme Mermaid : flux de provisionnement/déprovisionnement d'un utilisateur via Azure AD → SCIM → SaaS

---

#### Document 2 — Journalisation et observabilité dans un environnement SaaS

**Identifiant** : `BNSL-ARCH-SAAS-002`
**Titre** : Guide de journalisation et d'observabilité pour les solutions SaaS

**Contenu attendu** :
- Objectif : définir les exigences de journalisation applicables aux SaaS afin de soutenir la détection des incidents, la conformité réglementaire et la forensique
- **Contexte réglementaire** : obligations de journalisation sous OSFI B-10, PIPEDA, et exigences internes BNSL (rétention minimale de 7 ans pour les journaux à valeur probante)
- **Catégories de journaux exigés** depuis un SaaS :
  1. Journaux d'authentification et d'accès (qui, quand, depuis où)
  2. Journaux d'actions administratives (changements de configuration, gestion des droits)
  3. Journaux d'activité métier à valeur probante (ex. : approbation d'une transaction, modification d'un profil client)
  4. Journaux d'intégration (appels API entrants/sortants, erreurs de synchronisation)
- **Mécanismes d'export** : exigences techniques (API d'export en temps réel ou near-real-time, formats acceptés : CEF, JSON, syslog), distinction entre SaaS qui peuvent pousser les logs (push) vs qui ne fournissent qu'un export (pull)
- **Centralisation SIEM** : agrégation obligatoire vers le SIEM central BNSL (Splunk), délai maximal d'ingestion, règles de corrélation à prévoir
- **Ce que le SaaS doit garantir contractuellement** : durée de rétention native des logs dans le SaaS, accès aux logs sans surcoût, immutabilité
- **Lacunes fréquentes** à investiguer lors de l'évaluation d'un SaaS (liste de questions types)
- Tableau : exigences de journalisation par niveau de criticité du SaaS (Critique / Important / Standard)
- Diagramme Mermaid : architecture de collecte des journaux SaaS → SIEM (push via webhook/API et pull via connecteur)
- Zone incomplète : intégration avec la plateforme SOAR (Security Orchestration) — à développer

---

#### Document 3 — Exposition des services internes via l'API Gateway BNSL à destination des SaaS

**Identifiant** : `BNSL-ARCH-SAAS-003`
**Titre** : Guide d'intégration entrante via l'API Gateway BNSL — Flux SaaS vers systèmes internes

**Contenu attendu** :
- Objectif : décrire comment un SaaS tiers peut consommer des services ou des données exposés par la BNSL via l'API Gateway institutionnel, de façon sécurisée et gouvernée
- **Architecture de l'API Gateway BNSL** : présentation de la couche (Kong Enterprise / Apigee — mentionner les deux comme options en cours d'évaluation), zones réseau (DMZ, zone interne), distinction entre API Gateway externe (internet) et interne
- **Patrons d'appel autorisés** depuis un SaaS externe vers les API BNSL :
  1. Appel synchrone REST/HTTPS depuis le SaaS (client credentials OAuth 2.0)
  2. Webhook sortant BNSL vers SaaS (push d'événements)
  3. Accès indirect via iPaaS/ESB (MuleSoft) comme intermédiaire
- **Patron interdit** : appel direct du SaaS vers les systèmes backend sans passer par le Gateway
- **Sécurité des appels entrants** : authentification OAuth 2.0 (client credentials), IP allowlisting obligatoire pour les SaaS, validation des payloads (schéma JSON/XML), rate limiting, protection DDoS
- **Gestion des secrets côté SaaS** : le SaaS doit démontrer comment il stocke le client_secret (exigence de gestion des secrets, pas de stockage en clair)
- **Cycle de vie des API Keys / tokens** : rotation, révocation en cas d'incident
- **Processus d'enregistrement d'un SaaS comme consommateur API** : étapes, parties prenantes, délais
- Diagramme Mermaid : flux d'appel entrant SaaS → DMZ → API Gateway → système interne (core banking), avec les couches de sécurité représentées
- Tableau : patrons d'intégration selon le type de flux (temps réel, batch, événementiel) et la direction
- Renvoi vers BNSL-ARCH-SAAS-001 pour les prérequis IAM (authentification du SaaS)

---

#### Document 4 — Bring Your Own Key (BYOK) et chiffrement des données au repos dans les SaaS

**Identifiant** : `BNSL-ARCH-SAAS-004`
**Titre** : Patron BYOK — Contrôle du chiffrement des données au repos dans les solutions SaaS

**Contenu attendu** :
- Objectif : définir les exigences et le patron d'implémentation du modèle Bring Your Own Key (BYOK) pour les SaaS traitant des données confidentielles ou restreintes de la BNSL
- **Contexte et enjeu** : pourquoi le chiffrement natif géré par le fournisseur (provider-managed keys) n'est pas suffisant pour certaines catégories de données — scénario de compromission du fournisseur, obligation de révocation
- **Définition des modèles** :
  - Provider-managed keys (PMK) : acceptable pour données Publiques et Internes
  - Customer-managed keys (CMK) via KMS du fournisseur cloud : acceptable pour données Confidentielles
  - Bring Your Own Key (BYOK) avec HSM BNSL : requis pour données Restreintes
  - Bring Your Own Encryption (BYOE) / Hold Your Own Key (HYOK) : cas extrêmes, usage limité
- **Infrastructure de gestion des clés BNSL** : HashiCorp Vault Enterprise comme KMS interne, intégration avec Azure Key Vault pour les workloads Azure, HSM physique (Thales Luna) pour les clés racines
- **Patron BYOK avec un SaaS** : comment la clé BNSL est amenée dans l'environnement du fournisseur (ex. : AWS KMS External Key Store, Salesforce Shield Platform Encryption, Microsoft Customer Key)
- **Obligations lors de l'évaluation d'un SaaS** : le SaaS supporte-t-il BYOK ? Quel niveau (tablespace, champ, fichier) ? Impact sur les performances et les fonctionnalités (champs chiffrés non indexables, etc.)
- **Procédures de rotation et de révocation des clés** : fréquence, impact opérationnel, procédure de révocation d'urgence (scénario de breach)
- **Limites et risques connus** : le BYOK ne protège pas contre un fournisseur compromis disposant d'un accès applicatif, distinction chiffrement au repos vs en transit vs en usage
- Diagramme Mermaid : flux de chiffrement/déchiffrement dans un patron BYOK (SaaS → appel au KMS BNSL pour déchiffrer la clé de données → déchiffrement de la donnée)
- Zone incomplète : procédures de gestion des clés en contexte de reprise après sinistre — à développer conjointement avec l'équipe Continuité

---

#### Document 5 — Réseautique et segmentation pour les intégrations SaaS

**Identifiant** : `BNSL-ARCH-SAAS-005`
**Titre** : Guide de réseautique et de segmentation réseau pour les flux d'intégration SaaS

**Contenu attendu** :
- Objectif : définir les patrons réseau approuvés pour les flux entre les systèmes internes BNSL et les SaaS externes, afin de garantir la segmentation, la traçabilité et la conformité aux politiques de sécurité réseau
- **Topologie réseau de référence BNSL** (description textuelle + diagramme) : zones réseau (Internet, DMZ externe, DMZ interne, zone applicative, zone de données, réseau de gestion), principe de zero-trust en cours d'adoption
- **Flux sortants (BNSL → SaaS)** :
  - Tout flux sortant doit transiter par le proxy sortant BNSL (Zscaler Internet Access) — pas de sortie internet directe depuis la zone applicative
  - FQDN allowlisting obligatoire (pas de règles par IP pour les SaaS cloud natifs)
  - Inspection TLS activée sauf exceptions documentées (certificate pinning, flux sensibles)
- **Flux entrants (SaaS → BNSL)** :
  - Tout flux entrant initié par un SaaS doit arriver sur l'API Gateway en DMZ (voir BNSL-ARCH-SAAS-003)
  - Pas d'ouverture de port entrant directement vers la zone applicative ou la zone de données
  - VPN site-à-site ou Private Link uniquement pour les cas d'exception justifiés (SaaS hébergé sur Azure/AWS avec peering privé)
- **Cas particulier — Private Connectivity** : conditions d'usage de Azure Private Link / AWS PrivateLink pour les SaaS supportant la connectivité privée (évite le transit internet, réduit la surface d'exposition)
- **Exigences de chiffrement en transit** : TLS 1.2 minimum, TLS 1.3 recommandé, liste des cipher suites acceptées, interdiction de SSL et TLS 1.0/1.1
- **Gestion des certificats** : certificats internes (PKI BNSL / DigiCert) vs certificats du SaaS, validation de la chaîne de confiance, Certificate Transparency
- **Journalisation des flux réseau** : obligation d'activer les logs de flux (NSG Flow Logs, Zscaler logs) pour tous les flux SaaS, corrélation avec les journaux applicatifs
- Diagramme Mermaid : topologie simplifiée des zones réseau BNSL et positionnement des flux SaaS entrants et sortants
- Zone incomplète : patron réseau pour les agents IA SaaS accédant à des données internes (LLM-as-a-service, Copilot, etc.) — en cours d'élaboration, voir groupe de travail IA de la BNSL
- Renvoi vers BNSL-ARCH-SAAS-003 (API Gateway) et BNSL-ARCH-SAAS-002 (journalisation des flux)

---

### Directives finales de génération

- Produis les 5 documents de façon **complète et séquentielle**.
- Chaque document doit être **autonome** : un architecte qui le lit sans les autres doit pouvoir l'utiliser.
- Les diagrammes Mermaid doivent être **syntaxiquement valides** et suffisamment détaillés pour être utiles.
- Là où un choix technologique n'est pas encore arrêté à la BNSL (ex. : Kong vs Apigee), le document doit le mentionner explicitement plutôt que de trancher arbitrairement.
- Les zones incomplètes doivent être signalées avec `> ⚠️ TODO :` en citation Markdown.
```
