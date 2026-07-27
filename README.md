# 🚀 SAP Fiori Elements - Gestion des Demandes de Matériel IT & Télétravail (Low-Code / Key User Extensibility)

[![SAP S/4HANA](https://img.shields.io/badge/SAP%20S%2F4HANA-Clean%20Core-008FD3?style=for-the-badge&logo=sap)](https://www.sap.com)
[![Fiori Elements](https://img.shields.io/badge/UI-SAP%20Fiori%20Elements-F0AB00?style=for-the-badge&logo=sap)](https://fiori.tools)
[![Extensibility Tier](https://img.shields.io/badge/Extensibility-Tier%201%20(In--App)-28A745?style=for-the-badge)](https://help.sap.com)
[![Agile Methodology](https://img.shields.io/badge/Project%20Management-JIRA%20%2F%20Scrum-0052CC?style=for-the-badge&logo=jira)](https://www.atlassian.com/software/jira)

Bienvenue dans le dépôt officiel du projet **"Gestion des Demandes de Matériel IT & Télétravail"**. 

Ce projet est une implémentation de référence illustrant la création d'une application professionnelle SAP Fiori de bout en bout en mode **"Less Code / Low-Code"** via la couche **Key User Extensibility (Tier 1 - In-App)** d'SAP S/4HANA, dans le respect strict du paradigme **Clean Core**.

---

## 📋 Présentation et Objectifs du Projet

Dans le cadre du passage au travail hybride (Télétravail), le service DSI et RH a besoin d'une application Fiori moderne intégrée au Launchpad permettant aux salariés de demander des équipements informatiques (Écrans 4K, Claviers ergonomiques, Casques audio, Fauteuils) avec un circuit de validation par le manager.

### 🌟 Points Forts Techniques (Pourquoi ce projet est unique) :
* ⚡ **Zéro Code ABAP Lourd :** L'intégralité du modèle de données, du service OData et de l'interface Fiori Elements a été générée automatiquement sans ouvrir Eclipse ADT.
* 🛡️ **100% Clean Core & Upgrade-Safe :** Aucun impact sur le noyau standard SAP. Résistant à 100% aux montées de version annuelles de S/4HANA.
* 🎨 **UI Adaptation :** Personnalisation de l'interface utilisateur à chaud par glisser-déposer.
* 📊 **Gestion Agile sous JIRA :** Projet découpé et piloté en Epics et User Stories avec des critères d'acceptation (DoD) rigoureux.

---

## 🏗️ Architecture Technique (Modèle 3-Tier)

```mermaid
graph TD
    subgraph "Couche Présentation (Fiori Launchpad)"
        A[Tuile Fiori : Demande Matériel IT] -->|OData V4| B(App Fiori Elements Générée)
    end
    
    subgraph "Couche Key User Extensibility (Tier 1 - In-App)"
        B --> C[Custom Business Objects : YY1_EQUIPMENTREQUEST]
        C -->|Validation & Determination| D[Custom Logic : BAdI Web ABAP Sandbox]
    end
    
    subgraph "Couche Base de Données (HANA)"
        C --> E[(Table DdIC Générée & Draft Table)]
    end
```

---

## 📥 Gestion de Projet Agile & Backlog JIRA (GDDDMRITLT)
Ce projet est entièrement découpé en tâches professionnelles (Scrum / Agile) et importé dans notre instance Jira Software (projet `GDDDMRITLT`).
* 👉 Consultez le fichier **[01_JIRA_AGILE_WORKFLOW.md](01_JIRA_AGILE_WORKFLOW.md)** pour voir le détail technique complet des 9 tickets (de `GDDDMRITLT-1` à `GDDDMRITLT-9`).

### 📸 Vue du Backlog JIRA Importé :
> **📌 Capture de la liste de nos tickets dans JIRA à ajouter dans :** `screenshots/00_jira_backlog.png`
![Backlog JIRA Importé avec succès](screenshots/00_jira_backlog.png)

---

## 📸 Guide d'Implémentation Pas-à-Pas & Portfolio de Validation

Voici les étapes techniques précises à réaliser dans votre système SAP Fiori. Dès que vous ajoutez une capture d'écran dans le dossier `screenshots/` avec le nom de fichier indiqué, elle s'affichera automatiquement ci-dessous sur votre GitHub !

### 🛠️ Étape 1 : Ouverture de l'application "Custom Business Objects" (Ticket GDDDMRITLT-2)
1. Connectez-vous à votre SAP Fiori Launchpad (via `/n/UI2/FLP`).
2. Tapez `Custom Business Objects` dans la recherche et ouvrez l'application (`F1712`).
> **📌 Image de validation à ajouter dans `screenshots/01_cbo_app_open.png` :**
![Capture 1 : Application Custom Business Objects](screenshots/01_cbo_app_open.png)

---

### 🛠️ Étape 2 : Création de l'Objet Métier `YY1_EQUIPMENTREQUEST` (Ticket GDDDMRITLT-4)
1. Cliquez sur **`+` (New)**.
2. Nom : `Equipment Request` | Identifier : `YY1_EQUIPMENTREQUEST`.
3. **Cochez les 3 cases Low-Code :** ☑️ *Determination and Validation*, ☑️ *UI Generation*, ☑️ *Service Generation*.
> **📌 Image de validation à ajouter dans `screenshots/02_cbo_properties_checked.png` :**
![Capture 2 : Configuration du Custom Business Object](screenshots/02_cbo_properties_checked.png)

---

### 🛠️ Étape 3 : Définition des Champs et Publication (Ticket GDDDMRITLT-4)
1. Ajoutez les 5 champs : `RequesterName` (Text 40), `Department` (Text 20), `EquipmentType` (Code List : `ECRAN`, `CLAVIER`, `CASQUE`), `EstimatedPrice` (Amount + Currency), `RequestStatus` (Code List : `NEW`, `APPR`, `REJ`).
2. Cliquez sur **Publish** et attendez 2 minutes que le statut passe à **Published**.
> **📌 Image de validation à ajouter dans `screenshots/03_fields_list_published.png` :**
![Capture 3 : Liste des champs publiée](screenshots/03_fields_list_published.png)

---

### 🛠️ Étape 4 : Logique Métier In-App (Ticket GDDDMRITLT-6)
1. Allez dans *Determination and Validation* ➡️ **New Determination** (*After Modification*).
2. Nom : `SetDefaultStatus`.
3. Dans l'éditeur Web ABAP Sandbox, collez :
   ```abap
   IF equipmentrequest-requeststatus IS INITIAL.
     equipmentrequest-requeststatus = 'NEW'.
   ENDIF.
   ```
4. Cliquez sur **Test** et **Publish**.
> **📌 Image de validation à ajouter dans `screenshots/04_custom_logic_editor.png` :**
![Capture 4 : Logique In-App dans l'éditeur Web](screenshots/04_custom_logic_editor.png)

---

### 🛠️ Étape 5 : Assignation au Catalogue et Découverte de la Tuile (Ticket GDDDMRITLT-9)
1. Dans l'application **Custom Catalog Extensions** (`F1484`), assignez `YY1_EQUIPMENTREQUEST` à votre catalogue utilisateur.
2. Retournez sur le Launchpad : Votre tuile est prête !
> **📌 Image de validation à ajouter dans `screenshots/05_fiori_launchpad_tile.png` :**
![Capture 5 : La tuile sur le Fiori Launchpad](screenshots/05_fiori_launchpad_tile.png)

---

### 🏁 Étape 6 : RÉSULTAT FINAL EN PRODUCTION (Recette UAT & UI Adaptation - Ticket GDDDMRITLT-7)
Voici le résultat ultime de votre développement Low-Code en fonctionnement sur le Launchpad ! L'application Fiori Elements est active, les données de test sont créées, la logique pré-remplit le statut sur `NEW` et les colonnes ont été réorganisées ergonomiquement via *Adapt UI*.

> **📌 Image finale de votre application en production à ajouter dans `screenshots/06_final_fiori_app_uat.png` :**
![Résultat Final : Application Fiori en Production](screenshots/06_final_fiori_app_uat.png)

---
*Projet réalisé et certifié 100% Clean Core Low-Code Extensibility.*
