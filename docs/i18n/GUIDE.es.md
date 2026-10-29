[English](../GUIDE.md) · [简体中文](GUIDE.zh-CN.md) · [繁體中文](GUIDE.zh-TW.md) · [Русский](GUIDE.ru.md) · **Español** · [Português (Brasil)](GUIDE.pt-BR.md) · [Français](GUIDE.fr.md) · [Deutsch](GUIDE.de.md) · [Türkçe](GUIDE.tr.md) · [Tiếng Việt](GUIDE.vi.md) · [한국어](GUIDE.ko.md) · [日本語](GUIDE.ja.md)

> Esta traducción se ofrece por comodidad; el README en inglés es la versión de referencia. *(This translation is provided for convenience; the English README is authoritative.)*

# Guía de TheNewMiner

Todo lo que va más allá del primer inicio: comprobar la descarga, el uso diario en Linux, Windows y HiveOS, las
actualizaciones, qué hacer cuando no mina y las preguntas frecuentes. Las descargas y el inicio rápido están en la
[página principal](README.es.md); los problemas en Windows tienen su propia página:
[solución de problemas en Windows](WINDOWS-TROUBLESHOOTING.es.md).

## Verificar la descarga

| Archivo | SHA-256 |
|---|---|
| [`TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz) | `86c215dc9401209b8d2a2346252ed5cc1901527b6a51520372e3128583c63656` |
| [`TheNewMiner-v0.1.47-windows-wsl2-r3.zip`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-windows-wsl2-r3.zip) | `45a459646713bef9f8285848486c0a4bd66dee65e8c63090c8d3345c998b2659` |
| [`thenewminer-0.1.47.tar.gz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/thenewminer-0.1.47.tar.gz) (HiveOS) | `6b2515c5bf23f60daa236ff4a8b165ce5e55265d7eecac9dd408896e1e31b784` |

El zip de Windows es todo en uno: ya contiene el minero para Linux, así que solo necesitas el zip. Coloca el paquete (o
los paquetes) y [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/SHA256SUMS) en la misma carpeta y
luego:

```bash
# Linux (--ignore-missing: solo se comprueban los paquetes que descargaste)
sha256sum -c --ignore-missing SHA256SUMS
# macOS
shasum -a 256 -c --ignore-missing SHA256SUMS
```

```bat
:: Windows (Símbolo del sistema): compara el resultado con el SHA-256 de la tabla anterior
certutil -hashfile TheNewMiner-v0.1.47-windows-wsl2-r3.zip SHA256
```

Cada archivo debe indicar `OK` (en Windows: los mismos 64 caracteres que en la tabla). Si no coincide, no lo uses.

## Linux

```bash
tar -xJf TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz
cd thenewminer
./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background
```

| Comando | Qué hace |
|---|---|
| `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background` | Primer inicio: instala lo necesario y luego mina en segundo plano |
| `./thenewminer --background` | Inicios posteriores: tu configuración se recuerda |
| `./thenewminer` | Minar en primer plano (Ctrl+C lo detiene) |
| `./thenewminer --logs` | Ver el minero (Ctrl+C sale de la vista; la minería continúa) |
| `./thenewminer --status` | Si está minando, su último hashrate y los shares aceptados / rechazados; tras una parada, por qué se detuvo y cómo solucionarlo |
| `./thenewminer --stop` | Detener la minería |
| `./thenewminer --help` | Todas las opciones |

- `-u` es la dirección de tu billetera de TensorCash, `-p` tu contraseña del pool y `-w` el nombre del worker que se
  muestra en el pool (predeterminado: el nombre de host de la máquina; de 1 a 64 letras, dígitos, `_`, `-` o `.`).
- La dirección de la billetera se comprueba antes de iniciar nada: una dirección con un error de escritura (su suma de
  verificación no coincide) o la dirección de ejemplo se rechaza, indicando qué está mal.
- La contraseña del pool: de 6 a 128 caracteres, cualquier carácter visible, sin espacios. Ponla siempre entre comillas
  simples en la línea de comandos, p. ej. `-p 'my$pass!'` (una `'` dentro de comillas simples se escribe `'\''`).
- La configuración se recuerda en `thenewminer/.miner/` (el archivo con la contraseña solo puedes leerlo tú). La billetera
  y la contraseña se retiran de la línea de comandos de inmediato, así que no aparecen en `ps` (tu shell puede seguir
  guardando el comando en su historial).
- En el primer inicio de una máquina con una sola GPU, el minero comprueba la billetera y la contraseña con el pool
  **antes** de descargar el modelo, así que un error de escritura se detecta en segundos (un rig con varias GPU lo
  detecta al empezar a minar). Si el pool las rechaza, el minero lo indica y se detiene: revísalas y vuelve a iniciarlo
  con `-u` y `-p`.
- El primer inicio descarga el modelo (unos 8.1 GiB, una sola vez), instala el motor de GPU para tu tarjeta y ejecuta
  una autocomprobación en la tarjeta; después empieza a minar. Las líneas como `[ RATE] ... N/s` indican que está
  minando. El log es `thenewminer/.miner/thenewminer.log`.
- Un Ubuntu 22.04 / 24.04 estándar (incluido el de WSL) ya tiene todas las bibliotecas que necesita el minero, así que no
  se instala nada. Si falta alguna (imágenes de Docker, sistemas mínimos), el primer inicio en Ubuntu, Debian y HiveOS la
  enumera y la instala con `apt` (requiere sudo o root); en otros sistemas te indica cuál debes instalar.
- Rigs con varias GPU: compatibles, cada GPU mina; esta versión se probó con una GPU por máquina.
- En Docker u otros contenedores con poca memoria compartida, el minero ejecuta sus archivos de programa desde
  `thenewminer/.miner/run/` en el disco y lo indica. Para elegir otra carpeta, inícialo con
  `TNPSEAL_RUNTIME_BASE=<folder> ./thenewminer` (una carpeta en la que solo tú puedas escribir: `chmod 700 <folder>`).
- **Actualizar:** `./thenewminer --stop`, descomprime el nuevo paquete en el mismo lugar (se descomprime en
  `thenewminer/` y conserva `thenewminer/.miner/` con tu configuración y el modelo) y vuelve a iniciarlo. Si la versión
  anterior todavía se está ejecutando, la nueva se niega a iniciarse y te pide que la detengas primero. Si en cambio la
  descomprimes en una carpeta nueva, encuentra el modelo de la anterior (mismo tamaño y mismo SHA-256) y lo reutiliza:
  no hay una segunda descarga de 8.1 GiB. Allí vuelve a indicar `-u -p -w`.

### Cuando no mina

`./thenewminer --status` indica por qué se detuvo el minero y cómo solucionarlo. Los mensajes más habituales:

| Ves | Qué hacer |
|---|---|
| `bash: ./thenewminer: Permission denied` | La carpeta no permite ejecutar programas (un montaje `noexec`, como algunos `/tmp`, o una memoria USB). Descomprímelo en tu carpeta personal: `cd ~ && tar -xJf <the package>.tar.xz && cd thenewminer` |
| `this package is on a FAT (vfat) drive` (o exFAT / NTFS, o `Windows (DrvFs, 9p)`: una unidad de Windows como `/mnt/d` en WSL) | Una unidad así no puede conservar la configuración privada del minero (modo 600). Pega la línea `mkdir -p ~/thenewminer && cp -a ...` que muestra: copia la carpeta a tu carpeta personal de Linux y la inicia allí |
| `this system has no xz` | `sudo apt-get install -y xz-utils` (como root: sin `sudo`) y vuelve a iniciarlo |
| `This system is missing what TheNewMiner needs` | Ejecuta la línea `sudo apt-get install -y ...` que muestra y vuelve a iniciarlo |
| `No NVIDIA GPU found on this machine` | No se detecta ninguna tarjeta NVIDIA. Comprueba que la tarjeta esté bien insertada y alimentada; en una máquina en la nube, elige una con GPU NVIDIA |
| `NVIDIA driver ... is too old` | Instala el controlador que indica (580 o posterior), reinicia el equipo y vuelve a iniciarlo (consulta "Actualiza el controlador de NVIDIA" más abajo) |
| `the pool rejected the wallet / password` | Revisa `-u` y `-p` y que la cuenta exista en el sitio web del pool; luego `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' --background` |
| `no download source answered` | La descarga del modelo no puede llegar a internet. Detrás de un proxy: `https_proxy=http://<HOST>:<PORT> ./thenewminer --background`; o copia el modelo (consulta "¿Ya tienes el modelo?" más abajo) |
| `cannot reach the pool` / `pool disconnected` | Comprueba la conexión a internet, el DNS y que el firewall permita el tráfico TCP 443 saliente; si tu red bloquea el pool, usa `--proxy` (consulta "Proxy" más abajo). Se vuelve a conectar solo |

### Iniciar tras un reinicio (opcional)

El minero no se inicia solo después de un reinicio (HiveOS lo hace por ti). En Linux, añade una línea de `crontab`:

```bash
crontab -e      # y añade esta línea (tu carpeta):
@reboot cd /home/<YOU>/thenewminer && ./thenewminer --background
```

o bien un servicio de usuario de systemd (`loginctl enable-linger <YOU>` una vez, para que se inicie sin iniciar
sesión):

```ini
# ~/.config/systemd/user/thenewminer.service, y luego: systemctl --user enable --now thenewminer
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

El código de salida 77 significa que el pool rechazó la billetera / contraseña: reiniciar no puede ayudar, así que el
servicio se detiene ahí.

## Windows

1. Descomprime todo `TheNewMiner-v0.1.47-windows-wsl2-r3.zip` (clic derecho › Extraer todo) donde quieras: cualquier
   carpeta, cualquier unidad; las rutas con espacios o caracteres chinos funcionan bien. No lo ejecutes desde la vista
   del zip.
2. Haz doble clic en **`TheNewMiner.bat`**.
   - **La primera vez:** UNA ventana te pide primero tu cuenta del pool: la dirección de la billetera (empieza por `tc1`;
     se comprueba su suma de verificación, y el ejemplo que se muestra no es una billetera real), la contraseña del pool
     (de 6 a 128 caracteres, cualquier carácter visible, sin espacios) y el nombre del worker (se rellena con el nombre
     del equipo). OK permanece en gris hasta que los tres datos son correctos. Después de OK y Sí no se pregunta nada
     más: comprueba Windows, el controlador y el disco, activa WSL2 si hace falta (una solicitud de administrador; puede
     que haga falta reiniciar una vez, y después la instalación continúa sola), muestra dónde se guardarán los datos,
     crea su propio entorno Linux (WSL), instala el minero incluido, descarga el modelo y empieza a minar. Sin
     escritorio (por SSH), las mismas preguntas aparecen en la consola.
   - **Las veces siguientes:** haz doble clic y empieza a minar de inmediato con tu configuración guardada. También se
     crea un acceso directo "TheNewMiner" en el escritorio. Si vuelves a hacer doble clic mientras ya está minando, te
     lo indica y muestra el log.
   - **Si la instalación se detiene,** la ventana indica en qué paso y qué hacer, y `TheNewMiner.bat` lo vuelve a
     mostrar. Consulta la [solución de problemas en Windows](WINDOWS-TROUBLESHOOTING.es.md).

Comandos de uso diario (abre un Símbolo del sistema en la carpeta descomprimida):

| Comando | Qué hace |
|---|---|
| `TheNewMiner.bat` | Instala si hace falta y luego mina en esta ventana (cerrar la ventana detiene la minería) |
| `TheNewMiner.bat background` | Minar sin ventana |
| `TheNewMiner.bat logs` | Ver el minero (Ctrl+C sale de la vista; la minería continúa) |
| `TheNewMiner.bat status` | Si está en ejecución y los últimos hashrates |
| `TheNewMiner.bat stop` | Detener la minería |
| `TheNewMiner.bat setup` | Volver a introducir la billetera / contraseña / nombre del worker (el botón "Cancel" los conserva) |
| `TheNewMiner.bat uninstall` | Quitar el minero, su carpeta de datos y el acceso directo (pregunta antes) |

- **Dónde se guardan los datos:** el minero y el modelo se guardan en una carpeta de datos elegida en la primera
  instalación (por defecto `TheNewMinerData` en la raíz de la unidad donde descomprimiste, p. ej. `D:\TheNewMinerData`),
  no en la carpeta descomprimida. Allí se necesitan unos 20 GB libres. Para elegir otra carpeta:
  `TheNewMiner.bat setup -DataDir <folder>`.
- **Ubuntu:** la instalación instala Ubuntu 24.04 con `wsl --install`. Cuando esa descarga no puede conectarse, descarga
  la imagen WSL oficial de Ubuntu `ubuntu-24.04.5-wsl-amd64.wsl` de un mirror público en China (Tsinghua TUNA, luego
  USTC, luego Aliyun) y solo la usa si su SHA-256 es exactamente
  `bb415d824822c4b878125729af451a5d18fb13d1cf5cbed9a7393ad64ac6039e`.
- Si el pool rechaza la billetera o la contraseña, la ventana lo indica y la minería se detiene: ejecuta
  `TheNewMiner.bat setup` para volver a introducirlas.
- **Actualizar:** descomprime la nueva versión donde quieras y haz doble clic en su `TheNewMiner.bat`. Encuentra el
  minero instalado, conserva tu configuración y el modelo e instala la nueva versión sin hacer preguntas. Una
  instalación hecha por una versión anterior o a mano dentro de otra distribución de WSL (por ejemplo `Ubuntu-24.04`) se
  detecta y se actualiza donde está: sin una segunda instalación, sin un Ubuntu nuevo y sin una segunda descarga del
  modelo. Si no se puede actualizar allí, la instalación se detiene e indica el motivo. La carpeta antigua se puede
  eliminar.
- **Desinstalar:** `TheNewMiner.bat uninstall`. Nunca elimina una distribución de WSL que sea tuya.
- El kit de Windows está en inglés.

## HiveOS (experimental)

La compatibilidad con HiveOS es **experimental** (desde la v0.1.45, que se ejecutó en un rig HiveOS real; los cambios
desde la v0.1.46 aún no se han ejecutado en uno). Usa el paquete de minero personalizado (custom miner) para HiveOS
`thenewminer-0.1.47.tar.gz` (su SHA-256 está en la tabla de "Verificar la descarga").

Configuración rápida (importar el flight sheet):

1. **Wallets:** añade una billetera con la moneda `TSC` y tu dirección de TensorCash (`tc1...`). Solo hace falta una
   vez: HiveOS necesita una billetera `TSC` seleccionada, y el minero usa la dirección que pongas en el flight sheet.
2. Abre [`hiveos/flightsheet-thenewminer.json`](../../hiveos/flightsheet-thenewminer.json) y copia su contenido.
   Sustituye `<YOUR_WALLET>` por la dirección de tu billetera (mantén `.%WORKER_NAME%` detrás) y `<YOUR_POOL_PASSWORD>`
   por tu contraseña del pool (en el JSON, una `"` de la contraseña se escribe `\"` y una `\` se escribe `\\`). En
   HiveOS elige Flight Sheets › **Import from clipboard**, pégalo, selecciona tu billetera `TSC` y guárdalo.
3. Aplica el flight sheet a tu rig. El rig descarga el paquete y empieza a minar.

Configuración manual en su lugar: crea un flight sheet con Coin `TSC`, Wallet: tu billetera `TSC`, Pool: "Configure in
miner", Miner: "Custom", haz clic en **Setup Miner Config** y rellena:

| Campo | Valor |
|---|---|
| Miner name | `thenewminer` (se rellena a partir de la Installation URL) |
| Installation URL | `https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/thenewminer-0.1.47.tar.gz` |
| Hash algorithm | déjalo vacío |
| Wallet and worker template | `%WAL%.%WORKER_NAME%` |
| Pool URL | `tsc-wss.the-new.win` (se ignora: el pool viene integrado) |
| Pass | tu contraseña del pool |
| Extra config arguments | vacío, o `--proxy <YOUR_PROXY>` (consulta la sección Proxy) |

- El nombre del worker en el pool es el nombre del rig en HiveOS: usa solo letras, dígitos, `_`, `-` o `.` (sin
  espacios), hasta 64 caracteres. Para usar otro nombre de worker, añade `-w <RIG_NAME>` al campo Extra config
  arguments.
- Se necesitan unos 12 GB libres en la unidad de HiveOS para el modelo y el minero (compruébalo con
  `df -h /hive/miners`).
- El modelo (unos 8.1 GiB) se descarga una vez en `/hive/miners/custom/.thenewminer-models` y se conserva al actualizar.
- La billetera y la contraseña llegan al minero a través de su entorno, no de su línea de comandos.
- **Cuando algo falla** (por ejemplo, el pool rechaza la billetera o la contraseña), el log del minero y el panel de
  HiveOS muestran el motivo, y el rig lo vuelve a intentar con pausas crecientes (de uno a diez minutos). Corrige el
  flight sheet y aplícalo: eso reinicia el minero de inmediato.
- Logs: la pantalla del minero en HiveOS (`miner` en la shell del rig) o `/var/log/miner/custom/custom.log`.
- **Actualizar:** cambia los dos números de versión en la Installation URL (por ejemplo
  `.../raw/vX.Y.Z/thenewminer-X.Y.Z.tar.gz`) y aplica el flight sheet. HiveOS solo instala cuando cambia el nombre del
  archivo en la URL.
- **El primer arranque** descarga el modelo e instala el motor de GPU, lo que tarda unos minutos.
- **¿Ya tienes el modelo?** Cópialo a `/home/user/` en el rig antes del primer arranque (consulta la sección siguiente).

**Si el rig no puede acceder a GitHub:** descarga `thenewminer-0.1.47.tar.gz` en un PC (comprueba su SHA-256), cópialo
al rig y descomprímelo donde HiveOS guarda los mineros personalizados. Después aplica el flight sheet como siempre:
HiveOS indica "Already installed" y no lo descarga. Mantén exactamente el nombre de archivo `thenewminer-0.1.47.tar.gz`
(un navegador puede añadir " (1)") y no cambies la Installation URL del flight sheet: HiveOS compara ese nombre.

```bash
# en el PC
scp thenewminer-0.1.47.tar.gz user@<RIG_IP>:/tmp/
# en el rig, como root (Hive Shell, o SSH y luego sudo -i)
mkdir -p /hive/miners/custom/downloads
mv /tmp/thenewminer-0.1.47.tar.gz /hive/miners/custom/downloads/
cd /hive/miners/custom && tar -xzf downloads/thenewminer-0.1.47.tar.gz
```

## ¿Ya tienes el modelo? Cópialo

El minero necesita un único archivo de modelo de 8.1 GiB: `Qwen3-8B-Q8_0-full-v2.gguf` (`8709518112` bytes, SHA-256
`408b955510e196121c1c375201744783b5c9a43c7956d73fc78df54c66e883d6`). Si ya está en esta máquina, o en otro ordenador de
tu red, cópialo en lugar de volver a descargarlo.

Antes de descargar nada, el minero busca el archivo por su nombre y tamaño exactos: en su propia carpeta de modelos, en
carpetas anteriores de TheNewMiner, en tu carpeta personal (y una o dos carpetas por debajo de ella, o en una carpeta
`models` allí), en las carpetas personales dentro de `/home`, en `/data`, `/mnt/*`, `/media/*`, `/opt`,
`/srv`, `/workspace`, `/var/lib` y en las cachés de Hugging Face / ModelScope. Si no encuentra nada ahí, dedica hasta 20
segundos a buscar en esas carpetas del mismo disco (los demás discos montados solo se comprueban por nombre) cualquier
`.gguf` de exactamente ese tamaño. Un archivo que encuentre se usa **solo si su SHA-256 coincide**; entonces lo enlaza
(mismo disco) o lo copia en su propia carpeta de modelos, de modo que cada versión posterior lo encuentra al instante.
También puedes indicarlo directamente: `./thenewminer --model /path/to/Qwen3-8B-Q8_0-full-v2.gguf`.

**Linux:** cópialo en tu carpeta personal y luego inicia el minero como siempre. Desde un Mac o un PC de la misma red
(Windows 10 y 11 tienen `scp` en PowerShell), sustituye `user` y `<MINER_IP>` por el usuario y la dirección de tu
minero:

```bash
# en el propio minero
cp /path/to/Qwen3-8B-Q8_0-full-v2.gguf ~/
# o desde otro ordenador
scp Qwen3-8B-Q8_0-full-v2.gguf user@<MINER_IP>:~/
```

**HiveOS:** cópialo a `/home/user/` del rig (el usuario SSH de HiveOS es `user`) y luego aplica el flight sheet. El
minero lo encuentra ahí, lo comprueba y lo guarda junto al paquete (`/hive/miners/custom/.thenewminer-models`), así que
una versión nueva nunca vuelve a descargarlo. El disco de HiveOS es pequeño: ejecuta primero `disk-expand` si
`df -h /hive/miners` muestra menos de 12 GB libres.

```bash
scp Qwen3-8B-Q8_0-full-v2.gguf user@<RIG_IP>:/home/user/
```

**Windows:** después de que el primer `setup` haya creado la distribución WSL de TheNewMiner, copia el archivo en ella
desde un Símbolo del sistema (cambia `D:\Models` por la ubicación de tu copia). Si la descarga ya había empezado, cierra
la ventana, ejecuta estos comandos y vuelve a iniciar TheNewMiner: encuentra la copia y se salta la descarga. La copia
tarda unos minutos y no muestra el progreso.

```bat
wsl -d TheNewMiner -u root -- mkdir -p ~/thenewminer/models
wsl -d TheNewMiner -u root -- cp /mnt/d/Models/Qwen3-8B-Q8_0-full-v2.gguf ~/thenewminer/models/
```

## Cambiar la billetera, la contraseña del pool o el nombre del worker

El pool vincula de forma permanente cada dirección de pago a su propia cuenta del pool: la página del pool
[tsc.the-new.win](https://tsc.the-new.win) describe la dirección de pago de TSC como "永久绑定 · 不可修改" (vinculada
permanentemente, no se puede modificar) e indica que, para cambiarla, uses la nueva dirección directamente en el minero.
Por lo tanto, otra billetera necesita su propia cuenta del pool y su propia contraseña del pool (créalas primero en la
página del pool). Si cambias la contraseña del pool en la página del pool, actualízala en todos tus mineros. El pool no
puede recuperar una contraseña olvidada.

**Linux:** detén el minero y luego inícialo una vez con la nueva configuración; esta reemplaza la configuración
recordada:

```bash
./thenewminer --stop
./thenewminer -u <NEW_WALLET> -p '<PASSWORD>' -w <RIG_NAME> --background
```

Indica `-u` y `-p` juntos: uno solo de ellos no cambia la billetera ni la contraseña recordadas. El nombre del worker se
puede cambiar por separado: `./thenewminer -w <RIG_NAME> --background` (después de `--stop`).

**Windows:** ejecuta `TheNewMiner.bat setup`. Abre la ventana de cuenta con tu nombre de worker ya rellenado; introduce
la billetera y la contraseña del pool y luego inícialo como siempre (doble clic en `TheNewMiner.bat`).

**HiveOS:** edita el flight sheet: pon la nueva dirección de la billetera en "Wallet and worker template"
(`<NEW_WALLET>.%WORKER_NAME%`) y la nueva contraseña del pool en Pass; luego guárdalo y aplícalo. El nombre del worker
es el nombre del rig en HiveOS (o `-w <RIG_NAME>` en Extra config arguments).

## Proxy (opcional)

El minero se conecta al pool directamente. Si tu red necesita un proxy:

```bash
# Linux (se recuerda hasta que lo cambies)
./thenewminer --proxy <IP>[:<PORT>]            # relé TCP (puerto 443 si se omite)
./thenewminer --proxy socks5://<HOST>:<PORT>   # SOCKS5, también socks5://<USER>:<PASS>@<HOST>:<PORT>
./thenewminer --proxy http://<HOST>:<PORT>     # proxy HTTP CONNECT
./thenewminer --proxy none                     # volver a la conexión directa
```

Estos comandos también inician el minero: detén el minero primero y añade `--background` para mantenerlo en segundo
plano.

```bat
:: Windows
TheNewMiner.bat setup -Proxy <IP>[:<PORT>]
TheNewMiner.bat setup -Proxy socks5://<HOST>:<PORT>
TheNewMiner.bat setup -Proxy http://<HOST>:<PORT>
TheNewMiner.bat setup -Proxy none
```

TLS siempre va de extremo a extremo con el pool, así que el proxy no puede leer la conexión. En Windows, una aplicación
de proxy que se ejecute en el mismo PC normalmente no es accesible como `127.0.0.1` desde WSL (red predeterminada de
WSL): usa la IP de LAN del PC y permite las conexiones LAN en la aplicación.

## Requisitos

| Elemento | Requisito |
|---|---|
| GPU | NVIDIA GeForce RTX serie 30 / 40 / 50, con 10 GB de VRAM o más. Rigs con varias GPU: compatibles, cada GPU mina; en esta versión se probó con una GPU por máquina |
| Controlador | NVIDIA **580 o posterior** (`nvidia-smi` debe funcionar). En Windows, instálalo en Windows, nunca dentro de WSL |
| Linux | x86_64 con **glibc 2.34 o posterior**, por ejemplo Ubuntu 22.04 o posterior o Debian 12 o posterior. El instalador comprueba lo que necesita e indica lo que falte |
| Windows | Windows 10 21H2 o posterior, o Windows 11, de 64 bits; virtualización (Intel VT-x / AMD SVM) activada en la BIOS |
| Disco | unos 20 GB libres |
| RAM | 4 GB o más |
| Modelo | unos 8.1 GiB, se descarga una vez en el primer inicio (en paralelo, desde varios mirrors) y se reutiliza después |

## Actualiza el controlador de NVIDIA (580 o posterior)

El minero necesita el controlador de NVIDIA **580 o posterior**. Comprueba cuál tienes:

```bash
nvidia-smi    # la cabecera muestra "Driver Version: ..."; debe ser 580 o posterior
```

- **Ubuntu:** `sudo ubuntu-drivers install` instala el controlador recomendado (comprueba después que sea 580 o
  posterior). Para elegir tú la versión:
  `sudo apt install nvidia-driver-580` (o posterior; las tarjetas de la serie RTX 50 necesitan la variante open, por
  ejemplo `nvidia-driver-580-open`). Después reinicia. Usa los paquetes de controlador de Ubuntu o el instalador `.run` de
  NVIDIA, nunca ambos.
- **Debian:** los paquetes de controlador propios de Debian son anteriores a 580 (535 en Debian 12, 550 en Debian 13).
  Instala un controlador más reciente desde el repositorio de NVIDIA como se describe en la
  [NVIDIA Driver Installation Guide](https://docs.nvidia.com/datacenter/tesla/driver-installation-guide/debian.html)
  (guía de instalación del controlador de NVIDIA) y después reinicia.
- **Windows:** actualiza con la [NVIDIA app](https://www.nvidia.com/en-us/software/nvidia-app/) (antes GeForce
  Experience) o descarga el controlador para tu tarjeta desde [nvidia.com](https://www.nvidia.com/en-us/drivers/) (580 o
  posterior). WSL2 usa este controlador de Windows: nunca instales un controlador de NVIDIA para Linux dentro de WSL.
- **HiveOS:** como root (Hive Shell, o SSH y luego `sudo -i`), ejecuta `nvidia-driver-update --list` para ver las
  versiones y luego, por ejemplo, `nvidia-driver-update 595` (cualquiera 580 o posterior) y después `sreboot`.

## Preguntas frecuentes

**Windows dice "Windows protegió su PC" / "Editor desconocido".**
El kit no tiene firma de código. Haz clic en **Más información** y después en **Ejecutar de todas formas**.

**Mi antivirus marca o elimina el minero.**
Microsoft Defender y otros antivirus suelen marcar los mineros de criptomonedas como "aplicaciones potencialmente no
deseadas", incluso uno legítimo que tú elegiste ejecutar, porque el malware a menudo oculta mineros. Si un archivo está
en cuarentena, abre Seguridad de Windows › Protección antivirus y contra amenazas › Historial de protección, selecciona
el elemento de TheNewMiner y elige **Permitir** (o agrega la carpeta descomprimida a las Exclusiones). Haz esto solo con
un paquete descargado de esta página cuyo SHA-256 coincida.

**El primer inicio tarda mucho.**
El primer inicio descarga el modelo de ~8.1 GiB y ejecuta una autocomprobación de la GPU en tu tarjeta; en Windows
también puede configurar WSL2. Esto ocurre una sola vez. Los inicios posteriores empiezan a minar mucho antes.

**El minero dice que el pool rechazó la billetera / contraseña.**
Revisa la dirección de la billetera y la contraseña del pool (la cuenta debe existir en el sitio web del pool) y vuelve
a iniciarlo: Linux `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>'`, Windows `TheNewMiner.bat setup`. El
minero no reintenta por sí solo con credenciales incorrectas.

**¿Está minando? ¿Dónde están los logs?**
Linux: `./thenewminer --status`, `./thenewminer --logs`, o el archivo `thenewminer/.miner/thenewminer.log`.
Windows: `TheNewMiner.bat status` y `TheNewMiner.bat logs`.

**Mi RTX 3080 (10 GB) mina mucho más despacio en Windows que en Linux.**
En Windows, otros programas comparten la VRAM. Consulta la
[solución de problemas en Windows](WINDOWS-TROUBLESHOOTING.es.md): cierra los navegadores con aceleración por hardware y
Docker Desktop.
