# TheNewMiner

TheNewMiner est un mineur GPU pour **TensorCash** sur les cartes NVIDIA GeForce RTX des séries 30, 40 et 50 dotées de 10 GB de VRAM ou plus ; le pool **`tsc-wss.the-new.win`** est intégré.

[English](../../README.md) · [简体中文](README.zh-CN.md) · [繁體中文](README.zh-TW.md) · [Русский](README.ru.md) · [Español](README.es.md) · [Português (Brasil)](README.pt-BR.md) · **Français** · [Deutsch](README.de.md) · [Türkçe](README.tr.md) · [Tiếng Việt](README.vi.md) · [한국어](README.ko.md) · [日本語](README.ja.md)

> Cette traduction est fournie pour plus de commodité ; seul le README en anglais fait foi. *(This translation is provided for convenience; the English README is authoritative.)*

## Téléchargement

Version actuelle : **v0.1.47** · Site web du pool : [tsc.the-new.win](https://tsc.the-new.win/#/home)

| Système | Téléchargement | Ensuite |
|---|---|---|
| Linux x86_64 | [`TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz) | ou utilisez l'installation en un clic ci-dessous |
| Windows 10 / 11 | [`TheNewMiner-v0.1.47-windows-wsl2-r3.zip`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-windows-wsl2-r3.zip) | décompressez-le, double-cliquez sur `TheNewMiner.bat` |
| HiveOS (expérimental) | [`hiveos/flightsheet-thenewminer.json`](../../hiveos/flightsheet-thenewminer.json) | importez la flight sheet (voir HiveOS ci-dessous) |

Sommes de contrôle : [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/SHA256SUMS). La façon de les vérifier est
expliquée dans le [guide](GUIDE.fr.md). Si un fichier ne correspond pas, ne l'utilisez pas.

## Avant de commencer : créez votre compte sur le pool

Pour miner, il faut un compte sur le pool : votre adresse de paiement TSC (portefeuille) et un mot de passe du pool que
vous choisissez sur le site web du pool. Le site existe en anglais (utilisez son menu de langue).

1. Ouvrez [tsc.the-new.win](https://tsc.the-new.win/#/home) et allez dans **Start mining** (开始挖矿).
2. Sous **1. Bind Wallet**, saisissez votre **TSC payout address** (`tc1...` ; vérifiez-la soigneusement : les revenus
   de minage envoyés à une mauvaise adresse ne peuvent pas être récupérés), choisissez un **Pool Password** (6–128
   caractères) et saisissez-le à nouveau sous **Confirm Password**.
3. Cliquez sur **Apply to join pool**. La page confirme que le portefeuille est lié ; le compte fonctionne
   immédiatement.
4. Utilisez le même portefeuille et le même mot de passe dans le mineur : `-u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>'`
   sous Linux, la fenêtre de compte de `TheNewMiner.bat` sous Windows, ou la flight sheet sous HiveOS. Environ 3–5
   minutes après le début du minage, votre rig apparaît sous **My Miners** sur le site web du pool.

Le pool ne peut pas récupérer un mot de passe du pool oublié, et l'adresse de paiement ne peut pas être modifiée par la
suite. Le mot de passe du pool n'est pas la clé privée de votre portefeuille : ne donnez jamais votre clé privée à
personne.

## Installation en un clic (Linux)

Remplacez `<YOUR_WALLET>`, `<YOUR_POOL_PASSWORD>` et `<RIG_NAME>` (y compris les `<` `>`) à la fin de cette commande
par votre portefeuille, votre mot de passe du pool et un nom pour ce rig, puis collez-la dans un terminal. Elle
télécharge le paquet Linux et `SHA256SUMS`, vérifie le SHA-256, décompresse le mineur dans `~/thenewminer` et le démarre
en arrière-plan :

```bash
mkdir -p ~/thenewminer-download && cd ~/thenewminer-download && V=v0.1.47 && P=TheNewMiner-$V-linux-x86_64-sealed.tar.xz && U=https://github.com/ngasuka855/TheNewMiner/raw/$V && rm -f "$P" SHA256SUMS && if command -v curl >/dev/null 2>&1; then curl -fL -o "$P" "$U/$P" && curl -fL -o SHA256SUMS "$U/SHA256SUMS"; else wget -O "$P" "$U/$P" && wget -O SHA256SUMS "$U/SHA256SUMS"; fi && sha256sum -c --ignore-missing SHA256SUMS && tar -xJf "$P" -C ~ && cd ~/thenewminer && ./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w <RIG_NAME> --background
```

Une erreur de téléchargement ou un SHA-256 qui ne correspond pas ne lance jamais rien. Gardez les guillemets simples
autour du mot de passe (un `'` dans le mot de passe s'écrit `'\''`). Sur une machine à un seul GPU, le mineur vérifie le
portefeuille et le mot de passe auprès du pool avant de télécharger le modèle (environ 8.1 GiB, une seule fois). Ensuite,
`./thenewminer --logs` montre qu'il mine et `./thenewminer --status` indique s'il tourne, ou pourquoi il s'est arrêté.
Pour mettre à jour, exécutez d'abord `~/thenewminer/thenewminer --stop`, puis la même commande : vos paramètres et le
modèle sont conservés. Tout le reste (commandes courantes, mises à jour, démarrage automatique, que faire quand il ne mine
pas) se trouve dans le [guide](GUIDE.fr.md).

## Windows

1. Décompressez l'intégralité de `TheNewMiner-v0.1.47-windows-wsl2-r3.zip` (clic droit › Extraire tout...) où vous voulez.
2. Double-cliquez sur **`TheNewMiner.bat`**. La première fois, UNE fenêtre demande votre compte sur le pool (portefeuille,
   mot de passe du pool, nom du worker) ; ensuite, plus aucune question : il installe WSL2 et Ubuntu, installe le mineur
   fourni, télécharge le modèle et commence à miner. Les doubles-clics suivants lancent le minage immédiatement. Windows
   peut d'abord afficher « Windows a protégé votre ordinateur » (le kit n'est pas signé numériquement) : cliquez sur
   **Informations complémentaires**, puis sur **Exécuter quand même**. L'activation de WSL demande une fois les droits
   d'administrateur et peut nécessiter un redémarrage ; l'installation continue ensuite d'elle-même.
3. Une version antérieure est détectée et mise à jour sur place, avec vos paramètres et le modèle.

S'il s'arrête ou mine lentement, consultez le [dépannage Windows](WINDOWS-TROUBLESHOOTING.fr.md) (DNS, lecteurs Windows,
la VRAM utilisée par les navigateurs et Docker Desktop, l'horloge).

## HiveOS (expérimental)

1. **Wallets (portefeuilles) :** ajoutez un portefeuille avec la monnaie `TSC` et votre adresse TensorCash (`tc1...`).
2. Ouvrez [`hiveos/flightsheet-thenewminer.json`](../../hiveos/flightsheet-thenewminer.json), copiez son contenu et
   remplacez `<YOUR_WALLET>` (en gardant `.%WORKER_NAME%` à la suite) et `<YOUR_POOL_PASSWORD>`. Dans HiveOS, choisissez
   Flight Sheets › **Import from clipboard**, collez le contenu, sélectionnez votre portefeuille `TSC` et enregistrez.
3. Appliquez la flight sheet à votre rig. Elle installe `thenewminer-0.1.47.tar.gz` et commence à miner.

Configuration manuelle, mises à jour et rig qui ne peut pas accéder à GitHub : voir le [guide](GUIDE.fr.md).

## Nouveautés de la v0.1.47

- **Installation plus facile :** sur une machine à un seul GPU, le mineur vérifie votre portefeuille et votre mot de
  passe du pool auprès du pool **avant** le téléchargement du modèle de 8.1 GiB, si bien qu'une faute de frappe est
  détectée en quelques secondes. Une adresse de portefeuille contenant une faute de frappe (sa somme de contrôle ne
  correspond pas) ou l'adresse d'exemple est refusée, avec l'explication de ce qui ne va pas.
- **Windows : identifiants demandés une seule fois,** dans une seule fenêtre au début, avant toute étape longue. Une
  installation faite par une version antérieure, y compris dans une autre distribution WSL, est détectée et **mise à jour
  sur place** (paramètres et modèle conservés). Si Ubuntu ne peut pas être téléchargé depuis Microsoft, l'installation
  récupère l'image WSL officielle d'Ubuntu sur un miroir en Chine et ne l'utilise que si son SHA-256 correspond.
- **Pas d'apt sur un Ubuntu standard :** Ubuntu 22.04 / 24.04 (y compris celui de WSL) dispose déjà de tout ce dont le
  mineur a besoin, donc rien n'est installé. S'il manque quelque chose, le premier démarrage l'énumère avant de
  l'installer.
- **Un paquet sur un lecteur Windows** (comme `/mnt/d` dans WSL) est refusé immédiatement, avec une ligne prête à coller
  qui le copie dans votre dossier personnel Linux et le démarre à cet endroit.
- **Des messages réseau clairs :** quand le DNS ne fonctionne pas dans WSL, l'installation s'arrête à cette étape en
  indiquant la solution ; un mineur qui ne peut pas joindre Internet ou le pool indique ce qu'il faut vérifier.
- **Le modèle est réutilisé :** une mise à jour dans un nouveau dossier trouve le modèle d'une installation antérieure
  (même taille et même SHA-256) et ne le télécharge pas à nouveau.
- **`--status` indique pourquoi** le mineur s'est arrêté et comment y remédier, y compris après un démarrage avec
  `--background`.
- **HiveOS :** chaque échec attend désormais et réessaie avec des pauses croissantes (d'une à dix minutes), et sa raison
  s'affiche dans le journal et sur le tableau de bord.

Notes complètes : [`docs/releases/RELEASE-NOTES-v0.1.47.md`](../releases/RELEASE-NOTES-v0.1.47.md).

## Problèmes connus

- **Sous Windows, les cartes de 10 GB ont besoin d'environ 9.3 GB de VRAM libre pour la pleine vitesse.** Avec moins, le
  mineur utilise une disposition plus lente (une RTX 3080 10 GB est passée de 21–22 à environ 9 N/s). Un navigateur avec
  l'accélération matérielle ou Docker Desktop peut occuper cette mémoire : fermez-les (ou désactivez l'accélération
  matérielle du navigateur), puis relancez le minage.
- **Horloge de Windows :** si le mineur signale une erreur d'horloge, ouvrez Paramètres Windows › Heure et langue › Date
  et heure, activez « Définir le fuseau horaire automatiquement » et « Régler l'heure automatiquement », cliquez sur
  **Synchroniser maintenant**, puis exécutez `wsl --shutdown` et redémarrez.

## Hashrates

Les meilleurs hashrates mesurés sur un vrai pool avec nos propres rigs. Ce sont des valeurs **maximales** (« jusqu'à »),
pas une promesse : des cartes du même modèle diffèrent d'environ 5–8%, et le hashrate baisse si la carte gère aussi un
écran ou si d'autres programmes utilisent le GPU.

| GPU | Hashrate |
|---|---|
| RTX 5090 | jusqu'à ~99 N/s |
| RTX 4090 | jusqu'à ~70 N/s |
| RTX 3080 10 GB | jusqu'à ~22 N/s |

Mesures datées par version : [résultats des tests](../TEST-RESULTS.md). Les grandes étapes du projet :
[historique du développement](../DEVELOPMENT-RECORD.md).

## Signaler un problème

Ouvrez une issue sur [GitHub Issues](https://github.com/ngasuka855/TheNewMiner/issues/new/choose) et remplissez le
formulaire de rapport de bug : la version de TheNewMiner, votre système, le ou les modèles de GPU et le pilote, la sortie
de `./thenewminer --status` et les dernières lignes du journal. Les issues sont publiques : **ne publiez jamais votre mot
de passe du pool ni la clé privée de votre portefeuille**, et retirez le mot de passe (et, si vous préférez, l'adresse de
votre portefeuille) de tout ce que vous collez.

## Note de sécurité

Le mineur et le moteur GPU sont livrés chiffrés (ChaCha20-Poly1305) dans le paquet ; le mineur les déchiffre sur votre
machine au démarrage. Cela rend leur copie plus coûteuse, mais ce n'est **pas inviolable** : toute personne disposant d'un
accès administrateur à une machine qui exécute le mineur peut, avec suffisamment d'efforts, récupérer la clé ou le code
déchiffré. L'adresse de votre portefeuille et le mot de passe de votre pool sont stockés sur votre propre machine et
envoyés uniquement au pool.
