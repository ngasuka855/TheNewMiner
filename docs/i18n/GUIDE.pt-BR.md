[English](../GUIDE.md) · [简体中文](GUIDE.zh-CN.md) · [繁體中文](GUIDE.zh-TW.md) · [Русский](GUIDE.ru.md) · [Español](GUIDE.es.md) · **Português (Brasil)** · [Français](GUIDE.fr.md) · [Deutsch](GUIDE.de.md) · [Türkçe](GUIDE.tr.md) · [Tiếng Việt](GUIDE.vi.md) · [한국어](GUIDE.ko.md) · [日本語](GUIDE.ja.md)

> Esta tradução é fornecida por conveniência; o README em inglês é a versão de referência. *(This translation is provided for convenience; the English README is authoritative.)*

# Guia do TheNewMiner

Tudo o que vem depois da primeira inicialização: verificar o download, o uso diário no Linux, no Windows e no HiveOS, as
atualizações, o que fazer quando ele não minera e as perguntas frequentes. Os downloads e o início rápido estão na
[página principal](README.pt-BR.md); os problemas do Windows têm a sua própria página:
[Solução de problemas no Windows](WINDOWS-TROUBLESHOOTING.pt-BR.md).

## Verificar o download

| Arquivo | SHA-256 |
|---|---|
| [`TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz) | `86c215dc9401209b8d2a2346252ed5cc1901527b6a51520372e3128583c63656` |
| [`TheNewMiner-v0.1.47-windows-wsl2-r3.zip`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-windows-wsl2-r3.zip) | `45a459646713bef9f8285848486c0a4bd66dee65e8c63090c8d3345c998b2659` |
| [`thenewminer-0.1.47.tar.gz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/thenewminer-0.1.47.tar.gz) (HiveOS) | `6b2515c5bf23f60daa236ff4a8b165ce5e55265d7eecac9dd408896e1e31b784` |

O zip para Windows é tudo em um: ele já contém o minerador para Linux, então você só precisa do zip. Coloque o(s)
pacote(s) e o [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/SHA256SUMS) na mesma pasta e depois:

```bash
# Linux (--ignore-missing: só os pacotes que você baixou são verificados)
sha256sum -c --ignore-missing SHA256SUMS
# macOS
shasum -a 256 -c --ignore-missing SHA256SUMS
```

```bat
:: Windows (Prompt de Comando): compare a saída com o SHA-256 da tabela acima
certutil -hashfile TheNewMiner-v0.1.47-windows-wsl2-r3.zip SHA256
```

Cada arquivo deve mostrar `OK` (no Windows: os mesmos 64 caracteres da tabela). Se não bater, não use o arquivo.

## Linux

```bash
tar -xJf TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz
cd thenewminer
./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background
```

| Comando | O que faz |
|---|---|
| `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w rig-01 --background` | Primeira inicialização: instala o que for necessário e depois minera em segundo plano |
| `./thenewminer --background` | Inicializações seguintes: suas configurações ficam salvas |
| `./thenewminer` | Minerar em primeiro plano (Ctrl+C para) |
| `./thenewminer --logs` | Acompanhar o minerador (Ctrl+C sai da visualização; a mineração continua) |
| `./thenewminer --status` | Se está minerando, o último hashrate e as shares aceitas / rejeitadas; depois de uma parada, por que parou e como resolver |
| `./thenewminer --stop` | Parar a mineração |
| `./thenewminer --help` | Todas as opções |

- `-u` é o endereço da sua carteira TensorCash, `-p` a senha da pool e `-w` o nome do worker exibido na pool
  (padrão: o nome de host da máquina; de 1 a 64 letras, dígitos, `_`, `-` ou `.`).
- O endereço da carteira é verificado antes de qualquer coisa começar: um endereço com erro de digitação (o checksum dele
  não confere) ou o endereço de exemplo é recusado, com a explicação do que está errado.
- A senha da pool: de 6 a 128 caracteres, qualquer caractere visível, sem espaços. Coloque-a sempre entre aspas simples
  na linha de comando, por exemplo `-p 'my$pass!'` (uma `'` dentro de aspas simples é escrita como `'\''`).
- As configurações ficam salvas em `thenewminer/.miner/` (o arquivo com a senha só pode ser lido por você). A carteira
  e a senha são retiradas da linha de comando na hora, então não aparecem no `ps` (o seu shell ainda pode guardar o
  comando no histórico).
- Na primeira inicialização de uma máquina com uma única GPU, o minerador verifica a carteira e a senha com a pool
  **antes** do download do modelo, então um erro de digitação é descoberto em segundos (um rig com várias GPUs o
  descobre quando a mineração começa). Se a pool rejeitar os dados, o minerador avisa e para: confira-os e inicie
  novamente com `-u` e `-p`.
- A primeira inicialização baixa o modelo (cerca de 8.1 GiB, uma única vez), instala o motor de GPU para a sua placa e
  executa um autoteste na placa; depois começa a minerar. Linhas como `[ RATE] ... N/s` indicam que ele está minerando.
  O log é `thenewminer/.miner/thenewminer.log`.
- Um Ubuntu 22.04 / 24.04 padrão (incluindo o do WSL) já tem todas as bibliotecas de que o minerador precisa, então nada
  é instalado. Onde alguma estiver faltando (imagens Docker, sistemas mínimos), a primeira inicialização no Ubuntu, no
  Debian e no HiveOS a lista e a instala com `apt` (requer sudo ou root); em outros sistemas, ela informa o nome para você
  instalar.
- Rigs com várias GPUs: suportados, cada GPU minera; esta versão foi testada com uma GPU por máquina.
- No Docker ou em outros contêineres com pouca memória compartilhada, o minerador executa seus arquivos de programa a
  partir de `thenewminer/.miner/run/` no disco e avisa. Para escolher outra pasta, inicie-o com
  `TNPSEAL_RUNTIME_BASE=<folder> ./thenewminer` (uma pasta em que só você possa gravar: `chmod 700 <folder>`).
- **Atualização:** `./thenewminer --stop`, descompacte o novo pacote no mesmo lugar (ele é descompactado em
  `thenewminer/` e mantém `thenewminer/.miner/` com suas configurações e o modelo) e inicie novamente. Se a versão
  antiga ainda estiver rodando, a nova se recusa a iniciar e pede que você pare a antiga primeiro. Se, em vez disso, for
  descompactado em uma pasta nova, ele encontra o modelo da versão antiga (mesmo tamanho e SHA-256) e o reutiliza: nada
  de um segundo download de 8.1 GiB. Informe `-u -p -w` de novo nessa pasta.

### Quando ele não minera

`./thenewminer --status` diz por que o minerador parou e como resolver. As mensagens mais comuns:

| Você vê | O que fazer |
|---|---|
| `bash: ./thenewminer: Permission denied` | A pasta não permite executar programas (uma montagem `noexec`, como alguns `/tmp`, ou um pendrive). Descompacte-o na sua pasta pessoal: `cd ~ && tar -xJf <the package>.tar.xz && cd thenewminer` |
| `this package is on a FAT (vfat) drive` (ou exFAT / NTFS, ou `Windows (DrvFs, 9p)`: uma unidade do Windows como `/mnt/d` no WSL) | Uma unidade assim não consegue manter as configurações privadas do minerador (modo 600). Cole a linha `mkdir -p ~/thenewminer && cp -a ...` que ele mostra: ela copia a pasta para a sua pasta pessoal do Linux e o inicia lá |
| `this system has no xz` | `sudo apt-get install -y xz-utils` (como root: sem `sudo`) e depois inicie de novo |
| `This system is missing what TheNewMiner needs` | Execute a linha `sudo apt-get install -y ...` que ele mostra e depois inicie de novo |
| `No NVIDIA GPU found on this machine` | Nenhuma placa NVIDIA foi detectada. Verifique se a placa está bem encaixada e alimentada; em uma máquina na nuvem, escolha uma com GPU NVIDIA |
| `NVIDIA driver ... is too old` | Instale o driver indicado (580 ou mais recente), reinicie e inicie de novo (veja "Atualize o driver da NVIDIA" abaixo) |
| `the pool rejected the wallet / password` | Confira `-u` e `-p` e se a conta existe no site da pool, depois `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' --background` |
| `no download source answered` | O download do modelo não consegue acessar a internet. Atrás de um proxy: `https_proxy=http://<HOST>:<PORT> ./thenewminer --background`; ou copie o modelo (veja "Já tem o modelo?" abaixo) |
| `cannot reach the pool` / `pool disconnected` | Verifique a conexão com a internet, o DNS e se o firewall permite TCP 443 de saída; se a sua rede bloqueia a pool, use `--proxy` (veja "Proxy" abaixo). Ele se reconecta sozinho |

### Iniciar depois de reiniciar (opcional)

O minerador não inicia sozinho depois de uma reinicialização (o HiveOS faz isso por você). No Linux, adicione uma linha ao
`crontab`:

```bash
crontab -e      # depois adicione esta linha (com a sua pasta):
@reboot cd /home/<YOU>/thenewminer && ./thenewminer --background
```

ou crie um serviço de usuário do systemd (`loginctl enable-linger <YOU>` uma vez, para que ele inicie sem login):

```ini
# ~/.config/systemd/user/thenewminer.service, depois: systemctl --user enable --now thenewminer
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

O código de saída 77 significa que a pool rejeitou a carteira / senha: reiniciar não adianta, então o serviço para aí.

## Windows

1. Descompacte o `TheNewMiner-v0.1.47-windows-wsl2-r3.zip` inteiro (botão direito › Extrair Tudo) em qualquer lugar:
   qualquer pasta, qualquer unidade; caminhos com espaços ou caracteres chineses não são problema. Não execute de dentro
   da visualização do zip.
2. Clique duas vezes em **`TheNewMiner.bat`**.
   - **Na primeira vez:** UMA única janela pede primeiro a sua conta da pool: o endereço da carteira (começa com `tc1`;
     o checksum dele é verificado, e o exemplo mostrado não é uma carteira real), a senha da pool (de 6 a 128
     caracteres, qualquer caractere visível, sem espaços) e o nome do worker (o nome do computador já vem preenchido).
     O botão OK fica cinza até que os três dados estejam corretos. Depois de OK e Sim, nada mais é perguntado: ele
     verifica o Windows, o driver e o disco, ativa o WSL2 se necessário (um aviso de administrador; pode ser preciso
     reiniciar uma vez, e a instalação continua sozinha depois), mostra onde os dados vão ficar, cria seu próprio
     ambiente Linux (WSL), instala o minerador incluído, baixa o modelo e começa a minerar. Sem área de trabalho (via
     SSH), as mesmas perguntas aparecem no console.
   - **Nas vezes seguintes:** clique duas vezes e ele começa a minerar na hora com as configurações salvas. Também é
     criado um atalho "TheNewMiner" na área de trabalho. Clicar duas vezes de novo enquanto ele já está minerando
     avisa isso e mostra o log.
   - **Se a instalação parar,** a janela diz em qual etapa e o que fazer, e o `TheNewMiner.bat` mostra isso de novo.
     Veja [Solução de problemas no Windows](WINDOWS-TROUBLESHOOTING.pt-BR.md).

Comandos do dia a dia (abra um Prompt de Comando na pasta descompactada):

| Comando | O que faz |
|---|---|
| `TheNewMiner.bat` | Instala se necessário e depois minera nesta janela (fechar a janela para a mineração) |
| `TheNewMiner.bat background` | Minerar sem janela |
| `TheNewMiner.bat logs` | Acompanhar o minerador (Ctrl+C sai da visualização; a mineração continua) |
| `TheNewMiner.bat status` | Se está rodando e os últimos hashrates |
| `TheNewMiner.bat stop` | Parar a mineração |
| `TheNewMiner.bat setup` | Informar de novo a carteira / senha / nome do worker (Cancel mantém os dados atuais) |
| `TheNewMiner.bat uninstall` | Remover o minerador, sua pasta de dados e o atalho (pergunta antes) |

- **Onde ficam os dados:** o minerador e o modelo ficam em uma pasta de dados escolhida na primeira instalação (por
  padrão `TheNewMinerData` na raiz da unidade onde você descompactou, por exemplo `D:\TheNewMinerData`), e não na pasta
  descompactada. É preciso ter cerca de 20 GB livres nela. Para escolher outra pasta:
  `TheNewMiner.bat setup -DataDir <folder>`.
- **Ubuntu:** a instalação instala o Ubuntu 24.04 com `wsl --install`. Quando esse download não consegue se conectar,
  ela baixa a imagem WSL oficial do próprio Ubuntu, `ubuntu-24.04.5-wsl-amd64.wsl`, de um mirror público na China
  (Tsinghua TUNA, depois USTC, depois Aliyun) e só a usa se o SHA-256 dela for exatamente
  `bb415d824822c4b878125729af451a5d18fb13d1cf5cbed9a7393ad64ac6039e`.
- Se a pool rejeitar a carteira ou a senha, a janela avisa e a mineração para: execute `TheNewMiner.bat setup` para
  informá-las de novo.
- **Atualização:** descompacte a nova versão em qualquer lugar e clique duas vezes no `TheNewMiner.bat` dela. Ele
  encontra o minerador instalado, mantém suas configurações e o modelo e instala a nova versão sem fazer perguntas. Uma
  instalação feita por uma versão anterior ou à mão dentro de outra distribuição WSL (por exemplo `Ubuntu-24.04`) é
  encontrada e atualizada onde está: nenhuma segunda instalação, nenhum Ubuntu novo, nenhum segundo download do modelo.
  Se ela não puder ser atualizada ali, a instalação para e diz por quê. A pasta antiga pode ser apagada.
- **Desinstalação:** `TheNewMiner.bat uninstall`. Ele nunca remove uma distribuição WSL que seja sua.
- O kit para Windows está em inglês.

## HiveOS (experimental)

O suporte a HiveOS é **experimental** (desde a v0.1.45, que rodou em um rig HiveOS real; as mudanças feitas desde a
v0.1.46 ainda não rodaram em um). Ele usa o pacote de minerador personalizado (custom miner) para HiveOS
`thenewminer-0.1.47.tar.gz` (o SHA-256 dele está na tabela em "Verificar o download").

Configuração rápida (importar a flight sheet):

1. **Wallets:** adicione uma carteira com a moeda `TSC` e o seu endereço TensorCash (`tc1...`). Isso só é necessário uma
   vez: o HiveOS precisa de uma carteira `TSC` selecionada, e o minerador usa o endereço que você colocar na flight
   sheet.
2. Abra [`hiveos/flightsheet-thenewminer.json`](../../hiveos/flightsheet-thenewminer.json) e copie o conteúdo. Substitua
   `<YOUR_WALLET>` pelo endereço da sua carteira (mantenha `.%WORKER_NAME%` depois dele) e `<YOUR_POOL_PASSWORD>` pela
   senha da sua pool (no JSON, uma `"` na senha é escrita como `\"` e uma `\` é escrita como `\\`). No HiveOS, escolha
   Flight Sheets › **Import from clipboard**, cole o conteúdo, selecione a sua carteira `TSC` e salve.
3. Aplique a flight sheet ao seu rig. O rig baixa o pacote e começa a minerar.

Configuração manual, como alternativa: crie uma flight sheet com Coin `TSC`, Wallet: a sua carteira `TSC`, Pool:
"Configure in miner", Miner: "Custom", clique em **Setup Miner Config** e preencha:

| Campo | Valor |
|---|---|
| Miner name | `thenewminer` (preenchido a partir da Installation URL) |
| Installation URL | `https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/thenewminer-0.1.47.tar.gz` |
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
- **Quando algo falha** (por exemplo, a pool rejeita a carteira ou a senha), o log do minerador e o painel do HiveOS
  mostram o motivo, e o rig tenta de novo com pausas crescentes (de um a dez minutos). Corrija a flight sheet e
  aplique-a: isso reinicia o minerador na hora.
- Logs: a tela do minerador no HiveOS (`miner` no shell do rig) ou `/var/log/miner/custom/custom.log`.
- **Atualização:** altere os dois números de versão na Installation URL (por exemplo
  `.../raw/vX.Y.Z/thenewminer-X.Y.Z.tar.gz`) e aplique a flight sheet. O HiveOS só instala quando o nome do arquivo na
  URL muda.
- **A primeira inicialização** baixa o modelo e instala o motor de GPU, o que leva alguns minutos.
- **Já tem o modelo?** Copie-o para `/home/user/` no rig antes da primeira inicialização (veja a próxima seção).

**Se o rig não conseguir acessar o GitHub:** baixe `thenewminer-0.1.47.tar.gz` em um PC (confira o SHA-256 dele),
copie-o para o rig e descompacte-o onde o HiveOS guarda os mineradores personalizados. Depois aplique a flight sheet como
de costume: o HiveOS informa "Already installed" e não faz o download. Mantenha o nome do arquivo exatamente
`thenewminer-0.1.47.tar.gz` (um navegador pode acrescentar " (1)") e não altere a Installation URL na flight sheet: o
HiveOS compara esse nome.

```bash
# no PC
scp thenewminer-0.1.47.tar.gz user@<RIG_IP>:/tmp/
# no rig, como root (Hive Shell, ou SSH e depois sudo -i)
mkdir -p /hive/miners/custom/downloads
mv /tmp/thenewminer-0.1.47.tar.gz /hive/miners/custom/downloads/
cd /hive/miners/custom && tar -xzf downloads/thenewminer-0.1.47.tar.gz
```

## Já tem o modelo? Copie-o

O minerador precisa de um único arquivo de modelo de 8.1 GiB: `Qwen3-8B-Q8_0-full-v2.gguf` (`8709518112` bytes, SHA-256
`408b955510e196121c1c375201744783b5c9a43c7956d73fc78df54c66e883d6`). Se ele já estiver nesta máquina, ou em outro
computador da sua rede, copie-o em vez de baixá-lo de novo.

Antes de baixar qualquer coisa, o minerador procura o arquivo pelo nome e pelo tamanho exatos: na própria pasta de
modelos, em pastas anteriores do TheNewMiner, na sua pasta pessoal (e uma ou duas pastas abaixo dela, ou em uma pasta
`models` ali), nas pastas pessoais dentro de `/home`, em `/data`, `/mnt/*`, `/media/*`, `/opt`, `/srv`,
`/workspace`, `/var/lib` e nos caches do Hugging Face / ModelScope. Se nada for encontrado ali, ele passa até 20
segundos procurando nessas pastas do mesmo disco (os outros discos montados são verificados só pelo nome) qualquer
`.gguf` com exatamente esse tamanho. Um arquivo encontrado é usado **somente se o SHA-256 dele bater**; então o
minerador cria um link para ele (mesmo disco) ou o copia para a própria pasta de modelos, para que todas as versões
seguintes o encontrem na hora. Você também pode indicá-lo diretamente:
`./thenewminer --model /path/to/Qwen3-8B-Q8_0-full-v2.gguf`.

**Linux:** copie-o para a sua pasta pessoal e depois inicie o minerador normalmente. De um Mac ou PC na mesma rede (o
Windows 10 e 11 têm `scp` no PowerShell), substitua `user` e `<MINER_IP>` pelo login e pelo endereço do seu minerador:

```bash
# no próprio minerador
cp /path/to/Qwen3-8B-Q8_0-full-v2.gguf ~/
# ou de outro computador
scp Qwen3-8B-Q8_0-full-v2.gguf user@<MINER_IP>:~/
```

**HiveOS:** copie-o para `/home/user/` no rig (o login SSH do HiveOS é `user`) e depois aplique a flight sheet. O
minerador o encontra ali, verifica e o guarda ao lado do pacote (`/hive/miners/custom/.thenewminer-models`), então uma
nova versão nunca o baixa de novo. O disco do HiveOS é pequeno: rode `disk-expand` antes se `df -h /hive/miners` mostrar
menos de 12 GB livres.

```bash
scp Qwen3-8B-Q8_0-full-v2.gguf user@<RIG_IP>:/home/user/
```

**Windows:** depois que o primeiro `setup` tiver criado a distribuição WSL do TheNewMiner, copie o arquivo para ela a
partir de um Prompt de Comando (troque `D:\Models` pelo local da sua cópia). Se o download já tiver começado, feche a
janela, execute estes comandos e inicie o TheNewMiner de novo: ele encontra a cópia e pula o download. A cópia leva
alguns minutos e não mostra progresso.

```bat
wsl -d TheNewMiner -u root -- mkdir -p ~/thenewminer/models
wsl -d TheNewMiner -u root -- cp /mnt/d/Models/Qwen3-8B-Q8_0-full-v2.gguf ~/thenewminer/models/
```

## Trocar a carteira, a senha da pool ou o nome do worker

A pool vincula de forma permanente cada endereço de pagamento à sua própria conta na pool: a página da pool
[tsc.the-new.win](https://tsc.the-new.win) descreve o endereço de pagamento de TSC como "永久绑定 · 不可修改" (vinculado
permanentemente, não pode ser modificado) e orienta que, para trocar, você use o novo endereço diretamente no minerador.
Por isso, uma carteira diferente precisa da sua própria conta na pool e da sua própria senha da pool (crie-as primeiro
na página da pool). Se você trocar a senha da pool na página da pool, atualize-a em todos os mineradores. A pool não
consegue recuperar uma senha esquecida.

**Linux:** pare o minerador e depois inicie-o uma vez com as novas configurações; elas substituem as que estavam salvas:

```bash
./thenewminer --stop
./thenewminer -u <NEW_WALLET> -p '<PASSWORD>' -w <RIG_NAME> --background
```

Informe `-u` e `-p` juntos: apenas um deles não altera a carteira e a senha salvas. O nome do worker pode ser trocado
sozinho: `./thenewminer -w <RIG_NAME> --background` (depois de `--stop`).

**Windows:** execute `TheNewMiner.bat setup`. Ele abre a janela da conta com o nome do worker já preenchido; informe a
carteira e a senha da pool e depois inicie-o como de costume (clique duas vezes em `TheNewMiner.bat`).

**HiveOS:** edite a flight sheet: coloque o novo endereço da carteira em "Wallet and worker template"
(`<NEW_WALLET>.%WORKER_NAME%`) e a nova senha da pool em Pass, depois salve e aplique-a. O nome do worker é o nome do
rig no HiveOS (ou `-w <RIG_NAME>` em Extra config arguments).

## Proxy (opcional)

O minerador se conecta diretamente à pool. Se a sua rede precisar de um proxy:

```bash
# Linux (fica salvo até ser alterado)
./thenewminer --proxy <IP>[:<PORT>]            # relay TCP (porta 443 se omitida)
./thenewminer --proxy socks5://<HOST>:<PORT>   # SOCKS5, também socks5://<USER>:<PASS>@<HOST>:<PORT>
./thenewminer --proxy http://<HOST>:<PORT>     # proxy HTTP CONNECT
./thenewminer --proxy none                     # volta para a conexão direta
```

Esses comandos também iniciam o minerador: pare o minerador primeiro e adicione `--background` para mantê-lo em segundo
plano.

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
  Instale um driver mais recente do repositório da NVIDIA, conforme descrito no
  [NVIDIA Driver Installation Guide](https://docs.nvidia.com/datacenter/tesla/driver-installation-guide/debian.html)
  (guia de instalação do driver da NVIDIA), e depois reinicie.
- **Windows:** atualize pelo [NVIDIA app](https://www.nvidia.com/en-us/software/nvidia-app/) (antigo GeForce
  Experience) ou baixe o driver da sua placa em [nvidia.com](https://www.nvidia.com/en-us/drivers/) (580 ou mais
  recente). O WSL2 usa este driver do Windows: nunca instale um driver NVIDIA para Linux dentro do WSL.
- **HiveOS:** como root (Hive Shell, ou SSH e depois `sudo -i`), execute `nvidia-driver-update --list` para ver as
  versões e depois, por exemplo, `nvidia-driver-update 595` (qualquer uma 580 ou mais recente) e, em seguida, `sreboot`.

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
Confira o endereço da carteira e a senha da pool (a conta precisa existir no site da pool) e inicie novamente:
Linux `./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>'`, Windows `TheNewMiner.bat setup`. O minerador não
tenta de novo sozinho com credenciais erradas.

**Está minerando? Onde ficam os logs?**
Linux: `./thenewminer --status`, `./thenewminer --logs`, ou o arquivo `thenewminer/.miner/thenewminer.log`.
Windows: `TheNewMiner.bat status` e `TheNewMiner.bat logs`.

**A minha RTX 3080 (10 GB) minera bem mais devagar no Windows do que no Linux.**
No Windows, outros programas compartilham a VRAM. Veja [Solução de problemas no Windows](WINDOWS-TROUBLESHOOTING.pt-BR.md):
feche os navegadores com aceleração de hardware e o Docker Desktop.
