# Aula 14 — Roteiro minuto a minuto

**Sábado, 14/11/2026 — 10h00 às 12h00**
**Tema:** git e GitHub — a máquina do tempo do código · **abre o Projeto 2**
**Ambiente:** VS Code na máquina do aluno + navegador no GitHub
**Turma:** até 6 alunos

---

## Objetivo da aula

Ao final, cada aluno deve:

1. Saber **que problema o git resolve** — e por que `projeto_final_v2_AGORA_VAI.py` é o sintoma
2. Rodar sozinho o ciclo `status` → `diff` → `add` → `commit` → `log`
3. Ler um `git diff` e dizer o que entrou e o que saiu
4. Escrever um `.gitignore` que cobre `__pycache__/`, `.venv/`, log e **segredo**
5. Entender, com a saída na tela, que **o git lembra para sempre** — e o que fazer quando uma chave vaza
6. Escrever mensagem de commit que responde **por quê**
7. Ter **um repositório público no GitHub**, com `README.md` e `requirements.txt`, e saber dar `push`

**A dívida que abre a aula:** *"mudei alguma coisa e não sei o quê."*

| Gancho | De onde veio | Onde é pago |
|---|---|---|
| *"mudei alguma coisa e não sei o quê"* | Fim da Aula 13 | Abertura, 10h00 — a pasta do horror projetada |
| *"rodei duas vezes e duplicou tudo"* | Fim da Aula 13 | 11:50 — critério 5 do Projeto 2 |
| `requirements.txt` | Aula 11 | 11:30 — clonar e não rodar |
| `venv` ("vai aparecer no Projeto 2") | Aula 11 | 10:45 — `.gitignore` |
| `try/except` e log em arquivo | Aula 9 e 13 | 10:45 — `*.log` não vai para o git |
| A automação da semana passada | Aula 13 | É **ela** que vai para o ar no Projeto 2 |
| Chave de API | Aula 12 | 11:05 — e é a **semente da Aula 18** |

> **A frase que amarra a aula:**
> **commit é um ponto de retorno; o resto é coragem.**

---

## Antes de começar

- [ ] Notebook testado (`43-aula-14-notebook.ipynb`) e link curto no quadro
- [ ] Slides abertos (`42-aula-14-slides.html`)
- [ ] **`git --version` na máquina de cada aluno.** Esta é a checagem mais importante da aula — quem
      não tem git instalado não faz nada. Instalador: `git-scm.com`. **No Windows, depois de instalar,
      fechar e reabrir o VS Code**, senão o terminal não enxerga o comando
- [ ] **A turma já tem conta no GitHub? Não assuma.** Pergunte no grupo na quinta-feira:
      *"quem já tem conta no github.com? responde sim ou não, é rápido"*. Quem não tiver, o caminho
      curto: `github.com` → **Sign up** → e-mail, senha, usuário → confirma o código que chega no
      e-mail → pronto, 3 minutos, **não pede cartão**. Se alguém chegar sem conta, não pare a aula:
      mande criar durante a pausa e siga
- [ ] **Token pronto antes da aula**, o seu. Gere um na sexta e deixe num arquivo de texto fora do
      projeto, para colar no `push` ao vivo sem procurar na hora
- [ ] Um repositório vazio **seu** criado e ainda sem push, para fazer o `remote add` + `push` ao vivo
- [ ] Testar o `push` na rede da sala, na sexta. Rede de coworking às vezes bloqueia porta — se
      bloquear, o plano B é HTTPS pelo navegador com a extensão GitHub do VS Code
- [ ] Recolher a tarefa da Aula 13: quem conseguiu agendar o script?

---

## Roteiro

### 10:00 — 10:12 | A dívida: a pasta do horror (12 min)

**Recolha a tarefa (4 min).** Pergunte quem conseguiu **agendar** o script e quem viu o log de uma
execução que rodou sem ninguém olhando. Pergunte também, de propósito: *"alguém mexeu no script
depois que ele estava funcionando, quebrou, e não conseguiu voltar?"* Quase sempre tem um. Essa
pessoa é a melhor abertura possível — você vai resolver o problema dela em dez minutos.

**Agora a dívida (8 min).** Projete o slide da pasta do horror. É esta lista, e é a saída real da
célula 0 do notebook:

```
organiza.py
organiza_FINAL.py
organiza_FINAL_agora_vai (1).py
organiza_FINAL_agora_vai.py
organiza_backup_ontem.py
organiza_v2.py
organiza_v2_ok.py
```

Deixe a turma rir. Depois faça as três perguntas, uma por uma, esperando resposta:

> "Qual desses é o mais novo?"
> "O que mudou entre o `organiza_v2.py` e o `organiza_v2_ok.py`?"
> "E **por que** você fez essa mudança?"

> "Vocês não sabem responder nenhuma das três. E o arquivo é **seu**, de **ontem**."

A virada:

> "Todo mundo inventa esse sistema sozinho, e ele falha sempre no mesmo ponto: ele guarda o **código**
> e joga fora o **motivo**. O git guarda os dois."

E mate de uma vez a ideia errada mais comum:

> "Git não é ferramenta de empresa grande, nem de equipe. É **memória** para uma pessoa só. Eu uso
> em projeto que ninguém vai ver, e vocês vão usar hoje num repositório com um único autor."

**Os três lugares — a única teoria da aula (não corte).** Desenhe no quadro, três caixas em fila:

| Lugar | O que é | Comando que leva para o próximo |
|---|---|---|
| **Pasta de trabalho** | os arquivos como estão agora na sua tela | `git add` |
| **Área de preparo** *(staging)* | o rascunho do próximo commit | `git commit` |
| **Histórico** | a fila de commits, cada um com código, autor, data e mensagem | — |

> "Noventa por cento da confusão com git é não saber em qual dessas três caixas a sua mudança está.
> E existe um comando que responde isso: `git status`. Vocês vão rodar ele umas quarenta vezes hoje."

---

### 10:12 — 10:30 | `init`, `status`, `add`, `commit` (18 min)

Abram o notebook e rodem a célula 0. Ela cria uma pasta temporária com o `organiza.py` da Aula 13
dentro — **ninguém mexe nos próprios arquivos hoje**.

Explique a função `git(...)` em uma frase e siga:

> "Essa função só chama o git do sistema e imprime a saída. Os `-c user.name=...` ali são para o
> notebook não depender da configuração da máquina de vocês. No projeto de verdade vocês configuram
> uma vez e esquecem — a gente faz isso às 11h35."

**`git init` (3 min).**

```
$ git init -b main
Initialized empty Git repository in /tmp/xgh-aula14-.../organizador/.git/
```

> "Repararam no que ele disse? Criou um **diretório**. O repositório é uma pasta escondida chamada
> `.git` dentro do seu projeto. Não está em nuvem nenhuma, não tem conta, não tem internet. Se vocês
> apagarem a pasta `.git`, acabou o histórico."

Sobre o `-b main`:

> "`-b main` é o nome do primeiro ramo. O git antigo chamava `master`, o GitHub chama `main`, e se
> vocês não disserem nada o git fica mandando um aviso enorme sobre isso. Digam `main` e pronto."

**`git status` (5 min).** Leia a saída em voz alta, linha por linha — ela ensina sozinha:

```
$ git status
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	organiza.py

nothing added to commit but untracked files present (use "git add" to track)
```

> "Três informações: em que ramo estou, que o histórico está vazio, e que existe um arquivo
> `organiza.py` que ele está vendo **e deixando de fora**."

Grife a palavra:

> "***Untracked***. Essa é a palavra mais importante dessa tela. O git **não** versiona nada sozinho.
> Ele versiona o que você mandar. Isso vai ser muito importante às 11h05."

> "E notem que ele **te diz o próximo comando**. O git é xingado por ser difícil, mas ele passa a
> aula inteira avisando o que fazer. Leiam a saída."

**`git add` (4 min).**

```
$ git add organiza.py
(sem saída)

$ git status --short
A  organiza.py
```

> "Sem saída nenhuma. Em terminal, silêncio é bom sinal."

Deixe a legenda do `--short` no quadro — eles vão usar o resto da vida:

| Marca | Significa |
|---|---|
| `??` | o git não conhece esse arquivo |
| `A` | arquivo novo, já preparado |
| `M` na **2ª** coluna | modificado, **ainda não** preparado |
| `M` na **1ª** coluna | modificado **e** preparado |

> "`git add .` com um ponto adiciona tudo. É o que todo mundo usa — e é exatamente por isso que
> todo mundo um dia commita o que não devia. Guardem essa frase para as 11h05."

**`git commit` (6 min).**

```
$ git commit -m "organizador move arquivo para subpasta por extensao"
[main (root-commit) cbee89e] organizador move arquivo para subpasta por extensao
 1 file changed, 13 insertions(+)
 create mode 100644 organiza.py
```

```
$ git log
commit cbee89e9d0fc59dbcff1ac1ef7aba0e3d983aac6
Author: Turma XGH <turma@xgh.local>
Date:   Sat Nov 14 10:20:00 2026 -0300

    organizador move arquivo para subpasta por extensao
```

> "Aquele monte de letras é o **hash**, o nome do commit. Sete caracteres do começo já bastam para
> você se referir a ele. Vocês vão ver `cbee89e` aparecer o resto da aula."

*(Se alguém perguntar por que o hash dele é igual ao do slide: é porque a célula 0 trava a data.
Normalmente o hash muda a cada commit, mesmo com o mesmo código.)*

E agora a frase da aula, dita devagar:

> "Esse commit é um **ponto de retorno**. De agora em diante, aconteça o que acontecer com o
> `organiza.py`, essa versão existe e é recuperável."

> "É daqui que vem a coragem de mexer no código. **Commit é um ponto de retorno; o resto é coragem.**"

---

### 10:30 — 10:45 | `git diff` e `git log` — a dívida paga (15 min)

Mexa no script: entra o **modo simulação** da Aula 13.

```
$ git status --short
 M organiza.py
```

> "`M` na segunda coluna: modificado, não preparado. Agora a pergunta da semana passada:
> **o que exatamente eu mudei?**"

```
$ git diff
diff --git a/organiza.py b/organiza.py
index d1826b0..e7d36ca 100644
--- a/organiza.py
+++ b/organiza.py
@@ -1,11 +1,14 @@
 import sys, shutil
 from pathlib import Path

-def organizar(pasta):
+def organizar(pasta, simular=False):
     pasta = Path(pasta)
     for arquivo in pasta.iterdir():
         if arquivo.is_file():
             destino = pasta / arquivo.suffix.lstrip(".").lower()
+            if simular:
+                print(f"moveria {arquivo.name} -> {destino.name}/")
+                continue
             destino.mkdir(exist_ok=True)
             shutil.move(str(arquivo), str(destino / arquivo.name))
```

Ensine a ler, apontando na tela:

- `---` é como estava, `+++` é como está
- `@@ -1,11 +1,14 @@` — mexeu no trecho que começa na linha 1; eram 11 linhas, agora são 14
- `-` na frente: **saiu**. `+`: **entrou**. Sem nada na frente: só contexto, para você se achar

> "*'Mudei alguma coisa e não sei o quê'* — **esta tela é a resposta**. Uma linha saiu, quatro
> entraram, e vocês estão olhando exatamente para elas. Dívida paga."

**A regra de ouro, para o quadro:**

> "Rodem `git diff` **antes** de todo `git add`. Leva dois segundos e é a última chance de ver o que
> vocês estão guardando — inclusive aquele `print('teste 123')` que ficou no meio do código."

Commit da mudança, e repare na mensagem:

```
$ git commit -m "modo simulacao: testar sem mover arquivo nenhum"
[main e48efc2] modo simulacao: testar sem mover arquivo nenhum
 1 file changed, 4 insertions(+), 1 deletion(-)

$ git log --oneline
e48efc2 modo simulacao: testar sem mover arquivo nenhum
cbee89e organizador move arquivo para subpasta por extensao
```

> "`git log --oneline` é o comando que vocês vão usar mais vezes na vida. Um commit por linha."

> "E olhem a mensagem: ela não diz *'adicionei um parâmetro simular'* — isso o diff já disse. Ela diz
> **para que** o modo simulação existe. Guardem, que às 11h25 isso vira bloco inteiro."

---

### 10:45 — 10:55 | `.gitignore`: o que **não** entra (10 min)

Simule o estado real da pasta depois de algumas execuções: cache do Python, o `venv` da Aula 11 e o
log da Aula 13.

```
$ git status --short
?? .venv/
?? __pycache__/
?? organiza.log
```

Pergunte antes de responder: *"qual desses três é o seu código?"*

> "Nenhum. E essa é a pergunta que decide tudo: **isso é código que eu escrevi, ou isso é resultado?**"

Passe um por um:

- `__pycache__/` — o Python gera de novo sozinho, e é **binário**: o diff fica ilegível
- `.venv/` — pode ter **milhares** de arquivos e é específico da sua máquina. Quem clona recria com o
  `requirements.txt` da Aula 11. *("Olha o `venv` que eu prometi na Aula 11 aparecendo no Projeto 2.")*
- `*.log` — cresce para sempre e muda a cada execução. Cada commit viraria um diff gigante de lixo

A solução, um arquivo de texto, uma regra por linha:

```
__pycache__/
.venv/
*.log
```

```
$ git status --short
?? .gitignore
```

> "Sumiram os três. Sobrou o `.gitignore` em si — e ele **entra** no commit, porque a regra também é
> parte do projeto. Quem clonar herda os mesmos *'não commite isso'*."

Diga que ignorar não é apagar, porque alguém vai se confundir:

> "Os arquivos continuam na pasta de vocês. Ignorar é dizer ao **git** para não olhar, não ao
> computador para deletar."

**A regra para o quadro:**

> **Se for gerado, baixado, pesado ou secreto: não vai para o git.**

Pare meio segundo na última palavra:

> "*Secreto.* Segura essa. Depois do café."

---

### 10:55 — 11:05 | ☕ Pausa (10 min)

*(Quem chegou sem conta no GitHub cria agora. Leva três minutos e não pede cartão.)*

---

### 11:05 — 11:25 | 💥 nº 1 — **o git lembra para sempre** (20 min)

**Este é o bloco mais importante da aula.** Se a aula desandar e sobrarem 20 minutos, é este que
você dá. Avise a turma disso.

> "Vou contar uma história que acontece toda semana com gente que programa há anos. E no fim vocês
> vão ver a saída na tela, não vão ter que acreditar em mim."

**A história (3 min).** Você resolveu fazer o organizador avisar no Telegram quando terminar. Precisa
de uma chave. Cria um `config.py`, cola a chave, e commita junto — porque senão o programa não roda.

```
$ git commit -m "avisa no telegram quando terminar"
[main 54269bb] avisa no telegram quando terminar
 2 files changed, 13 insertions(+)
 create mode 100644 avisa.py
 create mode 100644 config.py
```

**O conserto que todo mundo faz (4 min).** Duas semanas depois cai a ficha. Você faz exatamente o
certo: tira o arquivo do git, coloca no `.gitignore`, deixa um `config.exemplo.py` sem segredo, e
commita o conserto.

```
$ git rm --cached config.py
rm 'config.py'

$ git commit -m "tira a chave do repositorio"
[main a7d9005] tira a chave do repositorio
 3 files changed, 3 insertions(+), 2 deletions(-)
 create mode 100644 config.exemplo.py
 delete mode 100644 config.py
```

Explique o `--cached`:

> "`git rm --cached` quer dizer: *pare de versionar isso, mas não apague do meu disco*. É o comando
> certo. O que vem agora não é culpa dele."

```
$ git status
On branch main
nothing to commit, working tree clean
```

> "Pasta limpa. `status` limpo. É aqui que todo mundo respira aliviado e vai dormir."

**💥 O soco (8 min).** Mude o tom. Fale mais devagar.

> "O commit **não é um save**. É uma **foto**. Apagar o arquivo hoje não apaga a foto de ontem."

Rode. Esta é a saída real:

```
$ git log -p -- config.py
commit a7d90056a4e6f42bf7133fb1cac81edb9a6832fa
    tira a chave do repositorio

diff --git a/config.py b/config.py
deleted file mode 100644
--- a/config.py
+++ /dev/null
@@ -1,2 +0,0 @@
-CHAVE_API = "xgh-live-8f3b91d04c7a4e26"
-CHAT_ID = "-1001998877665"

commit 54269bb944d6b61c32f68205e5a59d9caeb22468
    avisa no telegram quando terminar

diff --git a/config.py b/config.py
new file mode 100644
--- /dev/null
+++ b/config.py
@@ -0,0 +1,2 @@
+CHAVE_API = "xgh-live-8f3b91d04c7a4e26"
+CHAT_ID = "-1001998877665"
```

```
------------------------------------------------------------
tempo para recuperar a chave apagada: 0.004 segundos
a chave aparece no histórico: True
```

Deixe o silêncio trabalhar. Depois o detalhe que ninguém espera:

> "Olhem com atenção: a chave aparece **duas vezes**. No commit em que ela entrou, com `+`. E no
> commit em que vocês a **apagaram**, com `-` — porque para o diff mostrar que uma linha saiu, ele
> **tem que dizer qual linha era**."

> "O commit do conserto é o que mais grita a chave. Vocês a escreveram no histórico duas vezes."

E não para no diff. O arquivo inteiro está lá, do jeito que era:

```
$ git show 54269bb:config.py
CHAVE_API = "xgh-live-8f3b91d04c7a4e26"
CHAT_ID = "-1001998877665"
```

E dá para varrer o histórico inteiro procurando um texto:

```
$ git grep -n xgh-live $(git rev-list --all)
54269bb944d6b61c32f68205e5a59d9caeb22468:config.py:1:CHAVE_API = "xgh-live-8f3b91d04c7a4e26"
```

> "Cinco commits varridos, um comando, resultado na hora. Agora imaginem isso rodando 24 horas por
> dia em cima de **tudo que é público no GitHub**. Não é imaginação: é um negócio. Tem robô que acha
> chave em repositório novo em **minutos**."

> "Quatro milésimos de segundo. Foi o que custou achar a chave que vocês *acham* que apagaram."

*(O número varia um pouco a cada execução — na sua máquina deve dar algo entre 3 e 10 milésimos. Se
sair diferente do slide, fale a ordem de grandeza: "milésimos de segundo".)*

**O que fazer de verdade (4 min).** A ordem importa, e a primeira linha é a única que resolve:

1. **Revogue a chave.** Painel do provedor, invalidar. Chave vazada não tem conserto — tem
   **substituição**. Tudo depois disso é limpeza
2. Gere uma nova e guarde **fora** do repositório — `config.py` ignorado, ou variável de ambiente
3. Commite o `.gitignore` e um `config.exemplo.py` sem segredo, para quem clonar saber o que criar
4. **Só então** pense em reescrever o histórico (`git filter-repo`, `BFG`). É cirurgia: muda todos os
   hashes depois do ponto, quebra o repositório de quem já clonou — e **não serve para nada** se a
   chave já foi copiada

> "Reparem por que o passo 1 é o passo 1: reescrever o histórico não desfaz quem já copiou. Revogar,
> sim. Mexer no histórico é para a chave **não** aparecer na próxima varredura; não é para desfazer."

**🌱 A semente da Aula 18 — diga isso com todas as letras (1 min):**

> "Hoje a chave é falsa e o prejuízo é zero. **Na Aula 18 vocês vão ter uma chave de verdade na mão**
> — uma que chama modelo de linguagem, e que, se alguém pegar, gasta na conta de vocês."

> "É por isso que esta aula vem **antes** daquela e não depois. Quando chegarmos lá, a primeira
> pergunta que eu vou fazer, antes de qualquer código, é: ***onde você guardou a chave?***"

> "O segredo mais seguro do mundo é o que **nunca** entrou num commit."

---

### 11:25 — 11:35 | 💥 nº 2 e nº 3 — mensagem inútil e clonar sem rodar (10 min)

**💥 nº 2 — mensagem de commit inútil (6 min).** Projete os dois logs lado a lado. Saída real da
célula do notebook — **os dois repositórios têm exatamente as mesmas mudanças de código**, só as
mensagens mudam:

```
git log --oneline    RUIM   | git log --oneline    BOM
----------------------------+-------------------------------------------------------
d35051a fix                 | 739355e nao refaz o que ja foi feito: rodar duas vezes nao duplica
9fa33c0 correcoes           | 12475bc caminho absoluto: o relativo quebrava no agendador
20ca3c0 update              | 55ed99e log em arquivo: script das 3h tem que deixar rastro
dd28f7d agora vai           | 124f01c ignora cache, venv e log
1139313 teste               | 05b1614 modo simulacao: testar sem mover arquivo nenhum
9520dc5 ajustes             | dcaaf24 organizador move arquivo para subpasta por extensao
```

Faça a pergunta e espere:

> "Na coluna da esquerda: **em qual desses commits entrou o modo simulação?**"

> "Não dá. E não é que a informação esteja escondida — ela **nunca foi escrita**. O git guardou o
> código perfeitamente. O que se perdeu foi o *porquê*, que só existia na cabeça de vocês naquela
> terça-feira."

Agora a direita:

> "A coluna da direita é um **diário do projeto**. Dá para ler de cima a baixo e entender a história:
> nasceu movendo arquivo, ganhou modo de teste, parou de versionar lixo, ganhou log, consertou o
> caminho que quebrava no agendador — todas as dívidas da Aula 13, em ordem — e virou idempotente."

A regra, para o quadro:

| | |
|---|---|
| **O quê** mudou | o `diff` já mostra — não repita |
| **Por quê** mudou | só você sabe — **é isso que vai na mensagem** |

O teste que funciona:

> "A mensagem tem que fazer sentido depois de *'este commit...'*."
> "*'Este commit **ajustes**'* — não é frase. *'Este commit **faz o caminho ser absoluto porque o
> relativo quebrava no agendador**'* — é."

E o diagnóstico que vale mais que a regra:

> "Se vocês não conseguem resumir numa linha, normalmente o problema não é a frase: **é que o commit
> está fazendo cinco coisas ao mesmo tempo.** Commit pequeno é mensagem fácil."

**💥 nº 3 — clonar e não rodar (4 min).** `git clone` copia o repositório inteiro. Clone o nosso para
outra pasta, como se fosse o colega baixando do GitHub, e rode:

```
$ ls clone-do-colega/
  .gitignore  avisa.py  config.exemplo.py  organiza.py

$ python avisa.py
Traceback (most recent call last):
  File ".../clone-do-colega/avisa.py", line 1, in <module>
    import schedule, time
ModuleNotFoundError: No module named 'schedule'
```

> "`ModuleNotFoundError`. É o erro nº 1 da Aula 11 de volta — e agora mais caro, porque está na
> máquina de **outra pessoa**, que não sabe o que instalar."

Falta o `requirements.txt`. Conserto em duas linhas, e a mensagem de commit já ensina:

```
$ git commit -m "requirements.txt: sem isso o projeto nao roda na maquina de ninguem"
```

E tem um **segundo** motivo, que é consequência direta do bloco anterior:

```
>>> from config import CHAVE_API
ModuleNotFoundError: No module named 'config'
```

> "O `config.py` está no `.gitignore`, então ele **não veio no clone**. E isso está certo! Mas o
> programa importa dele. Por isso existe o `config.exemplo.py` — e por isso o README tem que dizer
> *'copie o exemplo para `config.py` e preencha'*."

> "Esses dois erros são o teste de qualidade do Projeto 2. Eles dão a lista do que o README precisa
> responder."

---

### 11:35 — 11:50 | 🛠️ Mão na massa: no ar, com README (15 min)

**Este é o bloco em que eles fazem.** Você faz uma vez ao vivo, em três minutos, e solta a turma.

**Ao vivo (3 min).** Projete e vá falando:

```
git config --global user.name "Guilherme"
git config --global user.email "seu@email.com"
```

> "Uma vez na vida, por máquina. Usem o **mesmo e-mail da conta do GitHub** — é assim que o commit
> aparece com a cara de vocês no site."

No GitHub: **New** → nome → **Public** → **não marque nenhuma caixinha** (`README`, `.gitignore`,
licença).

> "Repositório vazio. Se vocês marcarem as caixinhas, o GitHub já cria um commit lá, e o primeiro
> `push` de vocês dá conflito. Vazio é mais fácil."

```
git remote add origin https://github.com/SEU-USUARIO/organizador.git
git remote -v
git push -u origin main
```

> "`origin` é só um apelido para o endereço. Poderia ser qualquer palavra; todo mundo usa `origin`."
> "`-u origin main` liga o `main` daqui ao `main` de lá. **Da segunda vez em diante é só `git push`.**"

**A senha que não é senha — avise antes de eles baterem nisso:**

> "O GitHub não aceita mais a senha da conta no `push`. Vocês vão usar um **token**: foto →
> *Settings* → *Developer settings* → *Personal access tokens* → *Tokens (classic)* →
> *Generate new token* → marcar o escopo **`repo`** → copiar."

> "Quando o `push` pedir usuário e senha: usuário é o login, **senha é o token**."

> "E esse token é **exatamente** o tipo de coisa das 11h05. Ele dá acesso de escrita a todos os
> repositórios de vocês. Nunca em arquivo, nunca em commit, nunca no grupo do WhatsApp."

*(Quem preferir, a extensão GitHub do VS Code faz login pelo navegador e você não vê token nenhum.
Mais confortável — mas é bom saber que o token existe, para entender o erro quando aparecer.)*

**O ciclo do dia a dia, para o quadro, e fica lá até o fim do curso:**

```
git status      # onde está cada coisa
git diff        # o que exatamente eu mudei
git add .       # escolho o que entra
git commit -m "por que eu mudei"
git push        # manda para o GitHub
```

> "Cinco comandos. É isso que 95% do uso de git é, para sempre."

**Agora eles (12 min).** Tarefa do bloco, no quadro:

1. `git init` na pasta de um projeto seu — **o da Aula 13, se estiver de pé**
2. `.gitignore` com `__pycache__/`, `.venv/`, `*.log` e o seu arquivo de segredo, se tiver
3. `requirements.txt` com `pip freeze > requirements.txt`
4. `README.md` com **cinco seções**: o que é · como instalar · como rodar · como agendar · detalhes
5. Commit e `push`
6. **Manda o link no grupo**

Circule. Os travamentos que você vai ver, em ordem de frequência:

- **`git` não é reconhecido** (Windows) → não reabriu o VS Code depois de instalar
- **pediu senha e rejeitou** → é token, não senha
- **`nothing added to commit`** → esqueceu o `git add`
- **`.gitignore` não funcionou** → o arquivo já estava sendo versionado antes da regra.
  É `git rm --cached` — e aproveite para repetir que o histórico continua lá
- **`.gitignore` salvo como `.gitignore.txt`** pelo Notepad do Windows → clássico, olhe o nome

**Sobre o README, diga isso e não mais que isso:**

> "Três coisas fazem um README ruim ficar bom na hora: a **primeira frase** diz o que o programa faz,
> sem *'este projeto tem como objetivo'*; os comandos estão em bloco de código, **copiáveis e
> completos**, não *'instale as dependências'*; e tem a linha do caso chato — o segredo que precisa
> ser criado, a versão do Python, o agendamento."

> "**O teste do seu README:** entregue o link para alguém que nunca viu o projeto e fique calado. Se
> a pessoa consegue rodar, presta. Se ela te perguntar algo, a pergunta dela **é** a linha que falta."

---

### 11:50 — 11:54 | Projeto 2: o quadro de critérios (4 min)

Projete o quadro e leia **linha por linha**. Diga que é exatamente por isso que você vai conferir.

**Entrega: Aula 16, em duas semanas. O que se entrega é um link de repositório público.**

| # | Critério | Como eu vou conferir |
|---|---|---|
| 1 | **Roda sozinho** — é um `.py`, recebe o que precisa por argumento, não pede nada no meio | eu rodo pelo terminal, sem abrir o código |
| 2 | **`README.md`** com: o que é · como instalar · como rodar (comando copiável) | eu leio o README e tento usar **sem te perguntar nada** |
| 3 | **`requirements.txt`** | eu clono num ambiente limpo e instalo com `pip install -r` |
| 4 | **`.gitignore`** — sem `__pycache__/`, sem `.venv/`, sem log, **sem segredo** | eu abro a lista de arquivos do repositório |
| 5 | **Sobrevive a rodar duas vezes seguidas sem duplicar nada** | eu rodo duas vezes e comparo o resultado |
| 6 | **Histórico legível** — no mínimo 5 commits com mensagem que diz *por quê* | `git log --oneline` |

> "O critério 5 é o que mais reprova, e é a dívida da Aula 13 cobrada com juros. Antes de entregar,
> rodem o mesmo comando duas vezes seguidas e **olhem o resultado**. Linha repetida no relatório,
> arquivo duplicado, e-mail mandado duas vezes, erro na segunda — não está pronto."

> "As duas saídas são as de sábado passado: **verificar antes de agir** — *'o arquivo já está lá?
> então não mexe'* — ou **marcar o que já foi feito**."

**O atalho honesto, diga para não perder ninguém:**

> "Se a automação da Aula 13 não está de pé, **publiquem o que existe**, com um README dizendo o que
> falta. Repositório com projeto pequeno e honesto vale mais que repositório vazio com nome bonito."

Sugestões, para quem não tem projeto: organizador de Downloads · relatório automático de uma planilha
sua · monitor de cotação ou de preço · gerador de recibo em PDF.

---

### 11:54 — 12:00 | O que isso destrava + tarefa (6 min)

**O que isso destrava (3 min).** Projete e leia:

> - **Mexer no código sem medo.** Quebrou? Volta. O commit é o ponto de retorno
> - **Trabalhar na mesma coisa de dois computadores** — o do trabalho e o de casa
> - **Ter o que mostrar.** Link de repositório é o portfólio de quem programa. Não é currículo
>   falando de você; é código seu, com data
> - **Pegar código do mundo.** `git clone` de qualquer projeto público — vocês já podem ler, rodar e
>   mexer em projeto de gente que nunca vão conhecer
> - **Guardar segredo no lugar certo** — que é o que a Aula 18 vai exigir de vocês

> "Vocês não ficaram melhores em Python hoje. Ficaram com **direito de errar**."

**A tarefa (2 min):**

1. **Publicar um repositório** — pode ser o Projeto 1. Com `README.md`, `.gitignore` e
   `requirements.txt`. **Link no grupo**
2. **Cinco commits de verdade**, cada um passando no teste do *"este commit..."*
3. **Abrir o repositório de um colega e tentar rodar sem perguntar nada.** Depois mandar para a
   pessoa, no privado, **a primeira coisa que te travou**. Isso vale mais que elogio
4. **Rodar o próprio script duas vezes seguidas** e dizer no grupo o que aconteceu na segunda
5. **Desafio:** procurar na documentação do git o que `git restore` e `git revert` fazem e qual é a
   diferença. É a Aula 11 aplicada — ler documentação de ferramenta que ninguém ensinou. Resposta em
   uma frase

**A pergunta que fica (1 min).** Vire o tom, olhe para a turma:

> "Olhem para tudo que a gente fez em quatorze aulas. **Cada decisão que o programa de vocês toma,
> foi vocês que escreveram.** `if nota >= 7`. `if extensao == '.pdf'`. `if status_code == 200`."

> "O limiar é de vocês. A regra é de vocês."

> "**E se eu não souber a regra?** Se eu tiver dez mil e-mails, metade spam, e ninguém no mundo
> conseguir escrever o `if` que separa os dois?"

> "**Como é que o programa decide a partir do dado, e não da regra que eu escrevi?**"

*(Aula 15: machine learning de verdade — e a gente volta na Aula 3.)*

---

## Se sobrar tempo

- `git restore organiza.py` ao vivo: estrague o arquivo na frente deles e traga de volta. É o momento
  em que a ficha cai de verdade sobre "ponto de retorno"
- `git log --oneline --graph --all` só para mostrar que ramo existe — **sem ensinar branch hoje**
- `git diff --staged` — a diferença entre "o que mudei" e "o que já preparei"
- Abrir um projeto famoso no GitHub e ler o `README.md` e o `.gitignore` dele. Dá repertório rápido
- `git blame organiza.py` — quem escreveu cada linha e quando. Costuma arrancar um "não acredito"

## Se faltar tempo

Corte nesta ordem: (1) o `git grep` em todos os commits do bloco das 11h05 — o `git log -p` sozinho
já dá o soco; (2) o bloco do README em detalhe, deixando só as cinco seções no quadro; (3) o
💥 nº 3 (clonar e não rodar) — mas **diga em uma frase** que falta `requirements.txt` e segredo;
(4) a tabela de legenda do `status --short`.

**Nunca corte:** a pasta do horror com as três perguntas na abertura · os três lugares no quadro ·
`status` → `diff` → `add` → `commit` rodado por eles · **o bloco das 11h05 inteiro, com a semente da
Aula 18 dita em voz alta** · o quadro de critérios do Projeto 2 com o critério 5 lido em voz alta.
Se der ruim de verdade e sobrarem 25 minutos, dê o ciclo de cinco comandos e o bloco da chave. É a
aula.

---

## Depois da aula — 15 minutos

- [ ] Mandar no grupo: link do notebook, a tarefa escrita, **o quadro de critérios do Projeto 2** e
      **o ciclo de cinco comandos** (`status` → `diff` → `add` → `commit` → `push`) — eles vão
      consultar por meses
- [ ] Mandar também, separado e em uma linha só: *"chave, senha e token nunca entram num commit —
      o git lembra para sempre"*. Repetição é o que faz pegar
- [ ] **Anotar quem conseguiu dar `push` até o fim da aula.** Quem não conseguiu vira prioridade na
      Aula 15: sem repositório no ar, não tem Projeto 2
- [ ] Anotar em `duvidas-da-turma.md` o que te pegou — aposto no token e no `.gitignore.txt`
- [ ] **Abrir os repositórios que eles mandarem no grupo e clonar um**, de verdade. Se você não
      conseguir rodar, é diagnóstico para a Aula 15 e não bronca
- [ ] **Preparar a Aula 15:** confira se o `scikit-learn` instala e importa na sua máquina (`pip
      install scikit-learn`, `import sklearn` — nome diferente, Aula 11) e rode o notebook inteiro
      medindo os números de treino e teste. **Não leve número não medido**
- [ ] 📓 **Nota Unipds — escrever para si mesmo, sem jargão:** *por que o git guarda o conteúdo inteiro
      de cada versão e ainda assim o repositório não fica gigante?* Procure "objeto", "blob" e
      "packfile", e explique como se fosse para a turma. Você vai precisar disso no dia em que alguém
      perguntar por que a pasta `.git` é pequena.
