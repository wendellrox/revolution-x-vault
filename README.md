# Revolution X Vault
### Backup Seguro, Rápido e Versionado — por RX-Corp

> **Baseado no [Kopia](https://github.com/kopia/kopia)** — motor open-source de backup rápido, seguro, criptografado e com deduplicação. Customizado visualmente pela **RX-Corp** com identidade própria, tema claro e interface intuitiva por cores semânticas.

---

## ✨ O que é o Revolution X Vault?

O **Revolution X Vault** é um sistema completo de backup que permite:

- 🔒 **Criptografia ponta-a-ponta** (AES-256-GCM) — seus dados ficam protegidos antes mesmo de saírem do seu computador
- 🚀 **Deduplicação inteligente** — só envia o que mudou, economizando espaço e banda
- ☁️ **Suporte a múltiplos destinos** — Cloudflare R2, Amazon S3, Google Cloud, Azure, SFTP, WebDAV ou disco local
- 📸 **Snapshots versionados** — cada backup é uma "foto" completa do momento, com histórico
- 📋 **Políticas de retenção** — defina quantas versões manter por hora, dia, semana, mês e ano
- 🖥️ **Interface web moderna** — tema branco limpo com cores que indicam claramente cada ação
- 💻 **App desktop** — funciona no Windows, macOS e Linux como aplicativo nativo (Electron)
- 🔧 **CLI poderosa** — para quem prefere o terminal

---

## 🎨 Identidade Visual RX-Corp

O Revolution X Vault usa um sistema de **cores semânticas** nos botões — cada cor comunica a intenção da ação:

| Cor | Significado | Exemplos |
|:---:|:---|:---|
| 🟢 **Verde** | Confirmar, Salvar, Iniciar Backup | *Backup Now*, *Save Policy*, *Connect* |
| 🔵 **Azul** | Navegar, Restaurar, Baixar | *Restore*, *Browse*, *Download*, *Edit* |
| 🟡 **Amarelo** | Atenção, Voltar, Sincronizar | *Sync*, *Cancel*, *Back*, alertas |
| 🔴 **Vermelho** | Excluir, Desconectar, Remover | *Delete*, *Disconnect*, *Remove* |

---

## 📦 Estrutura do Projeto

```
revolution-x-vault/
├── app/                    # Aplicativo desktop (Electron)
├── cli/                    # Interface de linha de comando (Go)
├── htmlui/                 # Interface web (React + Vite)
│   ├── src/
│   │   ├── css/
│   │   │   ├── App.css                  # Estilos base
│   │   │   ├── Theme.css                # Temas (light, dark, etc.)
│   │   │   └── custom-revolutionx.css   # ✨ Tema RX-Corp (override visual)
│   │   ├── components/     # Componentes React
│   │   ├── pages/          # Páginas da interface
│   │   └── App.jsx         # Componente principal (navbar customizada)
│   └── public/             # Assets estáticos
├── repo/                   # Motor de repositório e storage (Go)
├── snapshot/               # Lógica de snapshots (Go)
├── internal/               # Pacotes internos (Go)
├── notification/           # Sistema de notificações
├── icons/                  # Ícones do aplicativo
├── site/                   # Documentação (Hugo)
├── main.go                 # Ponto de entrada do CLI
├── go.mod / go.sum         # Dependências Go
├── Makefile                # Build system
└── GUIA_INICIANTE.md       # 📖 Guia completo para iniciantes
```

---

## ⚖️ Licença e Créditos

Este projeto é uma customização visual do **Kopia** ([github.com/kopia/kopia](https://github.com/kopia/kopia)), distribuído sob a licença **Apache 2.0**.

- **Motor de backup:** [Kopia](https://kopia.io) — Copyright Kopia Authors
- **Customização visual e branding:** RX-Corp
- **Repositório:** [github.com/wendellrox/revolution-x-vault](https://github.com/wendellrox/revolution-x-vault)

Nenhuma funcionalidade do Kopia foi alterada. Todas as modificações são estritamente visuais (CSS, títulos, cores e identidade de marca).
