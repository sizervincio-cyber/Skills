# Tabelas Dinâmicas (Pivot Tables)

A ferramenta de maior alavancagem do Excel: resume milhares de linhas em segundos, sem fórmula.
Se a pergunta é "quanto/quantos por alguma coisa", a resposta quase sempre é uma tabela dinâmica.

## Sumário
1. [Criação e anatomia](#criação-e-anatomia)
2. [Técnicas essenciais](#técnicas-essenciais)
3. [Mostrar valores como (a joia escondida)](#mostrar-valores-como)
4. [Agrupamento](#agrupamento)
5. [Campos calculados e limitações](#campos-calculados)
6. [Segmentações e linha do tempo](#segmentações-e-linha-do-tempo)
7. [Boas práticas e armadilhas](#boas-práticas-e-armadilhas)
8. [Além da dinâmica: Modelo de Dados](#modelo-de-dados)

---

## Criação e anatomia

Pré-requisito: base tabular limpa, formatada como Tabela nomeada (ver `limpeza-preparacao.md`).
Criar: clique na tabela → `Inserir → Tabela Dinâmica`.

Quatro áreas:
- **Linhas**: a dimensão que quebra a análise (produto, região, vendedor).
- **Colunas**: segunda dimensão, cruzada (meses, canal). Use com parcimônia — mais de ~12 colunas
  vira leitura difícil.
- **Valores**: a métrica agregada (soma de valor, contagem de pedidos, média de ticket).
- **Filtros**: corte global. Hoje, prefira **segmentações** (mais visíveis e auditáveis).

Leitura mental correta: "**[agregação] de [métrica] por [linhas] e [colunas], filtrado por [filtros]**"
— ex.: "soma de Valor por Região e Mês, filtrado por Canal = Online".

## Técnicas essenciais

- **Campo numérico entra como Contagem em vez de Soma?** A coluna tem texto ou vazio no meio —
  volte e limpe; é um diagnóstico de sujeira, não um capricho do Excel.
- **A mesma métrica pode entrar várias vezes em Valores** com agregações diferentes: Soma de
  Valor + Contagem de Valor + Média de Valor lado a lado.
- **Duplo clique num número** da dinâmica abre uma aba com as linhas que o compõem (*drill-down*).
  Ferramenta de auditoria e de resposta a "de onde saiu esse número?".
- **Layout**: `Design → Layout do Relatório → Mostrar em Formato de Tabela` + `Repetir Todos os
  Rótulos de Item` produz saída tabular reutilizável (bom para copiar/exportar). Subtotais e
  Totais Gerais podem ser desligados no mesmo menu.
- **Classificação**: clique com o botão direito num valor → Classificar. Top N: filtro do rótulo
  de linha → Filtros de Valor → 10 Primeiros.
- **Atualizar**: dinâmica **não** recalcula sozinha — botão direito → `Atualizar` (ou
  `Dados → Atualizar Tudo`). Causa clássica de relatório desatualizado apresentado em reunião.

## Mostrar valores como

Botão direito no valor → `Mostrar Valores como`. Transforma a mesma soma em análise:

| Opção | Responde |
|---|---|
| % do Total Geral | Qual o peso de cada item no todo? |
| % do Total da Coluna / da Linha | Composição dentro de cada mês / de cada região |
| Diferença de... (mês anterior) | Crescemos quanto em valor absoluto? |
| % da Diferença de... | Crescemos quanto em %? (variação MoM/YoY pronta) |
| Total Acumulado em | YTD — acumulado do ano |
| Classificação (do Maior para o Menor) | Ranking automático |

Combinação poderosa: a mesma métrica duas vezes em Valores — uma como soma, outra como
"% da Diferença de mês anterior" — dá o relatório de crescimento pronto sem uma fórmula.

## Agrupamento

- **Datas**: botão direito numa data → `Agrupar` → Meses/Trimestres/Anos (marque Anos junto com
  Meses, senão janeiros de anos diferentes se somam!). Versões novas criam os campos
  automaticamente ao arrastar uma data para Linhas.
- **Números**: agrupar por faixas (0–100, 100–200...) cria histogramas de ticket, idade, prazo.
- **Manual**: selecione itens de texto → Agrupar → cria categorias ad hoc (ex.: juntar UFs em
  regiões) — mas se o agrupamento é permanente, prefira uma coluna DE-PARA na base.

## Campos calculados

`Analisar → Campos, Itens e Conjuntos → Campo Calculado`: cria métrica derivada
(ex.: `= Faturamento - Custo`).

**Limitação crítica**: o campo calculado opera sobre **somas**, não linha a linha. Margem como
`=Lucro/Faturamento` funciona (razão de somas — correto); mas `=Qtde*Preço` multiplica a soma
das quantidades pela soma dos preços — **errado**. Cálculo linha a linha pertence à base
(coluna auxiliar) ou ao Modelo de Dados (medida DAX). Na dúvida: crie a coluna na base.

## Segmentações e linha do tempo

- **Segmentação de Dados** (*Slicer*): `Inserir → Segmentação de Dados` — botões de filtro
  visíveis. `Ctrl+clique` para multi-seleção.
- **Linha do Tempo**: filtro deslizante específico para datas (anos → dias).
- **Conexões de Relatório** (botão direito na segmentação): uma segmentação controla **várias
  dinâmicas ao mesmo tempo** — este é o mecanismo que transforma um conjunto de dinâmicas num
  dashboard interativo (ver `dashboards.md`).

## Boas práticas e armadilhas

1. **Uma dinâmica por análise**, todas alimentadas pela mesma Tabela nomeada, em abas de
   cálculo — nunca montadas em cima da aba de apresentação.
2. Dinâmicas criadas da mesma fonte compartilham cache: agrupar datas numa **agrupa em todas**.
   Se precisar de agrupamentos independentes, crie a segunda dinâmica com fonte re-selecionada
   ou colunas de período na base.
3. **"GETPIVOTDATA apareceu do nada"**: ao referenciar célula de dinâmica em fórmula, o Excel
   insere `INFODADOSTABELADINÂMICA`. Ela é *estável* (segue o valor mesmo se a dinâmica mudar
   de tamanho) — boa para alimentar cartões de dashboard; se preferir referência simples,
   digite o endereço manualmente ou desligue em Opções.
4. Nova linha na fonte não aparece? Fonte não era Tabela nomeada (intervalo fixo) — converta
   com `Ctrl+Alt+T` e a dinâmica passa a acompanhar o crescimento (após Atualizar).
5. Ao apresentar: desligue "Autoajustar largura das colunas ao atualizar" (Opções da Tabela
   Dinâmica) para o layout parar de pular a cada atualização.

## Modelo de Dados

Quando marcar "Adicionar ao Modelo de Dados" ao criar a dinâmica (ou usar Power Pivot):

- **Relacionamentos entre tabelas**: Vendas + Clientes + Produtos relacionados por chave —
  a dinâmica cruza as três **sem nenhum PROCV** para achatar a base.
- **Contagem Distinta** (Distinct Count) como agregação — indisponível na dinâmica comum;
  essencial para "quantos clientes únicos compraram".
- **Medidas DAX**: cálculos linha a linha corretos, inteligência de tempo, e a porta de entrada
  conceitual para o Power BI (mesma engine, mesma linguagem).

Sinal de que é hora de usar: você está mantendo PROCVs para juntar 2+ tabelas grandes só para
alimentar uma dinâmica, ou precisa de contagem distinta.
