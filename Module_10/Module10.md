# 🛡️ Module 10 : Rédiger un Rapport de Pentesting Professionnel

Ce document explique étape par étape comment écrire un bon rapport après un test d'intrusion (pentest). L'objectif est que tout le monde puisse le comprendre, du directeur d'entreprise jusqu'au développeur.

---

## À quoi sert un rapport de pentest ? (Le rôle du rapport)

Un rapport de test d’intrusion n'est pas juste une liste de problèmes informatiques. C'est un **outil pour aider l'entreprise à prendre de bonnes décisions**. 

### Pour les différentes équipes de l'entreprise :
* **Pour la direction (les chefs) :** Comprendre les dangers pour l'entreprise et décider combien d'argent (budget) il faut dépenser pour tout réparer.
* **Pour l'équipe de sécurité :** Organiser le travail pour protéger l'entreprise tous les jours et sur le long terme.
* **Pour les développeurs :** Savoir exactement où sont les erreurs dans le code et comment les corriger rapidement.
* **Pour le RSSI (Responsable de la Sécurité) :** Prouver que l'entreprise respecte bien les lois et les règles de sécurité (comme les normes ISO 27001 ou PCI-DSS pour les paiements).

### Le rôle légal (devant la loi) :
Le rapport est aussi un document officiel très important. Il peut servir de :
* **Preuve** lors d'un contrôle officiel (un audit réglementaire).
* **Document de contrat** s'il y a un désaccord (un litige) entre deux entreprises.
* **Justificatif** pour expliquer ce qu'il s'est passé si l'entreprise se fait pirater (incident de sécurité).

---

##  La structure complète d'un rapport

Un rapport professionnel doit toujours suivre un ordre logique. Voici les parties obligatoires :

### 📄 La page de garde
C'est la première page du document. Elle doit contenir :
* **Le titre :** Ex: *Rapport de test d'intrusion - Application Web*.
* **Le type de test :** Interne (depuis l'intérieur de l'entreprise), externe (depuis Internet), applicatif (sur un site web ou une app mobile), etc.
* **Le niveau de confidentialité :** Très important pour dire qui a le droit de lire le document (Ex : *Confidentiel – Diffusion limitée*).
* **Les infos sur le client :** Nom de l'entreprise qui a demandé le test.
* **Les infos sur l'équipe d'audit :** Le nom des personnes (les pentesters) qui ont fait le test.
* **La date de rédaction** et **la version du document** (ex : Version 1.0 - Brouillon, Version 2.0 - Finale).

### 📑 La table des matières
Elle est obligatoire dans un rapport officiel (souvent au format PDF). Elle permet de trouver facilement chaque partie. Elle doit être **générée automatiquement** par votre logiciel (Word, Google Docs, etc.) pour que les numéros de pages soient toujours exacts.

### 👔 Le résumé exécutif (Pour les chefs)
Cette partie est écrite pour les dirigeants qui n'ont pas le temps de lire les détails techniques. Elle doit être courte, claire et aller droit au but.
**Ce qu'il faut mettre dedans :**
* **Une note globale de sécurité :** Ex: une lettre (A, B, C, D) ou une note sur 100.
* **Le nombre de failles trouvées :** Classées par niveau de danger (Critique, Élevée, Moyenne, Faible).
* **Des images simples :** Un graphique (comme un diagramme en barres) pour voir les résultats en un coup d'œil.
* **Les gros risques pour l'entreprise (business) :** Perte d'argent, vol de données clients, arrêt du site web, etc.
* **Ce qui a été testé** (le périmètre).
* **Un bilan des bonnes pratiques :** Ce que l'entreprise fait déjà bien et ce qu'elle fait mal.

> **💡 Exemple de paragraphe professionnel :**
> *"L’application présente plusieurs failles critiques qui permettent à un pirate de contourner la page de connexion et de voler des données privées. Le niveau de sécurité actuel est trop faible pour résister à une vraie attaque. Nous conseillons fortement de corriger les problèmes les plus graves dans les 30 prochains jours."*

### 🎯 La portée (Le Scope)
C'est le cadre du test. On explique ce qu'on avait le droit de tester et ce qui était interdit.
* **La liste des cibles testées :** Sites web, serveurs, applications.
* **Les adresses IP** et les noms de domaine concernés.
* **Les environnements :** Est-ce qu'on a testé le vrai site utilisé par les clients (production) ou un site de test (staging) ? Qu'est-ce qui était exclu ?
* **Les règles et restrictions :** Par exemple, l'interdiction de faire des attaques qui bloquent le serveur (Déni de Service / DoS) pour ne pas gêner les vrais utilisateurs.
* **Les contacts chez le client :** Les personnes qui ont suivi le test.

### 🛠️ La méthodologie (Comment on a travaillé)
On explique la méthode de travail pour montrer que le test a été fait sérieusement.
* **Les règles officielles suivies (Standards) :** 
  * *PTES* (Penetration Testing Execution Standard)
  * *OWASP Testing Guide* (la référence pour tester les sites web)
* **Les types de tests réalisés (sans donner les secrets de piratage) :**
  * Tests de connexion (authentification).
  * Tests sur la gestion des sessions (les cookies, la déconnexion).
  * Tests de la configuration des serveurs.
  * Tests des droits d'accès (est-ce qu'un utilisateur normal peut voir les pages d'un administrateur ?).
  * Tests de résistance (si le client a donné son accord).
* **La protection des secrets du client (Confidentialité) :**
  * Comment les preuves sont gardées en sécurité (fichiers chiffrés).
  * La suppression des données du client une fois le rapport terminé.
  * Le masquage des informations privées dans le rapport (anonymisation).

---

## Le cœur du rapport : Les vulnérabilités (Les failles)

C'est la partie la plus longue et la plus importante pour les techniciens. Chaque faille trouvée doit avoir sa propre fiche détaillée avec les éléments suivants :

### 🏷️ Titre de la faille
Il doit être court, clair et précis.
* *Bon exemple :* "Contrôle d’accès absent sur l’API /users"
* *Bon exemple :* "Mots de passe enregistrés sans protection (en clair) dans la base de données"

### 📝 Description détaillée
Expliquez simplement le problème :
* **Le principe :** C'est quoi cette faille ?
* **L'impact technique :** Qu'est-ce qu'un pirate peut faire avec ?
* **La cause :** Pourquoi cette erreur existe ? (Ex : le développeur a oublié de vérifier les données envoyées par l'utilisateur).

### 📍 Contexte et localisation précise
Dites exactement où se trouve le problème pour que le développeur ne perde pas de temps à chercher :
* L'adresse URL exacte (ex : `https://site.com/api/profil`).
* Le paramètre qui pose problème (ex : `id_utilisateur=123`).
* L'environnement (site de test ou vrai site).

### ⚖️ Impact technique vs Impact business
Il faut toujours séparer les deux pour parler aux techniciens ET aux chefs :
* **Impact technique (pour les informaticiens) :** Le pirate peut lire toute la base de données.
* **Impact business (pour les chefs) :** L'entreprise risque une grosse amende à cause de la loi RGPD (protection des données), peut perdre de l'argent et abîmer sa réputation auprès des clients.

### 📊 Criticité (Niveau de danger)
Pour savoir quelle faille réparer en premier, on évalue sa gravité avec :
* **Le score CVSS :** Une note officielle de **0 à 10** (0 = aucun danger, 10 = danger extrême).
* **La facilité d'attaque :** Est-ce qu'un débutant peut exploiter cette faille ou faut-il être un grand expert ?
* **La facilité de réparation :** Est-ce rapide ou long à corriger ?
* **L'impact pour l'entreprise.**

### 📸 Les preuves
Il faut prouver que la faille existe vraiment. Les preuves doivent être :
* **Horodatées :** On doit voir la date et l'heure du test.
* **Anonymisées :** Il faut flouter ou cacher les vrais mots de passe, les noms des vrais clients ou les numéros de carte bancaire.
* **Expliquées :** Ne mettez jamais une image toute seule. Écrivez une phrase en dessous pour expliquer ce qu'on voit.
* *Exemples de preuves :* Captures d'écran d'un message d'erreur, extrait de code, journaux du serveur (logs).

### 🔄 Étapes de reproduction
Expliquez la logique pour refaire le test, **sans donner d'outils dangereux**.
* *Exemple :* "Le site ne vérifie pas si l'utilisateur est un administrateur quand il ouvre la page `/admin/settings`. Un utilisateur normal peut ouvrir cette page simplement en changeant son rôle dans la requête envoyée au serveur."

### 💡 Recommandations (Comment réparer)
Donnez des solutions concrètes :
* **Actions immédiates (à faire tout de suite) :** Pour bloquer le danger rapidement (ex : fermer l'accès à la page).
* **Actions à long terme :** Pour corriger le problème proprement dans le code.
* **Liens utiles :** Ajoutez des liens vers des guides officiels (comme OWASP ou l'ANSSI) pour aider les développeurs.

---

## La conclusion et les annexes

### 🏁 La conclusion
Elle aide le client à organiser son travail après votre départ :
* **L'ordre des priorités :** Quoi réparer cette semaine, ce mois-ci, et plus tard.
* **Le plan d'action en étapes** (Phase 1, Phase 2, Phase 3).
* **Une estimation du temps ou du budget** nécessaire pour les réparations.
* **Des conseils d'organisation :** Par exemple, former les développeurs à la sécurité.

### 📎 Les annexes (À la fin du rapport)
Ce sont des documents en plus pour ceux qui veulent aller plus loin :
* **Un glossaire :** Un petit dictionnaire pour expliquer les mots techniques difficiles.
* **Les logs anonymisés :** Les fichiers techniques bruts.
* **Le plan de test :** La liste de tout ce qui a été vérifié.
* **L'explication du score CVSS :** Comment les notes ont été calculées.
* **Des schémas du réseau :** Des dessins pour montrer comment les serveurs sont reliés.

---

## L'attitude du consultant (Comment écrire)

Quand vous écrivez le rapport, vous devez adopter la bonne posture :
* **Rester neutre et objectif :** Ne donnez que des faits prouvés, pas votre avis personnel.
* **Ne jamais juger :** Ne dites pas *"Le développeur a fait un travail nul"*. Dites plutôt *"Une vérification de sécurité est manquante à cet endroit"*.
* **Être clair et pédagogue :** Écrivez des phrases simples pour aider le client à comprendre et à progresser.

---

## ❌ Les erreurs fréquentes à éviter

Voici la liste des pièges classiques quand on débute :
1. **Mettre trop de blabla technique inutile :** Ne copiez-collez pas 50 pages de résultats d'outils automatiques.
2. **Oublier le résumé exécutif :** Les chefs ne liront pas le rapport s'il n'y a pas de résumé simple au début.
3. **Mettre des captures d'écran illisibles :** Si le texte sur l'image est trop petit ou flou, la preuve ne sert à rien.
4. **Accuser les équipes du client :** Votre rôle est d'aider, pas de critiquer le travail des gens.
5. **Oublier de dire comment réparer :** Trouver une faille c'est bien, expliquer comment la corriger c'est mieux.
6. **Mélanger l'impact technique et l'impact business.**
7. **Ne pas classer les failles par ordre de gravité** (mettre les plus dangereuses en premier).

---

## 📚 Ressources utiles pour voir de vrais exemples

Si vous voulez voir à quoi ressemble un vrai rapport de pentest professionnel, vous pouvez consulter ces ressources en ligne :
* **GitHub - Public Pentesting Reports :** Une grande collection de vrais rapports publiés par des entreprises de cybersécurité (`santosomar/public-pentesting-reports`).
* **Modèles de rapports (Templates) :** Recherchez `sample-penetration-testing-report-template.pdf` sur Internet pour voir la mise en page officielle.