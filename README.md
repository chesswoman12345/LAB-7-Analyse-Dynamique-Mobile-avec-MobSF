# 🔬 LAB 7 — Analyse Dynamique Mobile avec MobSF


---

# 📌 Introduction

Ce laboratoire présente l’analyse dynamique d’applications Android avec :

- **MobSF (Mobile Security Framework)**
- **DIVA (Damn Insecure and Vulnerable App)**

Le but est de :

✅ Configurer un environnement Android rooté  
✅ Lancer MobSF avec Docker  
✅ Réaliser une analyse statique et dynamique  
✅ Intercepter le trafic HTTPS  
✅ Observer les logs Android  
✅ Utiliser Frida pour l’instrumentation dynamique  

---

# 🛠️ Outils Utilisés

| Outil | Description |
|---|---|
| Android Studio | Création de l’émulateur Android |
| MobSF | Analyse mobile |
| Docker | Exécution de MobSF |
| ADB | Communication Android |
| Frida | Instrumentation dynamique |
| DIVA APK | Application Android vulnérable |

---

# 📂 Structure du Projet

```text
LAB-7-Analyse-Dynamique-Mobile-avec-MobSF/
│
├── README.md
├── screen1.png
├── screen2.png
├── screen3.png
├── screen4.png
├── screen5.png
├── screen6.png
├── screen7.png
├── screen8.png
└── screen9.png
```

⚠️ IMPORTANT :

Les images doivent être dans le même dossier que le README.

Sinon GitHub n’affichera pas les captures.

---

# 📌 Étape 1 — Création de l’Émulateur Android (AVD)

Ouvrir :

```text
Android Studio → Tools → AVD Manager → Create Virtual Device
```

Choisir :

- Pixel 5
- Pixel 6

---

## 📌 Configuration Android

Dans **System Image** :

✅ Android API 29 ou 30  
✅ x86_64  
✅ Sans Google Play  

❌ Ne pas choisir Google Play.

---

## 📌 Nom recommandé

```text
MobSF_DIVA_API_30
```

---

# 📸 Capture — Création de l’AVD

<img width="404" height="287" alt="screen 2" src="https://github.com/user-attachments/assets/9383dafa-a55c-454b-a19e-0f57ea0b0134" />
<img width="404" height="287" alt="screen 2" src="https://github.com/user-attachments/assets/234f627e-891d-4303-9541-24c5c91dae5a" />
<img width="420" height="305" alt="screen 1" src="https://github.com/user-attachments/assets/3a85ab72-fd73-4bd6-a782-c7b25e88340d" />



---

# 📌 Étape 2 — Cloner MobSF

Ouvrir un terminal :

```bash
git clone https://github.com/MobSF/Mobile-Security-Framework-MobSF.git

cd Mobile-Security-Framework-MobSF
```

---

# 📸 Capture — Clone MobSF



<img width="404" height="287" alt="screen 2" src="https://github.com/user-attachments/assets/fca217ae-8305-4ef1-bfba-7a7231f1faf4" />


---

# 📌 Étape 3 — Lancement de l’Émulateur Rooté

## 🔹 Linux / Mac

```bash
./scripts/start_avd.sh
```

---

## 🔹 Windows PowerShell

```powershell
scripts\start_avd.ps1
```

---

## 📌 Démarrer l’AVD

```bash
./scripts/start_avd.sh MobSF_DIVA_API_30
```

---

## 📌 Vérification ADB

```bash
adb devices
```

Résultat attendu :

```text
List of devices attached
emulator-5554 device
```

⚠️ Garder cet identifiant.

---

# 📸 Capture — Vérification ADB


<img width="404" height="134" alt="screen 3" src="https://github.com/user-attachments/assets/f17971e5-e035-4a14-9490-cce0f136ef2d" />

---

# 📌 Étape 4 — Installation et Lancement de MobSF avec Docker

## 📌 Télécharger l’image Docker

```bash
docker pull opensecurity/mobile-security-framework-mobsf:latest
```

---

## 📌 Lancer MobSF

```bash
docker run -it --rm \
-p 8000:8000 \
-e MOBSF_ANALYZER_IDENTIFIER=emulator-5554 \
opensecurity/mobile-security-framework-mobsf:latest
```

⚠️ Remplacer :

```text
emulator-5554
```

par votre identifiant ADB.

---

# 📌 Accès à MobSF

Ouvrir :

```text
http://127.0.0.1:8000
```

---

# 🔐 Identifiants par défaut

```text
Username : mobsf
Password : mobsf
```

---

# 📸 Capture — Login MobSF


<img width="401" height="359" alt="screen 4" src="https://github.com/user-attachments/assets/6d00fd70-1644-4713-9de5-0d598e34c865" />


---

# 📌 Étape 5 — Téléchargement de DIVA APK

## 🔹 Site officiel

```text
http://www.payatu.com/damn-insecure-and-vulnerable-app/
```

---

## 🔹 GitHub officiel

```text
https://github.com/payatu/diva-android
```

---

# 📌 APK utilisé

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

MobSF réalise automatiquement :

- Analyse du Manifest
- Analyse des permissions
- Reverse Engineering
- Détection des vulnérabilités
- Analyse du code source

---

# 📸 Capture — Upload APK


<img width="311" height="185" alt="screen 5" src="https://github.com/user-attachments/assets/bde868d8-5094-41bf-a713-f9b606a1563b" />


---

# 📸 Capture — Rapport Statique


<img width="419" height="190" alt="screen 6" src="https://github.com/user-attachments/assets/67cc18d4-7ee8-4176-8d8e-79108bc10cde" />


---

# 📌 Étape 7 — Analyse Dynamique

Dans le rapport MobSF :

```text
Dynamic Analysis → Start Dynamic Analyzer
```

MobSF va :

✅ Installer DIVA  
✅ Configurer le proxy HTTPS  
✅ Installer le certificat Root CA  
✅ Lancer Frida Server  
✅ Connecter l’émulateur  

---

# 📸 Capture — Dynamic Analyzer


<img width="503" height="263" alt="screen 7" src="https://github.com/user-attachments/assets/3e27ab9d-35a4-4ac0-9dab-76582244b46b" />


---

# 📌 Étape 8 — Exploration de DIVA

Dans l’émulateur :

- Ouvrir DIVA
- Explorer les challenges

Exemples :

- Insecure Logging
- Hardcoded Credentials
- Insecure Storage
- SQL Injection
- Access Control
- Intent Vulnerabilities

---

# 📸 Capture — Application DIVA


<img width="157" height="314" alt="screen 8" src="https://github.com/user-attachments/assets/9fe7a33b-8602-427b-bb29-31a57570154f" />


---

# 📌 Étape 9 — Analyse Runtime avec MobSF

## 🔹 Runtime Logs

Permet de voir :

- Logs Android
- Exceptions
- Informations sensibles

---

## 🔹 Network Traffic

Permet d’intercepter :

- HTTP
- HTTPS
- API Calls

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

Permet d’observer :

- Création de fichiers
- Données sensibles
- Stockage insecure

---

## 🔹 Intent Monitor

Permet d’observer :

- Intents Android
- Activities exportées
- Communications inter-applications

---

# 📸 Capture — Runtime Analysis

<img width="149" height="319" alt="screen 9" src="https://github.com/user-attachments/assets/d596fd0b-694e-48a5-8db6-97e44b439136" />


---

# 📌 Menu Dynamic Analyzer

| Fonction | Description |
|---|---|
| Stop Screen | Arrêter le mirroring |
| Remove Root CA | Supprimer le certificat MobSF |
| Unset HTTP(S) Proxy | Désactiver le proxy |
| TLS/SSL Security Tester | Tester SSL/TLS |
| Exported Activity Tester | Tester les activities exportées |
| Activity Tester | Tester les activités Android |
| Get Dependencies | Voir les dépendances |
| Take a Screenshot | Capturer l’écran |
| Logcat Stream | Voir les logs Android |
| Generate Report | Générer le rapport final |

---

# 📌 Vulnérabilités Observées

| Vulnérabilité | Description |
|---|---|
| Insecure Storage | Données stockées en clair |
| Hardcoded Credentials | Secrets codés en dur |
| Exported Activities | Activities accessibles sans protection |
| Insecure Logging | Données sensibles dans les logs |
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

---

## ❌ MobSF ne détecte pas l’émulateur

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

MobSF est aujourd’hui l’un des outils les plus puissants pour l’analyse de sécurité mobile Android.

---

# 📚 Ressources Officielles

## 🔹 Documentation MobSF

```text
https://github.com/MobSF/docs
```

---

## 🔹 Projet DIVA

```text
https://github.com/payatu/diva-android
```

---

## 🔹 OWASP Mobile Security

```text
https://owasp.org/www-project-mobile-top-10/
```

