# Exercício 4.2 — MCP server local que consome a sua API (4.1)

> **Módulo 3 — Construindo interfaces (Aula 6: MCP).**
> Você vai pôr um **MCP server** na frente da API que construiu no 4.1, expondo
> *tools* que um agente de IA pode chamar. O autograder **executa o seu MCP
> server de verdade**: ele sobe o server, lista as tools, chama uma delas e
> confere que o resultado veio da sua API.

**Total: 100 pontos** — 35 de estrutura no GitHub + 65 de execução real do MCP.

---

## 1. Objetivo

No 4.1 você construiu a API REST de uma **aplicação de TODO list**. Mas um LLM
não fala HTTP — ele fala **MCP** (Model Context Protocol). Neste exercício você
cria a **camada do meio**: um MCP server que expõe duas *tools* da TODO list e as
implementa **chamando a sua API REST**.

```
  Agente / LLM   ──MCP──▶   seu MCP server   ──HTTP──▶   sua API 4.1 (localhost:8000)
```

É o conceito da Aula 6: o MCP **padroniza** como o agente pede coisas; a sua API
continua sendo a fonte do dado. O MCP é o adaptador entre os dois mundos.

---

## 2. Pré-requisitos

- Ter o **Exercício 4.1 concluído** (a API roda em `http://localhost:8000`).
- Python ≥ 3.10 e `pip`.
- SDK oficial de MCP para Python: `mcp` (inclui `FastMCP` e o cliente stdio).
- `autograde` instalado e logado.

> Durante a validação do 4.2, a **API do 4.1 precisa estar no ar** em
> `localhost:8000`, porque as tools do MCP chamam ela.

---

## 3. O que construir

### 4.1 `servidor_mcp.py` — o MCP server

Um server MCP (stdio) que expõe **duas tools**, ambas chamando a sua API:

| Tool | Assinatura | O que faz |
|---|---|---|
| `criar_tarefa` | `criar_tarefa(titulo: str) -> dict` | faz `POST /tarefas` e devolve a tarefa criada |
| `listar_tarefas` | `listar_tarefas() -> list` | faz `GET /tarefas` e devolve a lista |

Esqueleto com **FastMCP** (você completa a `listar_tarefas`):

```python
# servidor_mcp.py
import httpx
from mcp.server.fastmcp import FastMCP

API = "http://localhost:8000"
mcp = FastMCP("tarefas-mcp")


@mcp.tool()
def criar_tarefa(titulo: str) -> dict:
    """Cria uma tarefa na API e devolve o objeto criado."""
    resp = httpx.post(f"{API}/tarefas", json={"titulo": titulo}, timeout=10)
    resp.raise_for_status()
    return resp.json()


# TODO: implemente a tool listar_tarefas() -> list que faz GET /tarefas


if __name__ == "__main__":
    mcp.run()  # transporte stdio por padrão
```

### 4.2 `cliente_teste.py` — o teste que o autograder roda

Um script que sobe o `servidor_mcp.py` via **stdio**, exercita as tools e
imprime no stdout **um único envelope JSON** (e nada mais):

```json
{
  "tools": ["criar_tarefa", "listar_tarefas"],
  "criar_resultado": {"id": 1, "titulo": "tarefa via mcp", "concluida": false},
  "listar_resultado": [ {"id": 1, "titulo": "tarefa via mcp", "concluida": false} ]
}
```

Esqueleto com o cliente stdio do SDK:

```python
# cliente_teste.py
import asyncio
import json

from mcp import ClientSession, StdioServerParameters
from mcp.client.stdio import stdio_client


async def main() -> dict:
    params = StdioServerParameters(command="python", args=["servidor_mcp.py"])
    async with stdio_client(params) as (read, write):
        async with ClientSession(read, write) as session:
            await session.initialize()

            tools = await session.list_tools()
            nomes = [t.name for t in tools.tools]

            criar = await session.call_tool("criar_tarefa", {"titulo": "tarefa via mcp"})
            listar = await session.call_tool("listar_tarefas", {})

            return {
                "tools": nomes,
                "criar_resultado": json.loads(criar.content[0].text),
                "listar_resultado": json.loads(listar.content[0].text),
            }


if __name__ == "__main__":
    print(json.dumps(asyncio.run(main())))
```

> O formato exato do `call_tool(...).content` pode variar com a versão do SDK —
> ajuste o parsing para que o envelope final tenha **`criar_resultado` como um
> objeto** com `id`/`titulo`/`concluida`, e **`listar_resultado` como uma
> lista**. É isso que o autograder confere.

---

## 4. Estrutura do repositório

```
seu-repo/
├── servidor_mcp.py        # MCP server (tools que chamam a API 4.1)
├── cliente_teste.py       # imprime o envelope JSON que o autograder lê
├── requirements.txt       # mcp, httpx
├── README.md
└── .autograde-exercise    # conteúdo: 4.2
```

`requirements.txt`:

```
mcp
httpx
```

---

## 5. Como rodar localmente

1. **Terminal A** — suba a API do 4.1 (reinicie para o store ficar limpo):
   ```bash
   uvicorn app.main:app --port 8000     # no repo do 4.1
   ```
2. **Terminal B** — no repo do 4.2:
   ```bash
   pip install -r requirements.txt
   python cliente_teste.py
   ```
   Deve imprimir o envelope JSON com `tools`, `criar_resultado` e `listar_resultado`.

---

## 6. Como o autograder avalia (execução real)

Com a API do 4.1 no ar, no repo do 4.2:

```bash
autograde validar 4.2
```

O CLI roda `python cliente_teste.py` (rótulo `extract: mcp_test`), captura o
envelope JSON e o backend confere:

| Critério | O que verifica |
|---|---|
| `tools_expostos` | `tools` inclui `criar_tarefa` **e** `listar_tarefas` |
| `tool_criar_chama_api` | `criar_resultado.titulo == "tarefa via mcp"`, `concluida == false`, e existe `criar_resultado.id` |
| `tool_listar_chama_api` | `listar_resultado` é lista com ≥ 1 item |

Como o `id`/`concluida` só poderiam vir da sua API REST, passar nesses critérios
**prova que a tool realmente chamou a API** — não devolveu um valor fixo.

---

## 7. Rubrica (100 pts)

### Estrutura no GitHub — 35 pts
| Critério | Pts | Como passa |
|---|---|---|
| `repo_publico` | 5 | repositório existe e é público |
| `tem_servidor_mcp` | 8 | `servidor_mcp.py` existe e não está vazio |
| `tem_cliente_teste` | 7 | `cliente_teste.py` existe e não está vazio |
| `tem_requirements` | 5 | `requirements.txt` existe e não está vazio |
| `tem_readme` | 2 | `README.md` presente |
| `commits_min` | 4 | ≥ 3 commits |
| `pr_descritivo` | 4 | ≥ 1 PR com título descritivo |

### Execução real do MCP — 65 pts
| Critério | Pts | Como passa |
|---|---|---|
| `tools_expostos` | 20 | server expõe `criar_tarefa` e `listar_tarefas` |
| `tool_criar_chama_api` | 30 | `criar_tarefa` cria via API e devolve `{id, titulo, concluida}` |
| `tool_listar_chama_api` | 15 | `listar_tarefas` devolve a lista (≥ 1) da API |

---

## 8. Troubleshooting

| Sintoma | Causa provável | Correção |
|---|---|---|
| `tool_criar_chama_api` zerado | API 4.1 não está no ar | suba `uvicorn app.main:app --port 8000` no repo do 4.1 |
| `cliente_teste.py` imprime texto extra | logs/prints antes do JSON | imprima **só** `json.dumps(...)` no stdout (mande logs pro stderr) |
| `tools_expostos` falha | nome de tool diferente | as tools precisam se chamar exatamente `criar_tarefa` e `listar_tarefas` |
| `criar_resultado` sem `id` | tool não chamou a API (devolveu stub) | a tool tem que fazer o POST de verdade e devolver o JSON da API |
| timeout na validação | server demora a subir | mantenha o `cliente_teste.py` enxuto; evite trabalho pesado no import |

---

## 9. Reflexão (para o seu README)

No 4.1 o cliente precisava falar HTTP. No 4.2, o agente só precisa saber que
existe uma tool `criar_tarefa(titulo)`. **O que o MCP escondeu?** Em uma frase:
qual detalhe da sua API o MCP tornou irrelevante para quem chama? (Esse é o
ganho de abstração da Aula 6.)
