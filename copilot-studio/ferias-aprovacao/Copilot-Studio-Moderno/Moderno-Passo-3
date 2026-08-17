## 🔗 Passo 3 — Conectar a Ferramenta, Mapear Entradas e Testar (Experiência Moderna)

🎯 **Objetivo desta etapa:** conectar o fluxo **Enviar Aprovacao Ferias Moderno** ao agente como ferramenta, mapear as entradas (IA generativa vs. valor fixo), refinar a descrição da ferramenta e **testar a orquestração ponta a ponta** — fechando o laboratório sem nenhum tópico.
**Navegação:** Passo 3 de 3 · ⬅️ Voltar ao Passo 2 (Moderno-Passo-2.md)

> 🆕 **Este passo substitui todo o "Passo 2 — Tópico" do Clássico.** No Clássico, era preciso um tópico inteiro (nós de pergunta, variáveis, condição, mapeamento manual). Aqui, o mesmo resultado vem de **3 coisas**: uma **boa descrição da ferramenta**, o **mapeamento das entradas** e as **instruções do agente** (Passo 1). O orquestrador faz o resto.

---

### ✅ Pré-requisitos

- Agente **Holiday Assist Moderno** criado, com instruções e conhecimento (ver Moderno-Passo-1.md).
- Fluxo **Enviar Aprovacao Ferias Moderno** publicado (ver Moderno-Passo-2.md).
- Você clicou em **Voltar ao agente** ao final do Passo 2 e está de volta ao editor do agente, aba **Criar**.

---

### 1. Conectar o fluxo como ferramenta

Ao voltar do designer do fluxo, o **Holiday Assist Moderno** já reconhece o fluxo publicado. Se ele ainda não estiver listado no bloco **Ferramentas**:

- Na aba **Criar**, bloco **Ferramentas** (coluna direita), clique no **➕**.
- No painel **Adicionar ferramenta**, aba **Fluxo** (*Flow*), localize e selecione:
  ✅ **Enviar Aprovacao Ferias Moderno**
- Confirme para adicionar a ferramenta ao agente.

✅ Ao final, o fluxo aparece como um item dentro do bloco **Ferramentas** do agente.

---

### 2. Ajustar o nome e a descrição da ferramenta (decisivo)

Clique sobre a ferramenta **Enviar Aprovacao Ferias Moderno** para abrir seus detalhes.

🎯 **A descrição da ferramenta é o que faz o orquestrador decidir chamá-la.** No modelo sem tópico, não existe gatilho fixo — o agente compara a intenção do usuário com esta descrição. Quanto mais clara, melhor o acionamento.

- **Nome de exibição** (pode manter ou ajustar para leitura):
```
Enviar aprovação de férias
```
- **Descrição** — substitua o texto padrão por:
```
Use esta ferramenta quando o colaborador quiser solicitar férias, pedir folga ou registrar um período de ausência e o pedido precisar ser enviado para aprovação de uma pessoa do mesmo tenant. A ferramenta registra o pedido e dispara a aprovação por e-mail. Só a acione depois de já ter coletado data de início, data de término, quantidade de dias, e-mail do aprovador e a observação, e após o colaborador confirmar o resumo.
```

💡 **Opcional — blindar a demo:** se a plataforma oferecer a opção *"Perguntar antes de executar / exigir confirmação"* nesta ferramenta, **ative-a**. Assim o agente sempre pede o "ok" final antes de disparar — evita que ele chame o fluxo cedo demais durante a apresentação.

---

### 3. Mapear as entradas da ferramenta (o coração do modelo)

Ainda nos detalhes da ferramenta, abra a seção **Entradas** (*Inputs*). As **6 entradas** criadas no Passo 2 aparecem aqui. Para cada uma, defina **como o valor é preenchido**:

- **Preencher dinamicamente com IA** → o agente pergunta ao colaborador na conversa e preenche sozinho.
- **Valor personalizado / fixo** → você define uma fórmula ou variável de sistema.

Configure conforme a tabela:

| # | Entrada | Como preencher | Valor |
|---|---|---|---|
| 1 | **RequesterName** | Valor personalizado | `System.User.DisplayName` |
| 2 | **ApproverEmail** | Preencher com IA | *(o agente pergunta o e-mail do aprovador)* |
| 3 | **StartDate** | Preencher com IA | *(o agente pergunta a data de início)* |
| 4 | **EndDate** | Preencher com IA | *(o agente pergunta a data de término)* |
| 5 | **Days** | Preencher com IA | *(o agente pergunta a quantidade de dias)* |
| 6 | **Details** | Preencher com IA | *(o agente pergunta a observação — opcional)* |

🎯 **Por que RequesterName é fixo e o resto é IA:** o nome do solicitante não deve ser digitado — ele vem automaticamente de quem está logado, via **`System.User.DisplayName`**. Os demais dados são justamente o que o agente coleta na conversa, então ficam como **"Preencher com IA"**.

ℹ️ **Diferença-chave para o Clássico:** lá, cada entrada era mapeada manualmente a uma variável do tópico (`DataInicio`, `EmailAprovador` etc.). Aqui **não há variáveis de tópico** — o próprio orquestrador extrai os valores da conversa e preenche. Isso é o que elimina o tópico inteiro.

💡 Se preferir usar o e-mail do usuário logado em vez do nome, troque `System.User.DisplayName` por `System.User.Email` na entrada 1.

---

### 4. Confirmar/refinar as Instruções do agente

Volte ao campo **Instruções** (centro da tela). A versão escrita no Passo 1 já orienta o agente a coletar 1 a 1, resumir e só então acionar a ferramenta. Faça um ajuste fino para **amarrar o nome exato da ferramenta**:

- Confirme que a instrução menciona **acionar a ferramenta de aprovação após a confirmação**. Se quiser ser explícito, ajuste a frase-chave para:
```
Somente após o "sim" do colaborador, acione a ferramenta "Enviar aprovação de férias" para registrar o pedido e disparar a aprovação.
```

⚠️ **Publique depois de qualquer ajuste.** Salvar as instruções não basta — a versão testada só muda após **Publicar**.

---

### 5. Publicar o agente

- Clique em **Publicar** (canto superior direito).
- Confirme na janela de publicação.

🚨 **Sem publicar, o teste usa a versão anterior.** Todo ajuste de instrução, descrição de ferramenta ou mapeamento de entrada só vale após publicar.

---

### 6. Testar a orquestração ponta a ponta (aba Visualizar)

Abra a aba **Visualizar** e rode o roteiro de demonstração:

1. Inicie com a intenção:
```
quero pedir férias
```
2. **Confira:** o agente deve **conduzir a conversa, uma pergunta por vez** (data de início → término → dias → e-mail do aprovador → observação) — sem despejar tudo de uma vez, sem tópico.
3. Responda cada pergunta (use datas no formato DD/MM/AAAA).
4. **Confira:** o agente apresenta um **resumo** e pede **confirmação**.
5. Responda:
```
sim
```
6. **Confira:** o agente responde com a mensagem de confirmação do fluxo (*"Pedido de ferias registrado com sucesso..."*).
7. **Confira a aprovação:** o card de **Aprovação de ferias** chega por e-mail / Teams para a pessoa definida em **Atribuído a** (você mesmo, se estiver testando sozinho).

---

### 🔧 Se algo não funcionar

| Sintoma | Causa provável | Correção |
|---|---|---|
| O agente **não chama** a ferramenta | Descrição da ferramenta fraca ou instrução vaga | Reforce a **descrição** (seção 2) e a instrução de acionamento (seção 4); republique |
| O agente **despeja todas as perguntas** de uma vez | Instrução não enfatiza "uma pergunta por vez" | Ajuste as instruções (Passo 1, seção 4); republique |
| O agente chama a ferramenta **cedo demais** | Falta a confirmação antes de executar | Ative *"perguntar antes de executar"* (seção 2, dica) |
| **RequesterName vazio** no card | Entrada 1 não mapeada corretamente | Confirme `System.User.DisplayName` na entrada 1 (seção 3) |
| Você **não recebe** o card de aprovação | "Atribuído a" aponta para outra pessoa | Coloque você mesmo em **Atribuído a** (Passo 2, seção 5.3); confira o Histórico de execuções no Power Automate |
| Alterações **não têm efeito** | Agente não republicado | Clique em **Publicar** (seção 5) |

---

### 🗂️ Estrutura esperada ao final do laboratório

```
Agente: Holiday Assist Moderno   (Publicado)
│
├── Instruções ..................... coleta 1 a 1 + resumo + confirmação + acionar ferramenta
├── Conhecimento ................... Politica_de_Ferias_Contoso.docx (web desligada)
├── Orquestração generativa ........ Ativada
│
└── Ferramentas
    └── Enviar aprovação de férias  →  fluxo "Enviar Aprovacao Ferias Moderno"
        ├── Descrição ............... clara e específica (aciona por intenção)
        └── Entradas
            ├── RequesterName ....... System.User.DisplayName (fixo)
            ├── ApproverEmail ....... Preencher com IA
            ├── StartDate ........... Preencher com IA
            ├── EndDate ............. Preencher com IA
            ├── Days ................ Preencher com IA
            └── Details ............. Preencher com IA
```

✅ **Resultado esperado:** o **Holiday Assist Moderno** conduz sozinho toda a conversa de férias, coleta os dados um a um, confirma, aciona o fluxo e dispara a aprovação — **sem um único tópico**. Fim do laboratório moderno em **3 passos**.

---

### 🏁 Comparativo final — Clássico × Moderno

| Aspecto | Clássico | Moderno |
|---|---|---|
| Nº de passos | 3 + etapa 1.1 (4 arquivos) | **3 arquivos** |
| Coleta dos dados | Tópico com 5 nós de pergunta | Instruções + orquestrador |
| Mapeamento de entradas | Manual (variável a variável) | IA generativa + 1 valor fixo |
| Nome do solicitante | User.Email mapeado no tópico | `System.User.DisplayName` na entrada |
| Peça mais trabalhosa | O tópico (~6k tokens de doc) | **Deixou de existir** |

**Navegação:** Passo 3 de 3 · ⬅️ Voltar ao Passo 2 (Moderno-Passo-2.md)