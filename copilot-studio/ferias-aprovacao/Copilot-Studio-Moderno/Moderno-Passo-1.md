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

📌 **Mapa da tela:** no **topo**, as abas **Criar · Visualizar · Avaliar · Monitorar**; no **centro**, o **Nome** e o campo **Instruções**; na **coluna direita**, os blocos **Modelo, Skills, Ferramentas, Conhecimento e Connected agents**. Todo o Passo 1 acontece na aba **Criar**.

---

### 2. Nomear o agente

Clique sobre **"Agente Sem Título"** (centro da tela) e renomeie para:
```
Holiday Assist Moderno
```
📌 Nome **diferente** do "Holiday Assist Classic" para os dois conviverem na mesma lista sem confusão.

---

### 3. Definir o Modelo (coluna direita)

No bloco **Modelo**, escolha no dropdown:
```
GPT-5 Chat
```
ℹ️ Caso seu ambiente não ofereça essa opção, mantenha o **modelo padrão do tenant**. O modelo influencia o estilo das respostas; o **planejamento das ações** é feito pelo orquestrador generativo (nativo nesta experiência).

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
Após adicionar, o documento aparece como um chip no bloco **Conhecimento** (ex.: `Politica_de_Ferias_Contoso.docx`). Aguarde o processamento concluir — só então o agente passa a usar o documento.
⚠️ **Não teste antes de o processamento concluir.** Se perguntar sobre a política enquanto ainda estiver processando, o agente pode dizer que não encontrou a informação — parece erro, mas é só a indexação não ter terminado.

---

### 6. Demais blocos da coluna direita

⚠️ **Não há botão a clicar aqui — é apenas conferência.** Deixe todos vazios nesta etapa:

| Bloco | Estado esperado | Ação |
|---|---|---|
| **Skills** | vazio | Não adicionar (recurso avançado, fora do escopo) |
| **Ferramentas** | vazio | Não adicionar agora — o fluxo entra nos Passos 2 e 3 |
| **Connected agents** | vazio | Não adicionar |

ℹ️ Dependendo da versão/rollout do seu tenant, pode aparecer também um bloco **Memória (Preview)**. Se existir, deixe **desligado**; se não aparecer, ignore — não é necessário neste laboratório.

---

### 7. Confirmar as configurações de IA (via menu ⋯)

Abra o menu **⋯** (canto superior direito) → **Configurações**. A janela **Configurações do agente** tem 4 abas:

| Aba | O que contém |
|---|---|
| **Detalhes do agente** | Nome do esquema, Solução, Idioma primário — **somente leitura** (definidos na criação) |
| **IA e comportamento** | Orquestração (conexão de agentes) e Nível de moderação |
| **Segurança e acesso** | Autenticação e permissões |
| **Saudação e prompts** | Saudação e prompts sugeridos |

Clique na aba **IA e comportamento**. Ela tem apenas **duas** configurações:

| Configuração | Estado recomendado |
|---|---|
| **Permitir que outros agentes se conectem** | Deixe **desligado** (não precisamos que outros agentes usem este) |
| **Nível de moderação** | **Máximo** (mais seguro para demonstração) |

✅ **Não há botão de "orquestração generativa" para ativar.** No novo Copilot Studio, a **orquestração generativa já é o comportamento padrão e nativo** — é ela que faz o agente conduzir a conversa e chamar ferramentas **sem tópico**. Por isso não existe toggle: já vem ligada de fábrica.

ℹ️ **Onde ficam os controles de conhecimento:** "Usar informações da Web" e a fundamentação **não ficam nesta aba** — são controlados no próprio bloco **Conhecimento** (etapa 5, ao remover o chip *"Pesquisar em todos os sites"*).

ℹ️ **Idioma primário (aba Detalhes do agente):** pode aparecer como **"English"** e **não pode ser alterado** após a criação. Não é problema — como as **Instruções estão em português**, o agente responde em PT.

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
├── Modelo ......................... GPT-5 Chat (ou padrão do tenant)
├── Skills ......................... vazio
├── Ferramentas .................... vazio  (fluxo entra nos Passos 2 e 3)
├── Conhecimento
│   ├── "Pesquisar em todos os sites" ... REMOVIDO ❌
│   └── Politica_de_Ferias_Contoso.docx . adicionado ✅
├── Connected agents ............... vazio
│
└── Configurações (⋯) → IA e comportamento
    ├── Orquestração generativa ..... nativa (sem toggle) ✅
    ├── Permitir outros agentes ..... desligado
    └── Nível de moderação .......... Máximo
```

✅ **Resultado esperado:** agente **Holiday Assist Moderno** criado no novo Studio, com nome, modelo, instruções, conhecimento da política ativo (web desligada) e orquestração generativa nativa — **sem nenhum tópico**. Pronto para receber a ferramenta de aprovação nos próximos passos.

**Navegação:** Passo 1 de 3 · Próximo → Passo 2: Criação do fluxo (Moderno-Passo-2.md)