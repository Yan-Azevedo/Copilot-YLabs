# 🧭 Modelo 2 — Novo Studio COM tópico (determinístico)

> Copilot Studio **atual**, mas usando **tópico** para controlar a conversa. Escolha este quando precisar de **ordem fixa das perguntas, validação de dados ou adaptive card**. A orquestração generativa continua ligada, mas **o tópico conduz** o fluxo.

**Perfil:** controle total • previsível
**Complexidade:** ⭐⭐⭐☆☆ • ~40 min

---

## 🧭 O que vamos montar

```
[Agent flow "Enviar Aprovacao de Ferias"]  ← a ferramenta
[Tópico "Solicitar Ferias"]  → perguntas fixas → chama a ferramenta → confirma
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
- Em **Respond to the agent**: **Asynchronous = OFF** (tempo real, limite 100s).
- Saída `Resultado` = **Outcome** da aprovação.

### Passo 5 — **Save** → **Publish** (só aparece como ferramenta depois de publicado).

---

## PARTE 2 — Criar o Tópico

### Passo 6 — Tópico
- **Topics** → **+ Add a topic** → **From blank**. Nome: **`Solicitar Ferias`**.
- Frases de gatilho:
  ```
  quero pedir férias
  solicitar férias
  registrar minhas férias
  ```

### Passo 7 — Perguntas (Question nodes)

| Pergunta | Variável | Tipo |
|---|---|---|
| "Data de **início**?" | `StartDate` | Date |
| "Data de **fim**?" | `EndDate` | Date |
| "**Quantos dias**?" | `Days` | Number |
| "**E-mail do aprovador**?" | `ApproverEmail` | Text |
| "Alguma **observação**?" | `Details` | Text |

> 💡 Aqui você pode adicionar **validação** (ex.: data fim > data início) — a vantagem do tópico.

### Passo 8 — Chamar a ferramenta
- Nó **Add a tool** (Action) → **`Enviar Aprovacao de Ferias`**.
- Mapeie os inputs → variáveis (`RequesterName` ← `System.User.DisplayName`, `StartDate` ← `Topic.StartDate`, etc.).

### Passo 9 — Confirmação
- **Message node:** `Pronto! ✅ Enviado para {Topic.ApproverEmail}.`

### Passo 10 — **Save** → **Publish**.

---

## 🧪 Testar
"quero pedir férias" → perguntas em ordem fixa → aprovador recebe card → Aprovar/Recusar → confirmação.

---

## ⚠️ Notas do modelo
- **Quando usar:** precisa de ordem rígida, validação ou adaptive card.
- **Async OFF** e **publicar** sempre.
- Mais peças que o Modelo 3, porém **100% previsível**.

> ➡️ Se você **não** precisa desse controle, o **Modelo 3 (sem tópico)** é mais rápido e moderno.
