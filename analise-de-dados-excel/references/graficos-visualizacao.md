# Gráficos e Visualização de Dados

Um gráfico existe para responder uma pergunta mais rápido do que a tabela responderia.
Se a tabela responde melhor, use a tabela.

## Sumário
1. [Escolha do gráfico pela pergunta](#escolha-do-gráfico-pela-pergunta)
2. [Regras por tipo de gráfico](#regras-por-tipo-de-gráfico)
3. [O que evitar (e por quê)](#o-que-evitar-e-por-quê)
4. [Anatomia de um gráfico honesto](#anatomia-de-um-gráfico-honesto)
5. [Storytelling com dados](#storytelling-com-dados)

---

## Escolha do gráfico pela pergunta

A pergunta define o gráfico — nunca o contrário:

| Pergunta | Gráfico |
|---|---|
| Como evoluiu no tempo? | **Linha** (área só para volume acumulado/empilhado) |
| Quem é maior/menor? (comparar categorias) | **Barras horizontais**, ordenadas da maior para a menor |
| Quanto cada parte representa do todo? | **Barras empilhadas 100%** ou barras simples com %; pizza só com ≤4 fatias e diferenças óbvias |
| Como se distribui? (faixas, concentração) | **Histograma** (dinâmica com agrupamento numérico) ou boxplot |
| Duas variáveis se relacionam? | **Dispersão (XY)**; bolha para uma 3ª variável de tamanho |
| Onde estamos vs. meta? | **Cartão de KPI** com variação, ou barra com linha de meta |
| Conversão por etapa? | **Funil** ou barras decrescentes |
| Muitas séries no tempo? | **Linhas pequenas múltiplas** (*small multiples*) — um mini-gráfico por série, mesma escala — em vez de um espaguete de 10 linhas |
| O que compõe a variação entre dois números? | **Cascata (waterfall)** |
| Padrão em duas dimensões (dia × hora, região × mês)? | **Mapa de calor** (tabela + formatação condicional por escala de cor) |

Regras rápidas de decisão:
- Rótulos de categoria longos → barras **horizontais** (o texto cabe).
- Tempo no eixo X, sempre da esquerda para a direita.
- Mais de ~6 categorias → agrupe as menores em "Outros" (e diga o critério).

## Regras por tipo de gráfico

**Linha**
- Eixo Y pode começar acima de zero (linha mostra variação), mas sinalize quando cortar.
- Máx. 4–5 linhas; destaque 1–2 com cor e apague as demais em cinza (contexto).
- Marque eventos relevantes (mudança de preço, campanha) com anotação — transforma o gráfico em explicação.

**Barras/colunas**
- **Eixo de valor começa em zero, sempre.** Barra codifica o valor no comprimento; cortar o eixo
  mente visualmente (uma diferença de 5% parece 3×).
- Ordene pelo valor (não alfabeticamente), exceto quando a ordem natural importa (meses, faixas etárias).
- Rótulos de dados diretos nas barras > eixo com linhas de grade.

**Pizza/rosca**
- Use apenas para "parte do todo" com ≤4 fatias e uma fatia claramente dominante.
- Nunca: pizza 3D, duas pizzas para comparar períodos (use barras), fatias quase iguais
  (humanos comparam mal ângulos — barras comparam comprimentos, que lemos com precisão).

**Dispersão**
- Adicione linha de tendência somente se a relação for aproximadamente linear e você citar o R².
- Destaque os pontos que importam (outliers, o cliente em discussão).

**Combinado (colunas + linha, dois eixos)**
- Só quando as duas métricas têm relação de leitura (volume + taxa). Dois eixos Y facilitam
  manipulação visual — deixe claro qual série pertence a qual eixo e considere dois gráficos
  empilhados como alternativa mais honesta.

## O que evitar (e por quê)

| Prática | Problema |
|---|---|
| Efeito 3D em qualquer gráfico | Distorce percepção dos valores; perspectiva mente |
| Eixo Y cortado em barras | Exagera diferenças — mentira visual clássica |
| Duplo eixo sem sinalização | Permite "fabricar" correlação escalando eixos |
| Pizza com 8 fatias | Ilegível; ninguém compara ângulos parecidos |
| Cores do arco-íris por categoria | Nada se destaca; sem hierarquia de leitura |
| Linhas de grade escuras, bordas, sombras | Ruído compete com o dado (*data-ink ratio*) |
| Rótulo em todas as barras E eixo E legenda | Redundância que polui — escolha um |
| Gráfico de área com séries que se cruzam | Séries se escondem; use linhas |
| Legenda longe do gráfico (embaixo, com 6 itens) | Vai-e-volta de olho; prefira rótulo direto na série |
| Porcentagens que não somam 100% na composição | Erro de base — audite antes de publicar |

## Anatomia de um gráfico honesto

- **Título que afirma** o achado, não o tipo: "Sul cai 18% e puxa o resultado do trimestre",
  não "Gráfico de vendas por região". O título é a primeira (às vezes única) coisa lida.
- **Fonte e período** em nota discreta no rodapé.
- **Eixos rotulados** com unidade (R$ mil, %, un.) — sem decimais desnecessários.
- **Comparação embutida**: meta como linha de referência, ano anterior em cinza, média como
  linha pontilhada. Gráfico sem referência obriga o leitor a adivinhar se o número é bom.
- **Uma mensagem por gráfico.** Duas mensagens = dois gráficos.
- Consistência entre gráficos do mesmo relatório: mesma cor = mesma coisa em todas as telas
  (se "Online" é azul num gráfico, é azul em todos).

## Storytelling com dados

Para apresentações (não dashboards — dashboard é exploração, apresentação é narrativa):

1. **Estrutura**: contexto (o que acompanhamos) → conflito (o que mudou/preocupa) → resolução
   (o que fazer). Um slide = um ponto da narrativa = um gráfico.
2. **A conclusão é o título** de cada slide. Se a pessoa só ler os títulos, entende a história.
3. **Destaque guiado**: no gráfico, pinte só a série/barra da história; resto em cinza. A
   audiência olha para onde a cor aponta.
4. **Anime a revelação se necessário** (mostrar o "antes" e depois a linha da queda), mas nunca
   anime por estética.
5. Antecipe a objeção: leve o gráfico da pergunta que vão fazer ("e se for sazonalidade?") no
   apêndice.
