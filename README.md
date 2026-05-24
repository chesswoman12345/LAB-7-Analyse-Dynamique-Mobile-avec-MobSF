# 🔬 LAB 7 — Analyse Dynamique Mobile avec MobSF

## 🎯 Objectif du Lab

Ce laboratoire permet de découvrir l’analyse dynamique d’applications Android avec **MobSF (Mobile Security Framework)** et **DIVA (Damn Insecure and Vulnerable App)**.

L’objectif est de :

- Configurer un émulateur Android compatible avec MobSF
- Lancer MobSF avec Docker
- Réaliser une analyse statique et dynamique d’un APK Android
- Observer le trafic réseau, les logs et les fichiers
- Utiliser Frida pour l’instrumentation dynamique
- Comprendre les vulnérabilités Android courantes

---

# 🛠️ Outils Utilisés

| Outil | Rôle |
|---|---|
| Android Studio | Création de l’émulateur Android |
| MobSF | Analyse statique et dynamique |
| Docker | Exécution de MobSF |
| ADB | Communication avec l’émulateur |
| DIVA APK | Application Android vulnérable |
| Frida | Instrumentation dynamique |

---

# 📌 Architecture du Lab

```text
DIVA APK
   ↓
Émulateur Android Rooté
   ↓
MobSF Dynamic Analyzer
   ↓
Frida + Proxy HTTPS + Logcat
   ↓
Analyse Runtime
```

---

# 📌 Étape 1 — Création de l’Émulateur Android (AVD)

Ouvrez :

```text
Android Studio → Tools → AVD Manager → Create Virtual Device
```

Choisissez un téléphone :

- Pixel 5
- Pixel 6

---

## 📌 Configuration du Système Android

Dans **System Image** :

✅ Sélectionner :

- Android API 29 ou 30
- x86_64
- Version sans Google Play

❌ Ne pas choisir :

- Google Play

---

## 📌 Nom recommandé

```text
MobSF_DIVA_API_30
```

---

## 📸 Capture — Création de l’AVD

![Création AVD](images/screen1.png)

---

# 📌 Étape 2 — Cloner MobSF

Ouvrez un terminal :

```bash
git clone https://github.com/MobSF/Mobile-Security-Framework-MobSF.git

cd Mobile-Security-Framework-MobSF
```

---

## 📸 Capture — Clone MobSF

![Clone MobSF](images/screen2.png)

---

# 📌 Étape 3 — Lancement de l’Émulateur Rooté

## 🔹 Linux / Mac

```bash
./scripts/start_avd.sh
```

## 🔹 Windows PowerShell

```powershell
scripts\start_avd.ps1
```

---

## 📌 Démarrage de l’AVD

```bash
./scripts/start_avd.sh MobSF_DIVA_API_30
```

---

## 📌 Vérification ADB

Dans un nouveau terminal :

```bash
adb devices
```

Résultat attendu :

```text
List of devices attached
emulator-5554 device
```

⚠️ Gardez cet identifiant.

---

## 📸 Capture — Vérification ADB

![ADB Devices](images/screen3.png)

---

# 📌 Étape 4 — Installation et Lancement de MobSF avec Docker

## 🔹 Télécharger l’image Docker

```bash
docker pull opensecurity/mobile-security-framework-mobsf:latest
```

---

## 🔹 Lancer MobSF

```bash
docker run -it --rm \
-p 8000:8000 \
-e MOBSF_ANALYZER_IDENTIFIER=emulator-5554 \
opensecurity/mobile-security-framework-mobsf:latest
```

⚠️ Remplacez :

```text
emulator-5554
```

par votre propre identifiant ADB.

---

# 📌 Accès à MobSF

Ouvrez dans votre navigateur :

```text
http://127.0.0.1:8000
```

---

## 🔐 Identifiants par défaut

```text
Username : mobsf
Password : mobsf
```

---

## 📸 Capture — Login MobSF

![MobSF Login](images/screen4.png)

---

# 📌 Étape 5 — Téléchargement de DIVA APK

## 🔹 Site officiel

```text
http://www.payatu.com/damn-insecure-and-vulnerable-app/
```

---

## 🔹 Alternative GitHub

```text
https://github.com/payatu/diva-android
```

---

## 📌 APK à utiliser

```text
diva.apk
```

ou

```text
DIVA-debug.apk
```

---

# 📌 Étape 6 — Analyse Statique de DIVA

Dans MobSF :

```text
Upload & Analyze → Sélectionner diva.apk
```

MobSF lance automatiquement :

- Analyse du Manifest
- Permissions Android
- Reverse Engineering
- Détection des vulnérabilités
- Analyse du code source
- Scan sécurité

---

## 📸 Capture — Upload APK

![Upload APK](images/screen5.png)

---

## 📸 Capture — Rapport Statique

![Static Analysis](images/screen6.png)

---

# 📌 Étape 7 — Analyse Dynamique

Dans le rapport MobSF :

```text
Dynamic Analysis → Start Dynamic Analyzer
```

MobSF va automatiquement :

- Installer DIVA
- Configurer le proxy HTTPS
- Installer le certificat Root CA
- Lancer Frida Server
- Connecter l’émulateur

---

## 📸 Capture — Dynamic Analyzer

![Dynamic Analyzer](images/screen7.png)

---

# 📌 Étape 8 — Utilisation de DIVA

Dans l’émulateur :

- Ouvrir DIVA
- Explorer les challenges vulnérables

Exemples :

- Insecure Logging
- Hardcoded Credentials
- Insecure Storage
- Access Control Issues
- SQL Injection
- Intent Vulnerabilities

---

## 📸 Capture — Application DIVA

![DIVA App](images/screen8.png)

---

# 📌 Étape 9 — Analyse Runtime avec MobSF

## 🔹 Runtime Logs

Permet de voir :

- Logs Android
- Erreurs
- Exceptions
- Informations sensibles

---

## 🔹 Network Traffic

Permet d’intercepter :

- HTTP
- HTTPS
- API Calls
- Requêtes réseau

Même le trafic SSL peut être inspecté.

---

## 🔹 Frida Injection

Cliquer sur :

```text
Spawn & Inject
```

Exemple :

```javascript
Java.perform(function () {
    console.log("Frida Injected");
});
```

---

## 🔹 File Monitor

Permet de voir :

- Fichiers créés
- Données sensibles
- Stockage insecure

---

## 🔹 Intent Monitor

Permet d’observer :

- Intents Android
- Activities exportées
- Communication inter-applications

---

## 📸 Capture — DIVA Runtime

![Runtime Analysis](images/screen9.png)

---

# 📌 Menu Dynamic Analyzer

| Fonction | Description |
|---|---|
| Stop Screen | Arrêter le mirroring |
| Remove Root CA | Supprimer le certificat MobSF |
| Unset HTTP(S) Proxy | Désactiver le proxy |
| TLS/SSL Security Tester | Tester SSL/TLS |
| Exported Activity Tester | Tester les activities exportées |
| Activity Tester | Lancer des activités Android |
| Get Dependencies | Voir les dépendances |
| Take a Screenshot | Capturer l’écran |
| Logcat Stream | Logs Android temps réel |
| Generate Report | Générer le rapport final |

---

# 📌 Exemple de Vulnérabilités Observées

| Vulnérabilité | Description |
|---|---|
| Insecure Storage | Données stockées en clair |
| Hardcoded Credentials | Mots de passe codés en dur |
| Exported Activities | Activities accessibles sans protection |
| Insecure Logging | Informations sensibles dans les logs |
| Weak SSL Validation | Mauvaise validation HTTPS |

---

# 📌 Résultats Obtenus

✅ Analyse statique complète  
✅ Analyse dynamique runtime  
✅ Interception HTTPS  
✅ Utilisation de Frida  
✅ Observation des logs Android  
✅ Analyse du stockage insecure  
✅ Monitoring des intents Android  

---

# 📌 Dépannage Rapide

## ❌ Dynamic Analysis Failed

Vérifier :

```bash
adb devices
```

L’émulateur doit être visible.

---

## ❌ MobSF ne détecte pas l’émulateur

Relancer :

```bash
adb kill-server
adb start-server
```

---

## ❌ Docker Connection Error

Sous Linux :

```bash
--net=host
```

---

## ❌ Émulateur lent

Utiliser :

- API 29
- x86_64

---

# 📌 Conclusion

Ce laboratoire a permis de réaliser une analyse dynamique Android complète avec MobSF.

Nous avons appris à :

- Configurer un environnement Android sécurisé
- Utiliser MobSF Dynamic Analyzer
- Intercepter le trafic HTTPS
- Utiliser Frida
- Observer les vulnérabilités runtime

MobSF constitue aujourd’hui l’un des outils les plus puissants pour l’analyse de sécurité mobile Android.

---

# 📚 Ressources Officielles

## 🔹 Documentation MobSF

```text
https://github.com/MobSF/docs
```

## 🔹 Projet DIVA

```text
https://github.com/payatu/diva-android
```

## 🔹 OWASP Mobile Security

```text
https://owasp.org/www-project-mobile-top-10/
```

---

# 👨‍💻 Auteur

```text
Nom : [Votre Nom]
Formation : Cybersécurité / Sécurité Mobile
Lab : Analyse Dynamique Android avec MobSF
```

---
