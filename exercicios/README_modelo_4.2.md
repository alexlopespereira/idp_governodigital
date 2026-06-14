# Exercício 4.2 — MCP server local que consome a API (4.1)

**Aluno:** _Seu Nome Completo Aqui_
**Disciplina:** IDP-TD 2026

---

## O que este projeto faz

Um **MCP server local** que expõe tools para um agente de IA criar e listar
tarefas, implementadas chamando a **API REST do Exercício 4.1**
(`http://localhost:8000`). Ver contrato em [tutorial_4.2.md](tutorial_4.2.md#3-o-que-construir).

```
Agente / LLM ──MCP──▶ servidor_mcp.py ──HTTP──▶ API 4.1
```

## Estrutura

- [servidor_mcp.py](servidor_mcp.py) — MCP server com as tools `criar_tarefa` e `listar_tarefas`
- [cliente_teste.py](cliente_teste.py) — sobe o server via stdio, exercita as tools e imprime o envelope JSON
- [requirements.txt](requirements.txt) — `mcp`, `httpx`
- [`.autograde-exercise`](.autograde-exercise) — um arquivo com esse nome e o conteúdo: `4.2`

## Tools expostas

| Tool | Assinatura | O que faz |
|---|---|---|
| `criar_tarefa` | `criar_tarefa(titulo: str) -> dict` | `POST /tarefas` na API e devolve a tarefa criada |
| `listar_tarefas` | `listar_tarefas() -> list` | `GET /tarefas` na API e devolve a lista |

## Como rodar

```bash
# Terminal A — API do 4.1 (reinicie p/ store limpo)
uvicorn app.main:app --port 8000

# Terminal B — neste repo
pip install -r requirements.txt
python cliente_teste.py
```

## Como validar

Com a API do 4.1 no ar:

```bash
autograde validar 4.2
```

## Reflexão — o que o MCP abstraiu

_Em uma frase: qual detalhe da API REST o MCP tornou irrelevante para quem chama
a tool?_
