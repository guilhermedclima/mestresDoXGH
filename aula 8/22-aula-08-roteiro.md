# Aula 8 — Roteiro minuto a minuto

**Sábado, 03/10/2026 — 10h00 às 12h00**
**Tema:** Arquivos — o dado que sobrevive · **instalação local**
**Ambiente:** Google Colab na primeira hora, **máquina do aluno** na segunda
**Turma:** até 6 alunos

---

## ⚠️ Esta aula é diferente das outras

É a única com **instalação de software**, e instalação em máquina alheia é imprevisível: antivírus,
falta de permissão de administrador, disco cheio, Windows desatualizado. **Conte com isso.**

Três decisões tomadas de antemão para o dia não descarrilar:

1. **O Colab continua valendo.** Ninguém sai daqui sem ambiente. Se a instalação de alguém falhar,
   essa pessoa segue no Colab e a gente resolve durante a semana. Diga isso em voz alta no começo —
   tira a ansiedade da sala.
2. **A instalação vem depois do conteúdo**, não antes. Se atrasar, atrasa a pílula, não a matéria.
3. **O guia (`25-guia-instalacao-local.md`) é autossuficiente.** Quem não terminar em aula termina
   em casa sem depender de você.

---

## Objetivo da aula

Ao final, cada aluno deve:

1. Escrever e ler arquivo de texto com `with open(...)`
2. Saber que **`"w"` apaga o arquivo** e que `"a"` acrescenta
3. Salvar e carregar JSON e CSV — e ver a agenda da Aula 7 sobreviver ao fechamento
4. Entender que **tudo que sai de arquivo é texto**
5. Ter Python e VS Code rodando na própria máquina *(ou o guia para terminar em casa)*
6. Ter visto que o dataset de um modelo é um arquivo — e o que acontece quando ele é pequeno ou errado

**A dívida que abre a aula:** *"você fechou o Colab e tudo sumiu. Como o dado sobrevive?"*

| Gancho | De onde veio | Onde é pago |
|---|---|---|
| O dado que sumiu ao fechar o Colab | Fim da Aula 7 | Abertura, 10h00 |
| A agenda de contatos | Tarefa da Aula 7 | 10h45 — agora com endereço fixo |

> **A frase que amarra a aula:**
> **memória morre quando o programa fecha; arquivo não.**

---

## Antes de começar

- [ ] Notebook testado e link curto no quadro (`24-aula-08-notebook.ipynb`)
- [ ] Slides abertos (`23-aula-08-slides.html`)
- [ ] **Guia de instalação mandado no grupo na sexta**, com o pedido de já baixar os instaladores
- [ ] **Saber quem tem Windows e quem tem Mac** — e se alguém não é administrador da própria máquina
- [ ] Ter os dois instaladores num pendrive, para o caso de internet ruim
- [ ] Testar você mesmo o passo a passo do guia numa máquina limpa, se conseguir
- [ ] Levar extensão/filtro de tomada. Sério: seis notebooks, duas tomadas

---

## Roteiro

### 10:00 — 10:08 | A dívida: o dado que sumiu (8 min)

Recolha a tarefa rapidamente — hoje o tempo é curto. Pergunte quem fez o `contar_palavras` e
dê o nome dele:

> "Isso que vocês fizeram tem nome: **bag of words**, saco de palavras. É literalmente o que o
> detector de spam da Aula 3 faz por dentro."

**Agora a dívida (5 min).** Sem slide:

> "Semana passada eu mandei vocês fecharem o Colab e abrirem de novo. Sumiu tudo. Porque tudo que
> a gente fez em sete aulas viveu na **memória** — e memória morre quando o programa fecha."

> "A saída é o **arquivo**: um pedaço de dado que fica no disco depois que tudo acaba."

---

### 10:08 — 10:30 | Escrever e ler (22 min)

```python
with open("notas.txt", "w") as f:
    f.write("Ana 8.5\n")
    f.write("Bruno 4.0\n")
    f.write("Carla 9.5\n")
```

Destrinche a linha no quadro:

```
with open("notas.txt", "w") as f:
 │     │        │        │     └── apelido do arquivo aberto
 │     │        │        └──────── MODO: "w" = write
 │     │        └───────────────── nome do arquivo
 │     └────────────────────────── abre
 └──────────────────────────────── e FECHA sozinho no fim do bloco
```

Venda o `with`, porque ele parece enfeite e não é:

> "Sem o `with`, vocês teriam que lembrar de fechar o arquivo. Esquecer disso é uma das causas
> clássicas de arquivo corrompido. **O `with` fecha por vocês — inclusive se der erro no meio do
> caminho.**"

**No Colab, mostre o arquivo aparecendo** no ícone de pasta na lateral. Vale muito: eles veem o
arquivo existindo. E aproveite:

> "Repare onde ele foi parar: numa máquina do Google, que some quando a sessão fecha. Guardem
> esse detalhe — é metade do motivo da segunda parte da aula de hoje."

**Ler (8 min):**

```python
with open("notas.txt") as f:
    conteudo = f.read()
```

> "Sem o segundo argumento o modo é `r`, de *read*. É o padrão."

Depois, linha por linha, com `repr()` para o `\n` aparecer:

```python
with open("notas.txt") as f:
    for linha in f:
        print(repr(linha))
```

```
'Ana 8.5\n'
'Bruno 4.0\n'
'Carla 9.5\n'
```

> "**Toda linha vem com a quebra grudada no fim.** E aí entra o `.strip()` que vocês aprenderam
> na Aula 5."

```python
nome, nota = linha.strip().split()
print(f"{nome:6s} {float(nota)}")
```

**Pare no `float()` — este é o ponto pedagógico do bloco:**

> "Por que o `float`? Porque **tudo que sai de um arquivo é texto**. Exatamente igual ao `input()`
> da Aula 2. O arquivo não sabe o que é número. Quem sabe é vocês."

---

### 10:30 — 10:45 | Os três erros que apagam trabalho (15 min)

**💥 Quebrando de propósito nº 1 — e este é o mais importante do dia.**

> "O arquivo tem três nomes. Vou acrescentar um quarto."

```python
with open("notas.txt", "w") as f:
    f.write("Diego 6.0\n")
```

```
Diego 6.0
```

Deixe a sala perceber sozinha.

> "**Os três sumiram.** Sem aviso, sem erro, sem confirmação. `w` não quer dizer 'escrever no
> fim'. Quer dizer **'zerar o arquivo e escrever'**."

```python
with open("notas.txt", "a") as f:      # append, o mesmo nome da Aula 5
    f.write("Elis 7.5\n")
```

Tabela no slide:

| Modo | O que faz | Se o arquivo existe |
|---|---|---|
| `"r"` | lê | — |
| `"w"` | escreve | **apaga tudo antes** |
| `"a"` | acrescenta no fim | preserva |

> "Guardem: **`w` é a faca do dia.** Quase todo iniciante perde trabalho com ela pelo menos uma
> vez. Na dúvida, usem `a`."

**💥 Quebrando de propósito nº 2 — e ele planta a Aula 9:**

```python
with open("nao_existe.txt") as f:
    print(f.read())
```

```
FileNotFoundError: [Errno 2] No such file or directory
```

> "Esse erro é **inevitável** no mundo real: arquivo movido, renomeado, apagado, ou o programa
> rodando de outra pasta."

> "E aqui a gente esbarra num problema que vem se acumulando desde a Aula 2: **como é que o
> programa continua vivo quando algo dá errado?** Semana que vem. E é a aula em que eu finalmente
> pago a dívida do `abc`."

**💥 Quebrando de propósito nº 3 — rápido (2 min):** esquecer o `\n` e sair `'AnaBrunoCarla'`.
Nenhum erro, só um arquivo inútil.

---

### 10:45 — 11:00 | JSON e CSV em arquivo (15 min)

**JSON (7 min).** Aqui as duas últimas aulas se encontram — e é o momento de fechar a promessa:

```python
with open("agenda.json", "w", encoding="utf-8") as f:
    json.dump(agenda, f, indent=2, ensure_ascii=False)

with open("agenda.json", encoding="utf-8") as f:
    de_volta = json.load(f)
```

> "A agenda de vocês agora tem endereço fixo."

**Alerte para a pegadinha do `s`, porque ela vai acontecer:**

```
dumps / loads  →  mexem com STRING   (o s é de string)
dump  / load   →  mexem com ARQUIVO
```

> "Quando der erro estranho em JSON, chequem o `s` antes de qualquer outra coisa."

**CSV (8 min).**

```python
with open("turma.csv", encoding="utf-8") as f:
    for linha in csv.DictReader(f):
        print(linha)
```

```
{'nome': 'Ana', 'nota': '8.5', 'faltas': '2'}
```

> "O `DictReader` transforma **cada linha num dicionário**, usando o cabeçalho como chave. É a
> aula passada caindo do céu."

E aponte as aspas no `'8.5'`:

> "Está entre aspas. **É texto.** Mesmo parecendo número. É o mesmo problema do `\"2\" + \"2\"` que
> abriu a Aula 2 — ele volta em toda aula em que o dado vem de fora."

---

### 11:00 — 11:10 | Pausa (10 min)

*Use a pausa para começar a instalação de quem tiver máquina mais lenta.*

---

### 11:10 — 11:42 | 💻 Instalação local (32 min)

**Abra dizendo por que (2 min).** Isso não é burocracia, é consequência do que acabaram de ver:

> "O arquivo que vocês criaram hoje está numa máquina do Google e some quando a sessão fecha.
> Para o dado sobreviver de verdade, o programa tem que rodar **na máquina de vocês**. É isso que
> a gente vai fazer agora."

E tire a pressão da sala imediatamente:

> "Instalação em máquina alheia dá errado. É normal. Se a sua não for hoje, você continua no
> Colab e a gente resolve durante a semana. **Ninguém vai ficar sem ambiente.**"

**Siga o guia (`25-guia-instalacao-local.md`) projetado na tela**, passo a passo, todo mundo junto.
Não improvise a ordem — o guia foi escrito para ser seguido literalmente.

Marcos, nesta ordem:

- [ ] **Python instalado** — no Windows, o *"Add Python to PATH"* é o passo que todo mundo erra. Pare a turma inteira nessa tela e confirme um por um.
- [ ] `python --version` respondendo no terminal
- [ ] **VS Code instalado** e a extensão **Python** (Microsoft) instalada
- [ ] Uma pasta `mestres-do-xgh` criada e aberta no VS Code
- [ ] `ola.py` com um `print` rodando com o ▶

**Como conduzir os 32 minutos:**

- Vá por marcos, não por relógio: **ninguém avança sozinho.** Quem terminar antes ajuda o vizinho — isso funciona melhor que você correndo entre seis mesas.
- Quando alguém travar, **não pegue o teclado.** Pergunte o que está escrito na tela e deixe a pessoa ler o erro em voz alta. É a Aula 9 começando cedo.
- Se aos **11h35** ainda tiver gente longe, pare, anuncie que o resto vai por guia, e siga para a pílula. Não deixe a instalação comer a pílula inteira.

**O fechamento do bloco vale a pena (2 min).** Peça para todo mundo rodar o `ola.py` ao mesmo tempo:

> "Esse `print` é o mesmo da Aula 1. Só que agora ele não está no navegador de vocês. Está na
> **máquina** de vocês. O arquivo continua lá amanhã, e depois de amanhã."

---

### 11:42 — 11:56 | 💊 Pílula de IA: sem dado guardado não existe modelo (14 min)

**Etapa 1 — o dataset é um arquivo (4 min)**

> "Todo modelo que vocês viram neste curso saiu de dados que alguém digitou. O detector de spam
> da Aula 3 nasceu de 16 mensagens escritas na mão **dentro do código**. Isso não é como funciona
> de verdade."

Rode a célula que grava o `spam.csv` e depois treina lendo o arquivo:

```
texto,rotulo
promocao imperdivel clique aqui agora,spam
voce ganhou um premio gratis resgate ja,spam
...
```

```python
modelo.predict(...)   # ['spam']  /  ['normal']
```

> "Repare no que mudou de verdade: **para melhorar esse modelo você edita um arquivo, não um
> programa.**"

> "É a promessa da Aula 3 virando concreta. Lá eu disse 'você corrige o modelo dando mais
> exemplo'. Agora dá para ver **onde** os exemplos moram: numa planilha que qualquer pessoa abre,
> inclusive quem não sabe programar."

**Etapa 2 — quanto dado é preciso? (5 min)**

Explique o método antes do número, porque o método é conteúdo:

> "Teste separado de 50 mensagens que o modelo nunca viu, e **média de 30 sorteios diferentes**
> para não depender da sorte. Uma medição só é anedota."

```
   4 exemplos ->  77.8% de acerto
   8 exemplos ->  94.6% de acerto
  16 exemplos ->  99.9% de acerto
  32 exemplos -> 100.0% de acerto
  64 exemplos -> 100.0% de acerto
 128 exemplos -> 100.0% de acerto
```

Duas leituras, dê as duas:

1. **No começo, cada exemplo vale ouro.** De 4 para 16, salta de 78% para 100%.
2. **Depois, satura.** De 32 para 128 você quadruplicou o dado e ganhou **zero**.

**Seja honesto sobre o conjunto** — isso ensina ceticismo e é o estilo do curso:

> "Esse conjunto é fácil de propósito, montado por combinação de pedaços. Por isso chega a 100%.
> Num problema real o número seria bem menor. O que importa aqui é o **formato da curva** — e ele
> é sempre esse: sobe rápido, depois empaca."

> "Isso explica uma frase que vocês vão ouvir muito: *'a gente precisa de mais dados'*. Às vezes é
> verdade. Às vezes o modelo já saturou e o problema é outro."

**Etapa 3 — 💥 e se o dado estiver errado? (4 min)**

> "Mesmos 64 exemplos, mas com rótulos trocados de propósito. É o que acontece quando quem
> rotulou estava cansado."

```
 0 rótulos errados de 64 -> 100.0%
 4 rótulos errados de 64 -> 100.0%
 8 rótulos errados de 64 -> 100.0%
16 rótulos errados de 64 ->  97.7%
24 rótulos errados de 64 ->  85.9%
```

> "Ele **aguenta** um pouco de erro — oito trocados e nem sente. Depois de certo ponto, desaba."

E o ponto que amarra com as duas pílulas anteriores:

> "Repare no que ele **não** faz: não avisa. Não dá erro. Não fica vermelho. Ele só responde pior,
> com a mesma cara de confiança de sempre — igual à extrapolação da Aula 6."

**Feche com as três frases (1 min):**

1. **Sem dado guardado não existe modelo.** E dado guardado é arquivo.
2. **Mais dado ajuda muito no começo e cada vez menos depois.**
3. **Dado errado é pior que dado faltando** — porque não avisa.

> "E é por isso que a Aula 10 é sobre **limpar tabela**. Oitenta por cento do trabalho de IA
> acontece antes do modelo, no arquivo."

> **Nota para você (Unipds):** se perguntarem "então quanto dado eu preciso?", a resposta honesta
> na hora é *"depende de quão parecidos são os exemplos entre si — não existe número mágico"*.
> Depois da aula, escreva para si mesmo o que é uma **curva de aprendizado** e como ela ajuda a
> decidir entre juntar mais dado ou trocar de modelo. É uma decisão de custo que aparece em todo
> projeto real.

---

### 11:56 — 12:00 | Tarefa e a nova dívida (4 min)

**Anuncie primeiro a mudança de ambiente:**

> "Toda a tarefa desta semana é para rodar **na máquina de vocês, no VS Code**. Não no Colab. Se a
> instalação não terminou, o guia está no material e o grupo está aberto a semana inteira."

1. **Agenda que sobrevive:** carrega o `agenda.json` se existir, deixa acrescentar, salva de volta. Feche e abra de novo para confirmar
2. **Diário de bordo:** pergunta "o que você aprendeu hoje?" e **acrescenta** no `diario.txt` com a data — modo `"a"`
3. **Boletim em CSV:** um programa salva, outro lê. **Dois programas, um arquivo** — foi exatamente o que o modelo de spam fez
4. **Desafio:** fazer a tarefa 1 funcionar quando o `agenda.json` **não existe** ainda

**A pergunta que fica:**

> "No desafio vocês vão escrever algo como *'se o arquivo existir, abre'*. Mas e se ele existir e
> estiver **corrompido**? E se o usuário digitar `abc` onde vocês esperam número — aquela pergunta
> da **Aula 2** que eu ainda não respondi direito? E se o CSV tiver uma linha faltando coluna?"

> "É tudo o mesmo problema: **o programa morre no primeiro imprevisto.**"

*(A resposta é a Aula 9. E é lá que eu finalmente pago a dívida do `abc`, aberta desde a segunda
aula.)*

**Fechamento:**

> "Hoje vocês pararam de perder trabalho. O que vocês escreverem daqui para frente fica. Semana
> que vem a gente aprende a não deixar o programa morrer. Até sábado."

---

## Se sobrar tempo

*(É improvável nesta aula. Mas se sobrar:)*

- **`os.path.exists()`** — a checagem manual antes de abrir. Serve de contraste perfeito com o `try/except` da semana que vem
- Mostre o `diario.txt` crescendo a cada execução. Ver o arquivo engordar é mais convincente que qualquer explicação
- **Ler um CSV de verdade:** baixe qualquer planilha pública e abra com `DictReader`. Já prepara a Aula 10

## Se faltar tempo

Corte nesta ordem: (1) o terceiro "quebrar de propósito" (o `\n` esquecido), (2) o bloco de CSV
inteiro — ele volta na Aula 10 com força, (3) a Etapa 2 da pílula (a curva).

**Nunca corte:** o `"w"` apagando o arquivo, o `FileNotFoundError` (é o gancho da Aula 9), o
`json.dump`/`json.load` com a agenda sobrevivendo, e as Etapas 1 e 3 da pílula — "o dataset é um
arquivo" e "dado errado envenena em silêncio".

> **A regra de decisão do dia:** se a instalação atrasar, ela come a pílula — nunca o contrário. É
> a única aula do curso em que isso vale, porque sem ambiente instalado as próximas cinco aulas
> ficam mancas.

---

## Depois da aula — 20 minutos

*(Hoje são 20, não 15. Instalação sempre deixa rabo.)*

- [ ] Mandar no grupo: link do notebook, a tarefa escrita e **o guia de instalação de novo**
- [ ] **Listar quem ficou sem ambiente** e mandar mensagem individual. Não espere a pessoa pedir ajuda — quem não conseguiu instalar tende a sumir
- [ ] Anotar em `duvidas-da-turma.md` o que te pegou, **e também os erros de instalação** que apareceram: eles se repetem todo semestre e viram material
- [ ] Escrever para si mesmo, sem jargão: *o que é uma curva de aprendizado e como ela ajuda a decidir entre mais dado ou outro modelo?*
- [ ] **Preparar a Aula 9 com carinho:** é a aula que paga a dívida mais antiga do curso (o `abc`, da Aula 2). Vale abrir mostrando a linha do tempo dessa promessa
