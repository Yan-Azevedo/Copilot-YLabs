# 📚 Passo 1.1 — Adicionar Conhecimento ao Agente

> 🎯 **Objetivo desta etapa:** adicionar o documento de **Política de Férias — Contoso** como fonte de conhecimento do agente Holiday Assist.

> **Navegação:** Passo 1 · Etapa de conhecimento · [⬅️ Voltar ao Passo 1](Classico-PASSO-1-AGENTE.md) · [Próximo → Passo 2: Criação do tópico](Classico-PASSO-2-TOPICO.md)

---

## 1. Criar o documento

1. Abra o documento de conteúdo:

   👉 [**Política de Férias — Contoso**](Politica-de-Ferias-Contoso.md)

2. **Copie todo o conteúdo** do documento.

3. **Cole em um novo documento do Word** e salve com o nome de sua escolha, por exemplo:

```text
Politica_de_Ferias_Contoso.docx
```

> ℹ️ O nome do arquivo é livre. No exemplo das telas, foi usado `Politica_de_Ferias_Contoso.docx`.

---

## 2. Adicionar o documento no Copilot Studio

1. Volte ao Copilot Studio, na aba **Visão geral** do agente.
2. Localize a seção **Conhecimento**.
3. Clique em **Adicionar conhecimento**.
4. Na janela **Adicionar conhecimento**, clique em **selecionar para navegar**.
5. Selecione o arquivo Word criado na etapa anterior.
6. Confirme a adição.

> ℹ️ Também é possível carregar o arquivo a partir do **OneDrive** ou do **SharePoint**, ou escolher outras fontes (Sites públicos, Dataverse, etc.). Para este cenário, use o **upload do arquivo local**.

---

## 3. Aguardar o processamento

Após adicionar, o documento aparece na lista de **Conhecimento** com o status:

```text
Em andamento
```

Aguarde até o processamento ser concluído. Só então o agente passa a usar o documento nas respostas.

---

## ✅ Checkpoint

Ao final desta etapa, você deve ver:

- O documento **Politica_de_Ferias_Contoso.docx** listado na seção **Conhecimento**.
- A opção **Pesquisa na Web** permanece **Desabilitada** — o agente usa apenas o documento adicionado, não a internet.

---

> **Navegação:** Passo 1 · Etapa de conhecimento · [⬅️ Voltar ao Passo 1](Classico-PASSO-1-AGENTE.md) · [Próximo → Passo 2: Criação do tópico](Classico-PASSO-2-TOPICO.md)