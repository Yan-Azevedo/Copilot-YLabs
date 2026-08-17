# ⚙️ Passo 3 — Criação do Fluxo (Agent Flow)

> 🎯 **Objetivo desta etapa:** construir o fluxo que recebe os dados coletados pelo tópico, confirma o recebimento e envia o pedido para aprovação de forma assíncrona.

> **Navegação:** Passo 3 de 3 · [⬅️ Voltar ao Passo 2](Classico-PASSO-2-TOPICO.md)

---

## 🚧 Status deste documento

Este passo está sendo construído **em etapas**, junto com os testes reais no Copilot Studio. As seções abaixo cobrem até o nó de **aprovação**. A condição de resultado (Aprovado/Recusado) e as notificações finais entram na próxima atualização.

---

## ⚙️ Decisões tomadas para este fluxo

| Decisão | Escolha |
|---|---|
| Posição da aprovação | **Depois** do `Respond to the agent` (assíncrono — sem risco de timeout) |
| Campo "Atribuído a" | **Fixo**, uma pessoa específica (para efeito de treinamento) |
| Quem pode ser o aprovador | Você mesmo (o criador) **ou** um colega ao lado — desde que no mesmo tenant |

> ℹ️ **Por que assíncrono:** um agent flow chamado por um agente tem limite de **100 segundos** para responder. Como a aprovação espera uma pessoa clicar — o que pode levar minutos —, ela precisa vir **depois** do `Respond to the agent`, nunca antes.

---

## ✅ Pré-requisitos

- Tópico **Solicitar Ferias** criado, salvo, sem erros (ver [Passo 2](Classico-PASSO-2-TOPICO.md)).
- Você chegou a esta tela **a partir do próprio tópico**: no ramo "Todas as outras condições", depois da mensagem "Solicitação realizada com sucesso", clicando em **+ → Adicionar uma ferramenta → Novo fluxo de agente**.

> ℹ️ **Você não precisa criar um flow separado.** Ao clicar em "Novo fluxo de agente" no tópico, o Copilot Studio já abre o designer do fluxo com a estrutura inicial pronta — é essa tela que construímos abaixo.

---

## 1. Estrutura inicial (já vem pronta)

Ao abrir o designer, dois nós já existem, conectados automaticamente:

```text
Quando um agente chama o fluxo   (gatilho)
        ↓
Respond to the agent             (resposta — nome mantido em inglês pela plataforma)
```

> ℹ️ **Modo Expresso (Versão prévia)** aparece como um toggle no nó do gatilho. **Não é usado neste laboratório** — deixe desligado.

Não é necessário criar esses dois nós manualmente. O trabalho começa configurando as **entradas** do gatilho.

---

## 2. Adicionar as entradas do gatilho

No nó **Quando um agente chama o fluxo**:

1. Clique em **+ Adicionar uma entrada**.
2. Um menu de tipos abre. Selecione:

✅ **Texto**

3. Dois campos aparecem: o **nome da entrada** (à esquerda) e uma caixa à direita que já vem preenchida com o texto padrão:

```text
Insira sua entrada aqui
```

**Apague esse texto** — ele não é um placeholder que some sozinho, é texto real que precisa ser removido manualmente.

4. No campo de nome, digite o nome da entrada, conforme a tabela abaixo.
5. Repita o processo (**+ Adicionar uma entrada → Texto → apagar o texto padrão → nomear**) para todas as entradas.

| # | Nome da entrada | Tipo |
|---|---|---|
| 1 | `RequesterName` | Texto |
| 2 | `RequesterEmail` | Texto |
| 3 | `ApproverEmail` | Texto |
| 4 | `StartDate` | Texto |
| 5 | `EndDate` | Texto |
| 6 | `Days` | Texto |
| 7 | `Details` | Texto |

> ⚠️ **`RequesterEmail` não estava na lista original e foi adicionado aqui.** Sem ele, não há como notificar quem fez o pedido — só temos o nome (`RequesterName`), não o e-mail. Se você pretende notificar o solicitante por outro canal (Teams, por exemplo) em vez de e-mail direto, esta entrada pode não ser necessária — avise para ajustarmos.

> 📌 Todas as entradas são do tipo **Texto**, inclusive as datas — o mesmo motivo já usado no tópico: o valor chega como texto e é mais simples de mapear.

---

## 3. Salvar e corrigir os nomes

Este ambiente tem uma particularidade: **renomear uma entrada só funciona depois de salvar o rascunho ao menos uma vez.**

1. Adicione **todas as sete entradas** primeiro (passo 2), mesmo que algum nome não fique perfeito ainda.
2. Clique em:

✅ **Salvar rascunho** (canto superior direito)

3. Só então volte a cada entrada e **edite o nome** no campo correspondente, corrigindo para os valores exatos da tabela acima.
4. Salve novamente após os ajustes.

> ⚠️ **Tentar renomear antes de salvar o rascunho pela primeira vez não funciona de forma confiável.** Sempre salve primeiro, depois ajuste os nomes.

---

## ✅ Checkpoint 1

Neste ponto, o nó **Quando um agente chama o fluxo** deve mostrar as sete entradas, todas do tipo Texto (ícone "AA"):

```text
RequesterName
RequesterEmail
ApproverEmail
StartDate
EndDate
Days
Details
```

---

## 4. Configurar o Respond to the agent

No nó **Respond to the agent**:

1. Clique em **+ Adicionar uma saída**.
2. Escolha o tipo **Texto**.
3. Nomeie a saída:

```text
Resultado
```

4. No valor da saída, escreva a confirmação de envio (sem afirmar aprovação, já que a decisão ainda não aconteceu):

```text
Pedido de ferias registrado com sucesso e enviado para aprovacao. Voce sera notificado assim que houver uma resposta.
```

> ✅ Como a aprovação vem **depois** deste nó, o agente responde ao colaborador imediatamente com esta confirmação — sem esperar ninguém clicar em nada.

---

## 5. Adicionar o nó de aprovação

Abaixo de **Respond to the agent**, clique no ícone **+** e busque por `aprovação`.

Selecione a ação:

✅ **Iniciar e aguardar uma aprovação**

> 🔧 **Correção:** este é o nó correto. Outros conectores com nome parecido (como "Criar uma aprovação") **não bloqueiam a execução do fluxo** e não servem para este cenário — precisamos que o fluxo espere a resposta antes de continuar, que é exatamente o que "Iniciar e aguardar uma aprovação" faz.

### 5.1 Tipo de aprovação

No campo **Tipo de aprovação**, selecione:

✅ **Aprovar/Rejeitar – Primeiro a responder**

> ℹ️ Essa opção significa que basta **uma pessoa** responder (aprovar ou rejeitar) para o fluxo continuar — coerente com termos apenas um aprovador por pedido neste laboratório.

### 5.2 Título

No campo **Título** *(obrigatório)*:

```text
Aprovação de ferias
```

### 5.3 Atribuído a

No campo **Atribuído a** *(obrigatório)*:

Selecione **uma pessoa específica do seu tenant** — você mesmo ou um colega ao lado.

> 💡 **Para efeito de treinamento**, este campo fica **fixo** (uma pessoa escolhida manualmente), não dinâmico. No exemplo de referência, foi selecionado um colega (`Wallacy Felipe | Cloud Target`) — substitua pela pessoa que você quiser usar como aprovador no seu teste.

> ⚠️ **Nota técnica:** a entrada `ApproverEmail` (criada no passo 2) continua existindo no fluxo, mas **não está sendo usada** neste campo enquanto ele estiver fixo. Se no futuro você quiser que o aprovador seja o e-mail informado pelo colaborador na conversa, troque o preenchimento de "Atribuído a" para usar o **conteúdo dinâmico** da entrada `ApproverEmail`.

### 5.4 Detalhes

No campo **Detalhes** *(opcional, aceita Markdown)*, monte um resumo do pedido usando o conteúdo dinâmico das entradas:

```text
**Solicitante:** RequesterName
**Periodo:** StartDate ate EndDate
**Dias:** Days
**Observacao:** Details
```

> 🔧 **Ainda não confirmado por print:** para inserir cada valor (`RequesterName`, `StartDate` etc.) dentro do campo, clique no ponto do texto onde ele deve entrar — o Power Automate normalmente abre um painel **Conteúdo dinâmico** à direita, listando as entradas do gatilho. Selecione cada uma no lugar correto. Se a interação for diferente na sua tela, me envie o print que eu corrijo.

### 5.5 Campos não utilizados

| Campo | Uso neste laboratório |
|---|---|
| **Link do item** | Deixar em branco — não há item externo a linkar |
| **Descrição do link do item** | Deixar em branco |
| **Parâmetros avançados** | Manter os padrões: **Habilitar notificações = Sim**, **Habilitar reatribuição = Sim** |

> ℹ️ O painel mostra "Mostrando 2 de 4" em Parâmetros avançados — existem outros dois parâmetros além desses. Não precisamos deles neste laboratório; os padrões são suficientes.

---

## ✅ Checkpoint 2

Neste ponto, o fluxo deve estar nesta ordem:

```text
Quando um agente chama o fluxo
  (7 entradas: RequesterName, RequesterEmail, ApproverEmail,
   StartDate, EndDate, Days, Details)
        ↓
Respond to the agent
  (saída: Resultado)
        ↓
Iniciar e aguardar uma aprovação
  Tipo: Aprovar/Rejeitar – Primeiro a responder
  Título: Aprovação de ferias
  Atribuído a: [pessoa fixa escolhida]
  Detalhes: resumo com conteúdo dinâmico
```

Salve o fluxo antes de continuar.

---

## 🔜 Próxima seção (pendente)

As próximas etapas — ainda não construídas neste documento — são:

- Verificar o **Resultado** da aprovação (condição comparando com o valor de aprovado/rejeitado).
- Notificar o **solicitante** (`RequesterEmail`) conforme o resultado.
- Publicar o fluxo e testar o tópico ponta a ponta.

Envie os próximos prints quando avançar nessa parte que eu continuo o documento.

---

> **Navegação:** Passo 3 de 3 · [⬅️ Voltar ao Passo 2](Classico-PASSO-2-TOPICO.md)