# Aula 13 — Roteiro minuto a minuto

**Sábado, 07/11/2026 — 10h00 às 12h00**
**Tema:** automação de verdade — do notebook para o script que roda sozinho
**Ambiente:** VS Code na máquina do aluno · **terminal aberto o tempo todo** · Windows
**Turma:** até 6 alunos

---

## Objetivo da aula

Ao final, cada aluno deve:

1. Saber a diferença entre **notebook** e **arquivo `.py`**, e rodar o `.py` pelo terminal
2. Entender para que serve `if __name__ == "__main__"` — e ter visto o que acontece sem ele
3. Receber um argumento de linha de comando com `sys.argv`
4. Trocar `print` por **`logging` escrevendo em arquivo**, com data e hora
5. Fazer a pergunta **"o que acontece se isto rodar duas vezes?"** antes de agendar qualquer coisa
6. Ter um **modo simulação** no próprio script
7. Ter agendado o script no **Agendador de Tarefas do Windows** e visto rodar sem ele apertar nada

**A dívida que abre a aula:** *"isso só roda quando eu aperto o play. E se eu quiser que rode sozinho?"*

| Gancho | De onde veio | Onde é pago |
|---|---|---|
| *"só roda quando eu aperto o play"* | Fim da Aula 12 | Abertura, 10h00 |
| O buscador de CEP da tarefa | Tarefa da Aula 12 | Abertura — recolha antes |
| `os` e caminho de arquivo | Aula 8 | 11:00 — o `os.getcwd()` |
| `try/except` e stack trace | Aula 9 | 11:10 — deixa de ser zelo e vira requisito |
| `requirements.txt` e `sys.executable` | Aula 11 | 11:38 — o agendador chama *qual* Python? |

> **A frase que amarra a aula:**
> **script agendado erra sozinho, de madrugada, sem ninguém olhando.**

---

## Antes de começar

- [ ] Notebook testado (`40-aula-13-notebook.ipynb`) e link curto no quadro
- [ ] Slides abertos (`39-aula-13-slides.html`)
- [ ] **Terminal integrado do VS Code aberto no projetor** (`Ctrl+'`) — hoje a aula vive nele
- [ ] **Abrir o Agendador de Tarefas antes da aula** (`Win+R` → `taskschd.msc`) e deixar numa aba — abrir ao vivo demora e assusta
- [ ] Criar na sua máquina a pasta `C:\Users\<você>\scripts\organizador` — vai ser o exemplo projetado
- [ ] Ter o caminho do seu `python.exe` colado num bloco de notas (`where python` no terminal). Você vai precisar dele duas vezes
- [ ] **Avisar quem estiver no Colab:** hoje não dá. Não existe terminal nem agendador no Colab — é a primeira aula que **exige** máquina local

---

## Roteiro

### 10:00 — 10:10 | A dívida: "só roda quando eu aperto o play" (10 min)

**Recolha a tarefa (4 min).** Pergunte quem conseguiu o buscador de CEP tratando os três casos, e
quem bateu em `erro: true`. Depois faça a ponte que abre a aula:

> "O buscador de CEP de vocês é útil. Mas ele só é útil **quando vocês estão sentados na frente
> dele**. Vocês abrem o VS Code, aperta o play, ele pergunta, vocês digitam. Isso não é automação —
> isso é uma ferramenta manual que vocês escreveram."

**A dívida (6 min).** Escreva no quadro as duas colunas e deixe a aula inteira:

| O que vocês têm hoje | O que é automação |
|---|---|
| Você abre | Abre sozinho |
| Você aperta o play | Roda no horário |
| Você olha a resposta na tela | Deixa registro de que rodou |
| Se der erro, você vê | Se der erro, **alguém precisa ficar sabendo** |

> "Hoje a gente atravessa essa tabela da esquerda para a direita. E eu vou avisar de uma vez: a
> coluna da direita é um **mundo diferente**. Programa que roda com você olhando perdoa quase tudo.
> Programa que roda sozinho, às três da manhã, não perdoa nada."

E a frase da aula, dita já aqui:

> "**Script agendado erra sozinho, de madrugada, sem ninguém olhando.** Guardem essa. Metade da
> aula de hoje é sobre consertar isso antes de acontecer."

---

### 10:10 — 10:24 | Notebook não é programa (14 min)

**Comece pela pergunta,** não pela explicação: *"por que eu não consigo agendar um notebook?"*

Deixe tentarem. A resposta boa é desconfortável:

> "Porque notebook **não tem começo nem fim**. Vocês rodam a célula 3, voltam na 1, rodam a 7, mudam
> a 3 e rodam de novo. O resultado depende da **ordem em que vocês apertaram**. Um programa que roda
> sozinho precisa de uma ordem só: a de cima para baixo, uma vez."

**Isso não desmerece o notebook.** Diga explicitamente, senão a turma acha que perdeu 12 aulas:

> "Notebook é bancada de trabalho: você experimenta, olha o dado, erra, corrige. É insubstituível
> para isso — e é por isso que a gente vai continuar usando. O `.py` é o produto. Você **experimenta
> no notebook e entrega no `.py`**."

**Agora façam junto, no projetor.** Crie a pasta e o arquivo:

```
C:\Users\guilherme\scripts\organizador\organizador.py
```

E a primeira versão do organizador — **a ingênua, de propósito**. Digite com eles:

```python
import shutil
from pathlib import Path

PASTA = Path("Downloads_teste")

GRUPOS = {
    "PDFs":      [".pdf"],
    "Imagens":   [".jpg", ".jpeg", ".png"],
    "Planilhas": [".csv", ".xlsx"],
}

def grupo_de(sufixo):
    for nome, extensoes in GRUPOS.items():
        if sufixo.lower() in extensoes:
            return nome
    return "Outros"

movidos = 0
for item in PASTA.iterdir():
    if item.is_dir():
        continue
    destino = PASTA / grupo_de(item.suffix)
    destino.mkdir(exist_ok=True)
    shutil.move(item, destino / item.name)
    movidos += 1

print(f"{movidos} arquivos movidos")
```

Três coisas para apontar, porque são Aula 8 voltando:

- `Path` e `iterdir()` em vez de `os.listdir` — mesma ideia, interface melhor
- `item.suffix` já devolve `.pdf`; não precisa de `split(".")`
- `mkdir(exist_ok=True)` — **"cria se não existir"**. Marque essa no quadro, ela já é idempotência e a gente vai voltar nela em meia hora

**E agora o momento da aula:** rode pelo terminal, não pelo play.

```
cd C:\Users\guilherme\scripts\organizador
python organizador.py
```

```
12 arquivos movidos
```

> "Repare no que aconteceu: **eu não abri nada**. Digitei um comando e o programa rodou, fez o
> trabalho e morreu. Isso é um programa. O agendador sabe fazer exatamente isso — digitar um comando."

Mostre a pasta antes e depois, com número medido:

```
antes:   soltos na raiz: 12   |   arquivos no total: 12
depois:  soltos na raiz: 0    |   arquivos no total: 12
  Imagens/: 3
  Outros/: 3
  PDFs/: 3
  Planilhas/: 3
```

> "Doze entraram, doze saíram, só em pastas diferentes. **Guardem esse número doze.** A gente vai
> voltar nele."

---

### 10:24 — 10:34 | O porteiro: `if __name__ == "__main__"` (10 min)

Chegue pelo acidente, não pela sintaxe. Crie dois arquivos ao vivo:

```python
# saudacao.py
def saudar(nome):
    return f"bom dia, {nome}"

print("TESTANDO:", saudar("Guilherme"))
```

```python
# usa.py
from saudacao import saudar
print(saudar("turma"))
```

```
> python usa.py
TESTANDO: bom dia, Guilherme
bom dia, turma
```

> "Eu só queria **usar** a função. E o teste que eu tinha deixado no fim do arquivo rodou junto. Agora
> imaginem que em vez de um `print` ali tivesse `shutil.move`, ou um `requests.post`. **Importar um
> arquivo executa o arquivo inteiro.**"

A causa, mostrada e não contada:

```python
print("__name__ aqui vale:", __name__)
```

```
> python quem.py
__name__ aqui vale: __main__

> python -c "import quem"
__name__ aqui vale: quem
```

> "O Python coloca uma variável chamada `__name__` em todo arquivo. Se o arquivo foi **chamado
> direto**, vale `__main__`. Se ele foi **importado**, vale o nome do arquivo. É a única diferença."

Daí o porteiro:

```python
if __name__ == "__main__":
    print("TESTANDO:", saudar("Guilherme"))
```

```
> python saudacao.py
TESTANDO: bom dia, Guilherme

> python usa.py
bom dia, turma
```

> "Leia em voz alta como português: *se eu sou o programa principal, faça isto*. Tudo que está
> dentro só roda quando **este** arquivo é o que você chamou."

**A regra prática para o quadro:**

- em cima do arquivo, as **funções** (`def`)
- embaixo, dentro do `if __name__ == "__main__"`, o que **acontece**

> "Vocês vão ver isso em 100% dos scripts Python que encontrarem na vida. Agora vocês sabem por quê."

**Agora `sys.argv` (4 min).** Chegue pelo problema:

> "O meu organizador tem a pasta escrita dentro do código. Se o João quiser usar, ele tem que
> **editar o meu programa**. Isso está errado."

```python
import sys
print("sys.argv =", sys.argv)
```

```
> python argumentos.py
sys.argv = ['argumentos.py']

> python argumentos.py Downloads_teste --simular
sys.argv = ['argumentos.py', 'Downloads_teste', '--simular']
```

> "É uma **lista de strings** — a Aula 2 inteira serve aqui. A posição 0 é sempre o nome do arquivo.
> O que você digitou depois vem a partir da posição 1."

> "Reparem: `--simular` entrou na lista como uma string qualquer. O Python não sabe o que é opção e
> o que é pasta — **isso é você que decide**. E a gente vai decidir, na mão na massa."

---

### 10:34 — 10:50 | 💥 nº 1 — a pergunta da aula: "e se rodar duas vezes?" (16 min)

**Este é o bloco mais importante do dia.** Avise:

> "Vou mostrar o erro que mais estraga automação de gente iniciante. Não é erro de sintaxe, não dá
> stack trace, não aparece no log. O programa **funciona** — e é exatamente por isso que dói."

**Comece com o caso fácil de ver: o relatório (6 min).**

Um script que acrescenta as vendas do dia num `.csv`. Nada de errado nele:

```python
import csv
from pathlib import Path

AQUI = Path(__file__).parent
vendas = [
    {"loja": "Centro",   "pecas": 41},
    {"loja": "Shopping", "pecas": 58},
    {"loja": "Online",   "pecas": 33},
]

with open(AQUI / "relatorio.csv", "a", newline="", encoding="utf-8") as f:
    w = csv.writer(f)
    for v in vendas:
        w.writerow([v["loja"], v["pecas"]])

print("relatorio.csv atualizado")
```

Rode uma vez. Mostre o arquivo. Rode **de novo**:

```
> python relatorio.py
relatorio.csv atualizado
> python relatorio.py
relatorio.csv atualizado
```

```
loja,pecas
Centro,41
Shopping,58
Online,33
Centro,41
Shopping,58
Online,33
```

E agora **meça**, com o pandas da Aula 10, porque número convence e texto não:

```python
import pandas as pd
df = pd.read_csv("relatorio.csv")
print("linhas:", len(df))
print("total de pecas:", df["pecas"].sum())
```

```
linhas: 6
total de pecas: 264
```

> "O total verdadeiro é **132**. O relatório diz **264**. Nenhum erro, nenhum aviso, nenhuma linha
> vermelha. Só um número errado, com cara de certo."

Amarre com a Aula 10, que é a mesma lição em outra roupa:

> "Sábado da Aula 10 a gente viu dado sujo derrubar um modelo de 100% para 25% **sem dar erro**. Aqui
> é a mesma família de problema: o programa não reclama, o resultado mente. A diferença é que esse
> aqui **você** causou, e você pode consertar."

**Agora o caso difícil de ver — e o mais perigoso (10 min).**

> "Vocês vão dizer: tá, mas o meu organizador não acrescenta nada em arquivo nenhum. Ele só move.
> Vamos rodar duas vezes então."

```
> python organizador.py
12 arquivos movidos
> python organizador.py
0 arquivos movidos
```

```
soltos na raiz: 0   |   arquivos no total: 12
```

Deixe a turma respirar aliviada. Espere uns segundos. Então:

> "Zero movidos, doze arquivos, nada aconteceu. Parece seguro, né? **Não está seguro. Está com
> sorte.** Ele não duplicou porque na segunda vez não sobrou nada solto na raiz para mover. Foi o
> acaso que salvou, não o código."

**E agora o cenário real.** Você baixou outra nota fiscal, com o mesmo nome de uma que já foi
arquivada — coisa que acontece toda semana com `nota-fiscal.pdf`, `boleto.pdf`, `extrato.pdf`:

```
soltos na raiz: 1   |   arquivos no total: 13
```

```
> python organizador.py
1 arquivos movidos
```

```
soltos na raiz: 0   |   arquivos no total: 12
```

Pare. Projete os dois números lado a lado e **não fale por dez segundos**.

> "**Treze arquivos entraram. Doze saíram.** Um arquivo meu deixou de existir. Não foi para a
> lixeira, não deu erro, não tem log. O `shutil.move` escreveu em cima do que já estava lá e ficou
> quieto."

> "Esse é o tipo de bug que você descobre três meses depois, quando precisa da nota antiga."

**Agora extraia a lição, e esta é a aula:**

> "Antes de agendar qualquer coisa, você faz **uma** pergunta: *o que acontece se isto rodar duas
> vezes?* E agendamento **vai** rodar duas vezes. Você testa na mão, o agendador roda no horário,
> a máquina hiberna e ele roda ao acordar, você clica em 'Executar' para conferir. Duas vezes é o
> normal, não a exceção."

Escreva no quadro a palavra e as duas saídas:

> **IDEMPOTENTE** — rodar uma vez ou dez vezes dá o mesmo resultado.
>
> 1. **Verificar antes de agir** — "já existe? já foi feito? então não faço"
> 2. **Marcar o que já foi feito** — uma lista, uma coluna de data, um nome de arquivo

> "O nome é feio e vocês vão ouvir em entrevista de emprego. Mas a ideia é a coisa mais simples do
> mundo: **o botão do elevador**. Apertar dez vezes é igual a apertar uma. Interruptor de luz não é
> idempotente — apertar duas vezes desliga."

**Conserte o relatório ao vivo, com a estratégia 1.** Pouco código, efeito grande:

```python
DIA = "2026-11-07"

ja_tem = set()
if ARQUIVO.exists():
    with open(ARQUIVO, newline="", encoding="utf-8") as f:
        for linha in csv.DictReader(f):
            ja_tem.add(linha["dia"])

if DIA in ja_tem:
    print(f"{DIA} já estava no relatório — não fiz nada")
else:
    ...  # escreve
```

Rode **três** vezes:

```
--- rodada 1 ---
2026-11-07 acrescentado — 3 linhas
--- rodada 2 ---
2026-11-07 já estava no relatório — não fiz nada
--- rodada 3 ---
2026-11-07 já estava no relatório — não fiz nada
```

```
linhas: 3 | total de pecas: 132
```

> "Três rodadas, três linhas, 132 peças. **Agora pode agendar.**"

> "E repare no `set` ali: é o conjunto da Aula 2. E o `DictReader` é o da Aula 8. Não tem nada novo
> nesse conserto — o que é novo é a **pergunta** que me fez escrever ele."

---

### 10:50 — 11:00 | ☕ Pausa (10 min)

---

### 11:00 — 11:10 | 💥 nº 2 — funciona no terminal, falha no agendador (10 min)

Abra com a cena, que é universal:

> "Você escreve o script, roda no terminal, funciona. Agenda. Na manhã seguinte: não fez nada, ou
> fez em algum lugar que você não encontra. Você roda na mão de novo e **funciona**. Isso enlouquece
> as pessoas."

A causa tem nome: **diretório de trabalho**.

> "Todo programa que roda tem uma pasta onde ele 'está'. Quando vocês digitam `python
> organizador.py`, essa pasta é a que vocês estavam no terminal. Quando o **agendador** roda,
> não é — ele escolhe outra, e a escolha não é óbvia."

Mostre, não conte. Dois arquivos:

```python
import os
from pathlib import Path

print("diretorio de trabalho :", os.getcwd())
print("onde o script mora    :", Path(__file__).parent)
```

Rodando de dentro da pasta do script:

```
diretorio de trabalho : /home/guilherme/scripts/organizador
onde o script mora    : /home/guilherme/scripts/organizador
```

Rodando de outro lugar — **exatamente o que o agendador faz**:

```
diretorio de trabalho : /
onde o script mora    : /home/guilherme/scripts/organizador
```

> "Os dois eram iguais e agora não são. E o `Path(\"Downloads_teste\")` do meu organizador é um
> caminho **relativo**: ele significa 'a partir do diretório de trabalho'. Se o diretório de trabalho
> mudou, o caminho aponta para outro lugar."

Rode o organizador de outra pasta e leia o erro com eles:

```
Traceback (most recent call last):
  File "/home/guilherme/scripts/organizador/organizador_v1.py", line 19, in <module>
    for item in PASTA.iterdir():
  File "/usr/lib/python3.11/pathlib.py", line 931, in iterdir
    for name in os.listdir(self):
                ^^^^^^^^^^^^^^^^
FileNotFoundError: [Errno 2] No such file or directory: 'Downloads_teste'
```

> "`FileNotFoundError` — Aula 9, vocês já leram esse. E olhem a última linha: ele mostra o nome
> **sem caminho nenhum**, `'Downloads_teste'`. Quando o erro de arquivo não tem caminho na frente,
> desconfie de caminho relativo na hora."

**A solução, e é uma linha.** Deixe no quadro:

```python
from pathlib import Path

AQUI = Path(__file__).parent          # onde o SCRIPT mora
ARQUIVO = AQUI / "relatorio.csv"      # não depende de onde você chamou
```

> "`__file__` é o caminho do próprio arquivo `.py`. `Path(__file__).parent` é a pasta dele. A partir
> daí tudo é absoluto, e **não importa de onde você chamou**."

Duas notas que valem o minuto:

> "Primeira: isso **não funciona em notebook**. No Colab e no Jupyter não existe `__file__`, e você
> ganha um `NameError`. É mais uma coisa que separa bancada de produto."

> "Segunda: a pasta que o usuário passa — essa aí vocês não controlam. Para ela, use
> `Path(caminho).expanduser().resolve()`. O `expanduser()` troca o `~` pela pasta do usuário e o
> `resolve()` deixa o caminho absoluto. Duas chamadas e você nunca mais pensa nisso."

**Regra para o quadro, em três linhas:**

- caminho **de arquivo do próprio projeto** → `Path(__file__).parent / "..."`
- caminho que **o usuário passou** → `.expanduser().resolve()`
- caminho relativo em script agendado → **nunca**

---

### 11:10 — 11:20 | 💥 nº 3 — morrer calado (10 min)

> "Terceiro erro. O script roda às 3h, dá pau no terceiro arquivo, e morre. Quem ficou sabendo?
> **Ninguém.** Seu `print` foi para um terminal que não existe, e o agendador anotou 'terminou com
> erro' numa tela que você não abre nunca."

Pergunte: *"vocês já viram o `print` de vocês ir para o lixo?"* Mostre:

```
> python organizador.py > nul
```

> "Nada apareceu. O `print` escreve na tela, e se não tem tela, ele escreve no nada. Quando o
> agendador roda a tarefa sem janela, é isso que acontece com **todos** os seus `print`."

**A troca.** Projete lado a lado e seja explícito que a estrutura é a mesma:

| `print` | `logging` |
|---|---|
| vai para a tela | vai para **arquivo** |
| sem hora | com **data e hora** |
| tudo igual | tem **nível**: INFO, WARNING, ERROR |
| você apaga depois | **fica** |

```python
import logging
from pathlib import Path

AQUI = Path(__file__).parent

logging.basicConfig(
    filename=AQUI / "organizador.log",
    level=logging.INFO,
    format="%(asctime)s | %(levelname)s | %(message)s",
    datefmt="%d/%m/%Y %H:%M:%S",
    encoding="utf-8",
)

logging.info("movido: anotacoes.txt -> Outros/anotacoes.txt")
```

E a linha que sai no arquivo — leia em voz alta, campo por campo:

```
22/10/2026 19:04:25 | INFO | movido: anotacoes.txt -> Outros/anotacoes.txt
```

> "Data, hora, gravidade, mensagem. Quatro campos. É **tudo** que você precisa às oito da manhã de
> uma segunda, quando o cliente diz que faltou arquivo."

> "E repare no `basicConfig`: ele é chamado **uma vez**, no começo do arquivo, e depois é só
> `logging.info(...)` em qualquer lugar. Não precisa passar nada de função em função."

**Agora a parte que amarra com a Aula 9, e diga isso com ênfase:**

> "Na Aula 9 o `try/except` era zelo — ficava bonito, evitava a tela vermelha. Em script agendado
> ele é **requisito**, e por um motivo novo: sem `except`, o programa para no primeiro arquivo
> problemático e os outros onze não são tratados."

```python
try:
    shutil.move(item, destino)
    logging.info(f"movido: {item.name} -> {sub.name}/{destino.name}")
except Exception as e:
    logging.error(f"falhou em {item.name}: {type(e).__name__}: {e}")
```

> "Um arquivo deu problema? Anota e **segue para o próximo**. É a diferença entre 'falhou' e
> 'falhou um de doze'."

**E o melhor comando da aula: `logging.exception`.** Mostre o log de um script que morreu:

```
22/10/2026 19:04:28 | INFO | --- inicio  em /home/guilherme/scripts/organizador/Downloads_do_fulano
22/10/2026 19:04:28 | ERROR | morreu antes de terminar: FileNotFoundError: [Errno 2] No such file or directory: '/home/guilherme/scripts/organizador/Downloads_do_fulano'
Traceback (most recent call last):
  File "/home/guilherme/scripts/organizador/organizador.py", line 90, in <module>
    movidos, iguais, erros = organizar(pasta, simulacao)
                             ^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/guilherme/scripts/organizador/organizador.py", line 45, in organizar
    for item in sorted(pasta.iterdir()):
                ^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.11/pathlib.py", line 931, in iterdir
    for name in os.listdir(self):
                ^^^^^^^^^^^^^^^^
FileNotFoundError: [Errno 2] No such file or directory: '/home/guilherme/scripts/organizador/Downloads_do_fulano'
```

*(as datas do log são do dia em que eu testei — na sua máquina vai sair a de hoje.)*

> "Olhem o que foi para dentro do arquivo: **o stack trace inteiro**. Aquele mesmo que vocês
> aprenderam a ler na Aula 9, agora esperando por vocês de manhã. É `logging.exception(...)` em vez
> de `logging.error(...)` — uma palavra de diferença."

> "A regra é curta: **`logging.exception` dentro do `except` de nível mais alto do seu script.**
> Sempre. Não tem desculpa."

---

### 11:20 — 11:38 | 🛠️ Mão na massa: o organizador de verdade (18 min)

Anuncie o que eles vão ter no fim:

> "Vocês vão sair daqui com um script que: recebe a pasta como argumento, não depende de onde foi
> chamado, escreve log, sobrevive a rodar dez vezes, e tem um **modo simulação** para você testar
> sem medo. Esse script é o Projeto 2 — eu só vou contar isso na Aula 14."

**Dê os quatro requisitos no quadro, não o código:**

1. a pasta vem de `sys.argv` — e se não vier, o programa explica como se usa e sai
2. `AQUI = Path(__file__).parent` para o log
3. `logging` em arquivo, com `try/except` por arquivo e `logging.exception` no fim
4. **idempotente**, com as três decisões decididas por escrito antes de codar

**Pare na decisão nº 4 e faça junto, no quadro.** Esta é a parte que vale os 18 minutos:

> "Para ser idempotente eu preciso responder: **o arquivo já está lá?** E são três respostas
> diferentes, não duas."

| Situação | O que fazer |
|---|---|
| no destino **não existe** nada com esse nome | move, normal |
| existe e é **do mesmo tamanho** | é o mesmo arquivo, já foi feito → **não mexe**, loga |
| existe e é de **outro tamanho** | é outro arquivo com o mesmo nome → move com nome novo, `nota-1.pdf` |

> "Repare que o segundo e o terceiro caso são os dois que o `shutil.move` resolvia do mesmo jeito:
> apagando. Agora um não faz nada e o outro guarda os dois. **Nenhum arquivo meu morre.**"

> "Tamanho é uma checagem fraca — dois arquivos diferentes podem ter o mesmo tamanho. O jeito certo
> é comparar o conteúdo com `hashlib`. Hoje tamanho basta; quando o Projeto 2 estiver de pé, troquem.
> Prefiro vocês com uma checagem simples que entendem a uma complicada que copiaram."

**O código do helper, que é a única parte nova de Python do dia:**

```python
def nome_livre(destino):
    """Acha o próximo nome livre: nota.pdf -> nota-1.pdf -> nota-2.pdf ..."""
    if not destino.exists():
        return destino
    n = 1
    while True:
        tentativa = destino.with_name(f"{destino.stem}-{n}{destino.suffix}")
        if not tentativa.exists():
            return tentativa
        n += 1
```

> "`while True` com `return` dentro — o `while` da Aula 4. `stem` é o nome sem extensão, `suffix` é
> a extensão. E é só isso."

**O modo simulação — dê destaque, pare a turma (5 min).**

> "Agora a parte que eu mais quero que vocês levem desta aula, e não é sintaxe. É um hábito."

```python
if simulacao:
    print(f"[SIMULACAO] moveria  {item.name}  ->  {sub.name}/{destino.name}")
    continue
```

```
> python organizador.py Downloads_teste --simular
[SIMULACAO] moveria  anotacoes.txt  ->  Outros/anotacoes.txt
[SIMULACAO] moveria  boleto-novembro.pdf  ->  PDFs/boleto-novembro.pdf
[SIMULACAO] moveria  canal-6.png  ->  Imagens/canal-6.png
[SIMULACAO] moveria  clientes.csv  ->  Planilhas/clientes.csv
[SIMULACAO] moveria  contrato-guaruja.pdf  ->  PDFs/contrato-guaruja.pdf
[SIMULACAO] moveria  estoque.xlsx  ->  Planilhas/estoque.xlsx
[SIMULACAO] moveria  instalador.exe  ->  Outros/instalador.exe
[SIMULACAO] moveria  musica-do-verao.mp3  ->  Outros/musica-do-verao.mp3
[SIMULACAO] moveria  nota-fiscal-santos.pdf  ->  PDFs/nota-fiscal-santos.pdf
[SIMULACAO] moveria  praia-grande.jpg  ->  Imagens/praia-grande.jpg
[SIMULACAO] moveria  sao-vicente.jpg  ->  Imagens/sao-vicente.jpg
[SIMULACAO] moveria  vendas-outubro.csv  ->  Planilhas/vendas-outubro.csv
movidos: 0 | ja estavam feitos: 0 | erros: 0
```

```
soltos na raiz: 12   |   arquivos no total: 12
```

> "Doze linhas de 'eu faria assim', e **nada mudou na pasta**. Zero movidos. Esse modo tem nome em
> inglês, `dry run`, e é a coisa mais profissional que tem nesta aula. Em ferramenta séria ele
> sempre existe: `git` tem `--dry-run`, o `rsync` tem `-n`, instalador de pacote tem."

> "E o motivo é bem pouco nobre: **ninguém confia em programa que apaga coisa.** Nem você, no seu
> próprio. O modo simulação é o que te dá coragem de apontar o script para a sua pasta de Downloads
> de verdade em vez da de teste."

Regra para o quadro:

> **Script que mexe em arquivo nasce com `--simular`. E o `--simular` é a primeira coisa que você
> roda quando aponta para uma pasta nova.**

**Deixe-os codar (resto do bloco).** Circule. Os três travamentos previsíveis:

- `sys.argv[1]` dando `IndexError` quando rodam sem argumento → é a hora de usar o item 1 do quadro
- esquecer de filtrar `item.is_dir()` e o script tentar mover as subpastas que ele mesmo criou
- `--simular` entrando na lista de caminhos → `[a for a in argumentos if not a.startswith("--")]`

**Quando o primeiro terminar, peça para ele rodar duas vezes e dizer os números em voz alta.**
`movidos: 12` e depois `movidos: 0`. É a prova da aula, e vindo da boca de um aluno vale o dobro.

---

### 11:38 — 11:54 | Agendar: o Agendador de Tarefas do Windows (16 min)

> "Agora a parte que vocês esperaram a aula inteira. E ela é mais chata do que vocês imaginam — o
> difícil não é agendar, é **agendar certo**."

**Primeiro, descubram o caminho do Python.** Isso é Aula 11 voltando:

```
> where python
```

Ou, mais confiável, de dentro do Python:

```python
import sys
print(sys.executable)
```

> "O agendador não tem o PATH de vocês. Vocês não podem escrever só `python` lá — tem que ser o
> caminho inteiro do `python.exe`. Copiem esse caminho para um bloco de notas agora."

**O passo a passo. Projete e vá devagar — eles vão fazer junto:**

1. `Win + R` → digite `taskschd.msc` → Enter
2. No painel da direita, **"Criar Tarefa..."** — *não* use "Criar Tarefa Básica". A básica não tem o campo que importa
3. Aba **Geral**: Nome → `Organizador de Downloads`. Deixe marcado *"Executar somente quando o usuário estiver conectado"*
4. Aba **Disparadores** → **Novo...** → `Diariamente`, hora `22:00` → OK
5. Aba **Ações** → **Novo...** → Ação: `Iniciar um programa`, e os **três campos que decidem tudo**:

| Campo | O que vai |
|---|---|
| **Programa/script** | `C:\Users\guilherme\AppData\Local\Programs\Python\Python312\python.exe` |
| **Adicionar argumentos** | `organizador.py "C:\Users\guilherme\Downloads"` |
| **Iniciar em (opcional)** | `C:\Users\guilherme\scripts\organizador` |

6. OK, OK. A tarefa aparece na lista
7. **Clique com o botão direito nela → "Executar".** Não espere as 22h para descobrir que está errado
8. Abra o `organizador.log` na pasta do script. Se tem linha nova, funcionou

**Pare no campo 3 da Ação e insista, porque é a aula de volta:**

> "**'Iniciar em (opcional)'** diz *opcional* e **não é opcional**. Esse campo é o diretório de
> trabalho. Se vocês deixarem vazio, o Windows escolhe `C:\Windows\System32`, e todo caminho relativo
> do script aponta para lá. É literalmente o erro nº 2 desta aula, com a palavra 'opcional' do lado
> para te enganar."

> "E vocês repararam nas aspas em volta do caminho dos argumentos? É por causa do espaço em
> 'Program Files', em 'Meus Documentos', no nome de vocês. **Caminho do Windows tem espaço; sempre
> ponha aspas.**"

**Como ler o resultado (3 min).** Selecione a tarefa e mostre as colunas:

- **Última Execução** — a hora
- **Resultado da Última Execução** — `0x0` quer dizer que o programa terminou sem erro. Qualquer
  outra coisa quer dizer que terminou com erro
- a aba **Histórico**, que pode estar desligada — tem um botão "Habilitar histórico de todas as tarefas"

> "`0x0` é o sucesso. Mas cuidado com o que `0x0` significa: quer dizer que o **programa terminou**,
> não que fez a coisa certa. É o `status 200` da Aula 12 de novo — sucesso na entrega não é sucesso
> no conteúdo. **Quem conta a verdade é o seu log.**"

**O truque da janelinha preta (1 min).**

> "Se incomodar aquele terminal piscando na tela, troque `python.exe` por `pythonw.exe`, na mesma
> pasta. O `w` é de *windowless*. Mas só depois que o log estiver funcionando — sem janela e sem log
> você fica cego."

**E o `.bat`, para quem quiser o caminho mais robusto:**

```bat
@echo off
cd /d "C:\Users\guilherme\scripts\organizador"
"C:\Users\guilherme\AppData\Local\Programs\Python\Python312\python.exe" organizador.py "C:\Users\guilherme\Downloads"
```

> "Aponta o agendador para o `.bat` e pronto. A vantagem é que o `cd /d` resolve o diretório de
> trabalho de um jeito que você **vê**, num arquivo que está no seu projeto, e não num campo de uma
> janela do Windows que ninguém lembra de conferir."

**O equivalente em Linux e Mac (1 min).** Não ensine, só mostre, porque metade dos tutoriais da
internet está nessa língua:

```
crontab -e
```

```
0 22 * * * /usr/bin/python3 /home/guilherme/scripts/organizador.py /home/guilherme/Downloads
```

> "`0 22 * * *` é 'minuto 0 da hora 22, todo dia'. Mesma ideia, mesma armadilha: o `cron` tem um
> ambiente ainda mais pelado que o do Windows — o diretório de trabalho é a casa do usuário e o PATH
> é quase vazio. É por isso que ali também vai o caminho completo do `python3`."

**O aviso final do bloco, e não deixe de dar:**

> "Vocês acabaram de ganhar a capacidade de fazer o computador mexer nos arquivos de vocês sozinho,
> de madrugada. **Agende primeiro apontando para uma pasta de teste.** Rode com `--simular`. Olhe o
> log. Só depois aponte para a pasta de verdade. Eu já apaguei coisa minha com script meu, e não
> quero companhia."

---

### 11:54 — 12:00 | O que isso destrava + tarefa (6 min)

**O que isso destrava (3 min).** Projete e leia:

> "Com `.py` + argumento + log + agendador, estas coisas saíram do 'um dia eu faço':"
>
> - **relatório que chega pronto** toda segunda, sem você abrir nada
> - **backup** de uma pasta, no horário, com registro do que foi copiado
> - a **cotação da Aula 12** anotada num `.csv` de hora em hora, virando histórico seu
> - **monitor de preço** que avisa quando cai
> - e, do Módulo 4 em diante, o mesmo esqueleto chamando **IA** em cima dos seus arquivos

> "E repare no que mudou de verdade hoje: até ontem o computador de vocês fazia o que vocês pediam.
> De hoje em diante ele faz o que vocês **combinaram**. É outro patamar — e é por isso que a aula
> teve três erros de propósito em vez de um."

**A tarefa (2 min):**

1. **Terminar o organizador** com as quatro exigências: `sys.argv`, `Path(__file__).parent`, `logging` em arquivo e `--simular`
2. **Rodar duas vezes seguidas e provar com número** que nada duplicou e nada desapareceu. Conte os arquivos antes e depois e mande os dois números no grupo
3. **Agendar de verdade** no Agendador de Tarefas, apontando para uma **pasta de teste**, e mandar no grupo o `organizador.log` com as linhas das duas execuções
4. **Pensar** (não codar) na sua automação pessoal: o que você faz na mão toda semana e cabe num script? Traga a ideia escrita — ela vira o Projeto 2 sábado que vem
5. **Desafio:** fazer o script apagar a cópia duplicada quando o arquivo é idêntico ao que já está arquivado. Só aceite se funcionar com `--simular` **primeiro**

**A pergunta que fica (1 min):**

> "Vocês vão passar a semana mexendo nesse script. Vão consertar uma coisa, quebrar outra, voltar
> atrás. E em algum momento — eu garanto — ele vai parar de funcionar e vocês não vão saber **o que
> mudou**."

> "Aí vem o `organizador_v2.py`, o `organizador_FINAL.py`, o `organizador_FINAL_agora_vai.py`. Todo
> mundo já fez isso. Sábado que vem a gente resolve isso de verdade — e de graça."

*(Aula 14: Git e GitHub, e o Projeto 2 nasce lá.)*

---

## Se sobrar tempo

- `hashlib.md5` comparando conteúdo em vez de tamanho — é a versão certa da checagem do bloco das 11:20
- `logging` com dois destinos ao mesmo tempo (arquivo **e** tela) com `logging.StreamHandler()`
- `argparse` no lugar do `sys.argv` na mão. **Mostre, não ensine:** três linhas e você ganha `--help` de graça. Serve para plantar a vontade
- Agendar de hora em hora e deixar rodando durante a mão na massa, para eles verem o log crescer sozinho

## Se faltar tempo

Corte nesta ordem: (1) o `.bat` e o `cron`, (2) o `pythonw.exe` e a janelinha, (3) o bloco do
`sys.argv` — deixe a pasta escrita no código e **diga** que na tarefa é argumento, (4) a demonstração
do `print > nul`, (5) a leitura do stack trace dentro do log — mas **mostre o arquivo de log aberto**,
mesmo sem ler linha por linha.

**Nunca corte:** o 💥 nº 1 inteiro (o relatório com 264 contra 132 **e** os treze arquivos virando
doze), a pergunta *"o que acontece se isto rodar duas vezes?"* escrita no quadro, o campo
**"Iniciar em (opcional)"** do Agendador, e o **modo simulação**. Se der tempo só para isso, a aula
aconteceu.

---

## Depois da aula — 15 minutos

- [ ] Mandar no grupo: link do notebook, a tarefa escrita e **o print do Agendador de Tarefas com os três campos preenchidos** (eles vão errar esse campo durante a semana)
- [ ] Mandar também o `organizador.py` completo, comentado, **como referência** — e dizer que é referência, não resposta: quem copiar sem digitar não vai conseguir mexer nele na Aula 14
- [ ] Anotar em `duvidas-da-turma.md` o que te pegou, e em especial **quem não tem permissão de criar tarefa na máquina** (acontece em notebook de trabalho) — precisa de plano B na Aula 14
- [ ] **Ler as ideias de automação pessoal da tarefa 4.** É delas que sai o Projeto 2; se alguém trouxer ideia grande demais, corte antes do sábado, não durante
- [ ] **Preparar a Aula 14:** criar um repositório de teste vazio no GitHub e confirmar que o `git` está instalado nas máquinas da turma. Se alguém não tiver, resolva por mensagem durante a semana — instalar git ao vivo come 20 minutos
- [ ] **Nota Unipds:** escrever para si mesmo, sem jargão, *qual é a diferença entre uma operação idempotente e uma operação segura para repetir por acaso* — e por que fila de mensagens e `PUT` de HTTP vivem falando disso
