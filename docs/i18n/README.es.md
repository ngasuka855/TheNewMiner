# TheNewMiner

TheNewMiner es un minero por GPU para **TensorCash** en tarjetas NVIDIA GeForce RTX de las series 30, 40 y 50 con 10 GB o más de VRAM; el pool **`tsc-wss.the-new.win`** viene integrado.

[English](../../README.md) · [简体中文](README.zh-CN.md) · [繁體中文](README.zh-TW.md) · [Русский](README.ru.md) · **Español** · [Português (Brasil)](README.pt-BR.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Türkçe](README.tr.md) · [Tiếng Việt](README.vi.md) · [한국어](README.ko.md) · [日本語](README.ja.md)

> Esta traducción se ofrece por comodidad; el README en inglés es la versión de referencia. *(This translation is provided for convenience; the English README is authoritative.)*

## Descarga

Versión actual: **v0.1.47** · Sitio web del pool: [tsc.the-new.win](https://tsc.the-new.win/#/home)

| Sistema | Descarga | Después |
|---|---|---|
| Linux x86_64 | [`TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz) | o usa la instalación con un clic de más abajo |
| Windows 10 / 11 | [`TheNewMiner-v0.1.47-windows-wsl2-r3.zip`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-windows-wsl2-r3.zip) | descomprímelo y haz doble clic en `TheNewMiner.bat` |
| HiveOS (experimental) | [`hiveos/flightsheet-thenewminer.json`](../../hiveos/flightsheet-thenewminer.json) | importa el flight sheet (consulta HiveOS más abajo) |

Sumas de verificación: [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/SHA256SUMS). Cómo comprobarlas se
explica en la [guía](GUIDE.es.md). Si un archivo no coincide, no lo uses.

## Antes de empezar: crea tu cuenta en el pool

Para minar necesitas una cuenta en el pool: tu dirección de cobro de TSC (billetera) y una contraseña del pool que eliges
en el sitio web del pool. El sitio tiene una versión en inglés (usa su menú de idioma).

1. Abre [tsc.the-new.win](https://tsc.the-new.win/#/home) y ve a **Start mining** (开始挖矿).
2. En **1. Bind Wallet**, introduce tu **TSC payout address** (`tc1...`; compruébala con cuidado: los ingresos de minería
   enviados a una dirección equivocada no se pueden recuperar), elige una **Pool Password** (6–128 caracteres) y vuelve
   a introducirla en **Confirm Password**.
3. Haz clic en **Apply to join pool**. La página confirma que la billetera está vinculada; la cuenta funciona de
   inmediato.
4. Usa la misma billetera y contraseña en el minero: `-u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>'` en Linux, la ventana
   de cuenta de `TheNewMiner.bat` en Windows o el flight sheet en HiveOS. Unos 3–5 minutos después de que empiece a
   minar, tu rig aparece en **My Miners** en el sitio web del pool.

El pool no puede recuperar una contraseña del pool olvidada, y la dirección de cobro no se puede cambiar más adelante. La
contraseña del pool no es la clave privada de tu billetera: nunca le des tu clave privada a nadie.

## Instalación con un clic (Linux)

Sustituye `<YOUR_WALLET>`, `<YOUR_POOL_PASSWORD>` y `<RIG_NAME>` (incluidos los `<` `>`) al final de este comando por tu
billetera, tu contraseña del pool y un nombre para este rig, y pégalo en una terminal. Descarga el paquete para Linux y
`SHA256SUMS`, comprueba el SHA-256, descomprime el minero en `~/thenewminer` y lo inicia en segundo plano:

```bash
mkdir -p ~/thenewminer-download && cd ~/thenewminer-download && V=v0.1.47 && P=TheNewMiner-$V-linux-x86_64-sealed.tar.xz && U=https://github.com/ngasuka855/TheNewMiner/raw/$V && rm -f "$P" SHA256SUMS && if command -v curl >/dev/null 2>&1; then curl -fL -o "$P" "$U/$P" && curl -fL -o SHA256SUMS "$U/SHA256SUMS"; else wget -O "$P" "$U/$P" && wget -O SHA256SUMS "$U/SHA256SUMS"; fi && sha256sum -c --ignore-missing SHA256SUMS && tar -xJf "$P" -C ~ && cd ~/thenewminer && ./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w <RIG_NAME> --background
```

Un error de descarga o un SHA-256 que no coincide nunca inicia nada. Mantén las comillas simples alrededor de la
contraseña (una `'` en la contraseña se escribe `'\''`). En una máquina con una sola GPU, el minero comprueba la
billetera y la contraseña con el pool antes de descargar el modelo (unos 8.1 GiB, una sola vez). Después,
`./thenewminer --logs` muestra que está minando y `./thenewminer --status` indica si está en ejecución o por qué se
detuvo. Para actualizar, ejecuta primero `~/thenewminer/thenewminer --stop` y luego el mismo comando: se conservan tu
configuración y el modelo. Todo lo demás (comandos de uso diario, actualizaciones, inicio automático, qué hacer cuando no
mina) está en la [guía](GUIDE.es.md).

## Windows

1. Descomprime todo `TheNewMiner-v0.1.47-windows-wsl2-r3.zip` (clic derecho › Extraer todo) donde quieras.
2. Haz doble clic en **`TheNewMiner.bat`**. La primera vez, UNA ventana te pide tu cuenta del pool (billetera,
   contraseña del pool, nombre del worker); después no se pregunta nada más: configura WSL2 y Ubuntu, instala el minero
   incluido, descarga el modelo y empieza a minar. Los dobles clics posteriores empiezan a minar de inmediato. Es
   posible que Windows muestre primero "Windows protegió su PC" (el kit no tiene firma de código): haz clic en **Más
   información** y después en **Ejecutar de todas formas**. Activar WSL pide permisos de administrador una vez y puede
   requerir un reinicio; después la instalación continúa sola.
3. Si hay una versión anterior, se detecta y se actualiza en su sitio, con tu configuración y el modelo.

Si se detiene o mina despacio, consulta la [solución de problemas en Windows](WINDOWS-TROUBLESHOOTING.es.md) (DNS,
unidades de Windows, la VRAM que usan los navegadores y Docker Desktop, el reloj).

## HiveOS (experimental)

1. **Wallets:** añade una billetera con la moneda `TSC` y tu dirección de TensorCash (`tc1...`).
2. Abre [`hiveos/flightsheet-thenewminer.json`](../../hiveos/flightsheet-thenewminer.json), copia su contenido y
   sustituye `<YOUR_WALLET>` (mantén `.%WORKER_NAME%` detrás) y `<YOUR_POOL_PASSWORD>`. En HiveOS elige Flight Sheets ›
   **Import from clipboard**, pégalo, selecciona tu billetera `TSC` y guárdalo.
3. Aplica el flight sheet a tu rig. Instala `thenewminer-0.1.47.tar.gz` y empieza a minar.

Configuración manual, actualizaciones y un rig que no puede acceder a GitHub: consulta la [guía](GUIDE.es.md).

## Novedades de la v0.1.47

- **Instalación más sencilla:** en una máquina con una sola GPU, el minero comprueba tu billetera y tu contraseña del
  pool con el pool **antes** de descargar el modelo de 8.1 GiB, así que un error de escritura se detecta en segundos.
  Una dirección de billetera con un error de escritura (su suma de verificación no coincide) o la dirección de ejemplo
  se rechaza, indicando qué está mal.
- **Windows: las credenciales se piden una sola vez,** en una ventana al principio, antes de cualquier paso largo. Una
  instalación hecha por una versión anterior, también en otra distribución de WSL, se detecta y se **actualiza en su
  sitio** (se conservan la configuración y el modelo). Si Ubuntu no se puede descargar de Microsoft, la instalación toma
  la imagen WSL oficial de Ubuntu de un mirror en China y solo la usa si su SHA-256 coincide.
- **Sin apt en un Ubuntu estándar:** Ubuntu 22.04 / 24.04 (incluido el de WSL) ya tiene todo lo que necesita el minero,
  así que no se instala nada. Si falta algo, el primer inicio lo enumera antes de instalarlo.
- **Un paquete en una unidad de Windows** (como `/mnt/d` en WSL) se rechaza de inmediato, con una línea lista para pegar
  que lo copia a tu carpeta personal de Linux y lo inicia allí.
- **Mensajes de red claros:** cuando el DNS no funciona dentro de WSL, la instalación se detiene en ese paso con la
  solución; un minero que no puede llegar a internet o al pool indica qué hay que comprobar.
- **El modelo se reutiliza:** una actualización en una carpeta nueva encuentra el modelo de una instalación anterior
  (mismo tamaño y mismo SHA-256) y no lo vuelve a descargar.
- **`--status` explica por qué** se detuvo el minero y cómo solucionarlo, también después de un inicio con
  `--background`.
- **HiveOS:** ahora cada fallo espera y lo vuelve a intentar con pausas crecientes (de uno a diez minutos) y muestra su
  motivo en el log y en el panel.

Notas completas: [`docs/releases/RELEASE-NOTES-v0.1.47.md`](../releases/RELEASE-NOTES-v0.1.47.md).

## Problemas conocidos

- **Las tarjetas de 10 GB en Windows necesitan unos 9.3 GB de VRAM libre para ir a toda velocidad.** Con menos, el
  minero usa una disposición más lenta (una RTX 3080 10 GB bajó de 21–22 a unos 9 N/s). Un navegador con aceleración por
  hardware o Docker Desktop pueden ocupar esa memoria: ciérralos (o desactiva la aceleración por hardware del
  navegador) y vuelve a iniciar la minería.
- **Reloj de Windows:** si el minero informa de un error de reloj, abre Configuración de Windows › Hora e idioma › Fecha
  y hora, activa "Establecer la zona horaria automáticamente" y "Establecer la hora automáticamente", haz clic en
  **Sincronizar ahora**; después ejecuta `wsl --shutdown` y vuelve a iniciarlo.

## Hashrates

Los mejores hashrates medidos en el pool real con nuestros propios rigs. Son cifras **máximas** ("hasta"), no una
promesa: tarjetas del mismo modelo varían alrededor de un 5–8%, y el hashrate baja si la tarjeta también maneja una
pantalla o si otros programas usan la GPU.

| GPU | Hashrate |
|---|---|
| RTX 5090 | hasta ~99 N/s |
| RTX 4090 | hasta ~70 N/s |
| RTX 3080 10 GB | hasta ~22 N/s |

Mediciones fechadas por versión: [resultados de pruebas](../TEST-RESULTS.md). Los hitos del proyecto:
[registro de desarrollo](../DEVELOPMENT-RECORD.md).

## Informar de un problema

Abre un issue en [GitHub Issues](https://github.com/ngasuka855/TheNewMiner/issues/new/choose) y rellena el formulario de
informe de error: la versión de TheNewMiner, tu sistema, el modelo o los modelos de GPU y el controlador, la salida de
`./thenewminer --status` y las últimas líneas del log. Los issues son públicos: **nunca publiques tu contraseña del pool
ni la clave privada de tu billetera**, y elimina la contraseña (y, si lo prefieres, tu dirección de billetera) de todo
lo que pegues.

## Nota de seguridad

El minero y el motor de GPU se distribuyen cifrados (ChaCha20-Poly1305) dentro del paquete; el minero los descifra en
tu máquina al iniciarse. Esto encarece copiarlos, pero **no es irrompible**: cualquiera con acceso de administrador a una
máquina que ejecute el minero puede, con suficiente esfuerzo, recuperar la clave o el código descifrado. Tu dirección de
billetera y tu contraseña del pool se guardan en tu propia máquina y solo se envían al pool.
