# Exercício 2.1 — Mapa de Atores da Jornada de Serviço (URA Caixa / Seguro-Desemprego)

> **Disciplina:** IDP-TD 2026 · **Peso:** 100 pontos · **Tempo estimado:** 3–5h
> **Pré-requisitos:** exercícios 1.1–1.4 concluídos; conta em pelo menos
> **dois** assistentes de IA conversacionais (ChatGPT / Gemini / Claude.ai) e
> agente (Claude Code, codex, et) instalado localmente com a skill `grill-me`.

---

## 1. Contexto

O **Atendimento ao Seguro-Desemprego pela URA da Caixa** é um serviço público
em que o cidadão liga, navega menus de voz, é roteado entre IA e atendente
humano, e eventualmente obtém uma resposta (status do benefício, agendamento,
encaminhamento ao posto físico). Nessa jornada coexistem **atores humanos**
(cidadão, atendente, gerente de fila, auditor) e **atores de IA**
(reconhecimento de voz, classificador de intenção, sistema de roteamento,
chatbots de transbordo).

Você vai mapear os atores sando três técnicas complementares — uma de
**síntese inicial** (deep research em um assistente), uma de **verificação
adversária** (segundo assistente confronta o primeiro), e uma de **destilação
interativa** (sessão `/grill-me` no Claude Code ou outro agente de IA que força você a decidir).

## 2. Objetivos de aprendizagem

Ao final, você será capaz de:

- **Escrever meta-prompts** que produzam pesquisa estruturada em assistentes
  de IA (não apenas perguntas avulsas).
- **Operar pesquisa adversária** — usar um segundo assistente para refutar e
  triangular o primeiro, em vez de tratar uma única resposta como verdade.
- **Identificar atores** em um serviço público.
- **Iterar com `/grill-me`** para converter material bruto em um artefato
  decidido — não uma colagem.

## 3. Entregáveis

Crie um repositório GitHub público com **exatamente** estes seis arquivos na
raiz (nomes e capitalização importam — o autograder valida path-a-path):

| Arquivo | Conteúdo | Tamanho mínimo |
|---|---|---|
| `A_meta_prompt.md` | O meta-prompt que você usou na parte A | ≥ 200 palavras |
| `B_relatorio_original.md` | Relatório bruto do **assistente 1** (ex: Gemini Deep Research) | ≥ 800 palavras, ≥ 3 URLs |
| `B_relatorio_auditoria.md` | Relatório bruto do **assistente de auditoria** | ≥ 800 palavras, ≥ 3 URLs |
| `B_sintese_original_corrigido.md` | V2 do relatório original com inconsistencias corrigidas, **versionada** | ≥ 500 palavras, ≥ 2 iterações (`## v1`, `## v2`) |
| `C_grill_transcript.md` | Cópia integral da sessão `/grill-me` | ≥ 8 rodadas |
| `C_mapa_atores.md` | Mapa final de atores (tabela RACI **ou** diagrama mermaid) | ≥ 7 atores distintos |

> **Nomes idênticos aos da tabela.** O coletor é `path-strict` — `a_meta_prompt.md`
> (minúsculo) ou `A_meta-prompt.md` (hífen) contam como ausentes.

Coloque também um `README.md` curto na raiz com seu nome completo e um índice
clicável para os 6 arquivos. Crie o marcador `.autograde-exercise` com o
conteúdo `2.1` (uma única linha) — assim `autograde validar` detecta o
exercício automaticamente.

---

## 4. Tutorial passo a passo

### Parte A — Meta-prompt (≈ 30 min)

**O que é um meta-prompt:** um prompt que instrui o assistente *como* fazer a
pesquisa antes de pedir a resposta. Define escopo, profundidade, formato de
saída, fontes aceitáveis. É a diferença entre "me fala sobre a URA da Caixa"
(prompt) e "Você é um pesquisador de serviço público. Investigue X com
profundidade Y, retornando no formato Z, citando fontes do tipo W" (meta-prompt).

**Passo a passo:**

1. **Abra o assistente 1** (recomendo Gemini com Deep Research ativado ou
   ChatGPT em modo Deep Research, se sua conta tiver).
2. **Não envie nada ainda.** Antes, num editor de texto, rascunhe o
   meta-prompt cobrindo *no mínimo*:
   - **Persona** do assistente (ex: "pesquisador de operações de serviço
     público com 10 anos em call center governamental").
   - **Escopo** explícito: serviço = Atendimento ao Seguro-Desemprego;
     canal = URA telefônica da Caixa Econômica Federal; jornada = do
     momento em que o cidadão liga até a resolução ou encaminhamento.
   - **Atores a mapear, explicitamente HUMANOS E DE IA.** Se você não disser
     "incluindo sistemas automatizados, modelos de NLP, regras de roteamento
     algorítmico", o assistente quase sempre lista só pessoas.
   - **Horizonte temporal:** estado atual (2024–2026), ou inclui projeção?
   - **Formato de saída:** tabela markdown com colunas
     `[ator, tipo (humano/IA), papel na jornada, ponto de entrada, ponto de saída, evidência/fonte]`.
   - **Critérios de fonte:** documentos oficiais (gov.br, Caixa, MTE), relatos
     do TCU/CGU, papers acadêmicos, jornalismo investigativo. Recusar blogs
     sem autoria e o próprio chatbot do site.
3. **Salve** o meta-prompt em `A_meta_prompt.md`. Esse é o entregável de A —
   não o resultado dele.
4. **Envie** o meta-prompt para o assistente 1 e guarde a resposta inteira
   (você vai usar em B).

### Parte B — Deep research adversarial (≈ 90 min)

A ideia da pesquisa adversária: **a resposta de um único assistente é uma
hipótese, não um fato.** Você roda a mesma investigação em dois assistentes
diferentes e força os dois a se confrontarem.

**Passo a passo:**

1. **Cole a resposta integral do assistente 1** em `B_relatorio_assistente1.md`.
   Inclua um cabeçalho:

   ```markdown
   # Relatório — Assistente 1

   - **Ferramenta:** Gemini 2.x Deep Research
   - **Data:** 2026-MM-DD
   - **Prompt usado:** ver `A_meta_prompt.md`
   - **Link da conversa (se exportável):** ...
   ```

2. **Abra um assistente diferente** (assistente 2 — escolha um *modelo* e um
   *provedor* diferentes; ex: se A1 foi Gemini, A2 deve ser ChatGPT ou
   Claude.ai, não outra sessão do Gemini).
3. **Use o mesmo meta-prompt** — não reescreva. O ponto do adversarial é
   variar o modelo, não a pergunta.
4. **Cole a resposta integral em `B_relatorio_assistente2.md`** com o mesmo
   cabeçalho.
5. **Agora a parte adversária — e ela é iterativa.** A síntese **não** é
   um documento escrito de uma vez; é um log de versões. Você escreve `v1`
   logo depois de ler A1+A2, depois roda `/grill-me` (Parte C), volta e
   escreve `v2` incorporando o que a entrevista expôs, e (opcionalmente)
   uma `v3` se ainda restaram dúvidas que mandaram você reabrir o
   assistente 1 ou 2. **Mínimo: 2 iterações.** Estrutura obrigatória do
   `B_sintese_adversarial.md`:

   ```markdown
   # Síntese adversarial

   ## v1 — primeira leitura (data: 2026-MM-DD HH:MM)

   ### Pontos de concordância
   - [Atores que ambos listam, com confiança]

   ### Divergências
   - **Divergência 1:** [o que A1 afirma vs. o que A2 afirma]
     - Quem está mais defensável? Por quê? Que evidência decide?
   - **Divergência 2:** ...

   ### Lacunas dos dois
   - [Atores que você suspeita existirem mas nenhum dos dois citou]

   ### Perguntas em aberto que levarei ao /grill-me
   - ...

   ---

   ## v2 — após sessão /grill-me (data: 2026-MM-DD HH:MM)

   ### Mudanças nesta versão
   - **Mudou:** [posição X que eu defendia em v1 → posição Y agora]
     **Gatilho:** [pergunta N do grill-me / nova evidência / resposta que
     contradisse premissa em v1]
   - **Novo ator detectado:** [ator Z que não estava em v1]
     **Gatilho:** [como apareceu]
   - **Divergência resolvida:** [Divergência 1 de v1 → fechada porque ...]

   ### Estado atual da síntese (sobrescreve v1)
   - Concordância: ...
   - Divergências residuais: ...
   - Decisão para o mapa em C: ...

   ---

   ## v3 — (opcional) após follow-up no assistente 1/2

   ### Mudanças nesta versão
   - ...
   ```

   **Critérios não-negociáveis:**
   - Cada versão (a partir de v2) precisa de um bloco `### Mudanças nesta
     versão` com pelo menos **uma** mudança substantiva (mudou posição,
     descobriu ator, resolveu divergência, abriu nova pergunta), e cada
     mudança precisa citar o **gatilho** concreto (não vale "pensei
     melhor"). O LLM-judge rejeita evolução cosmética (reescrita sem novo
     conteúdo) — B7 = 0.
   - A versão final precisa conter pelo menos **uma** divergência real
     identificada em algum ponto do processo (não cosmética). Se a única
     "divergência" é "A1 usou bullet point e A2 usou tabela", você fez
     resumo, não pesquisa adversária.

### Parte C — Mapa de atores via `/grill-me` (≈ 60 min)

O `/grill-me` é uma skill do Claude Code que entrevista você adversarialmente
sobre um plano/design, **uma pergunta por vez**, até reduzir ambiguidades. Aqui
você usa para destilar B em um mapa decidido.

**Passo a passo:**

1. **Abra um terminal** no diretório do repositório do exercício (`cd`
   no diretório onde estão `A_*`, `B_*`).
2. **Inicie o Claude Code:** `claude` (assumindo que a CLI já está instalada
   e autenticada — pré-requisito do exercício 1.x).
3. **Cole o seguinte prompt** (não invente uma variação; o autograder espera
   esta estrutura no transcript):

   ```
   /grill-me

   Quero produzir um mapa de atores (humanos e de IA) da jornada
   "Atendimento ao Seguro-Desemprego pela URA da Caixa". Eu já fiz pesquisa
   adversária em dois assistentes (anexos em B_relatorio_assistente1.md,
   B_relatorio_assistente2.md, B_sintese_adversarial.md). Quero que você me
   entreviste, uma pergunta por vez, até eu ter clareza sobre:
     - quais atores são reais vs. inferidos
     - onde cada ator entra e sai da jornada
     - quais são humanos, quais são IA, quais são híbridos
     - quais relações entre eles importam para o mapa final
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
   | # | Ator | Tipo | Responsável (R) | Aprovador (A) | Consultado (C) | Informado (I) | Entra na jornada | Sai da jornada |
   |---|------|------|-----------------|---------------|----------------|---------------|------------------|----------------|
   | 1 | Cidadão | Humano | Iniciar chamada | — | — | Status | t=0 | resolução/desistência |
   | 2 | IVR (reconhecimento de voz) | IA | Classificar intenção | — | — | — | t=0+5s | encaminhamento |
   | ... | ... | ... | ... | ... | ... | ... | ... | ... |
   ```

   **Formato B — Diagrama mermaid + tabela de atores:**

   ```markdown
   ```mermaid
   flowchart LR
     Cidadao -->|liga| IVR[IA: IVR]
     IVR -->|intenção 'seguro-desemprego'| Router[IA: roteador]
     Router -->|caso simples| Bot[IA: chatbot transbordo]
     Router -->|caso complexo| Atendente[Humano: atendente N1]
     Atendente -->|escalation| GerenteFila[Humano: supervisor]
     ...
   ```

   | Ator | Tipo | Papel |
   |---|---|---|
   | ... | ... | ... |
   ```

   **Mínimo:** 7 atores distintos, com pelo menos **2 humanos** e **2 IA**.
   Cada ator no mapa deve aparecer no transcript da sessão `/grill-me` (a
   rubrica verifica essa consistência).

7. **Volte para a Parte B e escreva a `v2` da síntese.** O `/grill-me`
   provavelmente derrubou alguma premissa de `v1`, descobriu um ator novo,
   resolveu uma divergência, ou levantou pergunta que você teve que reabrir
   no assistente 1/2. Documente isso no bloco `### Mudanças nesta versão`
   de `v2`, citando o gatilho concreto (ex: "pergunta 4 do grill mostrou que
   eu estava misturando IVR com URA"). Sem esse retorno, B6 e B7 zeram.

---

## 5. Validação local e submissão

```bash
# 1. Garanta que está no diretório raiz do repo do exercício
cd ~/projetos/idp-2026/exercicio-2.1

# 2. Verifique os arquivos
ls A_*.md B_*.md C_*.md .autograde-exercise

# 3. Rode o autograder (faz preview antes de submeter)
autograde validar 2.1
```

O `autograde validar` vai:
1. Detectar o repo via `git remote.origin.url`.
2. Ler os 6 arquivos e calcular evidência local (existência, palavras, URLs,
   sha256, headings).
3. Enviar `artifacts_evidence` + `repo_url` ao backend.
4. Backend roda **checks determinísticos** (10 pts) + **LLM-as-judge** sobre
   o conteúdo (90 pts) contra a rubrica abaixo.
5. Mostra boletim. Se aceitar, digite `s` para submeter.

> Limite de previews por dia: 10. Use com critério.

---

## 6. Rubrica de avaliação (total: 100 pts)

> **Fonte canônica:** a versão autoritativa da rubrica vive em
> [`exercicios/2.1.yaml`](https://github.com/alexlopespereira/idp_governodigital/blob/main/exercicios/2.1.yaml).
> Leia esse arquivo antes de submeter — é exatamente o que o backend usa para
> calcular sua nota. Cada `id` da tabela abaixo (`A_meta_prompt_quality`,
> `B1_relatorios_distintos`, `C2_C3_C5_mapa_qualidade`, etc.) bate com um
> `criterios[].id` do YAML, e o campo `args` lá mostra os parâmetros exatos
> (palavras mínimas, número de URLs, sub-critérios passados ao LLM judge).
> Se a tabela aqui e o YAML divergirem, **o YAML manda** — esta seção é
> didática, aquele arquivo é o contrato.

A rubrica é fechada — o LLM judge no backend recebe a rubrica + cada artefato
e devolve `{criterio_id: {score, evidence_quote, missing}}`. A `evidence_quote`
aparece no boletim para você entender de onde veio a nota.

### Parte A — Meta-prompt (20 pts)

| ID | Critério | Pts | Como é checado |
|---|---|---|---|
| A1 | **Escopo explícito do serviço** — cita "Seguro-Desemprego", "URA", "Caixa" sem ambiguidade | 4 | judge: regex + leitura semântica |
| A2 | **Atores humanos E de IA pedidos explicitamente** — não basta "atores", precisa nomear os dois tipos | 4 | judge: leitura semântica |
| A3 | **Horizonte temporal definido** — estado atual ou projeção, com janela datada | 4 | judge: leitura semântica |
| A4 | **Formato de saída estruturado** — tabela/JSON/seções nomeadas, não prosa livre | 4 | judge: leitura + presença de marcadores |
| A5 | **Critérios de fonte verificáveis** — exige URLs, documentos oficiais, ou rejeição de fontes fracas | 4 | judge: leitura semântica |

### Parte B — Pesquisa adversarial (40 pts)

| ID | Critério | Pts | Como é checado |
|---|---|---|---|
| B1 | **Dois relatórios distintos existem** — sha256 diferente, primeiros 500 chars distintos | 6 | determinístico (collector) |
| B2 | **Cada relatório ≥ 800 palavras** (substância, não outline) | 8 | determinístico (`word_count`) |
| B3 | **Cada relatório ≥ 3 URLs externas** distintas | 8 | determinístico (`links`) |
| B4 | **Síntese identifica ≥ 1 divergência real** entre A1 e A2 (não cosmética) | 6 | judge com rubrica explícita |
| B5 | **Síntese propõe resolução** — qual posição é mais defensável ou abre pergunta | 4 | judge |
| B6 | **Síntese versionada com ≥ 2 iterações** (`## v1`, `## v2`) e cada iteração ≥ v2 tem bloco `### Mudanças nesta versão` | 4 | determinístico (`headings[]` conta `## v\d+` e `### Mudanças`) + judge (rejeita iterações vazias/duplicadas) |
| B7 | **Evolução substantiva entre versões** — cada bloco de mudanças cita ≥ 1 delta concreto (mudou posição, novo ator, divergência resolvida, pergunta aberta) **e** o gatilho que causou (pergunta de grill, nova evidência, resposta de A1/A2 reaberto). Cosmético/reescrita sem novo conteúdo = 0 | 4 | judge |

### Parte C — Mapa via grill-me (40 pts)

| ID | Critério | Pts | Como é checado |
|---|---|---|---|
| C1 | **Transcript com ≥ 8 rodadas Q&A** distinguíveis | 8 | determinístico (regex Q/A) + judge para qualidade |
| C2 | **Consistência transcript ↔ mapa** — todos atores do mapa aparecem em respostas do transcript | 8 | judge (cross-reference) |
| C3 | **≥ 7 atores distintos no mapa**, com ≥ 2 humanos e ≥ 2 IA | 10 | judge (tipagem) + determinístico (contagem linhas tabela) |
| C4 | **Relações entre atores explícitas** — RACI ou setas mermaid, não lista solta | 8 | judge (formato) + determinístico (detecção `\|...\|` ou `flowchart`) |
| C5 | **Decisões do grill citadas no mapa** — pelo menos 2 escolhas do transcript justificam categorização | 6 | judge |

### Penalidades

- Cópia entre alunos detectada via sha256 dos arquivos: **−100% no item afetado**.
- Mapa contém apenas atores humanos (zero IA): **−100% no item C** (a aula
  inteira é sobre tornar IA visível).
- Síntese B é resumo, não confronto (B4 = 0): **−50% adicional em B5**.
- Atraso: regras-padrão do autograder (perda diária definida no backend).

---

## 7. Critérios de "definição de pronto"

Antes de submeter, confirme:

- [ ] `autograde validar 2.1` roda sem erro de schema (todos os 6 arquivos
      `exists=True` no payload).
- [ ] Cada arquivo de B tem ≥ 800 palavras e ≥ 3 URLs distintos
      (verifique com `wc -w B_*.md` e `grep -oE 'https?://[^[:space:]]+' B_*.md | sort -u`).
- [ ] `B_sintese_adversarial.md` tem pelo menos **2 cabeçalhos `## v1`,
      `## v2`** (a v1 é a leitura inicial; v2 é o estado pós-`/grill-me`).
      Verifique com `grep -E '^## v[0-9]+' B_sintese_adversarial.md`.
- [ ] Cada versão a partir de v2 tem bloco `### Mudanças nesta versão`
      com pelo menos uma mudança que cita gatilho concreto (pergunta N do
      grill / nova evidência / etc.) — não vale "refleti melhor".
- [ ] **Ordem real de execução foi: A → leitura A1+A2 → escrever v1 → C
      (grill-me) → escrever v2.** Se você escreveu a síntese inteira no
      final, B6/B7 vão zerar (datas em v1/v2 fora de ordem disparam o
      judge).
- [ ] `C_grill_transcript.md` tem pelo menos 8 marcadores "## Pergunta N" ou
      equivalente — você pode formatar livremente, mas precisa dar pro
      autograder contar as rodadas.
- [ ] Todo ator de `C_mapa_atores.md` aparece nominalmente em
      `C_grill_transcript.md`.
- [ ] `.autograde-exercise` contém só a string `2.1`.
- [ ] README do repo tem seu nome completo e índice dos 6 arquivos.

---

## 8. Dicas e armadilhas comuns

- **Não rode `/grill-me` antes de B.** O ponto é destilar a pesquisa, não
  substituí-la. Sessões iniciadas em vazio produzem mapas genéricos.
- **Não use dois ChatGPTs como "dois assistentes".** Adversarial exige
  variar o modelo subjacente, não a sessão. Gemini vs. ChatGPT vs. Claude
  conta; ChatGPT-4 vs. ChatGPT-4o não conta.
- **Cuidado com PII em transcripts.** Se o assistente citar nomes reais de
  servidores, anonimize antes de commitar (o repo será público).
- **Atores de IA contam mesmo quando o serviço não os chama de "IA".** O
  reconhecimento de voz da URA é um modelo acústico — é IA. O classificador
  "esta chamada é sobre seguro-desemprego?" é IA. O motor de regras de
  roteamento *talvez* não seja IA estatística, mas é decisão algorítmica —
  vale citar como ator automatizado.
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
