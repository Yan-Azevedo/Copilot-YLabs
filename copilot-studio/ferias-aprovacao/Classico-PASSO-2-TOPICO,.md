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
| 1 | `Qual a data de início das suas férias? (formato DD/MM/AAAA)` | **Resposta inteira do usuário** | `DataInicio` (tipo `string`) |
| 2 | `Qual a data de término das suas férias? (formato DD/MM/AAAA)` | **Resposta inteira do usuário** | `DataFim` (tipo `string`) |
| 3 | `Quantos dias você está solicitando?` | **Resposta inteira do usuário** | `QtdDias` (tipo `string`) |
| 4 | `Qual o e-mail do aprovador?` | E-mail *(confirmado indiretamente — o mapeamento no Passo 3 funcionou sem erro de tipo)* | `EmailAprovador` (tipo `string`) |
| 5 | `Deseja adicionar alguma observação? Se não quiser, pode responder "não".` | **Resposta inteira do usuário** | `Observacao` (tipo `string`) |

> 🔧 **Correção — todas as respostas de texto livre usam `Resposta inteira do usuário`.** As perguntas 1, 2 e 3 usavam originalmente os tipos **Data** e **Número**, que produzem variáveis do tipo `date` e `number`. Ao mapear essas variáveis como entrada do agent flow no Passo 3 (que espera todos os campos como **Texto**), isso gerou o erro:
>
> ```text
> Input variável 'StartDate' é do tipo incorreto: Date
> Input variável 'EndDate' é do tipo incorreto: Date
> Input variável 'Days' é do tipo incorreto: Number
> ```
>
> A correção é usar **Resposta inteira do usuário** em todas as perguntas de texto livre — assim, toda variável do tópico fica como `string`, igual ao tipo esperado pelo fluxo, e o erro desaparece.

> 💡 A pergunta 5 é **opcional** conforme as instruções do agente. Se seu ambiente permitir marcar a pergunta como não obrigatória, use esse recurso; caso contrário, oriente o colaborador a responder "não" ou "sem observação" no próprio texto da pergunta, como no exemplo acima.

### 5.1 Sobre o texto livre nas datas e na quantidade de dias

Como `DataInicio`, `DataFim` e `QtdDias` agora usam **Resposta inteira do usuário**, não há extração automática — a variável salva **exatamente o que o colaborador digitar**, sem validar se é uma data ou um número.

Por isso, o texto da pergunta precisa deixar o formato esperado bem claro (já incluído nas perguntas 1 e 2 acima: `formato DD/MM/AAAA`). Mesmo assim, teste antes de seguir:

1. Salve o tópico.
2. Abra o painel de **Teste** (lateral direita).
3. Digite uma mensagem simulando o início da conversa:

   ```text
   quero pedir ferias
   ```

4. Responda às perguntas de data com um valor no formato pedido, por exemplo `10/09/2026`.
5. Abra o ícone **Variáveis** e confira se `DataInicio`, `DataFim` e `QtdDias` aparecem preenchidas com o texto exato que você digitou.

> ⚠️ **Diferente do tipo Data, aqui não há garantia de formato.** Se o colaborador responder algo fora do padrão (`mês que vem`, `uns 10 dias`), a variável salva esse texto do jeito que veio — sem erro, mas também sem correção automática. O texto claro na pergunta é a única proteção disponível neste modelo.

---

## 6. Confirmar os dados com o colaborador

Depois do último nó de pergunta, adicione um nó **Enviar uma mensagem** com o resumo do pedido.

### 6.1 Montar a mensagem de resumo

⚠️ **Não digite o nome da variável entre chaves (ex.: `{DataFim}`) diretamente no texto.** Isso não funciona — o Copilot Studio não reconhece a variável assim e mostra o erro **"Identificador não reconhecido na expressão"** em vermelho abaixo da mensagem. A variável precisa ser **inserida pelo botão**, conforme os passos abaixo.

⚠️ **Não use apenas Enter para separar os rótulos.** Um Enter simples entre linhas não vira quebra de linha de verdade no chat — o texto sai todo corrido, um atrás do outro, mesmo parecendo separado no editor. Para os cinco dados, use a **lista com marcadores**, não parágrafos soltos.

1. Clique em **+ Adicionar nó** → **Enviar uma mensagem**.
2. Na caixa de texto do nó **Mensagem**, digite a primeira linha (esta fica como texto normal, fora da lista):

   ```text
   Confirme os dados do seu pedido:
   ```

3. Pressione **Enter** uma vez para pular linha.
4. Na barra de ferramentas do editor, clique no ícone de **lista com marcadores** (ao lado dos ícones B e I, o mesmo usado para tópicos com bullet). Isso inicia um item de lista.
5. Digite o rótulo do primeiro dado, **sem o valor**:

   ```text
   Data de início:
   ```

6. Com o cursor logo após o texto digitado, clique no ícone **{x}** (Adicionar variável) na barra de ferramentas, ao lado do ícone **fx**.
7. O painel lateral **Selecionar uma variável** abre, na aba **Personalizado**.
8. Use o campo **Pesquisar variáveis** e digite `DataInicio`, ou localize a variável na lista (cada uma mostra seu nome, a referência `Topic.NomeDaVariavel` e o tipo).
9. **Clique na variável `DataInicio`** para inseri-la. Ela aparece na mensagem como um "chip" cinza com o ícone **fx**, e não mais como texto solto.
10. Pressione **Enter** — como você está dentro da lista com marcadores, isso cria automaticamente o **próximo item da lista**, já no formato correto. Repita os passos 5 a 9 para os quatro dados restantes:

    | Rótulo a digitar | Variável a inserir pelo painel |
    |---|---|
    | `Data de término:` | `DataFim` |
    | `Quantidade de dias:` | `QtdDias` |
    | `Aprovador:` | `EmailAprovador` |
    | `Observação:` | `Observacao` |

Ao final, a mensagem deve ter esta aparência — a primeira linha como texto normal, e os cinco dados como itens de uma **lista com marcadores**, cada um em sua própria linha (os nomes em destaque são os chips inseridos pelo painel, não texto digitado):

```text
Confirme os dados do seu pedido:

• Data de início: [fx DataInicio]
• Data de término: [fx DataFim]
• Quantidade de dias: [fx QtdDias]
• Aprovador: [fx EmailAprovador]
• Observação: [fx Observacao]
```

> ✅ **Checkpoint:** teste no painel de Teste e confira se cada dado aparece em **sua própria linha** no chat — não tudo corrido num só parágrafo. Se sair corrido, confirme que você usou o ícone de lista com marcadores antes de digitar os rótulos, não apenas Enter.

> ✅ Também não deve aparecer nenhuma mensagem em vermelho do tipo "Identificador não reconhecido na expressão" abaixo do nó. Se aparecer, alguma variável ainda está como texto digitado — apague o trecho e insira novamente pelo botão **{x}**.

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

👉 [**Passo 3 — Criação do Fluxo**](Classico-PASSO-3-FLUXO.md)

---

## 8. Mapear as entradas do fluxo (ao voltar do Passo 3)

Depois de publicar o fluxo e clicar em **Voltar ao agente** (último passo do [Passo 3](Classico-PASSO-3-FLUXO.md)), o tópico volta a este editor com um novo nó **Ação** (ícone verde ⚡), logo abaixo da mensagem "Solicitação realizada com sucesso" — no lugar de onde você clicou em "Novo fluxo de agente".

> ℹ️ Este nó representa a chamada ao flow **Enviar Aprovacao de Ferias**. Ele mostra **"Entradas do Power Automate (6)"** — todas em vermelho e vazias (`Selecionar ou inserir um valor`) até serem preenchidas.

### 8.1 Preencher cada entrada

Para cada entrada, clique na caixa **"Selecionar ou inserir um valor"**. Um painel **Selecionar uma variável** abre à direita, com as abas **Personalizado**, **Sistema** e **Ambiente**.

| Entrada do fluxo | Aba a usar | Selecionar |
|---|---|---|
| `RequesterName (String)` | **Sistema** | `User.Email` |
| `ApproverEmail (String)` | Personalizado | `EmailAprovador` |
| `StartDate (String)` | Personalizado | `DataInicio` |
| `EndDate (String)` | Personalizado | `DataFim` |
| `Days (String)` | Personalizado | `QtdDias` |
| `Details (String)` | Personalizado | `Observacao` |

> ℹ️ **`RequesterName` usa a aba Sistema, não Personalizado.** As outras cinco entradas já aparecem prontas na aba Personalizado, porque são variáveis criadas neste próprio tópico.

> 💡 **Neste laboratório, `RequesterName` recebe o e-mail do usuário logado (`User.Email`), não o nome de exibição.** É uma escolha deliberada para simplificar — o campo guarda um e-mail, apesar do nome. Se preferir usar o nome de exibição real, selecione `User.DisplayName` no lugar.

### 8.2 Se aparecer erro de tipo

Se alguma entrada mostrar borda vermelha com uma mensagem como:

```text
Input variável 'StartDate' é do tipo incorreto: Date
Input variável 'EndDate' é do tipo incorreto: Date
Input variável 'Days' é do tipo incorreto: Number
```

A causa é a variável do tópico estar com tipo `date` ou `number`, enquanto o fluxo espera **Texto** em todos os campos.

**Correção:** volte à seção 5 deste documento e confirme que `DataInicio`, `DataFim` e `QtdDias` estão configuradas com **Resposta inteira do usuário** (não Data nem Número) — essa mudança já está refletida na tabela da seção 5. Depois de ajustar, salve o tópico novamente; o erro desaparece porque a variável passa a ser `string`, igual ao tipo esperado pelo fluxo.

### 8.3 Saída disponível

Abaixo das entradas, o nó mostra:

```text
Saídas (1)
resultado (string) = Resultado (string)
```

Esta é a mensagem de confirmação vinda do `Respond to the agent` do fluxo (Passo 3, seção 4). Não é usada em nenhuma mensagem deste tópico ainda — fica disponível para uso futuro, se você quiser substituir o texto fixo "Solicitação realizada com sucesso" por esta saída dinâmica.

---

## ✅ Checkpoint — mapeamento concluído

1. Salve o tópico.
2. Confirme que **não há mais bordas vermelhas** no nó Ação.
3. Publique o agente (aba Overview → Publish), se ainda não publicou.

---

## 🗂️ Estrutura esperada ao final desta etapa

```text
Tópico: Solicitar Ferias
│
├── Gatilho
│   └── O agente escolhe (descrição preenchida)
│
├── Perguntas (todas tipo "Resposta inteira do usuário", exceto EmailAprovador)
│   ├── DataInicio (string)
│   ├── DataFim (string)
│   ├── QtdDias (string)
│   ├── EmailAprovador (string, tipo E-mail)
│   └── Observacao (string)
│
├── Mensagem de resumo (variáveis inseridas via {x}, não digitadas)
├── Pergunta de confirmação → Confirmacao (tipo choice: Sim/Não)
│
├── Condição: Confirmacao é igual a Não
│   ├── Não                → Ir para etapa (volta à pergunta "Data de início")
│   └── Todas as outras    → Mensagem "Solicitação realizada com sucesso"
│                             └── Ação (chama o fluxo Enviar Aprovacao de Ferias)
│                                   ├── RequesterName  = User.Email (Sistema)
│                                   ├── ApproverEmail  = EmailAprovador
│                                   ├── StartDate      = DataInicio
│                                   ├── EndDate        = DataFim
│                                   ├── Days           = QtdDias
│                                   ├── Details        = Observacao
│                                   └── Saída: resultado (não usada ainda)
│
└── Salvo, sem erros, publicado
```

---

> ✅ **Resultado esperado:** tópico "Solicitar Ferias" criado, coletando os dados, confirmando com o colaborador, voltando ao início em caso de "Não" e, em caso de "Sim", encerrando com a mensagem "Solicitação realizada com sucesso" e chamando o fluxo de aprovação com todas as entradas mapeadas corretamente, sem erro de tipo.

---

> **Navegação:** Passo 2 de 3 · [⬅️ Voltar ao Passo 1](Classico-PASSO-1-AGENTE.md) · [Próximo → Passo 3: Criação do fluxo](Classico-PASSO-3-FLUXO.md)