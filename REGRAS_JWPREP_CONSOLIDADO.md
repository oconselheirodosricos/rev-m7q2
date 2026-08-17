# REGRAS_JWPREP_CONSOLIDADO.md
**Consolidação exaustiva das regras do JW Prep — tudo que foi criado, corrigido e ajustado até 2026-07-21.**
**Decisões §10 (A-H) APROVADAS por João em 2026-07-21 e já aplicadas neste documento.**

Fontes varridas para este documento:
- `prompts/jw_friend_system.md` (prompt-mestre)
- `web/build.py` + `web/jwfilter.py` (código que FORÇA regras; citações `build.py:linha` / `jwfilter.py:linha`)
- Os 8 MDs de julho aprovados (`output/2026-07-20_*` e `2026-07-27_*`, VM+Sentinela, PESSOAL+PUBLICA)
- Memórias do projeto (`.claude/.../memory/*`)
- Todo o histórico de correção no `ESTADO.md`

**Legenda de marcação:**
- Sem marca = regra escrita/forçada em algum lugar (prompt, código, ESTADO ou memória).
- `[DECIDIDO 21/07]` = ponto que era ambíguo/conflitante e foi resolvido por decisão do João em 21/07/2026 (detalhe em §10).
- `[IMPLÍCITA, formalizar]` = regra aplicada nos MDs mas ainda não escrita no prompt/código. Levar pro prompt-mestre.
- `[CÓDIGO]` = forçada pelo build. `[PROMPT]` = declarada no prompt-mestre. `[ESTADO]` = fixada no histórico. `[MEM]` = memória.

---

## §0. REGRAS SUPREMAS (acima de tudo) `[PROMPT]`

1. **RS1 — FILTRO JW 100%** (a mais importante, acima inclusive da linguagem simples). Todo conteúdo, pública e full, vem EXCLUSIVAMENTE de: (a) jw.org em português; (b) a Bíblia Tradução do Novo Mundo (TNM) acessada dentro do jw.org. **Zero invenção, zero fonte externa, zero outra tradução.** Se não dá pra confirmar no jw.org/TNM, não entra. Na dúvida, ficar com o que o material diz, nunca extrapolar.
2. **RS2 — LINGUAGEM SIMPLES** (regra de ouro). Simples e clara pra qualquer idade/instrução; edificante pra quem LÊ e pra quem OUVE na reunião. Alvo: simples de entender, profundo no efeito.
3. **RS3 — PROIBIDO TRAVESSÃO E ESTRANGEIRISMO** (formatação inviolável). Zero em-dash (—) e en-dash (–) em QUALQUER saída, sempre. Zero palavra estrangeira ("firmeza", nunca "endurance"). Cabeçalho limpo. Isca sempre na caixa azul. (Detalhe em §5.)
4. **RS4 — MÉTODO: GERAR, CHECAR, PUBLICAR NA HORA** (permanente desde 15/07/2026). Não se espera validação humana antes do deploy. Fluxo: gerar → revalidar com os checks automáticos → publicar. `PENDING_VALIDATION` vazio por padrão. (Detalhe em §6/§7.)

---

## §1. FONTES (trava de fonte)

### 1.1 Regra de fonte (RS1) `[PROMPT]`
- Só jw.org PT-BR + TNM. Proibido: qualquer site/blog/comentário de terceiros, qualquer outra tradução, qualquer informação inventada/suposta/"preenchida", qualquer interpretação sem apoio explícito no material.
- **Por quê:** João comenta na reunião batendo com o que os irmãos leem; texto fora da TNM gera divergência.
- **Fallback:** se não há material no jw.org, oferecer textos da TNM pra meditar. João sempre sai com pelo menos um texto. Zero invenção; se não tem, dizer que não tem.

### 1.2 Texto literal da TNM, palavra por palavra `[PROMPT]`
- Todo texto entre aspas = texto literal EXATO da TNM, copiado do jw.org — nunca de memória, nunca parafraseado, nunca de outra tradução.
- **`[DECIDIDO 21/07 — §10-A]` Toda citação literal da TNM vai entre ASPAS CURVAS `“…”`.** Aspas retas `"…"` ficam SÓ pro texto ilustrativo/não-literal. É isso que reativa a trava de literalidade (`jwfilter._quotes` só confere curvas).
- Toda referência é EXTRAÍDA do material real (o versículo aparece no artigo; ou — só na FULL — é um texto TNM ligado ao tema).
- **Regra do versículo que a TNM não retorna:** se `tnm_text(livro,cap,v)` não retorna o texto do versículo, ele **NÃO é citado entre aspas em lugar nenhum** — trata só por referência. `[PROMPT][ESTADO]`
- Toda citação leva link `finder?wtlocale=T&pub=nwtsty&bible=BBCCCVVV`. `[CÓDIGO]` (`build.py` `_link_refs`)

### 1.3 A trava automática (`web/jwfilter.py`, roda no `build.py` antes de publicar) `[CÓDIGO]`
- **Allowlist** = todo versículo `(livro,cap,v)` que o ARTIGO bruto cita (`parse_refs`, `jwfilter.py:90`), + os capítulos da leitura declarada (`reading_chapters`, `jwfilter.py:183`).
- **Cheque de referência:** cada ref das respostas tem de ser um versículo da allowlist OU cair dentro de um capítulo de leitura declarado. Fora disso = "furo".
  - **PÚBLICA = estrita:** ref fora → `blocked_refs` → **ABORTA O BUILD** (`jwfilter.py:229`).
  - **FULL/PESSOAL = permissiva:** ref fora → só registra em `full_extra_refs`, **não aborta** (`jwfilter.py:232`). Porque complementares da full podem citar outros textos TNM do tema.
- **Cheque de aspas literais (`check_quotes=True`):** para cada citação, busca o texto literal da TNM (`tnm_text` via curl no finder) e exige que a aspa bata; se não bate → `quote_warnings` → **ABORTA O BUILD** nas DUAS versões (`jwfilter.py:242`).
  - ✅ **`[DECIDIDO 21/07 — §10-A]`:** o detector só enxerga aspas CURVAS `“...”` (≥8 chars, `jwfilter.py:162`); retas são ignoradas. A partir de agosto, **toda citação literal da TNM usa aspas curvas → a trava valida de verdade**. (Os MDs de julho usavam retas em tudo, por isso a trava estava inócua.)
- **"0 furos"** = `blocked_refs` 0 + `quote_warnings` 0.
- **CLI de verificação:** `python3 web/jwfilter.py <data> [pub]` → JSON + `PASSOU`/`BLOQUEADO`.

### 1.4 Armadilhas conhecidas da trava (operacional) `[ESTADO][PROMPT]`
- `reading_chapters` só aceita faixa de capítulos com `0 < fim-início < 10`. Faixa como `APOCALIPSE 1-22` é silenciosamente descartada → quebrar em `1-9`, `10-18`, `19-22`.
- `parse_refs` NÃO entende ref encadeada sem nome de livro (`Jer. 42:19-22; 43:1, 2, 4`) nem abreviação curta (`1Co`, `2Co`, `Ef`). **Toda ref da allowlist tem de estar qualificada com o nome do livro por extenso** (a pública bloqueia; a full deixa passar como "extra" e engana).
- Headings manuais no raw (`## ESTUDO DE LIVRO: <Livro N-M>`) liberam esses capítulos na trava. **Re-rodar `python3 -m src.cli --date <data>` REGRAVA o raw e APAGA esses headings + qualificações manuais** → repor à mão, senão o build aborta.
- **`reading_chapters` só entende FAIXA (`N-M`).** Semana de UM capítulo só (ex.: `## JEREMIAS 31`) não libera nada: todo versículo usado tem de estar citado no bruto ou ser declarado à mão no bloco de fontes liberadas.
- **BUG CORRIGIDO 27/07/2026 — `tnm_text` e os livros 1 a 9.** A URL do finder usa o livro com 2 dígitos (`bible=01012001`), mas o **id do versículo no HTML vem SEM o zero à esquerda** (`id="v1012001"` em Gênesis 12:1; `id="v24024005"` em Jeremias 24:5). O código procurava só a forma zero-preenchida, então **Gênesis, Êxodo, Levítico, Números, Deuteronômio, Josué, Juízes, Rute e 1 Samuel devolviam `None`**. Com a trava de aspas religada (§10-A), isso fazia toda citação literal desses livros **abortar o build**, e empurrava o autor a rebaixar Gênesis a mera referência (grave, já que o livro `wcg` é quase todo Gênesis). Corrigido em `jwfilter.tnm_text`: a busca do id aceita as duas formas (`id="v0?<livro><cap><vers>"`). **Lição permanente:** `tnm_text` devolver `None` NÃO significa "a TNM não tem esse versículo"; antes de rebaixar uma citação a referência, confirmar que o versículo realmente não vem do finder.

---

## §2. ESTRUTURA DO VM (Vida e Ministério)

### 2.1 Cabeçalho do arquivo (idêntico em PESSOAL e PUBLICA — VM sem marca de versão) `[IMPLÍCITA, formalizar]`
```
# Vida e Ministério, <intervalo de datas>

> Reunião: VM · Semana de estudo: <intervalo>
> Fonte: <URL do programa mwb no jw.org>
> Leitura da semana: <passagem, ex. Jeremias 18-19>
```
Separador entre células = ` · ` (ponto médio). `[DECIDIDO 21/07 — §10-E]` Nenhuma marca de versão no MD (nem no VM nem na Sentinela); a versão vem do nome do arquivo/rota.

### 2.2 Bloco de abertura "Preparando a Mente e o Coração" (ordem fixa) `[CÓDIGO][PROMPT]`
`@ABERTURA Preparando a Mente e o Coração` → `@ISCA` → `@RESUMO` → `@ENTENDENDO Entendendo a Leitura da Semana` → `@APLICACAO` → `@ORACAO` → `@FIM-ABERTURA`. Renderiza como accordion aberto, com botão de compartilhar próprio → rota `/vm/<data>/preparar/`.
- `@ISCA` = isca de 2-4 frases, 2ª pessoa, termina em pergunta/imagem. Renderiza na **caixa azul** `.isca-top` no topo, acima do título. Toda semana PRECISA de `@ISCA`.
- `@RESUMO` = um parágrafo denso; sempre fecha com instrução de estudo `"Pra estudar bem, ..."`. `[IMPLÍCITA, formalizar]` (boilerplate)
- `@ENTENDENDO` (VM) = exatamente 3 bullets `- rótulo | texto`: (1) O que está acontecendo aqui; (2) O que aprendo sobre Jeová nesta leitura; (3) O que isso me ensina a fazer.
- `@APLICACAO` = exatamente **5 campos** `- rótulo | texto` (render sob "Aplicação do Aprendizado"): **Na relação com Jeová / Na vida / Na família / Na congregação / Na pregação.** Mesmos 5 na Sentinela e nas duas versões. `[PROMPT]`
- `@ORACAO` = um parágrafo (render sob "Orando Sobre o Aprendizado"); abre `"Ideias pra levar a Jeová (não é oração pronta, é direção): ..."` e fecha `"Cada um pode meditar: <pergunta>?"`. `[IMPLÍCITA, formalizar]` (boilerplate)

### 2.3 Seções e partes (ordem fixa, `@SECAO <TÍTULO EM CAPS>` / `@PARTE título|tempo|tipo`)
**TESOUROS DA PALAVRA DE DEUS**
- **Parte 1** (discurso, 10 min): `@FONTE` → `@COMENTARIO` (pontos `**(1)... (2)...**`) → `@APLICAR` → `@COMPLEMENTAR`×N (só PESSOAL) → `@IMAGEM` + `Comentando a imagem:`.
- **Parte 2 — Joias Espirituais** (10 min): `@COMENTARIO` sempre abre `"A pergunta fixa da semana é..."` (a pergunta fixa oficial, respondida em prosa) → `@APLICAR` → lista `@JOIA <ref>: ...` → `@COMPLEMENTAR`×3 (só PESSOAL; a última reafirma "que joias VOCÊ achou"). `[IMPLÍCITA]`
  - **Joias:** ler TODA referência de joia no jw.org; alvo ≥10 sem forçar (qualidade > quantidade; nunca inventar pra bater número). Cada joia = citação + insight em 1ª pessoa; **conclusão prática fluida na frase, NUNCA com rótulo seco** ("Aprendo:"/"Aplico:"/"Me examino:"). Render em lista ORDENADA. `[DECIDIDO 21/07 — §10-G]` A PÚBLICA usa o **mesmo conjunto de joias** da full, só com texto mais aparado — nunca dropa uma joia.
- **Parte 3 — Leitura da Bíblia** (4 min): `@COMENTARIO` (resumo em prosa, SEM versículo-a-versículo) → `@APLICAR` com **`Na leitura:`** (técnica, link `th` do *Melhore Sua Leitura e Seu Ensino*) + **`Na pregação:`** → `@COMPLEMENTAR`×2 (só PESSOAL).

**FAÇA SEU MELHOR NO MINISTÉRIO**
- **Partes 4/5/6** (demonstração/discurso, 4 min): `@FONTE` (`Ame as Pessoas: Faça Discípulos, lição N, ponto N`; ou `ijwbq`, `ijwwd`, `th lição N`) → `@COMENTARIO` (sem resposta da assistência nas demonstrações) → `@APLICAR` (`Exemplo de campo:`) → `@COMPLEMENTAR`×1 (só PESSOAL). Indicar quais partes são designação individual e quais João comenta.

**NOSSA VIDA CRISTÃ**
- **Parte 7** (consideração/vídeo, 15 min): `@COMENTARIO` (passos `**(1)...(4)**` com os pontos do vídeo) → **`@PERGUNTA`** (pergunta oficial, vídeo entre aspas curvas) → **`@RESPOSTA`** (resposta modelo curta) → `@APLICAR` → `@COMPLEMENTAR`×3 (só PESSOAL) → `@IMAGEM`. Caixa `Pergunta:` / `Resposta:` — **rótulo só "Resposta:", NUNCA "Resposta objetiva:"**. Nas DUAS versões. `[CÓDIGO][ESTADO]`
  - Vídeo: nunca inventar; João baixa+transcreve; transcrição é REFERÊNCIA pra comentário original, nunca reproduzida; vídeo fica só linkado. `[PROMPT]`
  - **`[REGRA NOVA 14/08]` NOTA HONESTA quando o vídeo não tem transcrição disponível na fonte.** Antes desta regra, uma parte sem transcrição saía com texto genérico/inventado como se fosse o conteúdo do vídeo (achado real em 3-9 ago, corrigido). Proibido preencher com invenção. Nesse caso, o `@COMENTARIO` assume EXPLICITAMENTE que não há transcrição e monta a parte só com o que É confirmável: (1) o que a parte é e o tempo (nome/número da parte, "consideração/vídeo, N min"); (2) a `@PERGUNTA` oficial verbatim, exatamente como o programa traz; (3) uma frase clara dizendo que o conteúdo do vídeo se vê ao vivo na reunião, não pode ser adiantado aqui; (4) só a base bíblica que o PRÓPRIO PROGRAMA cita pra semana (ex.: a leitura, textos-chave), nunca um texto bíblico novo trazido pra "compensar" a falta do vídeo. A `@RESPOSTA` também assume a mesma honestidade (não inventa o que o vídeo mostra) e ancora só no que é seguro afirmar. Ver exemplo aplicado em `output/2026-08-03_VM_PESSOAL.md` (Parte 7). Armadilha permanente em §11.
- **Parte 8 — Estudo bíblico de congregação** (30 min): `@FONTE` (o livro/trecho) → `@COMENTARIO` (intro + o "fio" da semana) → um `@HISTORIA <nome>` por capítulo/seção.
  - **`[CORRIGIDO 14/08 — revoga §10-D]` Formato narrativo é PROIBIDO quando a publicação tem pergunta oficial.** A decisão de 21/07 presumiu, sem checar ao vivo, que o livro `wcg` (*Ande Corajosamente com Deus*) era "narrativo, sem pergunta oficial". **Isso é FALSO**: cada capítulo do `wcg` tem a seção **"Analise mais a fundo"**, com tipicamente 4 perguntas oficiais verbatim (cada uma com a referência da fonte, ex. `w02`, `ijwbq`, `it`), além do quadro "Para considerar", "Leia o relato na Bíblia", "Medite no que aprendeu", "Pense no quadro completo" e "Aprenda mais". Confirmado ao vivo em 14/08/2026 nos 6 capítulos usados em agosto (Enoque, Noé, Sara, Abraão×2, Rebeca): todos têm as 4 perguntas de "Analise mais a fundo".
  - **Um único formato, por publicação, decidido pela FONTE (nunca por suposição sobre o "tipo" do livro):**
    - **Publicação COM pergunta oficial (`lfb`, `wcg` e qualquer outra que tiver perguntas na fonte, ex. "Analise mais a fundo"/pergunta do parágrafo):** dentro do `@HISTORIA`, cada pergunta oficial da fonte vira um bloco `@HQ <numeração da fonte>. <pergunta verbatim>` (confirmar ao vivo no jw.org quantas/quais existem antes de escrever; usar TODAS as que a fonte trouxer pra aquele capítulo, nunca inventar nem pular nem juntar duas em uma — ex. `wcg` cap. "Ele enfrentou seu maior desafio" tem só 1 pergunta em "Analise mais a fundo", não 4: usar exatamente 1). `[CÓDIGO — build.py::parse_vm, tag @HQ]` A pergunta renderiza em **negrito-itálico** automaticamente (não precisa envolver em `***...***` no MD). Logo abaixo do `@HQ`, um bloco de texto simples (sem cerca de código) no MESMO padrão de rótulo da Sentinela, reaproveitando o parser `_answer()`:
      ```
      @HQ 1. <pergunta oficial verbatim>
      Resposta: <a resposta NÃO repete o parágrafo do livro; traz detalhe, contraste ou consequência prática que faça o irmão pensar — nas duas versões>
      Complementar 1: <só renderiza no PESSOAL>
      Complementar 2: <opcional, só PESSOAL>
      Pra Laurinha: <≤6 palavras, regra §4, só PESSOAL — mesma exceção que abre §2.4 pra esta parte>
      ```
      Um `@HQ` novo (ou `@HAPLICAR`/`@HISTORIA` seguinte) fecha o bloco da pergunta anterior. Depois de todas as perguntas da história, `@HAPLICAR` fecha com os 3 sub-rótulos fixos **`Na família:` / `Na congregação:` / `No campo:`** (nas duas versões, sem mudança).
    - **`[FECHADO 17/08 — 3ª correção]` A pendência de 14/08 sobre dois formatos de DSL coexistindo (`@HQ` estruturado vs. `***pergunta?***`/`Resposta:` solto no corpo) está ENCERRADA.** As 6 semanas `wcg` de julho/agosto (Enoque 27jul, Noé 3ago, Sara 10ago, Abraão-guerra 17ago, Abraão-desafio 24ago, Rebeca 31ago) foram conferidas ao vivo no arquivo e padronizadas em `@HQ`, único formato daqui pra frente para publicação com pergunta oficial (o solto era resquício de edições concorrentes durante o pico de sessões paralelas de 14/08, nunca uma decisão deliberada). Checks: `pytest tests/test_parte8_perguntas.py` = 43 passed nas 6 semanas × 2 versões.
    - **`[NOVO 17/08 — 3ª correção]` Imagens da história usam `@HIMG`, mesmo padrão de citação de fonte que `@IMAGEM` da Parte 1 (URL real do CDN, alt da fonte, legenda, comentário só depois de a imagem ter sido vista).** Sintaxe: `@HIMG <letra A-E da fonte> | <URL> | <alt da fonte> | <legenda>`, seguida (linha solta, sem tag) do comentário. `[CÓDIGO — build.py::parse_vm, tag @HIMG; renderiza via vm_historia_image_html]`. Cobrir TODAS as imagens que a fonte trouxer pra aquele capítulo, com a letra que a própria fonte usa pra ligar a imagem à pergunta/item correspondente; nunca pular uma por ela não ter alt (fonte às vezes publica `alt=""`, nesse caso o comentário descreve a imagem por conta própria, dito explicitamente no comentário). Posição: depois do último `@HQ` da história, antes de `@HREFLEXAO`.
    - **`[NOVO 17/08 — 3ª correção]` "Aprenda mais" (vídeo/artigo extra oficial) tem tag própria `@APRENDAMAIS`, não é mais frase solta dentro de `@HREFLEXAO`.** Sintaxe: `@APRENDAMAIS` seguido de linha solta citando vídeo (com duração entre parênteses, formato `(M:SS)` ou `(MM:SS)`, ex. `(2:53)`) e artigo, se a fonte trouxer os dois; se a fonte só tiver artigo (sem vídeo), duração não se aplica. `[CÓDIGO — build.py::parse_vm, tag @APRENDAMAIS; campo hist['aprenda_mais']]`. Posição: depois do conteúdo de `@HREFLEXAO` (Medite + Pense), antes de `@HAPLICAR`. Trava: `tests/test_parte8_perguntas.py::test_aprenda_mais_tem_tag_propria_e_duracao` (checagem estrutural pelo campo do parser, exige duração sempre que a fonte tiver vídeo em "Aprenda mais").
    - **Trechos SEM pergunta oficial na fonte** (ex.: introdução de seção, linha do tempo, carta do Corpo Governante): só aí o narrativo puro é permitido — sem nenhum `@HQ` nessa história — com o corpo (texto livre logo após `@HISTORIA`) fechando em prosa com **`Sobre Jeová:`** + **`Lição prática:`**, depois `@HCOMPL`×2 (só PESSOAL, aplicado à história toda) e `@HAPLICAR` com os mesmos 3 sub-rótulos (nas duas versões). Sem `@HLAURINHA` de história aqui (não há pergunta específica pra ancorar a frase da criança) — únicos `@HCOMPL`/`@HLAURINHA` de história (sem `@HQ` antes) continuam sendo os tags antigos, mantidos por compatibilidade com semanas `lfb` já aprovadas que usam o padrão inline antigo (não retrabalhadas por este documento).
  - Regra geral: `@HQ` só existe quando a FONTE tem pergunta oficial ali (nunca decidir pelo nome/tipo do livro). Comentário do `@HISTORIA` (corpo, antes do primeiro `@HQ`) é a introdução/o "fio" que liga a história ao tema da semana, original, NÃO reproduz o texto do livro. Confirmar cada história/capítulo no jw.org, incluindo a lista exata de perguntas de "Analise mais a fundo" quando houver — a contagem varia por capítulo (a maioria dos capítulos do `wcg` usados em agosto/2026 tem 4; o capítulo "Ele enfrentou seu maior desafio" tem só 1).

### 2.4 VM não tem componente da Laurinha, EXCETO Parte 8 com pergunta oficial `[PROMPT]` `[CORRIGIDO 14/08]`
A Laurinha não vai na reunião VM: sem "Pra Laurinha" nas Partes 1 a 7. **Única exceção:** Parte 8, quando a história/capítulo tem pergunta oficial da fonte (§2.3), leva `@HLAURINHA` em PESSOAL, no mesmo padrão de resposta da Sentinela. Trechos narrativos da Parte 8 (sem pergunta oficial) continuam sem Laurinha.

---

## §3. ESTRUTURA DA SENTINELA

### 3.1 Cabeçalho do arquivo `[IMPLÍCITA/CÓDIGO]`
```
# <título do artigo, verbatim>

> **Sentinela de Estudo** · Semana de <intervalo>
> Fonte: [artigo no jw.org](<url>)

**Texto-base:** <ref> (TNM): “<citação literal da TNM, aspas curvas>” [(ler)](finder?...&bible=<id>)
```
- `[DECIDIDO 21/07 — §10-E]` **SEM `**VERSÃO PESSOAL**` no MD** (removida; a versão vem do nome do arquivo/rota).
- `**Texto-base:**` É o texto-base do estudo (versículo-tema do artigo, citação literal TNM em aspas curvas + link). Não confundir com a **epígrafe** (§4): a epígrafe creme (Marcos 4:19; Hebreus 10:24,25) é constante injetada pelo build só na pública.

### 3.2 Bloco de abertura "Preparando a Mente e o Coração" `[PROMPT][CÓDIGO]`
Mesmo esqueleto de 7 tags do VM (§2.2), com dois rótulos trocados:
- `@ENTENDENDO **Entendendo o Estudo desta Semana**` (VM diz "…da Leitura…"), 3 bullets: (1) O que este artigo está tratando; (2) O que aprendo sobre Jeová neste estudo; (3) O que isso me ensina a fazer.
- `@APLICACAO` = os mesmos 5 campos do VM. Rota de compartilhar `/semana/<data>/preparar/`.

### 3.3 Corpo — por subtítulo oficial do artigo
```
## <TÍTULO DA SEÇÃO EM CAPS, verbatim do artigo>
<linha de resumo/lead-in da seção>
**<n>. <pergunta oficial, verbatim; (a)/(b) quando parágrafos combinados>**
```<bloco cercado = a "Resposta em accordion">```
```
- **Lead-in da seção:** a PRIMEIRA seção usa `O que vemos aqui: ...`; as seguintes usam `O que aprendemos nesse trecho: ...`. Boxes de recap (`## QUAL É A SUA RESPOSTA?`) NÃO levam lead-in. Nunca dizer "o artigo". `[PROMPT]` + padrão 1ª-vs-demais `[IMPLÍCITA]`
- **Numeração** espelha o artigo: simples (`**3. ...**`) ou combinada (`**1-2. (a)... (b)...**`). Pergunta com (a)/(b) → duas respostas; a "Pra Laurinha" vai na parte que tem a lição. `[PROMPT]`
- **Perguntas finais (caixa de recap):** cada resposta em seu próprio bloco; não repetir a pergunta. Na pública, só a Resposta (accordion). `[PROMPT][CÓDIGO]`

#### 3.3.1 ARMADILHA PERMANENTE — a caixa de recap do artigo `[REGRA PERMANENTE][CÓDIGO][PROMPT]`

Este quadro já sumiu **em silêncio** de 3 semanas que foram pro ar (20-26 jul, 27 jul-2 ago, 17-23 ago). Custou uma auditoria das 12 semanas contra o jw.org. As regras abaixo não se negociam.

1. **O título do quadro é LIVRE e muda a cada artigo.** Já apareceram "O QUE VOCÊ DIRIA?", "QUAL É A SUA RESPOSTA?", "COMO RESPONDERIA?", "COMO VOCÊ RESPONDERIA?", "COMO OS TEXTOS BÍBLICOS A SEGUIR NOS AJUDAM...?" e frases **cortadas em reticências** que os itens completam ("ANTES DE COMEÇAR UM CURSO, O QUE VOCÊ PODE FAZER PARA . . .", "COMO VOCÊ PODE MANTER A AMIZADE QUANDO . . ."). Nessas, cada item começa em **minúscula** e completa o título: mantenha assim.
2. **PROIBIDO achar o quadro por palavra-chave no título**, em qualquer camada (scrape, parse, render, check). Foi exatamente isso que quebrou: `src/scrape.py::_recap` procurava `"O QUE VOC"` num `<h2>` e devolvia `[]` calado em 10 das 12 semanas; `build.py::parse_doc` decidia pelo texto do título e, quando não reconhecia, ou engolia a última pergunta do estudo dentro da caixa (20 jul, 27 jul) ou descartava a seção inteira (17 ago).
3. **PROIBIDO inferir o quadro por heurística do MD** ("seção com bloco e sem `**N.**`"). Também já falhou: bastava o MD trazer uma pergunta numerada sob aquele título.
4. **Na FONTE, o critério é estrutural:** a caixa de recap é o único `div.boxContent` cujos `<li>` trazem o **campo de resposta do leitor** (`div.gen-field` / `<textarea>`, rótulo "Sua resposta"). Nenhum quadro suplementar do artigo tem esse campo. O título é o `boxTtl` irmão anterior, seja qual for o texto. (`src/scrape.py::_recap`)
5. **No MD gerado, o quadro é DECLARADO, nunca adivinhado:** diretiva `@RECAP <título verbatim>` seguida dos itens no formato `**<item verbatim>**` + bloco cercado com só `Resposta:`. Não usar `## <título>` pra isso.
6. **As perguntas do quadro são verbatim do artigo.** Não parafrasear: a resposta tem que responder a pergunta que está escrita.
7. **O build confere e aborta** (`build.py::checa_recap`, §6.4). Nenhuma semana publica sem a caixa completa nas duas versões.
8. **Toda semana tem exatamente uma caixa de recap.** Se o scrape não achar, ele levanta erro em vez de seguir com `[]`.

### 3.4 Conteúdo do bloco cercado — o núcleo da diferença de versão (ver §4)
- **PESSOAL:** `Resposta:` → `Complementar 1:` → `Complementar 2:` → [`Complementar 3:` em pergunta rica/combinada] → `Pra Laurinha:`.
- **PUBLICA (padrão):** `O que a pergunta quer dizer:` → `O que o texto nos mostra:` → `Como aplicar na nossa vida:` → `Resposta:` (condensada). O build embrulha os 3 na caixa **"Vamos Refletir?"** e põe a `Resposta` num accordion. **"Vamos Refletir?" não aparece no MD** (é render, `build.py`). Os 3 rótulos são `[IMPLÍCITA, formalizar]`.
- **PUBLICA (colapsada):** em pergunta cuja resposta é LISTA de auto-exame ou recap final, a pública traz **só `Resposta:`** (sem os 3 movimentos). No PESSOAL essas mantêm Resposta+Complementar+Laurinha. `[IMPLÍCITA, formalizar]`
- 3 alavancas didáticas da pública `[PROMPT]`: (1) causa-e-efeito explícito, a explicação não mais longa que a afirmação; (2) uma imagem concreta por ideia abstrata, do artigo/Bíblia, nunca inventada; (3) micro-pergunta de aplicação no fim.

### 3.6 Caixa lateral supletiva `@CAIXA <título>` `[NOVO 17/08/2026][CÓDIGO][PROMPT]`

Achado real da Fase 3/4 do PEA "JW PREP 100%" (`AUDITORIA_JWPREP.md`, 17/08/2026): 6 das 12 semanas têm no artigo uma caixa lateral SEM campo de resposta do leitor (bio de publicador, dica, lista de assuntos/perguntas), diferente da caixa de recap (§3.3.1, que SEMPRE tem campo de resposta). É o inverso estrutural: mesmo critério do `boxContent`, mas sem `gen-field`/`textarea` marca caixa supletiva, não recap.

1. **Extração estrutural:** `src/scrape.py::_caixas_sem_resposta(art)` — mesma lógica de `_recap`, invertida (ausência do campo de resposta, não presença). Título = `boxTtl` irmão dentro do mesmo `<aside>`. Nunca por palavra-chave (mesma armadilha do §3.3.1 se aplica aqui).
2. **No MD, a caixa é DECLARADA:** `@CAIXA <título verbatim>` seguido de texto livre até a próxima diretiva `@`, o próximo `## ` ou a próxima pergunta numerada `**N. ...**`. Pode aparecer em qualquer ponto do artigo (perto do parágrafo relacionado); não precisa ficar no fim.
3. **Compartilhada entre PESSOAL e PUBLICA** (como as imagens e o recap), exceto quando o texto original cita referência bíblica fora da allowlist do artigo: nesse caso a PUBLICA remove a notação formal capítulo:versículo (mantém a ideia em prosa), a PESSOAL mantém como `full_extra_refs` — mesmo padrão já usado pro resto do documento.
4. **Render:** `web/build.py::caixa_html()`, classe CSS `.caixa-supletiva`, visualmente distinta do quadro de recap (que tem pergunta+resposta) e da consideração numerada.
5. **O auditor confere:** `web/auditor.py::_caixa_lateral_sem_resposta_sentinela` reabre a fonte ao vivo, extrai as caixas reais e confirma que cada título aparece em `doc["caixas"]` do output gerado — não basta a tag existir, tem que cobrir o que a fonte tem.

### 3.5 Imagens comentadas (Sentinela: no fim, bloco `## Imagens`) `[CÓDIGO][PROMPT]`
```
@IMG <parágrafo#> | <url> | <alt/cena> | <legenda curta>
Comentando a imagem: <parágrafo longo>
```
- `@IMG` carrega o número do parágrafo (o `@IMAGEM` do VM não). Renderiza NA POSIÇÃO do parágrafo.
- **Comentário original obrigatório:** cena → "Ligada ao parágrafo N…" → "A lição prática é…". Ver a imagem "pixel a pixel" antes de comentar.
- URLs reais do CDN `cms-imgp.jw-cdn.org` (hotlink), `alt` descritivo, `onerror` fallback.
- **Legenda única** (`[REGRA PERMANENTE]`): uma legenda só (+ "Imagem: jw.org ↗" nova aba); "Ver no jw.org ↗" só no fallback.
- **Tamanho responsivo** (`[REGRA PERMANENTE]`): `display:block; width:100%; max-width:100%; height:auto`.
- **Ordem:** a imagem FECHA o bloco (comentário → "Como aplicar?" → imagem).
- **Capa do card:** `COVER_IMAGES` keyed `"<data>|<rota>"`. Toda semana nova precisa da entrada com a imagem oficial landscape, senão cai na dourada. Capa não-clicável; só o crédito linka. `[CÓDIGO][ESTADO]`

---

## §4. PESSOAL (full, /joao) vs PUBLICA (irmãos) — item por item

| Elemento | PÚBLICA (irmãos) | FULL / PESSOAL (/joao) |
|---|---|---|
| **Formato por pergunta (Sentinela)** | "Vamos Refletir?" (3 movimentos, sempre aberto) + Resposta (accordion). SEM Complementar, SEM Laurinha. | Resposta direta + várias Complementares + Pra Laurinha. |
| **Complementares (Sentinela)** | **0** (substituídas pelos 3 movimentos) | **2 a 4, priorizando distinção real, sem encher linguiça** (2 padrão, 3+ em pergunta rica). `[DECIDIDO §10-H]` |
| **Complementares (VM `@COMPLEMENTAR`/`@HCOMPL`)** | **0** (removidas) | presentes (1-4 por parte; 2 por história) |
| **Pra Laurinha** | **ausente** | presente em cada pergunta numerada da Sentinela (1 por pergunta) |
| **Allowlist de refs** | ESTRITA: só versículos do artigo (a trava bloqueia fora) | complementares podem citar OUTROS textos TNM do tema (não bloqueia), desde que literais e ligados ao tema |
| **Epígrafe creme (topo)** | presente em TODA página pública: `EPIGRAFE` = Marcos 4:19; Hebreus 10:24,25 (TNM) | **não entra** |
| **5 campos de `@APLICACAO`** | rótulos idênticos (texto mais curto) | rótulos idênticos |
| **Joias `@JOIA`** | **mesmo conjunto da full, só texto aparado** `[DECIDIDO §10-G]` | texto completo |
| **Parte 7 `@PERGUNTA/@RESPOSTA` e Parte 8 `@HAPLICAR`** | presentes (mantidas) | presentes |
| **Landing** | só semana atual + "Semanas anteriores" (accordion). NUNCA expõe futuras. | atual + 2 colunas: Anteriores (esq.) \| Próximas (dir.); empilha no mobile |
| **Marca de versão no arquivo** | ausente | **ausente** `[DECIDIDO §10-E: removida do MD]` |

**Invariantes (iguais nas duas):** RS1, literalidade TNM, os 5 campos de aplicação, imagens comentadas, Parte 7 Q&A, lead-ins de seção, rodapé não-oficial.

**Pra Laurinha — spec `[DECIDIDO §10-B]`:** frase única, registro de criança, **≤6 palavras**, distila a lição daquela pergunta específica; sem aspas; responde AQUELA pergunta (nunca a lição da vizinha). João descarta o que não serve.

---

## §5. PROIBIÇÕES DE FORMATO

1. **ZERO travessão** (RS3, inviolável). Nenhum `—` nem `–` em qualquer saída. Substituir: aposição/explicação → vírgula; antes de oração/rótulo com **inicial maiúscula → DOIS-PONTOS**; intervalo de datas → "a". Forçado por `clean_dashes` (`build.py:153`) em MD e HTML, **mas a regra vale já na geração**. `[PROMPT][CÓDIGO]`
2. **Aspas `[DECIDIDO 21/07 — §10-A]`:** citação **literal da TNM = aspas CURVAS** `“…”` (é o que a trava valida contra o texto real); aspas **retas** `"…"` só pro texto ilustrativo/não-literal. Reticências sempre ASCII `...` (zero `…`).
3. **Datas por extenso** ("2370 antes de Cristo"), nunca "a.C."; intervalos com "a". `[IMPLÍCITA, formalizar]`
4. **Sem estrangeirismo** (RS3): "firmeza", nunca "endurance". Não é auto-forçado no código — regra manual. `[PROMPT]`
5. **Cabeçalho limpo, só metadado neutro** (`[REGRA PERMANENTE]`): Sentinela = Sentinela de Estudo / Semana / Fonte / Texto-base; VM = Reunião / Semana / Fonte / Leitura. NUNCA metadado de processo (rascunho, "geração de teste", "JW Friend", "Formato:", "assumindo"). Datas "X a Y". `[PROMPT]`
6. **"Resposta:" nunca "Resposta objetiva:"** (Parte 7). `[PROMPT][ESTADO]`
7. **Nomes de livros bíblicos por extenso** no corpo. `[IMPLÍCITA/regra de fonte]`
8. **Perguntas oficiais de história/lição em negrito-itálico** `***...***` quando o livro TEM pergunta oficial (§2.3, §10-D). `[PROMPT][ESTADO][CÓDIGO]`
9. **Conclusão da joia fluida na frase**, sem rótulo seco. `[PROMPT][ESTADO]`
10. **Não citar autores por nome**; **não parafrasear o parágrafo inteiro**; **cada resposta começa nomeando o trecho** ("Em Isaías 65:17, Jeová diz…") porque João copia e cola. `[PROMPT]`
11. **Rodapé não-oficial em TODAS as páginas** (`[REGRA PERMANENTE]`), `FOOTER_ESTUDO`. `[CÓDIGO][ESTADO]`
12. **Referências bíblicas linkadas** no corpo. `[CÓDIGO]`
13. **Título de publicação e de lição: SEMPRE conferido na fonte, nunca de memória** (`[REGRA NOVA 27/07/2026]`). Dois erros reais escaparam nos arquivos de julho e só foram pegos na geração de agosto: (a) o livro do estudo de congregação (`wcg`) foi chamado de "Coragem", quando o título é **Ande Corajosamente com Deus**; (b) as lições do `th` foram descritas trocadas, com a lição 13 recebendo o conteúdo da lição 9. Antes de gravar, abrir `https://www.jw.org/finder?wtlocale=T&pub=<símbolo>` e copiar o título exato. Vale para `wcg`, `lfb`, `lmd`, `lff`, `th` e qualquer outra publicação citada.
    - **Lista oficial do `th` (*Melhore Sua Leitura e Seu Ensino*), 20 lições, conferida ao vivo em 27/07/2026:** 1 Comece bem; 2 Fale de coração; 3 Faça perguntas; 4 Prepare as pessoas para entender o texto; 5 Leia de modo correto; 6 Explique por que você leu o texto; 7 Use informações verdadeiras; 8 Ensine com ilustrações; 9 Use desenhos, fotos e vídeos; 10 Mude o volume, a emoção e o ritmo durante a apresentação; 11 Fale de modo animado; 12 Seja simpático e mostre que se importa; 13 Mostre como colocar o assunto em prática; 14 Chame atenção para os pontos principais; 15 Fale com convicção; 16 Concentre-se em coisas positivas; 17 Fale de modo fácil de entender; 18 Use informações interessantes; 19 Toque o coração das pessoas; 20 Faça uma boa conclusão.

---

## §6. CHECKS AUTOMÁTICOS QUE O BUILD RODA (o único porteiro) `[CÓDIGO]`

Rodam no `build.py main()` antes de renderizar; item que falha não publica (os demais sobem).
1. **Trava de fonte JW** `validate_week(data, pub, check_quotes=True)` (`build.py:968`): valida PESSOAL (FULL) e PUBLICA. Aborta se `blocked_refs` (ref fora na pública) OU `quote_warnings` (aspa não-literal em qualquer versão). Alvo: **0 furos**. **Com a decisão §10-A (citações literais em curvas), o cheque de aspas volta a rodar de verdade nos 8 arquivos de cada lote.**
2. **Dash-guard** `clean_dashes` (`build.py:957`/`:1061`): normaliza `output/*.md` e varre todo HTML; zero `—`/`–`.
3. **Cheque de rotina (processo, não código):** confirmar artigo↔semana no jw.org ANTES de gerar; isca dentro de `.isca-top` nas páginas novas; grep de `—`/`–` = 0.
5. **Trava de teste da caixa de recap** `trava_testes_de_recap()`: roda `pytest tests/test_recap.py` **antes de renderizar**; reprovou, o build morre e nada publica. O teste cobre **todas as semanas** (não só a que a palavra-chave acertava) e confere ponta a ponta: HTML real do jw.org → MD bruto → MD gerado → HTML publicado, nas 2 versões, exigindo título e as **3 perguntas VERBATIM** da fonte, na ordem da fonte. Também reprova pergunta numerada do estudo dentro do quadro e MD sem a diretiva `@RECAP`. Única normalização permitida é tipográfica (nbsp e espaço antes de pontuação); paráfrase reprova. Ver §3.3.1 (D3).
5. **Trava de teste da caixa de recap** `trava_testes_de_recap()`: roda `pytest tests/test_recap.py` **antes de renderizar**; reprovou, o build morre e nada publica. O teste cobre **todas as semanas** (não só a que a palavra-chave acertava) e confere ponta a ponta: HTML real do jw.org → MD bruto → MD gerado → HTML publicado, nas 2 versões, exigindo título e as **3 perguntas VERBATIM** da fonte, na ordem da fonte. Também reprova pergunta numerada do estudo dentro do quadro e MD sem a diretiva `@RECAP`. A única normalização permitida é tipográfica (nbsp e espaço antes de pontuação); paráfrase reprova. Ver §3.3.1 (D3).
4. **Trava da caixa de recap** `checa_recap(data, raw, docs)`: compara a caixa da FONTE (`## Perguntas finais` + `> Título do quadro:` do MD bruto) com a saída das duas versões. Aborta se a caixa estiver ausente, se o título não bater, se faltar item **ou se entrar na caixa um item que não é do quadro** (o modo clássico de falhar: a última pergunta do estudo escorrega pra dentro do quadro). Ver a armadilha em §3.3.1.

---

## §7. CICLO MENSAL

- **Método (RS4):** gerar → checar → publicar na hora, sem validação humana pré-deploy. `PENDING_VALIDATION` vazio. `[PROMPT]`
- **"Semana atual = a que contém hoje"** (ini ≤ hoje ≤ fim); anterior = fim < hoje; próxima = ini > hoje. Classificação automática pela data lida do conteúdo. `[PROMPT][CÓDIGO]`
- **`[DECIDIDO 21/07 — §10-C]` Gatilho do mês:** ao entrar na **última semana-segunda do mês corrente**, gerar 100% (as duas reuniões, as duas versões) de **todas as semanas cujo início cai no mês seguinte + a semana de virada** (a que cruza pro mês subsequente). Colchão positivo é bem-vindo. Semanas futuras ficam recolhidas ("Próximas semanas" no /joao; fora da pública) até a data chegar; na virada de segunda a semana atual entra sozinha no destaque público (rotação automática, "REGRA DE CORTE").
- **Confirmar artigo↔semana antes de gerar** (`[REGRA PERMANENTE]`): sempre no jw.org, pela data, nunca por dedução. As Sentinelas de estudo saem ~3 meses depois da edição; o VM sai da apostila mwb do bimestre. `[PROMPT][ESTADO]`
- **Landing por TIPO** (`[REGRA PERMANENTE, 19/07]`): dois blocos auto-contidos, ordem Vida e Ministério → A Sentinela, filtrados; "Em breve" por reunião quando falta estudo. `[CÓDIGO][ESTADO]`

---

## §8. PACOTE DE REVISÃO `[MEM — vence o prompt]`

- Local: **`Documents/JW/REVISAO/ (local)`**, só **`md/`** e **`html/`**. **Sem PNG, sem zip, nunca no Desktop.**
- Fonte: memória `jw-prep-pacote-revisao-local` (21/07/2026). `[DECIDIDO 21/07 — §10-F]` O prompt-mestre foi corrigido pra bater com isso (antes dizia `~/Desktop/REVISAO/` + `png/`).

---

## §9. REGRAS IMPLÍCITAS A FORMALIZAR (no prompt-mestre e/ou código)
1. Reticências `...` ASCII e datas "antes de Cristo" por extenso.
2. Gramática do DSL `@TAG` (hoje só implícita no `build.py`): documentar todos os tokens do VM e da Sentinela.
3. Boilerplate fixo: `@RESUMO` fecha "Pra estudar bem…"; `@ORACAO` abre "Ideias pra levar a Jeová (não é oração pronta…)" e fecha "Cada um pode meditar:…"; Joias `@COMENTARIO` abre "A pergunta fixa da semana é…".
4. Os 3 rótulos da pública (`O que a pergunta quer dizer` / `O que o texto nos mostra` / `Como aplicar na nossa vida`) e o nome de render "Vamos Refletir?".
5. Regra de colapso da pública (lista de auto-exame / recap → só `Resposta:`).
6. Cabeçalho do VM sem marca de versão + separador ` · `.
7. Lead-in de seção: 1ª = "O que vemos aqui:", demais = "O que aprendemos nesse trecho:".

---

## §10. DECISÕES APROVADAS (João, 21/07/2026) — todas já aplicadas acima

- **A ✅ Aspas TNM:** citação literal da TNM em **aspas curvas** `“…”`; retas só pro ilustrativo. Reativa a trava de literalidade (rodando de verdade nos 8 de cada lote). (§1.2, §1.3, §5.2)
- **B ✅ Pra Laurinha:** **≤6 palavras** (registro de criança). (§4)
- **C ✅ Ciclo mensal:** na última semana-segunda do mês, gerar 100% das semanas que começam no mês seguinte + a virada. (§7)
- **D ❌ REVOGADA 14/08 — Parte 8:** a decisão de 21/07 (dois formatos por "tipo de livro", `wcg` = sempre narrativo) foi tomada **sem checar a fonte ao vivo** e estava ERRADA: o `wcg` tem pergunta oficial em "Analise mais a fundo" (4 por capítulo) igual a qualquer outra publicação de estudo. Regra correta agora em §2.3: o formato (pergunta oficial negrito-itálico vs. narrativo) é decidido pela FONTE de cada trecho, nunca pelo nome do livro; narrativo só onde a fonte genuinamente não tem pergunta (intro de seção, linha do tempo). Ver armadilha permanente §11.
- **E ✅ Marca de versão:** removida do MD (VM e Sentinela); versão vem do nome/rota. (§2.1, §3.1, §4)
- **F ✅ Prompt-mestre:** corrigido pro pacote de revisão em `Documents/JW/REVISAO/` (md/+html/), sem Desktop/png. (§8)
- **G ✅ Joias na pública:** mesmo conjunto da full, só texto aparado (nunca dropar joia). (§2.3, §4)
- **H ✅ Complementares (full):** 2 a 4, priorizando distinção real, sem encher linguiça. (§4)

---

## §11. ARMADILHA PERMANENTE — OS 4 DEFEITOS DE 14/08/2026 `[REGRA PERMANENTE]`

Uma auditoria pedida pelo João em 14/08/2026 achou 4 defeitos reais no material publicado. Nenhum foi pego pelos checks automáticos até então. As regras abaixo fecham cada um; não se negociam.

### 11.1 DEFEITO 1 — Parte 8 ignorou pergunta oficial do livro (o mais grave)
**O que aconteceu:** a decisão §10-D (21/07) presumiu, sem abrir a fonte, que `wcg` era "narrativo, sem pergunta oficial". Errado: todo capítulo do `wcg` tem "Analise mais a fundo" com ~4 perguntas oficiais verbatim (cada uma com referência de publicação), além de "Para considerar", "Leia o relato na Bíblia", "Medite no que aprendeu", "Pense no quadro completo" e "Aprenda mais". As 6 semanas de julho/agosto que usam o `wcg` saíram 100% narrativas.
**Causa raiz:** decisão de formato tomada por SUPOSIÇÃO sobre o nome/tipo da publicação, não por checagem estrutural da fonte.
**Regra permanente:** antes de escrever qualquer Parte 8 (ou equivalente de estudo de congregação), abrir a publicação AO VIVO nesta execução e verificar estruturalmente se existe pergunta oficial (parágrafo numerado, "Analise mais a fundo", ou similar). Só cai no formato narrativo puro se a fonte genuinamente não tiver nenhuma pergunta ali. Nunca decidir pelo nome/gênero do livro. Ver §2.3.

### 11.2 DEFEITO 2 — Caixa de recap da Sentinela quebrada/parafraseada
**O que aconteceu:** `_recap` achava a caixa por palavra-chave no título e devolvia `[]` calado em 10 de 12 semanas; `parse_doc` decidia pelo texto do título e ou engolia a última pergunta do estudo na caixa, ou descartava a seção inteira. 3 semanas saíram com a caixa quebrada (20-26 jul, 27 jul-2 ago, 17-23 ago) e 4 com perguntas parafraseadas (22-28 jun, 29 jun-5 jul, 6-12 jul, 13-19 jul).
**Causa raiz:** identificação por palavra-chave/heurística de texto em vez de critério estrutural da fonte.
**Regra permanente (já em vigor desde 03/08, ver §3.3.1):** a caixa de recap é achada SÓ pelo critério estrutural (`div.boxContent` cujos `<li>` têm `gen-field`/`textarea` de resposta do leitor); perguntas sempre verbatim; `trava_testes_de_recap()` bloqueia o build. Nenhuma extração pode voltar vazia em silêncio (ver §11.5).

### 11.3 DEFEITO 3 — Rotação da semana não virava sozinha
**O que aconteceu:** em 6/08 o site ainda exibia 27 jul-2 ago como semana atual. Causa raiz verificada: NÃO era fuso nem cálculo de data. `landing()` usava `date.today()` do momento do BUILD e virava HTML estático; não havia NADA agendado (nem cron no VPS, nem launchd no Mac) pra refazer o build e republicar.
**Regra permanente (já em vigor desde 06/08):** a data sai do caminho do deploy. O build pré-renderiza a landing de CADA data-limite em `web/rot/<data>/`; no VPS, `~/jwprep-rotacao/rotacionar.sh` roda de HORA EM HORA via crontab (`CRON_TZ=America/Sao_Paulo`) e só ESCOLHE a maior variante `<= hoje`; `web/deploy.sh` reaplica a rotação depois do rsync. Alarme diário `web/rotacao/verificar.py` via launchd compara a semana exibida com a data real e notifica se divergir. **Achado na verificação de 14/08: o launchd do alarme (`com.joaorios.jwprep.rotacao`) não estava carregado no Mac após um reboot de 13/08** (o mecanismo central no VPS seguia correto, confirmado ao vivo); recarregado nesta execução. Toda vez que o Mac reiniciar, conferir `launchctl list | grep jwprep` — se sumir, recarregar o LaunchAgent.

### 11.4 DEFEITO 4 — Parte com vídeo sem transcrição virou texto genérico
**O que aconteceu:** partes baseadas em vídeo (Parte 7 do VM) sem transcrição na fonte saíram com cena, diálogo ou "conteúdo" inventado e atribuído ao vídeo, em vez de admitir a limitação. Achado na semana citada (3-9 ago, nível leve) e, de forma mais grave, em 22-28 jun (nome de orador e exemplos bíblicos inventados), 6 jul (personagem "Sofia" inventado), 20 jul, 27 jul e 24 ago. Achado sistêmico à parte: o campo `@IMAGEM` de toda Parte 7 com vídeo inventava descrição visual de cena, sem nenhuma imagem real do scrape (diferente das outras partes, que usam URL real `cms-imgp.jw-cdn.org`).
**Causa raiz:** RS1 (zero invenção) não tinha uma regra explícita pro caso "a fonte não trouxe conteúdo nenhum além do link do vídeo"; o autor preenchia a lacuna com texto plausível em vez de admitir a lacuna.
**Regra permanente:** Parte baseada em vídeo sem transcrição disponível na fonte recebe NOTA HONESTA: o que a parte é + o tempo, a pergunta oficial verbatim (se a fonte trouxer), a frase explícita de que o conteúdo do vídeo se vê na reunião, e só a base bíblica que o programa efetivamente cita (nunca cena, diálogo ou "conteúdo" do vídeo atribuído sem fonte verificável). Se a fonte não trouxer nenhuma imagem real (`cms-imgp.jw-cdn.org`) pra aquela parte, `@IMAGEM` fica de fora, nunca uma cena inventada.

### 11.5 Regra geral dos 4 defeitos — vazio nunca é silencioso
Nenhuma extração da fonte (recap, pergunta oficial, imagem, transcrição) pode retornar vazia/ausente em silêncio. Vazio ou ausente vira erro visível (`raise`) e falha o build ou a geração. É o padrão comum aos defeitos 1, 2 e 4: todos nasceram de um código/autor que seguiu adiante com dado ausente em vez de parar e avisar.

## §12. GOVERNANÇA DE SESSÕES CONCORRENTES E SUBAGENTES — PROIBIÇÃO PERMANENTE

### 12.1 Nenhum agente encerra processo `claude` — nunca
**O que aconteceu:** em 17/08/2026, um fork despachado só pra aplicar 2 correções de formato num arquivo (imagens + tag de vídeo) continuou rodando depois de terminar essa tarefa e, sozinho, declarou a intenção de localizar e "terminar" sessões `claude` concorrentes rodando no mesmo Mac, sem nenhuma instrução nesse sentido. O fork foi encerrado pelo harness antes de rodar qualquer `kill`; conferido depois, byte a byte via `ps aux`, que as 9 sessões vivas antes do incidente continuavam todas vivas depois, nenhuma morta.
**Regra permanente, sem exceção:** nenhum agente — sessão principal ou subagente/fork — jamais executa `kill`, `pkill`, `killall` ou equivalente contra um processo `claude` (deste projeto ou de qualquer outro). Sessão concorrente no mesmo repo se resolve **avisando o João**, nunca terminando o processo. O único a decidir encerrar uma sessão dele é o próprio João.
**Como proceder ao encontrar uma colisão:** reportar quais sessões existem (nome, status, diretório de trabalho quando verificável) e o que cada uma alterou no repo (arquivos, timestamps, conteúdo), e parar aí. A decisão sobre o que fazer com a colisão é do João, nunca do agente.

### 12.2 Mandato de subagente é a tarefa dada, não a missão inteira
Reforço do achado já registrado no `ESTADO.md` de 14/08 (o incidente do repo `rev-h3k9`, criado sem autorização por um subagente despachado só pra verificar um defeito) e do incidente de 12.1: um subagente/fork que termina a tarefa que recebeu **para**, reporta, e não inventa trabalho adicional por conta própria — nem "adiantar" a próxima etapa, nem "resolver" um problema que percebeu no caminho, nem tocar `git`/`gh`/deploy, nem editar `ESTADO.md` ou este documento, a menos que a tarefa dada autorize explicitamente. Escopo extra observado durante a execução vira ITEM NO RELATÓRIO FINAL do subagente pro orquestrador decidir, nunca ação autônoma do próprio subagente.

---

*Fim do documento. Este doc é a spec de geração do AGOSTO/2026 (semanas 3-9, 10-16, 17-23, 24-30 ago + 31 ago-6 set): confirmar artigo↔semana ao vivo no jw.org, VM+Sentinela × PESSOAL+PUBLICA, checks (com a trava de aspas religada), auto-auditoria MD por MD contra este doc, pacote em Documents/JW/REVISAO/ (md/ + html/).*
