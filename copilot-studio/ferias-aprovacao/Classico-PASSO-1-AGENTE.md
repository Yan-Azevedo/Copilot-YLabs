# 🤖 Passo 1 — Criação e Configuração Inicial do Agente

> 🎯 **Objetivo desta etapa:** criar o agente Holiday Assist no Microsoft Copilot Studio e deixar todas as configurações iniciais prontas antes da criação do tópico.

> **Navegação:** Passo 1 de 3 · [Próximo → Passo 2: Criação do tópico](PASSO-2-TOPICO.md)

---

## 1. Criar o agente

Na página inicial do Copilot Studio, clique em:

```text
+ Criar agente em branco
```

---

## 2. Nomear o agente

Na janela **Nomeie seu agente**:

1. No campo **Insira o nome agente**, informe:

```text
Holiday Assist
```

2. Expanda **Configurações do agente (opcional)** e confira:

| Campo | Valor |
|---|---|
| **Linguagem** | Português (Brasil) |
| **Solução** | selecione a solução desejada (no exemplo: `YSolution`) |
| **Nome do esquema** | preenchido automaticamente a partir do nome |

3. Clique em:

```text
Criar
```

> ℹ️ **Solução** e **Nome do esquema** são obrigatórios (marcados com `*`). O nome do esquema é gerado sozinho; só ajuste se sua organização exigir um padrão específico.

---

## 3. Configurar o agente na Visão geral

Após a criação, o Copilot Studio abre a aba **Visão geral**. Configure os itens **nesta ordem**.

### 3.1 Nome

Já preenchido na etapa anterior. Para revisar, use **Editar** no cartão **Detalhes**.

```text
Holiday Assist
```

### 3.2 Imagem

No cartão **Detalhes**, clique em **Editar** e defina a imagem/ícone do agente.

> ℹ️ Nenhuma imagem específica foi definida para este material. Use o ícone padrão ou envie um de sua escolha — é opcional.

### 3.3 Descrição

No campo **Descrição**, informe:

```text
O Holiday Assist é um agente conversacional voltado para ambiente corporativo e treinamentos práticos. Ele ajuda colaboradores a solicitar férias de forma simples, guiada e padronizada, coletando os dados essenciais do pedido e encaminhando a solicitação para aprovação de uma pessoa indicada no mesmo tenant.

O agente atua como uma interface conversacional entre o colaborador e o fluxo de aprovação, reduzindo mensagens informais, evitando solicitações incompletas e demonstrando como processos internos podem ser estruturados com recursos nativos do Microsoft 365.
```

> ℹ️ O campo aceita até **1024 caracteres**. Esta descrição usa cerca de 568.

### 3.4 Modelo

Na seção **Selecionar o modelo do seu agente**, escolha no dropdown:

```text
GPT-5 Chat
```

Caso seu ambiente não apresente esta opção, mantenha o modelo padrão do tenant.

### 3.5 Instruções

Na seção **Instruções**, clique em **Editar** e informe:

```text
Você é o Holiday Assist, um agente corporativo especializado em conduzir solicitações simples de férias em ambiente de treinamento.

Seu objetivo é receber pedidos de férias em linguagem natural, coletar os dados necessários, confirmar as informações com o colaborador, acionar o fluxo de aprovação e retornar o resultado final da decisão.

Atue com tom profissional, claro, cordial e objetivo. Evite linguagem excessivamente informal. A experiência deve ser simples para qualquer colaborador entender.

Quando o usuário demonstrar intenção de pedir férias, como "quero pedir férias", "preciso solicitar férias" ou frases similares, conduza a conversa coletando obrigatoriamente os seguintes dados:
1. Data de início das férias.
2. Data de término das férias.
3. Quantidade de dias solicitados.
4. E-mail do aprovador.
5. Observação opcional do colaborador.

Antes de acionar a aprovação, apresente um resumo do pedido e confirme os dados com o usuário.
O resumo deve conter:
- Data de início.
- Data de término.
- Quantidade de dias.
- Aprovador informado.
- Observação, se houver.

Caso algum dado obrigatório esteja ausente, solicite apenas a informação faltante, sem reiniciar toda a conversa.

O aprovador deve ser uma pessoa do mesmo tenant. Se o usuário informar que está sozinho ou que deseja simular a aprovação, permita que ele use o próprio e-mail como aprovador.

Não consulte saldo real de férias.
Não valide regras trabalhistas.
Não registre férias em sistema de RH.
Não prometa efetivação oficial do pedido.

Deixe claro, quando necessário, que este agente representa uma simulação ou fluxo de treinamento.

Após o retorno da aprovação, informe o resultado ao colaborador de forma objetiva.
Se aprovado, responda:
"Seu pedido de férias foi aprovado. Período solicitado: [data de início] a [data de término], total de [quantidade de dias] dias."
Se recusado, responda:
"Seu pedido de férias foi recusado pelo aprovador informado. Você pode revisar as informações e enviar uma nova solicitação, se necessário."
Se ocorrer erro no fluxo ou ausência de resposta da aprovação, informe que não foi possível concluir a solicitação e oriente o usuário a revisar os dados informados ou tentar novamente.
```

> ℹ️ Pode aparecer **1 Warning** em **Agent status**. É normal nesta fase — clique em **Review** para ver o alerta; ele costuma se resolver após publicar o agente.

---

## 4. Demais itens da Visão geral

Estes itens aparecem mais abaixo na mesma tela. **Nenhum deles exige configuração nesta etapa** — apenas confira se estão como abaixo:

| Item | Estado esperado | Ação |
|---|---|---|
| **Conhecimento** | vazio | Não adicionar fontes |
| **Pesquisa na Web** | Desabilitado(a) | Manter desabilitado |
| **Ferramentas** | vazio | Não adicionar (o Power Automate entra no Passo 3) |
| **Work IQ** | Habilitado | Ver observação abaixo |
| **Gatilhos** | vazio | Não adicionar |
| **Agentes** | vazio | Não adicionar |
| **Tópicos** | apenas os do sistema (Obrigado, Recomeçar, Saudação) | O tópico de férias é criado no Passo 2 |
| **Solicitações sugeridas** | vazio | Não adicionar |

> ⚠️ **Sobre o Work IQ:** no print ele está **Habilitado**. É a camada que conecta o agente a dados do Microsoft 365 (e-mails, calendário, Teams, arquivos) e possui **cobrança por uso**. Para este agente de treinamento, que não precisa desses dados, você pode **desabilitá-lo** para manter o escopo mínimo — ou mantê-lo como no print, se preferir. Não é obrigatório para o funcionamento do fluxo de férias.

---

## 5. Configurações do agente

Abra as **Configurações** do agente e deixe cada item conforme abaixo (estado dos prints).

### Orquestração

```text
Usar a orquestração de IA generativa para as respostas dos agentes?
→ Sim (respostas dinâmicas, usando ferramentas e conhecimentos disponíveis)
```

### Agentes conectados

```text
Permitir que outros agentes se conectem e usem este → Ativado
```

### Modelo

```text
Continuar usando Modelos desativados? → Desativado
```

### Respostas

```text
Formatação da resposta → deixar em branco
```

### Moderação

```text
Nível de moderação de conteúdo → Alta
```

### Comentários do Usuário

```text
Coletar reações do usuário a mensagens do agente → Ativado
Aviso de isenção de responsabilidade → deixar em branco
```

### Conhecimento

```text
Permitir respostas sem fundamentação → Ativado
Usar informações da Web → Desativado
```

### Recursos de processamento de arquivos

```text
Carregamentos de arquivo → Desativado
Intérprete de código → Desativado
```

### Pesquisar

```text
Fundamentação de gráfico do locatário com pesquisa semântica → Ativado
```

---

## 🗂️ Estrutura esperada ao final da etapa

```text
Agente: Holiday Assist
│
├── Detalhes
│   ├── Nome ......................... Holiday Assist
│   ├── Imagem ....................... opcional
│   └── Descrição .................... preenchida
│
├── Modelo .......................... GPT-5 Chat
├── Instruções ...................... preenchidas
│
├── Visão geral
│   ├── Conhecimento ................. vazio
│   ├── Pesquisa na Web .............. Desabilitado
│   ├── Ferramentas .................. vazio
│   ├── Work IQ ...................... Habilitado (ou desabilitado, ver nota)
│   ├── Gatilhos ..................... vazio
│   ├── Agentes ...................... vazio
│   ├── Tópicos ...................... apenas do sistema
│   └── Solicitações sugeridas ....... vazio
│
└── Configurações
    ├── Orquestração ................. Generativa (Sim)
    ├── Agentes conectados ........... Ativado
    ├── Modelos desativados .......... Desativado
    ├── Formatação da resposta ....... em branco
    ├── Moderação .................... Alta
    ├── Comentários do Usuário ....... Ativado
    ├── Respostas sem fundamentação .. Ativado
    ├── Usar informações da Web ...... Desativado
    ├── Carregamentos de arquivo ..... Desativado
    ├── Intérprete de código ......... Desativado
    └── Gráfico do locatário ......... Ativado
```

---

> ✅ **Resultado esperado:** agente Holiday Assist criado, configurado e pronto para iniciar a criação do tópico de Solicitação de Férias.

---

> **Navegação:** Passo 1 de 3 · [Próximo → Passo 2: Criação do tópico](PASSO-2-TOPICO.md)