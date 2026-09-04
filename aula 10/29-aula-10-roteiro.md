# Aula 10 — Roteiro minuto a minuto

**Sábado, 17/10/2026 — 10h00 às 12h00**
**Tema:** pandas — parar de escrever cem linhas · **fecha o Módulo 2**
**Ambiente:** VS Code na máquina do aluno *(Colab para quem preferir)*
**Turma:** até 6 alunos

---

## Objetivo da aula

Ao final, cada aluno deve:

1. Ler um CSV com `read_csv` e inspecionar com `.head()`, `.shape` e `.describe()`
2. Filtrar linhas por condição, com `&` e parênteses
3. Usar `groupby` para responder pergunta de negócio
4. **Rodar `.isnull().sum()` em toda tabela nova** — e saber por quê
5. Ter visto, medido, que dado sujo derruba um modelo de 100% para 25% **sem dar nenhum erro**

**A dívida que abre a aula:** *"e se o CSV tivesse 5.000 linhas? Quantas linhas de código isso vai ter?"*

| Gancho | De onde veio | Onde é pago |
|---|---|---|
| O CSV de 5.000 linhas | Fim da Aula 9 | Abertura, 10h00 — os dois códigos lado a lado |
| O CSV tolerante da tarefa | Tarefa da Aula 9 | Abertura — recolha antes |

> **A frase que amarra a aula:**
> **o pandas quase nunca dá erro — ele dá um número errado com cara de certo.**

---

## ⚠️ Esta aula fecha um ciclo

É a última **pílula de IA como bloco separado**. Da Aula 11 em diante o curso inteiro é sobre isso,
e o formato muda. Vale dizer isso à turma no fim — dá sensação de progresso, que é o que segura
gente na metade de um curso longo.

---

## Antes de começar

- [ ] Notebook testado e link curto no quadro (`31-aula-10-notebook.ipynb`)
- [ ] Slides abertos (`30-aula-10-slides.html`)
- [ ] **Rodar a célula 0 antes da aula** — é ela que gera os CSVs
- [ ] Quem está no VS Code precisa de `pip install pandas scikit-learn`. **Avise na sexta.** No Colab já vem
- [ ] Ter uma planilha **sua de verdade** à mão (extrato, controle de gastos) para mostrar no fim — funciona muito melhor que dado inventado
- [ ] Preparar duas frases sobre o que a turma consegue fazer hoje e não conseguia em agosto

---

## Roteiro

### 10:00 — 10:15 | A dívida: os dois códigos lado a lado (15 min)

**Recolha a tarefa (5 min).** Pergunte quem fez o **CSV tolerante** e, principalmente, **quem
inventou uma sujeira criativa**. Essa parte rende — pedir para alguém descrever como estragou o
arquivo é ótima entrada para a aula de hoje.

**Agora a dívida (10 min).** Projete o código da Aula 9, o que eles já sabem escrever:

```python
total_por_cidade = {}

with open("vendas.csv", encoding="utf-8") as f:
    for linha in csv.DictReader(f):
        cidade = linha["cidade"]
        total_por_cidade[cidade] = total_por_cidade.get(cidade, 0) + float(linha["valor"])

for cidade in sorted(total_por_cidade):
    print(f"{cidade:14s} {total_por_cidade[cidade]:10.2f}")
```

**Elogie antes de substituir** — isso importa:

> "Isso está **certo**. Usa `with`, `DictReader`, `for`, `.get()` com padrão, conversão de tipo,
> f-string. **Seis coisas que vocês aprenderam em nove aulas**, todas no lugar."

Rode. Depois:

```python
df = pd.read_csv("vendas.csv")
print(df.groupby("cidade")["valor"].sum().round(2))
```

```
cidade
Guaruja         52223.12
Praia Grande    56859.49
Santos          44765.02
Sao Vicente     50353.80
```

> "**Mesmos números. Duas linhas.**"

Deixe a reação acontecer. E então dê o aviso mais importante da aula:

> "**Não joguem fora as nove aulas anteriores.** O pandas é rápido de escrever porque alguém
> escreveu aquele `for` por vocês. Quem não sabe o que acontece por dentro trava no primeiro caso
> em que a biblioteca não faz exatamente o que precisa — e isso acontece toda semana."

---

### 10:15 — 10:35 | DataFrame e `.describe()` (20 min)

```python
df.head()
df.shape          # (200, 5)
list(df.columns)
```

Amarre com o que eles já sabem:

> "O DataFrame junta duas coisas que vocês conhecem: cada **coluna tem nome**, como um dicionário,
> e cada **linha tem posição**, como uma lista. É a lista de dicionários da Aula 7 com músculo."

**`.describe()` merece o tempo (8 min).** É o primeiro comando de qualquer tabela nova:

```
       quantidade    valor
count      200.00   200.00
mean         2.95  1021.01
std          1.44  1183.20
min          1.00    72.53
25%          2.00   292.07
50%          3.00   592.50
75%          4.00   962.01
max          5.00  4684.02
```

Pergunte antes de explicar: **"o que essa tabela está dizendo sobre a coluna `valor`?"**

Se ninguém vir, aponte:

> "A **média é 1021**. A **mediana, que é o 50%, é 592**. A média está quase o dobro da mediana."

> "Isso é sinal clássico de que tem venda muito grande puxando o número para cima. **Média sozinha
> mente.** O `.describe()` te avisa — e é por isso que ele vem antes de qualquer conta."

---

### 10:35 — 10:50 | Coluna e filtro (15 min)

```python
df["valor"].sum()     # 204201.43
df["valor"].mean()    # 1021.01
df["valor"].max()     # 4684.02
```

> "`df['valor']` pega a coluna inteira, e aí `.sum()`, `.mean()`, `.max()` funcionam direto. **É o
> acumulador da Aula 4, já pronto.**"

**O filtro (8 min).** Escreva a leitura no quadro, porque a sintaxe assusta:

```
df[ df["valor"] > 2000 ]
 │        └── isso vira uma coluna de True/False, um por linha
 └── e o df fica só com as linhas onde deu True
```

> "É o `if` da Aula 3 aplicado a duzentas linhas de uma vez. O `bool` continua sendo a peça
> central — sete aulas depois."

Duas condições, e avise dos dois tropeços:

```python
df[(df["cidade"] == "Santos") & (df["produto"] == "monitor")]     # 11 linhas
```

| Python puro | pandas |
|---|---|
| `and` | `&` |
| `or` | `\|` |
| `not` | `~` |

> "E **cada condição precisa de parênteses**. Sem eles o Python resolve na ordem errada e o erro é
> difícil de entender. É chato, é assim, e todo mundo esquece pelo menos uma vez."

---

### 10:50 — 11:00 | `groupby` (10 min)

```python
df.groupby("produto")["valor"].agg(["count", "sum", "mean"]).round(2)
```

```
         count        sum     mean
produto
headset     43   31515.74   732.92
monitor     47  128395.22  2731.81
mouse       34    7687.41   226.10
teclado     47   20629.70   438.93
webcam      29   15973.36   550.81
```

Dê a leitura em português:

> "`groupby('produto')` é: **separa em montinhos por produto, e faz a conta em cada montinho**."

E o insight de negócio, que é o que vende a ferramenta:

> "Essa tabela responde três perguntas de uma vez. Olhem monitor e teclado: **as mesmas 47
> vendas** — e o monitor fatura **seis vezes mais**. Na mão, isso seria um dicionário de
> dicionários e uns quinze `for`."

Se sobrar fôlego, mostre o agrupamento por mês, que reusa a Aula 5:

```python
df["mes"] = df["data"].str[5:7]        # fatiamento da Aula 5, numa coluna inteira
df.groupby("mes")["valor"].sum()
```

---

### 11:00 — 11:10 | Pausa (10 min)

---

### 11:10 — 11:35 | Os três erros que o pandas não avisa (25 min)

**Abra dizendo o porquê do bloco:**

> "Aqui está o motivo real desta aula existir. **O pandas quase nunca dá erro. Ele te dá um número
> errado com cara de certo** — e isso é muito pior."

**💥 nº 1 — o `NaN` que ninguém vê (10 min). É o mais importante.**

```
produto  vendas  nota
teclado   150.0   4.5
  mouse    80.0   NaN
monitor   900.0   4.8
headset   250.0   3.9
 webcam     NaN   4.1
```

```python
sujo["nota"].mean()     # 4.325
sujo["nota"].count()    # 4      ← conta 4
len(sujo)               # 5      ← mas são 5 linhas
```

> "**A média é de quatro notas, não de cinco.** O `.mean()` pulou o `NaN` e não avisou ninguém."

E o ponto que torna isso uma decisão, não um detalhe:

```python
sujo["nota"].mean()                # 4.325  — ignorando o vazio
sujo["nota"].fillna(0).mean()      # 3.46   — tratando como zero
```

> "Qual está certo? **Depende do que aquele vazio significa** — a nota não existe, ou a nota é
> zero? **Você tem que decidir, e o pandas não vai perguntar.**"

Deixe o comando no quadro:

```python
df.isnull().sum()
```

> "Rodem isso em **toda tabela nova**, antes de qualquer conta. É o `.describe()` do dado faltando."

**💥 nº 2 — a coluna que parece número (8 min):**

```python
d2["valor"].sum()
```

```
15080R$ 900
```

> "**Ele concatenou.** Um `R$` no meio de trezentas linhas fez a coluna inteira virar texto. E a
> soma virou aquilo que vocês viram na Aula 2 com `\"2\" + \"2\"` — nove aulas depois, o mesmo bug."

E o filtro, que pelo menos grita:

```
TypeError: '>' not supported between instances of 'str' and 'int'
```

> "Este **grita**. O `.sum()` não gritou. É aí que mora o perigo: o erro que aparece é o barato."

**💥 nº 3 — o filtro que devolve zero (5 min):**

```python
len(d3[d3["cidade"] == "santos"])    # 0
len(d3[d3["cidade"] == "Santos"])    # 2
```

> "Zero linhas. **Nenhum erro.** É o `\"Banana\" == \"banana\"` da Aula 3, agora afetando um
> relatório inteiro."

> "Quando um filtro voltar vazio, a primeira suspeita é sempre a mesma: caixa alta, espaço nas
> pontas, ou acento. `df['cidade'].unique()` mostra o que existe de verdade na coluna."

---

### 11:35 — 11:53 | 💊 Pílula de IA: 80% do trabalho é limpar tabela (18 min)

**Etapa 1 — um arquivo como ele chega de verdade (4 min)**

> "Este CSV são avaliações de clientes com o sentimento marcado. Montei imitando arquivo real:
> gente diferente digitando, exportação de sistema, planilha passando de mão em mão."

```
linhas: 180
rótulos faltando: 22
linhas duplicadas: 52
grafias distintas do rótulo: 8

NEGATIVO      29
Negativo      28
POSITIVO      23
NaN           22
 negativo     20
negativo      17
 positivo     14
Positivo      14
positivo      13
```

Pare na contagem:

> "**Oito grafias para duas coisas.** `NEGATIVO`, `Negativo`, `negativo` e `' negativo '` são a
> mesma palavra para vocês — e quatro rótulos completamente diferentes para o computador."

> "É o `\"Banana\" == \"banana\"` de novo. Terceira vez hoje."

**Etapa 2 — treinando no dado sujo (5 min)**

> "Vou só tirar as linhas sem rótulo, porque essas não dá para usar mesmo. O resto vai como está."

```
linhas de treino: 158
classes que o modelo enxerga: 8
acerto no teste: 25.0%
```

> "**Vinte e cinco por cento.** Pior que chutar."

E a explicação está na linha do meio — aponte:

> "O modelo achou que existiam **oito sentimentos**. Ele não estava aprendendo 'positivo vs
> negativo': estava tentando adivinhar entre oito rótulos, quatro dos quais significam a mesma
> coisa."

> "E reparem: **nenhum erro. Nenhum aviso.** O treino rodou lindamente."

**Etapa 3 — quatro linhas de limpeza (6 min)**

```python
limpo = av.dropna(subset=["sentimento"]).copy()
limpo["texto"] = limpo["texto"].str.strip().str.lower()
limpo["sentimento"] = limpo["sentimento"].str.strip().str.lower()
limpo = limpo.drop_duplicates()
```

```
linhas de treino: 46
classes: ['negativo', 'positivo']
acerto no teste: 100.0%
```

> "De **25% para 100%**. Com quatro linhas."

**E agora o detalhe mais contraintuitivo do curso inteiro** — deixe eles verem sozinhos:

> "Olhem quantas linhas cada modelo usou. O ruim treinou com **158**. O bom, com **46**."

> "**Menos dado, resultado melhor.** Porque as outras 112 linhas eram duplicata ou ruído de
> rótulo. Não eram informação — eram barulho."

Amarre com a Aula 8:

> "Semana retrasada a curva mostrou que **mais dado ajuda**. Hoje fica a outra metade da frase:
> **mais dado ajuda; mais barulho atrapalha.** E os dois chegam no mesmo arquivo."

**Etapa 4 — conte as linhas (3 min)**

| O que | Linhas |
|---|---|
| **Limpar** | 4 |
| **Treinar** | 3 |

> "Neste exemplo minúsculo quase empata. Num projeto de verdade, limpar é carregar de vários
> lugares, juntar tabelas, resolver data em três formatos, telefone com e sem DDD, cidade escrita
> de seis jeitos, valor com vírgula e com ponto, linha duplicada porque o sistema exportou duas
> vezes."

> "**E o treino continua sendo três linhas.**"

> "É daí que vem a frase que vocês vão ouvir muito: *80% do trabalho de ciência de dados é
> limpeza*. Não é reclamação. É a descrição do trabalho."

**Feche com as três frases:**

1. **O modelo não sabe que `NEGATIVO` e `negativo` são a mesma coisa.** Você sabe. Por isso a limpeza é sua.
2. **Dado sujo não dá erro** — dá resultado ruim com cara de bom.
3. **Menos dado limpo bate mais dado sujo.** Sempre.

> **Nota para você (Unipds):** se perguntarem "e quando o dado faltando não é aleatório?", a
> resposta honesta na hora é *"aí jogar fora a linha enviesa o resultado — e isso é um problema
> sério"*. Depois da aula, escreva para si mesmo a diferença entre dado faltando **por acaso** e
> faltando **por um motivo** (quem não respondeu a pesquisa salarial provavelmente ganha menos).
> É uma das armadilhas mais caras de análise de dados.

---

### 11:53 — 12:00 | Fechamento do Módulo 2 (7 min)

**Este fechamento vale os sete minutos.** É metade do curso.

**A tarefa (2 min):**

1. **Relatório de vendas:** cidade que mais fatura, cidade que mais vende peças, produto com maior ticket, melhor mês, as 10 maiores vendas
2. **`diagnosticar(df)`:** linhas, colunas com faltante, duplicatas, tipo de cada coluna — a função que vocês vão rodar em toda tabela nova pelo resto da vida
3. **`limpar(df)`** como função, com comentário em cada passo. Depois estraguem o arquivo de outro jeito e vejam se aguenta
4. **Desafio:** rodar o `diagnosticar()` numa planilha **de verdade de vocês** — extrato, controle de gastos, planilha do trabalho. *"Quase sempre tem surpresa. Tragam a surpresa."*

**A retrospectiva (3 min).** Projete e leia devagar:

> "Dez aulas. Olhem o que vocês fazem hoje e não faziam em agosto:
> - fazer o programa **decidir** e **repetir**
> - guardar dado em lista, dicionário e **arquivo**
> - organizar em **funções** e entregar um **projeto que funciona**
> - **não morrer** no primeiro imprevisto
> - e analisar uma tabela de verdade em três linhas"

> "Isso não é 'saber um pouco de Python'. Isso é **programar**."

**A virada (2 min):**

> "A partir da semana que vem o curso muda de marcha. Acabaram as pílulas de IA como bloco
> separado — porque da Aula 11 em diante **a aula inteira é isso**: bibliotecas dos outros, APIs,
> automação de verdade, e depois IA de ponta a ponta."

**A pergunta que fica:**

> "O `pandas` e o `sklearn` não vieram com o Python. Alguém escreveu, e alguém colocou num lugar
> de onde a máquina de vocês baixou."

> "Quem? Como? E se eu quiser uma dessas para **outra coisa** — mandar e-mail, mexer em PDF, ler
> planilha do Excel?"

*(Aula 11: bibliotecas e `pip`.)*

---

## Se sobrar tempo

- **Abra uma planilha sua de verdade** e rode `.describe()` e `.isnull().sum()` ao vivo. É o momento mais convincente que existe — dado real sempre tem surpresa
- **`merge`** de duas tabelas, só para mostrar que existe e que é o `JOIN` de banco de dados
- **`value_counts()`** — o `contar_palavras` da tarefa da Aula 7, pronto, numa linha

## Se faltar tempo

Corte nesta ordem: (1) o `groupby` por mês, (2) o `nlargest`, (3) o terceiro "quebrar de
propósito" (o filtro que devolve zero), (4) a Etapa 4 da pílula.

**Nunca corte:** os dois códigos lado a lado na abertura **com o elogio antes**, o `.describe()`
com a média vs mediana, o **`NaN` ignorado em silêncio** com o `.isnull().sum()` no quadro, e as
Etapas 1, 2 e 3 da pílula — o 25% → 100% é o que a turma vai lembrar.

> Se estiver atrasado, sacrifique o bloco de `groupby`, não a pílula. O `groupby` eles aprendem
> sozinhos com a tarefa; o argumento da limpeza, não.

---

## Depois da aula — 15 minutos

- [ ] Mandar no grupo: link do notebook, a tarefa escrita e **a retrospectiva das dez aulas por escrito** — vale mais lida com calma
- [ ] Anotar em `duvidas-da-turma.md` o que te pegou
- [ ] Escrever para si mesmo, sem jargão: *qual a diferença entre dado faltando por acaso e faltando por um motivo?*
- [ ] **Metade do curso.** Vale mandar mensagem individual para cada aluno com uma frase do que você viu ele evoluir. Custa vinte minutos e é o que mais segura turma em curso longo
- [ ] **Preparar a mudança de formato:** da Aula 11 em diante não existe mais pílula separada. Vale revisar o plano do curso e ajustar o roteiro-padrão antes de escrever a próxima
