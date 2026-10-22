# Mestres do XGH — Plano do Curso

**Formato:** todo sábado, 10h às 12h
**Início:** sábado, 15/08/2026
**Turma:** até 6 alunos, pouco ou nenhum conhecimento prévio
**Instrutor:** Guilherme (background em .NET e JavaScript/web, cursando Eng. de IA Aplicada na Unipds)

> **Revisão 2 — 19/08/2026, depois da Aula 1.** Três coisas mudaram e estão refletidas abaixo:
> 1. O formulário mostrou que o interesse da turma é **IA e automação** → os módulos foram reordenados para chegar lá mais cedo, e cada aula ganhou um bloco fixo chamado **Pílula de IA**.
> 2. **A turma segue no Colab** por enquanto. A instalação local foi adiada para a Aula 8, quando arquivos passarem a fazer diferença de verdade.
> 3. Turma pequena (até 6): dá para corrigir código individualmente e deixar os exercícios mais abertos.

---

## 1. Qual linguagem ensinar — e por quê

**Decidido: Python como espinha dorsal, com interface web só no fim.**

Comparando com o que você já domina:

| | .NET / C# | JavaScript | Python |
|---|---|---|---|
| Primeiro "olá mundo" | class + namespace + `static void Main` | precisa de HTML ou console do navegador | `print("olá")` — uma linha |
| Setup no dia 1 | SDK + IDE pesada | navegador (fácil) | Google Colab, zero instalação |
| Mensagens de erro | verbosas, cheias de stack | silenciosas e confusas (`undefined`, `NaN`) | legíveis, apontam a linha |
| Armadilhas para iniciante | tipagem estática, compilação | `==` vs `===`, `this`, assíncrono, callbacks | indentação (só isso, e é fácil) |
| Casa com seu curso de IA | não | pouco | **sim, diretamente** |

O ponto decisivo não é a linguagem em si — é a **distância entre o aluno escrever algo e ver o resultado**. Em Python essa distância é de segundos, sem nada instalado.

Com o interesse da turma confirmado em IA, a escolha ficou ainda mais clara: é a linguagem em que a coisa toda acontece.

**Onde sua experiência entra em cheio:** o Módulo 5 (Streamlit, publicar, interface) e o Módulo 6 (POO, boas práticas) — aí você ensina do lugar onde é forte.

---

## 2. Princípios do curso

1. **Ninguém sai de uma aula sem ter rodado código próprio.** Slide é apoio; a aula acontece no teclado do aluno.
2. **Erro é conteúdo, não acidente.** Quebrar de propósito e ler a mensagem é habilidade que se ensina.
3. **Cada aula entrega algo que funciona.** "Funcionou" é o combustível de um iniciante.
4. **Vocabulário antes de sintaxe.** Quem tem o nome das coisas consegue pesquisar sozinho — que é o objetivo real.
5. **20 minutos por dia batem 3 horas no domingo.** Repetir em toda aula.

**Sobre o nome da turma:** XGH ("eXtreme Go Horse") é a piada da gambiarra. Use como humor e como contraste: a gente aprende a gambiarra *e* aprende por que ela cobra o preço depois.

---

## 2.1 Pílula de IA — o bloco fixo de toda aula

**Últimos 15 minutos de cada sábado.** A turma veio por IA; se ela só ouvir falar de IA na aula 15, metade sai antes.

A regra que faz esse bloco funcionar: **a pílula sempre usa o conteúdo daquele mesmo dia.** Não é um "extra sobre IA", é a resposta para "por que eu aprendi isso hoje".

| Aula | Fundamento do dia | Pílula de IA |
|---|---|---|
| 2 | Tipos e conversão | Todo modelo só entende número. Texto vira número — é `int()` levado a sério |
| 3 | Condicionais | O que um classificador faz é decidir. `if` na mão vs. limiar aprendido |
| 4 | Repetição | Treinar é repetir milhões de vezes ajustando um pouquinho |
| 5 | Listas e strings | Um texto virando lista de pedaços: isso é tokenização |
| 6 | Funções | Um modelo é uma função: entra dado, sai resposta |
| 7 | Dicionários e JSON | Toda API de IA fala JSON. Vocês já sabem ler |
| 8 | Arquivos | Dado é o insumo. Sem dado guardado não existe modelo |
| 9 | Erros e debug | Quando a IA erra, quem depura é você |
| 10 | pandas | 80% do trabalho de IA é limpar tabela, não treinar modelo |

Da Aula 11 em diante o curso já está dentro do tema e a pílula deixa de ser necessária como bloco separado.

**Por que isso também serve a você:** cada pílula te obriga a explicar em 15 minutos, sem jargão, um conceito que você está vendo com profundidade na Unipds. Se você não conseguir montar a pílula, achou um buraco no seu próprio entendimento — e achou barato.

---

## 3. Roadmap por módulos

Sem prazo fechado — avança quando a turma acompanha. A numeração é referência, não meta.

### Módulo 0 — Ponto de partida ✅

| Aula | Tema | Status |
|---|---|---|
| 1 | Overview + primeiro código no Colab | **Feita em 15/08** — rodou no tempo |

### Módulo 1 — Fundamentos (aulas 2 a 6)

| Aula | Tema | O aluno sai sabendo |
|---|---|---|
| 2 | **Tipos, conversão e operadores** ✅ | Por que `"2" + "2"` dá `22`, e como consertar |
| 3 | **Condicionais:** `if`, `elif`, `else`, booleanos | Fazer o programa decidir |
| 4 | Repetição: `for`, `while`, `range` | Fazer o computador repetir sem copiar e colar |
| 5 | Listas e strings | Trabalhar com coleções e texto |
| 6 | Funções + **Projeto 1** | Organizar código em pedaços reutilizáveis |

**Projeto 1:** jogo de adivinhação, quiz, calculadora de IMC com faixas, ou gerador de senha.

### Módulo 2 — Dados (aulas 7 a 10)

| Aula | Tema | O aluno sai sabendo |
|---|---|---|
| 7 | Dicionários e JSON | Estruturar dados com significado — e ler resposta de API |
| 8 | Arquivos (`.txt`, `.csv`) + **instalação local** | Dados que sobrevivem; e rodar código na própria máquina |
| 9 | Erros e depuração: `try/except`, ler stack trace | Não travar no primeiro erro vermelho |
| 10 | `pandas`: ler, filtrar, agrupar | Mexer em planilha de verdade com código |

> A instalação local entra na Aula 8 porque é aí que ela passa a valer a pena: no Colab, arquivo salvo some quando a sessão fecha. Antes disso, instalar é atrito sem retorno.

### Módulo 3 — Automação e mundo real (aulas 11 a 14)

| Aula | Tema | O aluno sai sabendo |
|---|---|---|
| 11 | Bibliotecas e `pip` ✅ | Usar código que outras pessoas escreveram |
| 12 | APIs e `requests` ✅ | Buscar dados da internet dentro do programa |
| 13 | Automação de verdade ✅ | Um script que faz sozinho algo chato que você faria na mão |
| 14 | Git e GitHub + **Projeto 2** ✅ | Versionar e publicar o próprio código |

**Projeto 2:** uma automação pessoal — organizar arquivos, monitorar preço, relatório automático de uma planilha.

### Módulo 4 — IA aplicada (aulas 15 a 19) — *seu reforço direto da Unipds*

| Aula | Tema | O aluno sai sabendo |
|---|---|---|
| 15 | Machine learning de verdade: dados, features, treino/teste | Separar IA de mágica |
| 16 | Primeiro modelo com `scikit-learn` | Treinar e avaliar um classificador simples |
| 17 | LLMs por dentro: token, contexto, temperatura | Por que o modelo erra e como isso muda o uso |
| 18 | Chamar uma API de LLM no código | Colocar IA dentro de um programa próprio |
| 19 | Embeddings e RAG | Fazer o modelo responder sobre os *seus* documentos |

### Módulo 5 — Entrega (aulas 20 a 22)

| Aula | Tema | O aluno sai sabendo |
|---|---|---|
| 20 | Streamlit: app web sem front-end | Transformar script em aplicação com botão e campo |
| 21 | Publicar na internet | Mandar um link e a pessoa usar |
| 22 | **Projeto Final** | Uma aplicação com IA, publicada, no portfólio |

### Módulo 6 — Próximo nível (aulas 23 e 24)

| Aula | Tema |
|---|---|
| 23 | Introdução a objetos (POO) e boas práticas |
| 24 | Portfólio, currículo, comunidades e caminhos possíveis |

---

## 4. Como isso reforça o SEU estudo

Ensinar é a forma mais eficiente de descobrir o que você só *acha* que entendeu. Explicar para alguém sem base te obriga a chegar no mecanismo — não dá para se esconder atrás de jargão.

**Rotina sugerida, 3 momentos por semana:**

- **Segunda ou terça (~40 min):** escolha o tema do sábado. Escreva em uma frase, sem jargão nenhum, o que ele resolve. Se você não conseguir escrever essa frase, você ainda não entendeu — e esse é o sinal mais útil que existe.
- **Quinta (~40 min):** prepare **uma** analogia, **um** erro proposital e **a pílula de IA** do dia. A pílula é a parte que mais te obriga a estudar de verdade.
- **Sábado depois da aula (15 min):** anote em `duvidas-da-turma.md` as perguntas que você não soube responder. Esse arquivo é o mapa exato dos seus buracos, feito por gente sem vício de vocabulário para disfarçar.

---

## 5. Combinados com a turma

- Pergunta idiota não existe. Pergunta não feita vira desistência três aulas depois.
- Errar em aula é de graça. É para errar aqui.
- Ninguém copia e cola sem entender — se copiou, explica para o vizinho o que a linha faz.
- Grupo de mensagens para dúvidas durante a semana. Dúvida guardada apodrece.
- Ritmo: 20 minutos por dia.

---

## 6. Estado do curso

> **Revisão 3 — 04/09/2026.** As aulas 4 a 10 estão preparadas de uma vez, fechando os Módulos 1 e 2.
> A corrente de dívidas entre elas está montada; cada roteiro tem um bloco marcado de abertura para
> trocar pela pergunta que a turma trouxer de verdade.

- [x] Aula 1 dada (15/08) — rodou no tempo
- [x] Formulário respondido — interesse confirmado em **IA e automação**
- [x] Aula 2 dada (22/08) — rodou no tempo, mesmo ritmo
- [x] **Pílula de IA validada:** engajou bem na Aula 2. Confirmada como bloco fixo, e ampliada para ~16 min a partir da Aula 3, com demo rodando na tela sempre que der
- [x] Aula 3 dada (29/08) — condicionais
- [x] Aula 4 preparada (05/09) — repetição
- [x] Aula 5 preparada (12/09) — listas e strings
- [x] Aula 6 preparada (19/09) — funções + **Projeto 1: jogo de adivinhação**
- [x] Aula 7 preparada (26/09) — dicionários e JSON
- [x] Aula 8 preparada (03/10) — arquivos + **instalação local** (quatro peças, com guia para os alunos)
- [x] Aula 9 preparada (10/10) — erros e debug. **Paga a dívida do `abc`, aberta desde a Aula 2**
- [x] Aula 10 preparada (17/10) — pandas. **Fecha o Módulo 2 e é a última pílula separada**
- [x] Repositório no GitHub: `github.com/guilhermedclima/mestresDoXGH`
- [ ] Grupo de mensagens ativo com o material da Aula 1
- [ ] `duvidas-da-turma.md` iniciado
- [x] Aula 11 preparada (24/10) — bibliotecas e `pip`. **Primeira do formato novo: sem pílula, mais teclado**
- [x] Aula 12 preparada (31/10) — APIs e `requests`. **Status 200 não quer dizer que deu certo**
- [x] Aula 13 preparada (07/11) — automação. **Idempotência: 13 arquivos entram, 12 saem**
- [x] Aula 14 preparada (14/11) — Git e GitHub. **O git lembra a chave para sempre** — semente da Aula 18
- [x] Aula 15 preparada (21/11) — ML de verdade, **abre o Módulo 4**. Dummy bate 0.95 num problema 95/5
- [ ] **Próxima a preparar: Aula 16 — primeiro modelo com `scikit-learn`**
- [ ] Ver `ARCO-aulas-11-20.md` — desenho do bloco 11 a 20, com a decisão da chave de API

### A corrente de dívidas montada

Cada aula abre pagando a pergunta deixada na anterior:

| Aula | Abre pagando | Deixa em aberto |
|---|---|---|
| 4 | os 200 `elif` | "o `for` só anda do começo ao fim — e o terceiro? ordenar? o maior?" |
| 5 | o maior salário (na mão, antes do `max()`) | "escrevi o mesmo bloco em três lugares" |
| 6 | o bloco repetido | "a lista guarda ordem, mas não guarda **nome**" |
| 7 | o par nome/valor | "fechei o Colab e perdi tudo" |
| 8 | a perda do dado | "o programa morre no primeiro imprevisto" |
| 9 | esse imprevisto — **e o `abc` da Aula 2** | "o CSV tem 5.000 linhas" |
| 10 | as 5.000 linhas | "quem escreveu o pandas? como pego outras bibliotecas?" |
| 11 | essa pergunta | "instalei uma que busca dado da internet — de onde vem esse dado?" |
| 12 | de onde vem o dado | "só roda quando eu aperto o play" |
| 13 | rodar sozinho | "rodei duas vezes e duplicou. e mudei algo e não sei o quê" |
| 14 | não saber o que mudou | "quero que o programa decida a partir do dado" |
| 15 | regra escrita vs. aprendida | "ok, mas como ele decide por dentro?" |
