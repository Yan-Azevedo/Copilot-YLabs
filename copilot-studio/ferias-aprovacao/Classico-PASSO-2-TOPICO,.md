# 🕰️ Modelo 1 — Clássico (Copilot Studio antigo)

> **Orquestração clássica.** O **tópico comanda tudo**: reconhece a intenção pelas frases de gatilho, faz as perguntas em ordem fixa e chama a ação. É o modelo tradicional, bom para quem já conhecia o Power Virtual Agents / Studio antigo.

**Perfil:** determinístico • previsível • mais peças
**Complexidade:** ⭐⭐⭐☆☆ • ~40 min

---

## 🧭 O que vamos montar

```
[Tópico "Solicitar Ferias"]
   → frases de gatilho
   → Pergunta 1: início   → Pergunta 2: fim
   → Pergunta 3: dias      → Pergunta 4: e-mail do aprovador
   → Pergunta 5: observação
   → Call an action (fluxo "Enviar Aprovacao de Ferias")
   → Mensagem de confirmação
```

---

## PARTE 1 — Criar o Fluxo (Power Automate)

### Passo 1 — Novo fluxo
1. No agente, vá em **Flows** (ou **Actions**) → **+ New flow**.
2. Gatilho: **When Power Virtual Agents calls a flow** *(nome antigo do gatilho)*.
3. Nomeie: **`Enviar Aprovacao de Ferias`**.

### Passo 2 — Entradas
Adicione entradas de texto: `RequesterName`, `ApproverEmail`, `StartDate`, `EndDate`, `Days`, `Details`.

### Passo 3 — Enviar aprovação
- Ação **Start and wait for an approval** (conector **Approvals**):
  - **Approval type:** `Approve/Reject – First to respond`
  - **Title:** `Nova solicitação de férias de ` + `RequesterName`
  - **Assigned to:** `ApproverEmail`
  - **Details:** período, dias e observação.

### Passo 4 — Retornar ao Studio
- Ação **Return value(s) to Power Virtual Agents** com a saída `Resultado` = **Outcome** da aprovação.

### Passo 5 — Salvar e publicar o fluxo.

---

## PARTE 2 — Criar o Tópico

### Passo 6 — Tópico
- **Topics** → **+ New topic** → **From blank**. Nome: **`Solicitar Ferias`**.

### Passo 7 — Frases de gatilho
```
quero pedir férias
solicitar férias
pedir folga
como faço para tirar férias
registrar minhas férias
```

### Passo 8 — Perguntas (Question nodes)

| Pergunta | Variável | Tipo |
|---|---|---|
| "Data de **início**?" | `StartDate` | Date |
| "Data de **fim**?" | `EndDate` | Date |
| "**Quantos dias**?" | `Days` | Number |
| "**E-mail do aprovador** (colega ao lado)?" | `ApproverEmail` | Text |
| "Alguma **observação**?" | `Details` | Text |

### Passo 9 — Chamar o fluxo
- Nó **Call an action** → **`Enviar Aprovacao de Ferias`**.
- Mapeie cada entrada do fluxo para a variável do tópico (`RequesterName` ← `bot.UserDisplayName`, etc.).

### Passo 10 — Confirmação
- **Message node:** `Pronto! ✅ Enviado para {Topic.ApproverEmail}.`

### Passo 11 — Salvar e publicar o agente.

---

## 🧪 Testar
"quero pedir férias" → responde as perguntas → aprovador (colega/você) recebe card no Teams → Aprovar/Recusar → confirmação no chat.

---

## ⚠️ Notas do modelo clássico
- Nomes antigos: **"When Power Virtual Agents calls a flow"** e **"Return value(s) to Power Virtual Agents"**.
- Tudo é **manual**: nenhuma pergunta é gerada automaticamente.
- **Aprovador no mesmo tenant** e **publicar** para valer.

> ➡️ Compare com o **Modelo 2 (novo, com tópico)** e o **Modelo 3 (novo, sem tópico)** para ver a evolução da plataforma.
