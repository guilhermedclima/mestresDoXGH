# Mestres do XGH

Curso de programação do zero, **todo sábado das 10h às 12h**, para uma turma de até 6 pessoas sem conhecimento prévio.

> **XGH** é *eXtreme Go Horse*, a piada da gambiarra. A gente aprende a gambiarra **e** aprende por que ela cobra o preço depois.

**Linguagem:** Python · **Ambiente:** Google Colab (instalação local entra na Aula 8) · **Início:** 15/08/2026

---

## Como usar este repositório

Cada aula fica na sua própria pasta e gera três ou quatro peças, numeradas em sequência contínua:

| Peça | Para quem | O que é |
|---|---|---|
| `*-roteiro.md` | instrutor | Roteiro minuto a minuto, com o que cortar se faltar tempo e o que nunca cortar |
| `*-slides.html` | sala | Slides self-contained — abra no navegador, nada para instalar |
| `*-notebook.ipynb` | alunos | Notebook para subir no Colab: exemplos, exercícios e a demo da Pílula de IA |
| `*-guia-*.md` | alunos | Guias avulsos, quando a aula precisa |

**Slides — atalhos:** `←` `→` navegam · `T` alterna tema claro/escuro (use o claro em sala clara) · `F` tela cheia · no celular, deslize.

**Notebooks:** abra em [colab.research.google.com](https://colab.research.google.com) → *Upload* → escolha o `.ipynb`. `Shift + Enter` roda a célula.

---

## Índice das aulas

| # | Aula | Tema | Material | Status |
|---|---|---|---|---|
| 1 | 15/08/2026 | Overview + primeiro código | [roteiro](aula%201/01-aula-01-roteiro.md) · [slides](aula%201/02-aula-01-slides.html) · [guia de setup](aula%201/03-guia-setup-alunos.md) | ✅ dada |
| 2 | 22/08/2026 | Tipos, conversão e f-string | [roteiro](aula%202/04-aula-02-roteiro.md) · [slides](aula%202/05-aula-02-slides.html) · [notebook](aula%202/06-aula-02-notebook.ipynb) | ✅ dada |
| 3 | 29/08/2026 | Condicionais: `if` / `elif` / `else` | [roteiro](aula%203/07-aula-03-roteiro.md) · [slides](aula%203/08-aula-03-slides.html) · [notebook](aula%203/09-aula-03-notebook.ipynb) | ✅ preparada |
| 4 | 05/09/2026 | Repetição: `for`, `while`, `range` | [roteiro](aula%204/10-aula-04-roteiro.md) · [slides](aula%204/11-aula-04-slides.html) · [notebook](aula%204/12-aula-04-notebook.ipynb) | ✅ preparada |
| 5 | 12/09/2026 | Listas e strings | — | ⬜ a preparar |

O plano completo do curso — as 24 aulas, os módulos e o porquê de cada decisão — está em **[`00-plano-do-curso.md`](00-plano-do-curso.md)**.

---

## Os cinco princípios

1. **Ninguém sai de uma aula sem ter rodado código próprio.** Slide é apoio; a aula acontece no teclado do aluno.
2. **Erro é conteúdo, não acidente.** Toda aula tem um "quebrar de propósito".
3. **Cada aula entrega algo que funciona.** "Funcionou" é o combustível de um iniciante.
4. **Vocabulário antes de sintaxe.** Quem tem o nome das coisas consegue pesquisar sozinho.
5. **20 minutos por dia batem 3 horas no domingo.**

---

## Pílula de IA

Bloco fixo de ~16 minutos no fim de toda aula, ligando o fundamento daquele dia a como a IA usa aquilo.

**A regra que faz funcionar:** a pílula sempre usa o conteúdo **daquele mesmo dia**. Nunca é um "extra sobre IA" — é a resposta para *"por que eu aprendi isso hoje"*.

| Aula | Fundamento | Pílula |
|---|---|---|
| 2 | Tipos e conversão | Todo modelo só entende número — texto vira número |
| 3 | Condicionais | Um classificador é um `if` que ninguém escreveu |
| 4 | Repetição | Treinar é repetir milhões de vezes ajustando um pouquinho |
| 5 | Listas e strings | Texto virando pedaços: isso é tokenização |
| 6 | Funções | Um modelo é uma função: entra dado, sai resposta |
| 7 | Dicionários e JSON | Toda API de IA fala JSON |
| 8 | Arquivos | Sem dado guardado não existe modelo |
| 9 | Erros e debug | Quando a IA erra, quem depura é você |
| 10 | pandas | 80% do trabalho de IA é limpar tabela |

Da Aula 11 em diante o curso já está dentro do tema.

---

## As dívidas abertas

O curso tem um padrão narrativo: **cada aula abre pagando uma pergunta deixada na anterior.** Isso ensina que as ferramentas se acumulam e que as perguntas da turma são levadas a sério.

| Pergunta deixada em aberto | Onde é paga |
|---|---|
| `"2" + "2"` dá `22` — por quê? | Aula 2 |
| A sobra negativa da calculadora | Aula 3, abertura |
| "E se a pessoa digitar `abc`?" | Aula 3, parcialmente (`.isdigit()`) |
| "E se eu tivesse 200 faixas? Copiaria 200 `elif`?" | Aula 4, abertura e mão na massa |
| "O `for` só anda do começo ao fim — e se eu quiser o terceiro? ordenar? o maior?" | **Aula 5** |
| O `abc` resolvido de verdade, com `try/except` | **Aula 9 — prometido, cumprir** |

---

## Estrutura

```
mestresDoXGH/
├── 00-plano-do-curso.md      # o plano completo: 24 aulas, 6 módulos
├── aula 1/                   # 01, 02, 03
├── aula 2/                   # 04, 05, 06
├── aula 3/                   # 07, 08, 09
├── aula 4/                   # 10, 11, 12
└── README.md
```

A numeração dos arquivos é **sequencial e contínua** entre as pastas — ela conta a ordem em que o material foi produzido, não a posição dentro da aula.
