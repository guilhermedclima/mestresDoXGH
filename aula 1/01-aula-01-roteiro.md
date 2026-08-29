# Aula 1 — Roteiro minuto a minuto

**Sábado, 15/08/2026 — 10h00 às 12h00**
**Tema:** Overview — o que é programar, e o primeiro código de cada um

---

## Objetivo da aula

Ao final, cada aluno deve ter:

1. Rodado um programa escrito por ele mesmo
2. Visto um erro, lido a mensagem e consertado sozinho
3. Entendido o mapa do curso e o combinado da turma

**O que NÃO é objetivo hoje:** ensinar sintaxe. Se alguém sair sabendo o que é `if`, ótimo, mas o alvo é sair querendo voltar sábado que vem.

---

## Antes de começar (chegue 15 min antes)

- [ ] Projetor testado, slides abertos (`aula-01-slides.html`)
- [ ] Wi-Fi funcionando — **cheque isso primeiro, a aula inteira depende**
- [ ] Um Colab seu já aberto e testado
- [ ] Link do Colab encurtado e escrito no quadro (ex.: `bit.ly/xgh-aula1`)
- [ ] Grupo de mensagens criado, QR code pronto para projetar
- [ ] Plano B se a internet cair: papel e caneta para a dinâmica, e reescalar as partes práticas para 30 min quando voltar

---

## Roteiro

### 10:00 — 10:10 | Abertura e apresentações (10 min)

- Quem é você, em 2 minutos. **Inclua um fracasso**: um bug que te fez perder um dia inteiro. Isso derruba a barreira de "ele é gênio, eu não sou" mais rápido que qualquer outra coisa.
- Volta rápida na sala: nome + o que fez você vir. Uma frase cada. **Anote as respostas** — é a matéria-prima para calibrar o curso.
- Combinados da turma (slide). Leia em voz alta, é curto.

> Se a turma for grande (mais de 12), corte a volta de apresentações para nome + uma palavra.

---

### 10:10 — 10:25 | Dinâmica: "programe um humano" (15 min)

**Sem computador. Esta é a parte que faz a ficha cair.**

Você é o robô. A turma dá as instruções. Regra: você executa **literalmente** o que for dito, nunca o que foi pretendido.

Escolha uma tarefa: fazer um sanduíche, ou desenhar no quadro uma figura que só você viu.

O que vai acontecer (e é o ponto):

- "Pega o pão" → você pega o pacote inteiro, fechado
- "Passa a manteiga" → você passa o pote por cima do pão
- "Abre o pão" → você abre em qualquer lugar, não ao meio

**Fechamento da dinâmica — diga com estas palavras:**

> "Vocês sabiam exatamente o que queriam. O problema não foi inteligência de vocês, foi que o robô não adivinha. Programar é isso: transformar uma intenção óbvia para você em uma sequência de passos tão explícita que uma máquina burra e obediente consegue seguir. A dificuldade de programar quase nunca é matemática. É essa tradução."

---

### 10:25 — 10:45 | O mapa: o que é programação de verdade (20 min)

Ritmo rápido, slides, sem aprofundar. É panorama.

**1. O que acontece quando você escreve código (5 min)**
Código é texto → um programa traduz esse texto → o processador executa. Você escreve algo legível para humano; alguma coisa no meio converte para o que a máquina entende.

**2. Onde código roda (5 min)**
Peça exemplos à turma antes de mostrar: site, app do celular, caixa eletrônico, semáforo, geladeira, o carro, a maquininha de cartão, o algoritmo que escolheu o vídeo que ela viu ontem. Ponto: software está em tudo, não só em "site".

**3. Por que existem tantas linguagens e por que vamos usar Python (5 min)**
Analogia: linguagens são ferramentas, não religiões. Martelo não é melhor que chave de fenda.
Mostre o mesmo "olá mundo" em C#, em JavaScript e em Python, lado a lado (está no slide). Deixe a turma ver sozinha a diferença de cerimônia. Diga que você trabalha com .NET e JS, e que escolheu Python por ser o caminho mais curto entre a ideia e o resultado — e por ser a língua da IA.

**4. Onde a IA entra nesse mapa (5 min)**
Sem hype e sem terror. IA é software que aprende padrões a partir de dados em vez de seguir regras escritas à mão. Vocês vão chegar lá — mas depois de dominar o básico, porque sem base é só apertar botão. Mencione que você está estudando isso na faculdade e vai trazer para a turma.

---

### 10:45 — 10:55 | Pausa (10 min)

Aproveite para: passar o QR do grupo, resolver quem não conseguiu abrir o Colab, conversar individualmente com quem parecia perdido.

---

### 10:55 — 11:20 | Mão na massa 1: o primeiro programa (25 min)

**Todo mundo abre o Google Colab.** (`colab.research.google.com` → Novo notebook. Só precisa de conta Google.)

Faça você primeiro, projetado, e só depois solte a turma. Uma linha de cada vez.

```python
print("Olá, mundo")
```

Rode. Comemore de verdade — para metade da sala isso é a primeira vez.

```python
print("Meu nome é Guilherme")
print("Eu tenho 30 anos")
```

Agora o problema: e se eu quiser usar meu nome em dez lugares?

```python
nome = "Guilherme"
idade = 30

print("Meu nome é " + nome)
print("Eu tenho " + str(idade) + " anos")
```

**Explique variável assim:** uma caixa com etiqueta. Você guarda um valor e usa pela etiqueta. Se trocar o que está na caixa, tudo que usa a etiqueta muda junto.

Agora o programa conversa:

```python
nome = input("Qual é o seu nome? ")
print("Prazer, " + nome + "! Bem-vindo aos Mestres do XGH.")
```

**Exercício da turma (10 min):** cada um faz o próprio cartão de apresentação — nome, cidade, uma coisa que quer construir. Circule pela sala. Não conserte o código de ninguém: pergunte "o que você acha que essa linha faz?" e deixe a pessoa chegar sozinha.

---

### 11:20 — 11:45 | Mão na massa 2: decisão e o primeiro erro (25 min)

**Parte A — o programa decide (10 min)**

```python
idade = int(input("Qual sua idade? "))

if idade >= 18:
    print("Você pode tirar habilitação")
else:
    print("Ainda não pode tirar habilitação")
```

Dois pontos para destacar: os **dois pontos** no fim da linha e a **indentação** (o espaço antes do `print`). Em Python o espaço não é enfeite, é o que diz o que está dentro do quê.

**Parte B — quebrar de propósito (15 min). Esta é a parte mais importante da aula.**

Peça para todos apagarem a indentação de uma linha e rodarem:

```
IndentationError: expected an indented block after 'if' statement on line 3
```

**Ensine a ler a mensagem, em voz alta, na frente deles:**

- Última linha = o tipo do problema (`IndentationError`)
- O número da linha = onde olhar
- O texto = o que ele esperava e não achou

> "Erro vermelho não é bronca. É o computador te dizendo, com endereço e tudo, onde ele travou. Quem aprende a ler isso vira programador. Quem entra em pânico e apaga tudo, não."

Quebre mais duas vezes, deixando a turma prever o que vai acontecer antes de rodar:

```python
print("oi"      # falta fechar o parêntese → SyntaxError
idade = int("abc")   # texto que não é número → ValueError
```

Termine com: **"errar em aula é de graça. É para errar aqui."**

---

### 11:45 — 11:55 | O mapa do curso e como estudar (10 min)

- Mostre o roadmap por módulos (slide). Fundamentos → dados → interface → IA. Sem prazo travado; anda quando a turma anda.
- **Como estudar:** 20 minutos por dia batem 3 horas no domingo. Explique por quê — o cérebro fixa por repetição espaçada, não por volume. Estudar 3h de uma vez sem repetir é o mesmo que não estudar.
- **A regra do "explica para alguém":** se você não consegue explicar, você não entendeu ainda. Vale para eles e vale para você.
- Sobre IA para escrever código: ela vai ser usada no curso, mas depois. Agora atrapalha — é como usar calculadora antes de saber somar. E deixe claro por quê, sem soar proibitivo: quem só cola resposta de IA fica sem conseguir julgar quando ela erra, e ela erra.

---

### 11:55 — 12:00 | Tarefa e fechamento (5 min)

**Tarefa da semana (mande no grupo hoje mesmo):**

1. Responder o formulário: *por que você quer aprender a programar e o que gostaria de conseguir fazer daqui a 3 meses?* (você vai usar isso para calibrar o curso)
2. Terminar o cartão de apresentação no Colab e mandar print no grupo
3. Fazer um programa que pergunta dois números e mostra a soma — **dica:** vai dar errado na primeira tentativa, e isso é de propósito. `input` devolve texto, e texto + texto não soma, gruda. Descubram por quê.
4. Ler o guia de instalação — na aula 2 a gente instala Python na máquina de vocês

**Fechamento (30 segundos, olhando para a sala):**

> "Hoje vocês escreveram um programa. Já são programadores — ruins ainda, todo mundo começa ruim, eu comecei ruim. A diferença entre quem fica e quem sai não é talento, é aparecer no sábado seguinte. Até sábado."

---

## Se sobrar tempo (bolso de reserva)

- Mostrar um programa seu de verdade, em .NET ou JS, rodando. Não para entenderem — para verem onde isso chega.
- `import random` e um sorteio de número. Abre a porta de "existe código pronto que você usa".
- Responder perguntas soltas. Anote as que você não soube responder.

## Se faltar tempo

Corte nesta ordem: (1) o bolso de reserva, (2) a Parte A da mão na massa 2 — os erros valem mais que o `if`, (3) o detalhamento do roadmap, deixando só "fundamentos → dados → IA".

**Nunca corte:** a dinâmica do robô, o primeiro `print` de cada aluno, e a tarefa de casa.

---

## Depois da aula — 15 minutos, no mesmo dia

- [ ] Mandar no grupo: link do Colab da aula, guia de setup, tarefa escrita
- [ ] Anotar em `duvidas-da-turma.md` as perguntas que te pegaram
- [ ] Anotar quem pareceu perdido — puxe essa pessoa individualmente durante a semana. É nas duas primeiras semanas que se decide quem desiste.
