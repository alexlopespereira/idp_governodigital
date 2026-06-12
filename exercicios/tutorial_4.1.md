# Exercício 4.1 — API REST de uma aplicação de TODO list (POST, GET e PUT)

> **Módulo 3 — Construindo interfaces (Aula 6: APIs REST).**
> Você vai construir uma API REST mínima e o autograder vai **executá-la de
> verdade** na sua máquina: ele sobe nada por você — *você* deixa a API rodando
> e roda `autograde validar 4.1`, que dispara chamadas HTTP reais (POST/GET/PUT)
> e confere as respostas.

**Total: 100 pontos** — 30 de estrutura no GitHub + 70 de execução real da API.

---

## 1. Objetivo

Implementar a **API REST que serve de backend de uma aplicação de TODO list** —
ela gerencia uma lista de **tarefas** (os "todo items") em memória, expondo os
três verbos HTTP centrais do exercício:

- **POST** — cria um recurso
- **GET** — lê um recurso
- **PUT** — atualiza um recurso

A ideia (Aula 6): uma API é um **contrato**. Se você cumpre o contrato, qualquer
cliente — incluindo o autograder, e no 4.2 o seu MCP server — consegue te
consumir sem saber nada de como você implementou por dentro. *Padronize a
interface, não a implementação.*

---

## 2. Pré-requisitos

- Python ≥ 3.10 e `pip`.
- `curl` no PATH (o autograder usa `curl` para falar com a sua API).
- `autograde` instalado e logado (`autograde login`).
- Uma conta no GitHub com o `gh` configurado.

---

## 3. Contrato da API (obrigatório — o autograder depende disto)

A API roda em **`http://localhost:8000`**, com armazenamento **em memória**
(um `dict`; **zera ao reiniciar** — isso é proposital: o autograder conta com
um estado limpo).

Recurso `tarefa`:

```json
{ "id": 1, "titulo": "estudar APIs", "concluida": false }
```

| Método | Rota              | Corpo de entrada                                  | Resposta (status + corpo) |
|--------|-------------------|---------------------------------------------------|---------------------------|
| GET    | `/health`         | —                                                 | `200` `{"status":"ok"}` |
| POST   | `/tarefas`        | `{"titulo": "<str>"}`                              | `201` `{"id":<int>,"titulo":"<str>","concluida":false}` |
| GET    | `/tarefas/{id}`   | —                                                 | `200` a tarefa; `404` se não existe |
| GET    | `/tarefas`        | —                                                 | `200` `[ ...tarefas... ]` |
| PUT    | `/tarefas/{id}`   | `{"titulo": "<str>", "concluida": <bool>}`        | `200` a tarefa atualizada; `404` se não existe |

Regras que o autograder verifica:

1. **`id` autoincrementa começando em `1`.** Como o store começa vazio, o
   **primeiro** `POST` cria a tarefa de `id=1`. É por isso que o GET/PUT de
   avaliação usam `/tarefas/1`.
2. **`concluida` começa `false`** no POST.
3. O **PUT** substitui `titulo` e `concluida` e **devolve a tarefa atualizada**.

> ⚠️ Os nomes dos campos são exatamente `id`, `titulo`, `concluida` (sem acento,
> minúsculos). Mudar um nome quebra a avaliação.

---

## 4. Estrutura do repositório

```
seu-repo/
├── app/
│   ├── __init__.py
│   └── main.py            # sua API (o autograder confere que existe e não está vazio)
├── requirements.txt       # deve listar fastapi e uvicorn (ou seu framework)
├── README.md              # use o README_modelo_4.1.md como base
└── .autograde-exercise    # conteúdo: 4.1
```

Você pode usar **qualquer framework** (FastAPI, Flask, etc.) desde que cumpra o
contrato e responda em `localhost:8000`. Os exemplos abaixo usam **FastAPI**.

### `requirements.txt`

```
fastapi
uvicorn
```

### Esqueleto de `app/main.py` (ponto de partida — você completa, ou pede para a IA completar)

```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel

app = FastAPI()

# Store em memória. Zera quando o processo reinicia.
_tarefas: dict[int, dict] = {}
_proximo_id = 1


class TarefaIn(BaseModel):
    titulo: str


class TarefaUpdate(BaseModel):
    titulo: str
    concluida: bool


@app.get("/health")
def health():
    return {"status": "ok"}


@app.post("/tarefas", status_code=201)
def criar(tarefa: TarefaIn):
    global _proximo_id
    nova = {"id": _proximo_id, "titulo": tarefa.titulo, "concluida": False}
    _tarefas[_proximo_id] = nova
    _proximo_id += 1
    return nova


# TODO: implemente GET /tarefas/{id}, GET /tarefas e PUT /tarefas/{id}
# seguindo o contrato da seção 3. Lembre do 404 quando o id não existir.
```

> O esqueleto entrega `/health` e o `POST`. **Os critérios de GET e PUT valem
> 40 pontos** — eles são seu trabalho.

---

## 5. Como rodar localmente

Num terminal, dentro do repo:

```bash
pip install -r requirements.txt
uvicorn app.main:app --port 8000
```

Deixe **esse terminal rodando**. Teste à mão em outro terminal:

```bash
curl -s http://localhost:8000/health
curl -s -X POST http://localhost:8000/tarefas \
  -H "Content-Type: application/json" -d '{"titulo":"estudar APIs"}'
curl -s http://localhost:8000/tarefas/1
```

---

## 6. Como o autograder avalia (execução real)

> **⚠ Atualize o `autograde` antes de validar.** Exercícios novos podem exigir a
> versão mais recente do CLI. Se você **já** tinha o `autograde` instalado, rode
> `git pull && pip install -e .` no seu clone do `autograde-idp` antes de validar
> (instalação nova já vem atualizada).

Com a API rodando em `localhost:8000`, num **outro terminal** dentro do repo:

```bash
autograde validar 4.1
```

O CLI executa, **na sua máquina**, esta sequência fixa e envia o stdout de cada
chamada ao backend, que confere o JSON campo a campo:

| Rótulo (`extract`) | Comando | Resposta esperada |
|---|---|---|
| `health`      | `curl -s localhost:8000/health` | `{"status":"ok"}` |
| `post_tarefa` | `POST /tarefas {"titulo":"estudar APIs"}` | `{"id":1,"titulo":"estudar APIs","concluida":false}` |
| `get_tarefa`  | `GET /tarefas/1` | `{"id":1,"titulo":"estudar APIs","concluida":false}` |
| `put_tarefa`  | `PUT /tarefas/1 {"titulo":"estudar APIs REST","concluida":true}` | `{"id":1,"titulo":"estudar APIs REST","concluida":true}` |

> 🔁 **Reinicie a API antes de validar** (Ctrl-C e suba de novo) para o store
> voltar a ficar vazio — assim o POST de avaliação cria a tarefa de `id=1`. Se
> você já tinha criado tarefas à mão, o `id` não será 1 e o GET/PUT falham.

---

## 7. Rubrica (100 pts)

### Estrutura no GitHub — 30 pts
| Critério | Pts | Como passa |
|---|---|---|
| `repo_publico` | 5 | repositório existe e é **público** |
| `tem_app_main` | 6 | `app/main.py` existe e não está vazio |
| `tem_requirements` | 5 | `requirements.txt` existe e não está vazio |
| `tem_readme` | 2 | `README.md` presente |
| `commits_min` | 6 | ≥ 3 commits (trabalho incremental, não um dump único) |
| `pr_descritivo` | 6 | ≥ 1 PR com título descritivo (≥ 12 chars, não-placeholder) |

### Execução real da API — 70 pts
| Critério | Pts | Como passa |
|---|---|---|
| `api_no_ar` | 5 | `/health` responde `{"status":"ok"}` |
| `post_cria_tarefa` | 25 | POST devolve `titulo` ecoado, `concluida:false` e um `id` |
| `get_devolve_tarefa` | 15 | GET `/tarefas/1` bate com o que foi criado |
| `put_atualiza_tarefa` | 25 | PUT troca `titulo` e vira `concluida:true` |

---

## 8. Fluxo de entrega sugerido

1. `gh repo create` (repo **público**), clone, e crie o `.autograde-exercise` com `4.1`.
2. Implemente em commits pequenos (`feat: POST /tarefas`, `feat: GET /tarefas/{id}`…).
3. Abra um **PR** com título descritivo (ex.: `feat: API REST de tarefas com POST/GET/PUT`).
4. Suba a API, **reinicie-a**, e rode `autograde validar 4.1`.
5. Confira o boletim; ajuste; só então submeta.

---

## 9. Troubleshooting

| Sintoma | Causa provável | Correção |
|---|---|---|
| Todos os critérios de shell zerados | API não está no ar em `localhost:8000` | Suba `uvicorn app.main:app --port 8000` e deixe rodando |
| `get_devolve_tarefa` falha mas POST passa | o `id` criado não foi 1 (store sujo) | reinicie a API antes de validar |
| `concluida` reprovado | campo veio como string `"false"` ou nome errado | use booleano JSON `false`/`true`, campo `concluida` |
| `curl: not found` | `curl` ausente | instale o `curl` (vem por padrão no macOS/Linux; no Windows use o do Git Bash) |
| estrutura no GitHub zerada | repo privado ou caminho errado | torne público; arquivo precisa ser exatamente `app/main.py` |

---

## 10. Por que execução real (e não "confiar no código")?

O autograder é um **juiz independente**: ele não lê a sua intenção, ele **mede o
comportamento**. Uma API que "parece certa" mas devolve `concluida` como string,
ou erra o 404, **falha** — exatamente como falharia para um cliente real. Esse é
o valor de um contrato: ele é verificável.

> Continuação natural: no **Exercício 4.2** você vai pôr um **MCP server** na
> frente desta API, para que um agente de IA consiga criar e listar tarefas
> sem nunca falar HTTP diretamente.


## (Opcional - Nao vale nota) Por curiosidade, peça para o agente de IA codificar uma interface web que usa a API
Você vai ficar orgulhoso do que acabou de construir