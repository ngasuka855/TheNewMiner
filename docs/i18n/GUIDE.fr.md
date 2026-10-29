[English](../GUIDE.md) · [简体中文](GUIDE.zh-CN.md) · [繁體中文](GUIDE.zh-TW.md) · [Русский](GUIDE.ru.md) · [Español](GUIDE.es.md) · [Português (Brasil)](GUIDE.pt-BR.md) · **Français** · [Deutsch](GUIDE.de.md) · [Türkçe](GUIDE.tr.md) · [Tiếng Việt](GUIDE.vi.md) · [한국어](GUIDE.ko.md) · [日本語](GUIDE.ja.md)

> Cette traduction est fournie pour plus de commodité ; seul le README en anglais fait foi. *(This translation is provided for convenience; the English README is authoritative.)*

# Guide de TheNewMiner

Tout ce qui va au-delà du premier démarrage : vérifier le téléchargement, l'utilisation courante sous Linux, Windows et
HiveOS, les mises à jour, que faire quand il ne mine pas, et la FAQ. Les téléchargements et le démarrage rapide se
trouvent sur la [page principale](README.fr.md) ; les problèmes sous Windows ont leur propre page :
[dépannage Windows](WINDOWS-TROUBLESHOOTING.fr.md).

## Vérifier le téléchargement

| Fichier | SHA-256 |
|---|---|
| [`TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz) | `86c215dc9401209b8d2a2346252ed5cc1901527b6a51520372e3128583c63656` |
| [`TheNewMiner-v0.1.47-windows-wsl2-r3.zip`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-windows-wsl2-r3.zip) | `45a459646713bef9f8285848486c0a4bd66dee65e8c63090c8d3345c998b2659` |
| [`thenewminer-0.1.47.tar.gz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/thenewminer-0.1.47.tar.gz) (HiveOS) | `6b2515c5bf23f60daa236ff4a8b165ce5e55265d7eecac9dd408896e1e31b784` |

Le zip Windows est tout-en-un : il contient déjà le mineur Linux, vous n'avez donc besoin que du zip. Placez le ou les
paquets et [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/SHA256SUMS) dans le même dossier, puis :

```bash
# Linux (--ignore-missing : seuls les paquets que vous avez téléchargés sont vérifiés)
sha256sum -c --ignore-missing SHA256SUMS
# macOS
shasum -a 256 -c --ignore-missing SHA256SUMS
```

```bat
:: Windows (Invite de commandes) : comparez le résultat avec le SHA-256 du tableau ci-dessus
certutil -hashfile TheNewMiner-v0.1.47-windows-wsl2-r3.zip SHA256
```

Chaque fichier doit afficher `OK` (sous Windows : les mêmes 64 caractères que dans le tableau). S'il ne correspond pas,
ne l'utilisez pas.

## Linux

```bash
tar -xJf TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz
cd thenewminer
./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background
```

| Commande | Ce qu'elle fait |
|---|---|
| `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background` | Premier démarrage : installe ce qui est nécessaire, puis mine en arrière-plan |
| `./thenewminer --background` | Démarrages suivants : vos paramètres sont mémorisés |
| `./thenewminer` | Miner au premier plan (Ctrl+C arrête) |
| `./thenewminer --logs` | Suivre le mineur (Ctrl+C quitte l'affichage ; le minage continue) |
| `./thenewminer --status` | Indique s'il mine, son dernier hashrate et les parts acceptées / rejetées ; après un arrêt, pourquoi il s'est arrêté et comment y remédier |
| `./thenewminer --stop` | Arrêter le minage |
| `./thenewminer --help` | Toutes les options |

- `-u` est l'adresse de votre portefeuille TensorCash, `-p` le mot de passe de votre pool, `-w` le nom du worker affiché
  sur le pool (par défaut : le nom d'hôte de la machine ; 1–64 lettres, chiffres, `_`, `-` ou `.`).
- L'adresse du portefeuille est vérifiée avant tout démarrage : une adresse contenant une faute de frappe (sa somme de
  contrôle ne correspond pas) ou l'adresse d'exemple est refusée, avec l'explication de ce qui ne va pas.
- Le mot de passe du pool : 6–128 caractères, n'importe quel caractère visible, sans espaces. Mettez-le toujours entre
  guillemets simples sur la ligne de commande, p. ex. `-p 'my$pass!'` (un `'` à l'intérieur de guillemets simples s'écrit
  `'\''`).
- Les paramètres sont mémorisés dans `thenewminer/.miner/` (le fichier contenant le mot de passe n'est lisible que par
  vous). Le portefeuille et le mot de passe sont retirés immédiatement de la ligne de commande, ils n'apparaissent donc
  pas dans `ps` (votre shell peut toutefois conserver la commande dans son historique).
- Au premier démarrage d'une machine à un seul GPU, le mineur vérifie le portefeuille et le mot de passe auprès du pool
  **avant** le téléchargement du modèle, si bien qu'une faute de frappe est détectée en quelques secondes (un rig
  multi-GPU la détecte au début du minage). Si le pool les refuse, le mineur l'indique et s'arrête : vérifiez-les et
  redémarrez avec `-u` et `-p`.
- Le premier démarrage télécharge le modèle (environ 8.1 GiB, une seule fois), installe le moteur GPU pour votre carte et
  effectue un autotest sur la carte, puis commence à miner. Des lignes comme `[ RATE] ... N/s` indiquent qu'il mine. Le
  journal est `thenewminer/.miner/thenewminer.log`.
- Un Ubuntu 22.04 / 24.04 standard (y compris celui de WSL) dispose déjà de toutes les bibliothèques dont le mineur a
  besoin, donc rien n'est installé. S'il en manque une (images Docker, systèmes minimaux), le premier démarrage sous
  Ubuntu, Debian et HiveOS l'énumère et l'installe avec `apt` (nécessite sudo ou root) ; sur les autres systèmes, il vous
  indique laquelle installer.
- Rigs multi-GPU : pris en charge, chaque GPU mine ; cette version a été testée avec un GPU par machine.
- Dans Docker ou d'autres conteneurs disposant de peu de mémoire partagée, le mineur exécute ses fichiers programme
  depuis `thenewminer/.miner/run/` sur le disque et l'indique. Pour choisir un autre dossier, démarrez-le avec
  `TNPSEAL_RUNTIME_BASE=<folder> ./thenewminer` (un dossier où vous seul pouvez écrire : `chmod 700 <folder>`).
- **Mise à jour :** `./thenewminer --stop`, décompressez le nouveau paquet au même endroit (il se décompresse dans
  `thenewminer/` et conserve `thenewminer/.miner/` avec vos paramètres et le modèle), puis redémarrez-le. Si l'ancienne
  version tourne encore, la nouvelle refuse de démarrer et vous demande de l'arrêter d'abord. S'il est décompressé dans
  un nouveau dossier, il trouve le modèle de l'ancienne version (même taille et même SHA-256) et le réutilise : pas de
  second téléchargement de 8.1 GiB. Indiquez-y à nouveau `-u -p -w`.

### Quand il ne mine pas

`./thenewminer --status` indique pourquoi le mineur s'est arrêté et comment y remédier. Les messages les plus fréquents :

| Vous voyez | Que faire |
|---|---|
| `bash: ./thenewminer: Permission denied` | Le dossier n'autorise pas l'exécution de programmes (un montage `noexec`, comme certains `/tmp`, ou une clé USB). Décompressez-le dans votre dossier personnel : `cd ~ && tar -xJf <the package>.tar.xz && cd thenewminer` |
| `this package is on a FAT (vfat) drive` (ou exFAT / NTFS, ou `Windows (DrvFs, 9p)` : un lecteur Windows comme `/mnt/d` dans WSL) | Un tel lecteur ne peut pas conserver les paramètres privés du mineur (mode 600). Collez la ligne `mkdir -p ~/thenewminer && cp -a ...` qu'il affiche : elle copie le dossier dans votre dossier personnel Linux et le démarre à cet endroit |
| `this system has no xz` | `sudo apt-get install -y xz-utils` (en root : sans `sudo`), puis redémarrez |
| `This system is missing what TheNewMiner needs` | Exécutez la ligne `sudo apt-get install -y ...` qu'il affiche, puis redémarrez |
| `No NVIDIA GPU found on this machine` | Aucune carte NVIDIA n'est détectée. Vérifiez que la carte est bien enfichée et alimentée ; sur une machine cloud, choisissez-en une avec un GPU NVIDIA |
| `NVIDIA driver ... is too old` | Installez le pilote indiqué (580 ou plus récent), redémarrez la machine, puis relancez (voir « Mettre à jour le pilote NVIDIA » ci-dessous) |
| `the pool rejected the wallet / password` | Vérifiez `-u` et `-p` et que le compte existe sur le site web du pool, puis `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' --background` |
| `no download source answered` | Le téléchargement du modèle ne parvient pas à joindre Internet. Derrière un proxy : `https_proxy=http://<HOST>:<PORT> ./thenewminer --background` ; ou copiez le modèle (voir « Vous avez déjà le modèle ? » ci-dessous) |
| `cannot reach the pool` / `pool disconnected` | Vérifiez la connexion Internet, le DNS et que le pare-feu autorise le TCP 443 sortant ; si votre réseau bloque le pool, utilisez `--proxy` (voir « Proxy » ci-dessous). Il se reconnecte de lui-même |

### Démarrer après un redémarrage (facultatif)

Le mineur ne démarre pas de lui-même après un redémarrage (HiveOS s'en charge pour vous). Sous Linux, ajoutez soit une
ligne `crontab` :

```bash
crontab -e      # puis ajoutez cette ligne (votre dossier) :
@reboot cd /home/<YOU>/thenewminer && ./thenewminer --background
```

soit un service utilisateur systemd (`loginctl enable-linger <YOU>` une seule fois, pour qu'il démarre sans connexion) :

```ini
# ~/.config/systemd/user/thenewminer.service, puis : systemctl --user enable --now thenewminer
[Unit]
Description=TheNewMiner
After=network-online.target
[Service]
WorkingDirectory=%h/thenewminer
ExecStart=%h/thenewminer/thenewminer
ExecStop=%h/thenewminer/thenewminer --stop
Restart=on-failure
RestartSec=60
RestartPreventExitStatus=77
[Install]
WantedBy=default.target
```

Le code de sortie 77 signifie que le pool a refusé le portefeuille / le mot de passe : redémarrer n'y changerait rien,
le service s'arrête donc là.

## Windows

1. Décompressez l'intégralité de `TheNewMiner-v0.1.47-windows-wsl2-r3.zip` (clic droit › Extraire tout...) où vous
   voulez : n'importe quel dossier, n'importe quel lecteur ; les chemins contenant des espaces ou des caractères chinois
   sont acceptés. Ne le lancez pas depuis l'aperçu du zip.
2. Double-cliquez sur **`TheNewMiner.bat`**.
   - **La première fois :** UNE fenêtre demande d'abord votre compte sur le pool : l'adresse du portefeuille (commence
     par `tc1` ; sa somme de contrôle est vérifiée, et l'exemple affiché n'est pas un vrai portefeuille), le mot de passe
     du pool (6–128 caractères, n'importe quel caractère visible, sans espaces) et le nom du worker (le nom de
     l'ordinateur est prérempli). OK reste grisé tant que les trois ne sont pas corrects. Après OK et Oui, plus aucune
     question : il vérifie Windows, le pilote et le disque, active WSL2 si nécessaire (une seule demande administrateur ;
     un redémarrage peut être nécessaire une fois, puis l'installation reprend d'elle-même), indique où les données
     seront stockées, crée son propre environnement Linux (WSL), installe le mineur fourni, télécharge le modèle, puis
     commence à miner. Sans bureau (via SSH), les mêmes questions sont posées dans la console.
   - **Les fois suivantes :** double-cliquez et il commence immédiatement à miner avec vos paramètres enregistrés. Un
     raccourci « TheNewMiner » est également créé sur le bureau. Un nouveau double-clic alors qu'il mine déjà vous le
     signale et affiche le journal.
   - **Si l'installation s'arrête,** la fenêtre indique à quelle étape et que faire, et `TheNewMiner.bat` l'affiche à
     nouveau. Voir le [dépannage Windows](WINDOWS-TROUBLESHOOTING.fr.md).

Commandes courantes (ouvrez une Invite de commandes dans le dossier décompressé) :

| Commande | Ce qu'elle fait |
|---|---|
| `TheNewMiner.bat` | Installe si nécessaire, puis mine dans cette fenêtre (fermer la fenêtre arrête le minage) |
| `TheNewMiner.bat background` | Miner sans fenêtre |
| `TheNewMiner.bat logs` | Suivre le mineur (Ctrl+C quitte l'affichage ; le minage continue) |
| `TheNewMiner.bat status` | Indique s'il tourne, et les derniers hashrates |
| `TheNewMiner.bat stop` | Arrêter le minage |
| `TheNewMiner.bat setup` | Saisir à nouveau le portefeuille / mot de passe / nom du worker (le bouton « Cancel » les conserve) |
| `TheNewMiner.bat uninstall` | Supprime le mineur, son dossier de données et le raccourci (demande d'abord confirmation) |

- **Où vont les données :** le mineur et le modèle se trouvent dans un dossier de données choisi lors de la première
  installation (par défaut `TheNewMinerData` à la racine du lecteur où vous avez décompressé, p. ex.
  `D:\TheNewMinerData`), et non dans le dossier décompressé. Il faut environ 20 GB d'espace libre à cet endroit. Pour
  choisir un autre dossier : `TheNewMiner.bat setup -DataDir <folder>`.
- **Ubuntu :** l'installation installe Ubuntu 24.04 avec `wsl --install`. Quand ce téléchargement ne parvient pas à se
  connecter, elle télécharge l'image WSL officielle d'Ubuntu `ubuntu-24.04.5-wsl-amd64.wsl` depuis un miroir public en
  Chine (Tsinghua TUNA, puis USTC, puis Aliyun) et ne l'utilise que si son SHA-256 est exactement
  `bb415d824822c4b878125729af451a5d18fb13d1cf5cbed9a7393ad64ac6039e`.
- Si le pool refuse le portefeuille ou le mot de passe, la fenêtre l'indique et le minage s'arrête : exécutez
  `TheNewMiner.bat setup` pour les saisir à nouveau.
- **Mise à jour :** décompressez la nouvelle version n'importe où et double-cliquez sur son `TheNewMiner.bat`. Il trouve
  le mineur installé, conserve vos paramètres et le modèle, et installe la nouvelle version sans poser de questions. Une
  installation faite par une version antérieure ou à la main dans une autre distribution WSL (par exemple
  `Ubuntu-24.04`) est détectée et mise à jour là où elle se trouve : pas de seconde installation, pas de nouvel Ubuntu,
  pas de second téléchargement du modèle. Si elle ne peut pas y être mise à jour, l'installation s'arrête et en indique
  la raison. L'ancien dossier peut être supprimé.
- **Désinstallation :** `TheNewMiner.bat uninstall`. Elle ne supprime jamais une distribution WSL qui vous appartient.
- Le kit Windows est en anglais.

## HiveOS (expérimental)

La prise en charge de HiveOS est **expérimentale** (depuis la v0.1.45, qui a tourné sur un vrai rig HiveOS ; les
changements apportés depuis la v0.1.46 n'ont encore tourné sur aucun). Elle utilise le paquet custom-miner pour HiveOS
`thenewminer-0.1.47.tar.gz` (son SHA-256 figure dans le tableau sous « Vérifier le téléchargement »).

Configuration rapide (importer la flight sheet) :

1. **Wallets (portefeuilles) :** ajoutez un portefeuille avec la monnaie `TSC` et votre adresse TensorCash (`tc1...`).
   Cette étape n'est nécessaire qu'une fois : HiveOS exige qu'un portefeuille `TSC` soit sélectionné, et le mineur
   utilise l'adresse que vous indiquez dans la flight sheet.
2. Ouvrez [`hiveos/flightsheet-thenewminer.json`](../../hiveos/flightsheet-thenewminer.json) et copiez son contenu.
   Remplacez `<YOUR_WALLET>` par l'adresse de votre portefeuille (en gardant `.%WORKER_NAME%` à la suite) et
   `<YOUR_POOL_PASSWORD>` par le mot de passe de votre pool (dans le JSON, un `"` dans le mot de passe s'écrit `\"` et
   un `\` s'écrit `\\`). Dans HiveOS, choisissez Flight Sheets › **Import from clipboard**, collez le contenu,
   sélectionnez votre portefeuille `TSC` et enregistrez.
3. Appliquez la flight sheet à votre rig. Le rig télécharge le paquet et commence à miner.

Configuration manuelle à la place : créez une flight sheet avec Coin `TSC`, Wallet : votre portefeuille `TSC`,
Pool : « Configure in miner », Miner : « Custom », cliquez sur **Setup Miner Config** et remplissez :

| Champ | Valeur |
|---|---|
| Miner name | `thenewminer` (rempli automatiquement d'après l'URL d'installation) |
| Installation URL | `https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/thenewminer-0.1.47.tar.gz` |
| Hash algorithm | laisser vide |
| Wallet and worker template | `%WAL%.%WORKER_NAME%` |
| Pool URL | `tsc-wss.the-new.win` (ignoré : le pool est intégré) |
| Pass | le mot de passe de votre pool |
| Extra config arguments | vide, ou `--proxy <YOUR_PROXY>` (voir la section Proxy) |

- Le nom du worker sur le pool est le nom du rig dans HiveOS : utilisez uniquement des lettres, des chiffres, `_`, `-`
  ou `.` (sans espaces), jusqu'à 64 caractères. Pour utiliser un autre nom de worker, ajoutez `-w <RIG_NAME>` au champ
  Extra config arguments.
- Environ 12 GB doivent être libres sur le disque HiveOS pour le modèle et le mineur (vérifiez avec
  `df -h /hive/miners`).
- Le modèle (environ 8.1 GiB) est téléchargé une seule fois dans `/hive/miners/custom/.thenewminer-models` et conservé
  lors des mises à jour.
- Le portefeuille et le mot de passe parviennent au mineur par son environnement, et non par sa ligne de commande.
- **En cas d'échec** (par exemple si le pool refuse le portefeuille ou le mot de passe), le journal du mineur et le
  tableau de bord HiveOS en indiquent la raison, et le rig réessaie avec des pauses croissantes (d'une à dix minutes).
  Corrigez la flight sheet et appliquez-la : cela redémarre immédiatement le mineur.
- Journaux : l'écran du mineur dans HiveOS (`miner` dans le shell du rig) ou `/var/log/miner/custom/custom.log`.
- **Mise à jour :** changez les deux numéros de version dans l'URL d'installation (par exemple
  `.../raw/vX.Y.Z/thenewminer-X.Y.Z.tar.gz`) et appliquez la flight sheet. HiveOS n'installe que si le nom de fichier
  dans l'URL change.
- **Le premier démarrage** télécharge le modèle et installe le moteur GPU, ce qui prend quelques minutes.
- **Vous avez déjà le modèle ?** Copiez-le dans `/home/user/` sur le rig avant le premier démarrage (voir la section
  suivante).

**Si le rig ne peut pas accéder à GitHub :** téléchargez `thenewminer-0.1.47.tar.gz` sur un PC (vérifiez son SHA-256),
copiez-le sur le rig et décompressez-le à l'endroit où HiveOS conserve les mineurs personnalisés. Appliquez ensuite la
flight sheet comme d'habitude : HiveOS indique « Already installed » et ne le télécharge pas. Gardez exactement le nom
de fichier `thenewminer-0.1.47.tar.gz` (un navigateur peut ajouter « (1) ») et ne modifiez pas l'URL d'installation
dans la flight sheet : HiveOS compare ce nom.

```bash
# sur le PC
scp thenewminer-0.1.47.tar.gz user@<RIG_IP>:/tmp/
# sur le rig, en root (Hive Shell, ou SSH puis sudo -i)
mkdir -p /hive/miners/custom/downloads
mv /tmp/thenewminer-0.1.47.tar.gz /hive/miners/custom/downloads/
cd /hive/miners/custom && tar -xzf downloads/thenewminer-0.1.47.tar.gz
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
Invite de commandes (remplacez `D:\Models` par l'emplacement de votre copie). Si le téléchargement avait déjà commencé,
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

**Windows :** exécutez `TheNewMiner.bat setup`. Il ouvre la fenêtre de compte avec votre nom de worker prérempli ;
saisissez le portefeuille et le mot de passe du pool, puis démarrez-le comme d'habitude (double-clic sur
`TheNewMiner.bat`).

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

Ces commandes démarrent aussi le mineur : arrêtez d'abord le mineur, et ajoutez `--background` pour qu'il reste en
arrière-plan.

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

- **Ubuntu :** `sudo ubuntu-drivers install` installe le pilote recommandé (vérifiez ensuite qu'il s'agit de la version
  580 ou plus récente). Pour choisir vous-même la version :
  `sudo apt install nvidia-driver-580` (ou plus récent ; les cartes de la série RTX 50 ont besoin de la variante open, par
  exemple `nvidia-driver-580-open`). Redémarrez ensuite. Utilisez les paquets de pilote d'Ubuntu ou l'installateur `.run`
  de NVIDIA, jamais les deux.
- **Debian :** les paquets de pilote propres à Debian sont antérieurs à 580 (535 dans Debian 12, 550 dans Debian 13).
  Installez un pilote plus récent depuis le dépôt de NVIDIA comme décrit dans le
  [NVIDIA Driver Installation Guide](https://docs.nvidia.com/datacenter/tesla/driver-installation-guide/debian.html)
  (guide d'installation du pilote NVIDIA), puis redémarrez.
- **Windows :** mettez à jour avec la [NVIDIA app](https://www.nvidia.com/en-us/software/nvidia-app/) (anciennement
  GeForce Experience), ou téléchargez le pilote de votre carte sur [nvidia.com](https://www.nvidia.com/en-us/drivers/)
  (580 ou plus récent). WSL2 utilise ce pilote Windows : n'installez jamais de pilote NVIDIA pour Linux dans WSL.
- **HiveOS :** en tant que root (Hive Shell, ou SSH puis `sudo -i`), exécutez `nvidia-driver-update --list` pour voir
  les versions, puis par exemple `nvidia-driver-update 595` (n'importe quelle version 580 ou plus récente), puis
  `sreboot`.

## FAQ

**Windows affiche « Windows a protégé votre ordinateur » / « Éditeur inconnu ».**
Le kit n'est pas signé numériquement. Cliquez sur **Informations complémentaires**, puis sur **Exécuter quand même**.

**Mon antivirus signale ou supprime le mineur.**
Microsoft Defender et d'autres antivirus signalent souvent les mineurs de cryptomonnaie comme « applications
potentiellement indésirables », même un mineur légitime que vous avez choisi de lancer, car les logiciels malveillants
cachent fréquemment des mineurs. Si un fichier est mis en quarantaine, ouvrez Sécurité Windows › Protection contre les
virus et menaces › Historique de protection, sélectionnez l'élément TheNewMiner et choisissez **Autoriser** (ou ajoutez
le dossier décompressé aux Exclusions). Ne le faites que pour un paquet téléchargé depuis cette page dont le SHA-256
correspond.

**Le premier démarrage prend beaucoup de temps.**
Le premier démarrage télécharge le modèle d'environ 8.1 GiB et effectue un autotest GPU sur votre carte ; sous Windows,
il peut aussi installer WSL2. Cela n'arrive qu'une seule fois. Les démarrages suivants commencent à miner bien plus vite.

**Le mineur indique que le pool a refusé le portefeuille / le mot de passe.**
Vérifiez l'adresse du portefeuille et le mot de passe du pool (le compte doit exister sur le site web du pool), puis
redémarrez : Linux `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>'`, Windows `TheNewMiner.bat setup`. Le
mineur ne réessaie pas de lui-même avec des identifiants erronés.

**Est-ce qu'il mine ? Où sont les journaux ?**
Linux : `./thenewminer --status`, `./thenewminer --logs`, ou le fichier `thenewminer/.miner/thenewminer.log`.
Windows : `TheNewMiner.bat status` et `TheNewMiner.bat logs`.

**Ma RTX 3080 (10 GB) mine beaucoup plus lentement sous Windows que sous Linux.**
Sous Windows, d'autres programmes partagent la VRAM. Voir le [dépannage Windows](WINDOWS-TROUBLESHOOTING.fr.md) :
fermez les navigateurs qui utilisent l'accélération matérielle ainsi que Docker Desktop.
