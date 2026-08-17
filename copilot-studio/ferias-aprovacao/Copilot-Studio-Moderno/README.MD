# 🏖️ Ferias Aprovacao — Copilot Studio Moderno

> Agente conversacional de **solicitação de férias** construído na **experiência moderna** do Microsoft Copilot Studio — **sem tópico**, apoiado 100% na **orquestração generativa**.

O agente **Holiday Assist Moderno** conduz a conversa, coleta os dados um a um, confirma o resumo com o colaborador e dispara a aprovação via **Power Automate** — tudo sem um único tópico ou nó de pergunta.

---

## 🎯 O que este laboratório entrega

- Um agente que **conversa** e **decide sozinho** quando chamar a ferramenta (orquestração generativa nativa).
- Um **fluxo de trabalho** (agent flow) que registra o pedido e envia para **aprovação assíncrona** por e-mail.
- Conhecimento fundamentado na **Política de Férias — Contoso** (sem busca na web).
- O mesmo resultado do laboratório clássico, mas com **muito menos peças** para montar.

---

## 🗺️ Os 3 passos

| Passo | Arquivo | O que você faz |
|---|---|---|
| **1️⃣** | [Passo 1 — Criação do Agente + Conhecimento](Moderno-Passo-1.md) | Cria o agente, escreve as instruções e adiciona a política como conhecimento |
| **2️⃣** | Moderno-Passo-2.md | Constrói o agent flow (entradas + Respond to the agent + aprovação) e publica |
| **3️⃣** | Moderno-Passo-3.md | Conecta o fluxo como ferramenta, mapeia as entradas por IA e testa ponta a ponta |

> 💡 No moderno, o **Conhecimento** foi incorporado ao Passo 1 (é só um bloco na mesma tela) — por isso são **3 arquivos**, não 4 como no clássico.

---

## ✅ Pré-requisitos

- Acesso ao **Microsoft Copilot Studio** com a **New experience** ativada.
- Permissão para criar **fluxos** (Power Automate) no ambiente.
- Conector de **Aprovações** disponível no tenant.
- O documento **Política de Férias — Contoso** (../Politica-de-Ferias-Contoso.md).

---

## 🧩 Arquitetura em uma olhada

```
Colaborador
    |  "quero pedir férias"
    v
Holiday Assist Moderno   (orquestração generativa — sem tópico)
    |  coleta 1 a 1 + resumo + confirmação
    v
Ferramenta: Modern Aproval Holiday   (agent flow)
    |  Respond to the agent  (confirmação imediata)
    v
Iniciar e aguardar uma aprovação   (assíncrono)
    |
    v
Card de aprovação por e-mail  ->  Approve / Reject
```

---

## ⚙️ Decisões de projeto

| Item | Escolha | Por quê |
|---|---|---|
| Orquestração | Generativa (nativa) | É o que elimina o tópico e faz o agente conduzir a conversa |
| Aprovação | Assíncrona (depois do Respond to the agent) | Evita o timeout de 100s do agent flow |
| Preenchimento das entradas | Todas por **IA**, guiadas pela Descrição | Não há seletor de variável de sistema nesta experiência |
| Nome do solicitante | IA (usa o usuário logado) | Descrição da entrada orienta o agente a não perguntar |
| Nível de moderação | **Médio** | No **Máximo**, dispara SystemError (MCS-4034) intermitente |

---

## ⚠️ Lições aprendidas (troubleshooting rápido)

- **Erro `SystemError` (MCS-4034) no teste:** baixe o **Nível de moderação** de *Máximo* para *Médio* (⋯ → Configurações → IA e comportamento) e republique.
- **Agente não chama a ferramenta:** reforce a **Descrição da ferramenta** e a instrução de acionamento; sempre **Publique** depois de editar.
- **Não recebe o card de aprovação:** confirme que o campo **Atribuído a** aponta para você mesmo; confira o **Histórico de execuções** no Power Automate.
- **Alterações não têm efeito:** salvar não basta — a versão testada só muda após **Publicar**.

---

## 🏁 Clássico × Moderno

| Aspecto | Clássico | Moderno |
|---|---|---|
| Nº de arquivos | 4 (3 passos + etapa 1.1) | **3** |
| Coleta dos dados | Tópico com 5 nós de pergunta | Instruções + orquestrador |
| Mapeamento de entradas | Manual (variável a variável) | Por IA, guiado pela Descrição |
| Nome do solicitante | User.Email mapeado no tópico | IA (usa o usuário logado) |
| Peça mais trabalhosa | O tópico (documento extenso) | **Deixou de existir** |

> 👉 Quer comparar? Veja a versão clássica em [./Copilot-Studio-Classico.

---

## ▶️ Comece agora

Vá direto para o [**Passo 1 — Criação do Agente + Conhecimento**](Moderno-Passo-1.md) e siga a sequência. 🚀