# Revisão — PEA "JW PREP 100%" (Fases 1-5)

Corpus completo atual (71 MDs de conteúdo, 12 semanas x Sentinela/VM x
PESSOAL/PUBLICA quando geradas) + os 3 documentos novos deste ciclo:
`INVENTARIO_JWPREP.md` (Fase 1, estrutura da fonte ao vivo), `AUDITORIA_JWPREP.md`
(Fase 4, relatório do auditor obrigatório rodado contra as 12 semanas reais) e
`REGRAS_JWPREP_CONSOLIDADO.md` (spec de geração, com a proibição permanente de
governança de sessões registrada em §12).

## O que mudou neste ciclo

1. **Rodada 3** (Parte 8, livro `wcg`): imagens via `@HIMG` (padrão da Parte 1:
   URL real, alt, legenda, comentário só depois de ver a imagem), formato `@HQ`
   padronizado nas 6 semanas, tag `@APRENDAMAIS` própria com duração do vídeo.
2. **Fase 2 — transcrição LOCAL de vídeo** (whisper.cpp, Metal, sem API paga):
   as 8 semanas com vídeo real na Parte 7/8 foram transcritas localmente e o
   comentário reescrito com o conteúdo real da cena, nunca invenção. Nome
   próprio de personagem só entra no texto quando a fonte tinha legenda ou
   descrição oficial pra conferir a grafia; sem isso, descreve por papel.
   3 casos reais pegos pela regra "transcrição não é fonte": um nome (Ruth →
   Rute), três nomes históricos (Knorr/Henschel/Jaracz, todos mal-transcritos
   pelo áudio e corrigidos pela legenda oficial) e um trecho de Salmo 34:17,18
   mal-transcrito, corrigido contra a TNM antes de publicar.
3. **Fase 4 — auditor obrigatório**: `AUDITORIA_JWPREP.md` foi gerado por um
   script (`web/auditor.py`, não incluído aqui, código do projeto) que reabre
   a fonte — ao vivo ou via o MD bruto scrapeado — pra cada semana e cada item,
   nunca aceita o nosso output como verdade sozinho. Validado antes de rodar
   contra um fixture NEGATIVO conhecido (o estado quebrado original da Parte 8,
   cobrindo só 4 de 13 perguntas oficiais) — o auditor REPROVA esse fixture,
   prova de que a checagem tem dente de verdade.
4. **Achado real, documentado e NÃO corrigido nesta rodada**: 6 das 12 semanas
   da Sentinela têm uma caixa lateral no artigo (bio, contexto histórico,
   "Você Sabia?") sem campo de resposta do leitor, que a fonte tem e o nosso
   output hoje não gera — ver a seção "Caixa lateral sem campo de resposta" em
   `AUDITORIA_JWPREP.md`. É gap de cobertura, não defeito (nada errado
   publicado), decisão de criar tag+conteúdo fica pro revisor.

Verificado antes desta publicação: `pytest tests/` = 355 passed, 19 skipped
(skips legítimos, nenhum mascara falha de fonte); `python3 web/build.py` = OK,
12 Sentinela + 11 VM; zero travessão (`—`/`–`) em todo o `output/`.

Sem deploy. Repo de revisão temporário e isolado — não é o mesmo que `rev-k9t3`
(mantido intocado, aprovação pendente à parte).
