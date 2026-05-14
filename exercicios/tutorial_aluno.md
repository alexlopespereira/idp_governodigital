# Tutorial do Aluno — Autograder IDP-TD

Este tutorial leva você do **zero** até **submeter o primeiro exercício** e receber uma nota oficial.

> Tempo estimado: 15min de setup (uma vez no semestre) + 5min por exercício.

---

## Visão geral em 30 segundos

1. Você faz o exercício no GitHub (cria um repo, faz commits, abre PR — depende do exercício).
2. Roda `autograde validar <id>` dentro do diretório local do exercício.
3. A CLI mostra o **boletim** com cada critério avaliado e pergunta se quer submeter.
4. Se sim: a nota vai pra planilha do professor. Você pode resubmeter quantas vezes quiser — a maior nota conta.

---

## Parte 1 — Setup (uma vez no semestre)

### 1.1 Python 3.9 ou superior

Verifique:

```bash
python --version    # Windows / venvs ativos
python3 --version   # macOS / Linux
```

Se faltar:
- **macOS**: `brew install python` (ou já vem instalado).
- **Linux**: `sudo apt install python3 python3-pip` (Debian/Ubuntu).
- **Windows**: baixe em [python.org](https://www.python.org/downloads/) e marque **"Add Python to PATH"** no instalador.

### 1.2 Git

O autograder e o `gh` dependem do `git` estar instalado e no `PATH`. Verifique:

```bash
git --version
```

Se aparecer algo como `git version 2.43.0`, está OK. Se aparecer `git: command not found` (ou `'git' não é reconhecido como comando` no Windows), instale:

- **macOS**: `brew install git` (ou rode `xcode-select --install` para instalar o Command Line Tools).
- **Linux**: `sudo apt install git` (Debian/Ubuntu) ou `sudo dnf install git` (Fedora).
- **Windows**: baixe em [git-scm.com/download/win](https://git-scm.com/download/win). No instalador, **mantenha marcada a opção "Git from the command line and also from 3rd-party software"** — é isso que adiciona o `git` ao `PATH`.

Depois de instalar, **abra um terminal novo** e rode `git --version` de novo pra confirmar que está no `PATH`.

Configure seu nome e email (uma vez por máquina — o `git commit` exige):

```bash
git config --global user.name "Seu Nome"
git config --global user.email "seu.email@aluno.idp.edu.br"
```

> Se o `git` não estiver no `PATH`, os comandos `git clone`, `git commit`, `git push` falham — e o `gh` também não funciona, porque depende do `git` por baixo.

### 1.3 GitHub CLI (`gh`)

A partir do **Exercício 1.2** o autograder coleta evidência do `gh`. Instale agora pra não travar depois:

- **macOS**: `brew install gh`
- **Windows (winget)**: `winget install --id GitHub.cli`
- **Windows (scoop)**: `scoop install gh`
- **Linux/outros**: [cli.github.com](https://cli.github.com)

Autentique uma vez:

```bash
gh auth login 
````
escolha GitHub.com, HTTPS, Y, Login with a web browser, copie o código e cole no site, clique em Authorize Github

- Responda essas perguntas (as respostas estão indicadas com `R.:`):
```
? Where do you use GitHub? R.: GitHub.com                           
? What is your preferred protocol for Git operations on this host? R.: HTTPS                                
? Authenticate Git with your GitHub credentials? R.: (Y/n)Yes    
? How would you like to authenticate GitHub CLI? R.: [Use setas para mover]
> Login with a web browser             
``` 

- Um texto parecido com o seguinte vai aparecer no console no caso de sucesso:
```                    
! First copy your one-time code: XXXX-XXXX
Press Enter to open https://github.com/login/device in your browser... 
✓ Authentication complete.
- gh config set -h github.com git_protocol https
✓ Configured git protocol
✓ Logged in as alexlopespereira
! You were already logged in to this account
```

> Se o `gh` não estiver no PATH, o autograder marca os critérios `gh_*` como falhos com mensagem `gh not found in PATH` — não trava o resto, mas você perde os pontos correspondentes.

### 1.4 Instalar o CLI `autograde`

```bash
git clone https://github.com/alexlopespereira/autograde-idp.git
cd autograde-idp
pip install -e .
```

Verifique:

```bash
autograde --version
```

Se aparecer `autograde: command not found`, seu pip instalou em local fora do `PATH`. Tente `python -m pip install --user -e .` e adicione `~/.local/bin` ao `PATH`.

### 1.5 Login Google (Device Code Flow)

```bash
autograde login
```

A CLI mostra um código tipo `ABCD-1234` e uma URL (`google.com/device`). Abra a URL **em qualquer aparelho** (celular conta), digite o código, autorize com seu email.


### 1.6 Confirmar identidade

```bash
autograde whoami
```

Deve mostrar:

```
email: ana.silva@aluno.idp.edu.br
nome:  Ana Silva
turma: TD-2026-01
```

Se aparecer **`erro: email não está no roster`** → fale com o professor. Você não está na planilha da turma; o backend bloqueia qualquer submissão.

---

## Parte 2 — Fazendo o Exercício 1.1 (Seu Primeiro Repositório)
Você vai aprender a criar um repositorio no github, fazer add, commit e push dos arquivos. O exercise 1.1 exige que você crie um repositorio publico no github com pelo menos 2 commits.

### O que o exercício pede

Veja o YAML do exercício em [`idp_governodigital/exercicios/1.1.yaml`](https://github.com/alexlopespereira/idp_governodigital/blob/main/exercicios/1.1.yaml). Resumo:

| Critério | Peso | O que precisa |
|---|---:|---|
| `repo_existe` | 20 | repo público existe no seu usuário |
| `repo_publico` | 10 | visibilidade = public |
| `readme_existe` | 10 | arquivo `README.md` no root |
| `readme_nao_vazio` | 10 | conteúdo não vazio |
| `pelo_menos_1_commit` | 20 | ≥ 1 commit |
| `dois_commits` | 10 | ≥ 2 commits |
| `ultimo_commit_recente` | 10 | último commit nas últimas 24h |
| `reflexao_1` | 10 | resposta subjetiva avaliada por LLM (ver abaixo) |
| **Total** | **100** | |

> **Novidade — pergunta subjetiva.** A partir do exercício 1.1, antes do boletim a CLI faz uma pergunta de reflexão pedindo que você explique com suas palavras o que entendeu dos comandos. A resposta é avaliada por uma LLM (Gemini) e a nota vai pro critério `reflexao_1`. Respostas em branco são rejeitadas (a CLI fica em loop até você responder).

### Passo a passo

**1. Criar o repo no GitHub**

Use a UI: github.com → New repository → public 
Numa pasta vazia do seu computador, execute o comando para clonar seu repositorio com o comando abaixo:

`git clone https://github.com/SEU-USUARIO/meu-primeiro-repo.git`.

**2. Criar README.md com conteúdo**

Entre na pasta que foi criada com o comando abaixo:

```bash
cd meu-primeiro-repo
```

Crie o arquivo README.md e adicione-o ao repositorio:

```bash
echo "# Meu Primeiro Repositorio" > README.md
echo "" >> README.md
echo "Repo do exercicio 1.1 da disciplina Transformacao Digital." >> README.md
git add README.md
git commit -m "feat: README inicial"
```

**3. Segundo commit** (o exercício exige ≥ 2)

```bash
echo "" >> README.md
echo "## Sobre" >> README.md
echo "Estudante da TD-2026-01." >> README.md
git commit -am "docs: secao Sobre"
```

**4. Push**

```bash
git push origin main
```

**5. Validar**

Dentro do diretório do repo:

```bash
autograde validar 1.1
```

A CLI, na ordem:
1. detecta `repo_url` via `git config --get remote.origin.url`
2. chama `POST /grade-preview` (sem respostas) — descobre quais perguntas o exercício tem
3. **pergunta a você** cada questão subjetiva e espera a resposta (loop até resposta não-vazia)
4. chama `POST /grade-preview` de novo com suas respostas — backend roda Gemini, grada
5. mostra boletim por critério (já com a nota do `reflexao_N`)
6. pergunta `Deseja submeter? (s/n)`

### Como ler o boletim

A CLI primeiro pede a(s) pergunta(s):

```
Perguntas (1) — responda antes de submeter:

  [1/1] O que você entendeu dos comandos que acabou de executar?
  Resposta: criei o repo no github, clonei localmente, fiz commits com git
add e git commit, e enviei pro remoto com git push.
```

Depois aparece o boletim:

```
Boletim:
  ✅ 15/15  repositorio encontrado
  ✅ 15/15  1 commits (>= 1)
  ✅ 10/10  repositorio publico
  ✅ 10/10  arquivo 'README.md' presente
  ✅ 10/10  arquivo 'README.md' tem 122 bytes
  ✅ 10/10  2 commits (>= 2)
  ✅ 10/10  commit dentro de 24h
  ✅ 10/10  nome 'meu-primeiro-repo' bate com 'meu-primeiro-repo'
  ✅ 8/10
      Você citou git init, git add, git commit e git push corretamente,
      explicando que add prepara arquivos e commit registra mudanças.
      Faltou explicar o papel do git push (envio pro remoto) — 2 pts
      descontados.

  Total: 98/100

Deseja submeter? (s/n)
```

Saída com falhas:

```
  ✅ 15/15  repositorio encontrado
  ❌ 0/10   esperado 2 commits, encontrou 1
  ✅ 10/10  arquivo 'README.md' presente
  ❌ 0/10   README.md tem 0 bytes
  ...
  Total: 60/100
```

O símbolo `❌` traz uma **mensagem específica** explicando o que faltou. Use isso pra corrigir antes de submeter. O critério `reflexao_N` (último) traz o **feedback textual** do Gemini sobre sua resposta, justificando a nota.

**6. Submeter (ou não)**

- Responda `s` → envia resposta para a planilha do professor. 
- Responda `n` → não escreve. Você pode iterar e validar de novo.

> **Dica**: rode `autograde validar 1.1` quantas vezes quiser sem submeter. Só quando o boletim estiver bom, dê `s`. Se o `validar` aparecer "[OK]" em tudo mas você não quer submeter ainda, dê `n`.

### Atalho para scripts

Se quiser pular o prompt:

```bash
autograde validar 1.1 --auto-submit
```

---

## Parte 3 — Fazendo o Exercício 1.2 (GitHub CLI)
Neste exercicio você vai usar a CLI do github (`gh`) para interagir com o repositório. Você também vai aprender a criar uma branch e um PR (Pull Request).

Pré-requisito: `gh` instalado e autenticado (Parte 1.3).

### Critérios

| Critério | Peso | O que precisa |
|---|---:|---|
| `repo_publico` | 15 | repo público |
| `pelo_menos_1_pr` | 20 | ≥ 1 Pull Request (qualquer estado) |
| `pr_titulo_descritivo` | 15 | título do PR não-trivial (não é "WIP", "test", "asdf") |
| `gh_authenticated` | 15 | `gh auth status` retorna OK localmente |
| `gh_version_capturado` | 10 | `gh --version` capturado |
| `gh_repo_view_ok` | 15 | `gh repo view` no repo do exercício funciona |
| `reflexao_1` | 10 | resposta subjetiva (LLM avalia) |
| **Total** | **100** | |

Os 3 critérios `gh_*` são **evidência local de shell** — a CLI roda `gh` na sua máquina e manda o resultado pro backend. Se `gh` não estiver instalado, esses 40 pontos viram zero.

### Passo a passo
Numa pasta vazia, execute os comandos abaixo:
```bash
gh repo create exercicio-12 --public --clone
cd exercicio-12
```

Agora faça o commit inicial:
```bash
# commit inicial em main
echo "# Exercicio 1.2" > README.md
git add README.md
git commit -m "feat: README inicial"
git push origin main
```

Agora crie um branch, faça um commit e crie um PR:
```bash
# branch + PR
git checkout -b feat/algo
echo "linha nova" >> README.md
git commit -am "feat: adiciona linha"
git push -u origin feat/algo
gh pr create --title "feat: adiciona uma linha ao README" --body "Trabalho do exercicio 1.2"

Agora faça um merge do PR:
```bash
gh pr merge --squash --delete-branch
```

# validar
```bash
autograde validar 1.2
```

> **Importante**: o título do PR deve ser **descritivo**. Títulos como "WIP", "test", "fix", "asdf" falham no critério `pr_titulo_descritivo`.

---

## Parte 4 — Fazendo o Exercício 1.3 (Agente cria repositório e clona)

Neste exercício você vai usar um **agente de codificação** (Claude Code, Cursor, Codex CLI, GitHub Copilot CLI etc) para automatizar a criação de um repositório e o clone local com `gh`. A ideia não é digitar os comandos — é **instruir o agente** e entender o que ele faz por você.

Pré-requisito: `gh` autenticado (Parte 1.3) e um agente de codificação disponível no seu terminal.

### Critérios

| Critério | Peso | O que precisa |
|---|---:|---|
| `repo_existe` | 20 | repo público existe no seu usuário |
| `repo_publico` | 20 | visibilidade = public |
| `pelo_menos_1_commit` | 10 | ≥ 1 commit |
| `gh_authenticated` | 15 | `gh auth status` retorna OK localmente |
| `gh_version_capturado` | 10 | `gh --version` capturado |
| `gh_repo_view_ok` | 15 | `gh repo view` no repo funciona |
| `reflexao_1` | 10 | resposta subjetiva (LLM avalia) |
| **Total** | **100** | |

### Passo a passo

**1. Abrir uma pasta vazia + o agente**

```bash
mkdir ~/agente-cria-repo && cd ~/agente-cria-repo
claude  # ou: cursor . / codex / aider / etc
```

**2. Instruir o agente**

Diga algo como:

> *"Crie um repositório público no meu usuário GitHub chamado `meu-segundo-repo` usando `gh`, faça o clone local dentro desta pasta atual e adicione um README."*

O agente provavelmente vai rodar uma sequência tipo:

```bash
gh repo create meu-segundo-repo --public --clone
cd meu-segundo-repo
echo "# Meu Segundo Repositorio" > README.md
```

**Atenção**: leia o que o agente faz, não aceite cegamente. Você precisa **entender cada comando** pra responder a pergunta de reflexão depois.

**3. Validar**

Dentro da pasta `meu-segundo-repo`:

```bash
autograde validar 1.3
```

A CLI vai fazer a pergunta de reflexão (algo como *"Como você instruiu o agente para criar o repositório e cloná-lo? O que cada comando do gh executado faz?"*) e gradear sua resposta via Gemini, somando ao boletim.

> **Dica**: a nota da reflexão depende da **qualidade da sua explicação**, não da resposta exata. Cite pelo menos 2 comandos do `gh` que o agente executou e explique o que cada um faz com suas palavras.

---

## Parte 5 — Fazendo o Exercício 1.4 (Agente cria arquivo, abre PR e merge)

Continuação natural do 1.3: agora você pede pro agente automatizar o ciclo completo de uma contribuição — criar arquivo, abrir PR e fazer merge.

Pré-requisito: igual ao 1.3 (`gh` autenticado + agente disponível).

### Critérios

| Critério | Peso | O que precisa |
|---|---:|---|
| `repo_existe` | 10 | repo público existe no seu usuário |
| `repo_publico` | 10 | visibilidade = public |
| `pr_mergeado` | 25 | ≥ 1 PR em estado `merged` |
| `pelo_menos_2_commits` | 20 | ≥ 2 commits na main (initial + PR mergeado) |
| `gh_authenticated` | 10 | `gh auth status` retorna OK localmente |
| `gh_repo_view_ok` | 15 | `gh repo view` no repo funciona |
| `reflexao_1` | 10 | resposta subjetiva (LLM avalia) |
| **Total** | **100** | |

### Passo a passo

**1. Abrir uma pasta vazia + o agente**

```bash
mkdir ~/agente-cria-pr && cd ~/agente-cria-pr
claude  # ou outro agente
```

**2. Instruir o agente — duas etapas**

Para entender melhor cada parte do fluxo, divida a tarefa em **dois prompts** consecutivos:

**Etapa 1 — setup do repo.** Diga ao agente algo como:

> *Crie um repositório público no GitHub chamado `meu-terceiro-repo`, clone-o, faça um commit inicial com README.md*

O agente provavelmente vai rodar:

```bash
gh repo create meu-terceiro-repo --public --clone
cd meu-terceiro-repo
echo "# Meu Terceiro Repositorio" > README.md
git add README.md && git commit -m "feat: README inicial"
git push origin main
```

Confira que o repo está visível no GitHub com o README antes de continuar.

**Etapa 2 — branch + arquivo + PR + merge.** No mesmo agente, dê o segundo prompt:

> *Crie uma branch nova, adicione um arquivo `CONTRIBUINDO.md` com um texto curto, abra um Pull Request com título descritivo e faça o merge na main.*

O agente provavelmente vai rodar:

```bash
git checkout -b feat/contribuindo
echo "# Como contribuir" > CONTRIBUINDO.md
echo "Abra issues e PRs descritivos." >> CONTRIBUINDO.md
git add CONTRIBUINDO.md && git commit -m "docs: guia de contribuicao"
git push -u origin feat/contribuindo

gh pr create --title "docs: adiciona guia de contribuicao" --body "Texto inicial sobre como contribuir."
gh pr merge --squash --delete-branch
```

Confira no GitHub que o PR aparece como **Merged** e que a `main` tem 2 commits.

**3. Validar**

Dentro da pasta `meu-terceiro-repo`:

```bash
autograde validar 1.4
```

A pergunta de reflexão pede que você explique o papel do `gh pr create` e do `gh pr merge` com suas palavras. Mostre que entendeu — não copie do output do terminal.

> **Importante**: o critério `pr_mergeado` exige PR no estado `merged`. PR aberto mas não mergeado, ou fechado sem merge, **não** conta. Cheque na aba "Pull requests" do GitHub que ele aparece com a tag roxa "Merged".

---

## Parte 6 — Comandos do dia-a-dia

| Comando | Função |
|---|---|
| `autograde validar <id>` | Coleta evidência + grade-preview + prompt de submit |
| `autograde validar <id> --auto-submit` | Pula o prompt |
| `autograde notas` | Lista suas notas por exercício (melhor nota + nº de tentativas) |
| `autograde whoami` | Email autenticado + turma |
| `autograde login` | Re-autenticar (se token expirou) |
| `autograde --version` | Versão + plataforma |

---

## Parte 7 — Quando der errado

### "Could not detect exercise from CWD"

```bash
autograde validar
# ERRO: nao consegui detectar o exercicio. Use: autograde validar <id>
```

A CLI tenta inferir o exercício pelo nome do repo, mas não chuta. Especifique:

```bash
autograde validar 1.1
```

### "403 not_in_roster"

Backend rejeitou seu email. Causas comuns:
- Você logou com email pessoal (gmail) em vez do institucional.
- Professor ainda não atualizou a planilha do roster.

Confirme com `autograde whoami` qual email está autenticado. Re-logue se for o caso (`autograde login`). Se persistir, fale com o professor.

### "criterio X falhando" mas você jura que está OK

Lembre que o backend **não vê** o estado local do seu repo. Ele consulta o GitHub via API. Confira:

1. Você fez `git push`? (`git status` deve dizer `nothing to commit, working tree clean` E `Your branch is up to date with 'origin/main'`)
2. O repo é **público**? (UI do GitHub → Settings → Danger zone → "Change visibility")
3. Você está no diretório certo? (`git config --get remote.origin.url` deve apontar pro repo do exercício)

### "401 token_expired"

Token Google expirou (~5 meses). Re-logue:

```bash
autograde login
```

### Submissão atrasada

Se você submeter depois do `prazo.recomendado_ate`, a linha é gravada com `late=true` na planilha. Você não perde pontos automaticamente — depende da política do professor. Não impede submissão.

### Submissão antes da data de abertura

Backend rejeita com mensagem pedagógica:
```
exercicio 1.1 abre em 2026-03-10T08:00:00-03:00. Volte depois.
```

### "Deseja submeter? (s/n)" travou

Se rodou em script sem TTY, o prompt não funciona. Use `--auto-submit`.

---

## Parte 8 — Re-submissões e melhor nota

- **Tentativas ilimitadas**. Cada `autograde validar X --auto-submit` (ou `s` no prompt) gera uma linha nova na planilha.
- Há uma fórmula no Sheet do professor que pega **a maior nota** por aluno × exercício. Não importa quantas tentativas — a melhor conta.
- O backend é **idempotente por `submission_uuid`**. Se a CLI receber timeout e você re-rodar, ela reusa o mesmo UUID — não duplica linha.

> **Estratégia**: faça muitas iterações com `autograde validar X` (sem `--auto-submit`), confira o boletim, conserte, valide de novo. Só dê `s` quando estiver confortável com a nota.

---

## Parte 9 — FAQ rápido

**P: Posso usar email pessoal?**
Não. Backend cruza com o roster da turma, que tem email institucional.

**P: Posso fazer o exercício 1.1 sem `gh`?**
Sim, ex 1.1 só usa GitHub API; `gh` é só pra ex 1.2+.

**P: Windows nativo funciona?**
Sim para 1.1 e 1.2. Para o futuro **exercício 3** (evidência IA), use **WSL2** — Claude Code/Codex CLI têm comportamento divergente em Windows nativo.

**P: Posso ver minhas notas em algum lugar?**
`autograde notas`. Lê direto da planilha do professor.

**P: Onde fica meu token?**
`~/.git-exercicios/token.json`. Em Unix, chmod 0600 (só você lê). Em Windows, ACL do user.

**P: Apaguei o token sem querer.**
`autograde login` de novo.

---

## Parte 10 — Próximos exercícios

| Exercício | Estado | O que pede |
|---|---|---|
| 1.1 | ✅ disponível | Repo público + README + 2 commits |
| 1.2 | ✅ disponível | PR + uso de `gh` CLI |
| 1.3 | ✅ disponível | Agente cria repo + clone com `gh` |
| 1.4 | ✅ disponível | Agente cria arquivo + PR + merge com `gh` |
| 3 | em elaboração | Evidência ampliada de uso de IA |

Critérios e prazos de cada um vivem no YAML correspondente em [`idp_governodigital/exercicios/`](https://github.com/alexlopespereira/idp_governodigital/tree/main/exercicios).

---

## Suporte

- **Problema com a CLI**: abra issue em [autograde-idp/issues](https://github.com/alexlopespereira/autograde-idp/issues).
- **Backend / nota errada / 5xx persistente**: fale com o professor.
- **Você acha que tem bug no critério (ex: README OK mas falha)**: descreva no canal da disciplina com `submission_uuid` da última tentativa.
