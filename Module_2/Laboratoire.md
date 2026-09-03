# 🧪 Lab 2 : Casser les mots de passe de Windows XP

## 🎯 But du Lab

Prendre le contrôle d'une machine Windows XP vulnérable, extraire les mots de passe cachés (*hashs*) et les casser avec **John the Ripper**.

---

## 📋 Consignes

* Répondez à chaque question dans votre document.
* **Important :** Mettez une **capture d'écran** claire de la commande et du résultat pour chaque question.
* Scannez **uniquement** les adresses IP du laboratoire du cours.

> 🔗 **Aide :** Vous pouvez voir toutes les options de Nmap sur le site officiel : [Nmap Cheat Sheet](https://www.stationx.net/nmap-cheat-sheet/)

---

## 🛠️ Étapes à suivre

### Étape 1 : Scanner la cible

* Trouvez l'adresse IP de la machine Windows XP.
* Vérifiez que le port **445 (SMB)** est bien ouvert.
![screenshot](../images/2.png)
---

### Étape 2 : Lancer l'attaque avec Metasploit

* Lancer le terminal sur la machine kali.
* Lancer l'outil msfconsole

