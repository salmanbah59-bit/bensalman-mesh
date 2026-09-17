# Instructions de Compilation & Exportation BENSALMAN MESH 👑

## 1. Prérequis
- Flutter SDK >= 3.19.0
- Android SDK Tools avec API Level 34
- JDK 17
- Câble USB avec Débogage USB activé sur smartphone Android

## 2. Étapes de compilation

\`\`\`bash
# Nettoyer les anciens builds
flutter clean

# Télécharger les bibliothèques
flutter pub get

# Compiler l'APK universel de production
flutter build apk --release

# L'APK compilé est situé à :
# build/app/outputs/flutter-apk/app-release.apk
\`\`\`

## 3. Autorisations Android nécessaires à accorder au premier lancement
L'application demandera les autorisations suivantes (toutes gérées hors-ligne) :
- Appareils à proximité (Nearby Devices / Bluetooth)
- Recherche de réseaux Wi-Fi locaux (Wi-Fi Direct P2P)
- Microphone (pour l'enregistrement des mémos vocaux cryptés)
- Exécution en arrière-plan sans optimisation de batterie (pour assurer le relais de messages écran éteint)
