## ⚙* Passo 2 — Criação do Fluxo de Tra*alho (Agent Flow) — Experiência Mo*erna

🎯 **Objetivo desta etapa:***constru*r o fluxo de trabalho que recebe o* dados coletados pelo agente, conf*rma o recebimento e envia o pedido*para aprovação de forma **assíncro*a** — criado **direto do bloco Fer*amentas**, sem passar por nenhum t*pico.
**Navegação:** Passo 2 de 3 * ⬅️ Voltar ao Passo 1 (Moderno-Pas*o-1.md) · Pró*imo → Passo 3: Conectar e testar (*oderno-Passo-3.md)

> 🆕 **Diferen*a central para o Clássico:** no Cl*ssico, o fluxo era criado **de d*ntro do tópico**. Aqui **não há tó*ico** — o fluxo é criado a partir *o bloco **Ferramentas** do próprio*agente **Holiday Assist Moderno**.*A plataforma chama ess* peça de **"fluxo de trabalho"** (*orkflow), mas o conceito é o mesmo*agent flow.

---

### ✅ Status des*e documento

Este passo cobre a co*strução complet* do fluxo até a **publicação** — v*rsão funcional mínima (registra o *edido e envia para aprovação). A c*nexão do fluxo como ferramenta e o*mapeamento das entradas (IA gener*tiva vs. valor fixo) acontecem no **Passo 3**. A condição de resultad* e a notificação ao solicitante fi*am como melhoria futura, descrita *o final.

---

### ⚙️ Dec*sões tomadas para este fluxo

| De*isão | Escolha |
|---|---|
| Posiç*o da aprovação | **Depois** do *Re*pond to the agent* (assíncrono — s*m risco de timeout) |*| Campo "Atribuído a" | **Fixo**, *ma pessoa específica (para efeito *e treinamento) |
| Quem pode ser o*aprovador | Você mesmo (o criador)***ou***um colega ao lado — desde que no m*smo tenant |

ℹ️ **Por que assíncr*no:** um fluxo chamado por um agen*e tem lim*te de **100 segundos** para respon*er. Como a aprovação espera uma pe*soa clicar — o que pode levar minu*os —, ela precisa vir **depois** d* *Respond to the agent*, nunca ant*s.

---*
### ✅ Pré-requisitos

- Agente ***oliday Assist Moderno** criado e c*nfigurado, com conhecimento adicio*ado (ver Moderno-Passo-1.md).

---*
### 1. Ab*ir o designer do fluxo (a partir d* Ferramentas)

- No agente **Holid*y Assist Moderno**, aba **Criar**,*clique no bloco **Ferramentas** (c*luna direita) — ou no ***** ao lado dele.
- Abre o painel **"Adicionar uma ferramenta"**, com *s abas: *Em destaque · Protocolo d* Contexto do Modelo (*CP) · Conectores · Fluxos de traba*ho*.
- Clique na aba:
  ✅ **Fluxos*de trabalho**
- No canto superior *ireito do painel, clique no botão:*  ✅ *** Adicionar**

📌 **Atenção:** a ab* "Fluxos de trabalho" também lista*fluxos que já existem no ambiente *ex.: *Enviar Aprovacao de Fer*as* do lab clássico, *Save Summary*). **Não clique num existente** — *lique em **"➕ Adicionar"** para cr*ar um **novo**.

Isso ab*e o **designer do fluxo** já com a*estrutura inicial pronta, com o tí*ulo provisório **"Fluxo de trabalh* sem título"** (badge *Rascunho** e dois nós conectados:
```
When a* agent calls the flow   (gatilho —*nome mantido em inglês pela plataf*rma)
        ↓
Respond to the agen*           (resposta — nome mantid* em inglês pela*plataforma)
```

ℹ️ **Layout do de*igner:** à **esquerda**, o painel **"Adicionar"** (Agente, Classifica*, M365 Copilot, Revis*o humana, Conector, Função, Variáv*l, If/Else, Loop, Observação). À **direita**, o painel de propriedade* do nó selecionado. No **topo**: a*as *Criar · Atividade · Monitorame*to* e os*botões de **salvar (💾)**, **testa* (▶)** e **Publicar**.

---

### 2* Adicionar as entradas do gatilho
*Clique no nó **When an agent calls*the flow**. No painel direito aparece:
- ***ipo de gatilho:** *"Quando um agen*e chama o fluxo de trabalho — Disp*rar como uma ferramenta de um agen*e"* (já vem correto, não*altere).
- **Entradas:** com o bot*o **➕ Adicionar uma entrada**.

Pa*a cada entrada:
- Clique em **➕ Ad*cionar uma entrada**.
- Selecione * tipo:*  ✅ **Texto**
- Uma caixa à direit* pode vir com um texto padrão (ex.* *"Insira sua entrada aqui"*). **A*ague esse texto** se a*arecer — não some sozinho.
- No ca*po de nome, digite o nome da entra*a conforme a tabela.
- Repita para*todas.

| #*| Nome da entrada | Tipo |
|---|--*|---|
| 1 | RequesterName | Texto *
| 2 | ApproverEmail | Texto |
| 3*| StartDate | Texto |
| 4 | EndDat* | Texto |*| 5 | Days | Texto |
| 6 | Details*| Texto |

📌 **Todas as entradas *ão do tipo Texto**, inclusive as d*tas e a quantidade de dias — os va*ores chegam do orquestrador como t*xto e*são mais simples de tratar assim (*esmo princípio do Clássico).

ℹ️ **Por que só 6 entradas (sem Request*rEmail):** nesta versão base, o so*icitante não é notificado por e-ma*l — o nome d*le (`RequesterName`) vem automatic*mente do usuário logado no Passo 3* Se depois quiser notificar quem p*diu, adicione uma entrada **Reques*erEmail** (ver melhoria futura ao *inal).

---

### *. Nomear o fluxo

- Clique no títu*o **"Fluxo de trabalho sem título"** (topo esquerdo, ao lado de *Holi*ay Assist Moderno >*).
- Renomeie para:
```
Enviar Apr*vacao Ferias Moderno
```
- Clique *o ícone **💾 Salvar** (topo direit*) para grav*r o rascunho.

📌 Nome **com "Mode*no"** para não colidir com o fluxo*do lab clássico ("Enviar Aprovacao*de Ferias"), que aparece no m*smo ambiente (visto no painel de f*rramentas).
💡 Se o título não fic*r editável de primeira, clique em **💾 Salvar** uma vez e*tente renomear novamente.

---

##* ✅ Checkpoint 1

O nó **When an ag*nt calls the flow** deve mostrar a* seis entradas, todas do tipo Text*:
```
RequesterName
Approver*mail
StartDate
EndDate
Days
Detail*
```

---

### 4. Configurar o Res*ond to the agent

Clique no nó **R*spond to the agent**:

- Adicione *ma **saída** do tipo **Texto**.
- *omeie*a saída:
```
Resultado
```
- No va*or da saída, escreva a confirmação*de envio (sem afirmar aprovação, q*e ainda não aconteceu):
```
Pedido*de ferias registrado com sucesso e*enviado para aprovacao. Vo*e sera notificado assim que houver*uma resposta.
```

✅ Como a aprova*ão vem **depois** deste nó, o agen*e responde *o colaborador imediatamente com es*a confirmação — sem esperar ningué* clicar.

---

### 5. Adicionar o *ó de aprovação

Depois do **Respon* to the agent**, clique no ***** entre/abaixo dos nós (ou use o *ainel **"Adicionar"** à esquerda →***Conector**) e busque por *aprova*ão*.
Selecione a *ção:
✅ **Iniciar e aguardar uma ap*ovação**

🔧 Este é o nó correto. *utros conectores parecidos (como "*riar uma aprovação") **não bloquei*m a*execução** e não servem aqui — pre*isamos que o fluxo espere a respos*a, que é o que "Iniciar e aguardar*uma aprovação" faz.

#### 5.1 Tipo*de aprovação
No*campo **Tipo de aprovação**, selec*one:
✅ **Aprovar/Rejeitar – Primei*o a responder**
ℹ️ Basta **uma pes*oa** responder (aprovar ou rejeita*) para o*fluxo continuar — coerente com um *provador por pedido.

#### 5.2 Tít*lo
No campo **Título** *(obrigatór*o)*:
```
Aprovação de ferias
```

*### 5.3 Atribuído a
No campo***Atribuído a** *(obrigatório)*:
S*lecione **uma pessoa específica do*seu tenant** — você mesmo ou um co*ega ao lado.

💡 **Para efeito de *reinamento**, este campo fica **fi*o** (u*a pessoa escolhida manualmente), n*o dinâmico.
⚠️ A entrada **Approve*Email** continua existindo no flux*, mas **não é usada** neste campo *nquanto ele estiver fixo. Se depoi* quiser que*o aprovador seja o e-mail informad* na conversa, troque "Atribuído a"*para usar o **conteúdo dinâmico** *a entrada ApproverEmail.
🔧 **Se e*tá*testando sozinho, coloque a si mes*o aqui** — não um colega. Se "Atri*uído a" apontar para outra pessoa,*o card de aprovação chega na caixa***dela**,*não na sua. Parece "o flow não foi*chamado", mas ele rodou — só a not*ficação foi para outro lugar. **An*es de assumir falha, confira o*Power Automate:** Meus fluxos → En*iar Aprovacao Ferias Moderno → His*órico de execuções.

#### 5.4 Deta*hes
No campo **Detalhes** *(opcional, aceita Markdown)*, m*nte um resumo combinando texto fix* com o valor de cada entrada:

- D*gite o texto fixo, por exemplo `*****icitante:**`.
- Passe o m*use sobre o campo — um ícone de ***aio (⚡)** aparece na borda direita*
- Clique no raio. Um painel abre *om a lista das entradas do gatilho*
- Clique na*entrada desejada — ela aparece com* um **chip azul** dentro do campo.*- Continue digitando o texto fixo * inserindo os chips até completar * resumo.

Resultado final:
```***Solicitante:** [RequesterName]
**Periodo:** [StartDate] e [EndDate]***Dias:** [Days]
**Observacao:** [*etails]
```
✅ Onde aparece `[Nome]*, é um ***hip inserido pelo raio (⚡)**, não *exto digitado.

#### 5.5 Campos nã* utilizados

| Campo | Uso neste l*boratório |
|---|---|
| **Link do*item** | Deixar em branco — não há*item externo a linkar |
| **Descri*ão do link do item** | Deixar em b*anco |
| **Parâmetros avançados** * Manter padrões: **Habilitar notif*cações = Sim**, **Habil*tar reatribuição = Sim** |

---

#*# ✅ Checkpoint 2

O fluxo deve est*r nesta ordem:
```
When an agent c*lls the flow
  (6 entradas: Reques*erName, ApproverEmail,
   StartDat*,*EndDate, Days, Details)
        ↓
*espond to the agent
  (saída: Resu*tado)
        ↓
Iniciar e aguardar*uma aprovação
  Tipo: Aprovar/Reje*tar – Primeiro a responder
  Títul*: Aprovação de*ferias
  Atribuído a: [pessoa fixa*escolhida]
  Detalhes: resumo com *hips inseridos via raio (⚡)
```

-*-

### 6. Publicar o fluxo

- Cliq*e em ***� Salvar** (topo direito) para gar*ntir o rascunho gravado.
- Clique *m:
  ✅ **Publicar** (canto superio* direito)
- Aguarde a confirmação *e publicação.
- Vol*e ao editor do agente (breadcrumb **"Holiday Assist Moderno"** no top* esquerdo, ou feche o*designer).

O fluxo **Enviar Aprov*cao Ferias Moderno** agora existe * está pronto para ser **conectado * mapeado como ferramenta** — o que*fazemos no Passo 3.

✅ ***iferente do Clássico, aqui o fluxo*ainda NÃO está mapeado ao agente.** No moderno, o mapeamento das entr*das (IA generativa vs. `System.Use*.DisplayName`) é feito ***o Passo 3**, ao configurar a ferra*enta.

---

### 🔜 Melhoria futura*(opcional)

O que está publicado a*ora **registra o pedido e envia pa*a aprovação**, mas ainda não tr*ta o resultado (aprovado/recusado)*nem notifica quem pediu. Pode ser *dicionado depois, sem quebrar o qu* já funciona:

- Adicionar a entra*a***RequesterEmail** ao gatilho.
- N* **If/Else** comparando o resultad* da aprovação.
- Notificação ao so*icitante (RequesterEmail) com o re*ultado.

É só editar o fluxo*publicado e publicar de novo — sem*refazer nada.

**Navegação:** Pass* 2 de 3 · ⬅️ Voltar ao Passo 1 (Mo*erno-Passo-1.md) · Próximo*→ Passo 3: Conectar e testar (Mode*no-Passo-3.md)