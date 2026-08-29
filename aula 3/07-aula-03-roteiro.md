# Aula 3 — Roteiro minuto a minuto

**Sábado, 29/08/2026 — 10h00 às 12h00**
**Tema:** Condicionais — o programa começa a decidir sozinho
**Ambiente:** Google Colab
**Turma:** até 6 alunos

---

## Objetivo da aula

Ao final, cada aluno deve:

1. Ter resolvido **o desafio que ficou em aberto na Aula 2** (a sobra negativa) com as próprias mãos
2. Entender que uma comparação produz um `bool`, e que `if` só sabe olhar para `True`/`False`
3. Escrever `if` / `elif` / `else` com a indentação certa e na ordem certa
4. Combinar condições com `and`, `or` e `not`

**A aula tem dois ganchos, os dois vindos da semana passada.** Use os dois — eles mostram para a turma que as perguntas deles são levadas a sério e que as ferramentas se acumulam.

| Gancho | De onde veio | Onde é pago |
|---|---|---|
| A sobra negativa fica esquisita | Desafio 3 da calculadora | Abertura, 10h00 |
| E se a pessoa digitar `abc`? | Pergunta que você deixou no ar | Bônus da mão na massa, 11h35 |

---

## Antes de começar

- [ ] Notebook da aula testado e link curto no quadro (`09-aula-03-notebook.ipynb`)
- [ ] Slides abertos (`08-aula-03-slides.html`)
- [ ] Ler no grupo quem entregou o conversor de temperatura — **cite pelo nome quem tentou**
- [ ] Ter à mão a calculadora de gastos da Aula 2 para abrir e editar ao vivo

---

## Roteiro

### 10:00 — 10:15 | O desafio que ficou em aberto (15 min)

Sem slide. Abra a calculadora de gastos da Aula 2 e rode com **gasto maior que o salário**:

```
Sobra R$ -300.00
```

> "Semana passada eu pedi para vocês consertarem isso e disse que faltava uma ferramenta. Faltava mesmo. Ela chegou hoje."

Pergunte antes de mostrar: **"em português, o que o programa deveria fazer aqui?"**

Alguém vai dizer alguma versão de *"se sobrar for negativo, escrever outra coisa"*. Escreva a frase da pessoa no quadro, em português mesmo. Depois escreva embaixo:

```python
if sobra < 0:
    print("Cuidado: você está gastando mais do que ganha!")
else:
    print(f"Sobra R$ {sobra:.2f}.")
```

> "É a mesma frase. Só mudou de idioma."

Não explique a sintaxe agora — deixe a imagem. A aula inteira é destrinchar essas quatro linhas.

**Recolha também a pergunta do `abc`.** Pergunte se alguém pensou. Ouça as ideias, não corrija nenhuma, e diga: "guardem, a gente volta nisso às 11h35."

---

### 10:15 — 10:35 | Comparação e booleanos (20 min)

**A ideia central, diga assim:** antes de decidir, o computador precisa de uma pergunta que tenha resposta **sim ou não**. Em Python essa resposta é um tipo — o `bool` que vocês viram semana passada.

Rode no notebook, uma por vez, e peça o palpite antes:

```python
print(10 > 5)     # True
print(10 < 5)     # False
print(10 == 10)   # True
print(10 != 10)   # False
```

> "Repare: não imprimiu 10 nem 5. Imprimiu `True` e `False`. **A comparação vira um bool.** É esse bool que o `if` vai olhar."

Amarre com a Aula 2:

```python
print(type(10 > 5))   # <class 'bool'>
```

**Os seis comparadores** (slide, rápido): `>` `<` `>=` `<=` `==` `!=`

**Agora o erro que pega absolutamente todo mundo. Reserve tempo aqui:**

| | O que é | Exemplo |
|---|---|---|
| `=` | **guarda** um valor na variável | `idade = 18` |
| `==` | **pergunta** se são iguais | `idade == 18` |

> "Um sinal de igual manda. Dois sinais de igual perguntam."

Quebre de propósito:

```python
if idade = 18:
```

```
SyntaxError: invalid syntax
```

> "Esse erro vai acontecer com vocês. Muitas vezes. Quando ver `SyntaxError` numa linha de `if`, procure o igual solitário antes de qualquer outra coisa."

**Comparação com texto também funciona** — e vai ser útil daqui a pouco:

```python
print("banana" == "banana")   # True
print("Banana" == "banana")   # False  ← maiúscula importa!
```

---

### 10:35 — 11:00 | `if`, `else`, `elif` (25 min)

**Passo 1 — só o `if` (5 min)**

```python
idade = 20

if idade >= 18:
    print("Pode dirigir")
```

Dois detalhes, os mesmos da Aula 1:

- os **dois pontos** no fim da linha
- o **espaço** (indentação) na linha de baixo — é ele que diz o que está *dentro* do `if`

Mostre o que a indentação muda, rodando as duas versões com `idade = 15`:

```python
if idade >= 18:
    print("Pode dirigir")
    print("Boa viagem")     # dentro — só sai se for maior de idade
```

```python
if idade >= 18:
    print("Pode dirigir")
print("Boa viagem")         # fora — sai sempre
```

> "O espaço não é enfeite. É a única coisa que separa esses dois programas."

**Passo 2 — `else` (5 min)**

```python
if idade >= 18:
    print("Pode dirigir")
else:
    print("Ainda não pode")
```

> "`else` é o 'caso contrário'. Não leva condição, porque ele é tudo que sobrou."

**Passo 3 — `elif` (10 min)**

Chegue nele pelo problema. Peça: "e se eu quiser três respostas — criança, adolescente, adulto?"

```python
idade = 15

if idade < 12:
    print("Criança")
elif idade < 18:
    print("Adolescente")
else:
    print("Adulto")
```

**O ponto que quase ninguém explica e que evita horas de bug — dê ênfase:**

> "O Python lê de cima para baixo e **para no primeiro que der `True`**. Ele não testa o resto. Por isso a segunda linha pode ser só `idade < 18`: se chegou ali, já é garantido que não é menor que 12."

**Passo 4 — a ordem importa (5 min).** Mostre quebrado:

```python
idade = 5

if idade < 18:
    print("Adolescente")
elif idade < 12:
    print("Criança")      # nunca vai acontecer!
```

Rode. Sai `Adolescente` para uma criança de 5 anos. **Nenhum erro vermelho aparece.**

> "Esse é o bug mais perigoso que existe: o que não dá erro. O programa roda feliz e responde errado. Regra prática: **do mais específico para o mais geral.**"

---

### 11:00 — 11:10 | Pausa (10 min)

---

### 11:10 — 11:25 | `and`, `or`, `not` (15 min)

Chegue pelo problema outra vez: "e se eu precisar de **duas** coisas ao mesmo tempo?"

```python
idade = 25
tem_habilitacao = True

if idade >= 18 and tem_habilitacao:
    print("Pode dirigir")
else:
    print("Não pode dirigir")
```

| Operador | Quando é `True` | Em português |
|---|---|---|
| `and` | as **duas** condições verdadeiras | "e" |
| `or` | **pelo menos uma** verdadeira | "ou" |
| `not` | inverte | "não" |

Rode a tabela na cara dura, é rápido e fixa:

```python
print(True and True)    # True
print(True and False)   # False
print(True or False)    # True
print(False or False)   # False
print(not True)         # False
```

**Uma dica que vale ouro para iniciante:**

```python
# em vez de:
if tem_habilitacao == True:

# escreva:
if tem_habilitacao:
```

> "Se já é `bool`, não precisa comparar com `True`. Perguntar 'é verdade que é verdade?' é redundante."

**Bônus, se a turma estiver acompanhando bem** — o `in`, que vai reaparecer na pílula:

```python
mensagem = "promoção imperdível clique aqui"
print("promoção" in mensagem)   # True
```

---

### 11:25 — 11:42 | Mão na massa (17 min)

**Parte A — a calculadora com faixas (10 min).** Cada um no seu ritmo, você circula.

```python
salario = float(input("Salário: R$ "))
gastos = float(input("Gastos: R$ "))

sobra = salario - gastos
percentual = gastos / salario * 100

if sobra < 0:
    print(f"⚠️ Você está R$ {abs(sobra):.2f} no vermelho.")
elif percentual > 80:
    print(f"Apertado: {percentual:.0f}% do salário vai embora.")
elif percentual > 50:
    print(f"Equilibrado: {percentual:.0f}% comprometido.")
else:
    print(f"Tranquilo: sobra R$ {sobra:.2f}, {100-percentual:.0f}% do salário.")
```

`abs()` é novidade — explique em cinco segundos: "tira o sinal de menos".

**Parte B — o `abc`, finalmente (7 min).** Volte ao quadro e retome a pergunta da semana passada.

```python
texto = input("Digite um número: ")

if texto.isdigit():
    numero = int(texto)
    print(f"O dobro é {numero * 2}")
else:
    print("Isso não é um número. Tente de novo.")
```

> "`.isdigit()` devolve um `bool`: `True` se o texto só tem dígitos. Vocês perguntaram semana passada e a ferramenta chegou hoje. É assim que programar funciona — as perguntas ficam abertas e as ferramentas vão chegando."

**Seja honesto sobre o limite** — isso é importante e ensina ceticismo:

```python
print("-5".isdigit())    # False, mas -5 é um número!
print("3.14".isdigit())  # False, mas 3.14 é um número!
```

> "Resolve o caso comum, não todos. O jeito completo é `try/except`, na aula 9. Guardem mais essa."

---

### 11:42 — 11:58 | 💊 Pílula de IA: um classificador é um `if` que ninguém escreveu (16 min)

A pílula engajou na Aula 2, então hoje ela é maior — e tem um modelo de verdade rodando na tela.

**Etapa 1 — vocês já sabem fazer um classificador (4 min)**

> "Hoje vocês aprenderam a decidir. Detector de spam decide: é spam ou não é. Vamos fazer um."

```python
def eh_spam(mensagem):
    suspeitas = ["promocao", "gratis", "clique", "premio"]
    for palavra in suspeitas:
        if palavra in mensagem:
            return True
    return False

print(eh_spam("promocao gratis clique aqui"))   # True ✅
print(eh_spam("vamos almocar amanha"))          # False ✅
```

Diga que o `for` e o `def` são das aulas 4 e 6 — hoje só olhem o `if` no meio. **Ele é o classificador inteiro.**

**Etapa 2 — quebre o seu próprio classificador (3 min)**

```python
print(eh_spam("o curso e gratis para os alunos"))   # True ❌
```

> "Uma mensagem honesta virou spam. E agora? Adiciono uma exceção. Aí quebra outra. Adiciono outra. **A lista de regras nunca acaba** — e cada regra nova pode quebrar uma que já funcionava."

**Etapa 3 — o computador escreve as regras sozinho (5 min)**

Rode a célula do notebook: 16 mensagens de exemplo, marcadas como spam ou normal. Três linhas treinam o modelo.

```
spam    (100%)  <- promocao gratis clique aqui
normal   (88%)  <- vamos almocar amanha
spam     (99%)  <- clique no link e ganhe dinheiro
normal   (94%)  <- segue o relatorio em anexo
```

> "Repare no que **não** tem aqui: eu não escrevi nenhuma regra. Não listei nenhuma palavra suspeita. Só dei 16 exemplos com a resposta certa e ele descobriu sozinho."

**Etapa 4 — o modelo também erra (2 min). Não pule esta parte.**

```
spam    (61%)  <- o curso e gratis para os alunos
spam    (55%)  <- voce pode me mandar o link da reuniao
```

> "Errou igual à minha regra. **Mas olhem a confiança: 61% e 55%.** Ele está em cima do muro, e sabe disso. Minha regra dizia 'SPAM' com 100% de certeza absoluta. Um modelo que sabe que está inseguro é muito mais útil que uma regra convicta e errada."

**Etapa 5 — o desfecho, e é o ponto da aula toda (2 min)**

Rode a célula final: os mesmos exemplos **mais 8 frases normais** que usam "grátis", "clique" e "link".

```
normal  (96%)  <- o curso e gratis para os alunos
normal  (97%)  <- voce pode me mandar o link da reuniao
spam    (96%)  <- promocao gratis clique aqui       ← continua acertando o spam
```

> "Consertou. E reparem **como** consertou: eu não escrevi nenhuma regra nova. Eu dei mais exemplos. Essa é a diferença entre programação normal e machine learning — em uma você corrige escrevendo código, na outra você corrige mostrando dados."

Feche com as três frases:

1. **O `if` continua lá dentro.** O modelo decide com limiares numéricos. A diferença é que **você não escreveu os limiares** — ele calculou olhando exemplos.
2. **Por isso dado vale mais que código em IA.** Corrigir modelo é conseguir mais exemplo, não escrever mais regra. (Volta na aula 10.)
3. **O modelo diz o quanto está inseguro.** Aquele 61% é informação valiosa: sistema sério manda o caso duvidoso para um humano olhar.

**Nota para você (Unipds):** o modelo usado é Naive Bayes com contagem de palavras. Se te perguntarem "como ele calcula?", a resposta honesta na hora é *"conta quantas vezes cada palavra aparece em cada grupo e usa probabilidade — a gente vê direito no Módulo 4"*. Depois da aula, escreva para si mesmo por que ele se chama *naive*. Se travar, achou seu estudo da semana.

---

### 11:58 — 12:00 | Tarefa (2 min)

Mande escrita no grupo, aqui só anuncie:

1. **Classificador de nota:** pergunta a nota (0 a 10) e responde Reprovado / Recuperação / Aprovado
2. **Validador de senha:** pede uma senha e diz se é forte — precisa ter 8 caracteres ou mais **e** conter um número (`len(senha) >= 8`, e teste os dígitos com `any(c.isdigit() for c in senha)` — ou, mais simples para eles, peça só o tamanho e deixe o número como desafio)
3. **Desafio:** melhore o `eh_spam` da pílula com as suas próprias palavras suspeitas. Depois **tente quebrar o seu classificador** — ache uma mensagem honesta que ele marque como spam. Traga sábado.
4. **Para pensar:** o programa de faixas usa `elif` várias vezes. E se eu tivesse **200 faixas**? Copiaria 200 `elif`?

*(A resposta é a aula 4: repetição.)*

**Fechamento:**

> "Até hoje os programas de vocês faziam sempre a mesma coisa. A partir de agora eles reagem. Semana que vem eles vão repetir — e aí vocês param de escrever código na mão. Até sábado."

---

## Se sobrar tempo

- `if` dentro de `if` (aninhado) — mostre um e diga que quase sempre dá para trocar por `and`
- Peça para a turma tentar quebrar o classificador ao vivo, mensagem por mensagem. Vira competição e ensina muito.

## Se faltar tempo

Corte nesta ordem: (1) o bônus do `in`, (2) a Parte B da mão na massa (mas então **prometa** o `abc` para a aula 4 e cumpra), (3) a Etapa 4 da pílula.

**Nunca corte:** a abertura com a sobra negativa, o `=` vs `==`, a demonstração de que a **ordem do `elif` importa**, e as Etapas 2, 3 e 5 da pílula — é o arco todo dela.

---

## Depois da aula — 15 minutos

- [ ] Mandar no grupo: link do notebook e a tarefa escrita
- [ ] Anotar em `duvidas-da-turma.md` o que te pegou
- [ ] Escrever para si mesmo, sem jargão: *por que Naive Bayes se chama "naive"?* e *o que aquele 61% de confiança realmente significa?*
- [ ] Conferir quem ainda não entregou tarefa nenhuma. Terceira semana é o ponto onde se decide quem fica.
