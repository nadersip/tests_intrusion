# 🔎 Module 8 : DirBuster 

> ⚠️ **Avertissement**
>
> Les exemples de scan, de reconnaissance et de connexion à des services doivent être exécutés uniquement sur des systèmes que vous possédez ou pour lesquels vous avez une autorisation explicite.
> Pour les démonstrations de cybersécurité, privilégiez un environnement de laboratoire isolé.

---

## 📖 Introduction à DirBuster

**DirBuster** est une application multi-thread développée par l'**OWASP** (*Open Web Application Security Project*). Son rôle est d'énumérer les répertoires et fichiers masqués sur un serveur HTTP/HTTPS à l'aide d'attaques par dictionnaire (*wordlists*).

---

## Prérequis et Installation sur Kali Linux

DirBuster est codé en Java et repose sur l'environnement OpenJDK de Kali.

### Mise à jour des paquets et installation

```bash
sudo apt update
sudo apt install dirbuster -y
```
---

## Dictionnaires Natifs (Wordlists)

Kali Linux installe directement les dictionnaires de référence de DirBuster dans son arborescence système :

```bash
ls -l /usr/share/wordlists/dirbuster/
```

| Fichier | Entrées | Recommandation |
| :--- | :--- | :--- |
| directory-list-2.3-small.txt | ~87 000 | Idéal pour un scan initial rapide |
| directory-list-2.3-medium.txt | ~220 000 | Standard de référence pour un audit poussé |
| directory-list-lowercase-2.3-medium.txt | ~220 000 | Cibles insensibles à la casse |

---

## Utilisation via l'Interface Graphique (GUI)

### Lancement

Depuis un terminal Kali en environnement de bureau :

```bash
dirbuster &
```

### Paramétrage pas à pas

1. **Target URL :** Saisissez l'URL complète avec le protocole et le port.
   ```text
   http://10.10.10.50:80/
   ```
2. **Work Method :**
   * Choisir `Auto Switch (HEAD and GET)` ou forcer `GET` si le serveur refuse les requêtes HEAD.
3. **Number of Threads :**
   * Réglez entre `10` et `30` pour éviter les ralentissements ou les blocages côté serveur.
4. **Scanning Options :**
   * Cochez **List based brute force**.
   * Cliquez sur **Browse** et sélectionnez :
     `/usr/share/wordlists/dirbuster/directory-list-2.3-small.txt`
5. **File Extensions :**
   * Indiquez les extensions à tester séparées par une virgule :
     ```text
     php,txt,html,bak,zip
     ```
6. **Récursion :**
   * Cochez **Be Recursive** pour forcer l'outil à sonder les sous-dossiers trouvés.
7. **Lancement :**
   * Cliquez sur **Start**.

---

## Utilisation en Ligne de Commande (CLI / Headless)


```bash
dirbuster -H -u http://10.10.10.50/ -l /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt -e php,txt -r rapport.txt
```

### Arguments clés

* `-H` : Mode headless (désactive l'affichage graphique).
* `-u` : URL cible.
* `-l` : Chemin complet vers la wordlist.
* `-e` : Liste des extensions séparées par des virgules.
* `-t` : Nombre de threads (ex. `-t 20`).
* `-r` : Fichier de sortie du rapport d'analyse.

---

## Interprétation des Résultats HTTP

| Code HTTP | État | Interprétation pour l'auditeur |
| :--- | :--- | :--- |
| **200 OK** | Page ou fichier accessible | Analyser directement le code ou la page. |
| **301 / 302** | Redirection | Dossier souvent existant (ex. `/admin` -> `/admin/`). |
| **401 / 403** | Accès non autorisé / Interdit | La ressource existe mais est restreinte (piste d'escalade / bypass). |
| **500** | Erreur serveur | Traitement applicatif défaillant sur ce point de terminaison. |

---


