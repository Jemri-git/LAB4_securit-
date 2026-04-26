# Rapport d'analyse statique - Uncrackable Level 1

## A) Informations générales
- **Titre :** Analyse statique de Uncrackable-Level1.apk
- **Date d'analyse :** 26 Avril 2026
- **Analyste :** IKHERAZEN Maryam
- **APK analysé :** Uncrackable-Level1.apk
- **Version :** 1.0
- **Provenance :** OWASP Mobile Security Testing Guide (MSTG)
- **Outils utilisés :** JADX GUI v1.5.0, CyberChef

---

## B) Résumé exécutif
Cette analyse statique a révélé **3** vulnérabilités potentielles dans l'application Uncrackable Level 1.
Les principales préoccupations concernent le **chiffrement de secrets avec des clés statiques**, la **détection de root contournable** et l'**exposition de la logique métier** via la décompilation.
Le niveau de risque global est évalué comme **Élevé**.

**Actions prioritaires recommandées :**
1. Obfusquer le code source pour rendre la décompilation plus difficile.
2. Implémenter un stockage de clés sécurisé (Android Keystore System) au lieu de clés codées en dur.
3. Renforcer les mécanismes d'auto-protection (anti-tampering et anti-root).

---

## C) Constats détaillés

### Constat #1 : Stockage de secret avec clé statique
**Sévérité :** Élevée  
**Description :** Le flag est chiffré en AES, mais la clé de déchiffrement ainsi que la chaîne chiffrée sont stockées en clair dans le code Java.  
**Localisation :** Classe `sg.vantagepoint.uncrackable1.a`, méthode `a.a`  
**Impact potentiel :** Un attaquant peut extraire le secret sans exécuter l'application en utilisant simplement un décompileur.  
**Remédiation recommandée :** Ne jamais stocker de clés de chiffrement en dur dans le code. Utiliser des mécanismes de dérivation de clés ou le Keystore.

![Le code dans jadx](https://github.com/user-attachments/assets/445fb788-536d-469c-a841-2ce20faf3ce4)

### Constat #2 : Détection de Root contournable
**Sévérité :** Moyenne  
**Description :** L'application vérifie la présence de fichiers binaires "su" ou de tags "test-keys". Cette logique est centralisée et peut être neutralisée par instrumentation dynamique.  
**Localisation :** Classe `sg.vantagepoint.a.c`, méthodes `a`, `b` et `c`  
**Impact potentiel :** L'application peut être exécutée sur des appareils compromis, facilitant l'analyse dynamique.  
**Remédiation recommandée :** Utiliser des API de confiance comme Google Play Integrity pour une vérification plus robuste.

### Constat #3 : Absence d'obfuscation
**Sévérité :** Faible  
**Description :** Le code source est parfaitement lisible après décompilation (noms de classes et de variables explicites).  
**Localisation :** Ensemble de l'APK.  
**Impact potentiel :** Facilite grandement le reverse engineering et la compréhension de la logique de sécurité.  
**Remédiation recommandée :** Activer ProGuard ou R8 dans le pipeline de compilation.

---

## D) Annexes
L'analyse du code source décompilé nous permet de localiser la classe de vérification. On observe que l'application utilise un algorithme AES pour comparer l'entrée utilisateur avec une chaîne stockée en dur.

<img width="1481" height="648" alt="image" src="https://github.com/user-attachments/assets/445fb788-536d-469c-a841-2ce20faf3ce4" />
### Validation du Flag (CyberChef)
Le déchiffrement a été validé à l'aide de CyberChef en utilisant la clé extraite de JADX.
<img width="1439" height="490" alt="image" src="https://github.com/user-attachments/assets/927846dc-1c84-469e-9234-67d9120b5627" />

### Test de succès dans l'application
Le flag trouvé a été injecté avec succès dans l'application.
<p align="center">
<img width="357" height="340" alt="image" src="https://github.com/user-attachments/assets/75a2c12e-4f87-4f6c-8a91-ab6ce59fd766" />
<img width="357" height="668" alt="image" src="https://github.com/user-attachments/assets/2bd1a3a1-2f84-48df-ae89-f3b12973ef69" />
</p>
### Permissions demandées
- `android.permission.INTERNET`
