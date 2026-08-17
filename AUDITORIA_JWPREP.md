# AUDITORIA_JWPREP — relatório automático (Fase 4)

Gerado por `web/auditor.py`. Cada linha vem de uma checagem que abriu/contou a fonte AO VIVO ou reconferiu contra o MD bruto scrapeado (nunca aceita o nosso output como verdade sozinho). REPROVADO = falha de acesso à fonte, tratado como reprovação, nunca como aprovação por omissão.


**Resumo:** 12 semanas auditadas, 6 item(ns) não batendo ou reprovado(s) no total.


## Semana 2026-06-15

| Item | Bate? | Prova |
|---|---|---|
| Sentinela: nº de perguntas numeradas bate com a fonte | SIM | contagem pareada fonte x nosso, PESSOAL |
| Sentinela: nº de perguntas numeradas bate com a fonte | SIM | contagem pareada fonte x nosso, PUBLICA |
| Sentinela: nº de imagens bate com a fonte | SIM | contagem pareada fonte x nosso, PESSOAL |
| Sentinela: caixa de recap existe na fonte | SIM | estrutural (boxContent com gen-field/textarea) |
| Sentinela: caixa de recap verbatim no nosso output | SIM | citação pareada, PESSOAL |
| Sentinela: caixa de recap verbatim no nosso output | SIM | citação pareada, PUBLICA |
| Numeração combinada de parágrafo | N/A | fonte de 2026-06-15 não tem exemplo — checado no MD bruto, 0 ocorrências |
| Caixa lateral sem campo de resposta | N/A | fonte de 2026-06-15 não tem esse tipo de caixa — checado ao vivo em https://www.jw.org/pt/biblioteca/revistas/sentinela-estudo-abril-2026/Tenha-o-ponto-de-vista-de-Jeov%C3%A1-sobre-as-dificuldades/ |
| VM: título de cada parte bate com a fonte | N/A | skip legítimo do teste de origem: 2026-06-15: 2026-06-15_VM_PESSOAL.md ainda não existe |
| VM: título de cada parte bate com a fonte | N/A | skip legítimo do teste de origem: 2026-06-15: 2026-06-15_VM_PUBLICA.md ainda não existe |
| VM: tempo de cada parte bate com a fonte | N/A | skip legítimo do teste de origem: 2026-06-15: arquivo ainda não existe |
| VM: imagens usam URL real do CDN (não inventada) | N/A | skip legítimo do teste de origem: 2026-06-15: arquivo ainda não existe |
| VM: vídeo tem transcrição real OU nota honesta explícita | N/A | skip legítimo do teste de origem: 2026-06-15: 2026-06-15_VM_PESSOAL.md ainda não existe |
| Zero travessão/en-dash (—/–) em todo output | SIM | grep -n '—\\|–' = 0 hits nos arquivos desta semana |

**O QUE A FONTE TEM E A NOSSA SAÍDA NÃO TEM:** vazia — nada pendente nesta semana.

## Semana 2026-06-22

| Item | Bate? | Prova |
|---|---|---|
| Sentinela: nº de perguntas numeradas bate com a fonte | SIM | contagem pareada fonte x nosso, PESSOAL |
| Sentinela: nº de perguntas numeradas bate com a fonte | SIM | contagem pareada fonte x nosso, PUBLICA |
| Sentinela: nº de imagens bate com a fonte | SIM | contagem pareada fonte x nosso, PESSOAL |
| Sentinela: caixa de recap existe na fonte | SIM | estrutural (boxContent com gen-field/textarea) |
| Sentinela: caixa de recap verbatim no nosso output | SIM | citação pareada, PESSOAL |
| Sentinela: caixa de recap verbatim no nosso output | SIM | citação pareada, PUBLICA |
| Numeração combinada de parágrafo | N/A | fonte de 2026-06-22 não tem exemplo — checado no MD bruto, 0 ocorrências |
| Caixa lateral sem campo de resposta | N/A | fonte de 2026-06-22 não tem esse tipo de caixa — checado ao vivo em https://www.jw.org/pt/biblioteca/revistas/sentinela-estudo-abril-2026/Aprenda-com-o-Deus-de-todo-o-consolo/ |
| VM: título de cada parte bate com a fonte | SIM | citação pareada por parte, PESSOAL |
| VM: título de cada parte bate com a fonte | SIM | citação pareada por parte, PUBLICA |
| VM: tempo de cada parte bate com a fonte | SIM | citação pareada por parte |
| VM: imagens usam URL real do CDN (não inventada) | SIM | regex de URL cms-imgp.jw-cdn.org |
| VM: vídeo tem transcrição real OU nota honesta explícita | N/A | skip legítimo do teste de origem: 2026-06-22: transcrição real encontrada, nota honesta não é obrigatória |
| Zero travessão/en-dash (—/–) em todo output | SIM | grep -n '—\\|–' = 0 hits nos arquivos desta semana |

**O QUE A FONTE TEM E A NOSSA SAÍDA NÃO TEM:** vazia — nada pendente nesta semana.

## Semana 2026-06-29

| Item | Bate? | Prova |
|---|---|---|
| Sentinela: nº de perguntas numeradas bate com a fonte | SIM | contagem pareada fonte x nosso, PESSOAL |
| Sentinela: nº de perguntas numeradas bate com a fonte | SIM | contagem pareada fonte x nosso, PUBLICA |
| Sentinela: nº de imagens bate com a fonte | SIM | contagem pareada fonte x nosso, PESSOAL |
| Sentinela: caixa de recap existe na fonte | SIM | estrutural (boxContent com gen-field/textarea) |
| Sentinela: caixa de recap verbatim no nosso output | SIM | citação pareada, PESSOAL |
| Sentinela: caixa de recap verbatim no nosso output | SIM | citação pareada, PUBLICA |
| Numeração combinada de parágrafo | N/A | fonte de 2026-06-29 não tem exemplo — checado no MD bruto, 0 ocorrências |
| Caixa lateral sem campo de resposta | NÃO | fonte TEM 2 caixa(s) sem campo de resposta em https://www.jw.org/pt/biblioteca/revistas/sentinela-estudo-abril-2026/Marido-e-esposa-continuem-fortalecendo-a-amizade-entre-voc%C3%AAs/, sem tag dedicada no nosso output hoje (gap real, registrado no INVENTARIO) |
| VM: título de cada parte bate com a fonte | SIM | citação pareada por parte, PESSOAL |
| VM: título de cada parte bate com a fonte | SIM | citação pareada por parte, PUBLICA |
| VM: tempo de cada parte bate com a fonte | SIM | citação pareada por parte |
| VM: imagens usam URL real do CDN (não inventada) | SIM | regex de URL cms-imgp.jw-cdn.org |
| VM: vídeo tem transcrição real OU nota honesta explícita | N/A | skip legítimo do teste de origem: 2026-06-29: sem vídeo no programa desta semana |
| Zero travessão/en-dash (—/–) em todo output | SIM | grep -n '—\\|–' = 0 hits nos arquivos desta semana |

**O QUE A FONTE TEM E A NOSSA SAÍDA NÃO TEM:** Caixa lateral sem campo de resposta

## Semana 2026-07-06

| Item | Bate? | Prova |
|---|---|---|
| Sentinela: nº de perguntas numeradas bate com a fonte | SIM | contagem pareada fonte x nosso, PESSOAL |
| Sentinela: nº de perguntas numeradas bate com a fonte | SIM | contagem pareada fonte x nosso, PUBLICA |
| Sentinela: nº de imagens bate com a fonte | SIM | contagem pareada fonte x nosso, PESSOAL |
| Sentinela: caixa de recap existe na fonte | SIM | estrutural (boxContent com gen-field/textarea) |
| Sentinela: caixa de recap verbatim no nosso output | SIM | citação pareada, PESSOAL |
| Sentinela: caixa de recap verbatim no nosso output | SIM | citação pareada, PUBLICA |
| Numeração combinada de parágrafo | N/A | fonte de 2026-07-06 não tem exemplo — checado no MD bruto, 0 ocorrências |
| Caixa lateral sem campo de resposta | N/A | fonte de 2026-07-06 não tem esse tipo de caixa — checado ao vivo em https://www.jw.org/pt/biblioteca/revistas/sentinela-estudo-maio-2026/Por-que-os-princ%C3%ADpios-b%C3%ADblicos-s%C3%A3o-importantes/ |
| VM: título de cada parte bate com a fonte | SIM | citação pareada por parte, PESSOAL |
| VM: título de cada parte bate com a fonte | SIM | citação pareada por parte, PUBLICA |
| VM: tempo de cada parte bate com a fonte | SIM | citação pareada por parte |
| VM: imagens usam URL real do CDN (não inventada) | SIM | regex de URL cms-imgp.jw-cdn.org |
| VM: vídeo tem transcrição real OU nota honesta explícita | N/A | skip legítimo do teste de origem: 2026-07-06: transcrição real encontrada, nota honesta não é obrigatória |
| Zero travessão/en-dash (—/–) em todo output | SIM | grep -n '—\\|–' = 0 hits nos arquivos desta semana |

**O QUE A FONTE TEM E A NOSSA SAÍDA NÃO TEM:** vazia — nada pendente nesta semana.

## Semana 2026-07-13

| Item | Bate? | Prova |
|---|---|---|
| Sentinela: nº de perguntas numeradas bate com a fonte | SIM | contagem pareada fonte x nosso, PESSOAL |
| Sentinela: nº de perguntas numeradas bate com a fonte | SIM | contagem pareada fonte x nosso, PUBLICA |
| Sentinela: nº de imagens bate com a fonte | SIM | contagem pareada fonte x nosso, PESSOAL |
| Sentinela: caixa de recap existe na fonte | SIM | estrutural (boxContent com gen-field/textarea) |
| Sentinela: caixa de recap verbatim no nosso output | SIM | citação pareada, PESSOAL |
| Sentinela: caixa de recap verbatim no nosso output | SIM | citação pareada, PUBLICA |
| Numeração combinada de parágrafo | N/A | fonte de 2026-07-13 não tem exemplo — checado no MD bruto, 0 ocorrências |
| Caixa lateral sem campo de resposta | NÃO | fonte TEM 1 caixa(s) sem campo de resposta em https://www.jw.org/pt/biblioteca/revistas/sentinela-estudo-maio-2026/Como-usar-os-princ%C3%ADpios-b%C3%ADblicos-para-treinar-nossa-consci%C3%AAncia/, sem tag dedicada no nosso output hoje (gap real, registrado no INVENTARIO) |
| VM: título de cada parte bate com a fonte | SIM | citação pareada por parte, PESSOAL |
| VM: título de cada parte bate com a fonte | SIM | citação pareada por parte, PUBLICA |
| VM: tempo de cada parte bate com a fonte | SIM | citação pareada por parte |
| VM: imagens usam URL real do CDN (não inventada) | SIM | regex de URL cms-imgp.jw-cdn.org |
| VM: vídeo tem transcrição real OU nota honesta explícita | N/A | skip legítimo do teste de origem: 2026-07-13: transcrição real encontrada, nota honesta não é obrigatória |
| Zero travessão/en-dash (—/–) em todo output | SIM | grep -n '—\\|–' = 0 hits nos arquivos desta semana |

**O QUE A FONTE TEM E A NOSSA SAÍDA NÃO TEM:** Caixa lateral sem campo de resposta

## Semana 2026-07-20

| Item | Bate? | Prova |
|---|---|---|
| Sentinela: nº de perguntas numeradas bate com a fonte | SIM | contagem pareada fonte x nosso, PESSOAL |
| Sentinela: nº de perguntas numeradas bate com a fonte | SIM | contagem pareada fonte x nosso, PUBLICA |
| Sentinela: nº de imagens bate com a fonte | SIM | contagem pareada fonte x nosso, PESSOAL |
| Sentinela: caixa de recap existe na fonte | SIM | estrutural (boxContent com gen-field/textarea) |
| Sentinela: caixa de recap verbatim no nosso output | SIM | citação pareada, PESSOAL |
| Sentinela: caixa de recap verbatim no nosso output | SIM | citação pareada, PUBLICA |
| Numeração combinada de parágrafo | N/A | fonte de 2026-07-20 não tem exemplo — checado no MD bruto, 0 ocorrências |
| Caixa lateral sem campo de resposta | NÃO | fonte TEM 1 caixa(s) sem campo de resposta em https://www.jw.org/pt/biblioteca/revistas/sentinela-estudo-maio-2026/Seja-s%C3%A1bio-em-suas-decis%C3%B5es-sobre-ensino-adicional/, sem tag dedicada no nosso output hoje (gap real, registrado no INVENTARIO) |
| VM: título de cada parte bate com a fonte | SIM | citação pareada por parte, PESSOAL |
| VM: título de cada parte bate com a fonte | SIM | citação pareada por parte, PUBLICA |
| VM: tempo de cada parte bate com a fonte | SIM | citação pareada por parte |
| VM: imagens usam URL real do CDN (não inventada) | SIM | regex de URL cms-imgp.jw-cdn.org |
| VM: vídeo tem transcrição real OU nota honesta explícita | N/A | skip legítimo do teste de origem: 2026-07-20: transcrição real encontrada, nota honesta não é obrigatória |
| Zero travessão/en-dash (—/–) em todo output | SIM | grep -n '—\\|–' = 0 hits nos arquivos desta semana |

**O QUE A FONTE TEM E A NOSSA SAÍDA NÃO TEM:** Caixa lateral sem campo de resposta

## Semana 2026-07-27

| Item | Bate? | Prova |
|---|---|---|
| Sentinela: nº de perguntas numeradas bate com a fonte | SIM | contagem pareada fonte x nosso, PESSOAL |
| Sentinela: nº de perguntas numeradas bate com a fonte | SIM | contagem pareada fonte x nosso, PUBLICA |
| Sentinela: nº de imagens bate com a fonte | SIM | contagem pareada fonte x nosso, PESSOAL |
| Sentinela: caixa de recap existe na fonte | SIM | estrutural (boxContent com gen-field/textarea) |
| Sentinela: caixa de recap verbatim no nosso output | SIM | citação pareada, PESSOAL |
| Sentinela: caixa de recap verbatim no nosso output | SIM | citação pareada, PUBLICA |
| Numeração combinada de parágrafo | N/A | fonte de 2026-07-27 não tem exemplo — checado no MD bruto, 0 ocorrências |
| Caixa lateral sem campo de resposta | N/A | fonte de 2026-07-27 não tem esse tipo de caixa — checado ao vivo em https://www.jw.org/pt/biblioteca/revistas/sentinela-estudo-maio-2026/Continue-espiritualmente-forte-ao-obter-ensino-adicional/ |
| VM: título de cada parte bate com a fonte | SIM | citação pareada por parte, PESSOAL |
| VM: título de cada parte bate com a fonte | SIM | citação pareada por parte, PUBLICA |
| VM: tempo de cada parte bate com a fonte | SIM | citação pareada por parte |
| VM: imagens usam URL real do CDN (não inventada) | SIM | regex de URL cms-imgp.jw-cdn.org |
| VM: vídeo tem transcrição real OU nota honesta explícita | N/A | skip legítimo do teste de origem: 2026-07-27: transcrição real encontrada, nota honesta não é obrigatória |
| Parte 8 (livro wcg): os 4 blocos oficiais existem na fonte | SIM | contagem ao vivo via estudo_capitulo_completo() |
| Parte 8: todo item oficial dos 4 blocos aparece verbatim | SIM | citação pareada por item, PESSOAL |
| Parte 8: todo item oficial dos 4 blocos aparece verbatim | SIM | citação pareada por item, PUBLICA |
| Parte 8: 'Aprenda mais' com tag própria e duração do vídeo | SIM | regex de duração (M:SS) + campo hist['aprenda_mais'] |
| Zero travessão/en-dash (—/–) em todo output | SIM | grep -n '—\\|–' = 0 hits nos arquivos desta semana |

**O QUE A FONTE TEM E A NOSSA SAÍDA NÃO TEM:** vazia — nada pendente nesta semana.

## Semana 2026-08-03

| Item | Bate? | Prova |
|---|---|---|
| Sentinela: nº de perguntas numeradas bate com a fonte | SIM | contagem pareada fonte x nosso, PESSOAL |
| Sentinela: nº de perguntas numeradas bate com a fonte | SIM | contagem pareada fonte x nosso, PUBLICA |
| Sentinela: nº de imagens bate com a fonte | SIM | contagem pareada fonte x nosso, PESSOAL |
| Sentinela: caixa de recap existe na fonte | SIM | estrutural (boxContent com gen-field/textarea) |
| Sentinela: caixa de recap verbatim no nosso output | SIM | citação pareada, PESSOAL |
| Sentinela: caixa de recap verbatim no nosso output | SIM | citação pareada, PUBLICA |
| Numeração combinada de parágrafo | N/A | fonte de 2026-08-03 não tem exemplo — checado no MD bruto, 0 ocorrências |
| Caixa lateral sem campo de resposta | NÃO | fonte TEM 1 caixa(s) sem campo de resposta em https://www.jw.org/pt/biblioteca/revistas/sentinela-estudo-maio-2026/Respeite-as-decis%C3%B5es-de-outros/, sem tag dedicada no nosso output hoje (gap real, registrado no INVENTARIO) |
| VM: título de cada parte bate com a fonte | SIM | citação pareada por parte, PESSOAL |
| VM: título de cada parte bate com a fonte | SIM | citação pareada por parte, PUBLICA |
| VM: tempo de cada parte bate com a fonte | SIM | citação pareada por parte |
| VM: imagens usam URL real do CDN (não inventada) | SIM | regex de URL cms-imgp.jw-cdn.org |
| VM: vídeo tem transcrição real OU nota honesta explícita | N/A | skip legítimo do teste de origem: 2026-08-03: transcrição real encontrada, nota honesta não é obrigatória |
| Parte 8 (livro wcg): os 4 blocos oficiais existem na fonte | SIM | contagem ao vivo via estudo_capitulo_completo() |
| Parte 8: todo item oficial dos 4 blocos aparece verbatim | SIM | citação pareada por item, PESSOAL |
| Parte 8: todo item oficial dos 4 blocos aparece verbatim | SIM | citação pareada por item, PUBLICA |
| Parte 8: 'Aprenda mais' com tag própria e duração do vídeo | SIM | regex de duração (M:SS) + campo hist['aprenda_mais'] |
| Zero travessão/en-dash (—/–) em todo output | SIM | grep -n '—\\|–' = 0 hits nos arquivos desta semana |

**O QUE A FONTE TEM E A NOSSA SAÍDA NÃO TEM:** Caixa lateral sem campo de resposta

## Semana 2026-08-10

| Item | Bate? | Prova |
|---|---|---|
| Sentinela: nº de perguntas numeradas bate com a fonte | SIM | contagem pareada fonte x nosso, PESSOAL |
| Sentinela: nº de perguntas numeradas bate com a fonte | SIM | contagem pareada fonte x nosso, PUBLICA |
| Sentinela: nº de imagens bate com a fonte | SIM | contagem pareada fonte x nosso, PESSOAL |
| Sentinela: caixa de recap existe na fonte | SIM | estrutural (boxContent com gen-field/textarea) |
| Sentinela: caixa de recap verbatim no nosso output | SIM | citação pareada, PESSOAL |
| Sentinela: caixa de recap verbatim no nosso output | SIM | citação pareada, PUBLICA |
| Numeração combinada de parágrafo | N/A | fonte de 2026-08-10 não tem exemplo — checado no MD bruto, 0 ocorrências |
| Caixa lateral sem campo de resposta | N/A | fonte de 2026-08-10 não tem esse tipo de caixa — checado ao vivo em https://www.jw.org/pt/biblioteca/revistas/sentinela-estudo-junho-2026/Como-continuar-leal-a-Jeov%C3%A1-ao-enfrentar-testes-de-f%C3%A9/ |
| VM: título de cada parte bate com a fonte | SIM | citação pareada por parte, PESSOAL |
| VM: título de cada parte bate com a fonte | SIM | citação pareada por parte, PUBLICA |
| VM: tempo de cada parte bate com a fonte | SIM | citação pareada por parte |
| VM: imagens usam URL real do CDN (não inventada) | SIM | regex de URL cms-imgp.jw-cdn.org |
| VM: vídeo tem transcrição real OU nota honesta explícita | N/A | skip legítimo do teste de origem: 2026-08-10: sem vídeo no programa desta semana |
| Parte 8 (livro wcg): os 4 blocos oficiais existem na fonte | SIM | contagem ao vivo via estudo_capitulo_completo() |
| Parte 8: todo item oficial dos 4 blocos aparece verbatim | SIM | citação pareada por item, PESSOAL |
| Parte 8: todo item oficial dos 4 blocos aparece verbatim | SIM | citação pareada por item, PUBLICA |
| Parte 8: 'Aprenda mais' com tag própria e duração do vídeo | SIM | regex de duração (M:SS) + campo hist['aprenda_mais'] |
| Zero travessão/en-dash (—/–) em todo output | SIM | grep -n '—\\|–' = 0 hits nos arquivos desta semana |

**O QUE A FONTE TEM E A NOSSA SAÍDA NÃO TEM:** vazia — nada pendente nesta semana.

## Semana 2026-08-17

| Item | Bate? | Prova |
|---|---|---|
| Sentinela: nº de perguntas numeradas bate com a fonte | SIM | contagem pareada fonte x nosso, PESSOAL |
| Sentinela: nº de perguntas numeradas bate com a fonte | SIM | contagem pareada fonte x nosso, PUBLICA |
| Sentinela: nº de imagens bate com a fonte | SIM | contagem pareada fonte x nosso, PESSOAL |
| Sentinela: caixa de recap existe na fonte | SIM | estrutural (boxContent com gen-field/textarea) |
| Sentinela: caixa de recap verbatim no nosso output | SIM | citação pareada, PESSOAL |
| Sentinela: caixa de recap verbatim no nosso output | SIM | citação pareada, PUBLICA |
| Numeração combinada de parágrafo | N/A | fonte de 2026-08-17 não tem exemplo — checado no MD bruto, 0 ocorrências |
| Caixa lateral sem campo de resposta | N/A | fonte de 2026-08-17 não tem esse tipo de caixa — checado ao vivo em https://www.jw.org/pt/biblioteca/revistas/sentinela-estudo-junho-2026/Como-manter-a-amizade-com-nossos-irm%C3%A3os/ |
| VM: título de cada parte bate com a fonte | SIM | citação pareada por parte, PESSOAL |
| VM: título de cada parte bate com a fonte | SIM | citação pareada por parte, PUBLICA |
| VM: tempo de cada parte bate com a fonte | SIM | citação pareada por parte |
| VM: imagens usam URL real do CDN (não inventada) | SIM | regex de URL cms-imgp.jw-cdn.org |
| VM: vídeo tem transcrição real OU nota honesta explícita | N/A | skip legítimo do teste de origem: 2026-08-17: sem vídeo no programa desta semana |
| Parte 8 (livro wcg): os 4 blocos oficiais existem na fonte | SIM | contagem ao vivo via estudo_capitulo_completo() |
| Parte 8: todo item oficial dos 4 blocos aparece verbatim | SIM | citação pareada por item, PESSOAL |
| Parte 8: todo item oficial dos 4 blocos aparece verbatim | SIM | citação pareada por item, PUBLICA |
| Parte 8: 'Aprenda mais' com tag própria e duração do vídeo | SIM | regex de duração (M:SS) + campo hist['aprenda_mais'] |
| Zero travessão/en-dash (—/–) em todo output | SIM | grep -n '—\\|–' = 0 hits nos arquivos desta semana |

**O QUE A FONTE TEM E A NOSSA SAÍDA NÃO TEM:** vazia — nada pendente nesta semana.

## Semana 2026-08-24

| Item | Bate? | Prova |
|---|---|---|
| Sentinela: nº de perguntas numeradas bate com a fonte | SIM | contagem pareada fonte x nosso, PESSOAL |
| Sentinela: nº de perguntas numeradas bate com a fonte | SIM | contagem pareada fonte x nosso, PUBLICA |
| Sentinela: nº de imagens bate com a fonte | SIM | contagem pareada fonte x nosso, PESSOAL |
| Sentinela: caixa de recap existe na fonte | SIM | estrutural (boxContent com gen-field/textarea) |
| Sentinela: caixa de recap verbatim no nosso output | SIM | citação pareada, PESSOAL |
| Sentinela: caixa de recap verbatim no nosso output | SIM | citação pareada, PUBLICA |
| Numeração combinada de parágrafo | N/A | fonte de 2026-08-24 não tem exemplo — checado no MD bruto, 0 ocorrências |
| Caixa lateral sem campo de resposta | NÃO | fonte TEM 1 caixa(s) sem campo de resposta em https://www.jw.org/pt/biblioteca/revistas/sentinela-estudo-junho-2026/Nosso-grupo-de-servi%C3%A7o-de-campo-%C3%A9-um-presente-para-n%C3%B3s/, sem tag dedicada no nosso output hoje (gap real, registrado no INVENTARIO) |
| VM: título de cada parte bate com a fonte | SIM | citação pareada por parte, PESSOAL |
| VM: título de cada parte bate com a fonte | SIM | citação pareada por parte, PUBLICA |
| VM: tempo de cada parte bate com a fonte | SIM | citação pareada por parte |
| VM: imagens usam URL real do CDN (não inventada) | SIM | regex de URL cms-imgp.jw-cdn.org |
| VM: vídeo tem transcrição real OU nota honesta explícita | N/A | skip legítimo do teste de origem: 2026-08-24: transcrição real encontrada, nota honesta não é obrigatória |
| Parte 8 (livro wcg): os 4 blocos oficiais existem na fonte | SIM | contagem ao vivo via estudo_capitulo_completo() |
| Parte 8: todo item oficial dos 4 blocos aparece verbatim | SIM | citação pareada por item, PESSOAL |
| Parte 8: todo item oficial dos 4 blocos aparece verbatim | SIM | citação pareada por item, PUBLICA |
| Parte 8: 'Aprenda mais' com tag própria e duração do vídeo | SIM | regex de duração (M:SS) + campo hist['aprenda_mais'] |
| Zero travessão/en-dash (—/–) em todo output | SIM | grep -n '—\\|–' = 0 hits nos arquivos desta semana |

**O QUE A FONTE TEM E A NOSSA SAÍDA NÃO TEM:** Caixa lateral sem campo de resposta

## Semana 2026-08-31

| Item | Bate? | Prova |
|---|---|---|
| Sentinela: nº de perguntas numeradas bate com a fonte | SIM | contagem pareada fonte x nosso, PESSOAL |
| Sentinela: nº de perguntas numeradas bate com a fonte | SIM | contagem pareada fonte x nosso, PUBLICA |
| Sentinela: nº de imagens bate com a fonte | SIM | contagem pareada fonte x nosso, PESSOAL |
| Sentinela: caixa de recap existe na fonte | SIM | estrutural (boxContent com gen-field/textarea) |
| Sentinela: caixa de recap verbatim no nosso output | SIM | citação pareada, PESSOAL |
| Sentinela: caixa de recap verbatim no nosso output | SIM | citação pareada, PUBLICA |
| Numeração combinada de parágrafo | N/A | fonte de 2026-08-31 não tem exemplo — checado no MD bruto, 0 ocorrências |
| Caixa lateral sem campo de resposta | NÃO | fonte TEM 1 caixa(s) sem campo de resposta em https://www.jw.org/pt/biblioteca/revistas/sentinela-estudo-junho-2026/Como-voc%C3%AA-pode-escutar-com-mais-aten%C3%A7%C3%A3o/, sem tag dedicada no nosso output hoje (gap real, registrado no INVENTARIO) |
| VM: título de cada parte bate com a fonte | SIM | citação pareada por parte, PESSOAL |
| VM: título de cada parte bate com a fonte | SIM | citação pareada por parte, PUBLICA |
| VM: tempo de cada parte bate com a fonte | SIM | citação pareada por parte |
| VM: imagens usam URL real do CDN (não inventada) | SIM | regex de URL cms-imgp.jw-cdn.org |
| VM: vídeo tem transcrição real OU nota honesta explícita | N/A | skip legítimo do teste de origem: 2026-08-31: transcrição real encontrada, nota honesta não é obrigatória |
| Parte 8 (livro wcg): os 4 blocos oficiais existem na fonte | SIM | contagem ao vivo via estudo_capitulo_completo() |
| Parte 8: todo item oficial dos 4 blocos aparece verbatim | SIM | citação pareada por item, PESSOAL |
| Parte 8: todo item oficial dos 4 blocos aparece verbatim | SIM | citação pareada por item, PUBLICA |
| Parte 8: 'Aprenda mais' com tag própria e duração do vídeo | SIM | regex de duração (M:SS) + campo hist['aprenda_mais'] |
| Zero travessão/en-dash (—/–) em todo output | SIM | grep -n '—\\|–' = 0 hits nos arquivos desta semana |

**O QUE A FONTE TEM E A NOSSA SAÍDA NÃO TEM:** Caixa lateral sem campo de resposta