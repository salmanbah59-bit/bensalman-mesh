# BENSALMAN 👑 - Messagerie Mobile Hors-Ligne Décentralisée (Mesh BLE & Wi-Fi Direct)

Application de messagerie instantanée souveraine et sécurisée, conçue pour fonctionner **sans connexion Internet, sans carte SIM, sans routeur Wi-Fi, ni serveur central**.

---

## 🌟 Points Clés d'Architecture

1. **Réseau Maillé Radio (Multi-Hop Mesh) :**
   - Routage dynamique pair-à-pair via Bluetooth Low Energy (BLE) et Wi-Fi Direct (P2P).
   - Relais multi-sauts : un message pour un destinataire hors de portée physique transite de manière chiffrée par les nœuds intermédiaires (**A ➔ B ➔ C**) sans que les nœuds relais ne puissent en lire le contenu.
   - Déduplication automatique des paquets par identifiant unique (UUIDv4) et limitation du nombre de sauts (TTL - Time to Live par défaut = 7).
   - Stockage et réacheminement différé (*Store-and-Forward*) : les messages en attente sont persistés en mémoire sécurisée et retransmis dès qu'un nœud valide est découvert.

2. **Chiffrement de Bout en Bout Souverain (E2EE) :**
   - Échange de clés par courbe elliptique (Curve25519 / X25519).
   - Chiffrement symétrique des messages, métadonnées et fichiers audio en AES-256-GCM.
   - Signature cryptographique Ed25519 garantissant l'authenticité et l'intégrité sans tiers de confiance.
   - Empreinte de sécurité visuelle vérifiable par scan de QR code physique.

3. **Notes Vocales & Fichiers Audio Ultra-Compressés :**
   - Enregistrement audio basse consommation compressé pour le transit sur canaux BLE à débit restreint.
   - Lecteur audio intégré hors-ligne.

---

## 📁 Structure du Répertoire

\`\`\`
├── android/                             # Projet Android Natif & Configuration Gradle
│   ├── app/
│   │   ├── build.gradle                 # MinSdk 23, CompileSdk 34, MultiDex, ProGuard
│   │   └── src/main/
│   │       ├── AndroidManifest.xml      # Autorisations matérielles BLE & Wi-Fi Direct P2P
│   │       ├── kotlin/com/bensalman/mesh/
│   │       │   └── MainActivity.kt      # Canal natif (MethodChannel) vers puces radio
│   │       └── res/                     # Ressources graphiques, chaînes et thèmes
│   ├── build.gradle                     # Gradle racine
│   ├── settings.gradle
│   └── gradle/wrapper/                  # Gradle wrapper properties
├── lib/                                 # Code source Flutter (Dart)
│   ├── main.dart                        # Point d'entrée de l'application mobile
│   ├── models/                          # Modèles de données (Message, Friend, UserProfile)
│   ├── screens/                         # Écrans UI (Home, Chat, Topologie maillée)
│   └── services/                        # Services (MeshService, CryptoService, AudioService)
├── assets/                              # Ressources statiques de l'application
│   ├── images/                          # Logotype Bensalman Gold, splash screen
│   └── audio/                           # Échantillon audio mémo vocal hors-ligne
├── src/                                 # Application Web de Simulation & Dashboard interactif
│   ├── components/                      # Simulateur multi-appareils, console de paquets radio
│   ├── data/                            # Topologie réseau simulée et crypto locale
│   └── utils/
├── pubspec.yaml                         # Dépendances Flutter (BLE, Wi-Fi Direct, E2EE)
└── package.json                         # Configuration de la suite Web & simulateur
\`\`\`

---

## 🚀 Guide de Compilation & Installation

### Option 1 : Déploiement Direct sur Téléphone Android (USB Débogage)
\`\`\`bash
# 1. Vérifier l'installation de Flutter et d'Android SDK
flutter doctor

# 2. Installer les dépendances du projet
flutter pub get

# 3. Connecter le smartphone Android via câble USB (Débogage USB activé)
flutter devices

# 4. Lancer l'application en mode Release
flutter run --release
\`\`\`

### Option 2 : Génération de l'APK autonome
\`\`\`bash
# Générer le fichier APK de production signé
flutter build apk --release --split-per-abi=false

# L'APK généré sera disponible dans :
# build/app/outputs/flutter-apk/app-release.apk

# Installer sur l'appareil via ADB :
adb install -r build/app/outputs/flutter-apk/app-release.apk
\`\`\`
