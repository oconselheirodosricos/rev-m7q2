# Inventário JW Prep — estrutura da fonte ao vivo

Construído em 17/08/2026, Fase 1 do PEA "JW PREP 100%", a partir da fonte ao vivo
em jw.org (não do nosso `output/` atual — princípio "primeiro a fonte, depois o
nosso"). Referência viva usada pra este inventário: semana 17-23 de agosto de 2026
(VM: `jw-apostila-do-mes/julho-agosto-2026-mwb/.../17-23-de-agosto-de-2026/`;
Sentinela: `sentinela-estudo-junho-2026/Como-manter-a-amizade-com-nossos-irmãos/`),
mais os 6 capítulos do livro `wcg` já auditados nas rodadas 2 e 3 (Enoque, Noé,
Sara, Abraão I, Abraão II, Rebeca — docids 1102025901-06).

Ferramentas usadas (já existentes no projeto, não inventadas pra este documento):
`src/discover.py::discover()` (resolve URL da semana), `src/fetch.py::get_html()`,
`src/scrape.py::scrape()`/`estudo_capitulo_completo()`. Todo dado abaixo veio de
rodar essas funções contra a fonte real, não de memória de sessões anteriores.

Para cada elemento: **nome** | **obrigatório?** | **onde no nosso output** |
**tag** | **como verificar**.

---

## 1. VM — Vida e Ministério

Estrutura confirmada ao vivo (semana 17-23 ago 2026, `wcg cap. 4`; nem toda parte
tem tempo publicado no HTML da fonte — Partes 3-6 não trazem `min` no card, só no
PDF/app, então nosso "tempo" nessas partes vem de convenção de programa, não da
página web).

| # | Elemento | Obrigatório? | Onde no nosso output | Tag | Como verificar |
|---|----------|-------------|----------------------|-----|-----------------|
| — | Seção "JEREMIAS N-M" (bloco de leitura semanal) | Sim, sempre 1 por semana | Topo do MD | `@ISCA`/`@ENTENDENDO` (intro da semana) | `scrape()['subtitulos'][0]` = nome do bloco de leitura |
| — | Cântico + oração de abertura | Sim | `@ABERTURA` | `@ABERTURA` | Presente em toda semana, texto genérico (não vem numerado na fonte) |
| 1 | Tesouros da Palavra de Deus (discurso, 10 min) | Sim | Parte 1 | `@PARTE 1. <título> \| <tempo> \| discurso` + `@FONTE` + `@COMENTARIO` | Título e referências bíblicas verbatim contra `partes[i].texto`; tempo = "10 min" sempre nesta parte |
| 2 | Joias espirituais (10 min) | Sim | Parte 2 | `@PARTE 2. Joias Espirituais` + `@JOIA` × N | Mesmo conjunto de joias nas duas versões (regra já estabelecida); fonte não numera as joias, só dá o texto-base + 1 pergunta aberta |
| 3 | Leitura da Bíblia (4 min) | Sim | Parte 3 | `@PARTE 3. Leitura da Bíblia` + `@FONTE` | Referência bíblica exata (ex. `Jer. 28:5-17`) + lição do `th` citada verbatim |
| 4-6 | Faça Seu Melhor no Ministério (3 partes, tipo varia por semana: iniciando conversas / cultivando o interesse / fazendo discípulos / discurso / vídeo, etc.) | Sim, sempre 3 partes nesta seção, TIPO varia semana a semana | Partes 4-6 | `@PARTE N. <título> \| <tempo> \| <tipo>` | Tipo/fonte (`lmd lição X ponto Y`, `lff lição X`) verbatim contra a fonte; **não presumir o tipo de uma semana a partir de outra — cada semana declara o próprio** |
| 7 | Nossa Vida Cristã, 1ª parte (tipo varia: vídeo, consideração, "Necessidades locais", discurso do Corpo Governante etc.) | Sim, mas o CONTEÚDO varia mais que qualquer outra parte do programa | Parte 7 | `@PARTE 7. ...` + `@COMENTARIO`/`@PERGUNTA`/`@RESPOSTA` (quando há vídeo) | **Armadilha permanente já registrada em `REGRAS_JWPREP_CONSOLIDADO.md`:** se a fonte não trouxer transcrição do vídeo, NOTA HONESTA é obrigatória — nunca inventar cena/diálogo. Semanas sem vídeo nesta parte (ex. "Necessidades locais") não geram nota honesta nem `@RESPOSTA` (`tests/test_blindagem_estrutural.py` já trata esse skip como legítimo) |
| 8 | Estudo bíblico de congregação (30 min) | Sim | Parte 8 | `@PARTE 8. ...` + `@FONTE` + `@COMENTARIO` + `@HISTORIA` × N capítulos | Ver seção 3 (livro `wcg`) — cobertura completa dos 4 blocos oficiais |
| — | Comentários finais + cântico + oração | Sim | Fim do MD | linha livre | Presente sempre |
| — | **Variação conhecida:** semana de 9 partes | Não é padrão, ocorre em semanas de campanha especial (ex. "Campanha especial em setembro", já documentado no ESTADO.md em 24 ago) | Depende da semana | mesma DSL, uma `@PARTE` extra | **Nunca assumir 8 partes fixas: contar `partes` reais da fonte a cada semana antes de gerar** |

**O que a fonte tem e o nosso output não tem (VM):** nada identificado nesta
passada — os campos coletados (`titulo`, `partes`, `subtitulos`, `perguntas_finais`)
têm correspondência em alguma tag nossa. Gap real encontrado é de PROCESSO, não de
tag: Parte 7 depende de vídeo real transcrito (Fase 2, Transkriptor), hoje cai em
nota honesta quando a transcrição não existe — nota honesta é o fallback correto,
não um gap de tag.

**`[ATUALIZADO 17/08 — checagem ao vivo das 11 semanas]` Vídeo na Parte 7 é a
NORMA, não a exceção.** Checagem inicial desta fase presumiu (errado) que só 2
das 11 semanas tinham vídeo, porque a busca só olhava `partes[i].texto` (onde
"Mostre o vídeo" aparece como instrução direta). Confirmado ao vivo, com regex
case-insensitive contra o HTML bruto das 11 semanas: **8 de 11 têm "mostre o
vídeo"** (06-22, 07-06, 07-13, 07-20, 07-27, 08-03, 08-24, 08-31); só 3 são
"Necessidades locais" sem vídeo (06-29, 08-10, 08-17). **Gap de ferramenta real
achado no processo:** o título do vídeo às vezes só aparece no `alt` de uma
imagem de cena do vídeo (`<span class="jsRespImg" data-img-att-alt="Uma cena do
vídeo '<título>' ...">`), um padrão de marcação DIFERENTE do `figcaption` que
`src/scrape.py::_imagens()` já trata (usado pela Sentinela) — a função atual NÃO
captura essas imagens de cena de vídeo da VM, então `doc['imagens']` fica vazio
ou incompleto pra semanas com vídeo (confirmado: 07-06 tem só 1 imagem capturada,
a de introdução, faltando a imagem de cena do vídeo). Confirmado em 4 das 8
semanas com vídeo o título exato via essa imagem (07-06, 07-20, 07-27, 08-24);
nas outras 4 (06-22, 07-13, 08-03, 08-31) o título não veio por esse padrão —
precisa achar o mecanismo de cada uma antes da Fase 2 poder rodar `web/transcribe.py`
(que já existe, funcional, mas exige o "lank" do jw.org — ex. `pub-jwb-092_14_VIDEO`
— que a página da VM não expõe diretamente; falta um resolvedor título→lank).

---

## 2. Sentinela (revista de estudo)

Estrutura confirmada ao vivo (`Como manter a amizade com nossos irmãos`, edição
junho 2026, semana 17-23 ago): 20 parágrafos, 20 perguntas numeradas 1-20 (sem
numeração combinada nesta semana específica — ver nota abaixo), 5 subtítulos, 2
imagens, 1 caixa de recap.

| # | Elemento | Obrigatório? | Onde no nosso output | Tag | Como verificar |
|---|----------|-------------|----------------------|-----|-----------------|
| — | Título do artigo | Sim | Topo do MD | `# <título>` (H1 markdown, sem `@TAG` própria) | Verbatim contra `scrape()['titulo']` |
| — | Texto-base (versículo-tema) | Quase sempre presente | Logo abaixo do título | `**Texto-base:** <citação>` (bold markdown, sem `@TAG` própria) | `scrape()['texto_base']` |
| — | Subtítulos de seção | Sim, variável em quantidade (esta semana: 4 + 1 do recap = 5) | Antes de cada bloco de parágrafos | `## <SUBTITULO>` (H2 markdown) | Verbatim contra `scrape()['subtitulos']`, MAIÚSCULO igual à fonte |
| — | Parágrafos numerados com pergunta oficial | Sim, 1 pergunta por parágrafo na maioria; **pode haver numeração combinada** (ex. "5, 6.") quando 2 parágrafos compartilham 1 pergunta impressa — não ocorreu nesta semana, mas já ocorreu em semanas anteriores do lote (ver histórico) | Corpo do artigo | `**N. <pergunta>**` (bold markdown) seguido de bloco cercado (```` ``` ````) com `Resposta:`/`Complementar N:`/`Pra Laurinha:` — **não é `@TAG`, é convenção de markdown puro** (diferente da VM, que usa `@HQ`) | Cada `perguntas[i].texto` tem que aparecer verbatim (número + pergunta); `data-pid` é o identificador estrutural do parágrafo, nunca contar por título de subtítulo |
| — | Imagens + parágrafo de ligação | Sim, quantidade variável (esta semana: 2) | Junto ao parágrafo referenciado | `@IMG <nº do parágrafo> \| <url> \| <alt> \| <legenda>` — **`@IMG`, não `@IMAGEM`** (esse é o nome usado só na VM, tag distinta, confirmado em `web/build.py` linhas 328 e 1077) | `scrape()['imagens'][i]` traz `descricao` (legenda verbatim) + `paragrafo_ref` (nº do parágrafo que a legenda cita, ex. "Veja o parágrafo 8") — a imagem tem que estar posicionada perto desse parágrafo no nosso output, não solta no fim |
| — | Caixa de recap ("O que você diria?" / "Como responderia?" / título variável) | Sim, praticamente toda semana | Fim do artigo | `@RECAP <título verbatim>` | **NUNCA identificar por palavra-chave no título** (título muda toda semana) — critério estrutural: único `boxContent` com campo de resposta do leitor (`gen-field`/`textarea`). `scrape()['perguntas_finais_titulo']` + `scrape()['perguntas_finais']` já aplicam esse critério (`src/scrape.py::_recap`, comentário permanente no código sobre a armadilha) |
| — | Caixas suplementares sem campo de resposta (ex. biografia, box lateral de contexto) | Variável, nem toda semana tem | Onde a fonte posicionar | sem tag dedicada hoje | **Gap identificado abaixo** |

**Nota sobre numeração combinada:** não há exemplo ao vivo nesta passada (a
semana de referência não combina números), mas o parser (`web/build.py::parse_doc`)
e a trava (`tests/test_recap.py`) já foram testados contra semanas anteriores do
lote que tinham essa combinação — não é uma lacuna nova, só não reconfirmada
NESTA semana específica.

**O que a fonte tem e o nosso output não tem (Sentinela):** caixas suplementares
sem campo de resposta do leitor (bios, contexto histórico, quadros "Você Sabia?")
não têm tag própria hoje — quando existem, ou viram parágrafo comum (perdendo o
destaque visual da caixa) ou são omitidas. Não apareceu na semana de referência
(sem essas caixas), mas é um gap estrutural real a confirmar contra semanas que
tenham esse tipo de caixa antes da Fase 3.

---

## 3. Livro de estudo de congregação (`wcg`, Parte 8)

Estrutura já auditada ao vivo nas rodadas 2 e 3 desta sessão, nos 6 capítulos
usados em jul-ago 2026 (Enoque, Noé, Sara, Abraão I, Abraão II, Rebeca). Reconfir-
mada estruturalmente correta e completa por `pytest tests/test_parte8_perguntas.py`
= 43 passed nesta mesma sessão, após o fechamento da rodada 3.

| # | Elemento | Obrigatório? | Onde no nosso output | Tag | Como verificar |
|---|----------|-------------|----------------------|-----|-----------------|
| 1 | "Para considerar" | Sim, 1 por capítulo | Início do `@HISTORIA` (corpo, antes do 1º `@HQ`) | corpo livre do `@HISTORIA` | `estudo_capitulo_completo()['para_considerar']` |
| 2 | "Analise mais a fundo" (perguntas oficiais numeradas, contagem varia: 1 a 4 por capítulo) | Sim | `@HQ N. <pergunta>` + `Resposta:` | `@HQ` | `estudo_oficial_perguntas()`; **nunca presumir 4 — o capítulo "Ele enfrentou seu maior desafio" (Abraão II) tem só 1** |
| 3 | "Leia o relato na Bíblia" (texto/capítulos bíblicos indicados) | Sim | Referenciado no corpo ou nas Respostas | sem tag dedicada, citado inline | Confirmar que os capítulos indicados pela fonte aparecem citados em algum ponto da história |
| 4 | Imagens rotuladas (A-E, contagem varia por capítulo: 2 a 5) | Sim | Depois do último `@HQ`, antes de `@HREFLEXAO` | `@HIMG <letra> \| <url> \| <alt> \| <legenda>` + comentário | `estudo_capitulo_completo()['imagens']`; cobrir TODAS as letras que a fonte usar, mesmo quando `alt=""` (caso real: Abraão II imagem A) |
| 5 | "Medite no que aprendeu" (3-5 itens) | Sim | `@HREFLEXAO`, 1ª linha | dentro de `@HREFLEXAO` | `estudo_capitulo_completo()['medite_no_que_aprendeu']` |
| 6 | "Pense no quadro completo" (3 itens fixos: Jeová / propósito / pergunta pra quando ressuscitar) | Sim | `@HREFLEXAO`, 2ª linha | dentro de `@HREFLEXAO` | `estudo_capitulo_completo()['pense_no_quadro_completo']` |
| 7 | "Aprenda mais" (vídeo com duração + artigo, ou só artigo quando não há vídeo) | Sim | Depois de `@HREFLEXAO`, antes de `@HAPLICAR` | `@APRENDAMAIS` | `estudo_capitulo_completo()['aprenda_mais']` (lista de dicts `tipo`/`texto`/`href`); duração obrigatória no formato `(M:SS)` quando `tipo == "video"` — Rebeca é o único capítulo do lote sem vídeo (só 2 artigos) |

**O que a fonte tem e o nosso output não tem (livro `wcg`):** nada identificado —
os 7 elementos acima têm tag correspondente, confirmados nos 6 capítulos do lote
atual. Gap de PROCESSO (não de tag): a numeração oficial de "Analise mais a fundo"
precisa ser reconfirmada ao vivo a cada capítulo novo antes de escrever `@HQ`
(já é a prática desde a rodada 2, registrado como regra permanente na spec).

---

## 4. Resumo — gaps reais encontrados nesta Fase 1

1. **Sentinela: caixas suplementares sem campo de resposta** (bios, "Você Sabia?",
   contexto histórico) não têm tag própria hoje. Não confirmado contra uma semana
   real que tenha esse tipo de caixa ainda — próximo passo natural é achar uma
   semana do lote que tenha e decidir se cria tag ou é aceitável tratar como
   parágrafo comum.
2. **VM Parte 7: depende de transcrição real de vídeo** — hoje cobre com nota
   honesta quando não há transcrição. Isso é o comportamento CORRETO (regra
   permanente da spec), não um defeito, mas é o que a Fase 2 (Transkriptor) vai
   substituir por conteúdo real sempre que o vídeo existir e for transcritível.
3. **Numeração combinada de parágrafos da Sentinela** (ex. "5, 6.") não foi
   reconfirmada ao vivo nesta passada específica (a semana de referência não
   tinha exemplo) — o parser e a trava já lidam com isso de rodadas anteriores,
   mas vale reconfirmar contra uma semana real do lote na Fase 3.

Nenhum gap encontrado nesta fase é do tipo "a fonte tem um bloco oficial inteiro
que a gente nunca gerou" (o defeito original da Parte 8, já fechado nas rodadas
2 e 3). Os 3 itens acima são refinamentos de cobertura, não teto quebrado.
