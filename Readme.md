# 🤖 AutoSign Bot — Assinatura Digital Automatizada

> Projeto semestral de automação de processos corporativos (RPA / Backend) focado na validação, ingestão e assinatura digital automatizada de documentos em lote.

---

## 📌 Visão Geral do Projeto

O **AutoSign Bot** foi concebido para resolver o gargalo operacional enfrentado por empresas que processam grandes volumes de documentos (contratos, termos de adesão, formulários admissionais). O sistema orquestra robôs que monitoram fluxos de entrada, validam integridade de arquivos e executam assinaturas digitais com conformidade jurídica e segurança criptográfica.

### 🎯 Principais Objetivos
- **Redução de SLA:** Diminuir o tempo médio de assinatura de horas/dias para segundos.
- **Conformidade:** Garantir cadeia de custódia e validação de certificados digitais (A1 / ICP-Brasil / PAdES).
- **Rastreabilidade:** Logs detalhados de auditoria e monitoramento de falhas em tempo de execução.

---

## 🛠️ Stack Tecnológica & Dependências

Abaixo estão as ferramentas e bibliotecas recomendadas para a implementação ao longo do semestre:

| Camada | Tecnologias Sugeridas | Descrição |
| :--- | :--- | :--- |
| **Robôs / Core** | Python / Node.js | Linguagens com forte suporte a automação, filas e criptografia |
| **Manipulação PDF & Crypto** | PyMuPDF, OpenSSL, Endesive | Criação de carimbos, injeção de hashes e suporte a certificados Pkcs12 (`.pfx`/`.p12`) |
| **Automação Web/API (RPA)** | Playwright / Selenium / Requests | Interação com portais de terceiros ou consumo direto de APIs REST |
| **Fila & Agendamento** | Redis + Celery / BullMQ | Gerenciamento de tarefas em segundo plano e retentativas automáticas |
| **Banco de Dados** | PostgreSQL / SQLite | Armazenamento de metadados, status da fila e histórico de logs |
| **Infra & Container** | Docker & Docker Compose | Padronização dos ambientes de desenvolvimento e produção |

---

## 🏗️ Arquitetura do Fluxo

```text
[ Documentos Pendentes ]
          │
          ▼
┌──────────────────┐      ┌─────────────────────────┐
│  Ingestão/Fila   │ ───► │   Robô de Validação     │
│ (Folder/API/Web) │      │  (Hash & Metadados)     │
└──────────────────┘      └────────────┬────────────┘
                                       │
                                       ▼
┌──────────────────┐      ┌─────────────────────────┐
│ Armazenamento /  │ ◄─── │    Robô de Assinatura   │
│ Notificação Final│      │ (Injeção de Certificado)│
└──────────────────┘      └─────────────────────────┘