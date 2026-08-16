# 🤖 Passo 1 — Criação do Agente no Copilot Studio (Experiência Clássica)

> 🎯 **Objetivo:** criar um agente no Microsoft Copilot Studio utilizando a experiência clássica baseada em tópicos.

> **Navegação:** Passo 1 de 3 · [Próximo → Passo 2: Criação do tópico](PASSO-2-TOPICO.md)

---

## 📇 Dados do agente

| Campo | Valor |
|---|---|
| **Nome do agente** | Holiday Assist |
| **Título resumido** | Solicitação de férias com aprovação |

**Descrição:**

Agente criado para receber pedidos de férias em linguagem natural, coletar as informações necessárias, encaminhar a solicitação para aprovação e retornar o resultado ao colaborador de forma simples, padronizada e rastreável dentro do Microsoft 365.

---

## 1. Acessar o Microsoft Copilot Studio

1. Acesse o portal do Microsoft Copilot Studio.
2. Selecione o ambiente onde o agente será criado.
3. No menu lateral, clique em:

✅ **Create**

---

## 2. Iniciar a Criação do Agente

Na tela de criação:

1. Clique em:

✅ **New Copilot**

2. Aguarde a abertura do assistente de criação.

---

## 3. Configurar as Informações Básicas

### Nome

No campo **Name**, informe:

```text
Holiday Assist
```

---

### Descrição

No campo **Description**, informe:

```text
O Holiday Assist é um agente conversacional voltado para ambiente corporativo e treinamentos práticos. Ele ajuda colaboradores a solicitar férias de forma simples, guiada e padronizada, coletando os dados essenciais do pedido e encaminhando a solicitação para aprovação de uma pessoa indicada no mesmo tenant.

O agente atua como uma interface conversacional entre o colaborador e o fluxo de aprovação, reduzindo mensagens informais, evitando solicitações incompletas e demonstrando como processos internos podem ser estruturados com recursos nativos do Microsoft 365.
```

---

### Instruções

No campo **Instructions**, informe:

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

---

### Idioma

No campo **Language**, selecione:

```text
Português (Brasil)
```

---

## 4. Definir Ambiente

Na seção **Environment**:

1. Localize o ambiente desejado.
2. Selecione o ambiente onde o agente será armazenado.

Caso exista apenas um ambiente disponível, mantenha a seleção atual.

---

## 5. Criar o Agente

Após preencher todas as informações:

1. Revise os dados inseridos.
2. Clique em:

✅ **Create**

---

## 6. Aguardar Provisionamento

Após a criação:

1. Aguarde o carregamento completo do agente.
2. O Copilot Studio abrirá automaticamente a página principal do agente.

---

## 7. Validar a Estrutura Inicial

Após o carregamento:

1. Verifique se o menu lateral apresenta as opções:

✅ **Topics**
✅ **Analytics**
✅ **Publish**
✅ **Settings**

2. Confirme que o agente foi criado sem erros.

---

## 🗂️ Estrutura Esperada

```text
Copilot Studio
│
└── Agente
    │
    ├── Nome
    ├── Descrição
    ├── Instruções
    ├── Idioma
    ├── Ambiente
    │
    ├── Topics
    ├── Analytics
    ├── Publish
    └── Settings
```

---

> ⚠️ **Importante:** nesta etapa não serão criados tópicos, entidades, variáveis, ações ou fluxos. O objetivo é apenas provisionar o agente base no Microsoft Copilot Studio para posterior configuração.

---

> **Navegação:** Passo 1 de 3 · [Próximo → Passo 2: Criação do tópico](PASSO-2-TOPICO.md)