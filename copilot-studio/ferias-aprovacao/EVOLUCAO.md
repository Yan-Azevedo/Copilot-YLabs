# 📈 Evolução do Agente — de esqueleto a robusto

> Mostra como o **mesmo agente** de aprovação de férias cresce em camadas.
> No treinamento, **construímos só a v1** (qualquer um dos 3 modelos). As v2 e v3 são apresentadas como "para onde isso vai".

---

## 🪜 Visão em camadas

| Versão | Foco | Aprovador | Histórico | Complexidade |
|---|---|---|---|---|
| **v1 — Esqueleto** | Funcionar ponta a ponta | E-mail digitado (colega / você) | Não | ⭐⭐☆☆☆ |
| **v2 — Rastreável** | Registro + resposta automática | E-mail digitado | Sim (lista SharePoint) | ⭐⭐⭐☆☆ |
| **v3 — Inteligente** | Automação de RH de verdade | Gestor automático (Entra ID) | Sim + auditoria | ⭐⭐⭐⭐☆ |

---

## ✅ v1 — Esqueleto (a que construímos)
- Conversa → coleta → chama a ferramenta → dispara aprovação → confirma.
- Aprovador **digitado pelo usuário** (colega ao lado ou o próprio e-mail).
- Sem registro; resultado sai por e-mail simples.

**Por que começar aqui:** todo aluno conclui e vê a aprovação real funcionando. Ensina o padrão **conversa → coleta → ação → resposta**.

---

## 🔵 v2 — Rastreável (histórico + retorno automático)

Adicionamos sobre a v1:

1. **Registrar o pedido** — crie uma **lista SharePoint** `Solicitacoes de Ferias` (Solicitante, Início, Fim, Dias, Aprovador, Status, Data). No flow, **antes** da aprovação, use **Create item** com `Status = Pendente`.
2. **Atualizar o status** — após a decisão, use **Update item**: `Aprovado` ou `Recusado`.
3. **Notificar automaticamente** — e-mail dinâmico para quem pediu, com o resultado (opcional: card no Teams).

**Ganho didático:** ensina **gravar e ler dados** (não só enviar) e o conceito de **rastreabilidade**.

---

## 🟣 v3 — Inteligente (automação de RH de verdade)

Camada para mostrar o **potencial máximo** — normalmente só apresentada.

1. **Aprovador automático** — remova o input `ApproverEmail` e use **Get manager (V2)** (conector **Office 365 Users**): passe `System.User.Email` e o flow descobre o gestor. Use o campo **Mail** como **Assigned to**.
2. **Cálculo de dias úteis** — com **Power Fx**, calcule os dias entre início e fim ignorando fins de semana.
3. **Aprovação multi-estágio (multistage/AI approvals — preview)** — recurso exclusivo dos agent flows: gestor aprova → RH confirma, com **condições entre estágios** (ex.: > 15 dias exige aprovação dupla).
4. **Governança** — trilha de auditoria no histórico do flow + versionamento da lista SharePoint.

**Ganho didático:** mostra a fronteira entre "agente de demonstração" e "processo de negócio automatizado".

---

## 🗺️ Como usar no treinamento

```
Aula prática  →  constrói a v1 (todos concluem)
Demonstração  →  mostra a v2 pronta (rastreabilidade)
Visão/roadmap →  apresenta a v3 (para onde escala)
```

---

### 💡 Ideia fora da caixa
Feche pedindo para cada dupla **imaginar a v4** para o **departamento deles** (RH, TI, Financeiro...). Vira insumo direto para os próximos casos por área.
