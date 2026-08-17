# ⚡ Modelo 3 — Novo Studio SEM tópico (generativo) ⭐ recomendado

> Copilot Studio **atual** usando **orquestração generativa pura**. Você **não cria tópico**: o agente **pergunta os dados sozinho** e **redige a resposta sozinho**. Basta o **agent flow como ferramenta** + uma **boa descrição**. É o caminho mais enxuto e moderno — ideal para criação básica.

**Perfil:** mínimo de peças • moderno
**Complexidade:** ⭐⭐☆☆☆ • ~25 min

---

## 🧭 O que vamos montar

```
[Agent flow "Enviar Aprovacao de Ferias"]  ← a única peça a construir
   + descrição clara  +  instruções do agente
   → o orquestrador conduz toda a conversa
```

---

## PARTE 1 — Criar o Agent Flow (a ferramenta)

### Passo 1 — Novo agent flow
1. **Tools** → **Add a tool** → **New** → **Agent flow** *(pode aparecer como **Workflow**)*.
2. Abre com **When an agent calls the flow** + **Respond to the agent**.
3. Nome: **`Enviar Aprovacao de Ferias`**.

### Passo 2 — Entradas
No gatilho, **+ Add an input** (Text): `RequesterName`, `ApproverEmail`, `StartDate`, `EndDate`, `Days`, `Details`.

### Passo 3 — Enviar aprovação
- **Start and wait for an approval** (Approvals):
  - **Type:** `Approve/Reject – First to respond`
  - **Assigned to:** `ApproverEmail` • **Title/Details** com os campos.

### Passo 4 — Responder ao agente
- **Respond to the agent**: **Asynchronous = OFF** (tempo real, limite 100s).
- Saída `Resultado` = **Outcome** da aprovação.

### Passo 5 — **Save** → **Publish**.

---

## PARTE 2 — Conectar ao agente (sem tópico!)

### Passo 6 — Adicionar o flow como ferramenta
1. **Tools** → **Add a tool** → **Flow** → **`Enviar Aprovacao de Ferias`** → **Add and configure**.
2. **Descrição** (o orquestrador lê isto para decidir usar) — capriche:
   ```
   Use esta ferramenta quando o usuário quiser solicitar férias.
   Ela envia um pedido de aprovação ao aprovador informado.
   ```
3. **Inputs:** marque cada campo como **"Preencher com IA generativa"**
   (o agente pergunta ao usuário automaticamente). Para `RequesterName`, use `System.User.DisplayName`.

### Passo 7 — Instruções do agente
Em **Instructions**:
```
Você ajuda colaboradores a solicitar férias.
Ao pedir férias, colete: data de início, data de fim, número de dias,
o e-mail do aprovador (o colega ao lado) e uma observação opcional.
Depois use a ferramenta "Enviar Aprovacao de Ferias" e informe o resultado.
```

### Passo 8 — **Save** → **Publish**.

---

## 🧪 Testar
"quero pedir férias" → o **agente pergunta sozinho** os dados que faltam → chama o flow → aprovador recebe card → Aprovar/Recusar → o **agente redige a resposta** com o resultado.

---

## ⚠️ Notas do modelo
- **A descrição é tudo:** é ela que faz o orquestrador escolher (ou não) o flow. Descrição fraca = agente não usa a ferramenta.
- **Sem tópico, sem question nodes, sem mapeamento** — menos pontos de erro.
- **Async OFF** e **publicar** sempre. **Aprovador no mesmo tenant**.

> 💡 **Para criação básica, comece por este modelo.** Só migre para o Modelo 2 (com tópico) se precisar de ordem fixa/validação.
