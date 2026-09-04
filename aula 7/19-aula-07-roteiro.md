# Aula 7 — Roteiro minuto a minuto

**Sábado, 26/09/2026 — 10h00 às 12h00**
**Tema:** Dicionários e JSON — dado com nome
**Ambiente:** Google Colab
**Turma:** até 6 alunos

---

## Objetivo da aula

Ao final, cada aluno deve:

1. Criar e ler um dicionário, e saber que a chave é um **nome**, não uma posição
2. Usar `.get()` quando não tem certeza de que a chave existe
3. Percorrer com `.items()`
4. Trabalhar com **lista de dicionários** — a estrutura de quase todo dado real
5. Converter dicionário ↔ JSON com `dumps` e `loads`
6. **Ler a resposta de uma API de IA sem susto**

**A dívida que abre a aula:** *"duas listas em paralelo quebram no dia em que alguém ordenar uma e esquecer a outra."*

| Gancho | De onde veio | Onde é pago |
|---|---|---|
| A Ana com a nota do Bruno | Fim da Aula 6 | Abertura, 10h00 — rodando o bug ao vivo |
| O Projeto 1 pronto | Tarefa da Aula 6 | Abertura, 10h00 — projete os jogos |

> **A frase que amarra a aula:**
> **a lista guarda ordem; o dicionário guarda nome.**

---

## ⚠️ Bloco de abertura — troque pelo que a turma trouxer

Hoje a abertura tem duas partes e a **primeira é dos alunos**: os jogos do Projeto 1. Reserve os
6 minutos de verdade — é o primeiro trabalho de que eles têm orgulho, e cortar isso custa mais
caro do que qualquer conteúdo que você ganhe.

---

## Antes de começar

- [ ] Notebook testado e link curto no quadro (`21-aula-07-notebook.ipynb`)
- [ ] Slides abertos (`20-aula-07-slides.html`) — `T` se a sala estiver clara
- [ ] **Abrir os jogos que chegaram no grupo** e escolher dois ou três para rodar na tela
- [ ] Ter o boletim da Aula 5 aberto, para mostrar o bug ao vivo
- [ ] Avisar que hoje **eles passam a ler a língua que toda API de IA fala** — isso vende a aula

---

## Roteiro

### 10:00 — 10:14 | Os jogos, e a dívida das duas listas (14 min)

**Os jogos primeiro (7 min).** Rode dois ou três na tela e **deixe a pessoa explicar o que
acrescentou**. Não corrija código em público — elogie a ideia. Se alguém fez placar ou
dificuldade, destaque: isso é escopo próprio, não cópia.

Se ninguém entregou, não faça sermão: rode o seu, jogue com a turma, e mande a tarefa de novo.

**Agora a dívida (7 min).** Sem slide. Abra o boletim da Aula 5:

> "Semana passada eu falei que carregar duas listas em paralelo quebra no dia em que alguém
> ordenar uma e esquecer a outra. Vamos ver acontecer."

```python
nomes = ["Ana", "Bruno", "Carla"]
notas = [8.5, 4.0, 9.5]

notas.sort(reverse=True)      # ordenei só as notas

for i in range(len(nomes)):
    print(f"{nomes[i]:6s} {notas[i]}")
```

```
Ana    9.5
Bruno  8.5
Carla  4.0
```

Deixe a turma achar o erro. Alguém vai ver.

> "A Ana ficou com o 9.5, que é da Carla. A Carla ficou com o 4.0, que é do Bruno. E **nenhum
> erro vermelho apareceu** — de novo. É o mesmo tipo de bug da ordem dos `elif` e do acumulador
> zerado."

> "O problema de raiz é este: **a lista guarda ordem. Ela não guarda nome.**"

---

### 10:14 — 10:32 | Dicionário (18 min)

```python
aluno = {"nome": "Ana", "nota": 8.5, "faltas": 2}

print(aluno["nome"])    # Ana
print(aluno["nota"])    # 8.5
```

Escreva no quadro a comparação — é o que faz a ficha cair:

| | Como você pede | O que precisa saber |
|---|---|---|
| lista | `alunos[0]` | **em que posição** está |
| dicionário | `aluno["nome"]` | **o nome** da informação |

> "`aluno['nota']` continua funcionando mesmo que a ordem mude, mesmo que apareçam dez campos
> novos, mesmo que alguém ordene qualquer coisa. **O valor está amarrado ao nome, não à posição.**"

Vocabulário: **chave** e **valor**. Escreva no quadro.

**Mudar e criar (5 min):**

```python
aluno["nota"] = 9.0                    # muda
aluno["email"] = "ana@exemplo.com"     # CRIA
print(len(aluno))       # 4
print("nota" in aluno)  # True
```

Um contraste que vale destacar:

> "Numa lista de 3 itens, `notas[10] = 5` dá `IndexError`. Aqui não — atribuir numa chave que não
> existe simplesmente **cria** a chave. O dicionário cresce sozinho."

E note que o `in` é o mesmo da Aula 3, o do detector de spam.

---

### 10:32 — 10:45 | Os dois erros do dicionário (13 min)

**💥 Quebrando de propósito nº 1:**

```python
print(aluno["telefone"])
```

```
KeyError: 'telefone'
```

> "**`KeyError` é o `IndexError` do dicionário.** Quer dizer: essa chave não existe aqui."

Agora o `.get()`, e venda ele bem:

```python
aluno.get("telefone")                    # None, sem quebrar
aluno.get("telefone", "não informado")   # com um padrão
```

> "`.get()` é o que vocês usam quando **não têm certeza** de que a chave está lá. E, spoiler: isso
> é a regra, não a exceção, no dia em que o dado vier de fora — de um arquivo, de um formulário,
> de uma API. Guardem, porque daqui a quinze minutos vocês vão mexer numa resposta de IA."

**💥 Quebrando de propósito nº 2:**

```python
print(aluno[0])
```

```
KeyError: 0
```

> "**Dicionário não tem índice.** O `[0]` não é 'o primeiro item' — é uma tentativa de buscar a
> chave `0`, que não existe."

Dê a regra de leitura, ela é útil para sempre:

> "Quando vocês virem `[ ]` em Python, olhem **o que está dentro**. Número é posição, e a coisa é
> uma lista. Texto é nome, e a coisa é um dicionário."

---

### 10:45 — 11:00 | Percorrer (15 min)

```python
for chave in aluno:
    print(chave, "->", aluno[chave])
```

> "O `for` sozinho passa pelas **chaves**, não pelos valores. Isso pega gente."

Os três jeitos:

```python
print(list(aluno.keys()))
print(list(aluno.values()))

for chave, valor in aluno.items():
    print(f"{chave:8s} = {valor}")
```

> "`.items()` é o que vocês mais vão usar. Repare que o `for` agora tem **dois nomes** antes do
> `in` — ele entrega duas variáveis por volta."

Se a turma estranhar os dois nomes, não aprofunde agora — diga que é "desempacotar" e que volta
na Aula 23. Não vale gastar cinco minutos aqui.

---

### 11:00 — 11:10 | Pausa (10 min)

---

### 11:10 — 11:26 | Lista de dicionários (16 min)

Anuncie o peso disso:

> "Esta combinação é, sem exagero, o formato de **quase todo dado** que vocês vão encontrar na
> vida: uma lista, e cada item um dicionário."

```python
turma = [
    {"nome": "Ana",   "nota": 8.5, "faltas": 2},
    {"nome": "Bruno", "nota": 4.0, "faltas": 7},
    {"nome": "Carla", "nota": 9.5, "faltas": 0},
]

for a in turma:
    print(f"{a['nome']:6s} {a['nota']:4.1f}  {a['faltas']} faltas")
```

Avise da aspa antes que morda:

> "Fora é aspa dupla no `f\"...\"`, dentro é aspa simples no `a['nome']`. Se usar a mesma dos dois
> lados, o Python se perde."

**As funções da Aula 6 voltam (5 min)** — mostre que elas continuam servindo:

```python
def media_da_turma(turma):
    soma = 0
    for a in turma:
        soma += a["nota"]
    return soma / len(turma)
```

```
média: 7.33
aprovados: ['Ana', 'Carla']
```

**E agora o teste que fecha a aula (4 min).** Embaralhe a turma inteira:

```python
import random
random.seed(3)
random.shuffle(turma)
```

```
Bruno  4.0
Carla  9.5
Ana    8.5
```

> "A ordem mudou completamente. **E as notas foram junto com os nomes.** Era exatamente isso que
> estava quebrado às dez da manhã."

Repita a frase da aula aqui:

> **"A lista guarda ordem. O dicionário guarda nome."**

---

### 11:26 — 11:42 | JSON (16 min)

**A motivação primeiro (2 min):**

> "Um dicionário só existe **dentro** do Python, na memória. Para mandar pela internet ou salvar
> num arquivo, ele precisa virar **texto**. O formato universal para isso chama-se JSON — e ele é,
> na prática, um dicionário escrito."

```python
import json

aluno = {"nome": "Ana", "nota": 8.5, "aprovado": True, "faltas": 2}
texto = json.dumps(aluno)

print(texto)
print(type(texto))
```

```
{"nome": "Ana", "nota": 8.5, "aprovado": true, "faltas": 2}
<class 'str'>
```

> "Parece igual, mas **é uma string**. Dá para mandar por rede, salvar em arquivo, colar no
> WhatsApp. E repare no detalhe: o `True` do Python virou `true`, minúsculo."

```python
de_volta = json.loads(texto)
print(de_volta["nome"])
print(type(de_volta))     # dict
```

Escreva no quadro:

```
dict  →  json.dumps  →  texto  →  json.loads  →  dict
```

> "**`dumps` escreve, `loads` lê.** O `s` no fim é de *string*."

E o `indent`, que salva a vida quando o JSON é grande:

```python
print(json.dumps(aluno, indent=2, ensure_ascii=False))
```

**💥 Quebrando de propósito nº 3 (5 min):**

```python
json.loads("{'nome': 'Ana'}")
```

```
JSONDecodeError: Expecting property name enclosed in double quotes
```

> "**JSON exige aspas duplas.** Aspa simples funciona em Python e não funciona em JSON."

> "Esse erro vai acontecer com vocês, e quase sempre pelo mesmo motivo: alguém colou um dicionário
> **Python** onde o programa esperava um **JSON**. São parecidos de propósito, e diferentes o
> suficiente para dar dor de cabeça."

Tabela no slide, e fecha o bloco:

| Python | JSON |
|---|---|
| `True` / `False` | `true` / `false` |
| `None` | `null` |
| aspas simples ou duplas | **só duplas** |

---

### 11:42 — 11:58 | 💊 Pílula de IA: toda API de IA fala JSON (16 min)

**Etapa 1 — a resposta de um modelo é isto (2 min)**

Projete o JSON inteiro da resposta. Deixe assustar por três segundos.

> "Quando um programa pede algo a um modelo de IA, o que volta pela internet é isto. **Neste
> momento é texto.** Uma string gigante e inútil."

**Etapa 2 — uma linha, e vira dicionário (1 min)**

```python
r = json.loads(resposta_texto)
print(list(r.keys()))
```

```
['id', 'object', 'model', 'choices', 'usage']
```

**Etapa 3 — navegue até o texto (5 min). É o coração.**

Vá camada por camada, perguntando à turma o que vem:

```python
type(r["choices"])                      # <class 'list'>
len(r["choices"])                       # 1
list(r["choices"][0].keys())            # ['index', 'message', 'finish_reason']
list(r["choices"][0]["message"].keys()) # ['role', 'content']
```

E o soco:

```python
print(r["choices"][0]["message"]["content"])
```

```
Uma lista guarda ordem; um dicionario guarda nome.
```

> "**Aquilo ali é a resposta da IA.** E aquela linha comprida que vocês veem em todo tutorial de
> inteligência artificial é esta:"

```python
resposta["choices"][0]["message"]["content"]
```

> "Ela não tem mistério nenhum. Da esquerda para a direita: a chave *choices*, que é uma **lista**;
> o primeiro item dela; a chave *message*; a chave *content*. **Vocês aprenderam as duas coisas
> hoje e sábado passado.**"

**Etapa 4 — o `usage` é a Aula 5 cobrando a conta (4 min)**

```
tokens que EU mandei : 24
tokens que ELE gerou : 11
total                : 35
```

```
esta chamada: US$ 0.000105
mil chamadas: US$ 0.10
```

*(O preço da célula é um exemplo — diga isso em voz alta, porque preço de API muda o tempo todo.)*

> "São os **tokens da Aula 5**. Os mesmos que fizeram `gato` virar `['g', 'ato']`. É por isso que
> português custa mais caro: mais tokens na mesma frase."

**Etapa 5 — e o que você MANDA também é JSON (4 min)**

```python
{
  "model": "...",
  "messages": [
    {"role": "system", "content": "Voce e um professor de programacao."},
    {"role": "user", "content": "O que e um dicionario em Python?"}
  ],
  "temperature": 0.7
}
```

> "Aquele `role` / `content` que aparece em **todo** tutorial de IA é isto: uma **lista de
> dicionários** — exatamente a estrutura da turma lá em cima."

E então o ponto que mais surpreende:

> "É assim que o modelo 'lembra' da conversa: **ele não lembra.** Vocês reenviam a lista inteira
> toda vez. Por isso conversa longa fica mais cara — a lista cresce, e com ela os
> `prompt_tokens`."

**Feche com as três frases:**

1. **API de IA é JSON entrando e JSON saindo.** Vocês já sabem ler e escrever os dois.
2. **Aquela linha assustadora é só chave, índice, chave, chave.**
3. **O modelo não tem memória.** O histórico é uma lista de dicionários que *vocês* reenviam.

> "Falta **uma** peça para vocês chamarem uma API de verdade: como mandar isso pela internet. É a
> Aula 12."

> **Nota para você (Unipds):** se perguntarem "e a `temperature`?", a resposta honesta na hora é
> *"controla o quanto ele arrisca em vez de pegar sempre a palavra mais provável — a gente vê na
> Aula 17"*. Depois da aula, escreva para si mesmo o que temperatura faz com a distribuição de
> probabilidade dos tokens. É uma das perguntas mais frequentes de quem usa LLM.

---

### 11:58 — 12:00 | Tarefa e a nova dívida (2 min)

1. **Agenda de contatos:** lista de dicionários com `nome`, `telefone`, `cidade`; funções `buscar()` e `de_cidade()`, usando `.get()`
2. **Refatorar o boletim** trocando as duas listas por uma lista de dicionários — depois embaralhar e confirmar que nada se perde
3. **Ler a resposta da IA:** imprimir o texto, o modelo e o total de tokens, a partir do JSON da pílula
4. **Desafio:** `contar_palavras(frase)` devolvendo um dicionário palavra → quantidade *(dica: `contagem[p] = contagem.get(p, 0) + 1`)*

Sobre o desafio, dê o gancho:

> "Isso tem nome, e é o **coração do detector de spam da Aula 3**. Vocês vão reconhecer."

**A pergunta que fica — e ela é física, faça na frente deles:**

> "Fechem o Colab agora. Abram de novo."

Espere.

> "Cadê a agenda de contatos? Cadê a turma? Cadê tudo que vocês digitaram hoje?"

> "**Sumiu.** Tudo que a gente fez em sete aulas viveu só na memória — e memória morre quando o
> programa fecha. Um dicionário lindo que desaparece toda vez não serve para nada real."

> "Como é que o dado **sobrevive**?"

*(A resposta é a Aula 8: arquivos. E é a aula em que o Python sai do navegador e entra na máquina
de vocês.)*

**Fechamento:**

> "Hoje vocês passaram a ler a língua em que toda API de IA fala. Semana que vem vocês instalam
> Python na própria máquina e param de depender do Colab. Tragam o notebook carregado — vamos
> instalar coisa. Até sábado."

---

## Se sobrar tempo

- **Dicionário dentro de dicionário:** `escola["turmas"]["manha"]` — é para lá que a pílula aponta e prepara a Aula 10
- **`del aluno["email"]`** e `aluno.pop("email")` — apagar chave
- Peça para a turma inventar o JSON de **um produto de e-commerce** com o que quiserem dentro. Discutir as escolhas de chave ensina modelagem de graça

## Se faltar tempo

Corte nesta ordem: (1) `.keys()` e `.values()` isolados (fique só com `.items()`), (2) o
`ensure_ascii`, (3) a Etapa 4 da pílula (o custo), (4) as funções da Aula 6 sobre a lista de
dicionários.

**Nunca corte:** o bug das duas listas rodando ao vivo na abertura, o `KeyError` com o `.get()`,
a **lista de dicionários com o embaralhar**, o `dumps`/`loads`, e as Etapas 1, 2, 3 e 5 da pílula
— a linha `["choices"][0]["message"]["content"]` é o que a turma vai lembrar desta aula.

---

## Depois da aula — 15 minutos

- [ ] Mandar no grupo: link do notebook e a tarefa escrita
- [ ] **Avisar no grupo para trazerem o notebook carregado e o cabo** — a Aula 8 tem instalação, e alguém sempre esquece
- [ ] Anotar em `duvidas-da-turma.md` o que te pegou
- [ ] Escrever para si mesmo, sem jargão: *o que a `temperature` faz com a distribuição de probabilidade dos tokens?*
- [ ] **Preparar-se para o caos da Aula 8:** vale saber de antemão quem tem Windows, quem tem Mac, e se alguém está sem permissão de administrador na máquina
- [ ] **Dívidas em fila:** o `abc` com `try/except` continua prometido para a **Aula 9** — é a próxima da próxima. Não deixe passar.
