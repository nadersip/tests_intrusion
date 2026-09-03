# 🔑 Module 2 : Tester et trouver les mots de passe

## 📖 Introduction

Presque tout le monde utilise des mots de passe : sur les sites web, sur Windows et sur les serveurs. 

Dans ce cours, vous allez apprendre comment trouver les mots de passe faibles. Si un pirate trouve un seul mot de passe Windows, il peut souvent contrôler toute l'entreprise.

---

## 🎯 Ce que vous allez apprendre

* Voir quels mots de passe sont trop faciles à deviner.
* Comprendre la différence entre tester **sur internet (en ligne)** et tester **sur son propre PC (hors ligne)**.
* Comprendre pourquoi une carte graphique (GPU) est plus rapide qu'un processeur (CPU) pour casser un mot de passe.
* Connaître les bons outils à utiliser.

---

## ⚠️ Les mots de passe trop faciles

Beaucoup de gens utilisent de très mauvais mots de passe :

1. **Les mots de passe par défaut :** Des appareils neufs gardent le mot de passe d'usine (exemples : `admin / admin`, `cisco / cisco`). Il existe aussi parfois des mots de passe secrets laissés par le fabricant.
2. **Les mots de passe simples :** Par exemple `admin1234`, ou le nom de la machine comme `cisco1234`.
3. **Le prénom de la personne :** Par exemple, un employé nommé Alex utilise `alex1234` ou `Alex1234`.

> 📄 **Listes de mots de passe connus :**
> * [Liste SkullSecurity](http://www.skullsecurity.org/wiki/index.php/Passwords)
> * [Liste KoreLogic](http://contest-2010.korelogic.com/wordlists.html)

---

## 🌐 Où trouver des mots de passe ?

* Dans les navigateurs web (comme Firefox ou Chrome).
* En écoutant le trafic sur le réseau.
* Dans des dossiers partagés mal protégés.
* En profitant du fait que les gens mettent souvent le **même mot de passe partout**.

---

## ⚔️ Les deux manières d'attaquer

### 1. Attaque en ligne (Online)
Vous tapez les mots de passe directement sur la page de connexion du site ou du serveur.

* **Le problème :** Après 3 ou 5 faux essais, le compte se bloque.
* **La solution des pirates :** Au lieu de tester 1 000 mots de passe sur une seule personne, ils testent un seul mot de passe simple sur 3 000 personnes en même temps, puis ils attendent.

### 2. Attaque hors ligne (Offline)
Vous prenez d'abord le fichier qui contient les mots de passe cachés (les empreintes / *hashes*). Ensuite, vous travaillez chez vous sur votre propre PC.

* **L'avantage :** Aucun compte ne se bloque, et personne ne peut voir ce que vous faites.

---

## ⚡ Pourquoi utiliser la carte vidéo (GPU) ?

Pour deviner les mots de passe cachés, la carte graphique (**GPU**) est beaucoup plus rapide que le processeur (**CPU**) :

* **Processeur (CPU) :** Fait environ 300 essais par seconde.
* **Carte graphique (GPU) :** Fait environ 300 **milliards** d'essais par seconde.

| Mot de passe | Temps avec un CPU | Temps avec un GPU |
| :--- | :--- | :--- |
| **5 lettres** | Moins d'une minute | Moins d'une seconde |
| **7 lettres** | Plusieurs jours | Quelques minutes |
| **9 lettres** | 43 ans | 48 jours |

---

## 🛠️ Les outils du cours

### Pour attaquer en ligne (sur le réseau)
* **Hydra :** Très rapide pour essayer des mots de passe sur plein de serveurs.
* **Medusa :** Très bon aussi pour faire des tests en même temps.

### Pour casser hors ligne (sur son PC)
* **Hashcat :** Le meilleur outil pour utiliser la puissance de la carte graphique.
* **John the Ripper :** Un outil très connu et facile à utiliser.