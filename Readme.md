# 🤖 AutoSign Bot — Assinatura Digital Automatizada

> Projeto semestral de automação de processos corporativos focado na captura de notificações via Microsoft Outlook, orquestração de robôs (Scripts / Power Automate) e assinatura digital automatizada em plataformas e fluxos em lote.

---

## 📌 Visão Geral do Projeto

O **AutoSign Bot** foi concebido para resolver o gargalo operacional de empresas que recebem centenas de solicitações de assinatura por e-mail de diversas plataformas (DocuSign, ClickSign, Clicksign, Adobe Sign, D4Sign, etc.). 

O sistema monitora uma caixa de entrada dedicada no **Microsoft Outlook**, extrai metadados, links de acesso e anexos, e dispara robôs que executam as etapas de validação e assinatura com conformidade jurídica e segurança criptográfica.

### 🎯 Principais Objetivos
- **Ingestão Automatizada via Outlook:** Eliminar a triagem manual de caixas de entrada corporativas.
- **Redução de SLA:** Processar solicitações de assinatura em segundos a partir do recebimento do e-mail.
- **Conformidade & Rastreabilidade:** Garantir o ciclo de custódia, validação de certificados digitais (A1 / ICP-Brasil / PAdES) e registro completo de logs/auditoria.
- **Flexibilidade de Orquestração:** Suporte a abordagens híbridas via scripts customizados ou fluxos Low-Code com Microsoft Power Automate.

---

## 🛠️ Stack Tecnológica & Sugestões de Ferramentas

| Camada | Tecnologias / Abordagens | Descrição |
| :--- | :--- | :--- |
| **Ingestão & Notificações** | Microsoft Outlook (Graph API / IMAP / Add-ins) | Monitoramento de e-mails recebidos, parsing de links de assinatura e extração de PDFs |
| **Orquestração & Low-Code** | **Microsoft Power Automate** | *Sugestão empresarial:* Gatilhos automáticos disparados por novos e-mails no Outlook, chamadas HTTP/webhooks e acionamento de fluxos de Desktop (RPA) |
| **Robôs Customizados (RPA)** | Python (Playwright / Selenium) ou Node.js | Navegação automatizada nas interfaces das plataformas de assinatura e preenchimento de campos |
| **Manipulação PDF & Crypto** | PyMuPDF, OpenSSL, Endesive | Validação de hashes, carimbos de tempo e injeção de certificados digitais (`.pfx` / `.p12`) em documentos diretos |
| **Fila & Agendamento** | Redis + Celery / Azure Queue / BullMQ | Controle de concorrência, retentativas em falha de conexão e priorização de tarefas |
| **Banco de Dados** | PostgreSQL / SQL Server / SQLite | Registro de logs de auditoria, histórico de e-mails processados e status de execução |
| **Infra & Container** | Docker & Docker Compose | Padronização dos ambientes de desenvolvimento e dos robôs headless |

---

## 🏗️ Arquitetura do Fluxo

```text
 ┌─────────────────────────────────────────┐
 │   Plataformas de Assinatura Externa     │
 │ (DocuSign, ClickSign, D4Sign, etc.)     │
 └────────────────────┬────────────────────┘
                      │ Envio de e-mail de notificação
                      ▼
 ┌─────────────────────────────────────────┐
 │         Microsoft Outlook               │
 │    (Caixa corporativa / Webhook)        │
 └────────────────────┬────────────────────┘
                      │
        ┌─────────────┴─────────────┐
        │                           │
        ▼                           ▼
┌──────────────────┐        ┌─────────────────────────┐
│ Power Automate   │   OU   │ Worker / Script Dedicado│
│ (Triggers/Cloud) │        │ (Microsoft Graph API)   │
└───────┬──────────┘        └───────────┬─────────────┘
        │                               │
        └──────────────┬────────────────┘
                       │ Extrai link / anexo
                       ▼
 ┌─────────────────────────────────────────┐
 │        Fila de Tarefas / Dispatcher     │
 └─────────────────────┬───────────────────┘
                       │ Dispara execução
                       ▼
 ┌─────────────────────────────────────────┐
 │            Robô de Assinatura           │
 │ (RPA Web nas plataformas / Módulo PAdES)│
 └─────────────────────┬───────────────────┘
                       │ Confirmação & Evidências
                       ▼
 ┌─────────────────────────────────────────┐
 │    Banco de Dados & Logs de Auditoria   │
 └─────────────────────────────────────────┘

```

---

## 📋 Abordagens de Implementação do Robô

A equipe pode estruturar a solução seguindo duas abordagens principais:

### 1. Abordagem Híbrida / Corporativa (Power Automate + RPA)

* O **Power Automate Cloud** utiliza o conector nativo do Office 365 Outlook com o trigger *"Quando um novo email é recebido"*.
* Filtra por remetente/assunto das plataformas de assinatura.
* Executa parsing do link de assinatura ou envia o payload via webhook para um **Power Automate Desktop** ou robô backend em Python/Node.js.

### 2. Abordagem Standalone / Código Aberto (API + Bot Dedicado)

* Um worker consome a **Microsoft Graph API** (ou protocolo IMAP com autenticação moderna OAuth2).
* Monitora pastas específicas de e-mail, extrai o HTML do corpo da mensagem e localiza o token/URL de assinatura via expressões regulares.
* Dispara uma rotina em Playwright/Selenium para assinar e salva o comprovante.

---

## 🔒 Boas Práticas de Segurança

* **Credenciais e Autenticação:**
* Configurar registro de aplicativo no Microsoft Entra ID (Azure AD) para permissões delegadas/aplicação via Graph API.
* Certificados digitais (`.pfx`, `.p12`) e senhas **nunca** devem ser versionados no Git. Utilize variáveis de ambiente ou cofres de segredos (Key Vault / `.env`).


* **Idempotência:**
* Marcar e-mails como lidos, aplicar flags ou mover para pastas (ex: `/Processados`, `/Erros`) no Outlook para evitar processamento duplicado.


* **Proteção de Dados (LGPD):**
* Tratar com sigilo os dados cadastrais e conteúdos presentes nos documentos trafegados.



---

## 📅 Roadmap Semestral(VAMOS TER QUE DEFINIR BEM CERTINHO)

* [x] **Sprint 1 (Mês 1):** Definição de arquitetura, alinhamento de plataformas de assinatura da empresa e setup do repositório.
* [ ] **Sprint 2 (Mês 2):** Configuração do recebimento no Outlook (regras de caixa, permissões de API ou trigger no Power Automate).
* [ ] **Sprint 3 (Mês 3):** Desenvolvimento do robô de automação (interação web com as plataformas de assinatura ou injeção PAdES local).
* [ ] **Sprint 4 (Mês 4):** Orquestração ponta a ponta (Outlook ➔ Robô ➔ Registro de Logs), com tratamento de falhas e reprocessamento.
* [ ] **Sprint 5 (Mês 5):** Testes de carga em ambiente de homologação, validação com a empresa e entrega da documentação acadêmica.

---

## 👥 Equipe do Projeto

| Nome | Função / Responsabilidade | GitHub |
| --- | --- | --- |
| **Integrante 1** | Integração Outlook & Power Automate | [@user1](https://github.com) |
| **Integrante 2** | Automação RPA (Web / Navegador) | [@user2](https://github.com) |
| **Integrante 3** | Criptografia, PDFs & Backend | [@user3](https://github.com) |
| **Integrante 4** | Banco de Dados, Infra & Documentação | [@user4](https://github.com) |

---

## 📄 Licença

Este projeto é desenvolvido para fins acadêmicos sob a licença [MIT](https://www.google.com/search?q=LICENSE).
