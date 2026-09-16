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

* créer une connexion TCP ;
* écouter sur un port ;
* communiquer en UDP ;
* tester la disponibilité d'un port ;
* transférer des données ;
* transférer un fichier dans un environnement de laboratoire ;
* récupérer la bannière d'un service ;
* simuler un service réseau simple ;
* effectuer certains tests de dépannage réseau.

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

## Ncat

**Ncat** est une implémentation moderne fournie avec Nmap.

Elle ajoute notamment des fonctionnalités comme :

* TLS ;
* proxy ;
* IPv6 ;
* authentification ;
* fonctionnalités avancées de connexion réseau.

---

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

### Objectif pédagogique

Le banner grabbing permet de comprendre :

* comment fonctionnent les services réseau ;
* quelles informations sont exposées ;
* comment un attaquant pourrait identifier un service ;
* pourquoi il est important de limiter les informations fournies par les services.

> 🔐 Cette technique doit être réalisée uniquement sur des systèmes autorisés.

---

# 🔐 Bonnes pratiques et sécurité

Netcat est extrêmement simple, mais cette simplicité implique également certains risques.

## Pas de chiffrement par défaut

Une connexion Netcat classique ne chiffre pas les données.

```text
Client ────────────────> Serveur
          Données en clair
```

Il ne faut donc pas transmettre de :

* mots de passe ;
* clés privées ;
* informations personnelles ;
* tokens ;
* secrets ;
* données sensibles.

Pour les communications nécessitant du chiffrement, utiliser un protocole conçu pour cela, comme SSH ou TLS.

---

## Attention au mode `-e`

Certaines variantes de Netcat proposent :

```bash
-e <programme>
```

Cette option peut permettre de lancer automatiquement un programme lorsqu'une connexion est établie.

Dans certains scénarios, cela peut être utilisé pour créer un shell distant.

> ⚠️ Cette fonctionnalité est particulièrement sensible et n'est pas disponible dans toutes les versions de Netcat, notamment dans OpenBSD Netcat.

Elle doit être étudiée uniquement dans un **laboratoire isolé et autorisé**.

---

# 🛡️ Détection et mitigation

Les équipes de sécurité peuvent rechercher certains comportements associés à l'utilisation de Netcat.

## Surveillance réseau

Surveiller notamment :

* les connexions vers des ports inhabituels ;
* les connexions entrantes inattendues ;
* les connexions persistantes ;
* les transferts de données inhabituels ;
* les communications entre machines qui ne devraient pas communiquer.

## Endpoint / processus

Sur les systèmes Linux, surveiller l'exécution de processus tels que :

```text
nc
netcat
ncat
```

ainsi que les connexions réseau associées.

Des outils comme :

```bash
ss -tulpn
```

peuvent aider à identifier les ports et processus en écoute.

Exemple :

```bash
ss -lntup
```

---

# 🧱 Hardening

Quelques bonnes pratiques :

* utiliser un pare-feu ;
* limiter les ports exposés ;
* appliquer le principe du moindre privilège ;
* segmenter les réseaux ;
* désactiver les services inutiles ;
* surveiller les connexions sortantes ;
* utiliser TLS/SSH lorsque le chiffrement est nécessaire ;
* surveiller les outils réseau inhabituels sur les endpoints.

---

# 🐛 Dépannage

## Vérifier si un port est en écoute

Sur Linux :

```bash
ss -lnt
```

Pour obtenir les processus :

```bash
sudo ss -lntup
```

---

## Vérifier la connectivité

Tester une connexion :

```bash
nc -v <IP> <PORT>
```

Exemple :

```bash
nc -v 192.168.1.10 8080
```

---

## Tester avec un timeout

```bash
nc -v -w 5 <IP> <PORT>
```

Le délai d'attente est ici de 5 secondes.

---

## Problème de résolution DNS

Utiliser :

```bash
nc -n <IP> <PORT>
```

L'option `-n` empêche la résolution DNS.

---

# 🔄 Outils alternatifs

## Ncat

Ncat fait partie de l'écosystème Nmap et propose des fonctionnalités supplémentaires telles que TLS et les proxys.

---

## Socat

`Socat` est particulièrement puissant pour :

* les redirections ;
* les tunnels ;
* les proxys ;
* les connexions entre différents types de flux.

Il est plus complexe que Netcat, mais également beaucoup plus flexible.

---

## Nmap

Nmap est préférable lorsque l'objectif est l'analyse réseau et la reconnaissance de services.

Exemple :

```bash
nmap <IP>
```

> Netcat et Nmap ne sont pas des outils concurrents directs : ils répondent à des besoins différents.

---

# 🧑‍🔬 Démonstration en laboratoire

Une démonstration simple peut être réalisée avec deux machines virtuelles.

## Topologie

```text
              LABORATOIRE
        ┌─────────────────────┐
        │                     │
        │     Réseau isolé    │
        │                     │
        └──────────┬──────────┘
                   │
          ┌────────┴────────┐
          │                 │
     ┌────▼────┐       ┌────▼────┐
     │Machine A│       │Machine B│
     │ Client  │       │ Serveur │
     │   nc    │       │   nc    │
     └─────────┘       └─────────┘
```

## Étape 1 — Identifier les adresses IP

Sur chaque machine :

```bash
ip addr
```

ou :

```bash
ip a
```

---

## Étape 2 — Démarrer le serveur

Sur Machine B :

```bash
nc -l 12345
```

---

## Étape 3 — Se connecter au serveur

Sur Machine A :

```bash
nc <IP_MACHINE_B> 12345
```

---

## Étape 4 — Tester la communication

Machine A :

```text
Hello from Machine A
```

Machine B devrait recevoir :

```text
Hello from Machine A
```

Répondre depuis Machine B :

```text
Hello from Machine B
```

---

## Étape 5 — Tester le transfert de fichier

Sur Machine B :

```bash
nc -l 12345 > received.txt
```

Sur Machine A :

```bash
nc <IP_MACHINE_B> 12345 < test.txt
```

Vérifier ensuite :

```bash
cat received.txt
```

---

## Étape 6 — Tester un port

Depuis Machine A :

```bash
nc -z -v <IP_MACHINE_B> 12345
```

Cette commande permet de vérifier si le port est accessible.

---

# 📚 Résumé des commandes

| Objectif                      | Commande                   |
| ----------------------------- | -------------------------- |
| Connexion TCP                 | `nc <IP> <PORT>`           |
| Écoute TCP                    | `nc -l <PORT>`             |
| Connexion UDP                 | `nc -u <IP> <PORT>`        |
| Écoute UDP                    | `nc -u -l <PORT>`          |
| Mode verbeux                  | `nc -v <IP> <PORT>`        |
| Pas de résolution DNS         | `nc -n <IP> <PORT>`        |
| Timeout                       | `nc -w 5 <IP> <PORT>`      |
| Scan d'un port                | `nc -z -v <IP> <PORT>`     |
| Scan d'une plage              | `nc -z -v <IP> 20-1024`    |
| Réception d'un fichier        | `nc -l <PORT> > fichier`   |
| Envoi d'un fichier            | `nc <IP> <PORT> < fichier` |
| Vérification des ports locaux | `ss -lntup`                |

---

# 📝 Points importants à retenir

1. **Netcat permet de créer des connexions TCP et UDP.**
2. Il peut fonctionner comme **client ou serveur**.
3. Il peut être utilisé pour le **dépannage réseau**.
4. Il peut servir à effectuer des **tests simples de ports**.
5. Il peut être utilisé pour des **transferts de fichiers en laboratoire**.
6. Il peut récupérer certaines **bannières de services**.
7. Netcat classique **ne chiffre pas les communications**.
8. Les options disponibles varient selon la version installée.
9. Netcat n'est **pas un remplacement de Nmap** pour une analyse réseau complète.
10. Les fonctionnalités permettant d'exécuter des programmes doivent être manipulées avec une grande prudence.
11. Les tests de sécurité doivent toujours être effectués sur des systèmes **autorisés**.

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

### Vidéo

Ressource vidéo sur Netcat :

[Vidéo YouTube — Netcat](https://www.youtube.com/watch?v=bXCeFPNWjsM&t=518s&utm_source=chatgpt.com)

---

# 🎓 Objectifs pédagogiques du laboratoire

À la fin de cette activité, l'étudiant devrait être capable de :

* expliquer le rôle de Netcat ;
* différencier TCP et UDP ;
* établir une connexion client/serveur ;
* mettre un système en écoute ;
* tester l'accessibilité d'un port ;
* transférer des données dans un environnement contrôlé ;
* récupérer la bannière d'un service ;
* identifier les risques liés aux communications non chiffrées ;
* expliquer pourquoi les outils réseau doivent être utilisés dans un environnement autorisé ;
* comparer Netcat avec Nmap, Ncat et Socat.

---

## ⚠️ Rappel de sécurité

**Netcat est un outil légitime d'administration, de dépannage et de cybersécurité.**

Cependant, certaines de ses fonctionnalités peuvent être détournées à des fins malveillantes. Dans le cadre de ce laboratoire, toutes les manipulations doivent être réalisées sur les machines virtuelles et réseaux prévus à cet effet.

**Ne scannez pas et ne vous connectez pas à des systèmes tiers sans autorisation.**
