# Fundamentos de Análise de Dados

## Sumário
1. [O que é analisar dados](#o-que-é-analisar-dados)
2. [Os quatro tipos de análise](#os-quatro-tipos-de-análise)
3. [Estatística essencial para o dia a dia](#estatística-essencial-para-o-dia-a-dia)
4. [Métricas e KPIs](#métricas-e-kpis)
5. [Erros de raciocínio analítico](#erros-de-raciocínio-analítico)
6. [Perguntas que um bom analista faz](#perguntas-que-um-bom-analista-faz)

---

## O que é analisar dados

Análise de dados é transformar dados brutos em **decisões**. A cadeia de valor é:

```
Dado → Informação → Insight → Decisão → Ação
```

- **Dado**: 1.532 linhas de vendas de agosto.
- **Informação**: faturamento de agosto foi R$ 480 mil, 12% abaixo de julho.
- **Insight**: a queda veio inteira da região Sul, onde o principal vendedor saiu de férias.
- **Decisão**: cobrir férias com redistribuição de carteira a partir do próximo ciclo.

Se o trabalho para na "informação", ele foi relatório, não análise. O padrão de qualidade é:
**toda análise termina com um "e daí?" respondido**.

## Os quatro tipos de análise

| Tipo | Pergunta que responde | Exemplo | Ferramenta típica |
|---|---|---|---|
| **Descritiva** | O que aconteceu? | Vendas por mês, ticket médio, churn do trimestre | Tabela dinâmica, dashboard |
| **Diagnóstica** | Por que aconteceu? | Quebra da queda por região/produto/vendedor | Segmentação, drill-down, comparações |
| **Preditiva** | O que vai acontecer? | Projeção de vendas, tendência de inadimplência | `PREVISÃO.ETS`, regressão, linha de tendência |
| **Prescritiva** | O que devemos fazer? | Cenários, otimização de mix, Solver | Atingir Meta, Solver, simulações |

A maioria do trabalho corporativo vive nas duas primeiras — e o erro mais comum é entregar
a descritiva e parar. A diagnóstica é onde o analista se diferencia: **quebre todo número
agregado nas suas dimensões** (tempo, região, produto, canal, pessoa) até encontrar onde a
variação mora. Variações raramente são uniformes; quase sempre estão concentradas.

## Estatística essencial para o dia a dia

O que é indispensável saber para não errar o básico:

### Medidas de tendência central
- **Média** (`MÉDIA`/`AVERAGE`): sensível a valores extremos. Um cliente que compra R$ 1 milhão
  destrói a média de ticket.
- **Mediana** (`MED`/`MEDIAN`): o valor do meio; robusta a extremos. Para renda, ticket, prazo,
  tempo de atendimento — **prefira a mediana** ou reporte as duas.
- **Moda** (`MODO.ÚNICO`/`MODE.SNGL`): valor mais frequente; útil em dados categóricos-numéricos.

### Medidas de dispersão
- **Desvio-padrão** (`DESVPAD.A`/`STDEV.S`): quanto os valores fogem da média. Duas equipes com a
  mesma média de vendas e desvios muito diferentes são realidades completamente distintas.
- **Amplitude / Percentis** (`PERCENTIL.INC`): P10–P90 descreve a faixa "normal" ignorando extremos.
- **Outliers**: regra prática — valores além de 1,5× o intervalo interquartil (IIQ = P75 − P25).
  Antes de excluir um outlier, investigue: às vezes é erro de digitação, às vezes é o insight.

### Comparações e variações
- **Variação percentual**: `(atual − anterior) / anterior`. Cuidado com base pequena: crescer de
  2 para 4 vendas é "+100%", mas continua sendo 4 vendas.
- **Pontos percentuais vs. porcentagem**: margem que vai de 10% para 15% subiu **5 p.p.**
  (ou 50% em termos relativos). Misturar os dois é fonte clássica de relatório errado.
- **CAGR** (crescimento anual composto): `(final/inicial)^(1/anos) − 1`. Use para comparar
  crescimentos em janelas de tamanhos diferentes.
- **Sazonalidade**: compare agosto com agosto do ano anterior (YoY), não com julho, quando o
  negócio tem ciclo sazonal.

### Correlação
- `CORREL` mede associação linear entre −1 e 1. **Correlação não é causalidade**: sorvete e
  afogamento sobem juntos porque ambos dependem do verão. Antes de afirmar causa, procure a
  variável oculta e o mecanismo.

## Métricas e KPIs

- **Métrica** é qualquer coisa que se mede. **KPI** é a métrica ligada a um objetivo do negócio,
  com meta e dono. Um dashboard não precisa de 40 métricas; precisa de 5–8 KPIs e o resto como
  suporte de diagnóstico.
- Um bom KPI é: **relevante** (muda decisão), **mensurável** com os dados existentes,
  **comparável** (meta, histórico ou benchmark) e **acionável** (alguém pode fazer algo a respeito).
- Estruture famílias de métricas: volume (vendas, leads), eficiência (conversão, CAC, ticket médio),
  qualidade (churn, NPS, retrabalho), tempo (ciclo, SLA).
- **Métrica de vaidade**: cresce sempre e não muda nenhuma decisão (ex.: total acumulado desde a
  fundação). Corte do dashboard.

## Erros de raciocínio analítico

Estes erros derrubam análises tecnicamente perfeitas:

1. **Comparar bases diferentes** — misturar meses de 28 e 31 dias, lojas abertas há 1 mês com
   lojas maduras, carteiras de tamanhos distintos. Normalize (por dia útil, por loja-mês, per capita).
2. **Média de médias** — a média das taxas de conversão das lojas ≠ taxa de conversão da rede.
   Agregue numerador e denominador separados e divida no final (média ponderada).
3. **Paradoxo de Simpson** — a tendência agregada pode inverter a tendência de cada segmento.
   Sempre olhe o agregado E os segmentos.
4. **Viés de sobrevivência** — analisar só os clientes ativos e concluir que "todo mundo está
   satisfeito". Os insatisfeitos já saíram da base.
5. **Cherry-picking de janela** — escolher a data de início que faz o gráfico contar a história
   desejada. Fixe janelas padrão (12 meses, YTD) antes de olhar o resultado.
6. **Precisão falsa** — reportar "23,4782%" quando a coleta tem erro de ±5%. Arredonde para a
   precisão que o dado sustenta.
7. **Confundir meta com previsão** — meta é ambição; previsão é estimativa honesta. O dashboard
   deve mostrar as duas sem misturá-las.

## Perguntas que um bom analista faz

Antes de abrir a planilha:
- Que decisão essa análise vai apoiar? Quem decide? Quando?
- Qual a granularidade mínima necessária (dia? cliente? SKU?)?
- Qual o período e a base de comparação corretos?

Ao receber os dados:
- De onde vieram e quando foram extraídos? Há linhas duplicadas ou faltando?
- O total bate com alguma fonte oficial (financeiro, sistema)? **Sempre concilie o total.**
- Campos-chave têm valores vazios, "Outros", ou categorias inconsistentes?

Antes de entregar:
- Se eu mostrar só um número desta análise, qual seria? Ele responde a pergunta original?
- Alguém que discorda da conclusão: qual seria o primeiro ataque? A análise sobrevive?
