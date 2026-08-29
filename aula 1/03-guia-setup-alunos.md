# Guia de Instalação — Mestres do XGH

Este guia tem duas partes. **Faça a Parte 1 antes da aula 1** (leva 2 minutos) e a **Parte 2 antes da aula 2**.

Travou em algum passo? Manda print do erro no grupo. Ninguém aqui vai rir — todo mundo já travou nisso.

---

## PARTE 1 — Para a aula 1: Google Colab (nada para instalar)

O Colab roda Python direto no navegador. Você não instala nada e funciona igual no Windows, Mac, Linux e até no celular.

1. Abra `colab.research.google.com`
2. Entre com sua conta Google (qualquer Gmail serve)
3. Clique em **Novo notebook** (ou *New notebook*)
4. Na caixa cinza que aparece, digite:

   ```python
   print("Olá, mundo")
   ```

5. Aperte **Shift + Enter** (ou clique no ▶ à esquerda)
6. Se apareceu `Olá, mundo` embaixo: **está pronto.** Pode fechar.

> A primeira execução demora uns 10 segundos porque o Google está ligando uma máquina para você. As próximas são instantâneas.

**Coisas úteis do Colab:**

| Ação | Como |
|---|---|
| Rodar a célula | `Shift + Enter` |
| Nova célula de código | Botão **+ Código** no topo |
| Salvar | Salva sozinho no seu Google Drive |
| Achar de novo depois | Google Drive → pasta **Colab Notebooks** |

Renomeie seu notebook (clique no nome lá em cima) para `Aula 01 - Meu nome`. Daqui a um mês você vai ter dez e não vai achar nenhum.

---

## PARTE 2 — Para a aula 2: Python + VS Code na sua máquina

O Colab é ótimo para começar, mas programador de verdade roda código na própria máquina. São dois programas.

### 2.1 — Instalar o Python

#### Windows

1. Vá em `python.org/downloads`
2. Clique no botão amarelo **Download Python 3.x**
3. Abra o arquivo baixado
4. **⚠️ ANTES DE CLICAR EM QUALQUER COISA:** marque a caixinha **"Add python.exe to PATH"** na parte de baixo da janela.

   > Este é o passo que todo mundo esquece e depois passa uma hora tentando entender por que nada funciona. Marque a caixinha.

5. Clique em **Install Now**
6. Espere terminar e feche

#### macOS

1. Vá em `python.org/downloads`, baixe e instale normalmente
2. Ou, se você tiver Homebrew: `brew install python`

#### Linux (Ubuntu/Debian)

Já vem instalado. Confirme com `python3 --version`. Se faltar algo:

```bash
sudo apt update && sudo apt install python3 python3-pip
```

#### Conferir se deu certo

Abra o terminal:

- **Windows:** tecla Windows → digite `cmd` → Enter
- **Mac:** `Cmd + Espaço` → digite `terminal` → Enter
- **Linux:** `Ctrl + Alt + T`

Digite e dê Enter:

```
python --version
```

Se aparecer algo como `Python 3.13.1`, funcionou.

**Se der erro:**

| Erro | O que fazer |
|---|---|
| `python não é reconhecido...` (Windows) | Você não marcou "Add to PATH". Desinstale, instale de novo e marque a caixinha. |
| `command not found: python` (Mac/Linux) | Tente `python3 --version`. Se funcionar, no seu sistema o comando é `python3`. |

### 2.2 — Instalar o VS Code

O VS Code é o editor onde você vai escrever o código. É gratuito e é o mais usado no mercado.

1. Vá em `code.visualstudio.com`
2. Clique no botão grande de download (ele detecta seu sistema sozinho)
3. Instale. No Windows, marque **"Add to PATH"** se aparecer a opção.

### 2.3 — Instalar a extensão do Python no VS Code

1. Abra o VS Code
2. Na barra lateral esquerda, clique no ícone de **blocos/quadradinhos** (Extensions) — ou aperte `Ctrl + Shift + X` (`Cmd + Shift + X` no Mac)
3. Na busca, digite `Python`
4. Instale a primeira da lista, a da **Microsoft** (ícone azul e amarelo, tem milhões de downloads)

### 2.4 — Testar tudo junto

1. Crie uma pasta no seu computador chamada `mestres-do-xgh`
2. No VS Code: **File → Open Folder** e escolha essa pasta
3. **File → New File**, salve como `teste.py` (a extensão `.py` é obrigatória)
4. Escreva:

   ```python
   print("Meu ambiente está funcionando!")
   ```

5. Aperte `Ctrl + S` para salvar
6. Clique no **▶ (play)** no canto superior direito
7. Se apareceu a mensagem no painel de baixo: **está tudo pronto.** 🎉

---

## Antes da aula 2, confira

- [ ] Consigo abrir o Colab e rodar um `print`
- [ ] `python --version` responde no terminal
- [ ] VS Code instalado e abre
- [ ] Extensão Python da Microsoft instalada
- [ ] Rodei o `teste.py` e vi a mensagem
- [ ] Estou no grupo da turma

Não conseguiu tudo? **Vem mesmo assim.** A gente resolve na hora — mas avisa no grupo antes, para eu já chegar sabendo.

---

## Problemas comuns

**"Instalei o Python mas o VS Code não acha"**
No VS Code aperte `Ctrl + Shift + P`, digite `Python: Select Interpreter`, e escolha a versão que aparecer na lista.

**"Meu computador é fraco / não tenho computador"**
Sem problema: o Colab resolve tudo do Módulo 1 ao 3, e funciona até pelo celular. Me avisa no grupo que eu adapto os exercícios.

**"Tem antivírus bloqueando a instalação"**
Comum no Windows com instaladores baixados. Baixe **só** de `python.org` e `code.visualstudio.com` — nunca de site de download genérico. Se o Windows mostrar "Windows protegeu o computador", clique em **Mais informações → Executar assim mesmo** (só faça isso com esses dois sites).

**"Apareceu um monte de texto vermelho e eu me assustei"**
Respira. Vermelho não é fim do mundo, é endereço. Tira print e manda no grupo — ler erro é literalmente conteúdo da aula 1.
