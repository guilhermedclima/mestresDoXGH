# Aula 6 — Roteiro minuto a minuto

**Sábado, 19/09/2026 — 10h00 às 12h00**
**Tema:** Funções — dar nome a um pedaço de código · **Projeto 1**
**Ambiente:** Google Colab
**Turma:** até 6 alunos

---

## Objetivo da aula

Ao final, cada aluno deve:

1. Escrever uma função com `def`, parâmetro e `return`, e chamá-la
2. Saber que **`print` mostra e `return` entrega** — e que função sem `return` devolve `None`
3. Entender escopo: o que nasce dentro da função morre dentro
4. **Sair com um jogo funcionando** — o Projeto 1
5. Ter visto que um modelo de IA é uma função cujo corpo os dados escreveram

**A dívida que abre a aula:** *"você escreveu o mesmo bloco em três lugares — e quando a regra mudar?"*

| Gancho | De onde veio | Onde é pago |
|---|---|---|
| O bloco repetido três vezes | Fim da Aula 5 | Abertura, 10h00 |
| O `def` que apareceu nas Aulas 3, 4 e 5 e eu sempre empurrei | Três aulas de "isso é da Aula 6" | 10h20 — chegou |
| O jogo de adivinhação da tarefa | Tarefa da Aula 4 e da Aula 5 | Vira o **Projeto 1**, 11h10 |

> **A frase que amarra a aula.** Diga cedo e repita no fim:
> **função não é para escrever menos — é para ter um lugar só onde mexer.**

---

## ⚠️ Bloco de abertura — troque pelo que a turma trouxer

Se alguém chegou com um jogo de adivinhação **funcionando**, abra com o código dessa pessoa em
vez do exemplo do notebook. Projete, elogie, e use o código dela como matéria-prima da
refatoração das 10h. Vale muito mais do que qualquer exemplo meu — e o Projeto 1 já nasce com
dono.

---

## Antes de começar

- [ ] Notebook testado e link curto no quadro (`18-aula-06-notebook.ipynb`)
- [ ] Slides abertos (`17-aula-06-slides.html`) — `T` se a sala estiver clara
- [ ] **Ler os jogos de adivinhação que chegaram** e escolher um para projetar
- [ ] Ter o boletim da Aula 5 aberto numa aba, para refatorar ao vivo
- [ ] Avisar no começo que **hoje sai o Projeto 1** — isso muda a energia da sala

---

## Roteiro

### 10:00 — 10:20 | A dívida: o bloco repetido três vezes (20 min)

**Recolha a tarefa (6 min).** Comece pelo palíndromo — a solução de uma linha (`palavra ==
palavra[::-1]`) é bonita e todo mundo entende quando vê. Pergunte quem tentou o jogo de
adivinhação e **projete o de alguém**.

**Agora a dívida (14 min).** Projete os três blocos, um embaixo do outro, sem comentar:

```python
maior = notas[0]
for item in notas:
    if item > maior:
        maior = item

maior = salarios[0]
for item in salarios:
    if item > maior:
        maior = item

maior = palavras[0]
for item in palavras:
    if len(item) > len(maior):
        maior = item
```

Deixe feio na tela por uns segundos. Depois:

> **"O que muda de um bloco para o outro?"**

Vão dizer: *só a lista*.

> "Semana retrasada vocês aprenderam que quando **só um número muda**, o `for` resolve. Agora o
> que se repete não é uma linha — é um **bloco inteiro**. E a ferramenta para isso é esta."

```python
def maior_de(lista):
    maior = lista[0]
    for item in lista:
        if item > maior:
            maior = item
    return maior

print(maior_de(notas))      # 9.5
print(maior_de(salarios))   # 7200
```

**Não pare aqui — o melhor vem agora.** Volte à pergunta exata que você deixou:

> "Eu perguntei: e no dia em que a regra mudar? Aprovado passa a ser 6, e não 7. Vocês vão
> lembrar dos três lugares?"

```python
def situacao(nota, corte=7):
    if nota >= corte:
        return "aprovado"
    else:
        return "reprovado"

print(situacao(8.5))              # aprovado
print(situacao(6.0))              # reprovado
print(situacao(6.0, corte=6))     # aprovado
```

> "A regra mora **num lugar só**. Se mudar, muda ali. Não tem terceiro lugar esquecido para te
> morder daqui a três meses."

E então a frase da aula, devagar:

> **"Função não é para escrever menos. É para ter um lugar só onde mexer."**

---

### 10:20 — 10:32 | Anatomia, e a dívida do `def` (12 min)

```python
def saudacao(nome):
    return f"Olá, {nome}! Bem-vindo ao XGH."

print(saudacao("Ana"))
```

Desenhe no quadro, apontando cada pedaço:

```
def  saudacao (nome) :
 │       │       │   └── dois pontos (igual ao if e ao for)
 │       │       └────── PARÂMETRO: o que entra
 │       └────────────── NOME
 └────────────────────── "define"
```

Dois reconhecimentos rápidos: os **dois pontos** e a **indentação** são os mesmos do `if` e do
`for`.

**Um detalhe que confunde muita gente:** definir **não** executa.

> "Se eu só rodar o `def`, não sai nada. Ele é uma receita guardada. Só acontece quando alguém
> **chama**."

**E agora pague a outra dívida, a pequena — ela rende:**

> "Vocês já viram `def` três vezes: no `eh_spam` da Aula 3, no `erro_total` da Aula 4, no
> mini-BPE da Aula 5. E eu sempre disse 'isso é da Aula 6'. Chegou. Vocês já sabiam ler isso há
> três semanas."

---

### 10:32 — 10:50 | `return` não é `print` (18 min)

**Este é o bloco mais importante depois da abertura.** É a confusão nº 1 de quem começa.

**💥 Quebrando de propósito nº 1:**

```python
def dobro_errado(x):
    print(x * 2)

def dobro_certo(x):
    return x * 2

a = dobro_errado(5)
b = dobro_certo(5)

print("com print :", a)     # None
print("com return:", b)     # 10
```

> "As duas mostraram 10 na tela. Parecem iguais. Mas quando eu **guardo** o resultado, uma me
> entregou o 10 e a outra me entregou **nada**."

Aprofunde o buraco:

```python
print(dobro_errado(5) + 1)
```

```
TypeError: unsupported operand type(s) for +: 'NoneType' and 'int'
```

> "**Aquele `None` de novo.** É o mesmo da semana passada, quando `notas = notas.sort()` apagou
> a lista de vocês. Toda função sem `return` devolve `None`."

Escreva no quadro e deixe:

```
print  → para o HUMANO ver
return → para o PROGRAMA usar
```

> "Quase toda função que vocês escreverem deve ter `return`, não `print`. A função calcula; quem
> mostra é quem chamou."

**Feche com o segundo comportamento do `return` (3 min)** — ele encerra a função:

```python
def teste(n):
    if n > 10:
        return "grande"
    return "pequeno"
```

> "Chegou no `return`, acabou. Não precisa de `else`."

---

### 10:50 — 11:00 | Escopo (10 min)

**💥 Quebrando de propósito nº 2:**

```python
def calcular():
    resultado = 42
    return resultado

calcular()
print(resultado)
```

```
NameError: name 'resultado' is not defined
```

> "A função rodou. O `resultado` existiu. E sumiu quando ela acabou."

```python
resultado = calcular()
print(resultado)      # 42
```

**Vire isso em virtude, não em limitação:**

> "Isso não é chatice do Python — é o que faz função ser útil. Vocês podem escrever `maior_de()`
> sem se preocupar se já existe um `maior` em outro canto do programa. **Cada função é uma sala
> com a porta fechada.**"

---

### 11:00 — 11:10 | Pausa (10 min)

---

### 11:10 — 11:40 | 🎮 Projeto 1 — o jogo de adivinhação (30 min)

Anuncie:

> "É o primeiro programa de verdade de vocês. E ele fica bom justamente porque é feito de
> **funções pequenas, cada uma com um trabalho só**."

**Passo 1 — as três peças (8 min).** Escreva junto com a turma, perguntando o que cada uma
deveria devolver **antes** de escrever:

```python
import random

def sortear():
    return random.randint(1, 100)

def comparar(chute, segredo):
    if chute < segredo:
        return "maior"
    elif chute > segredo:
        return "menor"
    else:
        return "acertou"

def avaliar(tentativas):
    if tentativas <= 5:
        return "Excelente!"
    elif tentativas <= 8:
        return "Muito bom."
    else:
        return "Conseguiu!"
```

Teste as peças isoladas — isso é conteúdo, não enrolação:

```python
comparar(30, 50)   # maior
comparar(70, 50)   # menor
comparar(50, 50)   # acertou
```

> "Repare: dá para testar cada peça **sozinha**, sem jogar. Quando o programa é feito de funções,
> você acha o defeito na peça, não no programa inteiro."

**Passo 2 — o jogo (10 min).** Cada um digita o seu. Circule.

```python
def jogar():
    segredo = sortear()
    tentativas = 0

    print("Pensei num número de 1 a 100.")
    while True:
        chute = int(input("Seu chute: "))
        tentativas += 1
        resposta = comparar(chute, segredo)

        if resposta == "acertou":
            print(f"Acertou em {tentativas} tentativas! {avaliar(tentativas)}")
            break
        else:
            print(f"O número é {resposta}.")

jogar()
```

**Deixe todo mundo jogar o próprio jogo.** Esse é o momento da aula — não corra.

> "Leiam o `jogar()`. Ele se lê como a explicação do jogo, em português. Isso não é sorte: é o
> que acontece quando as funções têm nome de gente."

**Passo 3 — o robô (12 min). É a cereja, e é o que amarra a aula:**

> "Como o jogo é feito de funções, dá para **trocar o jogador** sem mexer em mais nada."

Rode o `jogar_robo()` — ele chuta sempre o meio:

```
  tentativa 1: chutei 50 -> maior
  tentativa 2: chutei 75 -> maior
  tentativa 3: chutei 88 -> menor
  tentativa 4: chutei 81 -> maior
  tentativa 5: chutei 84 -> menor
  tentativa 6: chutei 82 -> acertou
Acertei o 82 em 6 tentativas. Muito bom.
```

> "Ele usa **as mesmas** `sortear`, `comparar` e `avaliar`. Nada foi reescrito."

Agora mil partidas:

```
média: 5.78 tentativas
pior caso: 7
```

> "**Nunca mais que sete.** Nunca."

Desenhe o porquê no quadro:

```
100 → 50 → 25 → 13 → 7 → 4 → 2 → 1
```

> "Cada chute no meio corta o que sobrou pela metade. Sete cortes chegam em qualquer número, e
> `2⁷ = 128`, que é mais que 100. Isso tem nome — **busca binária** — e é uma das ideias mais
> usadas em computação. Vocês acabaram de escrever uma sem saber."

---

### 11:40 — 11:56 | 💊 Pílula de IA: um modelo é uma função (16 min)

**Etapa 1 — a caixa (2 min)**

```python
def dobro(x):
    return x * 2

dobro(21)    # 42
```

> "Entra 21, sai 42. Vocês não precisam saber o que tem dentro para usar."

**Etapa 2 — o modelo da Aula 4 é literalmente isto (4 min)**

> "Lembram do laço que ajustava o peso até o erro cair de 258 para 0.76? Ele parou em **1.681**.
> O modelo treinado é esta função aqui:"

```python
def prever_nota(horas):
    return 1.681 * horas

print(prever_nota(2))      # 3.362
print(prever_nota(3.5))    # 5.8835
```

Deixe o silêncio trabalhar.

> "É isso. **É o modelo inteiro.** Uma linha."

**Etapa 3 — a diferença está em quem escreveu o corpo (5 min). É o coração da pílula.**

Ponha as duas lado a lado no slide:

```python
def dobro(x):
    return x * 2          # ← o 2 fui EU que escrevi

def prever_nota(horas):
    return 1.681 * horas  # ← o 1.681 ninguém escreveu
```

> "O `1.681` saiu de **cinquenta voltas de um `for`** medindo erro. Nenhum humano digitou aquele
> número."

Rode a comparação antes/depois do treino:

```
2h -> chute 1.0 | treinado 3.4 | real 4
4h -> chute 2.0 | treinado 6.7 | real 7
6h -> chute 3.0 | treinado 10.1 | real 10
```

> "A chamada é **idêntica**. Só o corpo mudou. E foi o treino que escreveu o corpo."

Então a frase:

> **"Treinar um modelo é escrever o corpo de uma função sem digitar o corpo dela.** Você define o
> formato e deixa os dados escolherem os números."

**Etapa 4 — e ele erra com cara de certeza (3 min). Não pule.**

```python
prever_nota(7)    # 11.767
```

> "Nota **11.77**. Acima de 10."

> "O modelo não sabe que nota máxima é 10 — ninguém contou isso para ele. Ele só conhece a reta
> que aprendeu, e os dados iam até 6 horas. Isso se chama **extrapolar**, e é uma das formas mais
> comuns de IA falar bobagem com confiança total."

> "Guardem essa desconfiança: **um modelo não sabe que não sabe.**"

**Etapa 5 — a escala (2 min)**

| | Entra | Sai |
|---|---|---|
| `prever_nota` | horas | nota |
| o detector de spam da Aula 3 | mensagem | spam / normal |
| reconhecedor de imagem | foto | "gato" |
| **ChatGPT** | os tokens da Aula 5 | o próximo token |

> "O ChatGPT é `def responder(tokens): return proximo_token`. A forma é a mesma. O que muda é que
> o corpo dele tem **bilhões** de números — e nenhum deles foi digitado por uma pessoa."

**Feche com as três frases:**

1. **Um modelo é uma função.** Entra dado, sai resposta.
2. **A sua função vocês escreveram; a do modelo os dados escreveram.** Só isso muda.
3. **Fora do que ele viu, ele inventa** — com a mesma cara de certeza.

> **Nota para você (Unipds):** se perguntarem "então por que precisa de tanto número?", a resposta
> honesta na hora é *"porque uma reta só resolve problema de reta; para curva você precisa de mais
> peças, e é isso que camada faz — a gente vê no Módulo 4"*. Depois da aula, escreva para si mesmo,
> sem jargão, o que a função de ativação acrescenta que empilhar contas lineares não acrescenta.

---

### 11:56 — 12:00 | Tarefa e a nova dívida (4 min)

1. **Terminar o Projeto 1** com `input()` e acrescentar **uma** coisa sua: limite de tentativas, dificuldade, placar
2. **Refatorar o boletim da Aula 5** usando `media()`, `maior_de()`, `situacao()`, `contar_aprovados()` — nenhuma com `print` dentro
3. **IMC:** `calcular_imc(peso, altura)` e `faixa_imc(imc)` — duas funções, um trabalho cada
4. **Desafio:** `eh_primo(n)`, e depois todos os primos até 100

**A pergunta que fica:**

> "No boletim vocês carregam **duas listas em paralelo** — nomes e notas — amarradas pelo índice.
> Isso funciona até alguém ordenar uma e esquecer a outra. Aí a Ana fica com a nota do Bruno **e
> ninguém percebe**."

> "E se eu quisesse guardar junto de verdade: `nome: Ana, nota: 8.5, faltas: 2`? A lista guarda
> **ordem**. Ela não guarda **nome**."

*(A resposta é a Aula 7: dicionários — e é a mesma estrutura com que toda API de IA fala.)*

**Fechamento:**

> "Hoje vocês têm um programa de verdade, feito de peças, que dá para testar peça por peça. Isso
> não é mais 'exercício'. Guardem o arquivo: no fim do curso ele vai estar no portfólio de vocês.
> Até sábado."

---

## Se sobrar tempo

- **Função que chama função:** reescreva o boletim inteiro só com chamadas. É a demonstração mais forte de legibilidade que existe.
- **`return` de duas coisas:** `return maior, menor`. Abre a porta para tupla sem precisar nomear.
- Peça para a turma **medir o jogador humano**: quantas tentativas eles levam contra as 5.78 do robô? Vira competição e ensina busca binária de graça.

## Se faltar tempo

Corte nesta ordem: (1) o `return` que encerra a função, (2) o bloco das mil partidas (mostre só o
robô jogando uma vez), (3) a Etapa 5 da pílula, (4) o Passo 3 inteiro do projeto.

**Nunca corte:** a abertura com `situacao(nota, corte=7)` mostrando a regra num lugar só, o
`print` vs `return` com o `None`, **o Passo 2 do projeto — todo mundo tem que jogar o próprio
jogo** — e as Etapas 2, 3 e 4 da pílula.

> Se às 11h35 o projeto ainda não rodou para todo mundo, **sacrifique a pílula pela metade** e
> deixe o robô como tarefa. Hoje é a única aula em que o projeto vale mais que a pílula: ninguém
> esquece o dia em que fez um jogo.

---

## Depois da aula — 15 minutos

- [ ] Mandar no grupo: link do notebook e a tarefa escrita
- [ ] **Pedir que mandem o jogo pronto no grupo** — é o primeiro trabalho de que eles vão se orgulhar, e ver o dos outros puxa quem está atrasado
- [ ] Anotar em `duvidas-da-turma.md` o que te pegou
- [ ] Escrever para si mesmo, sem jargão: *o que a função de ativação acrescenta que empilhar contas lineares não acrescenta?*
- [ ] **Meia aula do curso.** Vale olhar quem sumiu e mandar mensagem individual — a Aula 6 é onde o Módulo 1 fecha e dá para retomar quem ficou para trás sem constrangimento
- [ ] **Dívidas em fila:** o `abc` com `try/except` continua prometido para a **Aula 9**
