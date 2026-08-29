# Aula 2 — Roteiro minuto a minuto

**Sábado, 22/08/2026 — 10h00 às 12h00**
**Tema:** Tipos, conversão e operadores — por que `"2" + "2"` dá `22`
**Ambiente:** Google Colab (sem instalação)
**Turma:** até 6 alunos

---

## Objetivo da aula

Ao final, cada aluno deve:

1. Saber **por que** o programa da soma quebrou na tarefa de casa — e ter consertado com as próprias mãos
2. Reconhecer os quatro tipos básicos (`str`, `int`, `float`, `bool`) e saber usar `type()` para descobrir
3. Converter entre tipos com `int()`, `float()` e `str()` sem chutar
4. Escrever texto com `f-string` em vez de somar strings

**O gancho da aula inteira é a tarefa de casa.** Você mandou eles fazerem um programa que soma dois números sabendo que ia dar errado. Hoje o erro deles vira a aula.

---

## Antes de começar

- [ ] Notebook da aula aberto e testado (`06-aula-02-notebook.ipynb` — suba no Colab e gere o link de compartilhamento)
- [ ] Link curto do notebook no quadro
- [ ] Slides abertos (`05-aula-02-slides.html`)
- [ ] Wi-Fi conferido
- [ ] Reveja no grupo quem entregou a tarefa e **quais erros apareceram** — cite pelo nome quem tentou, mesmo errando

> Turma de até 6: dá para olhar a tela de cada um. Use isso. Circule e pergunte, não conserte.

---

## Roteiro

### 10:00 — 10:20 | A tarefa de casa vira a aula (20 min)

Comece **sem slide**, direto no código.

Peça para alguém que fez a tarefa mostrar. Se ninguém fez, projete a versão "errada" que está no notebook:

```python
a = input("Primeiro número: ")
b = input("Segundo número: ")
print(a + b)
```

Digite `2` e `2`. Aparece:

```
22
```

**Deixe a sala reagir antes de explicar.** Pergunte: "por que 22 e não 4?" Escute os palpites — os palpites errados são úteis, mostram onde o modelo mental está.

Aí entregue a resposta:

> "O `input` **sempre** devolve texto. Sempre, mesmo que você digite número. E somar dois textos em Python não soma: gruda. `'2' + '2'` é `'22'` pela mesma razão que `'casa' + 'grande'` é `'casagrande'`. O computador não estava confuso. Ele fez exatamente o que vocês pediram — de novo."

Amarre com a aula 1: é o robô do sanduíche outra vez. A máquina é literal.

Agora conserte junto, projetado:

```python
a = int(input("Primeiro número: "))
b = int(input("Segundo número: "))
print(a + b)
```

**Quem acertou em casa:** peça para explicar como descobriu. Aluno explicando para aluno vale mais que você explicando — e você descobre o nível real da turma.

---

### 10:20 — 10:45 | Os quatro tipos (25 min)

**Ideia central, diga assim:** o computador não guarda só o valor. Ele guarda o valor **e o que aquilo é**. E o que ele acha que aquilo é muda o que o `+` faz.

Vá no notebook, um tipo de cada vez, rodando junto:

```python
nome = "Guilherme"     # str   — texto, sempre entre aspas
idade = 30             # int   — número inteiro
altura = 1.75          # float — número com vírgula (que em código é ponto)
maior_de_idade = True  # bool  — só True ou False
```

Agora a ferramenta de investigação — **destaque essa, é o que dá autonomia:**

```python
print(type(nome))
print(type(idade))
print(type(altura))
print(type(maior_de_idade))
```

> "Toda vez que der um erro estranho, antes de sofrer, roda `type()` na variável. Metade dos bugs de iniciante é o tipo não ser o que você achava que era."

**Três detalhes que confundem e valem parar:**

1. `"30"` e `30` **não são a mesma coisa.** Um é texto que parece número.
2. Em código o separador decimal é **ponto**, não vírgula. `1,75` não é número.
3. `True` e `False` com maiúscula, sem aspas. Com aspas viram texto.

**Micro-exercício (5 min):** no notebook tem uma lista de valores. Cada aluno adivinha o tipo antes de rodar `type()`. Errar aqui é ótimo — quem erra aprende melhor que quem acerta de sorte.

---

### 10:45 — 11:00 | Conversão: mudando de tipo (15 min)

```python
texto = "42"
numero = int(texto)     # texto vira inteiro
print(numero + 8)       # 50

n = 7
print("Eu tenho " + str(n) + " anos")   # número vira texto pra poder grudar

print(float("3.14"))    # texto vira decimal
print(int(9.99))        # 9  — corta, não arredonda!
```

Chame atenção no `int(9.99)` → `9`. Não arredonda, **corta**. É o tipo de detalhe que gera bug silencioso três semanas depois.

**Agora quebre de propósito** (o ritual da aula 1, mantenha em todas):

```python
int("abc")
```

```
ValueError: invalid literal for int() with base 10: 'abc'
```

Leia a mensagem em voz alta com eles: o tipo do erro (`ValueError` = o valor está errado, não a sintaxe), e o que ele diz — "isso não dá pra virar número". Faz sentido. Não dá mesmo.

Pergunte: **"e se o usuário digitar 'abc' no nosso programa da soma?"** Deixe a pergunta no ar. É a semente da aula 9 (try/except) e mostra que programar é antecipar o usuário.

---

### 11:00 — 11:10 | Pausa (10 min)

Turma pequena: aproveite para perguntar individualmente a quem falou pouco se está acompanhando. Quem está perdido na aula 2 e não fala, some na aula 4.

---

### 11:10 — 11:25 | f-string: o jeito certo de montar texto (15 min)

Mostre o jeito chato primeiro, para eles sentirem a dor:

```python
nome = "Ana"
idade = 25
print("Oi, " + nome + "! Você tem " + str(idade) + " anos.")
```

Conte os `+` e os `str()`. Agora:

```python
print(f"Oi, {nome}! Você tem {idade} anos.")
```

> "O `f` antes das aspas libera as chaves. O que estiver dentro de `{}` o Python calcula e coloca ali. Sem `+`, sem `str()`."

Mostre que dá para fazer conta dentro:

```python
print(f"Ano que vem você faz {idade + 1}.")
print(f"Média: {(7 + 9) / 2}")
```

A partir de hoje, **f-string é o padrão da turma.** Some com metade dos erros de concatenação.

---

### 11:25 — 11:45 | Mão na massa: calculadora de gastos (20 min)

Exercício guiado no notebook. Turma pequena → deixe cada um no seu ritmo e circule.

```python
nome = input("Seu nome: ")
salario = float(input("Quanto você recebe por mês? R$ "))
aluguel = float(input("Quanto paga de aluguel? R$ "))
mercado = float(input("Quanto gasta de mercado? R$ "))

gastos = aluguel + mercado
sobra = salario - gastos
percentual = gastos / salario * 100

print(f"{nome}, você gasta R$ {gastos:.2f} por mês.")
print(f"Isso é {percentual:.1f}% do que você ganha.")
print(f"Sobra R$ {sobra:.2f}.")
```

**O `:.2f`** — explique curto: "duas casas decimais". Não abra formatação inteira agora, só entregue a ferramenta.

**Desafios para quem terminar antes** (estão no notebook, tem sempre alguém mais rápido numa turma de 6):

1. Mostrar quanto sobra por dia (divida por 30)
2. Perguntar o gasto com transporte também
3. Se `sobra` for negativa, o texto deveria mudar — **como?** (não ensine `if`; deixe a pessoa perceber que falta ferramenta. Isso monta a aula 3 sozinho.)

**Circulando:** quando alguém travar, resista a consertar. Pergunte "roda `type()` aí, o que dá?" ou "lê a última linha do erro pra mim". Você está treinando o reflexo, não entregando a resposta.

---

### 11:45 — 11:57 | 💊 Pílula de IA: por que a máquina só entende número (12 min)

**Este bloco fecha toda aula a partir de hoje.** É a resposta para "onde isso vira IA".

Comece com a pergunta:

> "Vocês passaram a aula convertendo texto em número. Agora pensem: o ChatGPT recebe texto. Mas por dentro, um computador só sabe fazer conta. Como é que texto vira conta?"

**Resposta, e é literalmente a mesma ideia de hoje: converte.**

Mostre no notebook que a conversão texto→número existe até no nível mais bobo:

```python
print(ord("a"))    # 97
print(ord("b"))    # 98
print(ord("A"))    # 65
```

> "Cada letra já é um número por baixo. Isso não é IA ainda — é só a tabela de caracteres. Mas mostra que a ponte existe."

Agora o salto, **sem entrar em matemática**:

```python
frase = "o gato subiu no telhado"
pedacos = frase.split()
print(pedacos)

vocabulario = {"o": 1, "gato": 2, "subiu": 3, "no": 4, "telhado": 5}
numeros = [vocabulario[p] for p in pedacos]
print(numeros)   # [1, 2, 3, 4, 5]
```

Não explique a última linha em detalhe — diga "isso é da aula 5, hoje só olhem o resultado". O que importa é a imagem: **frase entrou, lista de números saiu.**

Feche com as três frases que sustentam tudo que vem no Módulo 4:

1. **Todo modelo de IA só come número.** Texto, imagem e som viram número antes de entrar.
2. **Esse processo tem nome: tokenização.** Vocês vão ver de verdade na aula 5, com strings.
3. **A diferença entre esse dicionário de brinquedo e um modelo real** é que no modelo o número não é "1, 2, 3" — é uma lista de centenas de números que carrega *significado*, e que o modelo aprendeu sozinho lendo bilhões de frases. Isso chama **embedding**, e é o Módulo 4.

> "Quando alguém falar que IA é mágica: ela é conta. Muita conta, com números que alguém precisou converter antes. Vocês fizeram a versão bebê disso hoje às 10h45."

**Nota para você:** esse é o ponto exato onde a Unipds e a sua turma se encostam. Depois da aula, escreva para si mesmo a resposta completa de "por que embedding é melhor que um dicionário de índices". Se travar, achou o que estudar na semana.

---

### 11:57 — 12:00 | Tarefa e fechamento (3 min)

**Tarefa da semana (manda no grupo hoje):**

1. Terminar a calculadora de gastos e mandar print no grupo
2. Fazer um **conversor de temperatura**: pergunta a temperatura em Celsius e mostra em Fahrenheit (`F = C * 9/5 + 32`). Use f-string, com uma casa decimal.
3. **Desafio (opcional):** um programa que pergunta nome, ano de nascimento e mostra `"Fulano, você faz X anos em 2026"`. Pense em qual tipo cada coisa precisa ser.
4. **Pergunta para pensar, não para responder:** no programa da soma, e se a pessoa digitar "abc"? Tragam ideias sábado.

**Fechamento:**

> "Hoje vocês aprenderam a coisa mais chata e mais importante do curso: que o computador diferencia texto de número, e que isso é o que quebra 90% dos programas de iniciante. Semana que vem o programa começa a **decidir** sozinho. Até sábado."

---

## Se sobrar tempo

- Mostrar `input()` com número decimal usando vírgula → quebra. Discutir por quê.
- `int("10") + int("20")` vs `"10" + "20"` lado a lado, na cara dura.
- Perguntar o que cada um quer construir até o fim do curso. Anote — vira o Projeto Final.

## Se faltar tempo

Corte nesta ordem: (1) desafios extras, (2) a parte do vocabulário na pílula — o `ord()` sozinho já entrega a ideia, (3) o micro-exercício de adivinhar tipo.

**Nunca corte:** a correção da tarefa no início, o `quebrar de propósito` com `int("abc")`, e a pílula de IA. A pílula é o que segura a turma até o Módulo 4.

---

## Depois da aula — 15 minutos

- [ ] Mandar no grupo: link do notebook, tarefa escrita
- [ ] Anotar em `duvidas-da-turma.md` o que te pegou — especialmente na pílula de IA
- [ ] Escrever para si mesmo, em uma frase sem jargão: *o que um embedding resolve que um índice numérico não resolve?* Se não sair, é o seu estudo da semana.
- [ ] Conferir quem não entregou a tarefa 1. Turma de 6 — dá para chamar individualmente, e é a hora certa de fazer isso.
