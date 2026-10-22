# Aulas 11 a 20 — desenho do arco

> Documento de desenho. O plano do curso (`00-plano-do-curso.md`) continua sendo a fonte oficial;
> isto aqui é o raciocínio por trás do bloco 11–20, para revisar antes de produzir tudo.

---

## 1. O formato muda na Aula 11

A Aula 10 anunciou: acabou a Pílula de IA como bloco separado, porque da 11 em diante o curso
inteiro é sobre isso. Isso abre 15 a 18 minutos por aula. O novo molde:

| Antes (aulas 2–10) | Agora (aulas 11–20) |
|---|---|
| Conteúdo ~60 min + mão na massa ~20 min + pílula ~16 min | Conteúdo ~45 min + **mão na massa ~45 min** |
| A pílula ligava o fundamento à IA | A aula **já é** a coisa — não precisa de ponte |
| Fechamento: 3 frases da pílula | Fechamento: **"o que isso destrava"**, 6 min |

**O que entra no lugar da pílula:** mais teclado. A partir daqui o conteúdo é ferramenta, e
ferramenta só entra na cabeça usando. O bloco final de 6 minutos (*"o que isso destrava"*) mantém
a sensação de trajetória sem virar aula teórica.

**O que NÃO muda, e não deve mudar:**

- abertura pagando a dívida da aula anterior
- pelo menos um **quebrar de propósito** por aula
- as falas prontas em citação, para o instrutor ler
- blocos "se sobrar tempo" / "se faltar tempo" com ordem de corte / "nunca corte"
- a nota de estudo do instrutor (Unipds) no fim
- toda saída de código citada tem que ter sido **executada de verdade**

---

## 2. A corrente de dívidas

| Aula | Tema | Abre pagando | Deixa em aberto |
|---|---|---|---|
| **11** | Bibliotecas e `pip` | *"quem escreveu o pandas? como pego outras?"* | *"instalei uma que busca dado da internet — de onde vem esse dado?"* |
| **12** | APIs e `requests` | de onde vem o dado | *"só roda quando eu aperto o play"* |
| **13** | Automação de verdade | rodar sozinho | *"rodei duas vezes e duplicou tudo. e eu mudei algo e não sei o quê"* |
| **14** | Git e GitHub + **Projeto 2** | não saber o que mudou | *"quero que o programa decida a partir do dado, não da regra que eu escrevi"* |
| **15** | ML de verdade | regra escrita vs. aprendida *(volta à Aula 3)* | *"ok, mas como ele decide por dentro?"* |
| **16** | Primeiro modelo com `sklearn` | como decide | *"isso classifica. o ChatGPT escreve. é a mesma coisa?"* |
| **17** | LLMs por dentro | é a mesma coisa? | *"como coloco isso dentro de um programa meu?"* |
| **18** | API de LLM no código | colocar no programa | *"ele não sabe nada dos MEUS documentos"* |
| **19** | Embeddings e RAG | os meus documentos | *"só eu consigo usar, tá no meu notebook"* |
| **20** | Streamlit | só eu consigo usar | *"e quem está fora da minha rede?"* → Aula 21 |

**Retornos planejados ao início do curso** — é o que faz o curso parecer uma coisa só:

- Aula 15 reabre a Aula 3: *classificador é um `if` cujo limiar ninguém escreveu*. Agora com rigor.
- Aula 18 reabre a Aula 14: **chave de API commitada no git**. O erro mais caro do bloco, e eles acabaram de aprender git.
- Aula 12 reabre a Aula 7: resposta de API é JSON, e eles já sabem ler.
- Aula 13 reabre a Aula 9: script que roda sozinho **precisa** de `try/except`, senão falha calado.

---

## 3. Decisão que precisa ser tomada agora: a chave de API

As aulas 18 e 19 exigem chave de LLM. Pesquisei o estado atual dos tiers gratuitos:

| Provedor | Limite grátis | Cartão | Observação |
|---|---|---|---|
| **Google Gemini** (AI Studio) | não publica mais os números | **não** | **Só precisa de conta Google — que a turma já tem desde a Aula 1, por causa do Colab** |
| **Groq** | 30 req/min · 1.000 req/dia | não | Limites publicados e previsíveis; não retém dado por padrão |
| OpenRouter | 20 req/min · 50 req/dia | não | Modelos `:free` dão 429 quando o upstream lota |
| Cohere | 1.000 chamadas/mês | não | Chave de teste proíbe uso comercial |

**Recomendação: Gemini como padrão, Groq como plano B.**

O motivo é atrito, não qualidade: **zero cadastro novo**. Eles já entraram no Colab com a conta
Google na Aula 1. Groq entra como alternativa na própria aula, porque ter dois provedores ensina
que a API é intercambiável — e salva a aula se um cair no sábado de manhã.

**Precisa estar no roteiro da Aula 18, sem rodeio:**

- no tier gratuito do Gemini, **o provedor pode usar os prompts para treinamento** → não colar dado pessoal, de cliente ou do trabalho. Vira lição de privacidade, não nota de rodapé.
- a chave é **sua, pessoal, e não vai para o git** — amarra com a Aula 14.

**Ação sua, antes da Aula 18:** gere sua própria chave e rode o notebook inteiro **na sexta**. Se o
provedor estiver instável, você troca para o Groq com uma linha e ninguém perde a aula.

---

## 4. Risco real do bloco: a Aula 19

Embeddings e RAG é a aula mais difícil das dez. Como eu pretendo resolver:

- **Embedding local e gratuito** com `sentence-transformers` — não gasta chave, roda no Colab, e deixa eles verem o vetor na tela
- A demo é **buscar nos próprios documentos da turma**: jogo os roteiros das aulas 1 a 10 como base de conhecimento. O modelo responde perguntas sobre o próprio curso — isso costuma ser o momento mais impressionante possível
- Explicar similaridade **sem matemática**: dois textos parecidos têm vetores apontando para o mesmo lado. Mostro o número, não a fórmula

Se a turma travar aqui, o corte é a parte do RAG; o embedding sozinho já entrega a aula.

---

## 5. Projeto 2 (Aula 14) e Projeto Final (Aula 22)

**Projeto 2 — uma automação pessoal, publicada no GitHub.** Sugestões por perfil:
organizador de pasta de downloads · relatório automático de uma planilha própria · monitor de preço
ou de cotação · gerador de recibo em PDF.

Critério de aceite: roda sozinho, tem `README`, tem `requirements.txt`, e **sobrevive a rodar duas
vezes seguidas sem duplicar nada** (a dívida da Aula 13).

**Projeto Final (22)** nasce na Aula 19: assistente que responde sobre documentos escolhidos pelo
aluno, com interface Streamlit. Vale dizer isso na Aula 19 — saber desde cedo qual é o projeto
final muda como a pessoa assiste as últimas aulas.

---

## 6. O que fica fora destas dez

Aulas 21 a 24 seguem no plano e não entram agora: publicar na internet (21), Projeto Final (22),
POO e boas práticas (23), portfólio e caminhos (24).
