# LAB4_securité : Uncrackable Level 1

Ce dépôt contient la solution et la méthodologie pour le challenge de reverse engineering Android **Uncrackable Level 1** (OWASP).

##  Description du Lab
L'objectif est de trouver le "secret" caché dans l'application Android. L'application contient des protections contre le root et le debug, ainsi qu'un mécanisme de vérification de flag chiffré.

---

##  Étape 1 : Analyse Statique avec JADX
L'analyse du code source décompilé nous permet de localiser la classe de vérification. On observe que l'application utilise un algorithme AES pour comparer l'entrée utilisateur avec une chaîne stockée en dur.

<img width="1481" height="648" alt="image" src="https://github.com/user-attachments/assets/445fb788-536d-469c-a841-2ce20faf3ce4" />

On y identifie :
* La méthode de déchiffrement.
* La clé de chiffrement.
* Le texte chiffré (Base64).

---

##  Étape 2 : Déchiffrement avec CyberChef
En utilisant les éléments extraits du code, nous utilisons **CyberChef** pour effectuer les opérations suivantes :
1.  Décodage From Base64.
2.  Déchiffrement AES (en utilisant la clé identifiée).

<img width="1439" height="490" alt="image" src="https://github.com/user-attachments/assets/927846dc-1c84-469e-9234-67d9120b5627" />

Le flag est ainsi révélé en clair.

---

##  Étape 3 : Validation dans l'Application
Une fois le flag récupéré, nous le testons directement dans l'émulateur ou sur le terminal physique.

<p align="center">
<img width="357" height="340" alt="image" src="https://github.com/user-attachments/assets/75a2c12e-4f87-4f6c-8a91-ab6ce59fd766" />
<img width="357" height="668" alt="image" src="https://github.com/user-attachments/assets/2bd1a3a1-2f84-48df-ae89-f3b12973ef69" />
</p>

L'application affiche un message de succès, confirmant que le secret extrait est correct.

---

## 🛠️ Outils utilisés
* **JADX-GUI** : Décompilation de l'APK.
* **CyberChef** : Manipulation de données et cryptographie.
* **Frida** : (Optionnel) Pour bypass la détection root au lancement.
