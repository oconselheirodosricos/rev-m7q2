# MOLDE DE SUCESSÃO DE CHAT

Estrutura genérica para passar um chat adiante quando ele estoura contexto, perde
uma ferramenta essencial ou precisa ser substituído. Vale para qualquer frente da
operação.

---

## O PRINCÍPIO

**Dossiê não é histórico.**

Conversa crua tem tela de terminal repetida, erro já corrigido, defeito que deixou
de existir e decisão que foi revertida depois. Sucessor lendo isso trata coisa
resolvida como pendente e passa a caçar fantasma.

O dossiê é destilado: só o que muda decisão futura.

**Teste para incluir ou cortar:** isto muda alguma decisão do sucessor? Se não
muda, fica de fora, por mais interessante que seja a história.

---

## AS OITO SEÇÕES

**1. O que é o projeto.** O que a coisa faz, para quem, o ritmo de trabalho, e o
objetivo declarado de quem manda. Sem jargão interno não explicado.

**2. Hierarquia e fluxo.** Quem constrói, quem audita, quem decide, quem publica.
Como a ordem viaja entre eles. Se os papéis são chats que não se falam, dizer isso
com todas as letras e nomear quem é a ponte. O sucessor precisa saber que peso tem
a palavra dele: se o que ele afirma vira ordem de trabalho para outro, isso muda
como ele deve afirmar.

**3. As regras que não se negociam.** Curtas e acionáveis, agrupadas por tema, não
o documento de spec copiado. Cada regra em uma ou duas linhas. O texto longo fica
onde já estava, com o ponteiro.

**4. Defeitos históricos, com causa raiz.** A seção mais valiosa. Para cada defeito
que já aconteceu, quatro linhas: o que falhou, a causa raiz nomeada, como foi
corrigido, e qual trava impede a volta. Agrupar por natureza (conteúdo,
ferramenta, infraestrutura, processo) quando forem muitos. Defeito corrigido mora
aqui, como lição, nunca na seção 6 como pendência.

**5. Decisões e o porquê.** Cada decisão que vale para sempre, com o critério que a
gerou. Decisão sem o porquê será revertida pelo sucessor na primeira dúvida.
Incluir as que parecem arbitrárias: são justamente as que voltam.

**6. Estado real de hoje.** Em quatro blocos: o que está no ar e funcionando; o que
está pronto e não publicado; o que está aberto, cada item com **dono** e
**critério de pronto**; e o que está catalogado sem correção **por escolha**, com a
escolha atribuída a quem a fez. Distinguir "aberto por descuido" de "aberto por
decisão" é metade do valor desta seção.

**7. O que já foi tentado e não funcionou.** Caminhos descartados, com o motivo em
uma linha. Evita que o sucessor refaça o que já se provou ruim e gaste uma rodada
para redescobrir.

**8. Como o operador humano trabalha.** Ritmo, formato de resposta esperado, o que
ele odeia, como ele corrige, o que é inegociável, e em que casos vale interromper.
Isto não é bajulação: é o que evita queimar a atenção dele, que costuma ser o
recurso mais caro da operação.

---

## O TESTE DE ADMISSÃO

Vai no fim do dossiê. Serve para o operador avaliar o sucessor antes de confiar
nele.

**Regra da pergunta boa:** testa julgamento, não memória. Pergunta que se responde
relendo o dossiê não serve. A pergunta boa revela o que a pessoa faz quando a regra
não cobre o caso.

Construa uma pergunta para cada eixo, adaptando ao domínio:

- o que ele faz quando **não consegue acessar a fonte** e precisa dar um veredito
- se ele entende que **sinal verde de máquina não prova conteúdo correto**
- se ele sabe distinguir **as duas atividades que se confundem** no papel dele
- o que ele faz quando **outro agente afirma algo que ele não conferiu**
- se ele reconhece **qual é a classe de erro que passa** nesta operação (quase
  sempre ausência, não conteúdo ruim)
- o que ele faz quando **o operador pede algo que contraria uma regra**
- o que ele faz com um **achado fora do escopo** encontrado no caminho
- a pergunta cuja resposta errada **reprova sozinha**, se a operação tiver uma
  proibição absoluta

Para cada pergunta, escreva **o que caracteriza resposta boa** e **o que
caracteriza resposta ruim**, para o operador comparar sem julgar no escuro.

**Sem gabarito decorável:** a resposta certa é a que demonstra o critério aplicado
ao caso concreto, não a que repete uma frase do dossiê.

---

## REGRAS DE ESCRITA

- Nada de tela de terminal colada.
- Nada de defeito já corrigido apresentado como pendente.
- Toda pendência traz **dono** e **critério de pronto**.
- Coisa incerta é marcada como incerta, em vez de afirmada.
- Sem travessão e sem reticências tipográficas, se essa for regra da operação.
- Nada de senha, credencial, endereço de servidor ou caminho absoluto de máquina,
  principalmente se o dossiê vai ser publicado. Aponte onde o dado sensível mora.
- Tamanho: o que couber sem repetir. Corte redundância, não conteúdo.

---

## ANTES DE ENTREGAR

Leia o dossiê como se fosse o sucessor, sem nenhum outro contexto, e responda três
coisas:

1. **O que eu faço primeiro?**
2. **O que eu não posso fazer nunca?**
3. **O que está esperando por mim agora?**

Se alguma das três não tiver resposta clara no texto, o dossiê está incompleto.
Corrija antes de entregar. Se as três só se respondem juntando pedaços de seções
diferentes, ponha um bloco curto no topo respondendo as três de uma vez.
