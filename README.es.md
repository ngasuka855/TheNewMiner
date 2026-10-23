[English](README.md) · [简体中文](README.zh-CN.md) · [繁體中文](README.zh-TW.md) · [Русский](README.ru.md) · **Español** · [Português (Brasil)](README.pt-BR.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Türkçe](README.tr.md) · [Tiếng Việt](README.vi.md) · [한국어](README.ko.md) · [日本語](README.ja.md)

> Esta traducción se ofrece por comodidad; el README en inglés es la versión de referencia. *(This translation is provided for convenience; the English README is authoritative.)*

# TheNewMiner

TheNewMiner es un minero por GPU para **TensorCash**. Funciona en tarjetas NVIDIA GeForce RTX de las series 30, 40 y 50
con **10 GB o más de VRAM** y mina en el pool **`tsc-wss.the-new.win`**, que viene integrado: no tienes que introducir
la dirección del pool y se conecta directamente.

Versión actual: **v0.1.44**

## Descarga

| Archivo | Para | SHA-256 |
|---|---|---|
| [`TheNewMiner-v0.1.44-linux-x86_64-sealed-r4.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/main/TheNewMiner-v0.1.44-linux-x86_64-sealed-r4.tar.xz) | Linux x86_64 (Ubuntu) | `99a8246e242c7d09f1c4b6335d974de0d163362d4dc634fff4188ab58a151ca6` |
| [`TheNewMiner-v0.1.44-windows-wsl2-r5.zip`](https://github.com/ngasuka855/TheNewMiner/raw/main/TheNewMiner-v0.1.44-windows-wsl2-r5.zip) | Windows 10 / 11 (se ejecuta mediante WSL2) | `264025136cf01d87d25ca2bb0554fe88efe01833850a697247cd657027215506` |
| [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/main/SHA256SUMS) | sumas de verificación de ambos paquetes | |

El zip de Windows es todo en uno: ya contiene el minero para Linux, así que solo necesitas el zip.

## Verificar la descarga

Coloca el paquete (o los paquetes) y `SHA256SUMS` en la misma carpeta y luego:

```bash
# Linux (añade --ignore-missing si descargaste solo un paquete)
sha256sum -c SHA256SUMS
# macOS
shasum -a 256 -c SHA256SUMS
```

```bat
:: Windows (Símbolo del sistema): compara el resultado con el SHA-256 de la tabla anterior
certutil -hashfile TheNewMiner-v0.1.44-windows-wsl2-r5.zip SHA256
```

Cada archivo debe indicar `OK` (en Windows: los mismos 64 caracteres que en la tabla). Si no coincide, no lo uses.

## Linux

```bash
tar -xJf TheNewMiner-v0.1.44-linux-x86_64-sealed-r4.tar.xz
cd thenewminer
./thenewminer -u <YOUR_WALLET> -p <YOUR_POOL_PASSWORD> -w rig-01 --background
```

| Comando | Qué hace |
|---|---|
| `./thenewminer -u <YOUR_WALLET> -p <YOUR_POOL_PASSWORD> -w rig-01 --background` | Primer inicio: instala lo necesario y luego mina en segundo plano |
| `./thenewminer --background` | Inicios posteriores: tu configuración se recuerda |
| `./thenewminer` | Minar en primer plano |
| `./thenewminer --logs` | Ver el minero (Ctrl+C sale de la vista; la minería continúa) |
| `./thenewminer --stop` | Detener la minería |

- `-u` es la dirección de tu billetera de TensorCash, `-p` tu contraseña del pool y `-w` el nombre del worker que se
  muestra en el pool (predeterminado: el nombre de host de la máquina).
- La configuración se recuerda en `thenewminer/.miner/` (el archivo con la contraseña solo puedes leerlo tú).
- El primer inicio descarga el modelo (unos 8.1 GiB, una sola vez), instala el motor de GPU para tu tarjeta y ejecuta
  una autocomprobación en la tarjeta; después empieza a minar. Las líneas como `[ RATE] ... N/s` indican que está minando.
- El primer inicio puede instalar algunos paquetes de Ubuntu con `apt` (requiere sudo o root).
- Exactamente una GPU NVIDIA por máquina en esta versión (los rigs con varias GPU todavía no son compatibles).
- Si `tar` muestra `xz: Cannot exec`, instala primero xz: `sudo apt-get install -y xz-utils`.
- El `README.md` que viene dentro del paquete es una nota interna antigua; sigue esta página (no se necesita `--proxy`).
- **Actualizar:** `./thenewminer --stop`, descomprime el nuevo paquete en el mismo lugar (se descomprime en `thenewminer/` y
  conserva `thenewminer/.miner/` con tu configuración y el modelo) y vuelve a iniciarlo.

## Windows (con un clic)

1. Descomprime todo `TheNewMiner-v0.1.44-windows-wsl2-r5.zip` (clic derecho › Extraer todo) donde quieras: cualquier
   carpeta, cualquier unidad; las rutas con espacios o caracteres chinos funcionan bien. No lo ejecutes desde la vista del zip.
2. Haz doble clic en **`TheNewMiner.bat`**.
   - **La primera vez:** comprueba Windows, el controlador y el disco, activa WSL2 si hace falta (una solicitud de
     administrador; puede que haga falta reiniciar una vez, y después la instalación continúa sola), muestra dónde se
     guardarán los datos (Enter acepta), crea su propio entorno Linux (WSL), instala el minero incluido, te pide **una
     sola vez** tu dirección de billetera (empieza por `tc1`), la contraseña del pool (se escribe dos veces; de 6 a 128
     letras, dígitos, `_` o `-`) y el nombre del worker (Enter = el nombre del equipo), descarga el modelo y empieza a minar.
   - **Las veces siguientes:** haz doble clic y empieza a minar de inmediato con tu configuración guardada. También se
     crea un acceso directo "TheNewMiner" en el escritorio.

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
- **Actualizar:** descomprime la nueva versión donde quieras y haz doble clic en su `TheNewMiner.bat`. Encuentra el
  minero instalado, conserva tu configuración y el modelo e instala la nueva versión sin hacer preguntas. La carpeta
  antigua se puede eliminar.
- **Desinstalar:** `TheNewMiner.bat uninstall`.
- El idioma en pantalla sigue el de Windows (English / 简体中文 / 繁體中文). Para elegirlo, añade `-Lang en` (o `zh-Hans`,
  `zh-Hant`) a cualquier comando, p. ej. `TheNewMiner.bat -Lang en`; se aplica a esa ejecución.

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
| GPU | Exactamente una tarjeta NVIDIA GeForce RTX serie 30 / 40 / 50, con 10 GB de VRAM o más |
| Controlador | NVIDIA **580 o posterior** (`nvidia-smi` debe funcionar). En Windows, instálalo en Windows, nunca dentro de WSL |
| Linux | Ubuntu 22.04, 24.04 o 26.04 x86_64 (otras distribuciones todavía no son compatibles) |
| Windows | Windows 10 21H2 o posterior, o Windows 11, de 64 bits; virtualización (Intel VT-x / AMD SVM) activada en la BIOS |
| Disco | unos 20 GB libres |
| RAM | 4 GB o más |
| Modelo | unos 8.1 GiB, se descarga una vez en el primer inicio (en paralelo, desde varios mirrors) y se reutiliza después |

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

Pruebas en el pool real de v0.1.44 en nuestros rigs de prueba el 2026-10-23 (una GPU por máquina). A = shares
aceptados, R = shares rechazados.

| Fecha | Tarjeta | Sistema operativo | Paquete | Hashrate | A / R |
|---|---|---|---|---|---|
| 2026-10-23 | RTX 4090 | Linux | sealed-r4 | 65.58 N/s (mediana, 16 min) | 10 / 0 |
| 2026-10-23 | RTX 5090 | Linux | sealed-r4 | 93.45 N/s (minería estable, 44 min; 90.39 incluyendo cambios de trabajo) | 31 / 0 (+1 stale al detener) |
| 2026-10-23 | RTX 3080 10 GB | Windows (WSL2) | windows-wsl2-r5 | ~22 N/s (ejecución propia del operador) | n/d |
| 2026-10-23 | RTX 3080 10 GB | Linux (Ubuntu 26.04) | sealed-r4 | 21.04 N/s (últimos 10 min de 23 min de minería) | 3 / 0 |

RTX 3080 en Linux: instalación 6/6 pasos OK, ninguna ventana inválida, detención limpia. Extracto del log:
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

**¿Dónde están los logs?**
Linux: `./thenewminer --logs`, o el archivo `thenewminer/.miner/thenewminer.log`.
Windows: `TheNewMiner.bat logs` (y `TheNewMiner.bat status` para los últimos hashrates).

**¿HiveOS?**
Un paquete para HiveOS llegará **pronto**; todavía está en fase de pruebas.

## Nota de seguridad

El minero y el motor de GPU se distribuyen cifrados (ChaCha20-Poly1305) dentro del paquete; el minero los descifra en
tu máquina al iniciarse. Esto encarece copiarlos, pero **no es irrompible**: cualquiera con acceso de administrador a una
máquina que ejecute el minero puede, con suficiente esfuerzo, recuperar la clave o el código descifrado.
Tu dirección de billetera y tu contraseña del pool se guardan en tu propia máquina y solo se envían al pool.
