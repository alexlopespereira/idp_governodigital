# Exercício 2.1 — Mapa de Atores da Jornada de um Serviço Público

> **Disciplina:** IDP-TD 2026 · **Peso:** 100 pontos · **Tempo estimado:** 3–5h
> **Pré-requisitos:** exercícios 1.1–1.4 concluídos; conta em pelo menos
> **dois** assistentes de IA conversacionais (ChatGPT / Gemini / Claude.ai) e
> Claude Code instalado localmente com a skill `grill-me`.

---

> **⚠ Escolha do serviço.** Este tutorial usa o **Atendimento ao
> Seguro-Desemprego pela URA da Caixa** como serviço-exemplo do início ao fim.
> Você **pode** mantê-lo **ou escolher outro serviço público** que conheça
> melhor (ex.: emissão de passaporte, agendamento no SUS, matrícula na rede
> pública, licenciamento de veículo, CadÚnico). Se trocar, **todos os exemplos
> abaixo — meta-prompt, transcript, mapa, di  agramas mermaid — passam a ser
> ILUSTRATIVOS**: adapte-os ao seu serviço. A rubrica avalia se você **definiu
> um serviço público concreto e nomeado** (qual serviço, qual canal, qual
> órgão) e identificou tipologia clara dos atores (papéis, categorias) — **não** qual serviço você
> escolheu.

## 1. Contexto

O **Atendimento ao Seguro-Desemprego pela URA da Caixa** — serviço-exemplo
deste tutorial — é um serviço público
em que o cidadão liga, navega menus de voz, é roteado entre canais
automatizados e atendentes, e eventualmente obtém uma resposta (status do
benefício, agendamento, encaminhamento ao posto físico). A jornada envolve
uma **diversidade de atores** — cidadãos na ponta, operadores, supervisores,
auditores, fornecedores de tecnologia, órgãos de controle — e sistemas
automatizados na retaguarda. **Mapear quem está ali e o papel de cada um**
é o trabalho.

Você vai mapear os atores desse serviço usando três técnicas complementares — uma de **meta-prompt**, deep research adversarial, e uma de **destilação interativa** (sessão `/grill-me` no Claude Code ou outro agente de codificacao).

**Atenção**: num contexto real, você entrevistaria os stakeholders para descobrir seus interesses e descobrir outros stakeholders. A IA não resolve o problema de falta de contexto.

## 2. Objetivos de aprendizagem

Ao final, você será capaz de:

- **Escrever meta-prompts** que produzam pesquisa estruturada em assistentes
  de IA (não apenas perguntas avulsas).
- **Operar pesquisa adversária** — usar um segundo assistente para refutar e
  triangular o primeiro, em vez de tratar uma única resposta como verdade.
- **Identificar atores frequentemente esquecidos** em mapas tradicionais —
  intermediários, órgãos de controle, fornecedores, sistemas de retaguarda
  (a maioria dos mapas para nos óbvios da linha de frente).
- **Iterar com `/grill-me`** para converter material bruto em um artefato
  decidido — não uma colagem.

## 3. Entregáveis

Crie um repositório GitHub público com **exatamente** estes oito arquivos na
raiz (nomes e capitalização importam — o autograder valida path-a-path):

| Arquivo | Conteúdo | Tamanho mínimo |
|---|---|---|
| `A_meta_prompt.md` | O meta-prompt que você usou na parte A | ≥ 200 palavras |
| `B_relatorio_assistente_v1.md` | Pesquisa original do **assistente 1** (ex: Gemini Deep Research) | ≥ 300 palavras |
| `B_relatorio_auditoria_v1.md` | Auditoria da v1 pelo **assistente 2** (ex: ChatGPT, Claude.ai) | ≥ 300 palavras |
| `B_relatorio_assistente_v2.md` | Correção/revisão do **assistente 1** baseada na auditoria_v1 | ≥ 300 palavras |
| `B_relatorio_auditoria_v2.md` | Segunda auditoria da v2 pelo **assistente 2** | ≥ 300 palavras |
| `B_relatorio_assistente_v3.md` | Versão final do **assistente 1** após audit_v2 | ≥ 300 palavras |
| `C_grill_transcript.md` | Cópia integral da sessão `/grill-me` | ≥ 6 rodadas |
| `C_mapa_atores.md` | Mapa final de atores (tabela RACI **ou** diagrama mermaid) | ≥ 7 atores distintos |

> **Nomes idênticos aos da tabela.** O coletor é `path-strict` — `a_meta_prompt.md`
> (minúsculo) ou `A_meta-prompt.md` (hífen) contam como ausentes.

Coloque também um `README.md` curto na raiz com seu nome completo e um índice
clicável para os 8 arquivos. Crie o marcador `.autograde-exercise` com o
conteúdo `2.1` (uma única linha) — assim `autograde validar` detecta o
exercício automaticamente.

---

## 4. Tutorial passo a passo

### Parte A — Meta-prompt (≈ 30 min)

**O que é um meta-prompt:** um prompt que instrui o assistente a elaborar um prompt por você.
Prompts mais detalhados com contexto, persona, critério de sucesso e outros detalhes tendem a gerar resultados mais satisfatorios. Tente contar seu problema e não definir uma tarefa de escopo fechado. No exercício em questão, queremos coletar dados para subsidiar uma tarefa posterior. Queremos coletar um contexto para que o assistente de IA na proxima tarefa (grill-me) não fique com pouco contexto. Se possível, anexe a este pedido de elaboração de prompt algum contexto que voce já tenha sobre o serviço que vai analisar. Solicite ao assistente de IA para "elaborar um prompt para um assistente de deep research realizar uma pesquisa".

**Passo a passo:**

1. **Abra o assistente 1**: recomendo Gemini, Claude ou ChatGPT com Deep Research ativado.
2. **Escreva seu meta-prompt:** Descreva o problema, o objetivo, anexe algum contexto e solicite que  seja elaborado um prompt.
3. **Salve:** o meta-prompt em `A_meta_prompt.md`. Esse é o entregável de A —
   não o resultado dele.

### Parte B — Auditoria iterativa (≈ 90 min)


**Pipeline (5 arquivos):**

```
B1.1 v1 → B1.2 audit_v1 → B1.3 v2 → B1.4 audit_v2 → B1.4 v3
```

**Pré-requisito:** o meta-prompt de A está pronto e o assistente 1 já
respondeu com seu prompt.

**Passo a passo:**

B1.1 **Pesquisa Inicial** — copie o prompt de `A_meta_prompt.md` e cole no chat do assistente 1 (a sua escolha: Ex.: Gemini, Claude, ChatGPT).
   

2. **Peça uma AUDITORIA no assistente 2** (modelo *diferente* — ChatGPT,
   Claude.ai, etc.; **não** outra sessão do mesmo modelo). Use este prompt:

   ```
   Vou te enviar uma pesquisa que outro assistente de IA produziu sobre
   [seu tópico]. Faça uma AUDITORIA RIGOROSA dela. Identifique TODAS as
   falhas que encontrar:
     - erros factuais (cite o trecho)
     - lacunas de evidência (afirmação sem fonte)
     - inferências mal-suportadas
     - fontes fracas ou ausentes
     - atribuições incorretas
     - atores omitidos relevantes
   NÃO conte como falha questões cosméticas (formatação, estilo, ordem).
   Para cada falha, cite o trecho e justifique.

   PESQUISA A AUDITAR:
   """
   [colar v1 aqui]
   """
   ```

   Cole a resposta integral em `B_relatorio_auditoria_v1.md`. Mínimo
   **100 palavras** (auditoria substantiva, não one-liner).

3. **Volte ao assistente 1** com a auditoria. Prompt:

   ```
   Você fez uma pesquisa que eu submeti a auditoria por outro assistente.
   Segue a auditoria abaixo. Produza uma v2 da pesquisa ABORDANDO CADA
   falha apontada — para cada uma escolha:
     (a) corrigir com texto novo,
     (b) defender com argumento contrário e evidência, ou
     (c) marcar explicitamente como pendente / em-aberto.
   NÃO ignore falhas (seguir do mesmo jeito sem mencionar não conta).

   AUDITORIA:
   """
   [colar audit_v1 aqui]
   """
   ```

   Cole a v2 em `B_relatorio_assistente_v2.md`. Mínimo **300 palavras**.

4. **Segunda auditoria no assistente 2** sobre a v2:

   ```
   Esta é a v2 de uma pesquisa que você auditou anteriormente
   (audit_v1). Faça uma SEGUNDA auditoria verificando:
     (a) se cada falha que você apontou no audit_v1 foi de fato
         endereçada na v2,
     (b) se a v2 introduziu falhas novas,
     (c) se restam pontos abertos.
   Continue rigoroso — não amenize por gentileza.

   v2 A AUDITAR:
   """
   [colar v2 aqui]
   """
   ```

   Cole em `B_relatorio_auditoria_v2.md`. Mínimo **100 palavras**.

5. **Volte ao assistente 1** para a versão final:

   ```
   Segue a segunda auditoria. Produza uma v3 que aborde
   os pontos restantes do audit_v2. 

   Segue em anexo a auditoria.
   ```

   Cole em `B_relatorio_assistente_v3.md`. Mínimo **300 palavras**.

**Critérios não-negociáveis (a rubrica zera se faltar):**

- **`audit_v1` substantiva (B10):** ≥ 1 falha REAL identificada. "A v1
  usou bullet em vez de prosa" não conta.
- **`v2` ABORDA a `audit_v1` (B11):** cada falha tem tratamento (a/b/c).
  Ignorar = zero.
- **`v3` evolui sobre v2 com gatilho da `audit_v2` (B12):** cada delta
  cita o ponto da audit_v2 que motivou. "Refleti melhor" não conta.

**Dois assistentes DIFERENTES (modelos subjacentes distintos):** Gemini
≠ ChatGPT ≠ Claude. ChatGPT-4 vs ChatGPT-4o **não** conta.

### Parte C — Mapa de atores via `/grill-me` (≈ 60 min)

O `/grill-me` é uma skill do Claude Code que entrevista você adversarialmente
sobre um plano/design, **uma pergunta por vez**, até reduzir ambiguidades. Aqui
você usa para destilar B em um mapa decidido.

**Passo a passo:**

1. **Abra um terminal** no diretório do repositório do exercício (`cd`
   no diretório onde estão `A_*`, `B_*`).
2. **Inicie o Claude Code:** `claude` (assumindo que a CLI já está instalada
   e autenticada — pré-requisito do exercício 1.x).
3. **Cole um prompt igual ou semelhante a esse**:
Substitua <RELATORIO V3> pelo seu relatorio.
   ```
   /grill-me

   Quero produzir um mapa de atores da jornada "Atendimento ao
   Seguro-Desemprego pela URA da Caixa". Considere como contexto o artefato @<RELATORIO V3>. Siga a metodologia do @<PDF da aula2>
   ```

4. **Responda cada pergunta** — não pule, não responda "tanto faz", não peça
   para o Claude decidir por você. O ponto é *você* tomar a decisão.
5. **Salve o transcript completo** em `C_grill_transcript.md` (basta copiar
   do terminal — inclua suas respostas E as perguntas do Claude). Mínimo 8
   rodadas de pergunta-resposta. Se o Claude encerrar antes, peça para
   continuar com novos eixos (ex: "e quanto à camada de auditoria?").
6. **Produza o mapa final** em `C_mapa_atores.md`. Escolha **um** dos formatos:

   **Formato A — Tabela RACI:**

   ```markdown
   | # | Ator | Categoria | Responsável (R) | Aprovador (A) | Consultado (C) | Informado (I) | Entra na jornada | Sai da jornada |
   |---|------|-----------|-----------------|---------------|----------------|---------------|------------------|----------------|
   | 1 | Cidadão | Demandante | Iniciar chamada | — | — | Status | t=0 | resolução/desistência |
   | 2 | IVR (reconhecimento de voz) | Sistema de atendimento | Classificar intenção | — | — | — | t=0+5s | encaminhamento |
   | ... | ... | ... | ... | ... | ... | ... | ... | ... |
   ```

   **Formato B — Diagrama mermaid + tabela de atores:**

   ```markdown
   ```mermaid
   flowchart LR
     Cidadao -->|liga| IVR[IVR/menu de voz]
     IVR -->|intenção 'seguro-desemprego'| Router[Roteador]
     Router -->|caso simples| Bot[Chatbot de transbordo]
     Router -->|caso complexo| Atendente[Atendente N1]
     Atendente -->|escalation| Supervisor[Supervisor]
     ...
   ```

   | Ator | Tipo | Papel |
   |---|---|---|
   | ... | ... | ... |
   ```

   **Mínimo:** 7 atores distintos. Cada ator no mapa deve aparecer no
   transcript da sessão `/grill-me` (a rubrica verifica essa consistência).

---

## 5. Validação local e submissão

```bash
# 1. Garanta que está no diretório raiz do repo do exercício (adapte ao seu diretorio)
cd ~/exercicio-2.1

# 2. Verifique os arquivos
ls A_*.md B_*.md C_*.md .autograde-exercise

# 3. Rode o autograder (faz preview antes de submeter)
autograde validar 2.1
```

O `autograde validar` vai:
1. Detectar o repo via `git remote.origin.url`.
2. Ler os 8 arquivos e calcular evidência local (existência, palavras, URLs,
   sha256, headings).
3. Enviar `artifacts_evidence` + `repo_url` ao backend.
4. Backend roda **checks determinísticos** (16 pts, todos em B) +
   **LLM-as-judge** sobre o conteúdo (84 pts: A=20, B=24, C=40) contra a
   rubrica abaixo.
5. Mostra boletim. Se aceitar, digite `s` para submeter.

> Limite de previews por dia: 10. Use com critério.

---

---

## 7. Critérios de "definição de pronto"

Antes de submeter, confirme:

- [ ] `autograde validar 2.1` roda sem erro de schema (todos os 8 arquivos
      `exists=True` no payload).
- [ ] Cada `assistente_v{1,2,3}` tem ≥ 300 palavras
      (`wc -w B_relatorio_assistente_v*.md`).
- [ ] Cada `auditoria_v{1,2}` tem ≥ 300 palavras
      (`wc -w B_relatorio_auditoria_v*.md`).
- [ ] As 3 iterações de assistente são distintas (não cópia de v1 com
      mudança trivial) e as 2 auditorias também (`audit_v2` não copia
      `audit_v1`).
- [ ] `audit_v1` aponta ≥ 1 falha REAL na `v1` (não cosmética). Auditoria
      tipo "está ótimo" ou só elogio derruba B10/B11/B12 juntos.
- [ ] `v2` aborda as falhas da `audit_v1` — cada falha tem tratamento
      (corrigida / defendida com argumento / marcada como aberta).
      Ignorar = zero em B11.
- [ ] `v3` cita gatilhos da `audit_v2` em cada delta sobre `v2`.
      "Refleti melhor" sem gatilho = zero em B12.
- [ ] `C_grill_transcript.md` tem pelo menos 6 marcadores de rodada
      (formato livre, contanto que dê pra contar).
- [ ] Todo ator de `C_mapa_atores.md` aparece nominalmente em
      `C_grill_transcript.md`.
- [ ] `.autograde-exercise` contém só a string `2.1`.
- [ ] README do repo tem seu nome completo e índice dos 8 arquivos.

---

## 8. Dicas e armadilhas comuns

- **Não rode `/grill-me` antes de B.** O ponto é destilar a pesquisa, não
  substituí-la. Sessões iniciadas em vazio produzem mapas genéricos.
- **Não use dois ChatGPTs como "dois assistentes".** Adversarial exige
  variar o modelo subjacente, não a sessão. Gemini vs. ChatGPT vs. Claude
  conta; ChatGPT-4 vs. ChatGPT-4o não conta.
- **Cuidado com PII em transcripts.** Se o assistente citar nomes reais de
  servidores, anonimize antes de commitar (o repo será público).
- **Atores invisíveis contam — não só quem está na linha de frente.** Quem
  aprova o processo? Quem audita? Quem fornece o sistema que opera por trás?
  Quem normatiza? Esses atores afetam a jornada e geralmente somem do mapa
  porque o aluno foca apenas no ponto de contato com o cidadão.
- **Não gere o `C_mapa_atores.md` a partir de um único prompt no Claude.** A
  rubrica C2 (consistência transcript ↔ mapa) e C5 (decisões citadas)
  detectam isso — você perde 14 pts dos 40.

---

## 9. Suporte

- Dúvidas conceituais: canal `#idp-2026-exercicios` no Slack.
- Bug no autograder: abra issue em
  [autograde-idp/issues](https://github.com/alexlopespereira/autograde-idp/issues).
- Limite de preview atingido (HTTP 429): aguarde reset à meia-noite BRT;
  o `submission_uuid` é preservado para retry.
