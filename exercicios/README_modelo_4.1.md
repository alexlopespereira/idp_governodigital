# Exercício 4.1 — API REST de uma aplicação de TODO list (POST/GET/PUT)

**Aluno:** _Seu Nome Completo Aqui_
**Disciplina:** IDP-TD 2026
**Framework usado:** _Ex.: FastAPI + Uvicorn_

---

## O que esta API faz

API REST que serve de **backend de uma aplicação de TODO list** — gerencia
**tarefas** (`{id, titulo, concluida}`), com armazenamento em memória, rodando em
`http://localhost:8000`. Implementa POST (criar), GET (ler) e
PUT (atualizar), seguindo o contrato do [tutorial_4.1.md](tutorial_4.1.md#3-contrato-da-api-obrigatório--o-autograder-depende-disto).

## Estrutura

- [app/main.py](app/main.py) — implementação da API
- [requirements.txt](requirements.txt) — dependências (`fastapi`, `uvicorn`)
- [`.autograde-exercise`](.autograde-exercise) — marcador do autograder (conteúdo: `4.1`)

## Como rodar

```bash
pip install -r requirements.txt
uvicorn app.main:app --port 8000
```

## Como validar

Com a API rodando (recém-reiniciada, store vazio), em outro terminal dentro do repo:

```bash
autograde validar 4.1
```

## Endpoints

| Método | Rota | Descrição |
|---|---|---|
| GET | `/health` | liveness — `{"status":"ok"}` |
| POST | `/tarefas` | cria tarefa a partir de `{"titulo": "..."}` |
| GET | `/tarefas/{id}` | lê uma tarefa (404 se não existe) |
| GET | `/tarefas` | lista todas |
| PUT | `/tarefas/{id}` | atualiza `titulo` e `concluida` |

## Decisões de implementação

_Explique brevemente: por que esse framework, como você modelou o store, como
tratou o 404, etc._
