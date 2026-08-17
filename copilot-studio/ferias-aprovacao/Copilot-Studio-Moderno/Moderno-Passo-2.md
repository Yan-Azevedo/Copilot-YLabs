## ⚙️ Passo 2 — Criação do Fluxo de Trabalho (Agent Flow) — Experiência Moderna

🎯 **Objetivo desta etapa:** construir o fluxo de trabalho que recebe os dados coletados pelo agente, confirma o recebimento e envia o pedido para aprovação de forma **assíncrona** — criado direto do bloco Ferramentas, sem passar por nenhum tópico.
**Navegação:** Passo 2 de 3 · ⬅️ Voltar ao Passo 1 (Moderno-Passo-1.md) · Próximo → Passo 3: Conectar e testar (Moderno-Passo-3.md)

> 🆕 **Diferença central para o Clássico:** no Clássico, o fluxo era criado de dentro do tópico. Aqui não há tópico — o fluxo é criado a partir do bloco Ferramentas do próprio agente Holiday Assist Moderno. A plataforma chama essa peça de "fluxo de trabalho" (workflow), mas o conceito é o mesmo agent flow.

---

### ✅ Status deste documento

Este passo cobre a construção completa do fluxo até a publicação — versão funcional mínima (registra o pedido e envia para aprovação). A conexão do fluxo como ferramenta e o mapeamento das entradas (IA generativa vs. valor fixo) acontecem no Passo 3. A condição de resultado e a notificação ao solicitante ficam como melhoria futura, descrita ao final.

---

### ⚙️ Decisões tomadas para este fluxo

| Decisão | Escolha |
|---|---|
| Posição da aprovação | Depois do Respond to the agent (assíncrono — sem risco de timeout) |
| Campo "Atribuído a" | Fixo, uma pessoa específica (para efeito de treinamento) |
| Quem pode ser o aprovador | Você mesmo (o criador) ou um colega ao lado — desde que no mesmo tenant |

ℹ️ **Por que assíncrono:** um fluxo chamado por um agente tem limite de 100 segundos para responder. Como a aprovação espera uma pessoa clicar — o que pode levar minutos —, ela precisa vir depois do Respond to the agent, nunca antes.

---

### ✅ Pré-requisitos

- Agente Holiday Assist Moderno criado e configurado, com conhecimento adicionado (ver Moderno-Passo-1.md).

---

### 1. Abrir o designer do fluxo (a partir de Ferramentas)

- No agente Holiday Assist Moderno, aba Criar, clique no bloco Ferramentas (coluna direita) — ou no ➕ ao lado dele.
- Abre o painel "Adicionar uma ferramenta", com as abas: Em destaque · Protocolo de Contexto do Modelo (MCP) · Conectores · Fluxos de trabalho.
- Clique na aba: Fluxos de trabalho.
- No canto superior direito do painel, clique no botão: ➕ Adicionar.

📌 **Atenção:** a aba "Fluxos de trabalho" também lista fluxos que já existem no ambiente (ex.: Enviar Aprovacao de Ferias do lab clássico, Save Summary). Não clique num existente — clique em "➕ Adicionar" para criar um novo.

Isso abre o designer do fluxo já com a estrutura inicial pronta, com o título provisório "Fluxo de trabalho sem título" (badge Rascunho) e dois nós conectados:

```
When an agent calls the flow   (gatilho — nome mantido em inglês pela plataforma)
        |
        v
Respond to the agent           (resposta — nome mantido em inglês pela plataforma)
```

ℹ️ **Layout do designer:** à esquerda, o painel "Adicionar" (Agente, Classificar, M365 Copilot, Revisão humana, Conector, Função, Variável, If/Else, Loop, Observação). À direita, o painel de propriedades do nó selecionado. No topo: abas Criar · Atividade · Monitoramento e os botões de salvar, testar e Publicar.

---

### 2. Adicionar as entradas do gatilho

Clique no nó When an agent calls the flow. No painel direito aparece:

- Tipo de gatilho: "Quando um agente chama o fluxo de trabalho — Disparar como uma ferramenta de um agente" (já vem correto, não altere).
- Entradas: com o botão ➕ Adicionar uma entrada.

Para cada entrada:

- Clique em ➕ Adicionar uma entrada.
- Selecione o tipo: Texto.
- Uma caixa à direita pode vir com um texto padrão (ex.: "Insira sua entrada aqui"). Apague esse texto se aparecer — não some sozinho.
- No campo de nome, digite o nome da entrada conforme a tabela.
- Repita para todas.

| # | Nome da entrada | Tipo |
|---|---|---|
| 1 | RequesterName | Texto |
| 2 | ApproverEmail | Texto |
| 3 | StartDate | Texto |
| 4 | EndDate | Texto |
| 5 | Days | Texto |
| 6 | Details | Texto |

📌 Todas as entradas são do tipo Texto, inclusive as datas e a quantidade de dias — os valores chegam do orquestrador como texto e são mais simples de tratar assim (mesmo princípio do Clássico).

ℹ️ **Por que só 6 entradas (sem RequesterEmail):** nesta versão base, o solicitante não é notificado por e-mail — o nome dele (RequesterName) vem automaticamente do usuário logado no Passo 3. Se depois quiser notificar quem pediu, adicione uma entrada RequesterEmail (ver melhoria futura ao final).

---

### 3. Nomear o fluxo

- Clique no título "Fluxo de trabalho sem título" (topo esquerdo, ao lado de Holiday Assist Moderno).
- Renomeie para:

```
Enviar Aprovacao Ferias Moderno
```

- Clique no ícone Salvar (topo direito) para gravar o rascunho.

📌 Nome com "Moderno" para não colidir com o fluxo do lab clássico ("Enviar Aprovacao de Ferias"), que aparece no mesmo ambiente (visto no painel de ferramentas).
💡 Se o título não ficar editável de primeira, clique em Salvar uma vez e tente renomear novamente.

---

### ✅ Checkpoint 1

O nó When an agent calls the flow deve mostrar as seis entradas, todas do tipo Texto:

```
RequesterName
ApproverEmail
StartDate
EndDate
Days
Details
```

---

### 4. Configurar o Respond to the agent

Clique no nó Respond to the agent:

- Adicione uma saída do tipo Texto.
- Nomeie a saída:

```
Resultado
```

- No valor da saída, escreva a confirmação de envio (sem afirmar aprovação, que ainda não aconteceu):

```
Pedido de ferias registrado com sucesso e enviado para aprovacao. Voce sera notificado assim que houver uma resposta.
```

✅ Como a aprovação vem depois deste nó, o agente responde ao colaborador imediatamente com esta confirmação — sem esperar ninguém clicar.

---

### 5. Configurar o nó "Iniciar e aguardar uma aprovação"

Clique no nó "Iniciar e aguardar uma aprovação de..." (badge "Precisa ser configurado") e preencha o painel Configurar da direita, de cima para baixo:

#### 5.1 Título (obrigatório)
```
Aprovação de ferias
```

#### 5.2 Texto sugerido (obrigatório)
Este campo é o texto que aparece para o aprovador. Preencha:
```
Aprovar ou rejeitar esta solicitação de férias.
```

#### 5.3 Atribuído a (obrigatório)
Insira o nome ou o e-mail da pessoa que vai aprovar.
- Se está testando sozinho, coloque o SEU próprio e-mail — assim o card chega na sua caixa.
- Deve ser alguém do mesmo tenant.

⚠️ Se apontar para outra pessoa, o card chega na caixa dela, não na sua. O fluxo roda mesmo assim — confira em Power Automate → Meus fluxos → Histórico de execuções antes de achar que falhou.

#### 5.4 Detalhes (opcional, aceita Markdown)
Monte o resumo do pedido. Digite o rótulo, depois clique no ícone de raio à direita do campo para inserir cada entrada como chip:
```
Solicitante: [RequesterName]
Periodo: [StartDate] e [EndDate]
Dias: [Days]
Observacao: [Details]
```

#### 5.5 Campos que ficam em branco
- Link do Item: deixar vazio.
- Descrição do Link do Item: deixar vazio.

#### 5.6 Parâmetros avançados
- O painel mostra "Showing 2 of 6". Clique em "Mostrar tudo" para ver os 6 parâmetros.
- Confirme que estão como padrão:
  - Habilitar notificações: true
  - Habilitar reatribuição: true
- Procure entre eles um campo de TIPO DE APROVAÇÃO (ex.: "Aprovar/Rejeitar – Primeiro a responder"). Se existir, selecione essa opção. Se NÃO aparecer, o conector já usa o modo aprovar/rejeitar padrão — pode seguir.

#### 5.7 Sobre o aviso de conexão
Se aparecer "A configuração da conexão não está disponível neste ambiente", siga normalmente — isso costuma se resolver ao publicar. Após publicar, se o nó ainda acusar erro de conexão, abra o nó e confirme/refaça a conexão do conector de Aprovações.

---

### ✅ Checkpoint 2

O fluxo deve estar nesta ordem:

```
When an agent calls the flow
  (6 entradas: RequesterName, ApproverEmail,
   StartDate, EndDate, Days, Details)
        |
        v
Respond to the agent
  (saída: Resultado)
        |
        v
Iniciar e aguardar uma aprovação
  Tipo: Aprovar/Rejeitar – Primeiro a responder
  Título: Aprovação de ferias
  Atribuído a: [pessoa fixa escolhida]
  Detalhes: resumo com chips inseridos via raio
```

---

### 6. Publicar o fluxo

- Clique em Salvar (topo direito) para garantir o rascunho gravado.
- Clique em Publicar (canto superior direito).
- Aguarde a confirmação de publicação.
- Volte ao editor do agente (breadcrumb "Holiday Assist Moderno" no topo esquerdo, ou feche o designer).

O fluxo Enviar Aprovacao Ferias Moderno agora existe e está pronto para ser conectado e mapeado como ferramenta — o que fazemos no Passo 3.

✅ Diferente do Clássico, aqui o fluxo ainda NÃO está mapeado ao agente. No moderno, o mapeamento das entradas (IA generativa vs. System.User.DisplayName) é feito no Passo 3, ao configurar a ferramenta.

---

### 🔜 Melhoria futura (opcional)

O que está publicado agora registra o pedido e envia para aprovação, mas ainda não trata o resultado (aprovado/recusado) nem notifica quem pediu. Pode ser adicionado depois, sem quebrar o que já funciona:

- Adicionar a entrada RequesterEmail ao gatilho.
- Nó If/Else comparando o resultado da aprovação.
- Notificação ao solicitante (RequesterEmail) com o resultado.

É só editar o fluxo publicado e publicar de novo — sem refazer nada.

**Navegação:** Passo 2 de 3 · ⬅️ Voltar ao Passo 1 (Moderno-Passo-1.md) · Próximo → Passo 3: Conectar e testar (Moderno-Passo-3.md)