# 📖 Guia do Iniciante — Revolution X Vault

> Este guia foi feito para quem **nunca mexeu** com backup por linha de comando, repositórios de dados ou programação. Vamos do zero até você ter seus arquivos protegidos na nuvem.

---

## 📋 Índice

1. [O que é o Revolution X Vault?](#1-o-que-é-o-revolution-x-vault)
2. [O que você precisa ter instalado](#2-o-que-você-precisa-ter-instalado)
3. [Instalando o Go (linguagem de programação)](#3-instalando-o-go)
4. [Instalando o Node.js (para a interface web)](#4-instalando-o-nodejs)
5. [Baixando o projeto](#5-baixando-o-projeto)
6. [Compilando a interface web (htmlui)](#6-compilando-a-interface-web)
7. [Compilando o executável principal](#7-compilando-o-executável-principal)
8. [Usando pela primeira vez](#8-usando-pela-primeira-vez)
9. [Conectando ao Cloudflare R2](#9-conectando-ao-cloudflare-r2)
10. [Fazendo seu primeiro backup](#10-fazendo-seu-primeiro-backup)
11. [Restaurando um arquivo](#11-restaurando-um-arquivo)
12. [Configurando políticas de retenção](#12-configurando-políticas-de-retenção)
13. [Usando a interface web (modo servidor)](#13-usando-a-interface-web)
14. [Glossário — palavras que você vai encontrar](#14-glossário)
15. [Problemas comuns e soluções](#15-problemas-comuns)

---

## 1. O que é o Revolution X Vault?

Imagine que você tira uma **foto** de todos os seus arquivos importantes. Essa foto (chamada de **snapshot**) fica salva em um cofre seguro na nuvem. Se algum dia você perder um arquivo, basta abrir o cofre e pegar a versão que quiser.

O Revolution X Vault faz exatamente isso, com algumas vantagens:

- ✅ **Só envia o que mudou** — se você tem 10GB de arquivos e mudou apenas 1 documento, ele envia só esse documento (isso se chama *deduplicação*)
- ✅ **Criptografa tudo** — nem quem hospeda o servidor consegue ver seus arquivos
- ✅ **Mantém várias versões** — alterou um arquivo por engano? Volte para a versão de ontem, da semana passada ou do mês passado
- ✅ **Funciona com vários destinos** — disco externo, Cloudflare R2, Amazon S3, Google Cloud, Backblaze B2, e mais

---

## 2. O que você precisa ter instalado

Antes de começar, seu computador precisa de:

| Programa | Para que serve | Onde baixar |
|:---|:---|:---|
| **Git** | Baixar o código do projeto | [git-scm.com/downloads](https://git-scm.com/downloads) |
| **Go 1.21+** | Compilar o programa principal | [go.dev/dl](https://go.dev/dl/) |
| **Node.js 18+** | Compilar a interface web | [nodejs.org](https://nodejs.org/) |

> 💡 **Dica:** Se você está no Windows, baixe os instaladores `.msi` (Go) e `.msi` (Node.js) — basta clicar "Avançar" em tudo.

---

## 3. Instalando o Go

### Windows
1. Acesse [go.dev/dl](https://go.dev/dl/)
2. Baixe o arquivo `go1.xx.x.windows-amd64.msi`
3. Execute o instalador (Avançar → Avançar → Instalar)
4. Abra o **Prompt de Comando** (ou PowerShell) e digite:
   ```
   go version
   ```
   Se aparecer algo como `go version go1.23.0 windows/amd64`, está tudo certo!

### macOS
```bash
brew install go
```

### Linux (Ubuntu/Debian)
```bash
sudo apt update && sudo apt install -y golang-go
```

---

## 4. Instalando o Node.js

### Windows
1. Acesse [nodejs.org](https://nodejs.org/)
2. Baixe a versão **LTS** (a mais estável)
3. Execute o instalador
4. Verifique no terminal:
   ```
   node --version
   npm --version
   ```

### macOS
```bash
brew install node
```

### Linux (Ubuntu/Debian)
```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs
```

---

## 5. Baixando o projeto

Abra o terminal e digite:

```bash
git clone https://github.com/wendellrox/revolution-x-vault.git
cd revolution-x-vault
```

Pronto! Agora você tem todos os arquivos do projeto na pasta `revolution-x-vault`.

---

## 6. Compilando a interface web

A interface visual (aquela com botões, tabelas, etc.) precisa ser compilada antes:

```bash
cd htmlui
npm install
npm run build
cd ..
```

> ⏳ O `npm install` vai baixar as dependências — pode levar uns minutos na primeira vez.
>
> Se der erro de permissão no Windows, abra o PowerShell **como Administrador**.

---

## 7. Compilando o executável principal

Volte para a pasta raiz do projeto e compile:

```bash
go build -o rxvault.exe .
```

No Linux/macOS:
```bash
go build -o rxvault .
```

> Isso cria o arquivo executável `rxvault` (ou `rxvault.exe` no Windows) que você pode rodar diretamente.

---

## 8. Usando pela primeira vez

### 8.1. Criando um repositório local (para testar)

Um **repositório** é o "cofre" onde seus backups ficam guardados. Para criar um no seu próprio computador:

```bash
rxvault repository create filesystem --path C:\MeuBackup
```

No Linux/macOS:
```bash
./rxvault repository create filesystem --path ~/MeuBackup
```

O programa vai pedir uma **senha**. Essa senha é usada para criptografar seus dados — **não perca ela**, pois sem ela não há como recuperar os backups.

### 8.2. Conectando a um repositório existente

Se o repositório já existe:
```bash
rxvault repository connect filesystem --path C:\MeuBackup
```

---

## 9. Conectando ao Cloudflare R2

O Cloudflare R2 é um serviço de armazenamento na nuvem **sem taxa de saída** (você não paga para baixar seus dados). Perfeito para backups.

### 9.1. Criando a conta no Cloudflare R2

1. Acesse [dash.cloudflare.com](https://dash.cloudflare.com)
2. Faça login ou crie uma conta
3. No menu lateral, clique em **R2 Object Storage**
4. Clique em **Create Bucket** e dê o nome `revolution-x-vault-backups`
5. Anote o **Account ID** (aparece no canto direito da tela)

### 9.2. Gerando a chave API

1. No painel R2, clique em **Manage R2 API Tokens**
2. Clique em **Create API Token**
3. Selecione **Object Read & Write** para o bucket criado
4. Clique em **Create API Token**
5. **Copie e salve** o `Access Key ID` e o `Secret Access Key` — eles só aparecem uma vez!

### 9.3. Conectando o Revolution X Vault ao R2

```bash
rxvault repository create s3 \
  --bucket revolution-x-vault-backups \
  --access-key SEU_ACCESS_KEY_ID \
  --secret-access-key SUA_SECRET_KEY \
  --endpoint SEU_ACCOUNT_ID.r2.cloudflarestorage.com
```

No Windows (uma linha só):
```
rxvault repository create s3 --bucket revolution-x-vault-backups --access-key SEU_ACCESS_KEY_ID --secret-access-key SUA_SECRET_KEY --endpoint SEU_ACCOUNT_ID.r2.cloudflarestorage.com
```

> Troque `SEU_ACCESS_KEY_ID`, `SUA_SECRET_KEY` e `SEU_ACCOUNT_ID` pelos valores que você copiou no passo anterior.

---

## 10. Fazendo seu primeiro backup

Agora que o repositório está conectado, basta mandar fazer o backup de uma pasta:

```bash
rxvault snapshot create C:\Users\SeuNome\Documentos
```

No Linux/macOS:
```bash
./rxvault snapshot create ~/Documentos
```

> 🎉 **Pronto!** Seus documentos estão protegidos. O primeiro backup pode demorar mais, pois ele precisa enviar tudo. Os próximos são muito mais rápidos (enviam só o que mudou).

### Verificando os backups existentes

```bash
rxvault snapshot list
```

Vai mostrar algo como:
```
damaziobr@meu-pc:C:\Users\damaziobr\Documentos
  2026-11-25 17:30:00 BRT  d7f8a2b3c4...  (1.2 GB)
```

---

## 11. Restaurando um arquivo

### Restaurar uma pasta inteira
```bash
rxvault snapshot restore HASH_DO_SNAPSHOT C:\PastaDeDestino
```

> O `HASH_DO_SNAPSHOT` é aquele código (tipo `d7f8a2b3c4...`) que aparece no `snapshot list`.

### Navegar dentro de um snapshot (explorar antes de restaurar)
```bash
rxvault snapshot list --all
rxvault mount HASH_DO_SNAPSHOT X:
```

Isso "monta" o backup como se fosse uma pasta/disco, e você pode copiar só os arquivos que precisa.

---

## 12. Configurando políticas de retenção

As **políticas** definem quantas versões dos backups manter. Exemplo:

```bash
rxvault policy set --global \
  --keep-latest 10 \
  --keep-hourly 24 \
  --keep-daily 30 \
  --keep-weekly 8 \
  --keep-monthly 12 \
  --keep-annual 3
```

Isso significa:
- Manter as **10 últimas** versões sempre
- Manter **1 por hora** nas últimas 24 horas
- Manter **1 por dia** nos últimos 30 dias
- Manter **1 por semana** nas últimas 8 semanas
- Manter **1 por mês** nos últimos 12 meses
- Manter **1 por ano** nos últimos 3 anos

---

## 13. Usando a interface web

O Revolution X Vault tem uma **interface web visual** (aquela com os botões coloridos). Para usá-la:

```bash
rxvault server start --insecure --address 0.0.0.0:51515
```

Depois abra no navegador: **http://localhost:51515**

Você verá a interface com:
- **Navbar** com o escudo 🛡️ e "Revolution X Vault by RX-Corp"
- **Snapshots** — ver e criar backups
- **Policies** — configurar retenção
- **Tasks** — acompanhar tarefas em andamento
- **Repository** — gerenciar a conexão com o cofre

> Os botões usam **cores semânticas**: verde para confirmar, azul para navegar/restaurar, amarelo para atenção, vermelho para excluir.

---

## 14. Glossário

| Termo | O que significa |
|:---|:---|
| **Snapshot** | Uma "foto" dos seus arquivos em determinado momento |
| **Repositório** | O "cofre" onde os backups são armazenados (disco, nuvem, etc.) |
| **Deduplicação** | Técnica que evita enviar dados repetidos — só envia o que mudou |
| **Criptografia** | Processo que "embaralha" seus dados para que ninguém leia sem a senha |
| **Retenção** | Regra de quantas versões antigas dos backups manter |
| **Políticas** | Conjunto de regras aplicadas aos backups (retenção, compressão, exclusões) |
| **Bucket** | "Balde" — nome usado para contêineres de armazenamento na nuvem (R2, S3) |
| **Endpoint** | Endereço do servidor de armazenamento na nuvem |
| **CLI** | Interface de Linha de Comando — quando você digita comandos no terminal |
| **UI / Interface Web** | Interface gráfica visual acessível pelo navegador |
| **Cloudflare R2** | Serviço de armazenamento de objetos do Cloudflare, compatível com S3, sem taxa de saída |

---

## 15. Problemas comuns

### ❌ "go: command not found"
O Go não foi instalado corretamente ou não está no PATH. Reinstale o Go e reinicie o terminal.

### ❌ "npm: command not found"
O Node.js não foi instalado. Baixe em [nodejs.org](https://nodejs.org/) e reinstale.

### ❌ "repository not connected"
Você precisa criar ou conectar um repositório antes de fazer backup. Veja a [seção 8](#8-usando-pela-primeira-vez).

### ❌ "access denied" ao conectar ao R2
Verifique se o Access Key e Secret Key estão corretos e se o token tem permissão de leitura e escrita no bucket.

### ❌ "password is required"
O repositório é protegido por senha. Use a mesma senha que você definiu na criação. Sem ela, não há como acessar os dados.

### ❌ Interface web não abre
Certifique-se de que o servidor está rodando (`rxvault server start`) e acesse `http://localhost:51515` (não https).

---

## 🤝 Créditos

- **Motor de backup:** [Kopia](https://kopia.io) — projeto open-source licenciado sob Apache 2.0
- **Customização e branding:** RX-Corp
- **Repositório:** [github.com/wendellrox/revolution-x-vault](https://github.com/wendellrox/revolution-x-vault)

---

> Feito com 🛡️ por **RX-Corp** — Protegendo seus dados com simplicidade.
