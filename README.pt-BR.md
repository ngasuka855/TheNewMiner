[English](README.md) · [简体中文](README.zh-CN.md) · [繁體中文](README.zh-TW.md) · [Русский](README.ru.md) · [Español](README.es.md) · **Português (Brasil)** · [Français](README.fr.md) · [Deutsch](README.de.md) · [Türkçe](README.tr.md) · [Tiếng Việt](README.vi.md) · [한국어](README.ko.md) · [日本語](README.ja.md)

> Esta tradução é fornecida por conveniência; o README em inglês é a versão de referência. *(This translation is provided for convenience; the English README is authoritative.)*

# TheNewMiner

O TheNewMiner é um minerador de GPU para **TensorCash**. Ele roda em placas NVIDIA GeForce RTX das séries 30, 40 e 50
com **10 GB ou mais de VRAM** e minera na pool **`tsc-wss.the-new.win`**, que já vem embutida: você não precisa
informar o endereço da pool, e a conexão é direta.

Versão atual: **v0.1.44**

## Download

| Arquivo | Para | SHA-256 |
|---|---|---|
| [`TheNewMiner-v0.1.44-linux-x86_64-sealed-r4.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/main/TheNewMiner-v0.1.44-linux-x86_64-sealed-r4.tar.xz) | Linux x86_64 (Ubuntu) | `99a8246e242c7d09f1c4b6335d974de0d163362d4dc634fff4188ab58a151ca6` |
| [`TheNewMiner-v0.1.44-windows-wsl2-r5.zip`](https://github.com/ngasuka855/TheNewMiner/raw/main/TheNewMiner-v0.1.44-windows-wsl2-r5.zip) | Windows 10 / 11 (roda pelo WSL2) | `264025136cf01d87d25ca2bb0554fe88efe01833850a697247cd657027215506` |
| [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/main/SHA256SUMS) | checksums dos dois pacotes | |

O zip para Windows é tudo em um: ele já contém o minerador para Linux, então você só precisa do zip.

## Verificar o download

Coloque o(s) pacote(s) e o `SHA256SUMS` na mesma pasta e depois:

```bash
# Linux (adicione --ignore-missing se você baixou só um pacote)
sha256sum -c SHA256SUMS
# macOS
shasum -a 256 -c SHA256SUMS
```

```bat
:: Windows (Prompt de Comando): compare a saída com o SHA-256 da tabela acima
certutil -hashfile TheNewMiner-v0.1.44-windows-wsl2-r5.zip SHA256
```

Cada arquivo deve mostrar `OK` (no Windows: os mesmos 64 caracteres da tabela). Se não bater, não use o arquivo.

## Linux

```bash
tar -xJf TheNewMiner-v0.1.44-linux-x86_64-sealed-r4.tar.xz
cd thenewminer
./thenewminer -u <YOUR_WALLET> -p <YOUR_POOL_PASSWORD> -w rig-01 --background
```

| Comando | O que faz |
|---|---|
| `./thenewminer -u <YOUR_WALLET> -p <YOUR_POOL_PASSWORD> -w rig-01 --background` | Primeira inicialização: instala o que for necessário e depois minera em segundo plano |
| `./thenewminer --background` | Inicializações seguintes: suas configurações ficam salvas |
| `./thenewminer` | Minerar em primeiro plano |
| `./thenewminer --logs` | Acompanhar o minerador (Ctrl+C sai da visualização; a mineração continua) |
| `./thenewminer --stop` | Parar a mineração |

- `-u` é o endereço da sua carteira TensorCash, `-p` a senha da pool e `-w` o nome do worker exibido na pool
  (padrão: o nome de host da máquina).
- As configurações ficam salvas em `thenewminer/.miner/` (o arquivo com a senha só pode ser lido por você).
- A primeira inicialização baixa o modelo (cerca de 8.1 GiB, uma única vez), instala o motor de GPU para a sua placa e
  executa um autoteste na placa; depois começa a minerar. Linhas como `[ RATE] ... N/s` indicam que ele está minerando.
- A primeira inicialização pode instalar alguns pacotes do Ubuntu com `apt` (requer sudo ou root).
- Exatamente uma GPU NVIDIA por máquina nesta versão (rigs com várias GPUs ainda não são suportados).
- Se o `tar` mostrar `xz: Cannot exec`, instale o xz primeiro: `sudo apt-get install -y xz-utils`.
- O `README.md` dentro do pacote é uma nota interna antiga; siga esta página (não é preciso usar `--proxy`).
- **Atualização:** `./thenewminer --stop`, descompacte o novo pacote no mesmo lugar (ele é descompactado em `thenewminer/` e
  mantém `thenewminer/.miner/` com suas configurações e o modelo) e inicie novamente.

## Windows (com um clique)

1. Descompacte o `TheNewMiner-v0.1.44-windows-wsl2-r5.zip` inteiro (botão direito › Extrair Tudo) em qualquer lugar:
   qualquer pasta, qualquer unidade; caminhos com espaços ou caracteres chineses não são problema. Não execute de dentro
   da visualização do zip.
2. Clique duas vezes em **`TheNewMiner.bat`**.
   - **Na primeira vez:** ele verifica o Windows, o driver e o disco, ativa o WSL2 se necessário (um aviso de
     administrador; pode ser preciso reiniciar uma vez, e a instalação continua sozinha depois), mostra onde os dados
     vão ficar (Enter aceita), cria seu próprio ambiente Linux (WSL), instala o minerador incluído, pede **uma única
     vez** o endereço da sua carteira (começa com `tc1`), a senha da pool (digitada duas vezes; de 6 a 128 letras,
     dígitos, `_` ou `-`) e o nome do worker (Enter = o nome do computador), baixa o modelo e começa a minerar.
   - **Nas vezes seguintes:** clique duas vezes e ele começa a minerar na hora com as configurações salvas. Também é
     criado um atalho "TheNewMiner" na área de trabalho.

Comandos do dia a dia (abra um Prompt de Comando na pasta descompactada):

| Comando | O que faz |
|---|---|
| `TheNewMiner.bat` | Instala se necessário e depois minera nesta janela (fechar a janela para a mineração) |
| `TheNewMiner.bat background` | Minerar sem janela |
| `TheNewMiner.bat logs` | Acompanhar o minerador (Ctrl+C sai da visualização; a mineração continua) |
| `TheNewMiner.bat status` | Se está rodando e os últimos hashrates |
| `TheNewMiner.bat stop` | Parar a mineração |
| `TheNewMiner.bat setup` | Informar de novo a carteira / senha / nome do worker |
| `TheNewMiner.bat uninstall` | Remover o minerador, sua pasta de dados e o atalho (pergunta antes) |

- **Onde ficam os dados:** o minerador e o modelo ficam em uma pasta de dados escolhida na primeira instalação (por
  padrão `TheNewMinerData` na raiz da unidade onde você descompactou, por exemplo `D:\TheNewMinerData`), e não na pasta
  descompactada. É preciso ter cerca de 20 GB livres nela.
- **Atualização:** descompacte a nova versão em qualquer lugar e clique duas vezes no `TheNewMiner.bat` dela. Ele
  encontra o minerador instalado, mantém suas configurações e o modelo e instala a nova versão sem fazer perguntas. A
  pasta antiga pode ser apagada.
- **Desinstalação:** `TheNewMiner.bat uninstall`.
- O idioma na tela segue o do Windows (English / 简体中文 / 繁體中文). Para escolher, adicione `-Lang en` (ou `zh-Hans`,
  `zh-Hant`) a qualquer comando, por exemplo `TheNewMiner.bat -Lang en`; vale para aquela execução.

## Proxy (opcional)

O minerador se conecta diretamente à pool. Se a sua rede precisar de um proxy:

```bash
# Linux (fica salvo até ser alterado)
./thenewminer --proxy <IP>[:<PORT>]            # relay TCP (porta 443 se omitida)
./thenewminer --proxy socks5://<HOST>:<PORT>   # SOCKS5, também socks5://<USER>:<PASS>@<HOST>:<PORT>
./thenewminer --proxy http://<HOST>:<PORT>     # proxy HTTP CONNECT
./thenewminer --proxy none                     # volta para a conexão direta
```

Esses comandos também iniciam o minerador: pare o minerador primeiro e adicione `--background` para mantê-lo em segundo plano.

```bat
:: Windows
TheNewMiner.bat setup -Proxy <IP>[:<PORT>]
TheNewMiner.bat setup -Proxy socks5://<HOST>:<PORT>
TheNewMiner.bat setup -Proxy http://<HOST>:<PORT>
TheNewMiner.bat setup -Proxy none
```

O TLS sempre vai de ponta a ponta com a pool, então o proxy não consegue ler a conexão. No Windows, um app de proxy
rodando no mesmo PC normalmente não é acessível como `127.0.0.1` a partir do WSL (rede padrão do WSL): use o IP de LAN
do PC e permita conexões da LAN no app.

## Requisitos

| Item | Requisito |
|---|---|
| GPU | Exatamente uma placa NVIDIA GeForce RTX série 30 / 40 / 50, com 10 GB de VRAM ou mais |
| Driver | NVIDIA **580 ou mais recente** (o `nvidia-smi` precisa funcionar). No Windows, instale no Windows, nunca dentro do WSL |
| Linux | Ubuntu 22.04, 24.04 ou 26.04 x86_64 (outras distribuições ainda não são suportadas) |
| Windows | Windows 10 21H2 ou mais recente, ou Windows 11, 64 bits; virtualização (Intel VT-x / AMD SVM) ativada na BIOS |
| Disco | cerca de 20 GB livres |
| RAM | 4 GB ou mais |
| Modelo | cerca de 8.1 GiB, baixado uma vez na primeira inicialização (em paralelo, de vários mirrors) e reutilizado depois |

## Hashrates de referência

Os melhores hashrates medidos na pool real com nossos próprios rigs (entre 2026-10-14 e 2026-10-23). São valores
**máximos** ("até"), não uma promessa: placas do mesmo modelo variam cerca de 5–8% conforme o chip individual, a
refrigeração, os clocks e o limite de energia, e o hashrate cai se a placa também alimenta um monitor ou se outros
programas usam a GPU.

| GPU | Hashrate |
|---|---|
| RTX 5090 | até ~99 N/s |
| RTX 4090 | até ~70 N/s |
| RTX 3080 10 GB | até ~22 N/s |

## Resultados dos testes

Testes na pool real da v0.1.44 em nossos rigs de teste em 2026-10-23 (uma GPU por máquina). A = shares aceitas,
R = shares rejeitadas.

| Data | Placa | Sistema operacional | Pacote | Hashrate | A / R |
|---|---|---|---|---|---|
| 2026-10-23 | RTX 4090 | Linux | sealed-r4 | 65.58 N/s (mediana, 16 min) | 10 / 0 |
| 2026-10-23 | RTX 5090 | Linux | sealed-r4 | 93.45 N/s (mineração estável, 44 min; 90.39 incluindo trocas de job) | 31 / 0 (+1 stale ao parar) |
| 2026-10-23 | RTX 3080 10 GB | Windows (WSL2) | windows-wsl2-r5 | ~22 N/s (execução do próprio operador) | n/d |
| 2026-10-23 | RTX 3080 10 GB | Linux (Ubuntu 26.04) | sealed-r4 | 21.04 N/s (últimos 10 min de 23 min de mineração) | 3 / 0 |

RTX 3080 no Linux: instalação com 6/6 etapas OK, nenhuma janela inválida, parada limpa. Trecho do log:
[`test-results/rtx3080-linux-20261023.txt`](test-results/rtx3080-linux-20261023.txt).

## Capturas de tela

Dos nossos rigs de teste, com versões anteriores: v0.1.41 na RTX 4090 e uma build inicial na RTX 5090. São leituras
ao vivo; os seus números dependem da placa.

**RTX 4090, v0.1.41:** 69–75 N/s por leitura, na maioria ~70 (média de 10 minutos ~68), 9 shares aceitas, 0 rejeitadas, ~445 W.

![TheNewMiner v0.1.41 minerando em uma RTX 4090](screenshots/rtx4090.png)

**RTX 5090, build inicial:** 100.83 N/s (média ao vivo das janelas concluídas), ~575 W.

![Uma build inicial do TheNewMiner minerando em uma RTX 5090](screenshots/rtx5090.png)

## Perguntas frequentes

**O Windows diz "O Windows protegeu o computador" / "Editor desconhecido".**
O kit não tem assinatura de código. Clique em **Mais informações** e depois em **Executar assim mesmo**.

**Meu antivírus acusa ou apaga o minerador.**
O Microsoft Defender e outros antivírus costumam marcar mineradores de criptomoedas como "aplicativos potencialmente
indesejados", mesmo um legítimo que você escolheu executar, porque malwares frequentemente escondem mineradores. Se um
arquivo for colocado em quarentena, abra Segurança do Windows › Proteção contra vírus e ameaças › Histórico de proteção,
selecione o item do TheNewMiner e escolha **Permitir** (ou adicione a pasta descompactada às Exclusões). Faça isso
somente com um pacote baixado desta página cujo SHA-256 confira.

**A primeira inicialização demora muito.**
A primeira inicialização baixa o modelo de ~8.1 GiB e executa um autoteste de GPU na sua placa; no Windows, ela também
pode configurar o WSL2. Isso acontece uma única vez. As inicializações seguintes começam a minerar bem mais rápido.

**Onde ficam os logs?**
Linux: `./thenewminer --logs`, ou o arquivo `thenewminer/.miner/thenewminer.log`.
Windows: `TheNewMiner.bat logs` (e `TheNewMiner.bat status` para os últimos hashrates).

**HiveOS?**
Um pacote para HiveOS **chega em breve**; ele ainda está em testes.

## Nota de segurança

O minerador e o motor de GPU são distribuídos criptografados (ChaCha20-Poly1305) dentro do pacote; o minerador os
descriptografa na sua máquina ao iniciar. Isso aumenta o custo de copiá-los, mas **não é inquebrável**: qualquer pessoa
com acesso de administrador a uma máquina que esteja rodando o minerador pode, com esforço suficiente, recuperar a chave
ou o código descriptografado.
O endereço da sua carteira e a senha da pool ficam armazenados na sua própria máquina e são enviados somente à pool.
