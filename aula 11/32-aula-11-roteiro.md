# Aula 11 — Roteiro minuto a minuto

**Sábado, 24/10/2026 — 10h00 às 12h00**
**Tema:** bibliotecas e `pip` — usar código que outras pessoas escreveram · **abre o Módulo 3**
**Ambiente:** VS Code na máquina do aluno *(Colab para quem preferir)*
**Turma:** até 6 alunos

---

## ⚠️ Primeira aula do formato novo

Acabou a Pílula de IA como bloco separado — a Aula 10 já avisou a turma. Isso libera uns 15
minutos, e eles vão **para o teclado**, não para mais conteúdo. O fechamento agora é um bloco curto
de 6 minutos chamado *"o que isso destrava"*.

Diga isso em voz alta na abertura. Mudança de formato anunciada é sensação de progresso;
mudança silenciosa é só confusão.

---

## Objetivo da aula

Ao final, cada aluno deve:

1. Saber a diferença entre **biblioteca que já vem com o Python** e biblioteca que precisa instalar
2. Instalar um pacote com `pip install` e usar na mesma sessão
3. Saber que **nome de instalação e nome de import podem ser diferentes** — e o que fazer quando são
4. Descobrir a versão de qualquer biblioteca instalada, **inclusive das que não têm `__version__`**
5. Gerar e ler um `requirements.txt`
6. Ter encontrado, instalado e usado **uma biblioteca que eu não ensinei**, só pela documentação

**A dívida que abre a aula:** *"o pandas não veio com o Python. Quem escreveu? Como eu pego outras?"*

| Gancho | De onde veio | Onde é pago |
|---|---|---|
| Quem escreveu o pandas | Fim da Aula 10 | Abertura, 10h00 — a página do PyPI projetada |
| O `diagnosticar()` da tarefa | Tarefa da Aula 10 | Abertura — recolha antes |

> **A frase que amarra a aula:**
> **instalar é a parte fácil. A habilidade é ler a documentação de uma biblioteca que ninguém te ensinou.**

---

## Antes de começar

- [ ] Notebook testado (`34-aula-11-notebook.ipynb`) e link curto no quadro
- [ ] Slides abertos (`33-aula-11-slides.html`)
- [ ] **Abrir no navegador, antes da aula:** `pypi.org/project/pandas` — vai ser projetado
- [ ] Testar `pip install openpyxl qrcode rich` na sua máquina para saber quanto demora na rede da sala
- [ ] Ter o `vendas.xlsx` à mão (a célula 0 do notebook gera)
- [ ] **Quem está no Colab:** avisar que ali as bibliotecas somem quando a sessão reinicia — é o preço de não instalar nada

---

## Roteiro

### 10:00 — 10:15 | A dívida: quem escreveu o pandas (15 min)

**Recolha a tarefa (4 min).** Pergunte quem rodou o `diagnosticar()` numa planilha **de verdade** e
**qual foi a surpresa**. Quase sempre tem. Essa é a melhor entrada possível: eles acabaram de usar
uma biblioteca para achar problema em dado próprio, e hoje a aula é sobre de onde ela veio.

**Agora a dívida (11 min).** Projete `pypi.org/project/pandas`. Deixe a turma olhar antes de falar.

Aponte três coisas da página:

- o **número de downloads** — é gigante
- o **nome das pessoas** que mantêm
- a **data da última versão** — alguém mexeu nisso recentemente

> "O PyPI é a prateleira. São centenas de milhares de bibliotecas, escritas por gente do mundo
> inteiro, de graça. Quando vocês rodaram `pip install pandas`, a máquina de vocês **baixou daqui**."

E então a virada que define o Módulo 3:

> "Até a Aula 10 vocês escreveram tudo. A partir de hoje, a pergunta muda: **antes de escrever, já
> existe?** Na prática, quase sempre existe — e programador experiente passa mais tempo
> procurando e lendo do que digitando."

**Não deixe virar preguiça.** Repita o aviso da Aula 10 com outras palavras:

> "Isso não anula as dez aulas. Quem não sabe o que a biblioteca faz por dentro trava no primeiro
> caso em que ela não faz exatamente o que precisa. A diferença é que agora vocês **sabem o
> suficiente** para ler o que os outros escreveram."

---

### 10:15 — 10:30 | Vocês já usam biblioteca desde a Aula 1 (15 min)

**Abra com a pergunta,** não com a resposta: *"quantas bibliotecas vocês já usaram neste curso?"*

A maioria vai dizer uma ou duas. Então mostre:

```python
import random       # Aula 4 — sorteio do jogo de adivinhação
import datetime     # Aula 5
import csv          # Aula 8 — DictReader
import json         # Aula 7
import os           # Aula 8 — caminhos de arquivo
import statistics   # média e mediana sem conta na mão
```

> "Seis. Desde a Aula 4. E **nenhuma delas vocês instalaram** — já vêm junto com o Python."

Isso se chama **biblioteca padrão**, e é a primeira coisa a checar:

> "Antes de instalar qualquer coisa, pergunte: **já não vem junto?** Data, hora, aleatório, JSON,
> CSV, arquivo, matemática, estatística básica — tudo isso já está aí."

Mostre o truque de onde o arquivo mora, que torna a coisa concreta:

```python
import random
print(random.__file__)
```

```
/usr/lib/python3.11/random.py
```

> "**É um arquivo `.py`.** Escrito em Python, igual ao que vocês escrevem. Dá para abrir e ler. Não
> tem nada de mágico — é código de alguém, num arquivo, no seu computador."

Essa frase costuma mudar a relação da turma com biblioteca. Vale o minuto.

---

### 10:30 — 10:50 | `pip install`: abrindo um formato novo (20 min)

Chegue pela necessidade, não pelo comando. Pergunte: *"a planilha que vocês trouxeram era `.csv`
ou `.xlsx`?"*

Quase sempre `.xlsx`. E aí:

```python
import pandas as pd
df = pd.read_excel("vendas.xlsx")
```

```
ImportError: Missing optional dependency 'openpyxl'.
Use pip or conda to install openpyxl.
```

> "Olhem que erro educado. Ele diz **o que falta** e **o que fazer**. Nem todo erro é assim, mas
> quando é, obedeça."

```
pip install openpyxl
```

E agora funciona:

```python
df = pd.read_excel("vendas.xlsx")
print(df.head(3))
```

```
      mes      loja  pecas  faturamento
0 2026-01  Shopping     81     24421.58
1 2026-01  Shopping     67     15696.03
2 2026-01    Online     34      7616.51
```

```python
print(df.groupby("loja")["faturamento"].sum().round(2))
```

```
loja
Centro      163393.82
Online      153940.47
Shopping    185658.66
```

> "**Um comando** e o pandas passou a ler Excel. Vocês não aprenderam nada novo de pandas — o
> `groupby` é o mesmo de sábado passado. Só abriu um formato a mais."

**O ponto pedagógico, diga assim:**

> "É isso que biblioteca faz: não te ensina a programar melhor, **te dá alcance**. O mesmo código
> que vocês já sabem passa a funcionar em cima de mais coisa."

---

### 10:50 — 11:00 | Pausa (10 min)

---

### 11:00 — 11:25 | 💥 Os quatro jeitos de quebrar (25 min)

Abra dizendo o porquê:

> "`pip install` funciona em cinco segundos. O que toma tempo na vida real são estes quatro. Vocês
> vão bater em todos — melhor bater aqui."

**💥 nº 1 — a biblioteca que não existe (4 min)**

```python
import tabelinha_magica
```

```
ModuleNotFoundError: No module named 'tabelinha_magica'
```

> "Esse é o erro mais comum do Python depois do de indentação. E ele quer dizer **uma** de três
> coisas: ou você não instalou, ou escreveu o nome errado, ou instalou no lugar errado. Nessa
> ordem de probabilidade."

**💥 nº 2 — nome de instalação ≠ nome de import (8 min). Este é o que mais confunde.**

Projete a tabela e deixe no quadro a aula inteira:

| Você instala | Você importa |
|---|---|
| `pip install scikit-learn` | `import sklearn` |
| `pip install pillow` | `import PIL` |
| `pip install beautifulsoup4` | `import bs4` |
| `pip install opencv-python` | `import cv2` |
| `pip install pyyaml` | `import yaml` |
| `pip install python-dateutil` | `import dateutil` |

> "Não tem regra. Não dá para deduzir. São nomes históricos, escolhidos por pessoas diferentes em
> épocas diferentes."

> "**E é por isso que você lê a documentação.** A primeira linha de qualquer página de biblioteca
> mostra o `pip install` e o `import`. Decorar é impossível; consultar leva cinco segundos."

Amarre com o `sklearn`, que eles já usaram sem perceber:

> "Reparem no primeiro da lista. Vocês já viram `import sklearn` nas pílulas de IA. O pacote chama
> `scikit-learn`. Se um dia precisarem instalar na mão e escreverem `pip install sklearn`, vai
> acontecer coisa estranha."

**💥 nº 3 — instalei e continua dizendo que não existe (8 min). É o mais frustrante.**

> "Esse é o clássico: você instala, dá certo, roda o programa e ele jura que a biblioteca não
> existe. Dá vontade de jogar o computador pela janela."

A causa: o `pip` instalou **num Python** e o programa rodou **em outro**. O jeito de conferir:

```python
import sys
print(sys.executable)
```

```
/usr/bin/python3
```

E no terminal:

```
pip -V
```

> "Se os dois caminhos não baterem, achou o problema. No VS Code, `Ctrl+Shift+P` →
> `Python: Select Interpreter` resolve. No Colab isso não acontece, porque só existe um Python lá."

Deixe a receita no quadro — resolve 90% dos casos:

```
python -m pip install nome-da-biblioteca
```

> "`python -m pip` garante que o pip usado é o do **mesmo** Python que vai rodar seu código. Pegue
> o hábito: use sempre essa forma e você nunca mais vê esse erro."

**💥 nº 4 — o pacote com nome parecido (5 min). Esse é de segurança.**

> "Se eu digitar `pip install pandsa`, com as letras trocadas, o que acontece?"

Deixe responderem. Depois:

> "Às vezes: nada, erro de não encontrado. Às vezes: **instala**. Existe gente que registra pacote
> com nome quase igual ao de bibliotecas famosas esperando o erro de digitação. Chama
> *typosquatting*."

> "Vocês vão rodar `pip install` com a máquina de vocês e com as permissões de vocês. **Confira o
> nome na documentação oficial antes.** Não é paranoia; já aconteceu com biblioteca grande."

Regra prática para o quadro:

- link da documentação oficial ou do PyPI, **não** o primeiro resultado de busca
- olhe downloads e data da última versão na página do PyPI
- desconfie de pacote novo, com poucos downloads e nome parecido com um famoso

---

### 11:25 — 11:40 | `requirements.txt` — o fim do "na minha máquina funciona" (15 min)

Chegue pelo problema:

> "Você termina o Projeto 2, manda para um colega, e ele abre: `ModuleNotFoundError`. Óbvio — as
> bibliotecas estão na **sua** máquina, não na dele. Como você diz a ele o que instalar?"

Deixe alguém sugerir "escreve uma lista". Exato:

```
pip freeze > requirements.txt
```

```
openpyxl==3.1.5
pandas==3.0.2
qrcode==8.2
rich==15.0.0
```

E do outro lado:

```
pip install -r requirements.txt
```

> "Um arquivo de texto com nome e **versão**. Quem receber roda um comando e fica com exatamente as
> mesmas bibliotecas que você."

**Por que a versão está ali — e por que importa:**

```python
import pandas as pd
print(pd.__version__)
```

```
3.0.2
```

> "Biblioteca muda. O que funcionava na versão 1 pode ter mudado de nome na versão 3. Travar a
> versão é dizer: *funciona com esta aqui*."

**E o caso que a turma vai encontrar:** biblioteca sem `__version__`.

```python
import qrcode
print(qrcode.__version__)
```

```
AttributeError: module 'qrcode' has no attribute '__version__'
```

> "Nem toda biblioteca expõe a versão assim. Mas o Python sabe, porque o pip anotou:"

```python
from importlib.metadata import version
print(version("qrcode"))
```

```
8.2
```

> "Guardem esse. Funciona para **qualquer** pacote instalado, tendo `__version__` ou não."

**Sobre ambiente virtual:** mencione, não ensine agora.

> "Existe uma coisa chamada ambiente virtual, que dá para cada projeto a sua própria caixa de
> bibliotecas, sem misturar. É a solução definitiva para o erro nº 3. Hoje a gente só precisa que
> vocês saibam que existe e que o nome é `venv` — vai aparecer no Projeto 2."

---

### 11:40 — 11:54 | 🛠️ Mão na massa: usar uma biblioteca que eu não ensinei (14 min)

**Este é o bloco mais importante da aula.** Avise que é prova de autonomia, não exercício.

> "Vou dar uma necessidade e **não vou dizer como se faz**. Vocês procuram, instalam, leem a
> documentação e fazem funcionar. É exatamente o que você faz num emprego."

Duas opções, cada aluno escolhe uma:

**A — gerar um QR Code** com o link do repositório do curso
**B — imprimir uma tabela bonita no terminal** com o faturamento por loja

Deixe-os procurar. Circule e **não entregue o nome do pacote** — pergunte *"o que você digitou na
busca?"* e ajude a melhorar a busca, não a pular a etapa.

Resultado esperado em A:

```python
import qrcode
img = qrcode.make("https://github.com/guilhermedclima/mestresDoXGH")
img.save("qr.png")
```

Resultado esperado em B:

```
       Faturamento por loja
┏━━━━━━━━━━┳━━━━━━━━━━━━━┳━━━━━━━┓
┃ Loja     ┃ Faturamento ┃ Pecas ┃
┡━━━━━━━━━━╇━━━━━━━━━━━━━╇━━━━━━━┩
│ Shopping │ 185658.66   │ 612   │
│ Centro   │ 163393.82   │ 579   │
│ Online   │ 153940.47   │ 534   │
└──────────┴─────────────┴───────┘
```

**Quando alguém conseguir, pare a turma e pergunte à pessoa:** *"como você achou?"* A resposta dela
ensina mais que a sua. Normalmente é alguma versão de "procurei *python qr code* e abri o primeiro
link que era documentação".

> "É isso. Não tem segredo, não tem atalho de gente experiente. A diferença é que quem tem prática
> **desconfia mais rápido** do link errado e **lê o exemplo antes** de copiar."

---

### 11:54 — 12:00 | O que isso destrava + tarefa (6 min)

**O que isso destrava (3 min).** Projete e leia:

> "Com `pip install`, estas coisas deixaram de ser impossíveis e viraram *uma tarde de trabalho*:"
>
> - ler e escrever **Excel**, **PDF**, **Word**
> - **buscar dado da internet** — sábado que vem
> - mandar **e-mail**, mexer em **imagem**, falar com **banco de dados**
> - e, do Módulo 4 em diante, **treinar modelo** e **chamar IA**

> "Vocês não ficaram melhores em Python hoje. Ficaram com **alcance**."

**A tarefa (2 min):**

1. **Rodar o `diagnosticar()` da Aula 10 num `.xlsx`** — agora que o `read_excel` funciona, use uma planilha de verdade sua
2. **Gerar o `requirements.txt`** do seu Projeto 1 e commitar junto *(se o Projeto 1 não tiver pasta própria ainda, crie uma)*
3. **Achar uma biblioteca sozinho para uma necessidade sua.** Qualquer uma. Instale, faça rodar e **mande no grupo: o que você queria, o que você digitou na busca, e o pacote que achou.** O caminho interessa mais que o resultado
4. **Desafio:** fazer o programa do QR Code perguntar o link com `input()` e salvar o arquivo com nome escolhido pelo usuário

**A pergunta que fica (1 min):**

> "Hoje a gente instalou uma biblioteca que lê Excel do **seu** computador. Semana que vem a gente
> instala uma que busca dado **de fora** — cotação do dólar, clima, informação de site."

> "Mas de onde vem esse dado? Quem está do outro lado? E por que alguém deixaria eu pegar de graça?"

*(Aula 12: APIs e `requests`.)*

---

## Se sobrar tempo

- `pip list` e `pip show pandas` — ver o que está instalado e quem mantém
- Abrir o `random.py` no VS Code e ler a função `randint`. **Costuma impressionar**: é código comum, legível, escrito por gente
- `python -m venv .venv` ao vivo, só para mostrar que são três segundos

## Se faltar tempo

Corte nesta ordem: (1) o bloco de `venv`, (2) o erro nº 4 (typosquatting) — mas **deixe a regra no
quadro**, (3) o `importlib.metadata`, (4) a segunda opção da mão na massa.

**Nunca corte:** a página do PyPI projetada na abertura, o erro nº 2 (nome ≠ import) **com a tabela
no quadro**, o erro nº 3 com a receita `python -m pip`, e a mão na massa. A mão na massa é a aula
— o resto é preparação para ela.

---

## Depois da aula — 15 minutos

- [ ] Mandar no grupo: link do notebook, a tarefa escrita e **a tabela nome-de-instalação × nome-de-import** (eles vão consultar por meses)
- [ ] Anotar em `duvidas-da-turma.md` o que te pegou
- [ ] **Ler as buscas que eles mandarem na tarefa 3.** É o diagnóstico mais honesto que existe de quão autônoma a turma está ficando
- [ ] **Preparar a Aula 12:** confira se a API que você vai usar está no ar e se funciona sem chave. Se precisar de chave, gere a sua na sexta e teste
- [ ] Escrever para si mesmo, sem jargão: *o que um ambiente virtual resolve que o `requirements.txt` sozinho não resolve?*
