[English](../WINDOWS-TROUBLESHOOTING.md) · [简体中文](WINDOWS-TROUBLESHOOTING.zh-CN.md) · [繁體中文](WINDOWS-TROUBLESHOOTING.zh-TW.md) · [Русский](WINDOWS-TROUBLESHOOTING.ru.md) · [Español](WINDOWS-TROUBLESHOOTING.es.md) · [Português (Brasil)](WINDOWS-TROUBLESHOOTING.pt-BR.md) · **Français** · [Deutsch](WINDOWS-TROUBLESHOOTING.de.md) · [Türkçe](WINDOWS-TROUBLESHOOTING.tr.md) · [Tiếng Việt](WINDOWS-TROUBLESHOOTING.vi.md) · [한국어](WINDOWS-TROUBLESHOOTING.ko.md) · [日本語](WINDOWS-TROUBLESHOOTING.ja.md)

> Cette traduction est fournie pour plus de commodité ; seul le README en anglais fait foi. *(This translation is provided for convenience; the English README is authoritative.)*

# Dépannage Windows

Sous Windows, TheNewMiner exécute le mineur Linux dans WSL2. Quand l'installation s'arrête, la fenêtre indique à quelle
étape et que faire, et `TheNewMiner.bat` l'affiche à nouveau (le message est aussi conservé dans
`%LOCALAPPDATA%\TheNewMiner\last-setup-failure.txt`). Cette page explique les cas courants. L'installation et les
commandes courantes se trouvent dans le [guide](GUIDE.fr.md).

## Le DNS ne fonctionne pas dans WSL

À l'étape 4, l'installation vérifie que WSL peut résoudre les noms du pool et du téléchargement du modèle. Si ce n'est
pas le cas, l'installation s'arrête là et ne modifie rien d'elle-même. Ouvrez `%UserProfile%\.wslconfig` dans le
Bloc-notes (créez le fichier s'il n'existe pas) et ajoutez ces deux lignes (sous Windows 11 22H2 ou plus récent,
`networkingMode=mirrored` sous `[wsl2]` fonctionne aussi) :

```ini
[wsl2]
dnsTunneling=true
```

Exécutez ensuite `wsl --shutdown` dans une Invite de commandes (cela arrête toutes les distributions WSL, y compris
celle de Docker Desktop) et double-cliquez à nouveau sur `TheNewMiner.bat`.

Un VPN ou un pare-feu qui bloque WSL peut provoquer le même message : autorisez WSL, ou définissez un proxy avec
`TheNewMiner.bat setup -Proxy ...` (voir « Proxy » dans le [guide](GUIDE.fr.md)).

## Ubuntu ne peut pas être téléchargé, ou la virtualisation est désactivée

- **Réseau :** l'installation installe Ubuntu 24.04 avec `wsl --install`. Là où ce téléchargement ne parvient pas à se
  connecter (par exemple là où GitHub est inaccessible), l'installation télécharge à la place l'image WSL officielle
  d'Ubuntu depuis un miroir public en Chine et ne l'utilise que si son SHA-256 correspond. Si aucun miroir ne répond non
  plus, vérifiez la connexion Internet et réessayez plus tard.
- **Virtualisation :** un message concernant la virtualisation (par exemple `0x80370102`) signifie qu'elle est
  désactivée. Activez Intel VT-x / AMD SVM dans le BIOS, puis double-cliquez à nouveau sur `TheNewMiner.bat`.

## Le paquet se trouve sur un lecteur Windows

Si vous avez décompressé vous-même le paquet Linux et lancé `./thenewminer` dans WSL depuis un lecteur Windows (comme
`/mnt/d/...`), le mineur refuse de démarrer : un lecteur Windows ne peut pas conserver son fichier de paramètres privé.
Il affiche une ligne `mkdir -p ~/thenewminer && cp -a ...` prête à coller, qui copie le dossier dans votre dossier
personnel Linux et le démarre à cet endroit. Vous pouvez aussi utiliser plutôt le zip Windows : `TheNewMiner.bat` met
tout au bon endroit de lui-même.

## Lent sur une carte de 10 GB (navigateurs, Docker Desktop)

Une carte de 10 GB n'obtient la disposition rapide que si environ 9.3 GB (9472 MiB) de VRAM sont libres au démarrage du
minage. Sous Windows, d'autres programmes partagent la carte : un navigateur avec l'accélération matérielle ou Docker
Desktop peut occuper assez de VRAM pour que le mineur se rabatte sur une disposition plus lente. Sur une RTX 3080 10 GB,
cela donnait environ 9 N/s au lieu de 21–22 N/s.

1. Fermez vos navigateurs, ou désactivez leur accélération matérielle (Chrome et Edge : Paramètres › Système ›
   « Utiliser l'accélération graphique si disponible » ; Firefox : Paramètres › Général › Performances), et quittez
   Docker Desktop.
2. Vérifiez la VRAM libre dans une Invite de commandes : `nvidia-smi --query-gpu=memory.used,memory.total --format=csv`
   (le total moins la mémoire utilisée doit être d'au moins 9472 MiB).
3. Relancez le minage : `TheNewMiner.bat stop`, puis double-cliquez sur `TheNewMiner.bat`.

Les cartes de 12 GB ont besoin des mêmes 9472 MiB libres ; les cartes de 16 GB ou plus ne sont pas concernées.

## Erreurs d'horloge

Le pool arrête un mineur dont l'horloge est décalée d'environ 30 secondes. L'horloge de WSL2 peut dériver, par exemple
après une mise en veille du PC, et l'installation avertit quand elle est décalée. Si le mineur signale une erreur
d'horloge :

1. Ouvrez Paramètres Windows › Heure et langue › Date et heure, activez « Définir le fuseau horaire automatiquement » et
   « Régler l'heure automatiquement », puis cliquez sur **Synchroniser maintenant**.
2. Dans une Invite de commandes, exécutez `wsl --shutdown`.
3. Relancez le minage.

## Alertes de sécurité de Windows

SmartScreen (« Windows a protégé votre ordinateur ») et les antivirus qui signalent le mineur : voir la FAQ du
[guide](GUIDE.fr.md).

## Toujours bloqué ?

Ouvrez une issue sur [GitHub Issues](https://github.com/ngasuka855/TheNewMiner/issues/new/choose) avec la sortie de
`TheNewMiner.bat status`, l'étape qui a échoué et le contenu de `last-setup-failure.txt`. Ne publiez jamais votre mot de
passe du pool ni la clé privée de votre portefeuille.
