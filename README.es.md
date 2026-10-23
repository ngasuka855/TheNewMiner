[English](README.md) · [简体中文](README.zh-CN.md) · [繁體中文](README.zh-TW.md) · [Русский](README.ru.md) · **Español** · [Português (Brasil)](README.pt-BR.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Türkçe](README.tr.md) · [Tiếng Việt](README.vi.md) · [한국어](README.ko.md) · [日本語](README.ja.md)

> Esta traducción se ofrece por comodidad; el README en inglés es la versión de referencia. *(This translation is provided for convenience; the English README is authoritative.)*

# TheNewMiner

TheNewMiner es un minero por GPU para **TensorCash**. Funciona en tarjetas NVIDIA GeForce RTX de las series 30, 40 y 50
con **10 GB o más de VRAM** y mina en el pool **`tsc-wss.the-new.win`**, que viene integrado: no tienes que introducir
la dirección del pool y se conecta directamente.

Versión actual: **v0.1.45**

## Novedades de la v0.1.45

- **Un mensaje claro cuando el pool rechaza tu billetera o tu contraseña.** El minero lo indica en una sola línea y se
  detiene en lugar de reintentar; `--status` muestra el motivo. Revisa la billetera y la contraseña y vuelve a iniciarlo
  con `-u` y `-p`.
- **Tu billetera y tu contraseña ya no son visibles en `ps`** ni en las listas de procesos de otros usuarios.
- **`./thenewminer --status`** muestra si está minando, desde cuándo, el último hashrate y los shares aceptados / rechazados.
- **Compatibilidad con HiveOS (experimental):** un paquete de minero personalizado (custom miner) para HiveOS y un flight sheet importable (consulta la sección de HiveOS más abajo).
- **Actualizaciones seguras:** la nueva versión se niega a iniciarse mientras la anterior siga minando (te indica que la
  detengas primero); después de `--stop` reinstala sus archivos de programa por sí sola y conserva tu configuración y el modelo.
- **`--background` ya no dice "iniciado" cuando el minero termina de inmediato**; muestra el motivo.
- **Docker y memoria compartida pequeña:** cuando la carpeta en memoria es demasiado pequeña (el valor predeterminado de
  Docker es 64 MiB) o no permite ejecutar programas, el minero usa `thenewminer/.miner/run/` en el disco y lo indica.
- **Las contraseñas del pool ya pueden usar cualquier carácter visible** (también signos de puntuación y letras no
  inglesas): de 6 a 128 caracteres, sin espacios.
- Más sistemas Linux: el instalador ahora comprueba lo que necesita (glibc 2.34 o posterior) en lugar de aceptar solo
  unas pocas versiones de Ubuntu.

## Descarga

| Archivo | Para | SHA-256 |
|---|---|---|
| [`TheNewMiner-v0.1.45-linux-x86_64-sealed.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/TheNewMiner-v0.1.45-linux-x86_64-sealed.tar.xz) | Linux x86_64 | `6281bf489fae843551114f09ead5498d3e47af33a07e4cfdfbdc0a5d6bdbafbb` |
| [`TheNewMiner-v0.1.45-windows-wsl2-r1.zip`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/TheNewMiner-v0.1.45-windows-wsl2-r1.zip) | Windows 10 / 11 (se ejecuta mediante WSL2) | `90732441b0dc67de84809d9bf3bda4441c0bf7cd9a52fefb95d29242ad7b15e7` |
| [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/SHA256SUMS) | sumas de verificación de todos los paquetes | |

El zip de Windows es todo en uno: ya contiene el minero para Linux, así que solo necesitas el zip.

## Verificar la descarga

Coloca el paquete (o los paquetes) y `SHA256SUMS` en la misma carpeta y luego:

```bash
# Linux (--ignore-missing: solo se comprueban los paquetes que descargaste)
sha256sum -c --ignore-missing SHA256SUMS
# macOS
shasum -a 256 -c --ignore-missing SHA256SUMS
```

```bat
:: Windows (Símbolo del sistema): compara el resultado con el SHA-256 de la tabla anterior
certutil -hashfile TheNewMiner-v0.1.45-windows-wsl2-r1.zip SHA256
```

Cada archivo debe indicar `OK` (en Windows: los mismos 64 caracteres que en la tabla). Si no coincide, no lo uses.

## Instalación con un clic (Linux)

Sustituye `<YOUR_WALLET>`, `<YOUR_POOL_PASSWORD>` y `<RIG_NAME>` (incluidos los `<` `>`) al final de este comando
y luego pégalo en una terminal. Descarga el paquete para Linux y el `SHA256SUMS` de esta versión (con `curl`, o con `wget`
si no hay `curl`), comprueba el SHA-256, descomprime el minero en `~/thenewminer` y lo inicia en segundo plano:

```bash
mkdir -p ~/thenewminer-download && cd ~/thenewminer-download && V=v0.1.45 && P=TheNewMiner-$V-linux-x86_64-sealed.tar.xz && U=https://github.com/ngasuka855/TheNewMiner/raw/$V && rm -f "$P" SHA256SUMS && if command -v curl >/dev/null 2>&1; then curl -fL -o "$P" "$U/$P" && curl -fL -o SHA256SUMS "$U/SHA256SUMS"; else wget -O "$P" "$U/$P" && wget -O SHA256SUMS "$U/SHA256SUMS"; fi && sha256sum -c --ignore-missing SHA256SUMS && tar -xJf "$P" -C ~ && cd ~/thenewminer && ./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w <RIG_NAME> --background
```

Se detiene en el primer paso que falle: un error de descarga o un SHA-256 que no coincide nunca inicia nada (la
comprobación protege contra una descarga dañada; ambos archivos vienen de esta página). Mantén las comillas simples
alrededor de la contraseña: dentro de ellas cualquier carácter es seguro (una `'` en la contraseña se escribe `'\''`).
Si `tar` muestra `xz: Cannot exec`, instala xz (`sudo apt-get install -y xz-utils`) y vuelve a ejecutar el comando.

Para actualizar una versión que ya está en `~/thenewminer`, ejecuta primero `~/thenewminer/thenewminer --stop` y luego
este comando (se conservan tu configuración y el modelo). Si tu versión anterior está en otra carpeta, detenla primero
allí, o actualízala allí como se describe en la sección Linux (de lo contrario, el modelo se vuelve a descargar).

## Linux

```bash
tar -xJf TheNewMiner-v0.1.45-linux-x86_64-sealed.tar.xz
cd thenewminer
./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background
```

| Comando | Qué hace |
|---|---|
| `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background` | Primer inicio: instala lo necesario y luego mina en segundo plano |
| `./thenewminer --background` | Inicios posteriores: tu configuración se recuerda |
| `./thenewminer` | Minar en primer plano (Ctrl+C lo detiene) |
| `./thenewminer --logs` | Ver el minero (Ctrl+C sale de la vista; la minería continúa) |
| `./thenewminer --status` | Si está minando, su último hashrate y los shares aceptados / rechazados |
| `./thenewminer --stop` | Detener la minería |
| `./thenewminer --help` | Todas las opciones |

- `-u` es la dirección de tu billetera de TensorCash, `-p` tu contraseña del pool y `-w` el nombre del worker que se
  muestra en el pool (predeterminado: el nombre de host de la máquina; de 1 a 64 letras, dígitos, `_`, `-` o `.`).
- La contraseña del pool: de 6 a 128 caracteres, cualquier carácter visible, sin espacios. Ponla siempre entre comillas
  simples en la línea de comandos, p. ej. `-p 'my$pass!'` (una `'` dentro de comillas simples se escribe `'\''`).
- La configuración se recuerda en `thenewminer/.miner/` (el archivo con la contraseña solo puedes leerlo tú). La billetera
  y la contraseña se retiran de la línea de comandos de inmediato, así que no aparecen en `ps` (tu shell puede seguir
  guardando el comando en su historial).
- Si el pool rechaza la billetera o la contraseña (o todavía no ha aprobado la cuenta), el minero lo indica y se detiene.
  Revísalas y vuelve a iniciarlo con `-u` y `-p`.
- El primer inicio descarga el modelo (unos 8.1 GiB, una sola vez), instala el motor de GPU para tu tarjeta y ejecuta
  una autocomprobación en la tarjeta; después empieza a minar. Las líneas como `[ RATE] ... N/s` indican que está minando.
- En Ubuntu y Debian, el primer inicio puede instalar con `apt` algunas bibliotecas del sistema que falten (requiere sudo
  o root); en otros sistemas te indica cuáles debes instalar.
- Rigs con varias GPU: compatibles, cada GPU mina; esta versión se probó con una GPU por máquina.
- Si `tar` muestra `xz: Cannot exec`, instala primero xz: `sudo apt-get install -y xz-utils`.
- En Docker u otros contenedores con poca memoria compartida, el minero ejecuta sus archivos de programa desde
  `thenewminer/.miner/run/` en el disco y lo indica. Para elegir otra carpeta, inícialo con
  `TNPSEAL_RUNTIME_BASE=<folder> ./thenewminer` (una carpeta en la que solo tú puedas escribir: `chmod 700 <folder>`).
- **Actualizar:** `./thenewminer --stop`, descomprime el nuevo paquete en el mismo lugar (se descomprime en `thenewminer/` y
  conserva `thenewminer/.miner/` con tu configuración y el modelo) y vuelve a iniciarlo. Si la versión anterior todavía se
  está ejecutando, la nueva se niega a iniciarse y te pide que la detengas primero.

## Windows (con un clic)

1. Descomprime todo `TheNewMiner-v0.1.45-windows-wsl2-r1.zip` (clic derecho › Extraer todo) donde quieras: cualquier
   carpeta, cualquier unidad; las rutas con espacios o caracteres chinos funcionan bien. No lo ejecutes desde la vista del zip.
2. Haz doble clic en **`TheNewMiner.bat`**.
   - **La primera vez:** comprueba Windows, el controlador y el disco, activa WSL2 si hace falta (una solicitud de
     administrador; puede que haga falta reiniciar una vez, y después la instalación continúa sola), muestra dónde se
     guardarán los datos (Enter acepta), crea su propio entorno Linux (WSL), instala el minero incluido, te pide **una
     sola vez** tu dirección de billetera (empieza por `tc1`), la contraseña del pool (se escribe dos veces; de 6 a 128
     caracteres, cualquier carácter visible, sin espacios) y el nombre del worker (Enter = el nombre del equipo),
     descarga el modelo y empieza a minar.
   - **Las veces siguientes:** haz doble clic y empieza a minar de inmediato con tu configuración guardada. También se
     crea un acceso directo "TheNewMiner" en el escritorio. Si vuelves a hacer doble clic mientras ya está minando, te lo
     indica y muestra el log.

Comandos de uso diario (abre un Símbolo del sistema en la carpeta descomprimida):

| Comando | Qué hace |
|---|---|
| `TheNewMiner.bat` | Instala si hace falta y luego mina en esta ventana (cerrar la ventana detiene la minería) |
| `TheNewMiner.bat background` | Minar sin ventana |
| `TheNewMiner.bat logs` | Ver el minero (Ctrl+C sale de la vista; la minería continúa) |
| `TheNewMiner.bat status` | Si está en ejecución y los últimos hashrates |
| `TheNewMiner.bat stop` | Detener la minería |
| `TheNewMiner.bat setup` | Volver a introducir la billetera / contraseña / nombre del worker |
| `TheNewMiner.bat uninstall` | Quitar el minero, su carpeta de datos y el acceso directo (pregunta antes) |

- **Dónde se guardan los datos:** el minero y el modelo se guardan en una carpeta de datos elegida en la primera
  instalación (por defecto `TheNewMinerData` en la raíz de la unidad donde descomprimiste, p. ej. `D:\TheNewMinerData`),
  no en la carpeta descomprimida. Allí se necesitan unos 20 GB libres.
- Si el pool rechaza la billetera o la contraseña, la ventana lo indica y la minería se detiene: ejecuta
  `TheNewMiner.bat setup` para volver a introducirlas.
- **Actualizar:** descomprime la nueva versión donde quieras y haz doble clic en su `TheNewMiner.bat`. Encuentra el
  minero instalado, conserva tu configuración y el modelo e instala la nueva versión sin hacer preguntas. La carpeta
  antigua se puede eliminar.
- **Desinstalar:** `TheNewMiner.bat uninstall`.
- El idioma en pantalla sigue el de Windows (English / 简体中文 / 繁體中文). Para elegirlo, añade `-Lang en` (o `zh-Hans`,
  `zh-Hant`) a cualquier comando, p. ej. `TheNewMiner.bat -Lang en`; se aplica a esa ejecución.

## HiveOS (experimental)

La compatibilidad con HiveOS es nueva en la v0.1.45 y **experimental**. Usa el paquete de minero personalizado (custom
miner) para HiveOS que aparece a continuación.

| Archivo | SHA-256 |
|---|---|
| [`thenewminer-0.1.45.tar.gz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/thenewminer-0.1.45.tar.gz) | `90576efd388ceeca4e456b62b568f267465320913a07c9f9736f42363b1701dc` |

Configuración rápida (importar el flight sheet):

1. **Wallets:** añade una billetera con la moneda `TSC` y tu dirección de TensorCash (`tc1...`). Solo hace falta una
   vez: HiveOS necesita una billetera `TSC` seleccionada, y el minero usa la dirección que pongas en el flight sheet.
2. Abre [`hiveos/flightsheet-thenewminer.json`](hiveos/flightsheet-thenewminer.json) y copia su contenido. Sustituye
   `<YOUR_WALLET>` por la dirección de tu billetera (mantén `.%WORKER_NAME%` detrás) y `<YOUR_POOL_PASSWORD>` por tu
   contraseña del pool (en el JSON, una `"` de la contraseña se escribe `\"` y una `\` se escribe `\\`). En HiveOS elige
   Flight Sheets › **Import from clipboard**, pégalo, selecciona tu billetera `TSC` y guárdalo.
3. Aplica el flight sheet a tu rig. El rig descarga el paquete y empieza a minar.

Configuración manual en su lugar: crea un flight sheet con Coin `TSC`, Wallet: tu billetera `TSC`, Pool: "Configure in
miner", Miner: "Custom", haz clic en **Setup Miner Config** y rellena:

| Campo | Valor |
|---|---|
| Miner name | `thenewminer` (se rellena a partir de la Installation URL) |
| Installation URL | `https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/thenewminer-0.1.45.tar.gz` |
| Hash algorithm | déjalo vacío |
| Wallet and worker template | `%WAL%.%WORKER_NAME%` |
| Pool URL | `tsc-wss.the-new.win` (se ignora: el pool viene integrado) |
| Pass | tu contraseña del pool |
| Extra config arguments | vacío, o `--proxy <YOUR_PROXY>` (consulta la sección Proxy) |

- El nombre del worker en el pool es el nombre del rig en HiveOS: usa solo letras, dígitos, `_`, `-` o `.` (sin
  espacios), hasta 64 caracteres. Para usar otro nombre de worker, añade `-w <RIG_NAME>` al campo Extra config
  arguments.
- Se necesitan unos 12 GB libres en la unidad de HiveOS para el modelo y el minero (compruébalo con `df -h /hive/miners`).
- El modelo (unos 8.1 GiB) se descarga una vez en `/hive/miners/custom/.thenewminer-models` y se conserva al actualizar.
- La billetera y la contraseña llegan al minero a través de su entorno, no de su línea de comandos.
- Si el pool rechaza la billetera o la contraseña, el log del minero lo indica y el minero espera 10 minutos antes de que
  HiveOS lo vuelva a iniciar: corrige el flight sheet y aplícalo (eso reinicia el minero de inmediato).
- Logs: la pantalla del minero en HiveOS (`miner` en la shell del rig) o `/var/log/miner/custom/custom.log`.
- **Actualizar:** cambia los dos números de versión en la Installation URL (por ejemplo
  `.../raw/v0.1.46/thenewminer-0.1.46.tar.gz`) y aplica el flight sheet. HiveOS solo instala cuando cambia el nombre del
  archivo en la URL.
- **El primer arranque tarda un poco:** tras la descarga del modelo, en las tarjetas de la serie RTX 30 una comprobación
  única de la GPU puede agotar su tiempo de espera en un disco lento del rig, y entonces HiveOS reinicia el minero por
  sí solo, dos o tres veces (unos 8 minutos). No lo toques; después empieza a minar.
- **¿Ya tienes el modelo?** Copia `Qwen3-8B-Q8_0-full-v2.gguf` (unos 8.1 GiB, exactamente con este nombre de archivo) a
  `/home/user/` en el rig antes del primer arranque: el minero lo encuentra, comprueba su SHA-256 y lo usa en lugar de
  volver a descargarlo.

**Si el rig no puede acceder a GitHub:** descarga `thenewminer-0.1.45.tar.gz` en un PC (comprueba su SHA-256 en la tabla
anterior), cópialo al rig y descomprímelo donde HiveOS guarda los mineros personalizados. Después aplica el flight sheet
como siempre: HiveOS indica "Already installed" y no lo descarga. Mantén exactamente el nombre de archivo
`thenewminer-0.1.45.tar.gz` (un navegador puede añadir " (1)") y no cambies la Installation URL del flight sheet: HiveOS
compara ese nombre.

```bash
# en el PC
scp thenewminer-0.1.45.tar.gz user@<RIG_IP>:/tmp/
# en el rig, como root (Hive Shell, o SSH y luego sudo -i)
mkdir -p /hive/miners/custom/downloads
mv /tmp/thenewminer-0.1.45.tar.gz /hive/miners/custom/downloads/
cd /hive/miners/custom && tar -xzf downloads/thenewminer-0.1.45.tar.gz
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

**Windows:** ejecuta `TheNewMiner.bat setup`. Detiene el minero en ejecución y vuelve a pedir la billetera, la
contraseña del pool y el nombre del worker. Luego inícialo como siempre (doble clic en `TheNewMiner.bat`).

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

Estos comandos también inician el minero: detén el minero primero y añade `--background` para mantenerlo en segundo plano.

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
  Instala un controlador más reciente
  desde el repositorio de NVIDIA como se describe en la [NVIDIA Driver Installation Guide](https://docs.nvidia.com/datacenter/tesla/driver-installation-guide/debian.html)
  (guía de instalación del controlador de NVIDIA) y después reinicia.
- **Windows:** actualiza con la [NVIDIA app](https://www.nvidia.com/en-us/software/nvidia-app/) (antes GeForce
  Experience) o descarga el controlador para tu tarjeta desde [nvidia.com](https://www.nvidia.com/en-us/drivers/) (580 o
  posterior). WSL2 usa este controlador de Windows: nunca instales un controlador de NVIDIA para Linux dentro de WSL.
- **HiveOS:** como root (Hive Shell, o SSH y luego `sudo -i`), ejecuta `nvidia-driver-update --list` para ver las
  versiones y luego, por ejemplo, `nvidia-driver-update 595` (cualquiera 580 o posterior) y después `sreboot`.

## Hashrates de referencia

Los mejores hashrates medidos en el pool real con nuestros propios rigs (entre el 2026-10-14 y el 2026-10-23). Son
cifras **máximas** ("hasta"), no una promesa: tarjetas del mismo modelo varían alrededor de un 5–8% según el chip
concreto, la refrigeración, las frecuencias y el límite de potencia, y el hashrate baja si la tarjeta también maneja
una pantalla o si otros programas usan la GPU.

| GPU | Hashrate |
|---|---|
| RTX 5090 | hasta ~99 N/s |
| RTX 4090 | hasta ~70 N/s |
| RTX 3080 10 GB | hasta ~22 N/s |

## Resultados de pruebas

Pruebas en el pool real en nuestros rigs de prueba (una GPU por máquina). A = shares aceptados, R = shares rechazados.
"Minería estable" es la mediana del hashrate entre cambios de trabajo.

| Fecha | Tarjeta | Sistema operativo | Paquete | Hashrate | A / R |
|---|---|---|---|---|---|
| 2026-10-23 | RTX 4090 | Linux | build final de v0.1.45 | 65.42 N/s (minería estable, 16 min; 62.35 incluyendo cambios de trabajo) | 13 / 0 (0 inválidos) |
| 2026-10-23 | RTX 5090 | Linux | build final de v0.1.45 | 94.18 N/s (minería estable; 92.24 incluyendo cambios de trabajo) | 17 / 0 (0 inválidos) |
| 2026-10-23 | RTX 4090 | Linux | v0.1.44 sealed-r4 | 65.58 N/s (mediana, 16 min) | 10 / 0 |
| 2026-10-23 | RTX 5090 | Linux | v0.1.44 sealed-r4 | 93.45 N/s (minería estable, 44 min; 90.39 incluyendo cambios de trabajo) | 31 / 0 (+1 stale al detener) |
| 2026-10-23 | RTX 3080 10 GB | Windows (WSL2) | v0.1.44 windows-wsl2-r5 | ~22 N/s (ejecución propia del operador) | n/d |
| 2026-10-23 | RTX 3080 10 GB | Linux (Ubuntu 26.04) | v0.1.44 sealed-r4 | 21.04 N/s (últimos 10 min de 23 min de minería) | 3 / 0 |

RTX 3080 en Linux (v0.1.44): instalación 6/6 pasos OK, ninguna ventana inválida, detención limpia. Extracto del log:
[`test-results/rtx3080-linux-20261023.txt`](test-results/rtx3080-linux-20261023.txt).

## Capturas de pantalla

De nuestros rigs de prueba, con versiones anteriores: v0.1.41 en la RTX 4090 y una build temprana en la RTX 5090. Son
lecturas en vivo; tus números dependen de la tarjeta.

**RTX 4090, v0.1.41:** 69–75 N/s por lectura, en su mayoría ~70 (media de 10 minutos ~68), 9 shares aceptados, 0 rechazados, ~445 W.

![TheNewMiner v0.1.41 minando en una RTX 4090](screenshots/rtx4090.png)

**RTX 5090, build temprana:** 100.83 N/s (media en vivo sobre las ventanas completadas), ~575 W.

![Una build temprana de TheNewMiner minando en una RTX 5090](screenshots/rtx5090.png)

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
Revisa la dirección de la billetera y la contraseña del pool (y que el pool haya aprobado tu cuenta) y vuelve a iniciarlo:
Linux `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>'`, Windows `TheNewMiner.bat setup`. El minero no
reintenta por sí solo con credenciales incorrectas.

**¿Está minando? ¿Dónde están los logs?**
Linux: `./thenewminer --status`, `./thenewminer --logs`, o el archivo `thenewminer/.miner/thenewminer.log`.
Windows: `TheNewMiner.bat status` y `TheNewMiner.bat logs`.

## Nota de seguridad

El minero y el motor de GPU se distribuyen cifrados (ChaCha20-Poly1305) dentro del paquete; el minero los descifra en
tu máquina al iniciarse. Esto encarece copiarlos, pero **no es irrompible**: cualquiera con acceso de administrador a una
máquina que ejecute el minero puede, con suficiente esfuerzo, recuperar la clave o el código descifrado.
Tu dirección de billetera y tu contraseña del pool se guardan en tu propia máquina y solo se envían al pool.
