# 🧭 Passo 2 — Criação do Tópico (Experiência Clássica)

> 🎯 **Objetivo desta etapa:** criar o tópico que conduz a conversa de solicitação de férias — do gatilho até a coleta e confirmação dos dados — e decidir como ele termina.

> **Navegação:** Passo 2 de 3 · [⬅️ Voltar ao Passo 1](Classico-PASSO-1-AGENTE.md) · [Etapa 1.1: Conhecimento](Classico-PASSO-1.1-AGENTE-Conhecimento.md) · [Próximo → Passo 3: Criação do fluxo](Classico-PASSO-3-FLUXO.md)

Este tópico pode terminar de **duas formas**. Você decide qual construir agora:

| Final | O que faz | Requer |
|---|---|---|
| **A — Com fluxo** | Chama o Power Automate, salva o pedido, envia para aprovação e retorna o resultado ao colaborador | [Passo 3 — Criação do fluxo](Classico-PASSO-3-FLUXO.md) |
| **B — Somente mensagem** | Encerra a conversa respondendo **"Solicitação realizada"**, sem acionar aprovação real | Nada além deste passo |

Neste documento construímos o tópico **até o Final B**, mais simples — e deixamos o gancho pronto para evoluir para o **Final A** (com fluxo real de aprovação), que começamos a construir no Passo 3.

---

## ✅ Pré-requisitos

- Agente **Holiday Assist** criado (ver [Passo 1](Classico-PASSO-1-AGENTE.md)).
- Conhecimento adicionado (ver [Passo 1.1](Classico-PASSO-1.1-AGENTE-Conhecimento.md)) — opcional para este passo, mas recomendado antes de publicar.

---

## 1. Acessar a área de Tópicos

1. No agente **Holiday Assist**, clique na aba:

✅ **Tópicos**

2. Você verá os tópicos já existentes do sistema, por exemplo:

```text
Obrigado
Recomeçar
Saudação
Tchau
```

> ℹ️ Esses tópicos são criados automaticamente pelo Copilot Studio e não precisam ser editados nesta etapa.

---

## 2. Criar um novo tópico em branco

1. Clique no botão:

✅ **+ Adicionar um tópico**

2. No menu que abre, selecione:

✅ **A partir de documento em branco**

> ℹ️ A outra opção, **Adicionar a partir da descrição com o Copilot**, gera o tópico automaticamente a partir de um texto — não é usada neste laboratório, pois construímos o tópico manualmente para controlar cada etapa.

---

## 3. Nomear o tópico

O editor abre com o tópico chamado **Sem título**.

1. Clique no nome **Sem título** (ou no ícone **Detalhes**, no canto superior direito).
2. Renomeie o tópico para:

```text
Solicitar Ferias
```

> 📌 Sem acentos no nome do tópico, seguindo o mesmo padrão usado no nome do agente e do agent flow.

---

## 4. Configurar o gatilho

O tópico já nasce com um nó **Gatilho**, configurado como **O agente escolhe**.

1. No campo **Descreva o que o tópico faz**, substitua o texto de exemplo por:

```text
Este tópico conduz uma solicitação de férias. Deve ser usado quando o colaborador quiser pedir férias, solicitar folga ou registrar um período de ausência. Ele coleta data de início, data de término, quantidade de dias, e-mail do aprovador e uma observação opcional, confirma os dados com o colaborador e envia o pedido para aprovação.
```

> 🎯 **Esta descrição é o que faz o orquestrador escolher este tópico.** Como o gatilho é "O agente escolhe" (orquestração generativa), não há lista de frases fixas — o agente decide entrar aqui comparando a mensagem do usuário com esta descrição. Quanto mais clara e específica, melhor o roteamento.

---

## 5. Adicionar as perguntas de coleta de dados

Abaixo do nó **Gatilho**, clique no ícone **+** (Adicionar nó) e selecione:

✅ **Fazer uma pergunta**

Repita esse processo para criar **cinco** nós de pergunta, um para cada dado. Para cada nó:

- Preencha a **caixa de mensagem** com a pergunta.
- Em **Identificar**, selecione o tipo de informação.
- Em **Salvar resposta do usuário como**, o Copilot Studio cria um nome padrão (ex.: `Var1`). **Clique em cima desse nome** para abrir o painel lateral **Propriedades de Variável**, e no campo **Nome da variável** digite o nome definido na tabela abaixo.

| # | Pergunta (caixa de mensagem) | Identificar | Nome da variável |
|---|---|---|---|
| 1 | `Qual a data de início das suas férias?` | **Data** *(confirmado — "Datas, dias da semana e meses em relação a um ponto no tempo extraídos como uma cadeia de caracteres")* | `DataInicio` (tipo `date`) |
| 2 | `Qual a data de término das suas férias?` | **Data** | `DataFim` (tipo `date`) |
| 3 | `Quantos dias você está solicitando?` | Número | `QtdDias` |
| 4 | `Qual o e-mail do aprovador?` | E-mail *(ou Texto, se essa opção não existir no seu ambiente — ainda não confirmado por print)* | `EmailAprovador` |
| 5 | `Deseja adicionar alguma observação? Se não quiser, pode responder "não".` | **Resposta inteira do usuário** *(confirmado — "Nenhuma extração de entidade; salvo como está")* | `Observacao` |

> ✅ **Tipo `Resposta inteira do usuário` confirmado** para a observação. Diferente das demais perguntas, aqui não há extração de entidade — o texto digitado pelo colaborador é salvo exatamente como foi escrito.

> ✅ **Tipo `Data` confirmado.** No seletor **Escolher as informações a serem identificadas**, use a opção **Data** (não "Data e hora" nem "Data e hora sem fuso horário"). Ela extrai a data da resposta do usuário como referência temporal, e a variável salva fica com o tipo `date`.

> ⚠️ **Confira os demais tipos no seu ambiente** — **Data** e **Resposta inteira do usuário** foram validados por print nesta etapa. Os nomes exatos das entidades de **Número** e **E-mail** ainda podem variar de tenant para tenant.

> 💡 A pergunta 5 é **opcional** conforme as instruções do agente. Se seu ambiente permitir marcar a pergunta como não obrigatória, use esse recurso; caso contrário, oriente o colaborador a responder "não" ou "sem observação" no próprio texto da pergunta, como no exemplo acima.

### 5.1 Verificar se as datas estão sendo preenchidas

O tipo **Data** extrai a informação a partir de texto livre — por isso, antes de seguir, **confirme que a variável realmente recebe um valor** quando o colaborador responde.

1. Salve o tópico.
2. Abra o painel de **Teste** (lateral direita).
3. Digite uma mensagem simulando o início da conversa, por exemplo:

   ```text
   quero pedir ferias
   ```

4. Quando o agente perguntar a data de início, responda com uma data em formato claro:

   ```text
   10/09/2026
   ```

5. Abra o ícone **Variáveis** (topo do editor de tópico, ao lado de Comentários) e confira se `DataInicio` aparece **preenchida** — não vazia e não em branco.
6. Repita para `DataFim`.

**Se a variável ficar vazia:**

- Confirme que o tipo **Data** foi realmente selecionado no nó (não "Data e hora" nem outro tipo).
- Teste respostas mais explícitas primeiro (`10/09/2026`) antes de testar frases livres (`dia 10 do mês que vem`) — respostas em formato de data completo têm reconhecimento mais confiável.
- Se mesmo assim a variável não preencher, verifique se há um **prompt de reprompt** configurado (mensagem que o agente envia quando não reconhece a resposta) e ajuste o texto da pergunta para orientar melhor o formato esperado, por exemplo incluindo `(formato DD/MM/AAAA)` na própria pergunta.

> ⚠️ **Não avance para a coleta dos demais dados até confirmar que `DataInicio` e `DataFim` preenchem corretamente.** Como o colaborador é quem digita a data, uma falha silenciosa de extração aqui quebra o resto do fluxo sem gerar erro visível.

---

## 6. Confirmar os dados com o colaborador

Depois do último nó de pergunta, adicione um nó **Enviar uma mensagem** com o resumo do pedido.

### 6.1 Montar a mensagem de resumo

⚠️ **Não digite o nome da variável entre chaves (ex.: `{DataFim}`) diretamente no texto.** Isso não funciona — o Copilot Studio não reconhece a variável assim e mostra o erro **"Identificador não reconhecido na expressão"** em vermelho abaixo da mensagem. A variável precisa ser **inserida pelo botão**, conforme os passos abaixo.

1. Clique em **+ Adicionar nó** → **Enviar uma mensagem**.
2. Na caixa de texto do nó **Mensagem**, digite a primeira linha:

   ```text
   Confirme os dados do seu pedido:
   ```

3. Pressione **Enter** duas vezes para pular uma linha e digite o rótulo do primeiro dado, **sem o valor**:

   ```text
   Data de início:
   ```

4. Com o cursor logo após o texto digitado, clique no ícone **{x}** (Adicionar variável) na barra de ferramentas do editor, ao lado do ícone **fx**.
5. O painel lateral **Selecionar uma variável** abre, na aba **Personalizado**.
6. Use o campo **Pesquisar variáveis** e digite `DataInicio`, ou localize a variável na lista (cada uma mostra seu nome, a referência `Topic.NomeDaVariavel` e o tipo, por exemplo `date` ou `string`).
7. **Clique na variável `DataInicio`** para inseri-la. Ela aparece na mensagem como um "chip" cinza com o ícone **fx**, e não mais como texto solto.
8. Pressione **Enter** para pular linha e repita o processo (passos 3 a 7) para os quatro dados restantes:

   | Rótulo a digitar | Variável a inserir pelo painel |
   |---|---|
   | `Data de término:` | `DataFim` |
   | `Quantidade de dias:` | `QtdDias` |
   | `Aprovador:` | `EmailAprovador` |
   | `Observação:` | `Observacao` |

Ao final, a mensagem deve ter esta aparência (os nomes em destaque são os chips inseridos pelo painel, não texto digitado):

```text
Confirme os dados do seu pedido:

Data de início: [fx DataInicio]
Data de término: [fx DataFim]
Quantidade de dias: [fx QtdDias]
Aprovador: [fx EmailAprovador]
Observação: [fx Observacao]
```

> ✅ **Checkpoint:** não deve aparecer nenhuma mensagem em vermelho do tipo "Identificador não reconhecido na expressão" abaixo do nó. Se aparecer, alguma variável ainda está como texto digitado — apague o trecho e insira novamente pelo botão **{x}**.

### 6.2 Adicionar a pergunta de confirmação

Abaixo da mensagem de resumo, adicione um nó **Fazer uma pergunta** com o texto:

```text
Está tudo correto? Posso enviar para aprovação?
```

Em **Identificar**, selecione **Opções de múltipla escolha** *(confirmado por print)*.

Em **Opções para o usuário**, adicione as duas opções:

```text
Sim
Não
```

Em **Salvar resposta do usuário como**, o Copilot Studio cria o nome padrão `Var1` (tipo `choice`). **Clique nesse nome** para abrir o painel lateral **Propriedades de Variável** e, no campo **Nome da variável**, renomeie para:

```text
Confirmacao
```

> ✅ Confira no painel: **Tipo** deve mostrar `choice` e a **Referência** deve mostrar o próprio nó da pergunta ("Está tudo correto?...").

---

### 6.3 Adicionar a condição

O nó de pergunta com **Opções de múltipla escolha** não ramifica sozinho — é preciso adicionar um nó de **Condição** logo abaixo para decidir o caminho.

1. Clique no ícone **+** abaixo do nó de pergunta.
2. Selecione **Adicionar uma condição**.
3. No nó **Condição**, configure:

   | Campo | Valor |
   |---|---|
   | Variável | `Confirmacao` |
   | Operador | **é igual a** |
   | Valor | `Não` |

Isso cria **dois ramos**:

- **Não** — a condição explícita que acabamos de configurar.
- **Todas as outras condições** — cobre a resposta **Sim** (e qualquer valor inesperado).

---

### 6.4 Configurar o ramo "Não" — voltar para o início da coleta

No ramo **Não**:

1. Clique no ícone **+** logo abaixo.
2. No menu que abre, passe o mouse sobre **Gerenciamento de tópicos**.
3. No submenu, selecione:

✅ **Ir para etapa**

4. Escolha o nó da **primeira pergunta do tópico** — "Qual a data de início das suas férias?" — como destino.

Isso faz a conversa **voltar para o início da coleta de dados**, dentro do mesmo tópico, sem reiniciar a saudação nem sair do fluxo de férias.

> ⚠️ **Não use um tópico de sistema aqui.** O caminho correto é **Gerenciamento de tópicos → Ir para etapa**, apontando para o nó de pergunta dentro deste mesmo tópico — não um redirecionamento para outro tópico.

---

## ✅ Checkpoint — antes de decidir o final

Neste ponto, o tópico deve ter, na ordem:

```text
Gatilho (O agente escolhe)
  ↓
Pergunta: Data de início      → DataInicio
  ↓
Pergunta: Data de término     → DataFim
  ↓
Pergunta: Quantidade de dias  → QtdDias
  ↓
Pergunta: E-mail do aprovador → EmailAprovador
  ↓
Pergunta: Observação          → Observacao
  ↓
Mensagem: resumo do pedido
  ↓
Pergunta: confirmar? (Sim/Não) → Confirmacao
  ↓
Condição: Confirmacao é igual a Não
  ├── Não                    → Ir para etapa (volta à pergunta "Data de início")
  └── Todas as outras (Sim)  → [PONTO DE DECISÃO — passo 7]
```

Se algo estiver fora dessa ordem, ajuste antes de continuar.

---

## 7. Fechar o tópico — Final B (somente mensagem)

No ramo **Todas as outras condições** (que cobre a resposta **Sim**), adicione um nó **Enviar uma mensagem**:

```text
Solicitação realizada com sucesso
```

Clique em:

✅ **Salvar**

> ✅ **Este é o final que estamos construindo neste passo.** O tópico coleta, confirma e encerra — sem enviar a aprovação de verdade.

---

## 🔀 Sobre o Final A (com fluxo) — próximo passo

Se você quiser o pedido realmente enviado para aprovação, não é necessário refazer o tópico. A partir da mensagem **"Solicitação realizada com sucesso"**, no mesmo ramo:

1. Clique no ícone **+** logo abaixo da mensagem.
2. Selecione **Adicionar uma ferramenta**.
3. No painel **Adicionar uma ferramenta**, aba **Ferramentas básicas**, selecione:

✅ **Novo fluxo de agente** — *"Permitir que seu agente conclua as tarefas automaticamente"*

> 🚨 **MUITO IMPORTANTE — salve o tópico antes de clicar em "Novo fluxo de agente".** Esse botão abre o construtor de fluxo e navega para fora do editor de tópico. Se o tópico não estiver salvo, as alterações feitas até aqui (perguntas, condição, mensagem) podem se perder. Clique em **Salvar** primeiro, confirme que não há erros, e só então clique em **Novo fluxo de agente**.

A partir daqui, a construção do fluxo continua no:

👉 [**Passo 3 — Criação do Fluxo**](Classico-PASSO-3-FLUXO.md) *(vamos construir agora)*

---

## 🗂️ Estrutura esperada ao final desta etapa

```text
Tópico: Solicitar Ferias
│
├── Gatilho
│   └── O agente escolhe (descrição preenchida)
│
├── Perguntas
│   ├── DataInicio (tipo Data — preenchimento verificado)
│   ├── DataFim (tipo Data — preenchimento verificado)
│   ├── QtdDias
│   ├── EmailAprovador
│   └── Observacao
│
├── Mensagem de resumo (variáveis inseridas via {x}, não digitadas)
├── Pergunta de confirmação → Confirmacao (tipo choice: Sim/Não)
│
├── Condição: Confirmacao é igual a Não
│   ├── Não                → Ir para etapa (volta à pergunta "Data de início")
│   └── Todas as outras    → Mensagem "Solicitação realizada com sucesso" (Final B)
│                             └── (opcional) Adicionar ferramenta → Novo fluxo de agente → Passo 3
│
└── Salvo
```

---

> ✅ **Resultado esperado:** tópico "Solicitar Ferias" criado, coletando os cinco dados, confirmando com o colaborador, voltando ao início em caso de "Não" e encerrando com a mensagem "Solicitação realizada com sucesso" em caso de "Sim". Pronto para evoluir para o Final A no Passo 3.

---

> **Navegação:** Passo 2 de 3 · [⬅️ Voltar ao Passo 1](Classico-PASSO-1-AGENTE.md) · [Próximo → Passo 3: Criação do fluxo](Classico-PASSO-3-FLUXO.md)