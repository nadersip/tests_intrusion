# 🛡️ Cours de Test d'Intrusion (Pentesting)

## 📖 Introduction

Ce cours a pour but de vous apprendre comment tester la sécurité d'un système informatique comme le font les professionnels.

Vous allez apprendre toutes les étapes pour trouver et exploiter les failles de sécurité, du début à la fin, tout en respectant la loi. Vous apprendrez à devenir des hackers éthiques (**White Hats**).

---

## 🎯 Ce que vous allez apprendre

* Comprendre pourquoi le test d'intrusion est important pour protéger les systèmes.
* Maîtriser les étapes d'un test d'intrusion :
  1. **Planification** (définir les règles et les limites)
  2. **Reconnaissance** (chercher des informations sur la cible)
  3. **Scan et analyse** (trouver les failles)
  4. **Exploitation** (utiliser la faille pour entrer)
  5. **Maintien de l'accès** (rester dans le système)
  6. **Rapport** (expliquer le problème et comment le corriger)

---

## 🛠️ Les outils utilisés

Nous allons utiliser les outils standards du domaine :

* **Nmap** : pour scanner le réseau
* **Metasploit** : pour tester les failles de sécurité
* **Burp Suite** : pour tester la sécurité des sites web
* **John the Ripper** : pour tester la force des mots de passe

> ⚠️ **Important :** Vous pouvez vous entraîner avec ces outils chez vous, mais **uniquement sur vos propres machines ou dans les laboratoires du cours**. Ne testez jamais un système sans avoir la permission écrite !

---

## ⚖️ Règle d'or : La Permission

Avant de tester un ordinateur ou un réseau, vous devez **toujours avoir l'autorisation écrite** du propriétaire. Sans cette autorisation, ce que vous faites est illégal et considéré comme du piratage.

### Pourquoi demander la permission ?
* **Respecter la loi :** Entrer dans un système sans accord est un crime.
* **Être professionnel :** Un bon hacker éthique respecte toujours les règles.
* **Éviter la casse :** Certains tests peuvent faire planter des serveurs ou effacer des données.
* **Garder la confiance :** Travailler honnêtement avec son client.

---

## Les lois au Canada (*Code Criminel*)

Si vous piratez un système sans autorisation, voici ce que vous risquez selon la loi canadienne :

| Infraction | Article de loi | Peine maximale |
| :--- | :--- | :--- |
| **Accès non autorisé à un ordinateur** | Art. 342.1 | Jusqu'à 10 ans de prison |
| **Bloquer ou détruire des données** | Art. 430(1.1) | Jusqu'à 10 ans de prison (prison à vie si danger pour la vie) |
| **Intercepter des communications privées** | Art. 184 | Jusqu'à 5 ans de prison |
| **Posséder ou distribuer des outils de piratage illégalement** | Art. 342.2 | Jusqu'à 2 ans de prison + saisie du matériel |
| **Fraude** | Art. 380(1) | Jusqu'à 14 ans de prison (si plus de 5 000 $) |
| **Vol ou usurpation d'identité** | Art. 402.2 et 403 | De 5 à 10 ans de prison |

## 🔍 Module : La Reconnaissance (Passive vs Active)

La reconnaissance consiste à collecter des informations sur la cible avant de lancer une attaque. Il existe deux grandes méthodes :

### 1. Reconnaissance Passive (OSINT)
Collecter des informations **sans toucher directement** aux systèmes de la cible.

* **But :** Rester 100 % discret et ne pas alerter la cible.
* **Sources utilisées :**
  * Moteurs de recherche (Google, Bing, DuckDuckGo)
  * Réseaux sociaux (LinkedIn, Twitter/X, Facebook)
  * Registres WHOIS et enregistrements DNS
  * Archives web (ex. : *Wayback Machine*)
  * Fuites de données publiques (leaks) et outils comme **OSINT Framework**
* **Exemple :** Trouver les adresses IP d'une entreprise via WHOIS, ou lister les employés sur LinkedIn pour préparer du phishing.
* **Avantage :** Discrétion maximale (invisible pour la cible).
* **Limite :** Les informations peuvent être anciennes ou incomplètes.

https://osintframework.com/

### 2. Reconnaissance Active
Collecter des informations en envoyant des requêtes directes à la cible.

* **But :** Savoir exactement quelles machines, quels ports et quels services sont allumés.
* **Techniques utilisées :**
  * Scan de ports
  * Détection de machines actives
  * Récupération de bannières pour connaître les versions des logiciels
  * Scanners de vulnérabilités
* **Exemple :** Lancer la commande nmap -sV -p- 192.168.1.10 pour voir tous les ports ouverts et leurs versions.
* **Avantage :** Informations très précises et directement utilisables pour l'attaque.
* **Inconvénient :** Facilement détectable par les systèmes de sécurité (pare-feu, logs, IDS/IPS).