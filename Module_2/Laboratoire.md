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

---

### Étape 2 : Lancer l'attaque avec Metasploit

* Lancer le terminal sur la machine kali.
* Lancer l'outil msfconsole


---

### Question 3 : Quelles sont les versions des applications qui tournent sur les hôtes ?
* **Ce qu'il faut faire :** Trouver le nom et le numéro de version des programmes qui utilisent les ports ouverts.
* **Preuve demandée :** Capture d'écran où la colonne **VERSION** est visible (par exemple : Apache 2.4, OpenSSH 8.2...).

---

### Question 4 : Quel est le système d’exploitation des hôtes ?
* **Ce qu'il faut faire :** Savoir si la machine cible utilise Windows, Linux ou un autre système.
* **Preuve demandée :** Capture d'écran montrant la ligne avec le système d'exploitation détecté (*Running* ou *OS details*).

---
