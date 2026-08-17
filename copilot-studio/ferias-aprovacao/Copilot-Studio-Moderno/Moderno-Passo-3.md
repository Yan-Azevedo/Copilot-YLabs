## 🔗 Passo 3 — Conectar a Ferramenta, Mapear Entradas e Testar (Experiência Moderna)

🎯 **Objetivo desta etapa:** conectar o fluxo (ferramenta "Modern Aproval Holiday") ao agente, mapear as entradas (IA vs. valor fixo), ajustar a descrição da ferramenta e testar a orquestração ponta a ponta — fechando o laboratório sem nenhum tópico.
**Navegação:** Passo 3 de 3 · ⬅️ Voltar ao Passo 2 (Moderno-Passo-2.md)

> 🆕 **Este passo substitui todo o "Passo 2 — Tópico" do Clássico.** No Clássico, era preciso um tópico inteiro (nós de pergunta, variáveis, condição, mapeamento manual). Aqui, o mesmo resultado vem de 3 coisas: uma boa descrição da ferramenta, o mapeamento das entradas e as Instruções do agente (Passo 1). O orquestrador faz o resto.

---

### ✅ Pré-requisitos

- Agente Holiday Assist Moderno criado, com instruções e conhecimento (ver Moderno-Passo-1.md).
- Fluxo publicado no Passo 2 (ver Moderno-Passo-2.md).
- Você voltou ao editor do agente, aba Criar.

---

### 1. Confirmar a ferramenta conectada

Ao publicar o fluxo e voltar ao agente, ele normalmente já aparece sozinho no bloco Ferramentas (coluna direita) — no exemplo, como "Modern Aproval Holiday".

Se por algum motivo NÃO aparecer:

- Na aba Criar, clique no bloco Ferramentas ou no ➕ ao lado.
- No painel "Adicionar uma ferramenta", clique na aba Fluxos de trabalho.
- Localize e clique no seu fluxo na lista.
- Confirme para adicioná-lo.

📌 Cuidado para não selecionar o fluxo do lab clássico (Enviar Aprovacao de Ferias) — os dois aparecem na mesma lista.

---

### 2. Ajustar o nome e a descrição da ferramenta (decisivo)

Clique sobre a ferramenta no bloco Ferramentas. Abre a janela "Detalhes do fluxo de trabalho", com três abas na lateral esquerda: Detalhes · Entradas · Saídas.

Na aba Detalhes:

- Nome (nome de exibição da ferramenta), por exemplo:
```
Modern Aproval Holiday
```
- Descrição — este texto é o que faz o orquestrador decidir chamar a ferramenta. Preencha:
```
Use esta ferramenta quando o colaborador quiser solicitar férias, pedir folga ou registrar um período de ausência e o pedido precisar ser enviado para aprovação de uma pessoa do mesmo tenant. A ferramenta registra o pedido e dispara a aprovação por e-mail. Só a acione depois de já ter coletado data de início, data de término, quantidade de dias, e-mail do aprovador e a observação, e após o colaborador confirmar o resumo.
```

🎯 No modelo sem tópico não existe gatilho fixo — o agente compara a intenção do usuário com esta descrição. Quanto mais clara, melhor o acionamento.

Clique em Salvar.

---

### 3. Mapear as entradas da ferramenta (o coração do modelo)

Na mesma janela, clique na aba Entradas. As 6 entradas criadas no Passo 2 aparecem aqui, uma a uma. Para cada entrada há a pergunta "Como isso é preenchido?", com dois botões:

- IA: o agente preenche o valor a partir da conversa/contexto.
- Valor: um valor fixo (literal) que você digita — NÃO há seletor de variáveis de sistema neste ambiente.

⚠️ Importante: neste ambiente, a opção "Valor" só permite digitar um texto literal (o campo pede uma string obrigatória). Não existe um seletor de System.User.DisplayName aqui. Por isso, mesmo o nome do solicitante é preenchido por IA.

Configure conforme a tabela:

| # | Entrada | Como é preenchido | Descrição a colocar (campo Descrição) |
|---|---|---|---|
| 1 | RequesterName | IA | Nome do colaborador que está logado e fazendo a solicitação. Use o nome do usuário atual; não pergunte. |
| 2 | ApproverEmail | IA | E-mail da pessoa que vai aprovar o pedido, informado pelo colaborador. |
| 3 | StartDate | IA | Data de início das férias (formato DD/MM/AAAA). |
| 4 | EndDate | IA | Data de término das férias (formato DD/MM/AAAA). |
| 5 | Days | IA | Quantidade de dias solicitados. |
| 6 | Details | IA | Observação opcional do colaborador sobre o pedido. |

Para cada entrada: selecione IA, preencha a Descrição da tabela e clique em Salvar.

🎯 Por que tudo em IA: como não há seletor de variável de sistema para o nome do solicitante, deixamos RequesterName em IA com uma descrição que orienta o agente a usar o nome do usuário logado (sem perguntar). Os demais campos são o que o agente coleta na conversa. A Descrição de cada entrada é o que guia o orquestrador a preencher o valor certo.

💡 Alternativa para uma demo 100% previsível: em vez de IA, você pode marcar RequesterName como Valor e digitar um nome fixo (ex.: seu próprio nome). Funciona sempre, mas fica "chumbado" num único nome — menos elegante para treino ao vivo.

ℹ️ Diferença-chave para o Clássico: lá, cada entrada era mapeada manualmente a uma variável do tópico (DataInicio, EmailAprovador etc.). Aqui não há variáveis de tópico — o próprio orquestrador extrai os valores da conversa e preenche, guiado pela Descrição de cada entrada. Isso é o que elimina o tópico inteiro.

---

### 4. Confirmar/refinar as Instruções do agente

Volte ao campo Instruções (centro da tela). A versão do Passo 1 já orienta a coletar 1 a 1, resumir e só então acionar a ferramenta. Faça um ajuste fino para amarrar o nome exato da ferramenta:

```
Somente após o "sim" do colaborador, acione a ferramenta "Modern Aproval Holiday" para registrar o pedido e disparar a aprovação. O nome do solicitante é o do usuário logado — não pergunte esse dado.
```

⚠️ Publique depois de qualquer ajuste. Salvar as instruções não basta — a versão testada só muda após Publicar.

---

### 5. Publicar o agente

- Clique em Publicar (canto superior direito).
- Confirme na janela de publicação.

🚨 Sem publicar, o teste usa a versão anterior. Todo ajuste de instrução, descrição de ferramenta ou mapeamento de entrada só vale após publicar.

---

### 6. Testar a orquestração ponta a ponta (aba Visualizar)

Abra a aba Visualizar e rode o roteiro:

1. Inicie com a intenção:
```
quero pedir férias
```
2. Confira: o agente conduz a conversa, uma pergunta por vez (início → término → dias → e-mail do aprovador → observação) — sem despejar tudo de uma vez, sem tópico, e sem perguntar seu nome.
3. Responda cada pergunta (datas no formato DD/MM/AAAA).
4. Confira: o agente apresenta um resumo e pede confirmação.
5. Responda:
```
sim
```
6. Confira: o agente responde com a mensagem de confirmação do fluxo ("Pedido de ferias registrado com sucesso...").
7. Confira a aprovação: o card "Aprovação de ferias" chega por e-mail / Teams para a pessoa definida em "Atribuído a" no Passo 2 (você mesmo, se testando sozinho).

---

### 🔧 Se algo não funcionar

| Sintoma | Causa provável | Correção |
|---|---|---|
| O agente não chama a ferramenta | Descrição da ferramenta fraca ou instrução vaga | Reforce a descrição (seção 2) e a instrução de acionamento (seção 4); republique |
| O agente despeja todas as perguntas de uma vez | Instrução não enfatiza "uma pergunta por vez" | Ajuste as Instruções (Passo 1); republique |
| O agente pergunta o nome do solicitante | Descrição da entrada RequesterName ausente/fraca | Confirme a Descrição da entrada 1 (seção 3) orientando a usar o usuário logado |
| RequesterName sai vazio ou errado no card | Entrada 1 sem descrição clara | Reforce a Descrição da entrada 1, ou use a alternativa "Valor" com nome fixo |
| Você não recebe o card de aprovação | "Atribuído a" aponta para outra pessoa | Coloque você mesmo em "Atribuído a" (Passo 2, seção 5.3); confira o Histórico no Power Automate |
| Alterações não têm efeito | Agente não republicado | Clique em Publicar (seção 5) |

---

### 🗂️ Estrutura esperada ao final do laboratório

```
Agente: Holiday Assist Moderno   (Publicado)
|
├── Instruções ..................... coleta 1 a 1 + resumo + confirmação + acionar ferramenta
├── Conhecimento ................... Politica_de_Ferias_Contoso.docx (web desligada)
├── Orquestração generativa ........ nativa
|
└── Ferramentas
    └── Modern Aproval Holiday  ->  fluxo publicado no Passo 2
        ├── Descrição ............... clara e específica (aciona por intenção)
        └── Entradas (todas por IA, guiadas por Descrição)
            ├── RequesterName ....... IA (usa o usuário logado; não pergunta)
            ├── ApproverEmail ....... IA
            ├── StartDate ........... IA
            ├── EndDate ............. IA
            ├── Days ................ IA
            └── Details ............. IA
```

✅ **Resultado esperado:** o Holiday Assist Moderno conduz sozinho toda a conversa de férias, coleta os dados um a um, confirma, aciona o fluxo e dispara a aprovação — sem um único tópico. Fim do laboratório moderno em 3 passos.

---

### 🏁 Comparativo final — Clássico × Moderno

| Aspecto | Clássico | Moderno |
|---|---|---|
| Nº de passos | 3 + etapa 1.1 (4 arquivos) | 3 arquivos |
| Coleta dos dados | Tópico com 5 nós de pergunta | Instruções + orquestrador |
| Mapeamento de entradas | Manual (variável a variável) | Por IA, guiado pela Descrição de cada entrada |
| Nome do solicitante | User.Email mapeado no tópico | IA (usa o usuário logado) |
| Peça mais trabalhosa | O tópico (documento extenso) | Deixou de existir |

**Navegação:** Passo 3 de 3 · ⬅️ Voltar ao Passo 2 (Moderno-Passo-2.md)