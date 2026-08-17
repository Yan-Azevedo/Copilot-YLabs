# 🧭 Passo 2 — Criação do Tópico (Experiência Clássica)

> 🎯 **Objetivo desta etapa:** criar o tópico que conduz a conversa de solicitação de férias — do gatilho até a coleta e confirmação dos dados — e decidir como ele termina.

> **Navegação:** Passo 2 de 3 · [⬅️ Voltar ao Passo 1](Classico-PASSO-1-AGENTE.md) · [Etapa 1.1: Conhecimento](Classico-PASSO-1.1-AGENTE-Conhecimento.md) · [Próximo → Passo 3: Criação do fluxo](Classico-PASSO-3-FLUXO.md)

Este tópico pode terminar de **duas formas**. Você decide qual construir agora:

| Final | O que faz | Requer |
|---|---|---|
| **A — Com fluxo** | Chama o Power Automate, salva o pedido, envia para aprovação e retorna o resultado ao colaborador | [Passo 3 — Criação do fluxo](Classico-PASSO-3-FLUXO.md) |
| **B — Somente mensagem** | Encerra a conversa respondendo **"Solicitação realizada"**, sem acionar aprovação real | Nada além deste passo |

Neste documento construímos o tópico **até o ponto de decisão** e fechamos com o **Final B**, mais simples. Se você quiser o **Final A**, monte o Final B normalmente e depois volte para trocá-lo pelo nó de ação, seguindo o **Passo 3**.

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
- Em **Salvar resposta do usuário como**, renomeie a variável para o nome indicado.

| # | Pergunta (caixa de mensagem) | Identificar | Nome da variável |
|---|---|---|---|
| 1 | `Qual a data de início das suas férias?` | Data e hora | `DataInicio` |
| 2 | `Qual a data de término das suas férias?` | Data e hora | `DataFim` |
| 3 | `Quantos dias você está solicitando?` | Número | `QtdDias` |
| 4 | `Qual o e-mail do aprovador?` | E-mail *(ou Texto, se essa opção não existir no seu ambiente)* | `EmailAprovador` |
| 5 | `Deseja adicionar alguma observação? Se não quiser, pode responder "não".` | Texto | `Observacao` |

> ⚠️ **Confira os tipos disponíveis em Identificar no seu ambiente** — os nomes exatos das entidades podem variar de tenant para tenant. O importante é: datas como tipo de data, dias como número, e-mail validado se disponível, observação como texto livre.

> 💡 A pergunta 5 é **opcional** conforme as instruções do agente. Se seu ambiente permitir marcar a pergunta como não obrigatória, use esse recurso; caso contrário, oriente o colaborador a responder "não" ou "sem observação" no próprio texto da pergunta, como no exemplo acima.

---

## 6. Confirmar os dados com o colaborador

Depois do último nó de pergunta, adicione:

1. Um nó **Enviar uma mensagem** com o resumo do pedido:

```text
Confirme os dados do seu pedido:

Data de início: {DataInicio}
Data de término: {DataFim}
Quantidade de dias: {QtdDias}
Aprovador: {EmailAprovador}
Observação: {Observacao}
```

2. Em seguida, um nó **Fazer uma pergunta** para confirmar:

```text
Está tudo correto? Posso enviar para aprovação?
```

Em **Identificar**, selecione **Opções de múltipla escolha** com as opções:

```text
Sim
Não
```

> 🔧 **Não sei confirmar o nome exato deste tipo de entidade na sua tela** (pode aparecer como "Opções de múltipla escolha" ou "Lista de opções", dependendo da versão). Ajuste conforme o que aparecer no seu ambiente.

3. Isso cria automaticamente um **caminho condicional** para cada resposta:
   - **Não** → adicione um nó de **Redirecionamento** (ou **Ir para outro tópico**) apontando para o tópico de sistema **Recomeçar**.
   - **Sim** → siga para o passo 7.

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
Pergunta: confirmar? (Sim/Não)
  ├── Não → Recomeçar (tópico de sistema)
  └── Sim → [PONTO DE DECISÃO — passo 7]
```

Se algo estiver fora dessa ordem, ajuste antes de continuar.

---

## 7. Fechar o tópico — Final B (somente mensagem)

No ramo **Sim**, adicione um nó **Enviar uma mensagem**:

```text
Solicitação realizada.
```

Clique em:

✅ **Salvar**

> ✅ **Este é o final que estamos construindo neste passo.** O tópico coleta, confirma e encerra — sem enviar a aprovação de verdade.

---

## 🔀 Sobre o Final A (com fluxo)

Se no futuro você quiser o pedido realmente enviado para aprovação, **não é necessário refazer o tópico**. Basta:

1. Voltar a este tópico.
2. No ramo **Sim**, remover o nó de mensagem **"Solicitação realizada"**.
3. No lugar dele, adicionar um nó de **Ação** chamando o agent flow criado no [**Passo 3 — Criação do fluxo**](Classico-PASSO-3-FLUXO.md).
4. Mapear as variáveis `DataInicio`, `DataFim`, `QtdDias`, `EmailAprovador` e `Observacao` como entradas do flow.
5. Adicionar um nó de mensagem final usando a saída do flow como resposta ao colaborador.

Essa troca é feita **quando o Passo 3 for construído** — não é necessária agora.

---

## 🗂️ Estrutura esperada ao final desta etapa

```text
Tópico: Solicitar Ferias
│
├── Gatilho
│   └── O agente escolhe (descrição preenchida)
│
├── Perguntas
│   ├── DataInicio
│   ├── DataFim
│   ├── QtdDias
│   ├── EmailAprovador
│   └── Observacao
│
├── Mensagem de resumo
├── Confirmação (Sim/Não)
│   ├── Não → Recomeçar
│   └── Sim → Mensagem "Solicitação realizada" (Final B)
│
└── Salvo
```

---

> ✅ **Resultado esperado:** tópico "Solicitar Ferias" criado, coletando os cinco dados, confirmando com o colaborador e encerrando com a mensagem "Solicitação realizada".

---

> **Navegação:** Passo 2 de 3 · [⬅️ Voltar ao Passo 1](Classico-PASSO-1-AGENTE.md) · [Próximo → Passo 3: Criação do fluxo](Classico-PASSO-3-FLUXO.md)