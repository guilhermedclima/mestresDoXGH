# Aula 5 — Roteiro minuto a minuto

**Sábado, 12/09/2026 — 10h00 às 12h00**
**Tema:** Listas e strings — abrindo o saco de coisas
**Ambiente:** Google Colab
**Turma:** até 6 alunos

---

## Objetivo da aula

Ao final, cada aluno deve:

1. Pegar um item pela posição e um pedaço por fatiamento, em lista **e** em string
2. Saber que a contagem começa no zero e que o fim da fatia não entra
3. Distinguir método que **muda** a coisa de método que **devolve** algo novo — o erro mais caro do dia
4. Usar `split()` e `join()` para ir de texto para lista e voltar
5. Ter visto que partir texto em pedaços **é** tokenização, e que os pedaços saem da contagem

**A dívida que abre a aula:** *"o `for` só anda do começo ao fim — e se eu quiser o terceiro? ordenar? o maior?"*

| Gancho | De onde veio | Onde é pago |
|---|---|---|
| "E o maior salário?" | Pergunta do fim da Aula 4 | Abertura, 10h00 — duas vezes: na mão e com `max()` |
| Tarefas: tabuada, jogo de adivinhação, contador de spam, soma dos pares | Tarefa da Aula 4 | Abertura, 10h00 |

> **A frase que amarra a aula:** *lista muda, string nunca muda.*
> Ela resolve os dois bugs do dia e economiza horas de confusão nas próximas semanas.

---

## ⚠️ Bloco de abertura — troque pelo que a turma trouxer

O roteiro abaixo assume que a dívida em aberto é a do maior salário. **Se na Aula 4 alguém
perguntou outra coisa e ficou no ar, use a pergunta dessa pessoa** — com o nome dela — e
encaixe o maior salário depois. A dívida real da turma vale mais que a dívida planejada.

---

## Antes de começar

- [ ] Notebook testado e link curto no quadro (`15-aula-05-notebook.ipynb`)
- [ ] Slides abertos (`14-aula-05-slides.html`) — `T` se a sala estiver clara
- [ ] **Rodar antes o `!pip install -q tiktoken`** da pílula. São ~10 segundos e você não quer
      isso às 11h50. Se falhar, veja "se faltar tempo" — a pílula sobrevive sem ele.
- [ ] Ver quem entregou o **jogo de adivinhação** — é o esboço do Projeto 1 da semana que vem
- [ ] Ter a lista `salarios` da Aula 4 pronta para colar

---

## Roteiro

### 10:00 — 10:12 | A dívida: e o maior salário? (12 min)

**Recolha a tarefa primeiro (5 min).** Comece pelo desafio dos pares (`2550`) porque é o de
resposta única — dá para conferir na hora e todo mundo vê se acertou. Depois pergunte quem
tentou o **jogo de adivinhação** e cite nomes. Se alguém travou num laço infinito, celebre:
é literalmente o conteúdo da semana passada acontecendo.

**Agora a dívida (7 min).** Sem slide. Escreva no quadro:

```python
salarios = [2800, 4500, 1900, 7200, 3100, 5600]
```

> "Semana passada eu falei que o `for` só sabe andar do começo ao fim. E perguntei: como eu pego
> o **maior**? Alguém pensou?"

Deixe tentarem. Alguém vai chegar perto. Escreva a resposta deles no quadro e rode:

```python
maior = salarios[0]
for salario in salarios:
    if salario > maior:
        maior = salario
print(maior)      # 7200
```

> "Repare no que isso é: **o acumulador da semana passada.** Só que em vez de somar, ele guarda
> o maior até agora. Vocês já sabiam fazer — só não sabiam que sabiam."

Deixe o elogio pousar. Só então:

```python
print(max(salarios))                    # 7200
print(salarios[2])                      # 1900   ← o terceiro
print(sorted(salarios, reverse=True))   # do maior para o menor
```

> "Cinco perguntas, cinco linhas. **Mas não joguem fora o laço de cima.** Saber fazer na mão é o
> que faz vocês entenderem o que o `max()` faz por dentro — e é o que salva no dia em que a
> pergunta for esquisita demais para existir função pronta."

---

### 10:12 — 10:28 | Índice (16 min)

```python
alunos = ["Ana", "Bruno", "Carla", "Diego", "Elis"]

print(alunos[0])    # Ana
print(alunos[2])    # Carla
print(alunos[-1])   # Elis
```

**Use nomes de alunos da turma.** Funciona muito melhor — eles se veem na lista.

Desenhe no quadro, e deixe desenhado a aula toda:

```
   Ana   Bruno   Carla   Diego   Elis
    0      1       2       3       4
   -5     -4      -3      -2      -1
```

> "Duas coisas: a contagem começa no **zero**, e o negativo conta de trás para frente. `[-1]` é
> sempre o último, não importa o tamanho da lista."

**💥 Quebrando de propósito nº 1.** Peça: "a lista tem cinco nomes — imprimam o quinto."

```python
print(alunos[5])
```

```
IndexError: list index out of range
```

> "São cinco itens, mas o último índice é **quatro**. Mesma história do `range` que mordeu vocês
> semana passada: a contagem começa no zero, então o último é sempre `len(lista) - 1`."

> "`IndexError` quer dizer sempre a mesma coisa: **você pediu uma posição que não existe.** Na
> dúvida, imprima o `len()` antes."

---

### 10:28 — 10:42 | Fatiamento (14 min)

```python
print(alunos[1:3])    # ['Bruno', 'Carla']
print(alunos[:2])     # ['Ana', 'Bruno']
print(alunos[2:])     # ['Carla', 'Diego', 'Elis']
print(alunos[-2:])    # ['Diego', 'Elis']
```

Peça o palpite antes de cada uma. O `[1:3]` é o que pega todo mundo — muita gente espera três
itens.

> "**O fim não entra.** É a mesma regra do `range(1, 5)`. O Python é teimosamente consistente
> nisso, e isso é bom: uma regra só para lembrar, não duas."

O jeito de nunca mais errar, diga assim:

> "`[1:3]` é *comece no 1 e vá até chegar no 3*. Quando chega no 3, para. Não pega o 3."

**Exercício 1 (5 min).** No notebook: o segundo aluno, os três primeiros, o último, do terceiro
em diante. Circule. **Não conserte — pergunte o que a linha faz.**

---

### 10:42 — 11:00 | Mexer na lista, ordenar, e a armadilha (18 min)

**Os métodos (6 min):**

```python
turma = ["Ana", "Bruno"]

turma.append("Carla")      # ['Ana', 'Bruno', 'Carla']
turma.insert(1, "Zeca")    # ['Ana', 'Zeca', 'Bruno', 'Carla']
turma.remove("Bruno")      # ['Ana', 'Zeca', 'Carla']
```

Aponte o ponto antes do nome:

> "`turma.append` se lê *turma, acrescente*. São coisas que a **lista sabe fazer com ela mesma**.
> Isso tem nome: **método**. Anotem a palavra — vocês vão ver ela a vida inteira."

Note também que `.remove()` tira **pelo valor**, não pela posição. E relembre `len()` e o `in`
da Aula 3.

**Ordenar (5 min):**

```python
notas = [7, 4, 9, 6, 10]

print(sorted(notas))                 # [4, 6, 7, 9, 10]
print(sorted(notas, reverse=True))   # [10, 9, 7, 6, 4]
print(notas)                         # [7, 4, 9, 6, 10]  ← não mudou!
```

```python
notas.sort()
print(notas)      # [4, 6, 7, 9, 10]  ← agora mudou
```

**💥 Quebrando de propósito nº 2 (7 min). Este é o bug do dia — reserve o tempo.**

```python
notas = [7, 4, 9]
notas = notas.sort()
print(notas)
```

```
None
```

> "**A lista sumiu.** Não deu erro. Ela virou nada."

Mostre o que acontece na linha seguinte:

```
len(notas)
→ TypeError: object of type 'NoneType' has no len()
```

> "O `.sort()` ordena a lista e **não devolve nada**. Ao escrever `notas = notas.sort()`, vocês
> jogaram esse nada por cima da própria lista. O erro só aparece três linhas depois — e aí você
> procura no lugar errado."

Escreva a regra no quadro e deixe lá:

```
sorted(lista)   → DEVOLVE uma lista nova   → guarde o resultado
lista.sort()    → MUDA a lista             → NÃO guarde nada
```

> "Nunca as duas coisas ao mesmo tempo. Guardem essa — ela volta daqui a vinte minutos com as
> strings, e aí vocês vão ver que é a mesma ideia."

---

### 11:00 — 11:10 | Pausa (10 min)

---

### 11:10 — 11:28 | A virada: string também é uma sequência (18 min)

**Anuncie como virada (4 min).** É o momento mais bonito da aula:

> "Tudo que vocês fizeram na primeira hora com lista funciona com texto. Porque **uma string é
> uma sequência de caracteres**. Os mesmos colchetes, a mesma contagem do zero, a mesma fatia."

```python
nome = "Guilherme"

print(nome[0])      # G
print(nome[-1])     # e
print(nome[0:4])    # Guil
print(len(nome))    # 9
```

Deixe a turma reagir. Muita gente acha que texto é "outra coisa".

**Os métodos de string (4 min):**

```python
print("guilherme".upper())
print("GUILHERME".lower())
print("   com espaço   ".strip() + "|")
print("XGH".replace("H", "!"))
```

**💥 Quebrando de propósito nº 3 (10 min). Vale o tempo — é a metade que falta da regra.**

> "Se string é sequência igual à lista, então eu troco uma letra, né?"

```python
nome = "guilherme"
nome[0] = "G"
```

```
TypeError: 'str' object does not support item assignment
```

> "**String é imutável.** Ela não muda. Nunca. Você só consegue criar uma nova."

E agora o irmão gêmeo do bug do `.sort()`:

```python
nome = "guilherme"
nome.upper()
print(nome)        # guilherme  ← minúsculo!
```

> "Cadê o maiúsculo? O `.upper()` **devolveu** a versão maiúscula, e vocês jogaram fora. Para
> ficar, tem que guardar."

```python
nome = nome.upper()
print(nome)        # GUILHERME
```

**Agora feche a regra do dia.** Volte ao quadro onde está a regra do `sort` e complete:

| | O que faz | Guarda o resultado? |
|---|---|---|
| `lista.append(x)`, `lista.sort()` | **muda** a lista | não |
| `sorted(l)`, `texto.upper()`, `texto.strip()` | **devolve** algo novo | **sim** |

> "**Lista muda. String nunca muda.** Quando um método 'não funcionou', a primeira pergunta é:
> ele muda ou devolve?"

---

### 11:28 — 11:34 | `split` e `join` (6 min)

Anuncie que estes dois voltam na pílula — eles vão prestar mais atenção.

```python
frase = "programar e a arte de resolver problemas com codigo"

palavras = frase.split()     # texto → lista
print(palavras)              # ['programar', 'e', 'a', ...]
print(len(palavras))         # 9

print("-".join(["a", "b", "c"]))     # lista → texto  →  a-b-c
print("2026-09-12".split("-"))       # ['2026', '09', '12']
```

> "`split` parte, `join` cola. São a ponte entre texto e lista — e vocês vão usar isso mais do
> que imaginam."

---

### 11:34 — 11:42 | Mão na massa (8 min)

**Rode o boletim e comente enquanto sai:**

```
Turma: 5 alunos
Média da turma: 7.10
Maior nota: 9.5  |  Menor: 4.0

Ana        8.5  aprovado
Bruno      4.0  reprovado
Carla      9.5  aprovado
Diego      6.0  reprovado
Elis       7.5  aprovado

Aprovados: 3 de 5
Notas ordenadas: [9.5, 8.5, 7.5, 6.0, 4.0]
Top 3: [9.5, 8.5, 7.5]
```

O ponto a destacar é o `range(len(nomes))`:

> "Esse é o truque para andar em **duas listas ao mesmo tempo**: o `i` serve de posição nas duas.
> Guardem — aparece muito."

**A frase (Parte B):**

```
Maior palavra: programar (9 letras)
Iniciais: PEAADRPCC
Invertida: codigo com problemas resolver de arte a e programar
```

Pare em `iniciais += p[0].upper()`:

> "É o acumulador de sábado passado — **somando texto em vez de número**. A ideia é a mesma;
> mudou o tipo. É assim que uma ferramenta boa se paga: você aprende uma vez e usa em tudo."

E o `[::-1]` como truque: fatia com passo `-1`, anda de trás para frente.

---

### 11:42 — 11:58 | 💊 Pílula de IA: você acabou de escrever um tokenizador (16 min)

**Etapa 1 — o `.split()` é um tokenizador (2 min)**

> "Um modelo de linguagem não lê frase. Ele lê **pedaços**. Partir texto em pedaços tem nome:
> tokenização. E vocês fizeram isso hoje, na segunda hora."

```python
frase = "o curso e gratis para os alunos"
print(frase.split())
```

> "Pronto. Tokenizador. É literalmente o primeiro passo de qualquer modelo que lida com texto."

**Etapa 2 — quebre o seu tokenizador (3 min)**

```python
print("Gato!".split())    # ['Gato!']
print("gato".split())     # ['gato']
print("gato,".split())    # ['gato,']
```

> "Três coisas completamente diferentes, para o computador."

E o problema maior — conte, não mostre:

> "Se cada palavra é um item do vocabulário: português tem mais de 400 mil palavras. Mais plurais,
> conjugações, nomes próprios. Mais `kkkkk`, mais o que a internet inventar amanhã. **O
> vocabulário nunca fecha.** E palavra que o modelo nunca viu vira um buraco."

**Etapa 3 — a saída: parta em pedaços, não em palavras (5 min). É o coração.**

> "A ideia: comece com as letras soltas, conte quais pares andam juntos, e **junte o par que mais
> aparece**. Repete. Um `for`, um contador, um `max` — tudo de hoje e de sábado passado."

Rode a célula do mini-BPE:

```
Vocabulário inicial: ['a', 'c', 'd', 'e', 'g', 'm', 'o', 'r', 't']

passo 1: junta 'a' + 't' -> 'at'   (aparecia 9x)
passo 2: junta 'g' + 'at' -> 'gat'   (aparecia 6x)
passo 3: junta 'o' + 'r' -> 'or'   (aparecia 6x)
passo 4: junta 'm' + 'e' -> 'me'   (aparecia 6x)
passo 5: junta 'gat' + 'o' -> 'gato'   (aparecia 3x)
passo 6: junta 'd' + 'or' -> 'dor'   (aparecia 3x)
```

E a célula seguinte, que é o soco:

```
gato     -> ['gato']
gata     -> ['gat', 'a']
rato     -> ['r', 'at', 'o']
```

> "Olhem o que ele descobriu **sozinho**. Juntou `a`+`t`, depois `g`+`at`, depois `gat`+`o`. E aí
> `gata` ficou `['gat', 'a']` — **compartilhando o `gat` com `gato`**."

> "Ninguém ensinou português para ele. Ninguém disse que gato e gata têm a mesma raiz. Ele só viu
> que aqueles caracteres andavam juntos e economizou. Isso se chama **BPE**, e é o algoritmo que
> o GPT usa. Vocês acabaram de rodar a versão de brinquedo."

**Etapa 4 — o tokenizador de verdade (4 min)**

```python
gato           -> 2 tokens: ['g', 'ato']
cachorro       -> 3 tokens: ['c', 'ach', 'orro']
sábado         -> 2 tokens: ['s', 'ábado']
programação    -> 2 tokens: ['program', 'ação']
```

> "`gato`. Quatro letras, palavra das mais comuns do português. **Dois tokens.** Por quê? Porque
> esse tokenizador foi treinado quase todo em inglês. Ele nunca viu português suficiente para
> achar que valia guardar `gato` inteiro."

```python
PT: 10 palavras -> 12 tokens
EN:  9 palavras -> 10 tokens
```

> "A mesma frase. Doze tokens em português, dez em inglês."

**Etapa 5 — por que isso importa para vocês (2 min)**

1. **A API cobra por token.** O mesmo texto em português custa mais caro. Não é impressão.
2. **O limite de contexto é em token.** "128 mil tokens" é bem menos texto em português.
3. **O modelo nunca vê a sua letra.** Vê esses pedaços virarem número — e aí volta a Pílula da
   Aula 2: *todo modelo só entende número*.

**Feche com as três frases:**

1. **Tokenizar é partir texto em pedaços.** Vocês fizeram com `.split()`.
2. **Partir em palavras não escala** — o vocabulário nunca fecha.
3. **Partir em pedaços frequentes escala** — e quem escolhe os pedaços é a **contagem**, não uma
   regra escrita por alguém. Igual à Aula 3: os dados escreveram.

> **Nota para você (Unipds):** se perguntarem "quantos pedaços o GPT tem?", a resposta honesta é
> *"na casa das 100 mil, e o número é escolhido antes do treino"*. Depois da aula, escreva para si
> mesmo por que aumentar o vocabulário encurta a sequência mas engorda o modelo. É a troca central
> de tokenização.

---

### 11:58 — 12:00 | Tarefa e a nova dívida (2 min)

1. **Boletim seu:** duas listas (nomes e notas), imprimir média, **o nome** de quem tirou a maior, e os aprovados
2. **Contador de palavras:** pede uma frase e responde quantas palavras, qual a maior, e a frase invertida
3. **Analisador de senha:** quantas letras, quantos números, e se tem 8+ caracteres
4. **Desafio:** dizer se uma palavra é **palíndromo** (`arara`, `radar`). Tem solução de uma linha com o que viram hoje

**A pergunta que fica — deixe no ar de propósito:**

> "Olhem o boletim e o contador de palavras. Nos dois vocês escreveram o **mesmo tipo de bloco**:
> percorre, compara, guarda o maior. E na tarefa vão escrever de novo. Três vezes o mesmo trecho,
> em três lugares."

> "No dia em que a regra mudar — digamos que aprovado passe a ser 6, e não 7 — vocês vão lembrar
> dos três lugares? Vão achar todos?"

*(A resposta é a Aula 6: funções. E é de lá que sai o Projeto 1.)*

**Fechamento:**

> "Hoje vocês abriram o saco de coisas. Semana que vem vocês vão **dar nome a pedaços de código**
> e parar de repetir — e no fim da aula sai o primeiro projeto de vocês, um jogo que funciona.
> Até sábado."

---

## Se sobrar tempo

- **`.count()` e `.index()`** em lista e string — dois métodos úteis e de graça, agora que a ideia de método está clara
- **Lista dentro de lista:** `[[1, 2], [3, 4]]` e `matriz[0][1]`. Prepara o terreno para a Aula 10
- Peça para a turma **quebrar o mini-BPE**: mudar o corpus e ver que pedaços aparecem. Vira competição e ensina que o resultado depende do dado

## Se faltar tempo

Corte nesta ordem: (1) o `join` com separador escolhido, (2) a Parte B da mão na massa, (3) a
Etapa 4 da pílula — **e o tiktoken é o primeiro a cair se der qualquer problema de instalação**;
o mini-BPE sozinho já entrega o argumento.

**Nunca corte:** a abertura com o maior salário feito **na mão** antes do `max()`, o `IndexError`,
o bug do `notas = notas.sort()` com a regra escrita no quadro, a imutabilidade da string, e as
Etapas 1, 2 e 3 da pílula — é o arco todo dela.

> Se estiver atrasado às 11h30, corte a mão na massa inteira e mande como tarefa. A pílula não
> se negocia: é o que a turma veio buscar.

---

## Depois da aula — 15 minutos

- [ ] Mandar no grupo: link do notebook e a tarefa escrita
- [ ] Anotar em `duvidas-da-turma.md` o que te pegou
- [ ] **Separar os jogos de adivinhação que chegaram** — na Aula 6 o Projeto 1 parte deles, e é muito melhor partir do código de um aluno do que de um seu
- [ ] Escrever para si mesmo, sem jargão: *por que aumentar o vocabulário encurta a sequência mas engorda o modelo?*
- [ ] **Dívidas em fila:** o `abc` com `try/except` continua prometido para a **Aula 9**. Cumprir.
