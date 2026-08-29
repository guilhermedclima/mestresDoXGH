# Aula 4 — Roteiro minuto a minuto

**Sábado, 05/09/2026 — 10h00 às 12h00**
**Tema:** Repetição — o computador para de te fazer copiar e colar
**Ambiente:** Google Colab
**Turma:** até 6 alunos

---

## Objetivo da aula

Ao final, cada aluno deve:

1. Escrever um `for` sobre uma lista e sobre um `range`, com a indentação certa
2. Entender o **acumulador** — `total = total + x` — e saber que ele nasce **antes** do laço
3. Combinar `if` (Aula 3) dentro de `for` (Aula 4) para contar coisas
4. Escrever um `while`, reconhecer um laço infinito e **saber parar o programa**
5. Ter visto, rodando na tela, que **treinar um modelo é este mesmo laço**

**A dívida que abre a aula:** *"e se eu tivesse 200 faixas? Copiaria 200 `elif`?"*

| Gancho | De onde veio | Onde é pago |
|---|---|---|
| Os 200 `elif` | Pergunta do fim da Aula 3 | Abertura (10h00) — a intuição · Mão na massa (11h34) — a resposta completa |
| "Tentei quebrar o meu `eh_spam`" | Desafio da tarefa | Abertura, 10h00 — recolha e cite nomes |

> **A frase que amarra a aula inteira**, e que vale repetir três vezes:
> **para ter 200 faixas, a lista cresce — o código não muda uma linha.**

---

## Antes de começar

- [ ] Notebook testado e link curto no quadro (`12-aula-04-notebook.ipynb`)
- [ ] Slides abertos (`11-aula-04-slides.html`) — aperte `T` se a sala estiver clara
- [ ] Ler no grupo **quem tentou quebrar o próprio `eh_spam`** e ter as frases à mão para ler em voz alta
- [ ] Ter a calculadora com faixas da Aula 3 aberta numa aba, pronta para editar ao vivo
- [ ] Rodar uma vez a célula do gráfico da pílula — no Colab a primeira importação do `matplotlib` demora alguns segundos, e você não quer esse silêncio às 11h48

---

## Roteiro

### 10:00 — 10:15 | A dívida dos 200 `elif` (15 min)

**Primeiro, recolha a tarefa (5 min).** Comece pelo desafio, não pelas duas primeiras:

> "Quem conseguiu quebrar o próprio detector de spam? Lê a frase aí."

Leia duas ou três em voz alta e **cite os nomes**. Se alguém achou uma frase honesta marcada como spam, escreva ela no quadro — ela é a melhor propaganda possível da pílula de hoje. Se ninguém fez, não passe pano nem sermão: rode você mesmo uma e siga.

**Agora a dívida (10 min).** Sem slide ainda. Abra a calculadora com faixas da Aula 3 e diga:

> "Semana passada eu deixei uma pergunta: e se fossem 200 faixas?"

Escreva no quadro, à mão, uns seis `elif` empilhados e depois "`...` mais 194". Deixe feio de propósito. Então pergunte — **e espere a resposta, não entregue**:

> **"Olhem essas linhas. O que muda de uma para a outra?"**

Alguém vai dizer: *só o número*. É exatamente isso, e é a aula toda.

> "Quando só o número muda, o computador consegue escrever as linhas sozinho."

Rode na tela:

```python
for numero in range(200):
    print(f"faixa {numero}")
```

Role a saída até o fim, devagar, deixando as 200 linhas passarem.

> "Duzentas linhas. Eu escrevi duas."

**Não explique a sintaxe agora.** Deixe a imagem. A aula é destrinchar essas duas linhas.

---

### 10:15 — 10:35 | O `for` e o vocabulário (20 min)

**Leia o `for` em português, apontando palavra por palavra:**

```
for  aluno  in  alunos:
para cada aluno dentro de alunos, faça:
```

```python
alunos = ["Ana", "Bruno", "Carla"]

for aluno in alunos:
    print(f"Olá, {aluno}!")
```

Use os nomes de **três alunos da turma**. Funciona muito melhor.

**Sobre os colchetes:** diga em uma frase que aquilo é uma **lista**, que hoje ela é só "o saco de coisas que o `for` percorre", e que na Aula 5 vocês abrem o saco. Não gaste mais que isso — é dívida nova, e ela fecha a aula.

**Vocabulário (slide, e vale escrever no quadro).** Este é o item 4 dos princípios do curso — quem tem o nome das coisas consegue pesquisar sozinho:

| Palavra | O que é |
|---|---|
| **laço** (ou *loop*) | o bloco que repete |
| **iteração** | uma volta do laço |
| **variável do laço** | o nome que muda a cada volta (`aluno`) |
| **corpo do laço** | as linhas indentadas |

**O ponto que quase todo iniciante entende errado — 2 minutos bem gastos:**

Rode a mesma coisa trocando `aluno` por `x`.

```python
for x in alunos:
    print(f"Olá, {x}!")
```

> "`aluno` não é palavra mágica do Python. É um nome que **eu** escolhi. O Python só precisa de *algum* nome para guardar o item da vez."

**A indentação, de novo (a mesma da Aula 3).** Rode as duas e compare:

```python
for aluno in alunos:
    print(aluno)
    print("--- fim ---")     # dentro: sai 3 vezes
```

```python
for aluno in alunos:
    print(aluno)
print("--- fim ---")         # fora: sai 1 vez
```

> "Mesma história de sábado passado: o espaço não é enfeite. Ele é a única coisa que separa esses dois programas."

---

### 10:35 — 10:50 | `range` (15 min)

> "Nem sempre você tem uma lista. Às vezes você só quer repetir N vezes."

```python
for i in range(5):
    print(i)
```

Pergunte **antes** de rodar quantos números vão sair e quais. Deixe errarem — é para isso que serve.

**As duas surpresas, e elas vêm juntas:**

1. Começou no **0**
2. Foram **5 números**, e o último é o **4**

> "O número dentro do `range` é a **quantidade de voltas**, não o último valor."

```python
print(list(range(5)))        # [0, 1, 2, 3, 4]
print(list(range(1, 6)))     # [1, 2, 3, 4, 5]
print(list(range(0, 10, 2))) # [0, 2, 4, 6, 8]
```

**💥 Quebrando de propósito nº 1 — o erro de um a menos.** Peça: "escrevam um laço que conte de 1 a 5." Quase todos escrevem:

```python
for i in range(1, 5):
    print(i)
```

```
1
2
3
4
```

> "Cadê o 5? `range(1, 5)` para **antes** do 5."

> "Esse erro tem nome no mundo inteiro: *off-by-one*, erro de um a menos. Quando o seu laço rodar uma volta a menos do que devia, olhe o `range` antes de qualquer outra coisa."

**A segunda pegadinha, rápida:**

```python
print(range(5))    # range(0, 5)  ← não mostra os números!
```

> "Ele é uma *receita* para gerar números, não os números prontos. Para ver, embrulhe em `list()`."

---

### 10:50 — 11:00 | Exercício 1: tabuada (10 min)

Solte a turma. **Circule, não conserte.** Pergunte "o que essa linha faz?" e deixe a pessoa achar.

```python
for i in range(1, 11):
    print(f"7 x {i} = {7 * i}")
```

O erro esperado é `range(1, 10)` (para no 9) ou `range(10)` (começa no 0 e sai `7 x 0 = 0`). Os dois são ótimos — deixe acontecer e peça para a própria pessoa explicar o que saiu.

---

### 11:00 — 11:10 | Pausa (10 min)

---

### 11:10 — 11:24 | O acumulador (14 min)

**Este é o bloco mais importante da aula.** Ele é o que faz a pílula funcionar às 11h45.

> "Repetir só vale a pena se alguma coisa **sobreviver** de uma volta para a outra."

```python
gastos = [300, 150, 80, 420]

total = 0
for gasto in gastos:
    total = total + gasto

print(f"Total gasto: R$ {total}")     # 950
```

**Pare na linha do meio.** Escreva ela grande no quadro:

```
total = total + gasto
```

> "Em matemática isso é absurdo. `x = x + 5` não existe. Mas lembram da Aula 3? **Um sinal de igual manda.** Ele está mandando: pegue o total de agora, some o gasto desta volta, e guarde de volta."

Essa é uma amarração direta com o `=` vs `==` de sábado passado — use as palavras "manda" e "pergunta" de novo, de propósito.

Depois o atalho, porque eles vão ver em código de verdade:

```python
total += gasto      # exatamente a mesma coisa
```

**Contador — e aqui as duas aulas se encontram:**

```python
notas = [4, 8, 6, 9, 3, 7, 10, 5]

aprovados = 0
for nota in notas:
    if nota >= 7:
        aprovados += 1

print(f"{aprovados} aprovados de {len(notas)}")    # 4 aprovados de 8
```

> "Olhem: o `if` de sábado passado **dentro** do `for` de hoje. É assim que as ferramentas se acumulam."

`len()` é novidade — cinco segundos: "quantos itens tem na lista".

Aponte a indentação em **dois níveis**: o `if` dentro do `for`, o `+= 1` dentro do `if`.

**💥 Quebrando de propósito nº 2 — o bug silencioso.** Este é o irmão da ordem dos `elif`:

```python
total = 0
for gasto in gastos:
    total = 0        # ← dentro do laço
    total += gasto

print(total)
```

```
420
```

> "Deu 420. O certo era 950. E 420 é o último gasto da lista — porque o total foi zerado logo antes de somar."

> "Nenhum erro vermelho. De novo: **o bug mais perigoso é o que não dá erro.** Regra para a vida: **o acumulador nasce antes do laço.**"

---

### 11:24 — 11:38 | `while` e o loop infinito (14 min)

> "O `for` é para quando você **sabe** quantas voltas. E quando você não sabe?"

```python
n = 5
while n > 0:
    print(n)
    n = n - 1

print("Fogo!")
```

**As três partes de todo `while`** (slide — e é isso que evita o infinito):

1. **Antes:** algo que a condição consiga olhar — `n = 5`
2. **A condição:** a pergunta de sim ou não — `n > 0`
3. **Dentro:** algo que **muda** a condição — `n = n - 1`

> "A terceira é a que todo mundo esquece. E quando esquece, acontece isto."

**💥 Quebrando de propósito nº 3 — o loop infinito. Este é o ritual do dia; não pule.**

```python
n = 1
while n < 5:
    print(n)
    # esqueci o n = n + 1
```

Rode ao vivo. Deixe a tela encher por uns segundos — o desconforto é parte da aula. Depois **mostre o botão ⏹ do Colab** e pare.

> "Nenhum erro. O programa está fazendo **exatamente** o que eu mandei: repetir enquanto `n < 5`. E o `n` nunca muda."

> "Vocês vão fazer isso. Todo mundo faz, sempre. Saber parar um programa travado é habilidade, não acidente — e agora vocês sabem onde fica o botão."

**`break` — a saída de emergência:**

```python
total = 0
numero = 0

while True:              # "para sempre"
    numero += 1
    total += numero
    if total > 100:
        break

print(f"Passamos de 100 no {numero} (total {total})")
```

```
Passamos de 100 no 14 (total 105)
```

**`for` ou `while`?** (slide, 30 segundos)

| | Use quando |
|---|---|
| `for` | você **sabe** quantas voltas |
| `while` | você **não sabe** — até acertar, até o erro ficar pequeno |

> "Na dúvida, comece pelo `for`. Ele não trava."

---

### 11:38 — 11:46 | Mão na massa: a resposta completa dos 200 `elif` (8 min)

**Parte A — a calculadora agora atende a equipe inteira (3 min).** Rode e comente:

```python
salarios = [2800, 4500, 1900, 7200, 3100, 5600]

soma = 0
acima = 0

for salario in salarios:
    soma += salario
    if salario > 3000:
        acima += 1

print(f"Folha total:    R$ {soma}")
print(f"Média:          R$ {soma / len(salarios):.2f}")
print(f"Acima de 3000:  {acima} de {len(salarios)}")
```

```
Folha total:    R$ 25100
Média:          R$ 4183.33
Acima de 3000:  4 de 6
```

> "A calculadora da Aula 3 atendia uma pessoa. Essa atende seis. Ou seis mil — o código é o mesmo."

**Parte B — os 200 `elif`, resolvido de verdade (5 min).** Volte ao quadro:

> "A pergunta tem **duas** respostas, e qual serve depende das faixas."

**Resposta 1 — se as faixas seguem uma regra, você não decide: você calcula.**

```python
for valor in [850, 2400, 4200, 7300, 25000]:
    faixa = valor // 1000
    print(f"R$ {valor} está na faixa {faixa}")
```

```
R$ 850 está na faixa 0
R$ 2400 está na faixa 2
R$ 4200 está na faixa 4
R$ 7300 está na faixa 7
R$ 25000 está na faixa 25
```

`//` é novidade — cinco segundos: "divide e joga o resto fora".

**Resposta 2 — se as faixas são arbitrárias, você guarda a tabela e percorre.**

```python
limites = [1000, 3000, 5000, 10000]

for valor in [850, 2400, 4200, 7300, 25000]:
    faixa = "até 1000"
    for limite in limites:
        if valor >= limite:
            faixa = f"acima de {limite}"
    print(f"R$ {valor} → {faixa}")
```

```
R$ 850 → até 1000
R$ 2400 → acima de 1000
R$ 4200 → acima de 3000
R$ 7300 → acima de 5000
R$ 25000 → acima de 10000
```

**Agora a frase da aula. Diga devagar:**

> **"Para ter 200 faixas, a lista `limites` cresce. O código não muda uma linha."**

> "Isso é a diferença entre um programa que você **escreve** e um programa que você **alimenta**. Guardem essa ideia — ela volta em cinco minutos."

*(É a ponte direta para a pílula, e ecoa o "dado vale mais que código" da Aula 3.)*

---

### 11:46 — 11:58 | 💊 Pílula de IA: treinar é repetir (12 min)

**Etapa 1 — o problema (2 min)**

Tabela no slide: horas estudadas → nota.

| Horas | 1 | 2 | 3 | 4 | 5 | 6 |
|---|---|---|---|---|---|---|
| Nota | 2 | 4 | 5 | 7 | 8 | 10 |

> "Quero prever a nota de quem estudou três horas e meia. O palpite mais simples do mundo: `nota = peso × horas`. Falta **um número**: o peso."

**Etapa 2 — o erro é só uma conta (3 min)**

> "Se eu chutar um peso, consigo medir o quanto errei. Para cada aluno: diferença entre previsto e real, ao quadrado para o sinal não atrapalhar, e somo tudo. **Um `for` com acumulador** — o que vocês aprenderam hoje."

```
peso 0.0 → erro  258.00
peso 0.5 → erro  127.75
peso 1.0 → erro   43.00
peso 2.0 → erro   10.00
peso 3.0 → erro  159.00
```

> "Repare no formato: o erro cai, cai... e volta a subir. Existe um peso melhor que todos, entre 1 e 3. Mas **onde**?"

> "Chutando na mão dá — com **um** número. Um modelo de verdade tem milhões. Ninguém chuta milhões de números."

**Etapa 3 — o laço que aprende (4 min). É o coração da pílula.**

> "Em vez de chutar, ele começa em qualquer lugar, olha para que lado o erro diminui, anda **um passinho** para lá — e repete."

Rode a célula:

```
passo  0 | peso 0.306 | erro  172.88
passo 10 | peso 1.497 | erro    3.86
passo 20 | peso 1.657 | erro    0.81
passo 30 | peso 1.678 | erro    0.76
passo 40 | peso 1.681 | erro    0.76
passo 49 | peso 1.681 | erro    0.76
```

Rode o gráfico logo em seguida — a curva do erro despencando é a imagem que fica.

**Agora aponte para o código na tela, linha por linha:**

> "Olhem o que isso **é**. Um `for` com `range`. Um acumulador. E uma linha `peso = peso - alguma coisa` — que é `total = total + gasto` com o sinal trocado."

> "**Vocês aprenderam as três coisas hoje de manhã.** Erro 258 no começo, 0.76 no fim. E ninguém escreveu o 1.681."

**Etapa 4 — 💥 e se o passo for grande demais? (2 min)**

Troque `taxa = 0.001` por `taxa = 0.02` e rode:

```
passo 0 | peso         6.12 | erro            1793.63
passo 3 | peso       -79.99 | erro          606978.67
passo 7 | peso     -3965.49 | erro      1432202077.83
```

> "Explodiu. De 258 para mais de **um bilhão** em oito voltas. O passo foi tão grande que ele passou do ponto, caiu do outro lado mais longe ainda, e da próxima vez passou mais ainda."

> "Isso tem nome, e é o botão mais importante de quem treina modelo: **taxa de aprendizado**. Pequena demais, demora uma eternidade. Grande demais, nunca chega. Quando vocês lerem que 'o treino divergiu', é exatamente esta célula."

**Etapa 5 — a escala (1 min)**

> "Esse laço rodou 50 vezes ajustando **um** número. Treinar um modelo de linguagem é **o mesmo laço**, rodando bilhões de vezes, ajustando bilhões de números. A conta do 'para que lado empurrar' fica mais complicada — na faculdade chama **gradiente descendente** — mas a estrutura é essa aí, e vocês acabaram de escrever."

**Feche com as três frases:**

1. **Treinar não é mágica.** É um `for` que chuta, mede o erro e corrige.
2. **O modelo não sabe a resposta** — ele erra de propósito e ajusta. Milhões de vezes.
3. **Ninguém escreveu o 1.681.** Igual à semana passada: os dados escreveram.

> **Nota para você (Unipds):** se perguntarem "como ele sabe para que lado empurrar?", a resposta honesta na hora é *"tem uma conta que diz a inclinação do erro naquele ponto — a gente vê direito no Módulo 4"*. Depois da aula, escreva para si mesmo, sem jargão, por que a derivada aponta a direção da subida. Se travar, achou o seu estudo da semana.

---

### 11:58 — 12:00 | Tarefa e a nova dívida (2 min)

Mande escrita no grupo; aqui só anuncie.

1. **Tabuada com escolha:** pergunta o número e imprime a tabuada dele
2. **Jogo de adivinhação:** sorteia de 1 a 100, o jogador chuta, o programa diz "maior" ou "menor" e conta as tentativas *(`while` + `if` + acumulador — e é o Projeto 1 chegando)*
3. **Contador de spam:** pegue o `eh_spam` da Aula 3 e conte quantas mensagens de uma caixa de entrada ele marca
4. **Desafio:** some todos os pares de 1 a 100 **sem escrever nenhum número além do 1 e do 100** *(dica: `n % 2`)*

**A pergunta que fica em aberto — deixe no ar de propósito:**

> "Hoje vocês usaram lista a aula inteira e eu **nunca expliquei o que ela é**. E o `for` só sabe uma coisa: andar do começo ao fim, um por um. E se eu quiser pegar **só o terceiro** aluno? Acrescentar um no meio? Ordenar os salários? Descobrir o **maior** deles?"

*(A resposta é a Aula 5: listas e strings.)*

**Fechamento:**

> "Semana passada os programas de vocês começaram a decidir. Hoje eles começaram a repetir. Com essas duas coisas vocês já conseguem escrever quase qualquer programa pequeno que existe — o resto do curso é sobre o que você repete e sobre o que você decide. Até sábado."

---

## Se sobrar tempo

- **`for` dentro de `for`** — a tabuada inteira, de 1 a 10, em quatro linhas. Impressiona e prepara a Aula 5.
- **A extrapolação da pílula:** peça ao modelo a nota de quem estudou 7 horas. Ele responde **11.8** — nota acima de 10. "O modelo não sabe que nota máxima é 10. Ele só sabe a reta. Isso se chama extrapolar, e é uma das formas mais comuns de IA falar bobagem com confiança."
- Peça para a turma **inventar um laço infinito de propósito** e parar. Vira brincadeira e tira o medo.

## Se faltar tempo

Corte nesta ordem: (1) a Parte A da mão na massa às 11h38, (2) o `break`, (3) a Resposta 1 (`//`) da Parte B — a Resposta 2 sozinha já paga a dívida, (4) a Etapa 4 da pílula.

**Nunca corte:** a abertura com os 200 `elif`, o **acumulador** e o bug do `total = 0` dentro do laço, o **loop infinito com o botão de parar**, a frase *"a lista cresce, o código não"*, e as Etapas 3 e 5 da pílula.

> Se estiver claramente atrasado às 11h40, pule direto da abertura da Parte B para a Resposta 2 e ganhe os 4 minutos ali — a pílula é o que a turma veio buscar.

---

## Depois da aula — 15 minutos

- [ ] Mandar no grupo: link do notebook e a tarefa escrita
- [ ] Anotar em `duvidas-da-turma.md` o que te pegou
- [ ] Escrever para si mesmo, sem jargão: *por que a derivada aponta a direção em que o erro sobe?* e *o que decide uma boa taxa de aprendizado?*
- [ ] As duas perguntas que ficaram da Aula 3 continuam abertas: *por que "naive" no Naive Bayes?* e *o que aquele 61% de confiança significa?* — se ainda não escreveu, esta semana é a semana
- [ ] Conferir quem entregou tarefa. Quarta semana: quem não entregou nada até agora precisa de uma mensagem direta, não de mais um aviso no grupo
- [ ] **Dívida pendente do curso:** o `abc` resolvido com `try/except` está prometido para a **Aula 9**. Continua na fila.
