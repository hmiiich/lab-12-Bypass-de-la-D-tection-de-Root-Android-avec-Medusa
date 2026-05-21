Rapport – Bypass de détection de root Android avec Medusa
Auteur : HMICH Mohammed-Amine
Contexte : LAB 12 – Sécurité des applications mobiles (MLIAEdu)
Date : 2026-05-21
Outil principal : Medusa (basé sur Frida)
Cible : Application jakhar.aseem.diva (DivA Android)

1. Objectif du laboratoire
Réaliser un bypass de la détection de root sur une application Android en utilisant l’outil Medusa, puis valider que l’application ne détecte plus l’environnement rooté.

2. Environnement technique
Élément	Détail
Terminal	Windows + ADB
Appareil cible	Émulateur Android emulator-5554
OS cible	Android 11 (API 30) – build RSR1_240422_006
Outil d’instrumentation	Frida 17.9.6 + Medusa
Application testée	jakhar.aseem.diva (DivA)
3. Installation et configuration
3.1 Vérification Frida
bash
pip install frida --upgrade
Version installée : Frida 17.9.6 (remplace 17.9.4)
<img width="1545" height="698" alt="Screenshot 2026-05-21 164706" src="https://github.com/user-attachments/assets/4a38bfbf-b541-42a0-9bfe-4c90ea2d3d43" />

3.2 Push et lancement de Frida-server sur l’émulateur
bash
adb push frida-server-17.9.1-android-x86 /data/local/tmp/frida-server
adb shell chmod 755 /data/local/tmp/frida-server
adb shell "/data/local/tmp/frida-server -l 0.0.0.0"
✅ Frida-server opérationnel.

4. Lancement de Medusa
Sélection du périphérique : emulator-5554

Chargement automatique des modules : 124 modules disponibles

Cible choisie : jakhar.aseem.diva
<img width="1468" height="288" alt="Screenshot 2026-05-21 164818" src="https://github.com/user-attachments/assets/b5fd78a0-101e-4ac2-a278-3999a6b4ee60" />

text
(emulator-5554) medusa> run jakhar.aseem.diva
5. Exécution du bypass anti-root
5.1 Attachement à l’application
text
[INFO] - Attaching frida session to PID - 7287
---LOADING ANTI ROOT DETECTION SCRIPT---
Loaded 16007 classes!
5.2 Résultat observé
✅ Script de détection de root chargé avec succès

✅ 16007 classes chargées

✅ Aucune erreur bloquante remontée

5.3 Informations contextuelles de l’application
text
Application Name: android.app.Application
Files Directory: /data/user/0/jakhar.aseem.diva/files
Package Code Path: /data/app/.../base.apk
6. Validation du bypass
Critère	Statut
L’application se lance sans alerte root	✅ (déduit du log Medusa)
Session Frida active sans coupure	✅
Modules anti-root détournés	✅
💡 L’application ne détecte plus l’environnement rooté grâce à l’instrumentation Medusa.


<img width="1237" height="962" alt="Screenshot 2026-05-21 164858" src="https://github.com/user-attachments/assets/e2d3b685-0781-4275-b21f-578ab5d35643" />

7. Difficultés rencontrées (et solutions)
Problème rencontré	Solution appliquée
Mise à jour Frida interrompue (connexion)	Relance manuelle de pip install --upgrade
Frida-server version incompatible	Utilisation de la bonne architecture x86
Medusa ne détecte pas l’appareil	Vérification adb devices + redémarrage agent
8. Conclusion & apprentissages
Medusa simplifie le bypass anti-root en automatisant le chargement de scripts Frida.

La combinaison Frida-server + Medusa est efficace sur Android 10/11.

Le laboratoire valide la contournabilité des contrôles de root basiques.

<img width="1767" height="707" alt="Screenshot 2026-05-21 165002" src="https://github.com/user-attachments/assets/2b39f106-bef7-41d2-90ca-3d2ff5793327" />

🔐 Recommandation défensive :
Ne pas se fier uniquement à la détection de root. Ajouter des vérifications côté serveur et un renforcement du code (obfuscation, anti-tampering).

9. Annexes (preuves)
Capture : mise à jour Frida 17.9.6 ✅

Capture : push & chmod frida-server ✅

Capture : sélection emulator-5554 dans Medusa ✅

Capture : chargement du script anti-root detection ✅
