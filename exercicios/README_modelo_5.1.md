# Exercício 5.1 — SQL por prompt

**Aluno:** _Seu Nome Completo Aqui_
**Disciplina:** IDP-TD 2026

---

## O que este exercício pede

Neste exercício você **não escreve SQL**. Para cada uma das 5 tarefas você
escreve um **prompt em português** que faça um modelo de linguagem (LLM) gerar a
consulta SQL correta. A base é o **sample database público do W3Schools** (o
mesmo do editor "Try MySQL"), então você pode treinar antes em
https://www.w3schools.com/mysql/trymysql.asp. Detalhes das tabelas, das tarefas e
de como escrever bons prompts estão em [tutorial_5.1.md](tutorial_5.1.md).

## Estrutura mínima do repositório

- [README.md](README.md) — este arquivo (preencha seu nome)
- [`.autograde-exercise`](.autograde-exercise) — marcador do autograder (conteúdo: `5.1`)

> O repositório só precisa **existir e ser público** — é a parte de cross-check
> (10 pts). Os outros 90 pts vêm dos prompts que você digita ao validar.

## Como validar

De dentro do repositório:

```bash
autograde validar 5.1
```

O comando apresenta **uma tarefa de cada vez** e pede o seu prompt. Ao final você
recebe o boletim com a nota de cada tarefa e o SQL que o LLM gerou a partir do
seu prompt.

## Meus prompts (registro pessoal — opcional)

_Anote aqui os prompts que funcionaram, para estudo._

1. ProductName e Price dos produtos da categoria 1: `...`
2. Quantos clientes da França: `...`
3. Soma dos preços dos produtos da categoria 1: `...`
4. Contagem de clientes por país: `...`
5. Soma dos preços por categoria: `...`
