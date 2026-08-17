# 🏖️ Caso 1 — Aprovação de Férias com Agente Conversacional

> Treinamento prático de criação de um agente de solicitação de férias no **Microsoft Copilot Studio**.

<p align="center">
  ../assets/Logo-YLabs.pngboratórios práticos de agentes de IA" width="100%"/>
</p>

**Apresentador:** Yan Azevedo | Cloud Target
**Área:** Inovação, adoção e treinamento prático em soluções com Microsoft 365 e Copilot Studio.

---

## 🎯 Caso apresentado

Foi apresentado um cenário de treinamento prático para criação de um agente de solicitação de férias no Microsoft Copilot Studio. O agente recebe o pedido do colaborador em linguagem natural, coleta informações básicas como data de início, data de fim, quantidade de dias, aprovador e observação, e aciona um fluxo de aprovação utilizando recursos nativos do Microsoft 365.

O aprovador pode ser um colega indicado por e-mail ou a própria pessoa, desde que esteja no mesmo tenant. A aprovação é enviada por meio dos canais Microsoft 365, como Teams e Outlook, e o resultado é retornado ao colaborador no próprio chat do agente.

---

## ❗ Problema encontrado

O processo de solicitação de férias, quando feito manualmente, pode depender de mensagens informais, e-mails sem padrão ou alinhamentos diretos entre colaborador e aprovador. Isso gera risco de informações incompletas, falta de padronização no pedido e pouca clareza sobre o status da aprovação.

No contexto do treinamento, a dor principal é demonstrar um processo realista de negócio que seja simples o suficiente para ser construído em prática, mas que também mostre valor concreto na combinação entre agente conversacional, coleta de dados e aprovação humana.

---

## 🎯 Objetivo

Criar um agente capaz de conduzir uma solicitação de férias do início ao fim em ambiente de treinamento. A solução deve receber o pedido em linguagem natural, coletar os dados necessários, encaminhar uma aprovação para o responsável informado e devolver ao colaborador o resultado da decisão.

O objetivo também é permitir que os participantes compreendam, na prática, como um agente pode transformar uma conversa em uma ação estruturada dentro do Microsoft 365.

---

## ✅ Problema que soluciona

A solução reduz a dependência de solicitações manuais e mensagens sem padrão, garantindo que os dados mínimos sejam coletados antes do envio para aprovação. Também melhora a experiência do colaborador, que passa a interagir com um agente em linguagem simples, sem precisar montar manualmente um pedido ou seguir instruções dispersas.

Para o treinamento, o case soluciona a dificuldade de demonstrar automação de processos de forma tangível, usando um exemplo comum, fácil de entender e aplicável a outros cenários de aprovação.

---

## 💎 Valor da construção

O principal valor está em mostrar como um processo corporativo simples pode ser convertido em uma experiência guiada por IA conversacional e automação.

A construção entrega valor porque:

- Padroniza a coleta das informações necessárias para o pedido de férias.
- Reduz retrabalho causado por solicitações incompletas.
- Demonstra o uso de aprovação humana dentro de um fluxo automatizado.
- Usa ferramentas já presentes no ecossistema Microsoft 365.
- Facilita o entendimento de agentes conectados a ações práticas.
- Serve como base reutilizável para outros processos simples, como pedido de ausência, home office, reembolso ou aprovação administrativa.
- Mantém o escopo adequado para treinamento, sem depender de sistemas externos ou integrações complexas.

---

## 📊 Complexidade

**Baixa.**

A complexidade é baixa porque o case utiliza apenas coleta de informações, envio de aprovação e retorno do resultado ao usuário. Não há integração com sistema de RH, consulta de saldo de férias, validação trabalhista, múltiplos níveis de aprovação ou banco de dados externo.

A solução é adequada para ambiente de treinamento prático, pois possui escopo controlado, baixo risco técnico e permite demonstrar valor rapidamente com conectores nativos do Microsoft 365.

---

## 🧭 Guias de construção

O mesmo caso é construído de **duas formas**, para você comparar a experiência clássica com a moderna do Copilot Studio:

| Caminho | Experiência | Como conduz a conversa | Guia |
|---|---|---|---|
| 🕰️ **Clássico** | Copilot Studio clássico | Tópico com nós de pergunta (passo a passo fixo) | Copilot-Studio-Classico/ |
| ⚡ **Moderno** | Copilot Studio (New experience) | Orquestração generativa, **sem tópico** | [CopilotStudio-Moderno/ |

### 🕰️ Caminho Clássico
1. Copilot-Studio-Classico/Classico-PASSO-1-AGENTE.md
2. Copilot-Studio-Classico/Classico-PASSO-1.1-AGENTE-Conhecimento.md
3. [Passo 2 — Criação do Tópico](CopilotO-2-TOPICO.md
4. Copilot-Studio-Classico/Classico-PASSO-3-FLUXO.md

### ⚡ Caminho Moderno
1. Copilot-Studio-Moderno/Moderno-Passo-1.md
2. [Passo 2 — Criação do Fluxo](Copilot-Studio-Moderno/Moderno-Passo-2/Moderno-Passo-3.md

---

## 🏁 Clássico × Moderno

| Aspecto | Clássico | Moderno |
|---|---|---|
| Nº de arquivos | 4 (3 passos + etapa 1.1) | 3 |
| Coleta dos dados | Tópico com 5 nós de pergunta | Instruções + orquestrador |
| Mapeamento de entradas | Manual (variável a variável) | Por IA, guiado pela Descrição |
| Nome do solicitante | User.Email mapeado no tópico | IA (usa o usuário logado) |
| Peça mais trabalhosa | O tópico (documento extenso) | Deixou de existir |

---

## 📈 Evolução do agente

Quer levar o agente além da versão base (esqueleto → rastreável → inteligente)? Veja as camadas de evolução em [EVOLUCAO.md](EVOLUCAO.md).
