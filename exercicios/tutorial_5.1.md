# Tutorial 5.1 — SQL por prompt

## 1. A ideia

SQL é a linguagem que faz perguntas a um banco de dados. Neste exercício você vai
treinar a **fazer a pergunta certa em português** — um *prompt* — de modo que um
modelo de linguagem (LLM) traduza esse prompt na consulta SQL correta.

> Você **não escreve SQL**. Você escreve o **prompt**. Se o prompt for claro o
> bastante para o LLM gerar a consulta certa, você ganha a nota cheia da tarefa.

### Como a nota é calculada

Para cada tarefa, ao rodar `autograde validar 5.1`:

1. você digita um prompt;
2. o backend envia o seu prompt **+ o esquema das tabelas** ao LLM, que devolve
   um `SELECT`;
3. o backend **executa** esse `SELECT` na base (somente leitura) e compara o
   resultado com o **gabarito** (o resultado da consulta correta do professor, na
   mesma base);
4. resultado igual → **nota cheia**; resultado diferente → **0**.

A ordem das linhas **não importa**.

> ⚠️ **Não tente cravar a resposta.** Um prompt como "retorne 11" faz o LLM gerar
> `SELECT 11`, que **não consulta nenhuma tabela** e é automaticamente reprovado,
> mesmo que o número esteja certo. Descreva **o que calcular sobre os dados**.

## 2. A base de dados — sample database do W3Schools

A base é **exatamente** a do editor "Try MySQL" do W3Schools. Você pode treinar
seus prompts/consultas lá antes de validar — os dados são idênticos:

👉 https://www.w3schools.com/mysql/trymysql.asp?filename=trysql_select_all

São 8 tabelas. As que você vai usar neste exercício:

**`Products`** (77 produtos)

| coluna | descrição |
|---|---|
| `ProductID` | id do produto |
| `ProductName` | nome |
| `SupplierID` | id do fornecedor |
| `CategoryID` | id da categoria (1 = Beverages, 2 = Condiments, …) |
| `Unit` | embalagem |
| `Price` | preço unitário |

**`Customers`** (91 clientes)

| coluna | descrição |
|---|---|
| `CustomerID` | id |
| `CustomerName` | nome da empresa |
| `ContactName` | contato |
| `Address`, `City`, `PostalCode`, `Country` | endereço |

As outras tabelas (`Categories`, `Orders`, `OrderDetails`, `Employees`,
`Shippers`, `Suppliers`) também estão na base, caso queira explorar — mas as
tarefas abaixo só precisam de `Products` e `Customers`.

## 3. As tarefas

| # | Conceito | Pede |
|---|---|---|
| 1 | `SELECT` + `WHERE` | `ProductName` e `Price` dos produtos com `CategoryID = 1` |
| 2 | `COUNT` | quantos clientes têm `Country = 'France'` |
| 3 | `SUM` | soma de `Price` dos produtos com `CategoryID = 1` |
| 4 | `GROUP BY` | para cada `Country`, a contagem de clientes |
| 5 | `GROUP BY` + `SUM` | para cada `CategoryID`, a soma dos `Price` dos produtos |

## 4. Como escrever um bom prompt

- **Diga qual tabela e quais colunas.** "na tabela `Products`, …".
- **Seja específico no filtro.** "…apenas os produtos cujo `CategoryID` é 1".
- **Peça o formato do resultado.** "retorne o país e a quantidade de clientes,
  um por linha" (tarefa 4).
- **Use os nomes exatos** das colunas e dos valores (ex: `'France'`, com a
  primeira maiúscula).

Exemplo de prompt para a **tarefa 3**:

> Na tabela `Products`, some a coluna `Price` considerando apenas as linhas em
> que `CategoryID` é igual a 1.

## 5. Preparar o repositório

```bash
# crie um repositório público (pode ser pelo site do GitHub) e clone-o
echo "5.1" > .autograde-exercise
# copie o README modelo (README_modelo_5.1.md) para README.md e preencha seu nome
git add . && git commit -m "exercicio 5.1" && git push
```

O repositório precisa **existir e ser público** (cross-check de 10 pts). O resto
da nota (90 pts) vem dos prompts.

## 6. Validar

De dentro do repositório:

```bash
autograde validar 5.1
```

Responda cada tarefa com o seu prompt. O boletim mostra, por tarefa, se o
resultado bateu e **qual SQL** o LLM gerou a partir do seu prompt — compare com o
que você queria; é assim que se aprende a refinar o prompt. Você pode validar
várias vezes (respeitando o limite diário).

## 7. Observações

- A correção usa um LLM para gerar o SQL; prompts ligeiramente diferentes podem
  gerar o mesmo SQL. Foque em **clareza**, não em "palavra mágica".
- Se a infraestrutura do LLM estiver indisponível no momento, a tarefa recebe
  nota cheia **provisória** (marcada como tal no boletim) e é recorrigida depois.
