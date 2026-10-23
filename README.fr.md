[English](README.md) · [简体中文](README.zh-CN.md) · [繁體中文](README.zh-TW.md) · [Русский](README.ru.md) · [Español](README.es.md) · [Português (Brasil)](README.pt-BR.md) · **Français** · [Deutsch](README.de.md) · [Türkçe](README.tr.md) · [Tiếng Việt](README.vi.md) · [한국어](README.ko.md) · [日本語](README.ja.md)

> Cette traduction est fournie pour plus de commodité ; seul le README en anglais fait foi. *(This translation is provided for convenience; the English README is authoritative.)*

# TheNewMiner

TheNewMiner est un mineur GPU pour **TensorCash**. Il fonctionne sur les cartes NVIDIA GeForce RTX des séries 30, 40 et 50
dotées de **10 GB de VRAM ou plus** et mine sur le pool **`tsc-wss.the-new.win`**, qui est intégré : vous n'avez pas à
saisir d'adresse de pool, et la connexion est directe.

Version actuelle : **v0.1.46** · Site web du pool : [tsc.the-new.win](https://tsc.the-new.win/#/home)

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
   sous Linux, les questions de `TheNewMiner.bat` sous Windows, ou la flight sheet sous HiveOS. Environ 3–5 minutes
   après le démarrage, votre rig apparaît sous **My Miners** sur le site web du pool.

Le pool ne peut pas récupérer un mot de passe du pool oublié, et l'adresse de paiement ne peut pas être modifiée par la
suite. Le mot de passe du pool n'est pas la clé privée de votre portefeuille : ne donnez jamais votre clé privée à
personne.

## Nouveautés de la v0.1.46

- **Moins d'échecs au premier démarrage sur les cartes de la série RTX 30 et sous HiveOS.** La vérification unique du
  GPU utilise désormais du code GPU précompilé pour toutes les familles de cartes prises en charge et s'exécute déjà une
  fois pendant l'installation ; le cache de compilation GPU est conservé entre les démarrages et les mises à jour. Si la
  vérification dépasse quand même le délai sur une carte de la série RTX 30, le mineur effectue cette étape sur le CPU
  et continue de miner. Si la configuration de la carte ne peut pas être préparée, le mineur en donne la raison sur une
  ligne `Reason:` (code de sortie 78).
- **HiveOS :** quand le mineur ne peut pas préparer sa configuration pour le GPU lors d'un premier démarrage (code de
  sortie 78), le rig attend désormais et réessaie avec des pauses croissantes (d'une à dix minutes) au lieu de
  redémarrer toutes les quelques secondes.
- **Des vagues plus régulières :** quand le checkpoint du pool change au milieu d'une vague, le travail de la vague
  suivante est désormais préparé à nouveau en arrière-plan. Sur une RTX 5090, cela a supprimé les attentes à ce
  moment-là (+0.3% de débit soutenu, résultats identiques).
- **Plus rien en mémoire après un mot de passe refusé :** `--status` et `--logs` après un arrêt ne laissent plus en
  mémoire les fichiers du programme décompressés (environ 1.5 GB).
- **Vous avez déjà le modèle ?** Copiez-le au lieu de le télécharger à nouveau : voir la section ci-dessous.

Les changements de la v0.1.45 sont décrits dans [`RELEASE-NOTES-v0.1.45.md`](RELEASE-NOTES-v0.1.45.md).

## Téléchargement

| Fichier | Pour | SHA-256 |
|---|---|---|
| [`TheNewMiner-v0.1.46-linux-x86_64-sealed.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.46/TheNewMiner-v0.1.46-linux-x86_64-sealed.tar.xz) | Linux x86_64 | `3585360e80993176475b6e400258888b4ddf8cb130ff97574e74e15cf1e248a8` |
| [`TheNewMiner-v0.1.46-windows-wsl2-r1.zip`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.46/TheNewMiner-v0.1.46-windows-wsl2-r1.zip) | Windows 10 / 11 (fonctionne via WSL2) | `dd9cb395e4012bfb81a5b9f0a1fdfd196955b781265a4a4e0d67266bd67b6a9e` |
| [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.46/SHA256SUMS) | sommes de contrôle de tous les paquets | |

Le zip Windows est tout-en-un : il contient déjà le mineur Linux, vous n'avez donc besoin que du zip.

## Vérifier le téléchargement

Placez le ou les paquets et `SHA256SUMS` dans le même dossier, puis :

```bash
# Linux (--ignore-missing : seuls les paquets que vous avez téléchargés sont vérifiés)
sha256sum -c --ignore-missing SHA256SUMS
# macOS
shasum -a 256 -c --ignore-missing SHA256SUMS
```

```bat
:: Windows (Invite de commandes) : comparez le résultat avec le SHA-256 du tableau ci-dessus
certutil -hashfile TheNewMiner-v0.1.46-windows-wsl2-r1.zip SHA256
```

Chaque fichier doit afficher `OK` (sous Windows : les mêmes 64 caractères que dans le tableau). S'il ne correspond pas, ne l'utilisez pas.

## Installation en un clic (Linux)

Créez d'abord votre compte sur le pool (voir « Avant de commencer » ci-dessus). Remplacez ensuite `<YOUR_WALLET>`,
`<YOUR_POOL_PASSWORD>` et `<RIG_NAME>` (y compris les `<` `>`) à la fin de cette commande par votre portefeuille, votre
mot de passe du pool et un nom pour ce rig, puis collez-la dans un terminal. Elle télécharge le paquet Linux et le `SHA256SUMS` de cette version (avec `curl`, ou
`wget` s'il n'y a pas de `curl`), vérifie le SHA-256, décompresse le mineur dans `~/thenewminer` et le démarre en
arrière-plan :

```bash
mkdir -p ~/thenewminer-download && cd ~/thenewminer-download && V=v0.1.46 && P=TheNewMiner-$V-linux-x86_64-sealed.tar.xz && U=https://github.com/ngasuka855/TheNewMiner/raw/$V && rm -f "$P" SHA256SUMS && if command -v curl >/dev/null 2>&1; then curl -fL -o "$P" "$U/$P" && curl -fL -o SHA256SUMS "$U/SHA256SUMS"; else wget -O "$P" "$U/$P" && wget -O SHA256SUMS "$U/SHA256SUMS"; fi && sha256sum -c --ignore-missing SHA256SUMS && tar -xJf "$P" -C ~ && cd ~/thenewminer && ./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w <RIG_NAME> --background
```

Elle s'arrête à la première étape qui échoue : une erreur de téléchargement ou un SHA-256 qui ne correspond pas ne lance
jamais rien (la vérification protège contre un téléchargement endommagé ; les deux fichiers proviennent de cette page).
Gardez les guillemets simples autour du mot de passe : à l'intérieur, chaque caractère est sûr (un `'` dans le mot de
passe s'écrit `'\''`). Si `tar` affiche `xz: Cannot exec`, installez xz (`sudo apt-get install -y xz-utils`) et relancez
la commande.

Pour mettre à jour une version déjà présente dans `~/thenewminer`, exécutez d'abord `~/thenewminer/thenewminer --stop`,
puis cette commande (vos paramètres et le modèle sont conservés). Si votre version précédente se trouve dans un autre
dossier, arrêtez-la d'abord à cet endroit, ou mettez-la à jour sur place comme décrit dans la section Linux (sinon le
modèle est téléchargé à nouveau).

## Linux

```bash
tar -xJf TheNewMiner-v0.1.46-linux-x86_64-sealed.tar.xz
cd thenewminer
./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background
```

| Commande | Ce qu'elle fait |
|---|---|
| `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background` | Premier démarrage : installe ce qui est nécessaire, puis mine en arrière-plan |
| `./thenewminer --background` | Démarrages suivants : vos paramètres sont mémorisés |
| `./thenewminer` | Miner au premier plan (Ctrl+C arrête) |
| `./thenewminer --logs` | Suivre le mineur (Ctrl+C quitte l'affichage ; le minage continue) |
| `./thenewminer --status` | Indique s'il mine, son dernier hashrate et les parts acceptées / rejetées |
| `./thenewminer --stop` | Arrêter le minage |
| `./thenewminer --help` | Toutes les options |

- `-u` est l'adresse de votre portefeuille TensorCash, `-p` le mot de passe de votre pool, `-w` le nom du worker affiché
  sur le pool (par défaut : le nom d'hôte de la machine ; 1–64 lettres, chiffres, `_`, `-` ou `.`).
- Le mot de passe du pool : 6–128 caractères, n'importe quel caractère visible, sans espaces. Mettez-le toujours entre
  guillemets simples sur la ligne de commande, p. ex. `-p 'my$pass!'` (un `'` à l'intérieur de guillemets simples s'écrit
  `'\''`).
- Les paramètres sont mémorisés dans `thenewminer/.miner/` (le fichier contenant le mot de passe n'est lisible que par
  vous). Le portefeuille et le mot de passe sont retirés immédiatement de la ligne de commande, ils n'apparaissent donc pas
  dans `ps` (votre shell peut toutefois conserver la commande dans son historique).
- Si le pool refuse le portefeuille ou le mot de passe, le mineur l'indique et
  s'arrête. Vérifiez-les et redémarrez avec `-u` et `-p`.
- Le premier démarrage télécharge le modèle (environ 8.1 GiB, une seule fois), installe le moteur GPU pour votre carte et
  effectue un autotest sur la carte, puis commence à miner. Des lignes comme `[ RATE] ... N/s` indiquent qu'il mine.
- Sous Ubuntu et Debian, le premier démarrage peut installer quelques bibliothèques système manquantes avec `apt`
  (nécessite sudo ou root) ; sur les autres systèmes, il vous indique lesquelles installer.
- Rigs multi-GPU : pris en charge, chaque GPU mine ; cette version a été testée avec un GPU par machine.
- Si `tar` affiche `xz: Cannot exec`, installez d'abord xz : `sudo apt-get install -y xz-utils`.
- Dans Docker ou d'autres conteneurs disposant de peu de mémoire partagée, le mineur exécute ses fichiers programme depuis
  `thenewminer/.miner/run/` sur le disque et l'indique. Pour choisir un autre dossier, démarrez-le avec
  `TNPSEAL_RUNTIME_BASE=<folder> ./thenewminer` (un dossier où vous seul pouvez écrire : `chmod 700 <folder>`).
- **Mise à jour :** `./thenewminer --stop`, décompressez le nouveau paquet au même endroit (il se décompresse dans
  `thenewminer/` et conserve `thenewminer/.miner/` avec vos paramètres et le modèle), puis redémarrez-le. Si l'ancienne
  version tourne encore, la nouvelle refuse de démarrer et vous demande de l'arrêter d'abord.

## Windows (en un clic)

1. Décompressez l'intégralité de `TheNewMiner-v0.1.46-windows-wsl2-r1.zip` (clic droit › Extraire tout...) où vous voulez :
   n'importe quel dossier, n'importe quel lecteur ; les chemins contenant des espaces ou des caractères chinois sont
   acceptés. Ne le lancez pas depuis l'aperçu du zip.
2. Double-cliquez sur **`TheNewMiner.bat`**.
   - **La première fois :** il vérifie Windows, le pilote et le disque, active WSL2 si nécessaire (une seule demande
     administrateur ; un redémarrage peut être nécessaire une fois, puis l'installation reprend d'elle-même), indique où
     les données seront stockées (Entrée pour accepter), crée son propre environnement Linux (WSL), installe le mineur
     fourni, demande **une seule fois** l'adresse de votre portefeuille (commence par `tc1`), le mot de passe du pool
     (saisi deux fois ; 6–128 caractères, n'importe quel caractère visible, sans espaces) et le nom du worker (Entrée = le
     nom de l'ordinateur), télécharge le modèle, puis commence à miner.
   - **Les fois suivantes :** double-cliquez et il commence immédiatement à miner avec vos paramètres enregistrés. Un
     raccourci « TheNewMiner » est également créé sur le bureau. Un nouveau double-clic alors qu'il mine déjà vous le
     signale et affiche le journal.

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
- Si le pool refuse le portefeuille ou le mot de passe, la fenêtre l'indique et le minage s'arrête : exécutez
  `TheNewMiner.bat setup` pour les saisir à nouveau.
- **Mise à jour :** décompressez la nouvelle version n'importe où et double-cliquez sur son `TheNewMiner.bat`. Il trouve le
  mineur installé, conserve vos paramètres et le modèle, et installe la nouvelle version sans poser de questions. L'ancien
  dossier peut être supprimé.
- **Désinstallation :** `TheNewMiner.bat uninstall`.
- La langue affichée suit celle de Windows (English / 简体中文 / 繁體中文). Pour la choisir, ajoutez `-Lang en` (ou `zh-Hans`,
  `zh-Hant`) à n'importe quelle commande, p. ex. `TheNewMiner.bat -Lang en` ; cela s'applique à cette exécution.

## HiveOS (expérimental)

La prise en charge de HiveOS est **expérimentale** (depuis la v0.1.45, qui a tourné sur un vrai rig HiveOS ; les
changements du premier démarrage de la v0.1.46 n'ont encore tourné sur aucun). Elle utilise le paquet custom-miner pour
HiveOS ci-dessous.

| Fichier | SHA-256 |
|---|---|
| [`thenewminer-0.1.46.tar.gz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.46/thenewminer-0.1.46.tar.gz) | `2284c258176e6b372bf56947a343116688e773dee2d479a7f5f125ad9f0a1915` |

Configuration rapide (importer la flight sheet) :

1. **Wallets (portefeuilles) :** ajoutez un portefeuille avec la monnaie `TSC` et votre adresse TensorCash (`tc1...`).
   Cette étape n'est nécessaire qu'une fois : HiveOS exige qu'un portefeuille `TSC` soit sélectionné, et le mineur
   utilise l'adresse que vous indiquez dans la flight sheet.
2. Ouvrez [`hiveos/flightsheet-thenewminer.json`](hiveos/flightsheet-thenewminer.json) et copiez son contenu. Remplacez
   `<YOUR_WALLET>` par l'adresse de votre portefeuille (en gardant `.%WORKER_NAME%` à la suite) et
   `<YOUR_POOL_PASSWORD>` par le mot de passe de votre pool (dans le JSON, un `"` dans le mot de passe s'écrit `\"` et
   un `\` s'écrit `\\`). Dans HiveOS, choisissez Flight Sheets › **Import from clipboard**, collez le contenu,
   sélectionnez votre portefeuille `TSC` et enregistrez.
3. Appliquez la flight sheet à votre rig. Le rig télécharge le paquet et commence à miner.

Configuration manuelle à la place : créez une flight sheet avec Coin `TSC`, Wallet : votre portefeuille `TSC`,
Pool : « Configure in miner », Miner : « Custom », cliquez sur **Setup Miner Config** et remplissez :

| Champ | Valeur |
|---|---|
| Miner name | `thenewminer` (rempli automatiquement d'après l'URL d'installation) |
| Installation URL | `https://github.com/ngasuka855/TheNewMiner/raw/v0.1.46/thenewminer-0.1.46.tar.gz` |
| Hash algorithm | laisser vide |
| Wallet and worker template | `%WAL%.%WORKER_NAME%` |
| Pool URL | `tsc-wss.the-new.win` (ignoré : le pool est intégré) |
| Pass | le mot de passe de votre pool |
| Extra config arguments | vide, ou `--proxy <YOUR_PROXY>` (voir la section Proxy) |

- Le nom du worker sur le pool est le nom du rig dans HiveOS : utilisez uniquement des lettres, des chiffres, `_`, `-`
  ou `.` (sans espaces), jusqu'à 64 caractères. Pour utiliser un autre nom de worker, ajoutez `-w <RIG_NAME>` au champ
  Extra config arguments.
- Environ 12 GB doivent être libres sur le disque HiveOS pour le modèle et le mineur (vérifiez avec `df -h /hive/miners`).
- Le modèle (environ 8.1 GiB) est téléchargé une seule fois dans `/hive/miners/custom/.thenewminer-models` et conservé lors
  des mises à jour.
- Le portefeuille et le mot de passe parviennent au mineur par son environnement, et non par sa ligne de commande.
- Si le pool refuse le portefeuille ou le mot de passe, le journal du mineur l'indique et le mineur attend 10 minutes
  avant que HiveOS ne le redémarre : corrigez la flight sheet et appliquez-la (cela redémarre immédiatement le mineur).
- Journaux : l'écran du mineur dans HiveOS (`miner` dans le shell du rig) ou `/var/log/miner/custom/custom.log`.
- **Mise à jour :** changez les deux numéros de version dans l'URL d'installation (par exemple
  `.../raw/vX.Y.Z/thenewminer-X.Y.Z.tar.gz`) et appliquez la flight sheet. HiveOS n'installe que si le nom de fichier
  dans l'URL change.
- **Le premier démarrage** télécharge le modèle et installe le moteur GPU, ce qui prend quelques minutes. Si le mineur
  ne peut ensuite pas préparer sa configuration pour le GPU, le journal en donne la raison sur une ligne `Reason:` et
  HiveOS réessaie avec des pauses croissantes (d'une à dix minutes).
- **Vous avez déjà le modèle ?** Copiez-le dans `/home/user/` sur le rig avant le premier démarrage (voir la section
  suivante).

**Si le rig ne peut pas accéder à GitHub :** téléchargez `thenewminer-0.1.46.tar.gz` sur un PC (vérifiez son SHA-256
dans le tableau ci-dessus), copiez-le sur le rig et décompressez-le à l'endroit où HiveOS conserve les mineurs
personnalisés. Appliquez ensuite la flight sheet comme d'habitude : HiveOS indique « Already installed » et ne le
télécharge pas. Gardez exactement le nom de fichier `thenewminer-0.1.46.tar.gz` (un navigateur peut ajouter « (1) ») et
ne modifiez pas l'URL d'installation dans la flight sheet : HiveOS compare ce nom.

```bash
# sur le PC
scp thenewminer-0.1.46.tar.gz user@<RIG_IP>:/tmp/
# sur le rig, en root (Hive Shell, ou SSH puis sudo -i)
mkdir -p /hive/miners/custom/downloads
mv /tmp/thenewminer-0.1.46.tar.gz /hive/miners/custom/downloads/
cd /hive/miners/custom && tar -xzf downloads/thenewminer-0.1.46.tar.gz
```

## Vous avez déjà le modèle ? Copiez-le

Le mineur a besoin d'un seul fichier de modèle de 8.1 GiB : `Qwen3-8B-Q8_0-full-v2.gguf` (`8709518112` octets, SHA-256
`408b955510e196121c1c375201744783b5c9a43c7956d73fc78df54c66e883d6`). S'il se trouve déjà sur cette machine, ou sur un
autre ordinateur de votre réseau, copiez-le au lieu de le télécharger à nouveau.

Avant de télécharger quoi que ce soit, le mineur cherche le fichier par son nom et sa taille exacts : dans son propre
dossier de modèles, dans les dossiers TheNewMiner précédents, dans votre dossier personnel (et un ou deux niveaux de
dossiers en dessous, ou un dossier `models` à cet endroit), dans les dossiers personnels sous `/home`, `/data`,
`/mnt/*`, `/media/*`, `/opt`, `/srv`, `/workspace`, `/var/lib`, ainsi que dans les caches Hugging Face / ModelScope.
S'il n'y trouve rien, il passe jusqu'à 20 secondes à chercher dans ces dossiers du même disque (les autres disques
montés ne sont vérifiés que par le nom) n'importe quel `.gguf` ayant exactement cette taille. Un fichier trouvé n'est
utilisé **que si son SHA-256 correspond** ; le mineur crée alors un lien vers lui (même disque) ou le copie dans son
propre dossier de modèles, si bien que chaque version ultérieure le trouve aussitôt. Vous pouvez aussi l'indiquer
directement : `./thenewminer --model /path/to/Qwen3-8B-Q8_0-full-v2.gguf`.

**Linux :** copiez-le dans votre dossier personnel, puis démarrez le mineur comme d'habitude. Depuis un Mac ou un PC du
même réseau (Windows 10 et 11 disposent de `scp` dans PowerShell), remplacez `user` et `<MINER_IP>` par l'identifiant et
l'adresse de votre mineur :

```bash
# sur le mineur lui-même
cp /path/to/Qwen3-8B-Q8_0-full-v2.gguf ~/
# ou depuis un autre ordinateur
scp Qwen3-8B-Q8_0-full-v2.gguf user@<MINER_IP>:~/
```

**HiveOS :** copiez-le dans `/home/user/` sur le rig (l'identifiant SSH de HiveOS est `user`), puis appliquez la flight
sheet. Le mineur l'y trouve, le vérifie et le conserve à côté du paquet (`/hive/miners/custom/.thenewminer-models`), si
bien qu'une nouvelle version ne le télécharge plus jamais. Le disque de HiveOS est petit : lancez d'abord `disk-expand`
si `df -h /hive/miners` indique moins de 12 GB libres.

```bash
scp Qwen3-8B-Q8_0-full-v2.gguf user@<RIG_IP>:/home/user/
```

**Windows :** une fois que le premier `setup` a créé la distribution WSL de TheNewMiner, copiez-y le fichier depuis une
invite de commandes (remplacez `D:\Models` par l'emplacement de votre copie). Si le téléchargement avait déjà commencé,
fermez la fenêtre, exécutez ces commandes, puis redémarrez TheNewMiner : il trouve la copie et saute le téléchargement.
La copie prend quelques minutes et n'affiche aucune progression.

```bat
wsl -d TheNewMiner -u root -- mkdir -p ~/thenewminer/models
wsl -d TheNewMiner -u root -- cp /mnt/d/Models/Qwen3-8B-Q8_0-full-v2.gguf ~/thenewminer/models/
```

## Changer de portefeuille, de mot de passe du pool ou de nom de worker

Le pool lie définitivement chaque adresse de paiement à son propre compte sur le pool : la page du pool
[tsc.the-new.win](https://tsc.the-new.win) qualifie l'adresse de paiement TSC de « 永久绑定 · 不可修改 » (liée définitivement,
non modifiable) et indique que, pour en changer, il faut utiliser directement la nouvelle adresse dans le mineur. Un
autre portefeuille nécessite donc son propre compte sur le pool et son propre mot de passe du pool (créez-les d'abord
sur la page du pool). Si vous changez le mot de passe du pool sur la page du pool, mettez-le à jour sur chaque mineur.
Le pool ne peut pas récupérer un mot de passe oublié.

**Linux :** arrêtez le mineur, puis démarrez-le une fois avec les nouveaux paramètres, qui remplacent les paramètres
mémorisés :

```bash
./thenewminer --stop
./thenewminer -u <NEW_WALLET> -p '<PASSWORD>' -w <RIG_NAME> --background
```

Indiquez `-u` et `-p` ensemble : l'un des deux seul ne change pas le portefeuille et le mot de passe mémorisés. Le nom
du worker peut être changé seul : `./thenewminer -w <RIG_NAME> --background` (après `--stop`).

**Windows :** exécutez `TheNewMiner.bat setup`. Il arrête le mineur en cours d'exécution et redemande le portefeuille,
le mot de passe du pool et le nom du worker. Démarrez-le ensuite comme d'habitude (double-clic sur `TheNewMiner.bat`).

**HiveOS :** modifiez la flight sheet : mettez la nouvelle adresse du portefeuille dans « Wallet and worker template »
(`<NEW_WALLET>.%WORKER_NAME%`) et le nouveau mot de passe du pool dans Pass, puis enregistrez-la et appliquez-la. Le nom
du worker est le nom du rig dans HiveOS (ou `-w <RIG_NAME>` dans Extra config arguments).

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
| GPU | NVIDIA GeForce RTX série 30 / 40 / 50, 10 GB de VRAM ou plus. Rigs multi-GPU : pris en charge, chaque GPU mine ; testé avec un GPU par machine dans cette version |
| Pilote | NVIDIA **580 ou plus récent** (`nvidia-smi` doit fonctionner). Sous Windows, installez-le dans Windows, jamais dans WSL |
| Linux | x86_64 avec **glibc 2.34 ou plus récent**, par exemple Ubuntu 22.04 ou plus récent ou Debian 12 ou plus récent. L'installateur vérifie ce dont il a besoin et indique tout ce qui manque |
| Windows | Windows 10 21H2 ou plus récent, ou Windows 11, 64 bits ; virtualisation (Intel VT-x / AMD SVM) activée dans le BIOS |
| Disque | environ 20 GB d'espace libre |
| RAM | 4 GB ou plus |
| Modèle | environ 8.1 GiB, téléchargé une seule fois au premier démarrage (en parallèle, multi-miroirs) puis réutilisé |

## Mettre à jour le pilote NVIDIA (580 ou plus récent)

Le mineur a besoin du pilote NVIDIA **580 ou plus récent**. Vérifiez lequel vous avez :

```bash
nvidia-smi    # l'en-tête affiche "Driver Version: ..." ; il doit être 580 ou plus récent
```

- **Ubuntu :** `sudo ubuntu-drivers install` installe le pilote recommandé (vérifiez ensuite qu'il s'agit
  de la version 580 ou plus récente). Pour choisir vous-même la version :
  `sudo apt install nvidia-driver-580` (ou plus récent ; les cartes de la série RTX 50 ont besoin de la variante open, par
  exemple `nvidia-driver-580-open`). Redémarrez ensuite. Utilisez les paquets de pilote d'Ubuntu ou l'installateur `.run`
  de NVIDIA, jamais les deux.
- **Debian :** les paquets de pilote propres à Debian sont antérieurs à 580 (535 dans Debian 12, 550 dans Debian 13).
  Installez un pilote plus récent depuis le
  dépôt de NVIDIA comme décrit dans le [NVIDIA Driver Installation Guide](https://docs.nvidia.com/datacenter/tesla/driver-installation-guide/debian.html)
  (guide d'installation du pilote NVIDIA), puis redémarrez.
- **Windows :** mettez à jour avec la [NVIDIA app](https://www.nvidia.com/en-us/software/nvidia-app/) (anciennement GeForce
  Experience), ou téléchargez le pilote de votre carte sur [nvidia.com](https://www.nvidia.com/en-us/drivers/) (580 ou
  plus récent). WSL2 utilise ce pilote Windows : n'installez jamais de pilote NVIDIA pour Linux dans WSL.
- **HiveOS :** en tant que root (Hive Shell, ou SSH puis `sudo -i`), exécutez `nvidia-driver-update --list` pour voir
  les versions, puis par exemple `nvidia-driver-update 595` (n'importe quelle version 580 ou plus récente), puis
  `sreboot`.

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

Tests sur un vrai pool, sur nos rigs de test (un GPU par machine). A = parts (shares) acceptées, R = parts rejetées.
« Minage stable » désigne le hashrate médian entre les changements de job.

| Date | Carte | OS | Paquet | Hashrate | A / R |
|---|---|---|---|---|---|
| 2026-10-23 | RTX 5090 | Linux | build final v0.1.46 | 95.41 N/s (minage stable, 12 min ; 91.70 en incluant les changements de job) | 10 / 0 (+1 périmée ; 0 invalide) |
| 2026-10-23 | RTX 4090 | Linux | build final v0.1.46 | 66.23 N/s (médiane, 10 min) | 2 / 0 |
| 2026-10-23 | RTX 4090 | Linux | build final v0.1.45 | 65.42 N/s (minage stable, 16 min ; 62.35 en incluant les changements de job) | 13 / 0 (0 invalide) |
| 2026-10-23 | RTX 5090 | Linux | build final v0.1.45 | 94.18 N/s (minage stable ; 92.24 en incluant les changements de job) | 17 / 0 (0 invalide) |
| 2026-10-23 | RTX 4090 | Linux | v0.1.44 sealed-r4 | 65.58 N/s (médiane, 16 min) | 10 / 0 |
| 2026-10-23 | RTX 5090 | Linux | v0.1.44 sealed-r4 | 93.45 N/s (minage stable, 44 min ; 90.39 en incluant les changements de job) | 31 / 0 (+1 périmée à l'arrêt) |
| 2026-10-23 | RTX 3080 10 GB | Windows (WSL2) | v0.1.44 windows-wsl2-r5 | ~22 N/s (exécution par l'opérateur lui-même) | n/d |
| 2026-10-23 | RTX 3080 10 GB | Linux (Ubuntu 26.04) | v0.1.44 sealed-r4 | 21.04 N/s (10 dernières min sur 23 min de minage) | 3 / 0 |

RTX 3080 sous Linux (v0.1.44) : installation 6/6 étapes OK, aucune fenêtre invalide, arrêt propre. Extrait du journal :
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

**Le mineur indique que le pool a refusé le portefeuille / le mot de passe.**
Vérifiez l'adresse du portefeuille et le mot de passe du pool (le compte doit exister sur le site web du pool), puis
redémarrez : Linux `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>'`, Windows `TheNewMiner.bat setup`. Le mineur
ne réessaie pas de lui-même avec des identifiants erronés.

**Est-ce qu'il mine ? Où sont les journaux ?**
Linux : `./thenewminer --status`, `./thenewminer --logs`, ou le fichier `thenewminer/.miner/thenewminer.log`.
Windows : `TheNewMiner.bat status` et `TheNewMiner.bat logs`.

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
déchiffré.
L'adresse de votre portefeuille et le mot de passe de votre pool sont stockés sur votre propre machine et envoyés uniquement
au pool.
