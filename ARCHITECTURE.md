# Architecture Technique : BENSALMAN MESH 👑

## 1. Pile Radio & Couches Réseau

### A. Couche Découverte & Publicité BLE (Bluetooth Low Energy)
- UUID de service personnalisé réservé : `0000B5A1-0000-1000-8000-00805F9B34FB` (Bensalman Service Mesh).
- Paquet publicitaire (Advertising Packet) :
  - **Identifiant Nœud Public (8 octets)** : Dérivé de la clé publique de l'utilisateur.
  - **Statut de Relais (1 octet)** : Indique si le nœud accepte de servir de relais Store-and-Forward.
  - **Séquence / Horodatage (4 octets)** : Pour l'estimation de fraîcheur du nœud.

### B. Couche Transport Haut Débit : Wi-Fi Direct (P2P Group Owner)
- Établissement automatique d'un groupe Wi-Fi Direct lorsque la charge utile dépasse le MTU BLE (e.g. mémos vocaux ou fichiers).
- Débit typique : 15 à 45 Mbps sans infrastructure ni point d'accès tiers.

## 2. Protocole de Routage Multi-Sauts

Chaque paquet réseau suit la structure binaire suivante :
\`\`\`
+-----------------------+-----------------------+
| Version (1 octet)     | Type Paquet (1 octet) |
+-----------------------+-----------------------+
| TTL (1 octet)         | Nombre de sauts (1)   |
+-----------------------+-----------------------+
| UUID Message (16 octets - UUIDv4)             |
+-----------------------+-----------------------+
| ID Émetteur (32 octets - Clé Publique)        |
+-----------------------+-----------------------+
| ID Destinataire (32 octets - Clé Publique)    |
+-----------------------+-----------------------+
| IV / Nonce (12 octets - AES-GCM)              |
+-----------------------+-----------------------+
| Charge Utile Chiffrée (Payload variable)      |
+-----------------------+-----------------------+
| Tag d'Authentification (16 octets Poly1305)   |
+-----------------------+-----------------------+
\`\`\`

### Cycle de Vie d'un Paquet :
1. **Émission** : TTL initial = 7. L'identifiant du paquet est inséré dans le cache local anti-boucle (LRU Cache).
2. **Réception par un pair intermédiaire** :
   - Vérification de l'UUID dans la table de déduplication. Si déjà vu : suppression immédiate silencieuse.
   - Si le destinataire correspond à la clé publique locale : déchiffrement et notification locale.
   - Si le destinataire est tiers et TTL > 1 : décrémentation de TTL, incrémentation du compteur de sauts, et diffusion sur les canaux radio voisins.
   - Si aucun voisin n'est en vue : mise en file d'attente *Store-and-Forward* (expiration au bout de 48 heures).

## 3. Sécurité Cryptographique (E2EE)
- Aucun serveur d'authentification centralisé.
- Les identités sont des paires de clés cryptographiques (Ed25519 pour la signature, X25519 pour l'accord de clé Diffie-Hellman).
- Clé de session éphémère dérivée par HKDF-SHA256.
- Confidentialité persistante parfaite (PFS - Perfect Forward Secrecy) sur les sessions directes.
