# Aula 12 — Roteiro minuto a minuto

**Sábado, 31/10/2026 — 10h00 às 12h00**
**Tema:** APIs e `requests` — buscar dado que não é seu
**Ambiente:** VS Code na máquina do aluno *(Colab para quem preferir)*
**Turma:** até 6 alunos

---

## Objetivo da aula

Ao final, cada aluno deve:

1. Saber explicar o que é uma **API** com as próprias palavras — um site feito para **programa** ler, não para pessoa ver
2. Fazer uma requisição com `requests.get` e ler o `status_code`
3. Transformar a resposta em dicionário com `.json()` e **navegar nela** (é a Aula 7 de volta)
4. Passar **parâmetros na URL** com `params=`
5. Saber que **status 200 não quer dizer que deu certo** — e checar o conteúdo, sempre
6. Proteger a chamada com `try/except` e `timeout` *(Aula 9 virando obrigação)*
7. Entender o que é **chave de API** e por que ela existe

**A dívida que abre a aula:** *"instalei uma biblioteca que busca dado da internet — de onde vem esse dado? Quem está do outro lado? E por que de graça?"*

| Gancho | De onde veio | Onde é pago |
|---|---|---|
| "De onde vem esse dado? Quem está do outro lado?" | Fim da Aula 11 | Abertura, 10h00 — o JSON do ViaCEP projetado no navegador |
| A biblioteca que cada um achou sozinho | Tarefa 3 da Aula 11 | Abertura — recolha antes de começar |
| JSON é dicionário, e dicionário vocês navegam | Aula 7 | 10h28 — `.json()` |
| `try/except` e leitura de stack trace | Aula 9 | 11h00 — 💥 nº 2, o `.json()` explodindo |
| "número errado com cara de certo" | Aula 10 | 11h00 — 💥 nº 1, **o coração da aula** |
| `pip install` e nome de import | Aula 11 | 10h12 — `pip install requests` / `import requests` |

> **A frase que amarra a aula:**
> **status 200 não quer dizer que deu certo.**

---

## Antes de começar

- [ ] Notebook testado (`37-aula-12-notebook.ipynb`) e link curto no quadro
- [ ] Slides abertos (`36-aula-12-slides.html`)
- [ ] **Na sexta, testar as três URLs no navegador** — se o ViaCEP estiver fora do ar no sábado, a aula inteira cai:
  - `https://viacep.com.br/ws/11013001/json/`
  - `https://viacep.com.br/ws/99999999/json/`
  - `https://api.agify.io?name=guilherme`
- [ ] **Duas abas prontas no navegador, antes da aula:** o site dos Correios (busca de CEP, feito para pessoa) e o ViaCEP em JSON (feito para programa). A aula abre com essas duas lado a lado
- [ ] Rodar `pip install requests` na sua máquina para saber quanto demora na rede da sala
- [ ] **Plano B se a internet da sala cair:** os slides têm **todas as respostas reais impressas** — o 200 do ViaCEP, o `{'erro': 'true'}`, o 400 em HTML, o 429, o `ReadTimeout`. Dá para dar a aula inteira pelos slides e deixar o teclado para a semana. Não é a mesma coisa, mas salva o sábado. *(Se quiser blindar: na sexta, salve as três respostas em `respostas.json` com `json.dump` e leve o arquivo — vira a mesma aula lendo de disco, com o `json` da Aula 7.)*
- [ ] Confira o CEP da sua casa e o de um ou dois alunos — ter CEP de verdade da turma na hora da mão na massa muda o clima

---

## Roteiro

### 10:00 — 10:12 | A dívida: o que está do outro lado (12 min)

**Recolha a tarefa (4 min).** Pergunte quem achou biblioteca sozinho e **o que digitou na busca**. Leia
duas ou três das buscas em voz alta. É o melhor diagnóstico que existe de quão autônoma a turma está.

**Agora a dívida (8 min).** Não comece explicando. Projete **duas abas** e deixe a turma olhar.

Aba 1 — o site dos Correios, busca de CEP. Aba 2 — `viacep.com.br/ws/11013001/json/`:

```json
{
  "cep": "11013-001",
  "logradouro": "Avenida João Pessoa",
  "bairro": "Centro",
  "localidade": "Santos",
  "uf": "SP",
  "ddd": "13"
}
```

> "As duas abas têm **o mesmo dado**. Uma está bonita, com menu, logo e botão. A outra está feia e é
> isso." *(aponte o JSON)* "Qual das duas vocês preferem?"

Alguém vai dizer a primeira. Então a virada:

> "Vocês preferem a primeira porque vocês são **gente**. Mas o programa de vocês não é gente. Para o
> programa, a primeira aba é um pesadelo e a segunda é um presente embrulhado."

E agora o nome:

> "**Isso é uma API.** É um endereço na internet que devolve dado puro, no formato que programa lê.
> Não tem menu, não tem cor, não tem botão. Tem o dado e mais nada."

Duas frases para o quadro — e repita as duas durante a aula inteira:

> "Site é para o olho. **API é para o `for`**."

> "Sábado passado vocês instalaram biblioteca que lê arquivo do **seu** computador. Hoje vocês vão
> ler dado do computador **de outra pessoa**."

**Quem está do outro lado.** Vale 1 minuto, porque é literalmente a pergunta que ficou:

> "Do outro lado do ViaCEP tem um servidor que alguém paga. Do outro lado da API de cotação tem uma
> empresa. Do outro lado da API da OpenAI tem um data center. **Nenhum deles é mágica, e nenhum deles
> é de graça para quem mantém.** Guardem isso — na última hora da aula a gente volta nesse ponto."

---

### 10:12 — 10:28 | A primeira chamada: `requests.get` (16 min)

Amarre com sábado passado antes de digitar:

```
pip install requests
```

```python
import requests
```

> "Repararam? **O nome de instalação e o nome de import batem.** Depois da tabela de sábado passado,
> aproveitem — é a exceção boazinha."

**A chamada mais curta do curso.** Comece pela Agify, que é pequena e engraçada:

```python
import requests

resposta = requests.get("https://api.agify.io?name=guilherme")
print(resposta)
```

```
<Response [200]>
```

> "Três linhas. Essa é a aula inteira em três linhas — o resto é cuidado."

Pare aqui e faça uma pergunta honesta: *"o que acabou de acontecer na rede?"* Deixe responderem.
Depois descreva, nesta ordem, devagar:

1. o Python abriu uma conexão com o servidor `api.agify.io`
2. mandou um pedido: *"me dá o que está em `/?name=guilherme`"*
3. o servidor respondeu com um **número de status** e um **corpo de texto**
4. o `requests` embrulhou as duas coisas num objeto e devolveu para a variável

> "Esse objeto **não é** o dado. É o **envelope**. Dentro dele tem o status e tem o corpo. A gente vai
> abrir os dois."

**O corpo cru:**

```python
print(resposta.text)
```

```
{"count":5405,"name":"guilherme","age":34}
```

> "Olhem bem: chaves, dois-pontos, vírgula. **Isso é JSON** — a Aula 7. Vocês já sabem ler isso."

Deixe a brincadeira acontecer:

> "A API olhou 5405 pessoas chamadas Guilherme e apostou que eu tenho 34 anos. Errou. Mas repare no
> que ela te deu junto: o `count`. Ela te disse **em quantos dados ela se baseou**. Isso é uma API
> honesta — guardem esse detalhe para o Módulo 4."

Cada aluno roda com o **próprio nome**. Vale os dois minutos: a sala ri e o conceito entra.

---

### 10:28 — 10:40 | `status_code` e `.json()` (12 min)

**O status primeiro.** Projete a tabela e deixe no quadro a aula inteira:

| Status | Quer dizer | Em português de gente |
|---|---|---|
| **200** | OK | o servidor entendeu e respondeu |
| **400** | Bad Request | você pediu errado |
| **401 / 403** | Unauthorized / Forbidden | você não provou quem é, ou não pode |
| **404** | Not Found | esse endereço não existe |
| **429** | Too Many Requests | você pediu demais, espere |
| **500** | Internal Server Error | **o problema é do outro lado**, não seu |

> "Decorem três: **200** deu certo, **4xx** o erro é seu, **5xx** o erro é dele. Isso já resolve 95%
> dos casos."

```python
print(resposta.status_code)
```

```
200
```

**Agora o `.json()`** — e aqui é o reencontro com a Aula 7:

```python
dados = resposta.json()
print(type(dados))
print(dados["age"])
```

```
<class 'dict'>
34
```

Pare a turma. Esse é um daqueles momentos em que o curso inteiro fecha:

> "Olhem o tipo. **`dict`.** É dicionário. É a mesma coisa da Aula 7, com as mesmas chaves e os mesmos
> colchetes. Vocês não precisam aprender nada novo para usar API — vocês já sabem navegar nisso desde
> setembro."

E a diferença entre `.text` e `.json()`, que confunde todo mundo no começo:

| | o que é | o que você faz com ele |
|---|---|---|
| `resposta.text` | **string** | `print`, procurar pedaço, salvar em arquivo |
| `resposta.json()` | **dicionário ou lista** | `dados["chave"]`, `for`, `.get()` |

> "`.text` é o papel. `.json()` é o papel já lido e organizado. É o mesmo pulo do `json.loads` da
> Aula 7 — só que o `requests` já faz para você."

---

### 10:40 — 10:50 | Parâmetros na URL (10 min)

Volte à URL da Agify e aponte o `?`:

```
https://api.agify.io?name=guilherme
                    ↑         ↑
                    │         └── o valor
                    └── começa a lista de parâmetros
```

> "Aquele `?` separa o endereço dos **parâmetros**. Depois dele vem `chave=valor`, e `&` para juntar
> mais de um. É assim que **uma** URL atende mil pedidos diferentes."

Mostre os dois jeitos de montar — e a razão de preferir o segundo:

```python
# jeito 1: colando na mão (funciona, mas quebra fácil)
nome = "maria"
resposta = requests.get("https://api.agify.io?name=" + nome)

# jeito 2: deixando o requests montar
resposta = requests.get("https://api.agify.io",
                        params={"name": "maria", "country_id": "BR"})
print(resposta.url)
print(resposta.json())
```

```
https://api.agify.io/?name=maria&country_id=BR
{'count': 3622, 'name': 'maria', 'age': 59, 'country_id': 'BR'}
```

> "O `params=` monta a URL por você. Por que isso importa? Porque no dia em que o valor tiver um
> espaço, um acento ou um `&`, o jeito 1 monta uma URL quebrada e você vai perder uma hora
> procurando o erro no lugar errado. O `requests` sabe escapar; você não precisa saber."

Repare no `country_id`: o mesmo nome, outro país, outra resposta.

> "Mesma API, mesmo endereço, **parâmetro diferente, resposta diferente**. Maria no Brasil tem 59.
> Guarde esse raciocínio: ler a documentação de uma API é, quase sempre, descobrir **que parâmetros
> ela aceita**."

E o ViaCEP, que é o outro padrão comum, onde o parâmetro vai no **caminho**:

```
https://viacep.com.br/ws/11013001/json/
                         ↑         ↑
                         CEP       formato
```

> "Nem toda API usa `?`. O ViaCEP coloca o CEP **no meio do endereço**. Não tem regra universal — tem
> documentação. De novo: é a habilidade de sábado passado."

---

### 10:50 — 11:00 | ☕ Pausa (10 min)

---

### 11:00 — 11:26 | 💥 Os quatro jeitos de quebrar (26 min)

Abra assim, porque define a hora seguinte:

> "`requests.get` funciona na primeira tentativa. É o resto que vai te morder. Destes quatro, **o
> primeiro é o mais importante que eu vou falar hoje** — e é o mais perigoso dos doze sábados, porque
> ele não parece erro."

#### 💥 nº 1 — o erro que não é erro (10 min) · **o coração da aula**

Monte com calma. Primeiro o caso que funciona, para ter referência:

```python
r = requests.get("https://viacep.com.br/ws/11013001/json/")
print(r.status_code)
print(r.json())
```

```
200
{'cep': '11013-001', 'logradouro': 'Avenida João Pessoa', 'complemento': 'até 197 - lado ímpar',
 'unidade': '', 'bairro': 'Centro', 'localidade': 'Santos', 'uf': 'SP', 'estado': 'São Paulo',
 'regiao': 'Sudeste', 'ibge': '3548500', 'gia': '6336', 'ddd': '13', 'siafi': '7071'}
```

Agora **pergunte antes de rodar.** Essa pergunta é o bloco inteiro:

> "Eu vou pedir um CEP que **não existe**: `99999999`. Oito dígitos, formato certo, mas não existe
> endereço nenhum com esse CEP. Que status vocês apostam que vem?"

Deixe a turma apostar. **Praticamente todo mundo diz 404.** Anote os palpites no quadro. Então rode:

```python
r = requests.get("https://viacep.com.br/ws/99999999/json/")
print(r.status_code)
print(r.json())
```

```
200
{'erro': 'true'}
```

Silêncio. Deixe o silêncio durar.

> "**Duzentos.** O servidor te disse que deu tudo certo. E dentro do envelope não tem endereço nenhum."

> "Se o seu programa fosse assim — `if r.status_code == 200: usa o dado` — ele teria usado. E teria
> escrito no relatório, ou no banco, ou no e-mail do cliente, **um endereço vazio.** Sem erro. Sem
> aviso. Sem stack trace."

**Agora amarre com a Aula 10.** Essa é a frase que fecha o arco das três últimas aulas:

> "Isso é o mesmo monstro de duas aulas atrás, de roupa nova. Na Aula 10, o pandas calculou média
> ignorando os vazios e te deu um **número errado com cara de número certo**. Hoje a API te deu
> **sucesso com cara de sucesso e dado nenhum dentro**."

> "Os erros que gritam são os baratos — vocês leem a mensagem e consertam. Os caros são estes: os que
> passam."

E a lição, no quadro, em letra grande:

> **Status 200 não quer dizer que deu certo. Status 200 quer dizer que o servidor respondeu.**
> **Quem decide se deu certo é você, olhando o conteúdo.**

O conserto é duas linhas, e é o `if`/`in` da Aula 3 com a Aula 7:

```python
dados = r.json()
if r.status_code != 200:
    print("o servidor recusou:", r.status_code)
elif "erro" in dados:
    print("CEP não encontrado")
else:
    print(dados["logradouro"], "-", dados["localidade"])
```

```
CEP não encontrado
```

> "Duas checagens. Primeiro o status, depois **o conteúdo**. E `"erro" in dados` é o `in` de
> dicionário da Aula 7 — vocês já tinham a ferramenta, faltava saber que precisavam dela."

**Diga o porquê de ser assim**, senão fica parecendo bug do ViaCEP:

> "E por que o ViaCEP faz isso? Porque **para o servidor nada deu errado.** Ele recebeu um pedido
> válido, consultou a base, não achou, e respondeu. Do ponto de vista dele a entrega foi perfeita.
> Cada API escolhe o seu jeito, e você descobre **lendo a documentação ou testando** — de preferência
> os dois."

#### 💥 nº 2 — `.json()` explodindo (6 min)

> "Agora um CEP **malformado**: `123`. Três dígitos. Nem parece CEP."

```python
r = requests.get("https://viacep.com.br/ws/123/json/")
print(r.status_code)
print(r.json())
```

```
400
requests.exceptions.JSONDecodeError: Expecting value: line 1 column 1 (char 0)
```

> "Dois fatos novos. O status mudou para **400** — *você pediu errado*, que é honesto. E o `.json()`
> **explodiu**."

Pergunte por quê, e mostre:

```python
print(r.text[:90])
```

```
<!DOCTYPE HTML>
<html lang="pt-br">

<head>
  <title>ViaCEP 400</title>
  <meta charset="u
```

> "Veio **HTML**. Uma página de erro, feita para pessoa ver. E o `.json()` tentou ler `<` como se fosse
> JSON e desistiu na primeira letra — é o que a mensagem diz: *expecting value, line 1, column 1*."

> "Guardem o padrão: **`JSONDecodeError` quase nunca é erro do seu código.** É o servidor te mandando
> outra coisa — página de erro, página de login, aviso de manutenção."

E então o ponto pedagógico, que é o retorno da Aula 9:

> "Lá na Aula 9 o `try/except` era zelo — aquela coisa de programador cuidadoso. Em código que fala
> com a internet **virou obrigação**. Porque agora o seu programa depende de uma máquina que você não
> controla, numa rede que você não controla."

```python
try:
    dados = r.json()
except requests.exceptions.JSONDecodeError:
    print("o servidor não mandou JSON. Status:", r.status_code)
    dados = None
```

```
o servidor não mandou JSON. Status: 400
```

#### 💥 nº 3 — 429, e o limite que não é seu bug (5 min)

```python
r = requests.get("https://economia.awesomeapi.com.br/json/last/USD-BRL")
print(r.status_code)
print(r.text)
```

```
429
{"status":429,"code":"QuotaExceeded","message":"Quota exceeded. To continue using the service, please see https://docs.awesomeapi.com.br/aviso-sobre-limites"}
```

> "Isso é de verdade — eu rodei na sexta e veio isso. **429: você pediu demais.** Não tem nada de
> errado no código. A API de cotação do dólar é de graça até um certo número de chamadas por hora, e
> alguém — talvez eu testando a aula — já gastou."

Três coisas para dizer, e a terceira é a que fica:

> "Primeira: na máquina de vocês pode vir **200 com a cotação**, porque o limite é contado por quem
> pede e quando. Se vier 200, ótimo; se vier 429, vocês acabaram de ver ao vivo."

> "Segunda: quando vier 429, a resposta certa é **esperar**, não insistir. Programa que vê 429 e
> tenta de novo na hora só piora."

> "Terceira, e é a mais importante: **não existe API de graça.** Existe API que alguém paga e deixa
> você usar um pouquinho. O 429 é o momento em que esse pouquinho acabou."

#### 💥 nº 4 — timeout, o travamento silencioso (5 min)

> "Esse é diferente dos três. Nos outros o programa erra. Nesse ele **não faz nada**, para sempre."

> "Imagina: o servidor aceita a conexão e nunca responde. Não é erro, não é 500 — é silêncio. Quanto
> tempo o `requests.get` espera?"

Deixe adivinharem. A resposta assusta:

> "**Para sempre.** Sem `timeout`, o padrão do `requests` é esperar indefinidamente. Seu programa fica
> pendurado e você não sabe se travou ou se está trabalhando."

```python
r = requests.get("https://viacep.com.br/ws/11013001/json/", timeout=0.001)
```

```
requests.exceptions.ReadTimeout: HTTPSConnectionPool(host='viacep.com.br', port=443):
Read timed out. (read timeout=0.001)
```

> "Eu pedi mil vezes mais rápido do que a internet consegue, só para vocês verem o erro. Mas repare:
> **ele deu erro.** É isso que a gente quer. Erro é tratável; travamento não é."

Regra para o quadro, sem exceção:

> **Todo `requests.get` da vida de vocês leva `timeout=`. Cinco ou dez segundos. Sempre.**

> "Semana que vem a gente vai fazer script rodar sozinho de madrugada. Um `get` sem `timeout` lá
> dentro é um programa que amanhece pendurado. Já estou plantando a semente."

---

### 11:26 — 11:34 | Chave de API: por que existe (8 min)

Chegue pelo 429, que acabou de acontecer:

> "O 429 abriu a porta. Se a API é de graça, **como é que eles sabem que fui eu que pedi demais?**"

Deixe responderem. Normalmente alguém diz "pelo IP". Está certo, e é insuficiente:

> "Pelo endereço de internet, por enquanto. Que funciona mal: a sala toda sai pelo mesmo endereço, e
> um aluno pedindo demais derruba os outros cinco. Para APIs maiores isso não serve. Então eles pedem
> que você **se identifique**."

**O que é, em uma frase:**

> "Chave de API é uma senha que **identifica o seu programa**, não você. É um texto comprido, feio,
> que você coloca na requisição."

```python
# formato típico — não rode, é só para ver a cara
requests.get("https://api.exemplo.com/dados",
             headers={"Authorization": "Bearer sk-9f2c...a81"},
             timeout=10)
```

Por que existe — as quatro razões, nesta ordem:

| Para quem mantém a API | Na prática |
|---|---|
| **Contar** | saber quantas chamadas são suas, para aplicar o limite |
| **Cobrar** | tier gratuito até X, pago depois |
| **Bloquear** | se alguém abusar, desligam **aquela** chave, não a API toda |
| **Saber quem usa** | o que mais chamam, o que podem desativar |

E então a regra que vai valer o resto do curso. Diga exatamente assim, porque daqui a seis sábados
vai doer:

> "A chave é **sua**, e ela identifica você. Se alguém pegar a sua, gasta o seu limite e, se for
> paga, **gasta o seu dinheiro**."

> "Então: chave **não vai** dentro do código. Chave **não vai** para o GitHub. Chave **não vai** no
> print de tela que você manda no grupo."

> "Na Aula 14 a gente aprende git, e eu vou mostrar que **o git lembra para sempre** — apagar a chave
> num commit seguinte não tira do histórico. Na Aula 18 vocês vão ter uma chave de LLM de verdade na
> mão. Quero que vocês cheguem lá já com medo saudável."

Para fechar e dar alívio:

> "Hoje nada do que a gente usou precisa de chave. ViaCEP, Agify, cotação — todos abertos. É de
> propósito: hoje era para aprender a conversar. A chave aparece quando a conversa fica cara."

---

### 11:34 — 11:54 | 🛠️ Mão na massa: buscador de CEP (20 min)

**O enunciado, projetado, inteiro:**

> "Um programa que pergunta o CEP com `input()` e imprime o endereço formatado. E que **não quebra**
> em nenhum dos três casos de hoje:"
>
> 1. CEP certo → imprime rua, bairro, cidade, UF
> 2. CEP que não existe → `200` com `{"erro": "true"}` → diz *"CEP não encontrado"*
> 3. CEP malformado, ou servidor fora, ou internet caída → trata e avisa, **sem stack trace na cara do usuário**

Dê os três CEPs de teste no quadro — e exija que testem os três:

```
11013001   →  deve achar (Santos)
99999999   →  não existe
123        →  malformado
```

> "Enquanto não passar nos três, não está pronto. Esse é o critério, e é o mesmo critério de código
> profissional: **o caminho felizinho é a parte fácil**."

Circule. Os travamentos que você vai ver, em ordem de frequência:

- **checar só o status** — é o 💥 nº 1 na pele; pergunte *"rodou com o `99999999`?"*
- **`KeyError: 'logradouro'`** — chegou perto; é consequência do anterior
- **`except` pelado** — `except:` sem nada, engolindo tudo. Aproveite: *"se engolir tudo, você nunca
  mais vai saber o que deu errado. Diga o que você espera."*
- **esquecer o `timeout`** — lembre uma vez e siga

Solução de referência, para projetar **depois** que a maioria chegar:

```python
import requests

def buscar_cep(cep):
    cep = cep.strip().replace("-", "").replace(".", "")
    url = f"https://viacep.com.br/ws/{cep}/json/"
    try:
        r = requests.get(url, timeout=5)
    except requests.exceptions.RequestException as e:
        return None, f"não deu para falar com o servidor ({type(e).__name__})"

    if r.status_code != 200:
        return None, f"o servidor recusou o pedido (status {r.status_code})"

    try:
        dados = r.json()
    except requests.exceptions.JSONDecodeError:
        return None, "o servidor não mandou JSON"

    if "erro" in dados:
        return None, "CEP não encontrado"

    return dados, None


cep = input("Digite o CEP: ")
dados, problema = buscar_cep(cep)

if problema:
    print("❌", problema)
else:
    print(f"{dados['logradouro']}, {dados['bairro']}")
    print(f"{dados['localidade']} - {dados['uf']}  (DDD {dados['ddd']})")
```

Rodando com `11013001`:

```
Avenida João Pessoa, Centro
Santos - SP  (DDD 13)
```

Com `99999999`:

```
❌ CEP não encontrado
```

Com `123`:

```
❌ o servidor recusou o pedido (status 400)
```

**Repare numa escolha de projeto e comente em voz alta** — vale mais que o código:

> "A função devolve **duas coisas**: o dado e o problema. Um dos dois é sempre `None`. Quem chama a
> função decide o que fazer com o problema — a função não dá `print` de nada."

> "Por que isso é melhor? Porque amanhã essa função vai ser usada dentro de um laço com mil CEPs, e
> ninguém quer mil `print` no meio do relatório. **Função busca; quem chamou decide.**"

**Bônus, se o grupo estiver voando** *(e aqui o pandas volta)*: o notebook tem `clientes.csv` com
uma coluna de CEP. Completar as colunas de endereço com a função acima.

```
           nome       cep                        logradouro        cidade   uf                                  situacao
0   Diego Matos  11013001               Avenida João Pessoa        Santos   SP                                        ok
1   Bruno Sales  11440003               Avenida Dom Pedro I       Guarujá   SP                                        ok
2     Ana Costa  11701000  Avenida Presidente Costa e Silva  Praia Grande   SP                                        ok
3  Gisele Prado  11310000         Praça Barão do Rio Branco   São Vicente   SP                                        ok
4   Elaine Reis  11075350     Rua Monsenhor Paula Rodrigues        Santos   SP                                        ok
5     Fábio Luz  01311000                  Avenida Paulista     São Paulo   SP                                        ok
6    Carla Dias  99999999                               NaN           NaN  NaN                        CEP não encontrado
7  Heitor Nunes       123                               NaN           NaN  NaN  o servidor recusou o pedido (status 400)
```

E o reflexo da Aula 10, logo depois:

```
situacao
ok                                          6
CEP não encontrado                          1
o servidor recusou o pedido (status 400)    1
```

> "Oito linhas, oito chamadas. **Duas falharam e as outras seis entraram.** O relatório ficou com
> buraco — e vocês sabem exatamente qual buraco e por quê. É isso que um programa bom faz: ele não
> esconde a falha, ele a localiza."

---

### 11:54 — 12:00 | O que isso destrava + tarefa (6 min)

**O que isso destrava (3 min).** Projete e leia:

> "Com `requests.get` e `.json()`, estas coisas saíram de *impossível* e viraram *uma tarde*:"
>
> - **CEP, endereço, cotação, clima, feriado** — dado público, de graça
> - **Qualquer sistema do seu trabalho** que tenha API: puxar dado sem abrir o site, sem exportar planilha
> - **Juntar duas fontes** que ninguém juntou ainda — é aí que mora quase todo projeto interessante
> - **Chamar IA de dentro do seu código** — Aula 18 é exatamente isto, com uma URL diferente e uma chave

> "E repare no tamanho do que vocês aprenderam hoje: `requests.get`, `status_code`, `.json()`,
> `params`, `timeout`. **Cinco coisas.** A API do ChatGPT se usa com essas mesmas cinco."

**A tarefa (2 min):**

1. **Terminar o buscador de CEP** e provar que passa nos três casos — `11013001`, `99999999`, `123`.
   Mande no grupo o print das três saídas
2. **Trocar o ViaCEP por outra API aberta** e fazer funcionar. Sugestões: `api.agify.io` (idade por
   nome), `brasilapi.com.br` (feriado, banco, DDD), `open-meteo.com` (clima). **Leia a documentação
   sozinho** — é a habilidade da Aula 11 outra vez
3. **Achar uma API que resolva um problema seu de verdade.** Não precisa programar: mande no grupo
   qual você achou, o link da documentação e **o que você faria com ela**
4. **Desafio:** pegar os CEPs da `clientes.csv`, completar o endereço de cada um e **salvar um
   `clientes_completo.csv`** com uma coluna extra dizendo o que aconteceu em cada linha (`ok`,
   `não encontrado`, `erro`). É o pandas da Aula 10 encontrando a API de hoje

**A pergunta que fica (1 min):**

> "Olhem o que vocês têm agora: um programa que busca dado de fora, trata os erros e monta um
> relatório. Isso é software de verdade."

> "Mas ele tem um defeito que nenhum de vocês vai conseguir ignorar depois que eu falar: **ele só roda
> quando você aperta o play.** Você precisa abrir o computador, abrir o VS Code e clicar."

> "E se a cotação precisa ser consultada todo dia às 9h? E se o relatório precisa sair na segunda de
> manhã, sem você? **Como é que isso roda sozinho?**"

*(Aula 13: automação de verdade.)*

---

## Se sobrar tempo

- **`resposta.headers`** — mostre `Content-Type` e, quando tiver, os cabeçalhos de limite. Deixa
  visível que a resposta tem metadado, não só corpo
- **ViaCEP ao contrário:** `viacep.com.br/ws/SP/Santos/Ana Costa/json/` devolve uma **lista** de CEPs,
  não um dicionário. Bom susto pedagógico: `dados["cep"]` quebra, e a correção é `dados[0]["cep"]`.
  Amarra com lista de dicionários da Aula 7
- **`raise_for_status()`** — `HTTPError: 400 Client Error: Bad Request for url: ...`, o jeito curto de
  transformar status ruim em exceção. Só mostre se a turma estiver confortável com `try/except`
- Trocar `/json/` por `/xml/` na URL do ViaCEP e ver o mesmo dado em outro formato. Mostra que
  **formato é escolha de quem serve**

## Se faltar tempo

Corte nesta ordem:

1. O bônus do `clientes.csv` com pandas na mão na massa
2. O 💥 nº 4 (timeout) como demonstração — mas **deixe a regra `timeout=5` no quadro** e no código de referência
3. A tabela `.text` × `.json()` (eles vão deduzir)
4. Metade da tabela de status: fique só com *200 deu certo, 4xx é seu, 5xx é dele*
5. O bloco de chave de API — reduza a três frases: existe, serve para contar e cobrar, e **não vai para o git**

**Nunca corte:** as duas abas lado a lado na abertura (site × JSON), o 💥 nº 1 **com a aposta da turma
antes de rodar**, a amarração explícita com a Aula 10, e a mão na massa testada nos **três** CEPs. Se
só der tempo de uma coisa hoje, que seja a turma descobrindo sozinha que `200` mentiu.

---

## Depois da aula — 15 minutos

- [ ] Mandar no grupo: link do notebook, a tarefa escrita, e **a função `buscar_cep` pronta** — eles
      vão copiar essa estrutura o resto do curso
- [ ] Mandar também a **tabela de status** como imagem. É consulta de meses
- [ ] Anotar em `duvidas-da-turma.md` o que te pegou. Aposta: alguém vai confundir `.text` com
      `.json()`, e alguém vai perguntar se a API "guarda" o que ele pediu
- [ ] **Ler as APIs que eles mandarem na tarefa 3.** Isso te diz o que a turma quer automatizar de
      verdade — e deve influenciar os exemplos da Aula 13
- [ ] **Preparar a Aula 13:** escrever o script organizador de Downloads na sua máquina **e rodar duas
      vezes** para ver a duplicação acontecer de verdade. A aula depende desse erro aparecer ao vivo
- [ ] *(nota Unipds)* Escrever para si mesmo, sem jargão: **qual a diferença entre um erro de rede, um
      erro de HTTP e um erro de conteúdo** — e por que o Python só levanta exceção no primeiro. Se você
      não conseguir explicar em três frases, a turma também não vai conseguir
