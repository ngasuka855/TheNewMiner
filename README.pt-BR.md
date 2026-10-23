[English](README.md) · [简体中文](README.zh-CN.md) · [繁體中文](README.zh-TW.md) · [Русский](README.ru.md) · [Español](README.es.md) · **Português (Brasil)** · [Français](README.fr.md) · [Deutsch](README.de.md) · [Türkçe](README.tr.md) · [Tiếng Việt](README.vi.md) · [한국어](README.ko.md) · [日本語](README.ja.md)

> Esta tradução é fornecida por conveniência; o README em inglês é a versão de referência. *(This translation is provided for convenience; the English README is authoritative.)*

# TheNewMiner

O TheNewMiner é um minerador de GPU para **TensorCash**. Ele roda em placas NVIDIA GeForce RTX das séries 30, 40 e 50
com **10 GB ou mais de VRAM** e minera na pool **`tsc-wss.the-new.win`**, que já vem embutida: você não precisa
informar o endereço da pool, e a conexão é direta.

Versão atual: **v0.1.45**

## Novidades da v0.1.45

- **Uma mensagem clara quando a pool rejeita sua carteira ou sua senha.** O minerador avisa em uma única linha e para,
  em vez de ficar tentando de novo; o `--status` mostra o motivo. Confira a carteira e a senha e inicie novamente com
  `-u` e `-p`.
- **Sua carteira e sua senha não aparecem mais no `ps`** nem nas listas de processos de outros usuários.
- **`./thenewminer --status`** mostra se ele está minerando, desde quando, o último hashrate e as shares aceitas / rejeitadas.
- **Suporte a HiveOS (experimental):** um pacote de minerador personalizado (custom miner) para HiveOS e uma flight sheet importável (veja a seção HiveOS abaixo).
- **Atualizações seguras:** a nova versão se recusa a iniciar enquanto a antiga ainda estiver minerando (ela pede que
  você pare a antiga primeiro); depois do `--stop`, ela reinstala os próprios arquivos de programa sozinha e mantém suas
  configurações e o modelo.
- **O `--background` não diz mais "iniciado" quando o minerador encerra logo em seguida**; ele mostra o motivo.
- **Docker e memória compartilhada pequena:** quando a pasta em memória é pequena demais (o padrão do Docker é 64 MiB)
  ou não permite executar programas, o minerador usa `thenewminer/.miner/run/` no disco e avisa.
- **As senhas da pool agora podem usar qualquer caractere visível** (inclusive pontuação e letras não inglesas): de 6 a
  128 caracteres, sem espaços.
- Mais sistemas Linux: o instalador agora verifica o que precisa (glibc 2.34 ou mais recente) em vez de aceitar só
  algumas versões do Ubuntu.

## Download

| Arquivo | Para | SHA-256 |
|---|---|---|
| [`TheNewMiner-v0.1.45-linux-x86_64-sealed.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/TheNewMiner-v0.1.45-linux-x86_64-sealed.tar.xz) | Linux x86_64 | `6281bf489fae843551114f09ead5498d3e47af33a07e4cfdfbdc0a5d6bdbafbb` |
| [`TheNewMiner-v0.1.45-windows-wsl2-r1.zip`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/TheNewMiner-v0.1.45-windows-wsl2-r1.zip) | Windows 10 / 11 (roda pelo WSL2) | `90732441b0dc67de84809d9bf3bda4441c0bf7cd9a52fefb95d29242ad7b15e7` |
| [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/SHA256SUMS) | checksums de todos os pacotes | |

O zip para Windows é tudo em um: ele já contém o minerador para Linux, então você só precisa do zip.

## Verificar o download

Coloque o(s) pacote(s) e o `SHA256SUMS` na mesma pasta e depois:

```bash
# Linux (--ignore-missing: só os pacotes que você baixou são verificados)
sha256sum -c --ignore-missing SHA256SUMS
# macOS
shasum -a 256 -c --ignore-missing SHA256SUMS
```

```bat
:: Windows (Prompt de Comando): compare a saída com o SHA-256 da tabela acima
certutil -hashfile TheNewMiner-v0.1.45-windows-wsl2-r1.zip SHA256
```

Cada arquivo deve mostrar `OK` (no Windows: os mesmos 64 caracteres da tabela). Se não bater, não use o arquivo.

## Instalação com um clique (Linux)

Substitua `<YOUR_WALLET>`, `<YOUR_POOL_PASSWORD>` e `<RIG_NAME>` (incluindo os `<` `>`) no final deste comando e
depois cole-o em um terminal. Ele baixa o pacote para Linux e o `SHA256SUMS` desta versão (com `curl`, ou com `wget` se
não houver `curl`), verifica o SHA-256, descompacta o minerador em `~/thenewminer` e o inicia em segundo plano:

```bash
mkdir -p ~/thenewminer-download && cd ~/thenewminer-download && V=v0.1.45 && P=TheNewMiner-$V-linux-x86_64-sealed.tar.xz && U=https://github.com/ngasuka855/TheNewMiner/raw/$V && rm -f "$P" SHA256SUMS && if command -v curl >/dev/null 2>&1; then curl -fL -o "$P" "$U/$P" && curl -fL -o SHA256SUMS "$U/SHA256SUMS"; else wget -O "$P" "$U/$P" && wget -O SHA256SUMS "$U/SHA256SUMS"; fi && sha256sum -c --ignore-missing SHA256SUMS && tar -xJf "$P" -C ~ && cd ~/thenewminer && ./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w <RIG_NAME> --background
```

Ele para na primeira etapa que falhar: um erro de download ou um SHA-256 que não bate nunca inicia nada (a verificação
protege contra um download corrompido; os dois arquivos vêm desta página). Mantenha as aspas simples em volta da senha:
dentro delas qualquer caractere é seguro (uma `'` na senha é escrita como `'\''`). Se o `tar` mostrar `xz: Cannot exec`,
instale o xz (`sudo apt-get install -y xz-utils`) e execute o comando de novo.

Para atualizar uma versão que já está em `~/thenewminer`, primeiro execute `~/thenewminer/thenewminer --stop` e depois
este comando (suas configurações e o modelo são mantidos). Se a sua versão anterior estiver em outra pasta, pare-a
primeiro lá, ou atualize-a lá como descrito na seção Linux (caso contrário, o modelo é baixado de novo).

## Linux

```bash
tar -xJf TheNewMiner-v0.1.45-linux-x86_64-sealed.tar.xz
cd thenewminer
./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background
```

| Comando | O que faz |
|---|---|
| `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background` | Primeira inicialização: instala o que for necessário e depois minera em segundo plano |
| `./thenewminer --background` | Inicializações seguintes: suas configurações ficam salvas |
| `./thenewminer` | Minerar em primeiro plano (Ctrl+C para) |
| `./thenewminer --logs` | Acompanhar o minerador (Ctrl+C sai da visualização; a mineração continua) |
| `./thenewminer --status` | Se está minerando, o último hashrate e as shares aceitas / rejeitadas |
| `./thenewminer --stop` | Parar a mineração |
| `./thenewminer --help` | Todas as opções |

- `-u` é o endereço da sua carteira TensorCash, `-p` a senha da pool e `-w` o nome do worker exibido na pool
  (padrão: o nome de host da máquina; de 1 a 64 letras, dígitos, `_`, `-` ou `.`).
- A senha da pool: de 6 a 128 caracteres, qualquer caractere visível, sem espaços. Coloque-a sempre entre aspas simples
  na linha de comando, por exemplo `-p 'my$pass!'` (uma `'` dentro de aspas simples é escrita como `'\''`).
- As configurações ficam salvas em `thenewminer/.miner/` (o arquivo com a senha só pode ser lido por você). A carteira
  e a senha são retiradas da linha de comando na hora, então não aparecem no `ps` (o seu shell ainda pode guardar o
  comando no histórico).
- Se a pool rejeitar a carteira ou a senha (ou ainda não tiver aprovado a conta), o minerador avisa e para. Confira os
  dados e inicie novamente com `-u` e `-p`.
- A primeira inicialização baixa o modelo (cerca de 8.1 GiB, uma única vez), instala o motor de GPU para a sua placa e
  executa um autoteste na placa; depois começa a minerar. Linhas como `[ RATE] ... N/s` indicam que ele está minerando.
- No Ubuntu e no Debian, a primeira inicialização pode instalar com `apt` algumas bibliotecas do sistema que estejam
  faltando (requer sudo ou root); em outros sistemas, ela informa quais você precisa instalar.
- Rigs com várias GPUs: suportados, cada GPU minera; esta versão foi testada com uma GPU por máquina.
- Se o `tar` mostrar `xz: Cannot exec`, instale o xz primeiro: `sudo apt-get install -y xz-utils`.
- No Docker ou em outros contêineres com pouca memória compartilhada, o minerador executa seus arquivos de programa a
  partir de `thenewminer/.miner/run/` no disco e avisa. Para escolher outra pasta, inicie-o com
  `TNPSEAL_RUNTIME_BASE=<folder> ./thenewminer` (uma pasta em que só você possa gravar: `chmod 700 <folder>`).
- **Atualização:** `./thenewminer --stop`, descompacte o novo pacote no mesmo lugar (ele é descompactado em `thenewminer/` e
  mantém `thenewminer/.miner/` com suas configurações e o modelo) e inicie novamente. Se a versão antiga ainda estiver
  rodando, a nova se recusa a iniciar e pede que você pare a antiga primeiro.

## Windows (com um clique)

1. Descompacte o `TheNewMiner-v0.1.45-windows-wsl2-r1.zip` inteiro (botão direito › Extrair Tudo) em qualquer lugar:
   qualquer pasta, qualquer unidade; caminhos com espaços ou caracteres chineses não são problema. Não execute de dentro
   da visualização do zip.
2. Clique duas vezes em **`TheNewMiner.bat`**.
   - **Na primeira vez:** ele verifica o Windows, o driver e o disco, ativa o WSL2 se necessário (um aviso de
     administrador; pode ser preciso reiniciar uma vez, e a instalação continua sozinha depois), mostra onde os dados
     vão ficar (Enter aceita), cria seu próprio ambiente Linux (WSL), instala o minerador incluído, pede **uma única
     vez** o endereço da sua carteira (começa com `tc1`), a senha da pool (digitada duas vezes; de 6 a 128 caracteres,
     qualquer caractere visível, sem espaços) e o nome do worker (Enter = o nome do computador), baixa o modelo e
     começa a minerar.
   - **Nas vezes seguintes:** clique duas vezes e ele começa a minerar na hora com as configurações salvas. Também é
     criado um atalho "TheNewMiner" na área de trabalho. Clicar duas vezes de novo enquanto ele já está minerando
     avisa isso e mostra o log.

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
- Se a pool rejeitar a carteira ou a senha, a janela avisa e a mineração para: execute `TheNewMiner.bat setup` para
  informá-las de novo.
- **Atualização:** descompacte a nova versão em qualquer lugar e clique duas vezes no `TheNewMiner.bat` dela. Ele
  encontra o minerador instalado, mantém suas configurações e o modelo e instala a nova versão sem fazer perguntas. A
  pasta antiga pode ser apagada.
- **Desinstalação:** `TheNewMiner.bat uninstall`.
- O idioma na tela segue o do Windows (English / 简体中文 / 繁體中文). Para escolher, adicione `-Lang en` (ou `zh-Hans`,
  `zh-Hant`) a qualquer comando, por exemplo `TheNewMiner.bat -Lang en`; vale para aquela execução.

## HiveOS (experimental)

O suporte a HiveOS é novo na v0.1.45 e **experimental**. Ele usa o pacote de minerador personalizado (custom miner)
para HiveOS abaixo.

| Arquivo | SHA-256 |
|---|---|
| [`thenewminer-0.1.45.tar.gz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/thenewminer-0.1.45.tar.gz) | `90576efd388ceeca4e456b62b568f267465320913a07c9f9736f42363b1701dc` |

Configuração rápida (importar a flight sheet):

1. **Wallets:** adicione uma carteira com a moeda `TSC` e o seu endereço TensorCash (`tc1...`). Isso só é necessário uma
   vez: o HiveOS precisa de uma carteira `TSC` selecionada, e o minerador usa o endereço que você colocar na flight
   sheet.
2. Abra [`hiveos/flightsheet-thenewminer.json`](hiveos/flightsheet-thenewminer.json) e copie o conteúdo. Substitua
   `<YOUR_WALLET>` pelo endereço da sua carteira (mantenha `.%WORKER_NAME%` depois dele) e `<YOUR_POOL_PASSWORD>` pela
   senha da sua pool (no JSON, uma `"` na senha é escrita como `\"` e uma `\` é escrita como `\\`). No HiveOS, escolha
   Flight Sheets › **Import from clipboard**, cole o conteúdo, selecione a sua carteira `TSC` e salve.
3. Aplique a flight sheet ao seu rig. O rig baixa o pacote e começa a minerar.

Configuração manual, como alternativa: crie uma flight sheet com Coin `TSC`, Wallet: a sua carteira `TSC`, Pool:
"Configure in miner", Miner: "Custom", clique em **Setup Miner Config** e preencha:

| Campo | Valor |
|---|---|
| Miner name | `thenewminer` (preenchido a partir da Installation URL) |
| Installation URL | `https://github.com/ngasuka855/TheNewMiner/raw/v0.1.45/thenewminer-0.1.45.tar.gz` |
| Hash algorithm | deixe vazio |
| Wallet and worker template | `%WAL%.%WORKER_NAME%` |
| Pool URL | `tsc-wss.the-new.win` (ignorado: a pool já vem embutida) |
| Pass | a senha da sua pool |
| Extra config arguments | vazio, ou `--proxy <YOUR_PROXY>` (veja a seção Proxy) |

- O nome do worker na pool é o nome do rig no HiveOS: use só letras, dígitos, `_`, `-` ou `.` (sem espaços), até 64
  caracteres. Para usar outro nome de worker, adicione `-w <RIG_NAME>` ao campo Extra config arguments.
- É preciso ter cerca de 12 GB livres no disco do HiveOS para o modelo e o minerador (verifique com `df -h /hive/miners`).
- O modelo (cerca de 8.1 GiB) é baixado uma vez para `/hive/miners/custom/.thenewminer-models` e mantido nas atualizações.
- A carteira e a senha chegam ao minerador pelo ambiente dele, não pela linha de comando.
- Se a pool rejeitar a carteira ou a senha, o log do minerador avisa e o minerador espera 10 minutos antes de o HiveOS
  iniciá-lo de novo: corrija a flight sheet e aplique-a (isso reinicia o minerador na hora).
- Logs: a tela do minerador no HiveOS (`miner` no shell do rig) ou `/var/log/miner/custom/custom.log`.
- **Atualização:** altere os dois números de versão na Installation URL (por exemplo
  `.../raw/v0.1.46/thenewminer-0.1.46.tar.gz`) e aplique a flight sheet. O HiveOS só instala quando o nome do arquivo na
  URL muda.
- **A primeira inicialização demora um pouco:** depois do download do modelo, em placas da série RTX 30, uma verificação
  única da GPU pode estourar o tempo limite em um disco lento do rig, e então o HiveOS reinicia o minerador sozinho,
  duas ou três vezes (cerca de 8 minutos). Não mexa; depois ele minera.
- **Já tem o modelo?** Copie `Qwen3-8B-Q8_0-full-v2.gguf` (cerca de 8.1 GiB, exatamente com este nome de arquivo) para
  `/home/user/` no rig antes da primeira inicialização: o minerador o encontra, verifica o SHA-256 dele e o usa em vez
  de baixá-lo de novo.

**Se o rig não conseguir acessar o GitHub:** baixe `thenewminer-0.1.45.tar.gz` em um PC (confira o SHA-256 dele na
tabela acima), copie-o para o rig e descompacte-o onde o HiveOS guarda os mineradores personalizados. Depois aplique a
flight sheet como de costume: o HiveOS informa "Already installed" e não faz o download. Mantenha o nome do arquivo
exatamente `thenewminer-0.1.45.tar.gz` (um navegador pode acrescentar " (1)") e não altere a Installation URL na flight
sheet: o HiveOS compara esse nome.

```bash
# no PC
scp thenewminer-0.1.45.tar.gz user@<RIG_IP>:/tmp/
# no rig, como root (Hive Shell, ou SSH e depois sudo -i)
mkdir -p /hive/miners/custom/downloads
mv /tmp/thenewminer-0.1.45.tar.gz /hive/miners/custom/downloads/
cd /hive/miners/custom && tar -xzf downloads/thenewminer-0.1.45.tar.gz
```

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
| GPU | NVIDIA GeForce RTX série 30 / 40 / 50, com 10 GB de VRAM ou mais. Rigs com várias GPUs: suportados, cada GPU minera; nesta versão, testado com uma GPU por máquina |
| Driver | NVIDIA **580 ou mais recente** (o `nvidia-smi` precisa funcionar). No Windows, instale no Windows, nunca dentro do WSL |
| Linux | x86_64 com **glibc 2.34 ou mais recente**, por exemplo Ubuntu 22.04 ou mais recente ou Debian 12 ou mais recente. O instalador verifica o que precisa e informa o que estiver faltando |
| Windows | Windows 10 21H2 ou mais recente, ou Windows 11, 64 bits; virtualização (Intel VT-x / AMD SVM) ativada na BIOS |
| Disco | cerca de 20 GB livres |
| RAM | 4 GB ou mais |
| Modelo | cerca de 8.1 GiB, baixado uma vez na primeira inicialização (em paralelo, de vários mirrors) e reutilizado depois |

## Atualize o driver da NVIDIA (580 ou mais recente)

O minerador precisa do driver da NVIDIA **580 ou mais recente**. Veja qual você tem:

```bash
nvidia-smi    # o cabeçalho mostra "Driver Version: ..."; precisa ser 580 ou mais recente
```

- **Ubuntu:** `sudo ubuntu-drivers install` instala o driver recomendado (depois confira se ele é o 580 ou mais recente).
  Para escolher a versão você mesmo:
  `sudo apt install nvidia-driver-580` (ou mais recente; placas da série RTX 50 precisam da variante open, por exemplo
  `nvidia-driver-580-open`). Depois reinicie. Use os pacotes de driver do Ubuntu ou o instalador `.run` da NVIDIA, nunca
  os dois.
- **Debian:** os pacotes de driver do próprio Debian são mais antigos que o 580 (535 no Debian 12, 550 no Debian 13).
  Instale um driver mais recente do
  repositório da NVIDIA, conforme descrito no [NVIDIA Driver Installation Guide](https://docs.nvidia.com/datacenter/tesla/driver-installation-guide/debian.html)
  (guia de instalação do driver da NVIDIA), e depois reinicie.
- **Windows:** atualize pelo [NVIDIA app](https://www.nvidia.com/en-us/software/nvidia-app/) (antigo GeForce
  Experience) ou baixe o driver da sua placa em [nvidia.com](https://www.nvidia.com/en-us/drivers/) (580 ou mais
  recente). O WSL2 usa este driver do Windows: nunca instale um driver NVIDIA para Linux dentro do WSL.
- **HiveOS:** como root (Hive Shell, ou SSH e depois `sudo -i`), execute `nvidia-driver-update --list` para ver as
  versões e depois, por exemplo, `nvidia-driver-update 595` (qualquer uma 580 ou mais recente) e, em seguida, `sreboot`.

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

Testes na pool real em nossos rigs de teste (uma GPU por máquina). A = shares aceitas, R = shares rejeitadas.
"Mineração estável" é a mediana do hashrate entre trocas de job.

| Data | Placa | Sistema operacional | Pacote | Hashrate | A / R |
|---|---|---|---|---|---|
| 2026-10-23 | RTX 4090 | Linux | build final da v0.1.45 | 65.42 N/s (mineração estável, 16 min; 62.35 incluindo trocas de job) | 13 / 0 (0 inválidas) |
| 2026-10-23 | RTX 5090 | Linux | build final da v0.1.45 | 94.18 N/s (mineração estável; 92.24 incluindo trocas de job) | 17 / 0 (0 inválidas) |
| 2026-10-23 | RTX 4090 | Linux | v0.1.44 sealed-r4 | 65.58 N/s (mediana, 16 min) | 10 / 0 |
| 2026-10-23 | RTX 5090 | Linux | v0.1.44 sealed-r4 | 93.45 N/s (mineração estável, 44 min; 90.39 incluindo trocas de job) | 31 / 0 (+1 stale ao parar) |
| 2026-10-23 | RTX 3080 10 GB | Windows (WSL2) | v0.1.44 windows-wsl2-r5 | ~22 N/s (execução do próprio operador) | n/d |
| 2026-10-23 | RTX 3080 10 GB | Linux (Ubuntu 26.04) | v0.1.44 sealed-r4 | 21.04 N/s (últimos 10 min de 23 min de mineração) | 3 / 0 |

RTX 3080 no Linux (v0.1.44): instalação com 6/6 etapas OK, nenhuma janela inválida, parada limpa. Trecho do log:
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

**O minerador diz que a pool rejeitou a carteira / senha.**
Confira o endereço da carteira e a senha da pool (e se a pool já aprovou a sua conta) e inicie novamente:
Linux `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>'`, Windows `TheNewMiner.bat setup`. O minerador não
tenta de novo sozinho com credenciais erradas.

**Está minerando? Onde ficam os logs?**
Linux: `./thenewminer --status`, `./thenewminer --logs`, ou o arquivo `thenewminer/.miner/thenewminer.log`.
Windows: `TheNewMiner.bat status` e `TheNewMiner.bat logs`.

## Nota de segurança

O minerador e o motor de GPU são distribuídos criptografados (ChaCha20-Poly1305) dentro do pacote; o minerador os
descriptografa na sua máquina ao iniciar. Isso aumenta o custo de copiá-los, mas **não é inquebrável**: qualquer pessoa
com acesso de administrador a uma máquina que esteja rodando o minerador pode, com esforço suficiente, recuperar a chave
ou o código descriptografado.
O endereço da sua carteira e a senha da pool ficam armazenados na sua própria máquina e são enviados somente à pool.
