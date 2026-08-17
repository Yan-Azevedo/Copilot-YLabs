# 🕰️ Ferias Aprovacao — Copilot Studio Clássico

> Agente conversacional de **solicitação de férias** construído na **experiência clássica** do Microsoft Copilot Studio — conduzido por um **tópico** com passo a passo fixo.

O agente **Holiday Assist** usa um **tópico** que faz as perguntas na ordem, coleta os dados, confirma com o colaborador e aciona um **fluxo (Power Automate)** que envia o pedido para **aprovação assíncrona** por e-mail.

---

## 🎯 O que este laboratório entrega

- Um agente que coleta os dados de férias por meio de um **tópico determinístico** (uma pergunta por vez, em ordem fixa).
- Um **fluxo** que registra o pedido e envia para **aprovação assíncrona** via conector de Aprovações.
- Conhecimento fundamentado na **Política de Férias — Contoso** (sem busca na web).
- Controle total do roteiro da conversa — ideal para quem quer **previsibilidade** em cada etapa.

---

## 🗺️ Os passos

| Passo | Arquivo | O que você faz |
|---|---|---|
| **1️⃣** | Classico-PASSO-1-AGENTE.md | Cria e configura o agente Holiday Assist |
| **1.1** | Classico-PASSO-1.1-AGENTE-Conhecimento.md | Adiciona a Política de Férias como conhecimento |
| **2️⃣** | Classico-PASSO-2-TOPICO.md | Cria o tópico que conduz a conversa e coleta os dados |
| **3️⃣** | Classico-PASSO-3-FLUXO.md | Constrói o fluxo de aprovação e mapeia as entradas do tópico |

> 💡 No clássico, o **Conhecimento** é uma etapa separada (1.1) e a conversa é comandada por um **tópico** — por isso são **4 arquivos**.

---

## ✅ Pré-requisitos

- Acesso ao **Microsoft Copilot Studio** (experiência clássica).
- Permissão para criar **fluxos** (Power Automate) no ambiente.
- Conector de **Aprovações** disponível no tenant.
- O documento **Política de Férias — Contoso** (../Politica-de-Ferias-Contoso.md).

---

## 🧩 Arquitetura em uma olhada

```
Colaborador
    |  "quero pedir férias"
    v
Tópico: Solicitar Ferias   (perguntas em ordem fixa)
    |  DataInicio -> DataFim -> QtdDias -> EmailAprovador -> Observacao
    v
Mensagem de resumo + confirmação (Sim/Não)
    |  Sim
    v
Ação: chama o fluxo Enviar Aprovacao de Ferias
    |  Respond to the agent  (confirmação imediata)
    v
Iniciar e aguardar uma aprovação   (assíncrono)
    |
    v
Card de aprovação por e-mail  ->  Aprovar / Rejeitar
```

---

## ⚙️ Decisões de projeto

| Item | Escolha | Por quê |
|---|---|---|
| Condução da conversa | **Tópico** (O agente escolhe) | Ordem fixa e previsível das perguntas |
| Tipos das variáveis | Todas **string** (Resposta inteira do usuário) | O fluxo espera texto em todos os campos; evita erro de tipo |
| Aprovação | Assíncrona (depois do Respond to the agent) | Evita o timeout de 100s do agent flow |
| Campo "Atribuído a" | Fixo, uma pessoa específica | Escopo de treinamento, sem lógica dinâmica |
| Nome do solicitante | User.Email (aba Sistema) | Simplifica o mapeamento no tópico |

---

## ⚠️ Lições aprendidas (troubleshooting rápido)

- **Erro de tipo ao mapear o fluxo** (`StartDate é do tipo incorreto: Date`): configure as perguntas de data/dias como **Resposta inteira do usuário** (string), não Data/Número.
- **Agente responde a lista de dados de uma vez** em vez de conduzir pelo tópico: enxugue as **Instruções** para não competir com o tópico — deixe nelas apenas tom, limites e a orientação de usar o tópico; **publique** depois.
- **"Identificador não reconhecido na expressão"** no resumo: insira as variáveis pelo **botão de variável**, nunca digitando `{DataFim}` como texto.
- **Não recebe o card de aprovação:** confirme que o campo **Atribuído a** aponta para você mesmo; confira o **Histórico de execuções** no Power Automate.
- **Alterações não têm efeito:** salvar o tópico não basta — publique o agente (**Overview → Publish**).

---

## 🏁 Clássico × Moderno

| Aspecto | Clássico | Moderno |
|---|---|---|
| Nº de arquivos | 4 (3 passos + etapa 1.1) | 3 |
| Coleta dos dados | Tópico com 5 nós de pergunta | Instruções + orquestrador |
| Mapeamento de entradas | Manual (variável a variável) | Por IA, guiado pela Descrição |
| Nome do solicitante | User.Email mapeado no tópico | IA (usa o usuário logado) |
| Peça mais trabalhosa | O tópico (documento extenso) | Deixou de existir |

> 👉 Quer ver a versão sem tópico? Veja o caminho moderno em ../Copilot-Studio-Moderno.

---

## 📈 Evolução do agente

Quer levar o agente além da versão base (esqueleto → rastreável → inteligente)? Veja as camadas de evolução em ../EVOLUCAO.md.

---

## ▶️ Comece agora

Vá direto para o Passo 1 — Criação do Agente e siga a sequência (1 → 1.1 → 2 → 3). 🚀