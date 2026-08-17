## 🤖 Passo 1 — Criação do Agente + Conhecimento (Experiência Moderna)

🎯 **Objetivo desta etapa:** criar o agente **Holiday Assist Moderno** no novo Copilot Studio (*New experience*), escrever as instruções e adicionar a **Política de Férias — Contoso** como conhecimento — tudo em uma única tela, **sem tópico**, apoiado na **orquestração generativa**.
**Navegação:** Passo 1 de 3 · Próximo → Passo 2: Criação do fluxo (Moderno-Passo-2.md)

> 🆕 **Diferença central para o Clássico:** aqui **não existe tópico** nem nós de pergunta. Quem conduz a conversa, pergunta os dados que faltam e decide chamar a ferramenta é o **orquestrador generativo**. Por isso, as **Instruções** (e, mais adiante, a **descrição da ferramenta**) substituem o trabalho que no Clássico exigia um tópico inteiro. O **Conhecimento**, que no Clássico era uma etapa separada, aqui é apenas um **bloco na mesma tela** — por isso foi incorporado a este passo.

---

### ⚠️ Pré-requisitos de ambiente

- Botão **New experience** (canto superior direito da lista de agentes) **ativado** — é ele que abre este novo editor sem tópicos.
- ℹ️ **Cobrança:** agentes criados **após 03/08** entram no modelo de **preços baseados em créditos**. Este agente moderno se enquadra — sem impacto para o laboratório, mas vale citar na apresentação.

---

### 1. Abrir o novo agente

Na página **Agentes**, clique em **Novo agente**.
A *New experience* abre **direto no editor**, já com o nome provisório **"Agente Sem Título"** — **não há** tela intermediária de nome/solução como no Clássico.

📌 **Mapa da tela:** no **topo**, as abas **Criar · Visualizar · Avaliar · Monitorar**; no **centro**, o **Nome** e o campo **Instruções**; na **coluna direita**, os blocos **Modelo, Skills, Ferramentas, Conhecimento, Connected agents e Memória**. Todo o Passo 1 acontece na aba **Criar**.

---

### 2. Nomear o agente

Clique sobre **"Agente Sem Título"** (centro da tela) e renomeie para:
```
Holiday Assist Moderno
```
📌 Nome **diferente** do "Holiday Assist Classic" para os dois conviverem na mesma lista sem confusão.

---

### 3. Definir o Modelo (coluna direita)

No bloco **Modelo**, o padrão do tenant aparece como **Claude Opus 5**.
- Mantenha **Claude Opus 5**, ou abra o **dropdown** e escolha outro modelo disponível se sua organização exigir.

ℹ️ O modelo influencia o estilo das respostas; o **planejamento das ações** é feito pelo orquestrador generativo (confirmado na etapa 6).

---

### 4. Escrever as Instruções (campo central — coração deste modelo)

No campo **Instruções**, apague o texto de exemplo e cole:
```
Você é o Holiday Assist Moderno, um agente corporativo que ajuda colaboradores a solicitar férias em ambiente de treinamento.

Quando o colaborador demonstrar intenção de pedir férias (ex.: "quero pedir férias", "preciso solicitar folga"), conduza a conversa coletando os dados necessários, UMA pergunta por vez, de forma cordial e objetiva, até ter: data de início, data de término, quantidade de dias, e-mail do aprovador e uma observação opcional. Não pergunte o nome do solicitante — ele é obtido automaticamente do usuário logado.

Antes de enviar, apresente um resumo do pedido e peça a confirmação do colaborador. Somente após o "sim", envie o pedido para aprovação usando a ferramenta de envio de aprovação de férias.

Atue sempre com tom profissional, claro e cordial. Deixe explícito, quando fizer sentido, que este é um fluxo de treinamento/simulação.

O aprovador deve ser uma pessoa do mesmo tenant. Se o colaborador estiver sozinho ou quiser simular, permita que use o próprio e-mail como aprovador.

Não consulte saldo real de férias. Não valide regras trabalhistas. Não registre férias em sistema de RH. Não prometa efetivação oficial. Não afirme que o pedido foi aprovado ou recusado antes que isso aconteça — a decisão do aprovador chega por e-mail, fora desta conversa.
```
🔧 **Por que as instruções são mais "carregadas" aqui do que no Clássico:** no Clássico elas foram *enxugadas* para não competir com o tópico. Aqui **não há tópico** — então a coleta "um dado por vez", o resumo e a confirmação precisam viver **nas instruções**, porque é o orquestrador (guiado por elas) que executa esse papel.

⚠️ A frase *"use a ferramenta de envio de aprovação de férias"* só terá efeito depois do Moderno-Passo-3.md, quando o fluxo for conectado como ferramenta. Deixe já escrita — ela ativa sozinha assim que a ferramenta existir.

---

### 5. Adicionar o Conhecimento (bloco na coluna direita)

No modelo moderno, o **Conhecimento** é um bloco na mesma tela — não uma etapa separada. São três movimentos: **remover a web**, **preparar o documento** e **adicioná-lo**.

#### 5.1 Remover a busca na web
O bloco **Conhecimento** já vem com o chip **"Pesquisar em todos os sites"** ativado por padrão.
- Clique no **❌** do chip **"Pesquisar em todos os sites"** para **removê-lo**.
🎯 Assim como no Clássico, este agente deve responder **apenas** pelo documento de política — **não** pela web. Com a busca ligada, ele poderia sair do escopo do treinamento.

#### 5.2 Preparar o documento
- Abra o documento de conteúdo:<br>👉 **Política de Férias — Contoso** (../Politica-de-Ferias-Contoso.md)
- **Copie todo o conteúdo**, cole em um novo documento do Word e salve, por exemplo:
```
Politica_de_Ferias_Contoso.docx
```
ℹ️ O nome do arquivo é livre. No exemplo das telas, foi usado `Politica_de_Ferias_Contoso.docx`.

#### 5.3 Adicionar o documento
- No bloco **Conhecimento**, clique no **➕**.
- Na janela **Adicionar conhecimento**, escolha **carregar arquivo** (upload).
- Selecione o arquivo **`Politica_de_Ferias_Contoso.docx`** e confirme.
ℹ️ Também é possível carregar do **OneDrive**, **SharePoint** ou outras fontes (Dataverse etc.). Para este cenário, use o **upload local**.

#### 5.4 Aguardar o processamento
Após adicionar, o documento aparece com um status (ex.: *Em andamento / Processing*). Aguarde até indicar **pronto** — só então o agente passa a usar o documento.
⚠️ **Não teste antes de o processamento concluir.** Se perguntar sobre a política enquanto o status ainda estiver "Em andamento", o agente pode dizer que não encontrou a informação — parece erro, mas é só a indexação não ter terminado.

---

### 6. Demais blocos da coluna direita

Confira — **nenhum exige ação nesta etapa**:

| Bloco | Estado esperado | Ação |
|---|---|---|
| **Skills** | vazio | Não adicionar (recurso avançado, fora do escopo) |
| **Ferramentas** | vazio | Não adicionar agora — o fluxo entra nos Passos 2 e 3 |
| **Connected agents** | vazio | Não adicionar |
| **Memória (Preview)** | **Desligada** | Manter desligada neste laboratório |

ℹ️ **Descrição e Solução:** não aparecem nesta tela principal; ficam no menu **⋯** (Configurações/Detalhes), no canto superior direito. São **opcionais** para o laboratório.

---

### 7. Confirmar a Orquestração Generativa (crítico)

Abra o menu **⋯ → Configurações** e confirme:

| Item | Estado esperado |
|---|---|
| **Orquestração de IA generativa** | **Ativada** (padrão em agentes novos) |
| **Usar informações da Web** | Desativado |
| **Nível de moderação** | Alta |
| **Respostas sem fundamentação** | Ativado |
| **Carregamento de arquivos / Intérprete de código** | Desativado |

🚨 **Se a orquestração generativa estiver desligada, este modelo não funciona.** Sem tópico e sem orquestração, o agente não conduz a conversa nem chama a ferramenta. Confirme que está **Ativada** antes de seguir.

---

### 8. Teste rápido do conhecimento (aba Visualizar)

- Abra a aba **Visualizar** (topo da tela).
- Faça uma pergunta que **só** o documento responde, por exemplo:
```
Com quantos dias de antecedência preciso solicitar minhas férias?
```
- Confirme que o agente responde com base na **Política de Férias — Contoso**, e não com uma resposta genérica.

---

### 🗂️ Estrutura esperada ao final da etapa

```
Agente: Holiday Assist Moderno   (aba: Criar)
│
├── Nome ............................ Holiday Assist Moderno
├── Instruções ..................... preenchidas (tom + coleta 1 a 1 + resumo + limites)
│
├── Modelo ......................... Claude Opus 5 (ou padrão do tenant)
├── Skills ......................... vazio
├── Ferramentas .................... vazio  (fluxo entra nos Passos 2 e 3)
├── Conhecimento
│   ├── "Pesquisar em todos os sites" ... REMOVIDO ❌
│   └── Politica_de_Ferias_Contoso.docx . adicionado ✅ (status: pronto)
├── Connected agents ............... vazio
├── Memória (Preview) .............. desligada
│
└── Configurações (⋯)
    ├── Orquestração generativa ..... Ativada ✅ (crítico)
    ├── Usar informações da Web ..... Desativado
    ├── Moderação ................... Alta
    └── Respostas sem fundamentação . Ativado
```

✅ **Resultado esperado:** agente **Holiday Assist Moderno** criado no novo Studio, com nome, modelo, instruções, conhecimento da política ativo (web desligada) e orquestração generativa confirmada — **sem nenhum tópico**. Pronto para receber a ferramenta de aprovação nos próximos passos.

**Navegação:** Passo 1 de 3 · Próximo → Passo 2: Criação do fluxo (Moderno-Passo-2.md)