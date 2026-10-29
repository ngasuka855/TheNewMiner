[English](../WINDOWS-TROUBLESHOOTING.md) · [简体中文](WINDOWS-TROUBLESHOOTING.zh-CN.md) · [繁體中文](WINDOWS-TROUBLESHOOTING.zh-TW.md) · [Русский](WINDOWS-TROUBLESHOOTING.ru.md) · **Español** · [Português (Brasil)](WINDOWS-TROUBLESHOOTING.pt-BR.md) · [Français](WINDOWS-TROUBLESHOOTING.fr.md) · [Deutsch](WINDOWS-TROUBLESHOOTING.de.md) · [Türkçe](WINDOWS-TROUBLESHOOTING.tr.md) · [Tiếng Việt](WINDOWS-TROUBLESHOOTING.vi.md) · [한국어](WINDOWS-TROUBLESHOOTING.ko.md) · [日本語](WINDOWS-TROUBLESHOOTING.ja.md)

> Esta traducción se ofrece por comodidad; el README en inglés es la versión de referencia. *(This translation is provided for convenience; the English README is authoritative.)*

# Solución de problemas en Windows

En Windows, TheNewMiner ejecuta el minero para Linux dentro de WSL2. Cuando la instalación se detiene, la ventana indica
en qué paso y qué hacer, y `TheNewMiner.bat` lo vuelve a mostrar (también se guarda en
`%LOCALAPPDATA%\TheNewMiner\last-setup-failure.txt`). Esta página explica los casos habituales. La instalación y los
comandos de uso diario están en la [guía](GUIDE.es.md).

## El DNS no funciona dentro de WSL

En el paso 4, la instalación comprueba que WSL puede resolver los nombres del pool y de la descarga del modelo. Si no
puede, la instalación se detiene ahí y no cambia nada por sí sola. Abre `%UserProfile%\.wslconfig` en el Bloc de notas
(crea el archivo si no existe) y añade estas dos líneas (en Windows 11 22H2 o posterior, `networkingMode=mirrored` en
`[wsl2]` también funciona):

```ini
[wsl2]
dnsTunneling=true
```

Después ejecuta `wsl --shutdown` en un Símbolo del sistema (detiene todas las distribuciones de WSL, también la de
Docker Desktop) y vuelve a hacer doble clic en `TheNewMiner.bat`.

Una VPN o un firewall que bloquee WSL puede provocar el mismo mensaje: permite WSL, o configura un proxy con
`TheNewMiner.bat setup -Proxy ...` (consulta "Proxy" en la [guía](GUIDE.es.md)).

## No se puede descargar Ubuntu, o la virtualización está desactivada

- **Red:** la instalación instala Ubuntu 24.04 con `wsl --install`. Donde esa descarga no puede conectarse (por
  ejemplo, donde GitHub no es accesible), la instalación descarga en su lugar la imagen WSL oficial de Ubuntu de un
  mirror público en China y solo la usa si su SHA-256 coincide. Si tampoco responde ningún mirror, comprueba la
  conexión a internet y vuelve a intentarlo más tarde.
- **Virtualización:** un mensaje sobre la virtualización (por ejemplo `0x80370102`) significa que está desactivada.
  Activa Intel VT-x / AMD SVM en la BIOS y vuelve a hacer doble clic en `TheNewMiner.bat`.

## El paquete está en una unidad de Windows

Si descomprimiste tú mismo el paquete para Linux e iniciaste `./thenewminer` dentro de WSL desde una unidad de Windows
(como `/mnt/d/...`), el minero se niega a iniciarse: una unidad de Windows no puede conservar su archivo de
configuración privado. Muestra una línea `mkdir -p ~/thenewminer && cp -a ...` lista para pegar, que copia la carpeta a
tu carpeta personal de Linux y lo inicia allí. O usa en su lugar el zip de Windows: `TheNewMiner.bat` lo coloca todo en
el sitio correcto por sí solo.

## Lento en una tarjeta de 10 GB (navegadores, Docker Desktop)

Una tarjeta de 10 GB solo obtiene la disposición rápida cuando hay unos 9.3 GB (9472 MiB) de VRAM libres al empezar a
minar. En Windows, otros programas comparten la tarjeta: un navegador con aceleración por hardware o Docker Desktop
pueden ocupar tanta VRAM que el minero recurre a una disposición más lenta. En una RTX 3080 10 GB esto supuso unos
9 N/s en lugar de 21–22 N/s.

1. Cierra los navegadores o desactiva su aceleración por hardware (Chrome y Edge: Configuración › Sistema › "Usar
   aceleración de gráficos cuando esté disponible"; Firefox: Ajustes › General › Rendimiento) y cierra Docker Desktop.
2. Comprueba la VRAM libre en un Símbolo del sistema: `nvidia-smi --query-gpu=memory.used,memory.total --format=csv`
   (el total menos lo usado debe ser al menos 9472 MiB).
3. Reinicia la minería: `TheNewMiner.bat stop` y después haz doble clic en `TheNewMiner.bat`.

Las tarjetas de 12 GB necesitan los mismos 9472 MiB libres; las tarjetas de 16 GB o más no se ven afectadas.

## Errores de reloj

El pool detiene un minero cuyo reloj tiene un desfase de unos 30 segundos. El reloj de WSL2 puede desajustarse, por
ejemplo después de que el PC entre en suspensión, y la instalación avisa cuando está desfasado. Si el minero informa de
un error de reloj:

1. Abre Configuración de Windows › Hora e idioma › Fecha y hora, activa "Establecer la zona horaria automáticamente" y
   "Establecer la hora automáticamente", y haz clic en **Sincronizar ahora**.
2. En un Símbolo del sistema, ejecuta `wsl --shutdown`.
3. Vuelve a iniciar la minería.

## Avisos de seguridad de Windows

SmartScreen ("Windows protegió su PC") y los antivirus que marcan el minero: consulta las preguntas frecuentes de la
[guía](GUIDE.es.md).

## ¿Sigues sin poder resolverlo?

Abre un issue en [GitHub Issues](https://github.com/ngasuka855/TheNewMiner/issues/new/choose) con la salida de
`TheNewMiner.bat status`, el paso que falló y el contenido de `last-setup-failure.txt`. Nunca publiques tu contraseña
del pool ni la clave privada de tu billetera.
