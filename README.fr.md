[English](README.md) · [简体中文](README.zh-CN.md) · [繁體中文](README.zh-TW.md) · [Русский](README.ru.md) · [Español](README.es.md) · [Português (Brasil)](README.pt-BR.md) · **Français** · [Deutsch](README.de.md) · [Türkçe](README.tr.md) · [Tiếng Việt](README.vi.md) · [한국어](README.ko.md) · [日本語](README.ja.md)

> Cette traduction est fournie pour plus de commodité ; seul le README en anglais fait foi. *(This translation is provided for convenience; the English README is authoritative.)*

# TheNewMiner

TheNewMiner est un mineur GPU pour **TensorCash**. Il fonctionne sur les cartes NVIDIA GeForce RTX des séries 30, 40 et 50
dotées de **10 GB de VRAM ou plus** et mine sur le pool **`tsc-wss.the-new.win`**, qui est intégré : vous n'avez pas à
saisir d'adresse de pool, et la connexion est directe.

Version actuelle : **v0.1.44**

## Téléchargement

| Fichier | Pour | SHA-256 |
|---|---|---|
| [`TheNewMiner-v0.1.44-linux-x86_64-sealed-r4.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/main/TheNewMiner-v0.1.44-linux-x86_64-sealed-r4.tar.xz) | Linux x86_64 (Ubuntu) | `99a8246e242c7d09f1c4b6335d974de0d163362d4dc634fff4188ab58a151ca6` |
| [`TheNewMiner-v0.1.44-windows-wsl2-r5.zip`](https://github.com/ngasuka855/TheNewMiner/raw/main/TheNewMiner-v0.1.44-windows-wsl2-r5.zip) | Windows 10 / 11 (fonctionne via WSL2) | `264025136cf01d87d25ca2bb0554fe88efe01833850a697247cd657027215506` |
| [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/main/SHA256SUMS) | sommes de contrôle des deux paquets | |

Le zip Windows est tout-en-un : il contient déjà le mineur Linux, vous n'avez donc besoin que du zip.

## Vérifier le téléchargement

Placez le ou les paquets et `SHA256SUMS` dans le même dossier, puis :

```bash
# Linux (ajoutez --ignore-missing si vous n'avez téléchargé qu'un seul paquet)
sha256sum -c SHA256SUMS
# macOS
shasum -a 256 -c SHA256SUMS
```

```bat
:: Windows (Invite de commandes) : comparez le résultat avec le SHA-256 du tableau ci-dessus
certutil -hashfile TheNewMiner-v0.1.44-windows-wsl2-r5.zip SHA256
```

Chaque fichier doit afficher `OK` (sous Windows : les mêmes 64 caractères que dans le tableau). S'il ne correspond pas, ne l'utilisez pas.

## Linux

```bash
tar -xJf TheNewMiner-v0.1.44-linux-x86_64-sealed-r4.tar.xz
cd thenewminer
./thenewminer -u <YOUR_WALLET> -p <YOUR_POOL_PASSWORD> -w rig-01 --background
```

| Commande | Ce qu'elle fait |
|---|---|
| `./thenewminer -u <YOUR_WALLET> -p <YOUR_POOL_PASSWORD> -w rig-01 --background` | Premier démarrage : installe ce qui est nécessaire, puis mine en arrière-plan |
| `./thenewminer --background` | Démarrages suivants : vos paramètres sont mémorisés |
| `./thenewminer` | Miner au premier plan |
| `./thenewminer --logs` | Suivre le mineur (Ctrl+C quitte l'affichage ; le minage continue) |
| `./thenewminer --stop` | Arrêter le minage |

- `-u` est l'adresse de votre portefeuille TensorCash, `-p` le mot de passe de votre pool, `-w` le nom du worker affiché
  sur le pool (par défaut : le nom d'hôte de la machine).
- Les paramètres sont mémorisés dans `thenewminer/.miner/` (le fichier contenant le mot de passe n'est lisible que par vous).
- Le premier démarrage télécharge le modèle (environ 8.1 GiB, une seule fois), installe le moteur GPU pour votre carte et
  effectue un autotest sur la carte, puis commence à miner. Des lignes comme `[ RATE] ... N/s` indiquent qu'il mine.
- Le premier démarrage peut installer quelques paquets Ubuntu avec `apt` (nécessite sudo ou root).
- Exactement un GPU NVIDIA par machine dans cette version (les rigs multi-GPU ne sont pas encore pris en charge).
- Si `tar` affiche `xz: Cannot exec`, installez d'abord xz : `sudo apt-get install -y xz-utils`.
- Le `README.md` contenu dans le paquet est une ancienne note interne ; suivez cette page (aucun `--proxy` n'est nécessaire).
- **Mise à jour :** `./thenewminer --stop`, décompressez le nouveau paquet au même endroit (il se décompresse dans
  `thenewminer/` et conserve `thenewminer/.miner/` avec vos paramètres et le modèle), puis redémarrez-le.

## Windows (en un clic)

1. Décompressez l'intégralité de `TheNewMiner-v0.1.44-windows-wsl2-r5.zip` (clic droit › Extraire tout...) où vous voulez :
   n'importe quel dossier, n'importe quel lecteur ; les chemins contenant des espaces ou des caractères chinois sont
   acceptés. Ne le lancez pas depuis l'aperçu du zip.
2. Double-cliquez sur **`TheNewMiner.bat`**.
   - **La première fois :** il vérifie Windows, le pilote et le disque, active WSL2 si nécessaire (une seule demande
     administrateur ; un redémarrage peut être nécessaire une fois, puis l'installation reprend d'elle-même), indique où
     les données seront stockées (Entrée pour accepter), crée son propre environnement Linux (WSL), installe le mineur
     fourni, demande **une seule fois** l'adresse de votre portefeuille (commence par `tc1`), le mot de passe du pool
     (saisi deux fois ; 6–128 lettres, chiffres, `_` ou `-`) et le nom du worker (Entrée = le nom de l'ordinateur),
     télécharge le modèle, puis commence à miner.
   - **Les fois suivantes :** double-cliquez et il commence immédiatement à miner avec vos paramètres enregistrés. Un
     raccourci « TheNewMiner » est également créé sur le bureau.

Commandes courantes (ouvrez une Invite de commandes dans le dossier décompressé) :

| Commande | Ce qu'elle fait |
|---|---|
| `TheNewMiner.bat` | Installe si nécessaire, puis mine dans cette fenêtre (fermer la fenêtre arrête le minage) |
| `TheNewMiner.bat background` | Miner sans fenêtre |
| `TheNewMiner.bat logs` | Suivre le mineur (Ctrl+C quitte l'affichage ; le minage continue) |
| `TheNewMiner.bat status` | Indique s'il tourne, et les derniers hashrates |
| `TheNewMiner.bat stop` | Arrêter le minage |
| `TheNewMiner.bat setup` | Saisir à nouveau le portefeuille / mot de passe / nom du worker |
| `TheNewMiner.bat uninstall` | Supprime le mineur, son dossier de données et le raccourci (demande d'abord confirmation) |

- **Où vont les données :** le mineur et le modèle se trouvent dans un dossier de données choisi lors de la première
  installation (par défaut `TheNewMinerData` à la racine du lecteur où vous avez décompressé, p. ex. `D:\TheNewMinerData`),
  et non dans le dossier décompressé. Il faut environ 20 GB d'espace libre à cet endroit.
- **Mise à jour :** décompressez la nouvelle version n'importe où et double-cliquez sur son `TheNewMiner.bat`. Il trouve le
  mineur installé, conserve vos paramètres et le modèle, et installe la nouvelle version sans poser de questions. L'ancien
  dossier peut être supprimé.
- **Désinstallation :** `TheNewMiner.bat uninstall`.
- La langue affichée suit celle de Windows (English / 简体中文 / 繁體中文). Pour la choisir, ajoutez `-Lang en` (ou `zh-Hans`,
  `zh-Hant`) à n'importe quelle commande, p. ex. `TheNewMiner.bat -Lang en` ; cela s'applique à cette exécution.

## Proxy (facultatif)

Le mineur se connecte directement au pool. Si votre réseau nécessite un proxy :

```bash
# Linux (mémorisé jusqu'à modification)
./thenewminer --proxy <IP>[:<PORT>]            # relais TCP (port 443 si omis)
./thenewminer --proxy socks5://<HOST>:<PORT>   # SOCKS5, également socks5://<USER>:<PASS>@<HOST>:<PORT>
./thenewminer --proxy http://<HOST>:<PORT>     # proxy HTTP CONNECT
./thenewminer --proxy none                     # retour à une connexion directe
```

Ces commandes démarrent aussi le mineur : arrêtez d'abord le mineur, et ajoutez `--background` pour qu'il reste en arrière-plan.

```bat
:: Windows
TheNewMiner.bat setup -Proxy <IP>[:<PORT>]
TheNewMiner.bat setup -Proxy socks5://<HOST>:<PORT>
TheNewMiner.bat setup -Proxy http://<HOST>:<PORT>
TheNewMiner.bat setup -Proxy none
```

Le TLS fonctionne toujours de bout en bout avec le pool, le proxy ne peut donc pas lire la connexion. Sous Windows, une
application proxy tournant sur le même PC n'est généralement pas joignable via `127.0.0.1` depuis WSL (réseau WSL par
défaut) : utilisez l'IP locale (LAN) du PC et autorisez les connexions LAN dans l'application.

## Configuration requise

| Élément | Exigence |
|---|---|
| GPU | Exactement une carte NVIDIA GeForce RTX série 30 / 40 / 50, 10 GB de VRAM ou plus |
| Pilote | NVIDIA **580 ou plus récent** (`nvidia-smi` doit fonctionner). Sous Windows, installez-le dans Windows, jamais dans WSL |
| Linux | Ubuntu x86_64 22.04, 24.04 ou 26.04 (les autres distributions ne sont pas encore prises en charge) |
| Windows | Windows 10 21H2 ou plus récent, ou Windows 11, 64 bits ; virtualisation (Intel VT-x / AMD SVM) activée dans le BIOS |
| Disque | environ 20 GB d'espace libre |
| RAM | 4 GB ou plus |
| Modèle | environ 8.1 GiB, téléchargé une seule fois au premier démarrage (en parallèle, multi-miroirs) puis réutilisé |

## Hashrates de référence

Les meilleurs hashrates mesurés sur un vrai pool avec nos propres rigs (entre le 2026-10-14 et le 2026-10-23). Ce sont des
valeurs **maximales**, pas une promesse : des cartes du même modèle diffèrent d'environ 5–8% selon la puce, le
refroidissement, les fréquences et la limite de puissance, et le hashrate baisse si la carte gère aussi un écran ou si
d'autres programmes utilisent le GPU.

| GPU | Hashrate |
|---|---|
| RTX 5090 | jusqu'à ~99 N/s |
| RTX 4090 | jusqu'à ~70 N/s |
| RTX 3080 10 GB | jusqu'à ~22 N/s |

## Résultats des tests

Tests de la v0.1.44 sur un vrai pool, sur nos rigs de test, le 2026-10-23 (un GPU par machine). A = parts (shares)
acceptées, R = parts rejetées.

| Date | Carte | OS | Paquet | Hashrate | A / R |
|---|---|---|---|---|---|
| 2026-10-23 | RTX 4090 | Linux | sealed-r4 | 65.58 N/s (médiane, 16 min) | 10 / 0 |
| 2026-10-23 | RTX 5090 | Linux | sealed-r4 | 93.45 N/s (minage stable, 44 min ; 90.39 en incluant les changements de job) | 31 / 0 (+1 périmée à l'arrêt) |
| 2026-10-23 | RTX 3080 10 GB | Windows (WSL2) | windows-wsl2-r5 | ~22 N/s (exécution par l'opérateur lui-même) | n/d |
| 2026-10-23 | RTX 3080 10 GB | Linux (Ubuntu 26.04) | sealed-r4 | 21.04 N/s (10 dernières min sur 23 min de minage) | 3 / 0 |

RTX 3080 sous Linux : installation 6/6 étapes OK, aucune fenêtre invalide, arrêt propre. Extrait du journal :
[`test-results/rtx3080-linux-20261023.txt`](test-results/rtx3080-linux-20261023.txt).

## Captures d'écran

Issues de nos rigs de test, avec des versions antérieures : la v0.1.41 sur la RTX 4090 et une première build sur la RTX 5090.
Ce sont des relevés en direct ; vos valeurs dépendent de la carte.

**RTX 4090, v0.1.41 :** 69–75 N/s par relevé, le plus souvent ~70 (moyenne sur 10 minutes ~68), 9 parts acceptées, 0 rejetée, ~445 W.

![TheNewMiner v0.1.41 en train de miner sur une RTX 4090](screenshots/rtx4090.png)

**RTX 5090, première build :** 100.83 N/s (moyenne en direct sur les fenêtres terminées), ~575 W.

![Une première build de TheNewMiner en train de miner sur une RTX 5090](screenshots/rtx5090.png)

## FAQ

**Windows affiche « Windows a protégé votre ordinateur » / « Éditeur inconnu ».**
Le kit n'est pas signé numériquement. Cliquez sur **Informations complémentaires**, puis sur **Exécuter quand même**.

**Mon antivirus signale ou supprime le mineur.**
Microsoft Defender et d'autres antivirus signalent souvent les mineurs de cryptomonnaie comme « applications potentiellement
indésirables », même un mineur légitime que vous avez choisi de lancer, car les logiciels malveillants cachent fréquemment
des mineurs. Si un fichier est mis en quarantaine, ouvrez Sécurité Windows › Protection contre les virus et menaces ›
Historique de protection, sélectionnez l'élément TheNewMiner et choisissez **Autoriser** (ou ajoutez le dossier décompressé
aux Exclusions). Ne le faites que pour un paquet téléchargé depuis cette page dont le SHA-256 correspond.

**Le premier démarrage prend beaucoup de temps.**
Le premier démarrage télécharge le modèle d'environ 8.1 GiB et effectue un autotest GPU sur votre carte ; sous Windows, il
peut aussi installer WSL2. Cela n'arrive qu'une seule fois. Les démarrages suivants commencent à miner bien plus vite.

**Où sont les journaux ?**
Linux : `./thenewminer --logs`, ou le fichier `thenewminer/.miner/thenewminer.log`.
Windows : `TheNewMiner.bat logs` (et `TheNewMiner.bat status` pour les derniers hashrates).

**HiveOS ?**
Un paquet HiveOS arrive **bientôt** ; il est encore en phase de test.

## Note de sécurité

Le mineur et le moteur GPU sont livrés chiffrés (ChaCha20-Poly1305) dans le paquet ; le mineur les déchiffre sur votre
machine au démarrage. Cela rend leur copie plus coûteuse, mais ce n'est **pas inviolable** : toute personne disposant d'un
accès administrateur à une machine qui exécute le mineur peut, avec suffisamment d'efforts, récupérer la clé ou le code
déchiffré.
L'adresse de votre portefeuille et le mot de passe de votre pool sont stockés sur votre propre machine et envoyés uniquement
au pool.
