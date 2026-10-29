[English](../WINDOWS-TROUBLESHOOTING.md) · [简体中文](WINDOWS-TROUBLESHOOTING.zh-CN.md) · [繁體中文](WINDOWS-TROUBLESHOOTING.zh-TW.md) · [Русский](WINDOWS-TROUBLESHOOTING.ru.md) · [Español](WINDOWS-TROUBLESHOOTING.es.md) · **Português (Brasil)** · [Français](WINDOWS-TROUBLESHOOTING.fr.md) · [Deutsch](WINDOWS-TROUBLESHOOTING.de.md) · [Türkçe](WINDOWS-TROUBLESHOOTING.tr.md) · [Tiếng Việt](WINDOWS-TROUBLESHOOTING.vi.md) · [한국어](WINDOWS-TROUBLESHOOTING.ko.md) · [日本語](WINDOWS-TROUBLESHOOTING.ja.md)

> Esta tradução é fornecida por conveniência; o README em inglês é a versão de referência. *(This translation is provided for convenience; the English README is authoritative.)*

# Solução de problemas no Windows

No Windows, o TheNewMiner roda o minerador para Linux dentro do WSL2. Quando a instalação para, a janela diz em qual
etapa e o que fazer, e o `TheNewMiner.bat` mostra isso de novo (também fica guardado em
`%LOCALAPPDATA%\TheNewMiner\last-setup-failure.txt`). Esta página explica os casos mais comuns. A instalação e os
comandos do dia a dia estão no [guia](GUIDE.pt-BR.md).

## O DNS não funciona dentro do WSL

Na etapa 4, a instalação verifica se o WSL consegue resolver a pool e o download do modelo. Se não conseguir, a
instalação para ali e não muda nada por conta própria. Abra `%UserProfile%\.wslconfig` no Bloco de Notas (crie o arquivo
se ele não existir) e adicione estas duas linhas (no Windows 11 22H2 ou mais recente, `networkingMode=mirrored` em
`[wsl2]` também funciona):

```ini
[wsl2]
dnsTunneling=true
```

Depois execute `wsl --shutdown` em um Prompt de Comando (isso para todas as distribuições WSL, inclusive a do Docker
Desktop) e clique duas vezes no `TheNewMiner.bat` de novo.

Uma VPN ou um firewall que bloqueia o WSL pode causar a mesma mensagem: libere o WSL ou defina um proxy com
`TheNewMiner.bat setup -Proxy ...` (veja "Proxy" no [guia](GUIDE.pt-BR.md)).

## O Ubuntu não pode ser baixado, ou a virtualização está desativada

- **Rede:** a instalação instala o Ubuntu 24.04 com `wsl --install`. Onde esse download não consegue se conectar (por
  exemplo, onde o GitHub está inacessível), a instalação baixa em vez disso a imagem WSL oficial do próprio Ubuntu de um
  mirror público na China e só a usa se o SHA-256 dela bater. Se nenhum mirror responder, verifique a conexão com a
  internet e tente de novo mais tarde.
- **Virtualização:** uma mensagem sobre virtualização (por exemplo `0x80370102`) significa que ela está desativada.
  Ative o Intel VT-x / AMD SVM na BIOS e depois clique duas vezes no `TheNewMiner.bat` de novo.

## O pacote está em uma unidade do Windows

Se você mesmo descompactou o pacote para Linux e iniciou o `./thenewminer` dentro do WSL a partir de uma unidade do
Windows (como `/mnt/d/...`), o minerador se recusa a iniciar: uma unidade do Windows não consegue manter o arquivo de
configurações privadas dele. Ele mostra uma linha `mkdir -p ~/thenewminer && cp -a ...` pronta para colar, que copia a
pasta para a sua pasta pessoal do Linux e o inicia lá. Ou use o zip para Windows: o `TheNewMiner.bat` coloca tudo no
lugar certo sozinho.

## Lento em uma placa de 10 GB (navegadores, Docker Desktop)

Uma placa de 10 GB só usa o layout rápido quando cerca de 9.3 GB (9472 MiB) de VRAM estão livres no início da mineração.
No Windows, outros programas compartilham a placa: um navegador com aceleração de hardware ou o Docker Desktop pode
ocupar VRAM suficiente para que o minerador volte a um layout mais lento. Em uma RTX 3080 10 GB isso deu cerca de 9 N/s
em vez de 21–22 N/s.

1. Feche os navegadores, ou desative a aceleração de hardware deles (Chrome e Edge: Configurações › Sistema › "Usar
   aceleração de gráficos quando disponível"; Firefox: Configurações › Geral › Desempenho), e saia do Docker Desktop.
2. Confira a VRAM livre em um Prompt de Comando: `nvidia-smi --query-gpu=memory.used,memory.total --format=csv`
   (o total menos o usado deve ser de pelo menos 9472 MiB).
3. Reinicie a mineração: `TheNewMiner.bat stop` e depois clique duas vezes em `TheNewMiner.bat`.

Placas de 12 GB precisam dos mesmos 9472 MiB livres; placas com 16 GB ou mais não são afetadas.

## Erros de relógio

A pool para um minerador cujo relógio esteja cerca de 30 segundos errado. O relógio do WSL2 pode se desviar, por exemplo
depois que o PC entra em suspensão, e a instalação avisa quando ele está errado. Se o minerador informar um erro de
relógio:

1. Abra Configurações do Windows › Hora e idioma › Data e hora, ative "Definir fuso horário automaticamente" e "Definir
   horário automaticamente" e depois clique em **Sincronizar agora**.
2. Em um Prompt de Comando, execute `wsl --shutdown`.
3. Comece a minerar de novo.

## Avisos de segurança do Windows

SmartScreen ("O Windows protegeu o computador") e antivírus que acusam o minerador: veja as perguntas frequentes no
[guia](GUIDE.pt-BR.md).

## Ainda com problemas?

Abra uma issue em [GitHub Issues](https://github.com/ngasuka855/TheNewMiner/issues/new/choose) com a saída de
`TheNewMiner.bat status`, a etapa que falhou e o conteúdo de `last-setup-failure.txt`. Nunca publique a sua senha da pool
nem a chave privada da sua carteira.
