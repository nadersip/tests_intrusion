# 🧪 Lab 1 : Découvrir le réseau avec Nmap

## 🎯 But du Lab

Apprendre à utiliser l'outil **Nmap** pour scanner un réseau local, trouver les ordinateurs connectés, voir leurs ports ouverts et identifier leurs systèmes.

---

## 📋 Consignes

* Répondez à chaque question dans votre document.
* **Important :** Mettez une **capture d'écran** claire de la commande et du résultat pour chaque question.
* Scannez **uniquement** les adresses IP du laboratoire du cours.

> 🔗 **Aide :** Vous pouvez voir toutes les options de Nmap sur le site officiel : [Nmap Cheat Sheet](https://www.stationx.net/nmap-cheat-sheet/)

---

## 🛠️ Questions

### Question 1 : Combien d’hôtes sont connectés au réseau ?
* **Ce qu'il faut faire :** Trouver toutes les machines allumées sur votre réseau sans scanner tous les ports.
* **Preuve demandée :** Capture d'écran montrant le nombre total de machines trouvées (*hosts up*) et leurs adresses IP.

---

### Question 2 : Quels sont les ports ouverts sur les hôtes connectés au réseau ?
* **Ce qu'il faut faire :** Scanner les machines trouvées pour voir quels ports sont ouverts.
* **Preuve demandée :** Capture d'écran de la liste des ports marqués comme `open`.

---

### Question 3 : Quelles sont les versions des applications qui tournent sur les hôtes ?
* **Ce qu'il faut faire :** Trouver le nom et le numéro de version des programmes qui utilisent les ports ouverts.
* **Preuve demandée :** Capture d'écran où la colonne **VERSION** est visible (par exemple : Apache 2.4, OpenSSH 8.2...).

---

### Question 4 : Quel est le système d’exploitation des hôtes ?
* **Ce qu'il faut faire :** Savoir si la machine cible utilise Windows, Linux ou un autre système.
* **Preuve demandée :** Capture d'écran montrant la ligne avec le système d'exploitation détecté (*Running* ou *OS details*).

---
