# Aula 15 — Roteiro minuto a minuto

**Sábado, 21/11/2026 — 10h00 às 12h00**
**Tema:** machine learning de verdade — e os três jeitos de se enganar sozinho
**Ambiente:** VS Code na máquina do aluno *(Colab para quem preferir)*
**Turma:** até 6 alunos

---

## O que esta aula é — e o que ela não é

Esta **não** é a aula de "como funciona um modelo". Essa é a 16.

Esta é a aula do **rigor**. Eles vão treinar um classificador de ponta a ponta em vinte minutos, e
depois passar uma hora descobrindo que o número que o modelo cuspiu não quer dizer o que parece
querer dizer. A aula é quase toda **quebrar de propósito** — três erros, cada um com número medido.

Isso muda o ritmo: os blocos de erro **são** a mão na massa. Eles estão no teclado desde 10h25,
rodando célula, olhando número e levando susto. O bloco final de 12 minutos é a versão sozinho.

---

## Objetivo da aula

Ao final, cada aluno deve:

1. Usar sem gaguejar as quatro palavras: **feature**, **rótulo**, **treino**, **teste**
2. Rodar um `train_test_split` e explicar **por que** ele existe
3. Medir acurácia nos dois lados e reconhecer **overfitting** pela distância entre os números
4. Nunca mais olhar uma acurácia sem perguntar: **quanto dá chutando?**
5. Ler uma **matriz de confusão** e dizer o que a acurácia escondeu
6. Reconhecer **vazamento de dado** pela pergunta *"no momento de prever, essa informação já existe?"*

**A dívida que abre a aula:** *"tudo que fiz até aqui foi regra que EU escrevi. Como faço o programa
decidir a partir do dado?"* — e, junto, a **Aula 3** volta: lá eles escreveram um classificador de
spam com `if` e depois viram um modelo aprender sozinho com 16 exemplos. Todo mundo achou mágica.
Hoje é a mesma coisa, **com rigor**.

| Gancho | De onde veio | Onde é pago |
|---|---|---|
| "quero que o programa decida a partir do dado" | Fim da Aula 14 | Abertura, 10h00 |
| O classificador de spam com `if` | Aula 3 | 10h00 e 10h18 — a regra medida de novo |
| "o modelo aprendeu com 16 exemplos" | Aula 3 (pílula de IA) | 10h18 — agora com treino e teste separados |
| Dado sujo levou de 100% para 25% | Aula 10 | 11h54 — no fechamento, lado a lado |
| `pip install scikit-learn` / `import sklearn` | Aula 11 (tabela nome ≠ import) | 10h12, e o `ModuleNotFoundError` real |
| `read_csv`, `value_counts`, olhar o dado antes | Aula 10 | 11h05 — o 95/5 aparece no `value_counts` |

> **A frase que amarra a aula:**
> **acurácia alta é a mentira mais confortável que existe.**

---

## Antes de começar

- [ ] Notebook testado (`46-aula-15-notebook.ipynb`) e link curto no quadro
- [ ] Slides abertos (`45-aula-15-slides.html`)
- [ ] **Confirmar `scikit-learn` instalado em todas as máquinas na sexta.** `python -m pip install scikit-learn` — é o maior risco de a aula travar nos primeiros dez minutos
- [ ] Ter o roteiro da **Aula 3** à mão, no trecho do classificador de spam. Você vai projetar o `if` original
- [ ] Rodar a célula 0 na sua máquina e confirmar que sai `0.81` no teste. Se não sair, é versão de biblioteca diferente — **refaça os números antes da aula e corrija nos slides**
- [ ] Deixar no quadro, do começo ao fim da aula, só isto: **treino · teste · baseline**

---

## Roteiro

### 10:00 — 10:12 | A dívida: a Aula 3 volta (12 min)

**Recolha o Projeto 2 (3 min).** Pergunte quem já subiu para o GitHub e quem travou no `push`.
Resolva na hora o que der, e marque o resto para depois da aula — hoje não dá para parar.

**Agora a dívida (9 min).** Projete o `if` da Aula 3, o original deles:

```python
if "promoção" in mensagem and "clique aqui" in mensagem:
    print("spam")
```

> "Vocês escreveram isso na terceira aula do curso. Funcionava. E aí eu mostrei um modelo
> aprendendo a mesma coisa com dezesseis exemplos, ninguém entendeu nada, e todo mundo achou lindo."

> "Sábado passado vocês terminaram a aula de git me perguntando: **tudo que eu fiz até aqui foi
> regra que EU escrevi. Como eu faço o programa decidir a partir do dado?** Hoje é isso."

Então a definição que você vai repetir a aula inteira — escreva no quadro:

> "Classificador de machine learning é um `if` **cujo limiar ninguém escreveu**. O computador
> procura o corte no dado. É isso. Não tem mais mágica que isso."

E a advertência que define a aula, dita com cara séria:

> "A parte fácil é treinar. São quatro linhas, vocês vão ver às dez e vinte. A parte difícil — a
> que separa quem sabe de quem acha que sabe — é **acreditar ou não** no número que sai no fim."

> "Hoje vocês vão treinar um modelo que acerta **100%**. E até o fim da aula vocês vão querer
> apagar esse modelo."

---

### 10:12 — 10:25 | O vocabulário certo (13 min)

**Primeiro o `import`, porque é o gancho da Aula 11 (3 min).** Não entregue de graça — pergunte:

> "Eu quero usar o scikit-learn. O que eu digito?"

Alguém vai dizer `import scikit-learn` ou `import scikit_learn`. Deixe rodar:

```python
import scikit_learn
```

```
ModuleNotFoundError: No module named 'scikit_learn'
```

> "Primeira linha da tabela da Aula 11. Instala com `scikit-learn`, importa como `sklearn`. Quem
> não tinha decorado, agora decorou — e quem decorou a tabela errada acabou de descobrir."

E mostre o pior caso, que nem roda:

```python
import scikit-learn
```

```
SyntaxError: invalid syntax
```

> "Hífen não vale em nome de variável, e o `import` espera um nome de variável. Esse erro nem chega
> a procurar a biblioteca."

**Agora as quatro palavras (10 min).** Projete a tabela e **não** avance antes de todo mundo
repetir em voz alta.

| Palavra | O que é | No dado de hoje |
|---|---|---|
| **feature** | uma informação de entrada, uma coluna | `tem_link`, `hora`, `tamanho` |
| **rótulo** | a resposta que você quer prever | `spam` (0 ou 1) |
| **treino** | os exemplos que o modelo pode ver | 300 mensagens |
| **teste** | os exemplos que o modelo **não** pode ver | 100 mensagens |

> "`X` são as features, `y` é o rótulo. Letra maiúscula e letra minúscula, e é a convenção de
> **todo** tutorial de machine learning do mundo. Decorem isso e metade do Stack Overflow fica
> legível."

Rode a célula 0 e mostre o dado:

```python
msg = pd.read_csv("mensagens.csv")
print(msg.head(5).to_string())
```

```
   tem_link  tem_promocao  qtd_emoji  tamanho  hora  remetente_desconhecido  moderador_apagou  spam
0         1             0          1       76     8                       1                 0     0
1         1             1          1      156    19                       0                 1     1
2         1             0          5      274    12                       1                 1     1
3         1             0          3      140     9                       0                 0     0
4         1             0          7      173     2                       1                 1     1
```

```python
print(msg["spam"].value_counts())
```

```
spam
0    205
1    195
Name: count, dtype: int64
```

Aponte duas coisas, nesta ordem:

> "Um: **tudo é número.** O modelo não lê texto. `tem_link` é zero ou um; `hora` é a hora;
> `tamanho` é quantos caracteres. Na Aula 3 vocês também não liam a frase inteira — olhavam se
> continha uma palavra. É a mesma ideia, com nome bonito: **feature**."

> "Dois: está quase meio a meio. Guardem isso, porque às onze e cinco eu vou trazer um dado que
> não está meio a meio, e o mundo vai cair."

**Deixe a coluna `moderador_apagou` passar sem comentário.** Se alguém perguntar, diga só *"é do
grupo, chego nela"*. Ela é a bomba do terceiro erro — não estrague.

---

### 10:25 — 10:45 | 💥 Erro nº 1 — testar no dado do treino (20 min)

**Primeiro o split, em silêncio (4 min).** Três linhas, sem filosofia ainda:

```python
from sklearn.model_selection import train_test_split

FEATURES = ["tem_link", "tem_promocao", "qtd_emoji",
            "tamanho", "hora", "remetente_desconhecido"]
X = msg[FEATURES]
y = msg["spam"]

X_treino, X_teste, y_treino, y_teste = train_test_split(
    X, y, test_size=0.25, random_state=15, stratify=y)

print("treino:", X_treino.shape)
print("teste :", X_teste.shape)
```

```
treino: (300, 6)
teste : (100, 6)
```

Explique os três argumentos e pare:

- `test_size=0.25` — um quarto fica de fora
- `random_state=15` — o sorteio é o mesmo toda vez *(sem isso, número diferente a cada execução — e o aluno acha que o modelo mudou)*
- `stratify=y` — mantém a mesma proporção de spam dos dois lados

**Antes de treinar, meça a regra da Aula 3 (3 min).** Este é o momento que amarra o curso:

```python
regra_ingenua = ((X_teste["tem_link"] == 1) & (X_teste["tem_promocao"] == 1)).astype(int)
print("regra ingenua       :", (regra_ingenua == y_teste).mean())

pontos = X_teste["tem_link"]*2 + X_teste["tem_promocao"]*2 + X_teste["remetente_desconhecido"]
regra_pontos = (pontos >= 3).astype(int)
print("regra de pontos     :", (regra_pontos == y_teste).mean())
```

```
regra ingenua       : 0.72
regra de pontos     : 0.8
```

> "A regra que vocês escreveriam na Aula 3 acerta **72%**. Uma versão ajustada na mão, com pesos
> que eu chutei olhando o dado, acerta **80%**. Guardem os dois. São o **chão** de hoje — e
> ninguém mais nesta aula vai poder dizer 'meu modelo é bom' sem bater nesses números."

**Agora o modelo, e a armadilha (6 min).** Treine e meça **no treino**. Faça com entusiasmo falso:

```python
from sklearn.tree import DecisionTreeClassifier
from sklearn.metrics import accuracy_score

modelo = DecisionTreeClassifier(random_state=15)
modelo.fit(X_treino, y_treino)

print("acuracia NO TREINO:", accuracy_score(y_treino, modelo.predict(X_treino)))
```

```
acuracia NO TREINO: 1.0
```

> "**Cem por cento.** Quatro linhas de código, 100% de acerto. Estou pensando em cobrar mais caro
> pelo curso."

Deixe a sala comemorar por uns segundos. Aí:

```python
print("acuracia NO TESTE :", accuracy_score(y_teste, modelo.predict(X_teste)))
```

```
acuracia NO TESTE : 0.81
```

> "Oitenta e um. **Dezenove pontos** de diferença entre o número que eu mostrei e o número que
> vale."

**A analogia, e use exatamente esta (4 min):**

> "Imagina um aluno que recebe o simulado **com o gabarito**. Decora as 300 respostas. Tira dez no
> simulado. Na prova, com questões novas, tira oito e um."

> "Ele não aprendeu a matéria. Ele **decorou o simulado**. E tem nome: **overfitting**."

Mostre que a decoreba é literal:

```python
print("profundidade da arvore:", modelo.get_depth())
print("folhas (regras no fim):", modelo.get_n_leaves())
```

```
profundidade da arvore: 13
folhas (regras no fim): 46
```

> "Quarenta e seis folhas para 300 exemplos. Ele praticamente abriu uma gavetinha para cada
> mensagem que viu. Não generalizou — arquivou."

**E a saída (3 min).** `max_depth` limita o tamanho da árvore:

```python
raso = DecisionTreeClassifier(max_depth=5, random_state=15)
raso.fit(X_treino, y_treino)
print("treino:", round(accuracy_score(y_treino, raso.predict(X_treino)), 4))
print("teste :", round(accuracy_score(y_teste,  raso.predict(X_teste)),  4))
```

```
treino: 0.8967
teste : 0.83
```

> "Olhem o que aconteceu: ficou **pior no treino** e **melhor no teste**. A distância caiu de 19
> pontos para 7."

> "Esse é o ofício. Você não persegue o número do treino — ele é fácil de enganar. Você persegue o
> número do teste **e a distância entre os dois.**"

Deixe esta no quadro:

> **Modelo que acerta tudo no treino quase sempre está mentindo para você.**

---

### 10:45 — 10:55 | Baseline: comparado a quê? (10 min)

Pergunte e **espere**: *"83% é bom?"*

Alguém vai dizer que sim. Então:

> "Bom **comparado a quê?** Essa é a pergunta. E quase ninguém faz."

Apresente o modelo mais burro que existe:

```python
from sklearn.dummy import DummyClassifier

burro = DummyClassifier(strategy="most_frequent")
burro.fit(X_treino, y_treino)
print("chutar sempre a classe maior:", accuracy_score(y_teste, burro.predict(X_teste)))
```

```
chutar sempre a classe maior: 0.51
```

> "O `DummyClassifier` olha o treino, vê qual resposta aparece mais, e responde **sempre aquela**.
> Nem olha as features. Isso se chama **baseline**: o chão do problema."

Projete a escada completa — todos no **mesmo** conjunto de 100 mensagens de teste:

| Quem decide | Acurácia no teste |
|---|---|
| chutar sempre a classe maior (`DummyClassifier`) | **0.51** |
| a regra ingênua da Aula 3 (`link` **e** `promoção`) | **0.72** |
| a regra de pontos, ajustada na mão | **0.80** |
| a árvore de decisão (`max_depth=5`) | **0.83** |

**Agora seja honesto, porque é aqui que a turma aprende de verdade:**

> "Leiam com calma. O modelo ganhou da minha regra na mão por **três pontos**. Três. Depois de
> toda essa conversa."

Deixe o desconforto no ar uns segundos. Depois a resposta, nos dedos:

> "Então para que serve? Três coisas. **Um:** eu não precisei inventar o limiar — e inventar
> limiar na mão, em problema com vinte colunas, é impossível. **Dois:** quando o spam mudar de
> cara, eu **retreino**, não reescrevo o `if`. **Três**, e é a mais importante: agora eu tenho um
> **número honesto** para dizer se melhorei."

> "Baseline não é formalidade de artigo acadêmico. É a diferença entre dizer *'meu modelo acerta
> 83%'* e dizer *'meu modelo acerta 83% onde chutar acerta 51%'*. **Só a segunda frase é
> informação.**"

---

### 10:55 — 11:05 | ☕ Pausa (10 min)

Avise antes de soltar:

> "Depois da pausa eu mostro uma acurácia de **95%** que não vale absolutamente nada. E essa é a
> que vocês vão ver num e-mail de trabalho algum dia."

---

### 11:05 — 11:25 | 💥 Erro nº 2 — os 95% que não valem nada (20 min)

**Troque de problema (3 min).** Agora é o mar da Ponta da Praia: **vai ter ressaca amanhã?**

> "Quem mora aqui sabe o que é ressaca: o mar sobe, invade a avenida, a Defesa Civil fecha a
> orla. Imagina que a prefeitura te contratou para avisar um dia antes."

Primeiro o hábito da Aula 10 — **olhe o dado antes**:

```python
mar = pd.read_csv("mar_santos.csv")
print(mar["ressaca"].value_counts())
print(mar["ressaca"].value_counts(normalize=True))
```

```
ressaca
0    760
1     40
Name: count, dtype: int64

ressaca
0    0.95
1    0.05
Name: proportion, dtype: float64
```

> "Noventa e cinco por cento dos dias não tem ressaca. Isso tem nome: dado **desbalanceado**. E
> quase todo problema que importa na vida real é assim — fraude, doença, defeito, acidente. O que
> você quer achar é **raro**."

**Agora o golpe (5 min).** Separe igual ao de antes e solte o modelo burro no dado novo:

```python
X_mar = mar[["vento_kmh", "pressao_hpa", "temp_agua", "mes"]]
y_mar = mar["ressaca"]

Xm_treino, Xm_teste, ym_treino, ym_teste = train_test_split(
    X_mar, y_mar, test_size=0.25, random_state=15, stratify=y_mar)

burro_mar = DummyClassifier(strategy="most_frequent")
burro_mar.fit(Xm_treino, ym_treino)
print("acuracia do modelo BURRO:", accuracy_score(ym_teste, burro_mar.predict(Xm_teste)))
```

```
acuracia do modelo BURRO: 0.95
```

Pare. Deixe o número na tela.

> "Noventa e cinco por cento de acurácia. Esse modelo responde **'não vai ter ressaca'** para
> qualquer dia que você perguntar. Para sempre. Não olhou vento, não olhou pressão, não olhou
> nada."

> "Se alguém te mandar um e-mail dizendo *'treinei um modelo e deu 95% de acurácia'*, a primeira
> pergunta **não** é 'que modelo?'. É: **quanto dá chutando?**"

Agora o modelo de verdade, no mesmo dado:

```python
modelo_mar = DecisionTreeClassifier(max_depth=4, random_state=15)
modelo_mar.fit(Xm_treino, ym_treino)
print("acuracia do modelo de verdade:", accuracy_score(ym_teste, modelo_mar.predict(Xm_teste)))
```

```
acuracia do modelo de verdade: 0.96
```

> "Noventa e seis contra noventa e cinco. **Um ponto.** Todo o trabalho para um ponto acima de
> chutar. E se eu te mostrasse só o 96%, você ia me achar competente."

**A matriz de confusão (9 min). Este é o bloco técnico mais importante da aula.**

> "A acurácia é **um** número, e um número mistura tudo. A matriz de confusão abre nos quatro casos:
> acertou o não, errou o não, acertou o sim, errou o sim."

```python
from sklearn.metrics import confusion_matrix

def mostrar(titulo, modelo_qualquer):
    mat = confusion_matrix(ym_teste, modelo_qualquer.predict(Xm_teste))
    print(titulo)
    print(pd.DataFrame(mat,
                       index=["real: SEM ressaca", "real: COM ressaca"],
                       columns=["previu SEM", "previu COM"]))
    print()

mostrar("--- modelo BURRO ---", burro_mar)
mostrar("--- modelo de verdade ---", modelo_mar)
```

```
--- modelo BURRO ---
                   previu SEM  previu COM
real: SEM ressaca         190           0
real: COM ressaca          10           0

--- modelo de verdade ---
                   previu SEM  previu COM
real: SEM ressaca         189           1
real: COM ressaca           7           3
```

Aponte com o dedo na tela, na coluna da direita do primeiro:

> "**Zero. Zero.** O modelo de 95% nunca, nem uma vez, avisou que ia ter ressaca. Teve dez
> ressacas no teste e ele perdeu **todas as dez**."

> "O modelo de verdade pegou três das dez. Três é pouco. Mas três é diferente de zero — e a
> acurácia dos dois é praticamente igual."

> "Se isso fosse o alerta da Defesa Civil, o modelo de 95% deixava as dez ressacas passarem sem
> avisar ninguém. Com um relatório bonito dizendo 95%."

E o relatório que o sklearn dá de graça:

```python
from sklearn.metrics import classification_report
print(classification_report(ym_teste, burro_mar.predict(Xm_teste), zero_division=0))
```

```
              precision    recall  f1-score   support

           0       0.95      1.00      0.97       190
           1       0.00      0.00      0.00        10

    accuracy                           0.95       200
   macro avg       0.47      0.50      0.49       200
weighted avg       0.90      0.95      0.93       200
```

> "Não precisa entender tudo hoje. Olhem **uma** coisa: a coluna `recall`, na linha `1`.
> **Zero vírgula zero zero.**"

> "`recall` é a pergunta: *dos dias que tiveram ressaca de verdade, quantos eu peguei?* Zero por
> cento. E na mesma tabela, três linhas abaixo: `accuracy 0.95`."

> **Em dado desbalanceado, acurácia é quase inútil. Quando o que você procura é raro, o número que
> importa é quantos dos raros você pegou.**

---

### 11:25 — 11:42 | 💥 Erro nº 3 — vazamento de dado (17 min)

Abra assim, e não suavize:

> "Esse é o erro mais caro que existe em projeto de machine learning, e o menos comentado em curso.
> Eu já vi empresa torrar seis meses em cima dele."

**Volte às mensagens (5 min).** Lembre a coluna que ficou de fora:

```python
print(msg.columns.tolist())
```

> "Tem uma coluna que eu pulei de propósito às dez e vinte: **`moderador_apagou`**. Alguém me diz
> por que ela não seria uma feature legítima? É informação sobre a mensagem, está na tabela, é
> número, zero ou um."

Ninguém vai achar problema. Ótimo. Coloque:

```python
FEATURES_VAZADAS = FEATURES + ["moderador_apagou"]
Xv = msg[FEATURES_VAZADAS]
Xv_treino, Xv_teste, yv_treino, yv_teste = train_test_split(
    Xv, y, test_size=0.25, random_state=15, stratify=y)

vazado = DecisionTreeClassifier(random_state=15)
vazado.fit(Xv_treino, yv_treino)
print("acuracia no treino:", accuracy_score(yv_treino, vazado.predict(Xv_treino)))
print("acuracia no TESTE :", accuracy_score(yv_teste,  vazado.predict(Xv_teste)))
```

```
acuracia no treino: 1.0
acuracia no TESTE : 1.0
```

> "Cem por cento **no teste**. Leiam de novo: no **teste**. Não é overfitting — é dado que ele
> nunca viu. A gente fez tudo certo: separou, usou `stratify`, mediu do lado de fora."

> "E deu perfeito. Perfeito. Quem aqui ficaria feliz?"

**De onde ele tirou a resposta (3 min):**

```python
importancia = pd.Series(vazado.feature_importances_, index=FEATURES_VAZADAS)
print(importancia.sort_values(ascending=False).round(3))
```

```
moderador_apagou          0.909
hora                      0.036
tamanho                   0.032
tem_link                  0.014
remetente_desconhecido    0.007
tem_promocao              0.002
qtd_emoji                 0.000
dtype: float64
```

> "Noventa e um por cento da decisão vem de **uma** coluna. As outras seis viraram enfeite."

**A pergunta que resolve (4 min).** Faça a turma pensar em *quando*:

> "Pensem em **quando** cada informação existe. O moderador apaga a mensagem **depois** de ler e
> concluir que é spam. Então `moderador_apagou` **é a resposta**, escrita com outro nome."

> "O modelo não aprendeu a reconhecer spam. Aprendeu a **copiar o moderador**. E o moderador já
> tinha a resposta."

> "Agora a parte prática: chegou mensagem nova no grupo agora. O moderador ainda não viu. **Essa
> coluna não existe.** É justamente a decisão que você queria que o modelo tomasse."

**Mostre o erro real (2 min).** Não conte — rode:

```python
mensagem_nova = pd.DataFrame([{
    "tem_link": 1, "tem_promocao": 1, "qtd_emoji": 4,
    "tamanho": 180, "hora": 2, "remetente_desconhecido": 1,
}])

vazado.predict(mensagem_nova)
```

```
ValueError: The feature names should match those that were passed during fit.
Feature names seen at fit time, yet now missing:
- moderador_apagou
```

> "**Leiam a mensagem.** O sklearn está dizendo exatamente o problema: o modelo quer uma coluna que
> você não tem. E você nunca vai ter, no momento em que precisa prever."

> "Um modelo de 100% de acurácia que **não roda**. É esse o preço do vazamento. A versão honesta,
> sem essa coluna, dá 0.81 — e essa aí você usa amanhã."

**Como se proteger (3 min).** Uma pergunta por feature, e deixe no quadro:

> **No momento em que eu preciso prever, essa informação já existe?**

Projete exemplos reais — peça para a turma dizer o porquê de cada um antes de você falar:

| Você quer prever | A feature traiçoeira | Por quê |
|---|---|---|
| se o cliente vai cancelar | `data_do_cancelamento` | só existe depois de cancelar |
| se o paciente tem a doença | `remedio_prescrito` | o médico já diagnosticou |
| se o aluno vai desistir | `nota_final` | vem no fim do semestre |
| se a peça vai quebrar | `custo_do_reparo` | o reparo já aconteceu |

E a regra de bolso para fechar o bloco:

> **Acurácia suspeita de boa é bug, não é talento.** Quando der perto de 100%, procure o vazamento
> antes de comemorar. Em dado real, 100% não existe.

---

### 11:42 — 11:54 | 🛠️ Mão na massa: de ponta a ponta, sozinho (12 min)

> "Agora vocês. Mesmo dado das mensagens, do zero, sem olhar o que eu fiz. Seis passos."

No quadro:

1. ler `mensagens.csv`
2. separar `X` (features **honestas**) e `y`
3. `train_test_split` com `test_size=0.3` e `random_state=42`
4. treinar `DecisionTreeClassifier(max_depth=4, random_state=42)`
5. medir acurácia **no treino** e **no teste**
6. medir o `DummyClassifier` no mesmo teste

E imprimir três frases:

```
meu modelo no teste:   0.??
chutar a classe maior: 0.??
distancia treino-teste: ?? pontos
```

**O que deve sair** *(para você conferir enquanto circula)*:

```
meu modelo no teste:   0.8583
chutar a classe maior: 0.5167
distancia treino-teste: 4.2 pontos
```

Circulando, use estas três perguntas e nenhuma outra:

- *"ganhou do baseline? por quanto?"*
- *"a distância treino-teste é grande? ele decorou?"*
- *"muda o `random_state` para 99 e roda de novo. O que aconteceu?"*

A terceira é a melhor da aula. Com `test_size=0.25` e `max_depth=5`, a acurácia de teste varia de
**0.83** a **0.90** só trocando o `random_state` do split. Quando alguém notar, pare a turma:

> "Olhem o que a gente acabou de descobrir. **O mesmo modelo, no mesmo dado**, mede 83% ou 90%
> dependendo de quais 100 mensagens caíram no teste. Então nenhum desses números é 'o' número — é
> uma medida com erro."

> "Resolver isso bem tem nome, chama **validação cruzada**, e é assunto da aula que vem. Hoje
> basta vocês desconfiarem de qualquer acurácia com duas casas decimais."

---

### 11:54 — 12:00 | O que isso destrava + tarefa (6 min)

**O que isso destrava (3 min).** Projete e leia:

> "Vocês não treinaram um modelo melhor hoje. Vocês ganharam o direito de **não acreditar** num
> número — inclusive nos seus."
>
> - ler uma métrica que alguém te mostra e saber **qual pergunta fazer**
> - perguntar *"isso é mais ou menos que chutar?"* — que quase ninguém pergunta
> - reconhecer **vazamento de dado**, o erro mais caro de projeto de empresa
> - e ter o vocabulário para ler qualquer tutorial: feature, rótulo, treino, teste, baseline

Amarre com a Aula 10, lado a lado — é o fechamento da aula:

> "Na Aula 10 o dado sujo derrubou um modelo de 100% para 25%, e **não deu erro nenhum**. Hoje o
> dado estava limpo, impecável, e o modelo mentiu de três jeitos diferentes — também sem dar erro
> nenhum."

> "Machine learning não quebra. Ele te dá um **número errado com cara de certo**. Por isso a aula
> de hoje existe antes da aula de como o modelo funciona."

**A tarefa (2 min):**

1. **Rodar a mão na massa** e mandar no grupo **as três frases** do relatório
2. **Mexer no `max_depth`:** testar `1`, `3`, `5`, `10` e `None`, montar uma tabelinha com treino e
   teste de cada um e dizer **onde começa a decoreba** *(resposta: entre 5 e 10 — em `10` o treino
   vai a 0.9867 e o teste cai para 0.79)*
3. **Voltar ao `mar_santos.csv`** e tentar pegar **mais de 3** das 10 ressacas. Dica:
   `class_weight="balanced"`. Olhar a matriz, não a acurácia *(com `balanced` ele pega 5 de 10, mas
   a acurácia cai de 0.96 para 0.835 e aparecem 28 falsos alarmes — e é exatamente essa troca que
   eu quero que eles vejam)*
4. **Caça ao vazamento:** pensar num problema do próprio trabalho, listar 5 features e passar a
   pergunta *"no momento de prever, isso já existe?"* em cada uma. Mandar no grupo a que não passou
   — todo mundo tem uma

**A pergunta que fica (1 min):**

> "A gente usou `DecisionTreeClassifier` a aula inteira e tratou como caixa preta: entra feature,
> sai resposta."

> "**Mas como ele decide por dentro?** Por que 'árvore'? Onde exatamente ele achou o limiar que
> ninguém escreveu? E existem outros modelos — por que eu escolheria um e não o outro?"

*(Aula 16: primeiro modelo com `sklearn`, por dentro.)*

---

## Se sobrar tempo

- **`cross_val_score`**, só para mostrar o número, sem explicar a mecânica. Treina cinco vezes com
  cinco recortes diferentes:
  ```
  [0.8875, 0.925, 0.875, 0.85, 0.875]   media 0.8825
  ```
  > "Cinco medições do mesmo modelo. Vai de 0.85 a 0.925. **Qual é a verdadeira?** Nenhuma — a
  > verdadeira é a média, e com a variação do lado. Isso é a aula 16."
- `modelo.feature_importances_` no modelo **honesto**: `hora` 0.380, `remetente_desconhecido` 0.298,
  `tamanho` 0.226. Rende conversa boa: por que a hora importa tanto?
- `DummyClassifier(strategy="stratified")` ao lado do `most_frequent` — dois baselines diferentes
- Exportar a árvore rasa com `export_text` e **ler os `if`** em voz alta. É a ponte perfeita para a
  Aula 16 — mas só se sobrar mesmo, senão rouba a aula que vem

## Se faltar tempo

Corte nesta ordem:

1. O bloco do `classification_report` — **mas mantenha a matriz de confusão**, que é onde o `0` mora
2. O `max_depth=5` do erro nº 1 (fica só treino 1.0 vs teste 0.81, que já entrega a lição)
3. O `get_depth()` / `get_n_leaves()` das 46 folhas
4. O `SyntaxError` do `import scikit-learn` com hífen
5. A tabela de vazamentos reais — mas **deixe a pergunta no quadro**

**Nunca corte:** os dois números do erro nº 1 (1.0 e 0.81) com a analogia do simulado · o
`DummyClassifier` dando 0.95 **com a matriz de confusão de zeros do lado** · o `moderador_apagou`
dando 100% e a pergunta *"no momento de prever, essa informação já existe?"* · e a escada de
baselines. Se der para fazer só uma coisa nesta aula, faça a escada — ela é a aula inteira num
quadro.

---

## Depois da aula — 15 minutos

- [ ] Mandar no grupo: link do notebook, a tarefa escrita, e **a escada de baselines como imagem**
      (0.51 → 0.72 → 0.80 → 0.83). Eles vão querer mostrar isso para alguém
- [ ] Mandar também a pergunta do vazamento em uma linha só, para ficar fixada no grupo:
      *"no momento de prever, essa informação já existe?"*
- [ ] Anotar em `duvidas-da-turma.md` quem ainda confundiu **treino** com **teste** — é o sinal de
      que a Aula 16 precisa começar com cinco minutos de revisão
- [ ] **Ler as respostas da tarefa 4** (caça ao vazamento no trabalho deles). É o melhor diagnóstico
      de quem entendeu a aula de verdade, e costuma render exemplo bom para reusar na Aula 16
- [ ] Separar o Projeto 2 de quem entregou e começar a correção — a devolutiva é na Aula 16
- [ ] **Preparar a Aula 16:** decidir se a árvore vai ser desenhada com `plot_tree` ou lida com
      `export_text`. Teste os dois na sexta e escolha o que couber na tela da sala
- [ ] **Nota Unipds — escrever para si mesmo, sem jargão:** qual é a diferença entre **viés** e
      **variância**, usando os números de hoje (a árvore cheia e a `max_depth=5`) como exemplo; e
      **por que validação cruzada existe**, dado que trocar o `random_state` mudou a acurácia de
      0.83 para 0.90
