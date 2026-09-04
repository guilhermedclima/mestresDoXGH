# Aula 9 — Roteiro minuto a minuto

**Sábado, 10/10/2026 — 10h00 às 12h00**
**Tema:** Erros e depuração — o programa que não morre
**Ambiente:** VS Code na máquina do aluno *(Colab para quem ainda não instalou)*
**Turma:** até 6 alunos

---

## Objetivo da aula

Ao final, cada aluno deve:

1. **Ler um traceback de baixo para cima** e saber o que é o quê
2. Reconhecer os erros mais comuns pelo nome e saber o que cada um quer dizer
3. Escrever `try` / `except` tratando **o erro específico** que espera
4. Saber por que `except:` pelado é gambiarra cara
5. Usar `raise` para dar uma mensagem de erro que ajuda
6. Ter visto que **modelo de IA não levanta exceção** — e construído um que sabe dizer "não sei"

**A dívida que abre a aula:** a mais antiga do curso — *"e se a pessoa digitar `abc`?"*, da **Aula 2**.

| Gancho | De onde veio | Onde é pago |
|---|---|---|
| O `abc` | **Aula 2**, meio-resolvido na Aula 3 | 10h35 — completo, com `try/except` |
| "O programa morre no primeiro imprevisto" | Fim da Aula 8 | A aula inteira |
| O `agenda.json` que não existe | Desafio da Aula 8 | 11h20 — a resposta certa |

> **A frase que amarra a aula:**
> **trate o erro que você espera, pelo nome. O que você não esperava deve continuar estourando.**

---

## Antes de começar

- [ ] Notebook testado e link curto no quadro (`28-aula-09-notebook.ipynb`)
- [ ] Slides abertos (`27-aula-09-slides.html`)
- [ ] **Conferir quem terminou a instalação.** Hoje é a primeira aula pensada para o VS Code — quem
      não instalou segue no Colab sem problema, mas você precisa saber quem é
- [ ] Ter à mão a linha do tempo da dívida do `abc` — ela abre a aula e é o melhor momento do dia
- [ ] Separar um erro **real e feio** que aconteceu com você na semana, para mostrar no começo

---

## Roteiro

### 10:00 — 10:12 | A dívida mais antiga do curso (12 min)

**Recolha a tarefa (4 min).** Pergunte quem conseguiu o `agenda.json` que funciona na primeira
execução. Quase certamente alguém usou `os.path.exists()` — **elogie**, porque funciona, e diga
que hoje sai o jeito que o Python prefere.

**Agora a abertura, e ela é especial (8 min).** Projete a linha do tempo:

| Quando | O que aconteceu |
|---|---|
| **Aula 2** | Alguém perguntou: *"e se a pessoa digitar `abc`?"* |
| **Aula 3** | Resolvi **meio**, com `.isdigit()`, e avisei que faltava |
| **Aula 8** | O `FileNotFoundError` mostrou que o problema é maior que o `abc` |
| **Aula 9** | Hoje |

> "Sete semanas. Essa é a promessa mais velha deste curso, e hoje ela é paga."

Se você lembrar **quem** perguntou, diga o nome. Vale muito.

Mostre o limite do `.isdigit()`:

```python
'42'     .isdigit() -> True
'abc'    .isdigit() -> False
'-5'     .isdigit() -> False     ← mas -5 É um número
'3.14'   .isdigit() -> False     ← e 3.14 também
```

> "Ele acerta o `abc` e erra o resto. Para funcionar de verdade, teria que saber sobre sinal
> negativo, ponto decimal, espaço nas pontas, notação científica... **A ferramenta certa é outra.**"

---

### 10:12 — 10:32 | Ler um erro (20 min)

**Anuncie a virada de postura (2 min):**

> "Em oito aulas vocês viram muita tela vermelha, e a reação de todo mundo é a mesma: fechar os
> olhos e começar a mexer no código. Hoje a tela vermelha deixa de ser tela vermelha e vira
> **informação**."

Rode o exemplo de três níveis:

```
Traceback (most recent call last):
  File "...", line 10, in <module>
    main()
  File "...", line 8, in main
    return processar("Ana oito")
  File "...", line 6, in processar
    return nome, ler_nota(nota)
  File "...", line 3, in ler_nota
    return float(texto)
ValueError: could not convert string to float: 'oito'
```

**Aponte a última linha primeiro. Literalmente com o dedo.**

> "**Leia de baixo para cima. Sempre.**"
>
> 1. A **última linha é o quê**: tentou virar número um texto que não é número, e o texto era `'oito'`.
> 2. A **linha acima é o onde**: `in ler_nota`, no `float(texto)`.
> 3. O resto é **o caminho até lá**: `main` chamou `processar`, que chamou `ler_nota`.

> "*Traceback* quer dizer isso: **o rastro de volta**. Iniciante olha e vê um monte de linha. Quem
> já sofreu olha a última linha primeiro, e só sobe se precisar."

**O catálogo (10 min).** Rode a célula que dispara os oito erros e leia a tabela com a turma —
apontando **em que aula** cada um apareceu:

| Erro | Quer dizer | Onde viram |
|---|---|---|
| `SyntaxError` | escrevi errado, nem rodou | Aula 3, o `if idade = 18` |
| `NameError` | esse nome não existe | Aula 6, escopo |
| `TypeError` | tipo errado para a operação | Aula 2, o `"2" + 2` |
| `ValueError` | tipo certo, valor impossível | hoje |
| `IndexError` | posição que não existe | Aula 5 |
| `KeyError` | chave que não existe | Aula 7 |
| `AttributeError` | esse objeto não tem esse método | erro de digitação, quase sempre |
| `FileNotFoundError` | o arquivo não está lá | Aula 8 |

> "Vocês colecionaram esses erros durante oito aulas sem perceber que estavam colecionando."

E o par que mais confunde, dê ênfase:

> "**`TypeError` é tipo errado.** `\"2\" + 2` — somar texto com número não existe.
> **`ValueError` é tipo certo, valor impossível.** `int(\"abc\")` — é texto, como deveria ser, mas
> esse texto não vira número."

---

### 10:32 — 10:50 | `try` / `except`, e o `abc` (18 min)

```python
try:
    numero = int("abc")
    print("consegui:", numero)
except ValueError:
    print("isso não é um número")

print("e o programa continua vivo")
```

> "Leia em português: **tente isto; se der aquele erro, faça aquilo em vez de morrer.**"

Aponte a última linha:

> "Ela rodou. **O programa não morreu.** É a aula inteira nessa frase."

**E agora o momento (8 min).** Anuncie antes de rodar:

> "Sete semanas depois."

```python
def pedir_numero(texto):
    try:
        return float(texto)
    except ValueError:
        return None
```

```
'42'     -> 42.0
'abc'    -> None
'-5'     -> -5.0      ← o .isdigit() errava este
'3.14'   -> 3.14      ← e este
''       -> None
'  7  '  -> 7.0       ← até o espaço nas pontas
```

Deixe pousar. Depois explique **por que** funciona melhor — isso é o conteúdo, não o resultado:

> "O `.isdigit()` tentava **prever** todas as formas de um texto ser número. O `try/except` faz o
> contrário: **deixa o Python tentar** e só reage se falhar."

> "Isso tem nome em programação: *é mais fácil pedir perdão do que permissão*. Em vez de checar
> mil condições antes, você tenta e trata a falha. Menos código, menos caso esquecido."

---

### 10:50 — 11:00 | 💥 O `except` pelado (10 min)

**Este é o bloco que separa quem programa de quem gambiarra.**

```python
def dividir(a, b):
    try:
        return a / b
    except:                    # sem dizer QUAL erro
        return "deu algum problema"
```

```
5.0
deu algum problema
deu algum problema
deu algum problema
```

> "Parece que funcionou: nada quebrou. **E é exatamente esse o problema.**"

> "O `except:` pelado engole **qualquer** erro — inclusive os que são bug seu. O
> `dividir(10, [1,2])` não é problema do usuário: é um erro de programação que você acabou de
> esconder **de si mesmo**."

Agora o certo:

```python
except ZeroDivisionError:
    return "não dá para dividir por zero"
```

E rode `dividir_certo(10, "dois")`:

```
TypeError: unsupported operand type(s) for /: 'int' and 'str'
```

> "**Agora o bug aparece.** E aparecer é bom: é assim que se conserta."

Escreva no quadro e deixe:

> **Trate o erro que você espera, pelo nome. Erro que você não esperava deve continuar estourando
> — ele está te avisando de algo.**

E amarre com o nome da turma:

> "`except:` pelado é a gambiarra mais cara do Python. Muito XGH — e você paga depois."

---

### 11:00 — 11:10 | Pausa (10 min)

---

### 11:10 — 11:30 | O resto da caixa de ferramentas (20 min)

**💥 O `try` grande demais (5 min):**

```python
try:
    idade = int("30")
    dobro = idade * 2
    nome = "Ana"
    print(nomee)          # erro de digitação escondido aqui dentro
except Exception as e:
    print("algo deu errado:", e)
```

> "Deu `name 'nomee' is not defined` — mas você teve que **ler a mensagem** para descobrir. Se o
> bloco tivesse trinta linhas, boa sorte."

> "**Envolva no `try` só a linha que pode falhar por um motivo que você já previu.** `try` grande
> é `except` pelado disfarçado."

**`else` e `finally` (4 min).** Rápido, sem aprofundar:

- `else` → roda **só se o `try` deu certo**
- `finally` → roda **sempre**

> "O `finally` é para limpeza: fechar conexão, apagar arquivo temporário. E foi exatamente isso
> que o `with` da Aula 8 fez por vocês nos bastidores."

**`raise` (6 min).** Este merece o tempo:

```python
def calcular_media(notas):
    if len(notas) == 0:
        raise ValueError("a lista de notas está vazia")
    return sum(notas) / len(notas)
```

> "Sem o `raise`, isso daria `ZeroDivisionError: division by zero`. Tecnicamente correto — e
> inútil para quem chamou a função."

> "**Um erro bom diz o que fazer.** `'a lista de notas está vazia'` é infinitamente melhor que
> `division by zero`, mesmo sendo o mesmo problema. Isso é o que separa biblioteca boa de
> biblioteca ruim."

**Depurar (5 min).** Três técnicas, em ordem:

1. **O `print` bem colocado.** Não é vergonha — é a ferramenta mais usada do mundo, por todo mundo.
   Marque com `[debug]` para achar e apagar depois.
2. **Bisseção.** Programa de 100 linhas com resultado errado? Não leia as 100: ponha um `print` na
   50. Certo? o problema está depois. Errado? está antes. Repete.
   > "Sete cortes acham a agulha em 100 linhas. **É a busca binária do robô da Aula 6.**"
3. **Ler o erro antes de mexer.** Em metade das vezes a última linha já resolve.

---

### 11:30 — 11:42 | Mão na massa: a agenda que não morre (12 min)

Volte ao desafio da semana passada e mostre a resposta certa:

```python
def carregar_agenda(arquivo="agenda.json"):
    try:
        with open(arquivo, encoding="utf-8") as f:
            return json.load(f)
    except FileNotFoundError:
        print(f"(primeira execução — criando {arquivo})")
        return []
    except json.JSONDecodeError:
        print(f"(o {arquivo} está corrompido — começando do zero)")
        return []
```

> "**Dois `except` diferentes** no mesmo `try`, cada um com sua mensagem. São dois problemas
> diferentes: um é primeira execução, o outro é arquivo estragado. Um `except:` pelado trataria
> os dois como 'sei lá, deu ruim'."

Deixe a turma rodar duas vezes: a primeira cria o arquivo, a segunda carrega. E peça para alguém
**estragar o JSON de propósito** num editor e rodar de novo — o segundo `except` acende.

---

### 11:42 — 11:58 | 💊 Pílula de IA: quando a IA erra, quem depura é você (16 min)

**Etapa 1 — o modelo nunca levanta exceção (4 min)**

Treine o detector de spam da Aula 3 de novo e jogue absurdo nele:

```
spam   ( 99.6%)  <- 'promocao gratis clique aqui'
normal ( 88.1%)  <- 'vamos almocar amanha'
spam   ( 61.2%)  <- 'o curso e gratis para os alunos'
normal ( 50.0%)  <- 'asdfgh qwerty zxcvbn'
normal ( 50.0%)  <- ''
normal ( 50.0%)  <- 'the quick brown fox jumps over the lazy dog'
normal ( 50.0%)  <- '1234567890'
```

**Pare na quinta linha e deixe a sala perceber.**

> "Eu mandei uma **mensagem vazia** e ele classificou. Mandei letras aleatórias: classificou.
> Mandei inglês, que ele nunca viu: classificou. Números: classificou."

> "**Nenhum `ValueError`. Nenhum traceback. Nenhuma tela vermelha.** Ele responde sempre."

**Etapa 2 — a diferença que muda o trabalho de vocês (3 min)**

| | Código | Modelo |
|---|---|---|
| Quando dá errado | **grita** — traceback, para tudo | **sussurra** — responde errado, educado |
| Como você descobre | na hora | só se for olhar |
| Onde está o defeito | numa linha | espalhado nos dados e nos números |
| Como conserta | corrige a linha | mais exemplo (Aula 3), melhor dado (Aula 8) |

> "Um programa quebrado **te avisa**. Um modelo quebrado **te deixa confiante**. É por isso que
> testar IA dá mais trabalho que testar código: não existe traceback. Você tem que ir procurar."

**Etapa 3 — mas olhem aquele 50% (3 min)**

> "Todas as absurdidades saíram com exatamente **50%**. Não é coincidência: num problema de duas
> classes, 50% é **cara ou coroa**. Ele está dizendo, com o único vocabulário que tem: *não faço a
> menor ideia*."

> "Ele **tem** a informação. Só ninguém estava olhando."

E puxe a memória da Aula 3:

> "Lembram do **61%** naquela frase 'o curso e gratis para os alunos'? Mesmo número, mesmo recado:
> em cima do muro."

**Etapa 4 — juntando tudo (5 min). É o fecho do curso inteiro até aqui.**

```python
def classificar_com_duvida(texto, limiar=80):
    if not texto.strip():
        raise ValueError("mensagem vazia")
    resultado, conf = classificar(texto)
    if conf < limiar:
        return "nao sei", conf
    return resultado, conf
```

Aponte as peças:

> "`if` da Aula 3. Função da Aula 6. `raise` de hoje. Três aulas numa função de seis linhas."

```
spam     ( 99.6%)            <- 'promocao gratis clique aqui'
normal   ( 88.1%)            <- 'vamos almocar amanha'
nao sei  ( 61.2%) -> HUMANO  <- 'o curso e gratis para os alunos'
nao sei  ( 50.0%) -> HUMANO  <- 'asdfgh qwerty zxcvbn'
ERRO                         <- ''  (mensagem vazia)
nao sei  ( 50.0%) -> HUMANO  <- 'the quick brown fox jumps over the lazy dog'
nao sei  ( 50.0%) -> HUMANO  <- '1234567890'
```

> "**Todos os casos absurdos foram para revisão humana.** E o erro que ele cometia na Aula 3 —
> marcar mensagem honesta como spam — virou um 'não sei' em vez de uma resposta errada com cara de
> certeza."

> "Isso tem nome na indústria: **human in the loop**. O modelo resolve o fácil; o que ele não sabe
> vai para uma pessoa."

E o limiar como decisão de negócio:

```
limiar 60% -> 3 de 6 casos para revisão humana
limiar 70% -> 4 de 6
limiar 80% -> 4 de 6
limiar 90% -> 5 de 6
limiar 95% -> 5 de 6
```

> "Limiar alto: mais segurança, mais trabalho humano, mais custo. Limiar baixo: mais automação,
> mais erro passando."

> "**Não existe resposta certa. Existe a certa para aquele negócio.** Um filtro de spam de e-mail
> pessoal e uma triagem de exame médico não têm o mesmo limiar — e essa conversa é com quem entende
> do problema, não com quem escreveu o código."

**Feche com as três frases:**

1. **Modelo não levanta exceção.** Ele responde sempre, inclusive sobre o que não conhece.
2. **Código grita, modelo sussurra.**
3. **A confiança é a única tela vermelha que o modelo tem** — e virar "não sei" é uma decisão de
   engenharia que vocês escrevem com um `if`.

> **Nota para você (Unipds):** se perguntarem "então esse 50% é confiável?", a resposta honesta na
> hora é *"a probabilidade que o modelo devolve nem sempre reflete a chance real de estar certo —
> tem modelo que é confiante demais"*. Depois da aula, escreva para si mesmo o que é **calibração**
> de um modelo, e por que uma rede grande costuma ser mal calibrada. É um dos assuntos mais úteis e
> menos falados de IA aplicada.

---

### 11:58 — 12:00 | Tarefa e a nova dívida (2 min)

1. **`pedir_numero()` de verdade:** insiste até receber número válido. **Use no Projeto 1** — o jogo quebra hoje se você digitar letra
2. **Agenda à prova de bala:** arquivo inexistente, corrompido, e menu com entrada errada — cada caso com seu `except`
3. **Leitor de CSV tolerante:** pula linha quebrada, avisa qual foi, e continua. Estrague o arquivo você mesmo
4. **Desafio:** quando o modelo diz "não sei", perguntar a resposta certa **para você** e acrescentar o exemplo no CSV. Retreinar e ver melhorar

Sobre o desafio, dê o nome:

> "Isso se chama **active learning**, e vocês vão ter construído um."

**A pergunta que fica:**

> "Na tarefa 3 vocês vão ler CSV, converter coluna na mão, tratar linha quebrada, contar, somar,
> dividir. Agora imaginem que o arquivo tem **5.000 linhas e 12 colunas**. E que precisam da média
> por cidade. E do total por mês. E dos dez maiores."

> "Dá para fazer com `for`, `if` e dicionário — vocês já sabem tudo que é preciso. **Quantas linhas
> de código isso vai ter?**"

*(A resposta é a Aula 10: pandas.)*

**Fechamento:**

> "Hoje os programas de vocês pararam de morrer. Semana que vem vocês param de escrever cem linhas
> para fazer o que cabe em três — e é a última aula antes de o curso virar IA de ponta a ponta.
> Até sábado."

---

## Se sobrar tempo

- **`logging` no lugar do `print`** — mostre `logging.info` e `logging.error` em cinco minutos. Prepara a Aula 13
- Peça para a turma **quebrar o programa do vizinho**: cada um tenta achar uma entrada que faz o código do outro estourar. Vira competição e ensina teste de borda
- **Erro personalizado** com `class MinhaExcecao(Exception)` — só como aperitivo da Aula 23

## Se faltar tempo

Corte nesta ordem: (1) `else` e `finally`, (2) as três técnicas de depuração (mande por escrito no
grupo), (3) a tabela de limiar no fim da pílula, (4) o `try` grande demais.

**Nunca corte:** a linha do tempo da dívida do `abc` na abertura, a leitura do traceback de baixo
para cima, o `abc` resolvido com `try/except`, o **`except` pelado** com a regra escrita no quadro,
e as Etapas 1, 3 e 4 da pílula.

---

## Depois da aula — 15 minutos

- [ ] Mandar no grupo: link do notebook e a tarefa escrita
- [ ] Anotar em `duvidas-da-turma.md` o que te pegou
- [ ] Escrever para si mesmo, sem jargão: *o que é calibração de um modelo, e por que rede grande costuma ser confiante demais?*
- [ ] **Marco do curso:** com a Aula 9 fecha o núcleo de "programar de verdade". Vale mandar no grupo uma retrospectiva curta do que eles já conseguem fazer sozinhos — a lista é maior do que eles imaginam, e a Aula 10 é a ponte para o Módulo 3
- [ ] **Nenhuma dívida antiga em aberto pela primeira vez no curso.** A da Aula 2 foi paga hoje. A partir daqui as dívidas são todas de uma aula para a seguinte
