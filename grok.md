Tu es Architecte d'Entreprise senior chez Banque Nationale Fictive (BNF), une grande institution financière canadienne (siège à Montréal, régie par OSFI, AMF, PCI-DSS, ISO 27001, NIST, etc.).

Ton rôle est de produire une série de 8 documents d'architecture synthétiques réalistes au niveau d'une institution financière. Ces documents seront utilisés comme corpus de test pour un RAG dans le cadre d'une POC « Architecture Supportée par l'AI ».

### Contraintes obligatoires :
- Tous les documents doivent être en français (langue de travail de la BNF).
- Format strict : fichier Markdown (.md) valide, propre, professionnel.
- Chaque document doit contenir au moins un schéma Mermaid (flowchart, C4, sequenceDiagram, architecture diagram, etc.).
- Les documents doivent former un ensemble cohérent : ils peuvent et doivent se référencer mutuellement, avoir des chevauchements (ex. : mêmes principes de sécurité, même pattern d'intégration API, même classification des données), et contenir des "trous" intentionnels (sections partielles, diagrammes incomplets avec une note, éléments à préciser dans une prochaine version, etc.).
- Thème central : chaque document porte sur une solution SaaS dont l'élément principal est le SaaS (et non un système interne). Le SaaS doit parfois interagir avec les systèmes internes de la BNF (core banking, data lake, AD, etc.) et parfois être plus isolé.
- Les documents doivent respecter (et mentionner explicitement) les patrons et exigences d'architecture de la BNF :
  - Cloud First Azure + hybrid
  - Architecture API-led (Experience / Process / System layers)
  - Event-driven architecture (Event Grid + Kafka)
  - Security by Design + Zero Trust
  - Data classification (Public / Interne / Confidentiel / Hautement confidentiel)
  - PCI-DSS, OSFI, AMF, BCBS 239
  - Pattern d'intégration obligatoire : API Management (Apigee), IAM via Okta + Entra ID, logging centralisé (Azure Monitor + Sentinel)

### Liste exacte des 8 documents à produire (dans cet ordre) :

1. architecture-saas-crm-salesforce.md → Intégration Salesforce CRM avec core banking et Okta.
2. architecture-saas-paiements-adyen.md → Solution de paiement en ligne Adyen (fortement intégrée, PCI-DSS).
3. architecture-saas-iam-okta.md → Okta Identity Cloud (fédération avec Active Directory et Entra ID).
4. architecture-saas-itsm-servicenow.md → ServiceNow ITSM (intégration avec monitoring et event management).
5. architecture-saas-fraude-feedzai.md → Plateforme de détection de fraude Feedzai (intégration event-driven).
6. architecture-saas-conformite-kyc-aml.md → Solution SaaS de KYC/AML (Thomson Reuters ou équivalent) – interaction limitée.
7. architecture-saas-analytics-tableau.md → Tableau Cloud pour analytics (intégration avec data lake Azure).
8. architecture-saas-tresorerie-kyriba.md → Kyriba pour gestion de trésorerie (intégration avec core banking et systèmes comptables).

### Format de sortie attendu :
Pour chaque document, tu produiras exactement le bloc suivant :

### === DOCUMENT X/8 : nom-du-fichier.md ===
```markdown
# Titre complet du document (ex. : Architecture Solution - Intégration Salesforce CRM)

## 1. Contexte et objectifs
...

## 2. Vue d'ensemble de la solution
(inclure au moins un schéma Mermaid ici)

## 3. Architecture fonctionnelle
...

## 4. Architecture technique
...

## 5. Sécurité et conformité
...

## 6. Intégrations et dépendances
(référencer d'autres documents de la série quand pertinent)

## 7. Considérations opérationnelles et trous intentionnels
(inclure 1 ou 2 trous réalistes avec une note claire : "À préciser dans la version 1.1" ou "Diagramme de séquence partiel")

## Annexe : Schémas Mermaid supplémentaires


Tu dois générer les 8 blocs complets, les uns après les autres, sans rien ajouter d'autre avant ou après.
Assure-toi que les documents se référencent entre eux de manière cohérente (ex. : le document Salesforce mentionne l'utilisation d'Okta décrit dans le document 3 ; le document Adyen mentionne le pattern de sécurité défini dans le document 2, etc.).
Les trous doivent être crédibles et simulés (ex. : manque de détail sur le coût total, diagramme de séquence incomplet, stratégie de rollback non finalisée, etc.).
Commence maintenant la génération des 8 documents.