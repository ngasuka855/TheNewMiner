# TheNewMiner

O TheNewMiner é um minerador de GPU para **TensorCash** em placas NVIDIA GeForce RTX das séries 30, 40 e 50 com 10 GB de VRAM ou mais; a pool **`tsc-wss.the-new.win`** já vem embutida.

[English](../../README.md) · [简体中文](README.zh-CN.md) · [繁體中文](README.zh-TW.md) · [Русский](README.ru.md) · [Español](README.es.md) · **Português (Brasil)** · [Français](README.fr.md) · [Deutsch](README.de.md) · [Türkçe](README.tr.md) · [Tiếng Việt](README.vi.md) · [한국어](README.ko.md) · [日本語](README.ja.md)

> Esta tradução é fornecida por conveniência; o README em inglês é a versão de referência. *(This translation is provided for convenience; the English README is authoritative.)*

## Download

Versão atual: **v0.1.47** · Site da pool: [tsc.the-new.win](https://tsc.the-new.win/#/home)

| Sistema | Download | Depois |
|---|---|---|
| Linux x86_64 | [`TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-linux-x86_64-sealed.tar.xz) | ou use a instalação com um clique abaixo |
| Windows 10 / 11 | [`TheNewMiner-v0.1.47-windows-wsl2-r3.zip`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/TheNewMiner-v0.1.47-windows-wsl2-r3.zip) | descompacte e clique duas vezes em `TheNewMiner.bat` |
| HiveOS (experimental) | [`hiveos/flightsheet-thenewminer.json`](../../hiveos/flightsheet-thenewminer.json) | importe a flight sheet (veja HiveOS abaixo) |

Checksums: [`SHA256SUMS`](https://github.com/ngasuka855/TheNewMiner/raw/v0.1.47/SHA256SUMS). Como verificá-los está no
[guia](GUIDE.pt-BR.md). Se um arquivo não bater, não o use.

## Antes de começar: crie a sua conta na pool

Para minerar, você precisa de uma conta na pool: o seu endereço de pagamento TSC (carteira) e uma senha da pool que você
escolhe no site da pool. O site tem uma versão em inglês (use o menu de idioma dele).

1. Abra [tsc.the-new.win](https://tsc.the-new.win/#/home) e vá em **Start mining** (开始挖矿).
2. Em **1. Bind Wallet**, informe o seu **TSC payout address** (`tc1...`; confira com cuidado: a renda de mineração
   enviada para um endereço errado não pode ser recuperada), escolha uma **Pool Password** (6–128 caracteres) e digite-a
   de novo em **Confirm Password**.
3. Clique em **Apply to join pool**. A página confirma que a carteira foi vinculada; a conta funciona na hora.
4. Use a mesma carteira e a mesma senha no minerador: `-u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>'` no Linux, a janela
   de conta do `TheNewMiner.bat` no Windows ou a flight sheet no HiveOS. Cerca de 3–5 minutos depois que ele começa a
   minerar, o seu rig aparece em **My Miners** no site da pool.

A pool não consegue recuperar uma senha da pool esquecida, e o endereço de pagamento não pode ser alterado depois. A senha
da pool não é a chave privada da sua carteira: nunca dê a sua chave privada a ninguém.

## Instalação com um clique (Linux)

Substitua `<YOUR_WALLET>`, `<YOUR_POOL_PASSWORD>` e `<RIG_NAME>` (incluindo os `<` `>`) no final deste comando pela sua
carteira, a sua senha da pool e um nome para este rig, e cole-o em um terminal. Ele baixa o pacote para Linux e o
`SHA256SUMS`, verifica o SHA-256, descompacta o minerador em `~/thenewminer` e o inicia em segundo plano:

```bash
mkdir -p ~/thenewminer-download && cd ~/thenewminer-download && V=v0.1.47 && P=TheNewMiner-$V-linux-x86_64-sealed.tar.xz && U=https://github.com/ngasuka855/TheNewMiner/raw/$V && rm -f "$P" SHA256SUMS && if command -v curl >/dev/null 2>&1; then curl -fL -o "$P" "$U/$P" && curl -fL -o SHA256SUMS "$U/SHA256SUMS"; else wget -O "$P" "$U/$P" && wget -O SHA256SUMS "$U/SHA256SUMS"; fi && sha256sum -c --ignore-missing SHA256SUMS && tar -xJf "$P" -C ~ && cd ~/thenewminer && ./thenewminer -u <YOUR_WALLET> -p '<YOUR_POOL_PASSWORD>' -w <RIG_NAME> --background
```

Um erro de download ou um SHA-256 que não bate nunca inicia nada. Mantenha as aspas simples em volta da senha (uma `'` na
senha é escrita como `'\''`). Em uma máquina com uma única GPU, o minerador verifica a carteira e a senha com a pool antes
de baixar o modelo (cerca de 8.1 GiB, uma única vez). Depois, `./thenewminer --logs` mostra a mineração e
`./thenewminer --status` diz se ele está rodando ou por que parou. Para atualizar, execute primeiro
`~/thenewminer/thenewminer --stop` e depois o mesmo comando: suas configurações e o modelo são mantidos. Todo o resto
(comandos do dia a dia, atualizações, inicialização automática, o que fazer quando ele não minera) está no
[guia](GUIDE.pt-BR.md).

## Windows

1. Descompacte o `TheNewMiner-v0.1.47-windows-wsl2-r3.zip` inteiro (botão direito › Extrair Tudo) em qualquer lugar.
2. Clique duas vezes em **`TheNewMiner.bat`**. Na primeira vez, UMA única janela pede a sua conta da pool (carteira,
   senha da pool, nome do worker); depois disso nada mais é perguntado: ele configura o WSL2 e o Ubuntu, instala o
   minerador incluído, baixa o modelo e começa a minerar. Os cliques duplos seguintes começam a minerar na hora. O
   Windows pode primeiro dizer "O Windows protegeu o computador" (o kit não tem assinatura de código): clique em **Mais
   informações** e depois em **Executar assim mesmo**. Ativar o WSL pede direitos de administrador uma vez e pode exigir
   uma reinicialização; depois a instalação continua sozinha.
3. Uma versão anterior é encontrada e atualizada no lugar, com suas configurações e o modelo.

Se ele parar ou minerar devagar, veja [Solução de problemas no Windows](WINDOWS-TROUBLESHOOTING.pt-BR.md) (DNS, unidades
do Windows, a VRAM que navegadores e o Docker Desktop usam, o relógio).

## HiveOS (experimental)

1. **Wallets:** adicione uma carteira com a moeda `TSC` e o seu endereço TensorCash (`tc1...`).
2. Abra [`hiveos/flightsheet-thenewminer.json`](../../hiveos/flightsheet-thenewminer.json), copie o conteúdo e substitua
   `<YOUR_WALLET>` (mantenha `.%WORKER_NAME%` depois dele) e `<YOUR_POOL_PASSWORD>`. No HiveOS, escolha Flight Sheets ›
   **Import from clipboard**, cole o conteúdo, selecione a sua carteira `TSC` e salve.
3. Aplique a flight sheet ao seu rig. Ela instala o `thenewminer-0.1.47.tar.gz` e começa a minerar.

Configuração manual, atualizações e um rig que não consegue acessar o GitHub: veja o [guia](GUIDE.pt-BR.md).

## Novidades da v0.1.47

- **Instalação mais fácil:** em uma máquina com uma única GPU, o minerador verifica a sua carteira e a senha da pool com
  a pool **antes** do download do modelo de 8.1 GiB, então um erro de digitação é descoberto em segundos. Um endereço de
  carteira com erro de digitação (o checksum dele não confere) ou o endereço de exemplo é recusado, com a explicação do
  que está errado.
- **Windows: credenciais pedidas uma única vez,** em uma janela no início, antes de qualquer etapa demorada. Uma
  instalação feita por uma versão anterior, inclusive em outra distribuição WSL, é encontrada e **atualizada no lugar**
  (configurações e modelo mantidos). Se o Ubuntu não puder ser baixado da Microsoft, a instalação pega a imagem WSL
  oficial do próprio Ubuntu em um mirror na China e só a usa se o SHA-256 dela bater.
- **Sem apt em um Ubuntu padrão:** o Ubuntu 22.04 / 24.04 (incluindo o do WSL) já tem tudo de que o minerador precisa,
  então nada é instalado. Onde algo estiver faltando, a primeira inicialização lista o que falta antes de instalar.
- **Um pacote em uma unidade do Windows** (como `/mnt/d` no WSL) é recusado na hora, com uma linha pronta para colar que
  o copia para a sua pasta pessoal do Linux e o inicia lá.
- **Mensagens de rede claras:** quando o DNS dentro do WSL não funciona, a instalação para nessa etapa e mostra a
  correção; um minerador que não consegue acessar a internet ou a pool diz o que verificar.
- **O modelo é reutilizado:** uma atualização em uma pasta nova encontra o modelo de uma instalação anterior (mesmo
  tamanho e SHA-256) e não o baixa de novo.
- **`--status` diz por que** o minerador parou e como resolver, inclusive depois de uma inicialização com `--background`.
- **HiveOS:** toda falha agora espera e tenta de novo com pausas crescentes (de um a dez minutos) e mostra o motivo no
  log e no painel.

Notas completas: [`docs/releases/RELEASE-NOTES-v0.1.47.md`](../releases/RELEASE-NOTES-v0.1.47.md).

## Problemas conhecidos

- **Placas de 10 GB no Windows precisam de cerca de 9.3 GB de VRAM livre para a velocidade máxima.** Com menos, o
  minerador usa um layout mais lento (uma RTX 3080 10 GB caiu de 21–22 para cerca de 9 N/s). Um navegador com
  aceleração de hardware ou o Docker Desktop pode ocupar essa memória: feche-os (ou desative a aceleração de hardware do
  navegador) e depois comece a minerar de novo.
- **Relógio do Windows:** se o minerador informar um erro de relógio, abra Configurações do Windows › Hora e idioma ›
  Data e hora, ative "Definir fuso horário automaticamente" e "Definir horário automaticamente", clique em **Sincronizar
  agora**, depois execute `wsl --shutdown` e inicie de novo.

## Hashrates

Os melhores hashrates medidos na pool real com nossos próprios rigs. São valores **máximos** ("até"), não uma promessa:
placas do mesmo modelo variam cerca de 5–8%, e o hashrate cai se a placa também alimenta um monitor ou se outros
programas usam a GPU.

| GPU | Hashrate |
|---|---|
| RTX 5090 | até ~99 N/s |
| RTX 4090 | até ~70 N/s |
| RTX 3080 10 GB | até ~22 N/s |

Medições datadas por versão: [resultados dos testes](../TEST-RESULTS.md). Os marcos do projeto:
[registro de desenvolvimento](../DEVELOPMENT-RECORD.md).

## Relatar um problema

Abra uma issue em [GitHub Issues](https://github.com/ngasuka855/TheNewMiner/issues/new/choose) e preencha o formulário de
relatório de bug: a versão do TheNewMiner, o seu sistema, o(s) modelo(s) de GPU e o driver, a saída de
`./thenewminer --status` e as últimas linhas do log. As issues são públicas: **nunca publique a sua senha da pool nem a
chave privada da sua carteira**, e remova a senha (e, se preferir, o endereço da sua carteira) de tudo o que colar.

## Nota de segurança

O minerador e o motor de GPU são distribuídos criptografados (ChaCha20-Poly1305) dentro do pacote; o minerador os
descriptografa na sua máquina ao iniciar. Isso aumenta o custo de copiá-los, mas **não é inquebrável**: qualquer pessoa
com acesso de administrador a uma máquina que esteja rodando o minerador pode, com esforço suficiente, recuperar a chave
ou o código descriptografado.
O endereço da sua carteira e a senha da pool ficam armazenados na sua própria máquina e são enviados somente à pool.
