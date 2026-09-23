# 🗡️ Module 3 : Netcat — Le couteau suisse du réseau

## 📖 Introduction
Netcat (`nc`) est un utilitaire réseau en ligne de commande permettant d'établir des connexions **TCP et UDP**. Grâce à sa simplicité et à sa polyvalence, il est couramment utilisé pour le dépannage réseau, les tests de connectivité, le transfert de données et les laboratoires de cybersécurité.

> ⚠️ **Avertissement**
>
> Les exemples de scan, de reconnaissance et de connexion à des services doivent être exécutés uniquement sur des systèmes que vous possédez ou pour lesquels vous avez une autorisation explicite.
> Pour les démonstrations de cybersécurité, privilégiez un environnement de laboratoire isolé.

---

# 📖 Présentation et historique

Netcat est un utilitaire réseau conçu pour effectuer des connexions réseau directement depuis un terminal.

Il est souvent surnommé :

> **« Le couteau suisse du réseau »**

Cette appellation vient de sa polyvalence. Avec une seule commande, il est possible de :

* Créer une connexion TCP.
* Écouter sur un port. 
* Communiquer en UDP. 
* Tester la disponibilité d'un port.
* Transférer des données. 
* Transférer un fichier dans un environnement de laboratoire.
* Récupérer la bannière d'un service.
* Simuler un service réseau simple.
* Effectuer certains tests de dépannage réseau.

Netcat est particulièrement intéressant dans les environnements Linux et les laboratoires de réseautique/cybersécurité parce qu'il est léger et fonctionne directement depuis la ligne de commande.

---

# 🔎 Qu'est-ce que Netcat ?

Netcat est un outil permettant de créer des connexions réseau sur des ports TCP ou UDP.

Son fonctionnement peut être résumé ainsi :

```text
┌──────────────┐                       ┌──────────────┐
│   Machine A  │                       │   Machine B  │
│              │      TCP / UDP        │              │
│   nc client  │ <-------------------> │   nc listen  │
│              │                       │              │
└──────────────┘                       └──────────────┘
```

Netcat peut fonctionner comme :

* **Client** : se connecte à un serveur.
* **Serveur** : écoute les connexions entrantes.
* **Émetteur** : envoie des données.
* **Récepteur** : reçoit des données.

---

# 💻 Installation

## Debian / Ubuntu

Pour installer la version OpenBSD :

```bash
sudo apt update
sudo apt install netcat-openbsd
```

Vérifier l'installation :

```bash
nc -h
```

ou :

```bash
nc --help
```

Selon la distribution, le paquet peut également être appelé :

```bash
netcat
```

ou

```bash
netcat-traditional
```

### Vérifier la version installée

```bash
nc -h
```

Les options disponibles peuvent varier selon la version installée.

---

# 🔀 Variantes de Netcat

Il existe plusieurs implémentations de Netcat.

## Netcat OpenBSD

Couramment disponible sur les distributions Linux modernes.

Installation :

```bash
sudo apt install netcat-openbsd
```

Cette version est généralement recommandée pour les systèmes modernes.

## Netcat Traditional

Une ancienne implémentation proposant certaines options historiques qui ne sont pas nécessairement présentes dans OpenBSD Netcat.

```bash
sudo apt install netcat-traditional
```

# 🧩 Syntaxe de base

La syntaxe générale dépend de l'implémentation, mais on retrouve généralement :

```bash
nc [options] <hôte> <port>
```

Exemple :

```bash
nc 192.168.1.10 80
```

---

# ⚙️ Options courantes

| Option | Description                                                                           |
| ------ | ------------------------------------------------------------------------------------- |
| `-l`   | Mode écoute (*listen*)                                                                |
| `-u`   | Utiliser UDP au lieu de TCP                                                           |
| `-v`   | Mode verbeux (*verbose*)                                                              |
| `-z`   | Scanner sans envoyer de données                                                       |
| `-w`   | Définir un délai d'attente                                                            |
| `-n`   | Ne pas effectuer de résolution DNS                                                    |
| `-p`   | Définir le port local dans les implémentations qui le supportent                      |
| `-e`   | Exécuter un programme après connexion — disponible seulement dans certaines variantes |

> ⚠️ **Important :** les options de Netcat ne sont pas identiques entre `netcat-openbsd`, `netcat-traditional` et `ncat`. Toujours consulter `nc -h` sur la machine utilisée.

---

# 🧪 Exemples pratiques

## 1. Client TCP simple

### Objectif

Se connecter à un service TCP et échanger des données texte.

```bash
nc example.com 80
```

Pour un test HTTP simple, on peut envoyer :

```http
GET / HTTP/1.1
Host: example.com
```

Puis terminer la requête avec une ligne vide.

### Utilisation

Cet exemple permet de comprendre qu'une application réseau peut être utilisée directement via une connexion TCP.

---

# 2. Mode écoute

### Objectif

Mettre une machine en attente d'une connexion entrante.

Sur la machine serveur :

```bash
nc -l 12345
```

Sur la machine cliente :

```bash
nc <IP_SERVEUR> 12345
```

Tout texte saisi sur le client peut alors être transmis au serveur.

Exemple :

```text
Client                         Serveur
  │                               │
  │──── "Hello Server" ─────────>│
  │                               │
  │<──── "Hello Client" ─────────│
```

### Utilisation en laboratoire

Cette technique permet de :

* tester une connectivité TCP ;
* vérifier un pare-feu ;
* simuler un service réseau ;
* observer une communication client/serveur.

> 💡 Avec certaines anciennes versions de Netcat, la syntaxe peut être `nc -l -p 12345`. Avec OpenBSD Netcat, `nc -l 12345` est généralement utilisé.

---

# 3. Communication UDP

### Serveur

```bash
nc -u -l 12345
```

### Client

```bash
nc -u <IP_SERVEUR> 12345
```

Le mode UDP peut être utilisé pour démontrer les différences entre TCP et UDP.

> ⚠️ Contrairement à TCP, UDP ne fournit pas les mêmes mécanismes de connexion, de retransmission et de livraison fiable.

---

# 4. Transfert de fichier

Netcat peut également être utilisé pour transférer des données entre deux machines.

> ⚠️ Cette méthode ne fournit **aucun chiffrement ni authentification**. Elle doit donc être réservée à un environnement de laboratoire ou à un réseau de confiance.

### Machine B — Réception

```bash
nc -l 12345 > fichier_recu.bin
```

### Machine A — Envoi

```bash
nc <IP_MACHINE_B> 12345 < fichier.bin
```

Flux de données :

```text
Machine A                         Machine B
──────────                        ──────────

fichier.bin
    │
    │
    └────── TCP 12345 ────────────────> fichier_recu.bin
```

### Vérification

Après le transfert, il est possible de comparer les fichiers :

```bash
sha256sum fichier.bin
sha256sum fichier_recu.bin
```

Si les deux valeurs SHA-256 sont identiques, les fichiers sont identiques.

---

# 5. Scan de ports

Netcat peut effectuer un scan très simple de ports.

Exemple :

```bash
nc -z -v <cible> 20-1024
```

### Signification

* `-z` → ne pas envoyer de données ;
* `-v` → afficher davantage d'informations ;
* `20-1024` → analyser la plage de ports 20 à 1024.

Pour éviter la résolution DNS :

```bash
nc -n -z -v <IP> 20-1024
```

### Exemple

```bash
nc -n -z -v 192.168.1.10 20-100
```

### Limitation

Netcat peut effectuer des vérifications simples, mais il ne remplace pas un outil spécialisé comme **Nmap**.

Nmap permet notamment :

* la détection de services ;
* la détection de versions ;
* différents types de scans ;
* la détection du système d'exploitation ;
* l'utilisation de scripts NSE.

---

# 6. Banner grabbing

Le *banner grabbing* consiste à récupérer les informations qu'un service réseau expose lorsqu'une connexion est établie.

Par exemple, certains services SMTP peuvent envoyer une bannière immédiatement après la connexion.

```bash
nc -v <cible> 25
```

Le service peut alors retourner une information permettant d'identifier le service.


## Mode `-e`

Certaines variantes de Netcat proposent :

```bash
-e <programme>
```

Cette option peut permettre de lancer automatiquement un programme lorsqu'une connexion est établie.

Dans certains scénarios, cela peut être utilisé pour créer un shell distant.

> ⚠️ Cette fonctionnalité est particulièrement sensible et n'est pas disponible dans toutes les versions de Netcat, notamment dans OpenBSD Netcat.

Elle doit être étudiée uniquement dans un **laboratoire isolé et autorisé**.

---



# 📖 Ressources

### Documentation locale

La documentation installée sur Linux peut généralement être consultée avec :

```bash
man nc
```

ou :

```bash
nc -h
```

### Netcat Cheat Sheet

Black Hills Information Security :

[Netcat Cheat Sheet — Black Hills Information Security](https://www.blackhillsinfosec.com/netcat-cheatsheet/?utm_source=chatgpt.com)
