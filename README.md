# HORS-SLA-VBA

**Monitoring email & SLA directement dans Outlook**

---

## 1. Contexte

Reprise d'un ancien projet visant à outiller le suivi des délais de traitement (SLA / hors-SLA) directement depuis Outlook, sans dépendre d'un outil tiers. Le besoin est double :

- Un usage individuel/managérial (le suivi de l'activité d'une BAL, d'une équipe).
- Un outil potentiellement réutilisable par d'autres équipes ou services de l'entreprise (template générique, adaptable par BAL).

## 2. Contrainte technique clé

| Contexte | Techno |
|---|---|
| Outlook local (client lourd, Windows) | **VBA** (macro / module Outlook, packagé en add-in installable) |
| Outlook Web (OWA) | **Impossible en VBA** → nécessite une approche par **requêtes** (Microsoft Graph API / Office Add-in JS / Power Automate) |

➡️ Le projet devra donc probablement vivre en **deux briques distinctes mais complémentaires** :
1. Un module VBA pour Outlook desktop (filtrage, classification, extraction).
2. Une brique "web" (Graph API ou Add-in Office.js) pour couvrir les utilisateurs OWA, avec idéalement un **format d'export commun** pour que les deux briques alimentent la même classification finale.

## 3. Objectif fonctionnel

Fournir un plugin Outlook (attachable / installable via le pack Office) permettant :
- de rechercher des éléments dans une ou plusieurs boîtes (BAL générique ou BAL d'équipe),
- d'appliquer des filtres rapides, simples, et adaptables,
- de trier selon les éléments déjà présents dans Outlook (dossiers, catégories, dates, expéditeurs, objets…),
- d'extraire les résultats vers Excel avec une classification pertinente,
- de mesurer les temps de traitement pour objectiver le respect ou non des SLA.

## 4. Axes de classification / tri

- **Temps de réunion** (durée des réunions liées à un sujet/ticket)
- **Temps d'évènement** (RDV, créneaux bloqués liés à une activité)
- **Type d'incident**, regroupé par objet de manière **conceptuelle** (pas de correspondance mot à mot — nécessite une logique de regroupement sémantique/mots-clés, pas juste un filtre texte strict)
- **Type de tâches**
- **Rendu par technicien** :
  - Activité par email (volume traité, envoyé, reçu)
  - **Temps de prise en compte** : délai entre la date/heure d'arrivée du mail et la première réponse (par email **ou** par création de ticket)
  - *(point à compléter — voir section 6)*

## 5. Fonctionnalités cibles du plugin

- [ ] Panneau de recherche/filtre rapide (mots-clés, expéditeur, dates, dossier)
- [ ] Filtres pré-configurés réutilisables (template par BAL)
- [ ] Moteur de classification par objet (regroupement conceptuel, pas juste mot-clé exact)
- [ ] Calcul automatique des temps de prise en charge (arrivée → 1ère réponse / création ticket)
- [ ] Export Excel structuré (feuille par axe : réunions, incidents, techniciens…)
- [ ] Installation simple (add-in packagé, pas de config manuelle lourde)
- [ ] Paramétrage par équipe/BAL sans toucher au code (fichier de config ou feuille de paramètres)

## 6. Points à clarifier avant développement

1. **Source du "ticket"** : les tickets sont créés dans quel outil (ex : ServiceNow, GLPI, Jira…) ? Y a-t-il une API/mailbox de notification pour détecter la création ?
2. **Définition du "hors-SLA"** : seuil de temps fixe ? Variable par type d'incident ? Par technicien ? Par plage horaire ouvrée ?
3. **Portée réunions/évènements** : comment les rattacher à un incident/ticket précis (objet, catégorie Outlook, ID dans le corps) ?
4. **Volume de BAL concernées** : une seule BAL de test au départ, ou plusieurs dès le départ (impact sur l'archi VBA vs Graph API) ?
5. **Distribution** : add-in signé/déployé via le centre d'administration Microsoft 365, ou macro simple partagée en fichier ?

## 7. Prochaines étapes proposées

1. Valider ce document (ajuster/compléter les sections 4 et 6).
2. Définir un **schéma de données commun** (les colonnes que devra produire aussi bien la brique VBA que la brique Web) — c'est ce qui garantit que les deux briques restent compatibles.
3. Prototyper la brique VBA sur une seule BAL de test avec filtres + export Excel basique.
4. Étudier la brique Graph API/Add-in pour OWA en parallèle.
