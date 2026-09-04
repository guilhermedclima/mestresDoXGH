# Guia de instalação — Python e VS Code na sua máquina

**Mestres do XGH · material da Aula 8**

Até agora tudo rodou no Google Colab, no navegador. Isso foi ótimo para começar: zero instalação,
funciona em qualquer máquina. Mas o Colab tem um problema que ficou claro na Aula 8: **os arquivos
que você cria somem quando a sessão fecha.** Eles estão numa máquina do Google, não na sua.

A partir daqui os programas rodam na sua máquina, e o que você escreve **fica**.

> **Se algo der errado, não trave.** Vá até onde conseguir, mande print da tela no grupo, e
> continue no Colab enquanto isso. Ninguém fica sem ambiente. Isso aqui é chato uma vez só.

---

## O que vamos instalar

| | O que é | Para que serve |
|---|---|---|
| **Python** | a linguagem | é quem realmente executa o seu programa |
| **VS Code** | um editor | onde você escreve, com cores, erros marcados e um botão de rodar |

São duas coisas separadas. O VS Code sem Python não roda nada; o Python sem VS Code roda, mas
escrever fica desconfortável.

---

# Parte 1 — Windows

## 1.1 Baixar o Python

1. Abra **python.org/downloads**
2. Clique no botão grande amarelo: **Download Python 3.x**
3. Espere baixar e abra o arquivo

## 1.2 Instalar — ATENÇÃO nesta tela

A primeira tela do instalador tem uma caixinha embaixo:

```
☑  Add python.exe to PATH        ← MARQUE ESTA CAIXA
```

**Marque antes de clicar em qualquer coisa.** Ela vem desmarcada.

Se você não marcar, o Python instala mas o computador não sabe onde ele está — e no passo 1.3 o
terminal vai dizer que não conhece o comando `python`. É de longe o erro mais comum, e a correção
é reinstalar.

Depois de marcar, clique em **Install Now** e espere. Pode levar alguns minutos.

No fim, se aparecer um botão **"Disable path length limit"**, clique nele também. Não faz mal e
evita problema com caminhos longos.

## 1.3 Conferir

Abra o **Prompt de Comando**: tecla Windows, digite `cmd`, Enter.

Digite:

```
python --version
```

Deve responder algo como:

```
Python 3.13.1
```

**Deu certo?** Vá para a Parte 3.

**Apareceu "python não é reconhecido como comando"?** O PATH não foi marcado. Reinstale: abra o
instalador de novo, escolha **Uninstall**, e depois instale novamente marcando a caixa.

**Abriu a Microsoft Store?** O Windows tem um atalho falso de `python` que faz isso. Instale pelo
python.org como acima; ele passa na frente.

---

# Parte 2 — Mac

## 2.1 Baixar o Python

1. Abra **python.org/downloads**
2. O site detecta o Mac e mostra o botão certo: **Download Python 3.x**
3. Abra o `.pkg` baixado e siga o instalador clicando em *Continuar*

Não tem caixinha para marcar aqui — no Mac é mais simples.

## 2.2 Conferir

Abra o **Terminal**: `Cmd + Espaço`, digite `Terminal`, Enter.

Digite:

```
python3 --version
```

Deve responder:

```
Python 3.13.1
```

> **No Mac o comando é `python3`, com o 3.** O `python` sozinho pode apontar para uma versão
> antiga que veio com o sistema. Sempre que este guia disser `python`, você digita `python3`.

---

# Parte 3 — VS Code (Windows e Mac)

## 3.1 Instalar

1. Abra **code.visualstudio.com**
2. Clique no botão de download (o site já detecta o seu sistema)
3. Instale normalmente
   - **Windows:** durante a instalação, marque *"Adicionar ao PATH"* e *"Abrir com Code"* se aparecerem
   - **Mac:** arraste o Visual Studio Code para a pasta *Aplicativos*

## 3.2 A extensão de Python — este passo é obrigatório

Abra o VS Code e olhe a barra de ícones da **esquerda**. Clique no que parece **quatro
quadradinhos** (Extensões). Ou use o atalho:

- Windows: `Ctrl + Shift + X`
- Mac: `Cmd + Shift + X`

Na caixa de busca, digite **Python**.

Instale a primeira, a **da Microsoft** — o ícone é azul e amarelo e ela tem centenas de milhões de
downloads. Clique em **Install**.

> Sem essa extensão o VS Code trata o seu arquivo como texto qualquer: sem cores, sem marcação de
> erro, sem o botão de rodar.

## 3.3 Deixar em português (opcional)

Ainda em Extensões, busque **Portuguese (Brazil) Language Pack**, instale e reinicie o VS Code.

---

# Parte 4 — O primeiro programa na sua máquina

## 4.1 Criar a pasta

Crie uma pasta chamada **`mestres-do-xgh`** onde você achar melhor — Documentos ou a Área de
Trabalho servem.

**Um conselho que economiza dor de cabeça:** evite acentos e espaços no caminho até essa pasta.
`C:\Users\joao\mestres-do-xgh` é melhor que `C:\Users\joão\Meus Documentos\Programação`.

## 4.2 Abrir no VS Code

No VS Code: **Arquivo → Abrir Pasta** (*File → Open Folder*) e escolha a pasta que você criou.

Se aparecer uma pergunta sobre *"confiar nos autores desta pasta"*, responda que **sim** — a pasta
é sua.

## 4.3 Criar o arquivo

Na barra lateral esquerda, passe o mouse sobre o nome da pasta e clique no ícone de **novo
arquivo** (a folhinha com um `+`).

Nomeie **`ola.py`**. A terminação `.py` é obrigatória — é ela que diz ao VS Code que aquilo é
Python.

Digite dentro:

```python
nome = "Guilherme"
print(f"Olá, {nome}! Isso está rodando na minha máquina.")
```

Salve: `Ctrl + S` (Windows) ou `Cmd + S` (Mac).

## 4.4 Rodar

Clique no **▶ (play)** no canto superior direito.

Um painel abre embaixo (o **terminal**) e mostra:

```
Olá, Guilherme! Isso está rodando na minha máquina.
```

**Pronto.** Esse `print` é o mesmo da Aula 1 — só que agora ele não está no navegador. Está na sua
máquina, e o arquivo continua lá amanhã.

## 4.5 Rodar pelo terminal (vale conhecer)

O ▶ é um atalho. Por baixo, ele faz isto:

```
python ola.py          (Windows)
python3 ola.py         (Mac)
```

Você pode digitar isso no terminal do VS Code (**Terminal → Novo Terminal**). Saber esse comando é
útil porque é assim que programa Python roda em servidor, e é o que você vai usar na Aula 13.

---

# Parte 5 — A diferença que você vai sentir

| | Colab | Sua máquina |
|---|---|---|
| Onde roda | servidor do Google | seu computador |
| Arquivos criados | **somem** com a sessão | ficam |
| Precisa de internet | sim | **não** |
| Como executa | célula por célula | o arquivo inteiro, de cima a baixo |
| `input()` | caixinha no meio da página | no terminal |

**Um detalhe que confunde na primeira semana:** no Colab você roda pedacinhos e as variáveis ficam
guardadas entre as células. No VS Code o arquivo roda **inteiro, do começo ao fim, toda vez**. Se
você quiser testar só um trecho, coloque ele num arquivo separado.

---

# Problemas comuns

**"python não é reconhecido como comando interno ou externo"** *(Windows)*
O `Add python.exe to PATH` não foi marcado. Reinstale marcando a caixa.

**Abre a Microsoft Store quando digito `python`** *(Windows)*
É um atalho falso do Windows. Instale pelo python.org; ele passa na frente. Se insistir, procure
por *"Aliases de execução de aplicativo"* nas Configurações e desligue os dois de Python.

**O ▶ não aparece no VS Code**
Falta a extensão Python da Microsoft (passo 3.2), ou o arquivo não termina em `.py`.

**"Select Interpreter" ou "Nenhum interpretador selecionado"**
`Ctrl + Shift + P` (`Cmd + Shift + P` no Mac), digite `Python: Select Interpreter`, e escolha a
versão que você instalou.

**O antivírus bloqueou a instalação**
Comum em máquina corporativa. Se for do trabalho, pode ser que você não tenha permissão. Fale no
grupo — dá para continuar no Colab sem prejuízo.

**Instalei mas o VS Code mostra erro vermelho embaixo do `print`**
Quase sempre é interpretador não selecionado. Veja o item acima.

**Nada disso resolveu**
Print da tela no grupo, com a mensagem de erro inteira aparecendo. **Print da tela inteira, não só
do pedaço** — o contexto costuma estar fora do recorte.

---

# Checklist final

- [ ] `python --version` (ou `python3 --version`) responde com um número
- [ ] VS Code instalado
- [ ] Extensão **Python** da Microsoft instalada
- [ ] Pasta `mestres-do-xgh` criada e aberta no VS Code
- [ ] `ola.py` criado, salvo e rodando com o ▶

Cinco caixinhas marcadas e você tem um ambiente de programação de verdade na sua máquina — o
mesmo que gente que faz isso profissionalmente usa todo dia.
