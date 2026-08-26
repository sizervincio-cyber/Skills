---
name: analise-de-dados-excel
description: Base de conhecimento de alto nível sobre análise de dados, planilhas (Excel/Google Sheets) e dashboards, inspirada nos temas do perfil @start.ex (Excel & Análise de dados, de Tiago Barbosa). Use esta skill sempre que o usuário pedir ajuda com Excel, fórmulas, PROCV/PROCX, tabelas dinâmicas, Power Query, limpeza de dados, KPIs, gráficos, dashboards, relatórios gerenciais ou análise de dados em geral — mesmo que ele não diga a palavra "planilha" (ex.: "organizar esses dados de vendas", "montar um painel de indicadores", "criar um relatório para a diretoria", "resumir essa base de clientes").
---

# Análise de Dados, Planilhas e Dashboards — Base de Conhecimento

Base de conhecimento em português (pt-BR) sobre análise de dados com planilhas e construção de
dashboards, cobrindo do fundamento ao nível avançado. Origem: os temas centrais do perfil
@start.ex — Excel & Análise de dados (Instagram, Tiago Barbosa) — consolidados e expandidos
com as práticas de referência da área. Esta é a versão em arquivo único: todo o conteúdo
está neste documento, organizado em sete partes.

## Como usar

1. Identifique em qual etapa do trabalho o usuário está (ver "O processo de análise" abaixo).
2. Consulte a parte correspondente deste documento antes de responder ou construir algo.
3. Responda em português, usando os nomes de funções em pt-BR com o equivalente em inglês
   entre parênteses na primeira menção — ex.: `PROCX` (`XLOOKUP`) — porque o usuário pode
   estar com o Excel em qualquer idioma.
4. Ao gerar planilhas ou dashboards de verdade (arquivos .xlsx), combine esta base de
   conhecimento com a skill `xlsx`; para dashboards em HTML, combine com `dataviz`.

## O processo de análise (espinha dorsal)

Toda análise séria percorre estas etapas — use-as para diagnosticar onde o usuário está
e o que ele realmente precisa:

1. **Pergunta** — que decisão essa análise vai apoiar? Sem pergunta clara, não há análise, há tabela.
2. **Coleta** — obter os dados (exportação, integração, digitação) e conhecer suas limitações.
3. **Limpeza e preparação** — 60–80% do trabalho real. Dados em formato tabular, sem mesclas,
   uma linha por registro, uma coluna por variável.
4. **Análise** — agregar, comparar, segmentar, encontrar padrões e exceções.
5. **Visualização** — traduzir números em gráficos que respondem à pergunta.
6. **Comunicação e decisão** — dashboard ou relatório que leva à ação, não à contemplação.

## Sumário do documento

| Parte | Conteúdo | Consulte quando a tarefa envolver |
|---|---|---|
| 1 | Fundamentos de Análise de Dados | Conceitos, tipos de análise, estatística essencial, métricas, erros de raciocínio analítico |
| 2 | Fórmulas e Funções Essenciais | PROCV/PROCX, SOMASES, ÍNDICE+CORRESP, SE, texto, datas, matriciais dinâmicas, LET/LAMBDA |
| 3 | Limpeza e Preparação de Dados | Limpar, padronizar e estruturar dados; Power Query; bases bagunçadas; duplicatas |
| 4 | Tabelas Dinâmicas | Resumir, agrupar, campos calculados, segmentações, Modelo de Dados |
| 5 | Dashboards | Escolha de KPIs, layout, interatividade, construção no Excel, checklist de qualidade |
| 6 | Gráficos e Visualização de Dados | Escolher o gráfico certo, boas práticas, storytelling com dados |
| 7 | Ferramentas e Ecossistema | Excel × Google Sheets × Power BI × SQL/Python, automação, atalhos, governança |

## Princípios que atravessam tudo

- **Dado bruto separado de análise separado de apresentação.** Estruture qualquer arquivo em três
  camadas: abas de dados (tabelas limpas), abas de cálculo (dinâmicas e medidas) e a aba de
  apresentação (dashboard). Nunca digite dado por cima de fórmula nem fórmula no meio do dado bruto.
- **Tabela formatada sempre** (`Ctrl+Alt+T` no Excel pt-BR / *Format as Table*): dá nome ao intervalo,
  expande fórmulas e alimenta dinâmicas e gráficos sem quebrar referências.
- **Fórmula boa é fórmula legível.** Prefira `PROCX` a `PROCV`, `LET` a repetições, colunas auxiliares
  nomeadas a mega-fórmulas de uma linha.
- **Todo número em um dashboard precisa de contexto**: comparação com meta, com período anterior
  ou com benchmark. Número solto não informa, decora.
- **Automatize o que se repete**: se a mesma limpeza acontece todo mês, ela pertence ao Power Query,
  não aos seus dedos.

---

# Parte 1 — Fundamentos de Análise de Dados

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

---

# Parte 2 — Fórmulas e Funções Essenciais (Excel / Google Sheets)

Nomes em **pt-BR** com o equivalente em inglês entre parênteses. Separador de argumentos no
Excel pt-BR é `;` (ponto e vírgula); em inglês e no Google Sheets configurado em en-US é `,`.

## Sumário
1. [Procura e referência](#procura-e-referência)
2. [Agregação condicional](#agregação-condicional)
3. [Lógica](#lógica)
4. [Texto](#texto)
5. [Datas](#datas)
6. [Matriciais dinâmicas (Excel 365)](#matriciais-dinâmicas-excel-365)
7. [LET e LAMBDA](#let-e-lambda)
8. [Tratamento de erros](#tratamento-de-erros)
9. [Boas práticas de fórmulas](#boas-práticas-de-fórmulas)

---

## Procura e referência

### PROCX (XLOOKUP) — a escolha padrão
```
=PROCX(valor_procurado; matriz_procura; matriz_retorno; [se_não_encontrado]; [modo_correspondência]; [modo_pesquisa])
=PROCX(A2; Clientes[CPF]; Clientes[Nome]; "não encontrado")
```
Vantagens sobre PROCV: procura para qualquer direção (esquerda inclusive), não quebra quando
colunas são inseridas, tem argumento nativo para "não encontrado", e faz busca do fim para o
início (`modo_pesquisa = -1`) — útil para "última ocorrência".

### PROCV (VLOOKUP) — o clássico
```
=PROCV(valor; tabela; nº_coluna; FALSO)
```
- **Sempre termine com `FALSO`** (ou `0`) para correspondência exata; o padrão `VERDADEIRO`
  retorna correspondência aproximada e produz resultados silenciosamente errados em base
  não ordenada. Exceção legítima: tabelas de faixa (imposto, comissão por faixa), onde o
  aproximado com base ordenada é exatamente o que se quer.
- Limitação: só procura da esquerda para a direita e o `nº_coluna` quebra se alguém inserir coluna.

### ÍNDICE + CORRESP (INDEX + MATCH)
```
=ÍNDICE(matriz_retorno; CORRESP(valor; matriz_procura; 0))
```
Substituto robusto do PROCV quando não há PROCX (versões antigas). Com dois CORRESP vira
busca bidimensional (linha e coluna):
```
=ÍNDICE(Tabela; CORRESP(cliente; coluna_clientes; 0); CORRESP(mês; linha_meses; 0))
```

### Busca com múltiplos critérios
Concatene as chaves ou use PROCX com `&`:
```
=PROCX(A2&"|"&B2; Tabela[Região]&"|"&Tabela[Produto]; Tabela[Preço])
```

## Agregação condicional

| Função | Uso |
|---|---|
| `SOMASE` (SUMIF) | Soma com 1 critério |
| `SOMASES` (SUMIFS) | Soma com vários critérios — **use sempre esta**, mesmo com 1 critério: a ordem dos argumentos é mais consistente |
| `CONT.SES` (COUNTIFS) | Contagem com critérios |
| `MÉDIASES` (AVERAGEIFS) | Média com critérios |
| `MÁXIMOSES`/`MÍNIMOSES` (MAXIFS/MINIFS) | Máximo/mínimo com critérios |

```
=SOMASES(Vendas[Valor]; Vendas[Região]; "Sul"; Vendas[Data]; ">="&DATA(2026;1;1); Vendas[Data]; "<"&DATA(2026;2;1))
```
- Critérios com comparação e célula: `">="&F1` (o `&` é obrigatório).
- Curinga: `"*silva*"` (contém), `"?"` (um caractere). Para procurar `*` literal, use `~*`.
- `SOMARPRODUTO` (SUMPRODUCT) resolve condições que SOMASES não aceita (OU entre colunas,
  cálculos linha a linha): `=SOMARPRODUTO((Região="Sul")*(Qtde)*(Preço))`.

## Lógica

```
=SE(condição; se_verdadeiro; se_falso)                       (IF)
=SES(cond1; res1; cond2; res2; ...; VERDADEIRO; padrão)      (IFS)
=E(...) =OU(...) =NÃO(...)                                   (AND / OR / NOT)
```
- Para faixas (notas, comissões), **prefira uma tabela de faixas + PROCX aproximado** a um SE
  aninhado de 7 níveis: mais legível e a regra vira dado editável, não fórmula.
- `SES` elimina o aninhamento: cada par condição→resultado em sequência.

## Texto

| Função | Uso |
|---|---|
| `ARRUMAR` (TRIM) | Remove espaços extras — primeira coisa a aplicar em dado importado |
| `MAIÚSCULA`/`MINÚSCULA`/`PRI.MAIÚSCULA` (UPPER/LOWER/PROPER) | Padronizar caixa |
| `TEXTOANTES`/`TEXTODEPOIS` (TEXTBEFORE/TEXTAFTER) | Extrair antes/depois de um delimitador |
| `DIVIDIRTEXTO` (TEXTSPLIT) | Quebra texto em colunas por delimitador |
| `UNIRTEXTO` (TEXTJOIN) | Junta com delimitador ignorando vazios: `=UNIRTEXTO(", "; VERDADEIRO; A1:A10)` |
| `EXT.TEXTO`/`ESQUERDA`/`DIREITA` (MID/LEFT/RIGHT) | Extração por posição |
| `SUBSTITUIR` (SUBSTITUTE) | Troca texto por texto (ex.: remover pontos de CPF) |
| `TEXTO` (TEXT) | Número → texto formatado: `=TEXTO(A1; "dd/mm/aaaa")`, `=TEXTO(A1; "R$ #.##0,00")` |
| `NÚM.CARACT` (LEN) | Tamanho — ótimo para validar CPF/CEP/códigos |

Limpeza clássica de código importado: `=ARRUMAR(SUBSTITUIR(SUBSTITUIR(A2;".";"");"-";""))`.

## Datas

- Data no Excel é número serial (dias desde 1900); hora é fração do dia. Se a "data" está
  alinhada à esquerda, é texto — converta com `DATA.VALOR` (DATEVALUE) ou Power Query.
- `HOJE()` (TODAY), `AGORA()` (NOW) — voláteis, recalculam sempre.
- `DATA(ano; mês; dia)` (DATE) — monta datas com aritmética embutida: `=DATA(2026;13;1)` = jan/2027.
- `FIMMÊS(data; n)` (EOMONTH) — fim do mês n meses à frente; `FIMMÊS(data;-1)+1` = primeiro dia do mês.
- `DATADIF(início; fim; "y"|"m"|"d")` (DATEDIF) — idade e tempos de casa (função "escondida", sem ajuda no Excel).
- `DIATRABALHOTOTAL(início; fim; feriados)` (NETWORKDAYS) — dias úteis; `DIATRABALHO` (WORKDAY) projeta prazos.
- Agrupamento por mês para análise: crie coluna `=FIMMÊS(Data;0)` ou `=TEXTO(Data;"aaaa-mm")` —
  a primeira mantém-se ordenável como data, a segunda como texto.

## Matriciais dinâmicas (Excel 365)

Fórmulas que "transbordam" (spill) resultados para várias células. Referencie o intervalo
transbordado com `#`: `=SOMA(F2#)`.

| Função | Faz |
|---|---|
| `FILTRO` (FILTER) | `=FILTRO(Tabela; (Tabela[UF]="SP")*(Tabela[Valor]>1000); "sem resultado")` |
| `CLASSIFICAR` (SORT) | Ordena um intervalo por qualquer coluna |
| `ÚNICO` (UNIQUE) | Lista valores distintos — base perfeita para relatórios e validação de dados |
| `SEQUÊNCIA` (SEQUENCE) | Gera séries numéricas/datas |
| `AGRUPARPOR` (GROUPBY) / `EMPILHARV` (VSTACK) | Mini tabela dinâmica por fórmula / empilhar tabelas |

Combinação típica de mini-relatório vivo:
```
=CLASSIFICAR(ÚNICO(Vendas[Vendedor]))          ← rótulos
=SOMASES(Vendas[Valor]; Vendas[Vendedor]; F2#) ← valores acompanham o transbordo
```

## LET e LAMBDA

`LET` nomeia cálculos intermediários — a fórmula fica legível e mais rápida (evita repetir o
mesmo cálculo):
```
=LET(margem; (Preço-Custo)/Preço;
     SE(margem<0; "prejuízo"; TEXTO(margem;"0,0%")))
```
`LAMBDA` cria funções próprias reutilizáveis (defina no Gerenciador de Nomes):
```
LIMPACPF = LAMBDA(x; ARRUMAR(SUBSTITUIR(SUBSTITUIR(x;".";"");"-";"")))
```
Uso na planilha: `=LIMPACPF(A2)`. Se a mesma lógica aparece em 3+ lugares, ela merece um LAMBDA.

## Tratamento de erros

```
=SEERRO(fórmula; valor_se_erro)     (IFERROR)  — captura qualquer erro
=SENÃODISP(fórmula; valor)          (IFNA)     — captura só #N/D, preferível em PROCX/PROCV
```
- **Não embrulhe tudo em SEERRO por reflexo**: um `#DIV/0!` escondido atrás de `0` vira número
  errado somado no total. Trate a causa (`SE(B2=0;...)`) e reserve SEERRO/SENÃODISP para o
  caso legítimo de "não encontrado".
- Erros comuns: `#N/D` (não achou), `#REF!` (referência apagada), `#VALOR!` (tipo errado,
  frequentemente número-como-texto), `#NOME?` (função inexistente — ou idioma trocado).

## Boas práticas de fórmulas

1. **Tabelas nomeadas** (`Ctrl+Alt+T`): `=SOMASES(Vendas[Valor]; ...)` se explica sozinha;
   `=SOMASES($D$2:$D$9871; ...)` não.
2. **Trave referências com propósito**: `$A$1` (fixa tudo), `A$1` (fixa linha), `$A1` (fixa
   coluna). `F4` alterna. Erro de travamento é a causa nº 1 de fórmula arrastada errada.
3. **Uma fórmula, uma responsabilidade**: quebre cálculos complexos em colunas auxiliares
   nomeadas (pode ocultá-las). Depurar 4 colunas simples é trivial; uma mega-fórmula, não.
4. **Audite**: `Ctrl+[` vai aos precedentes; "Avaliar Fórmula" executa passo a passo; selecione
   um trecho da fórmula e `F9` mostra o valor parcial (Esc para sair sem gravar).
5. **Número guardado como texto** é a praga clássica de dado exportado: `=A1*1` ou "Texto para
   Colunas" converte; `ÉNÚM`/`ÉTEXTO` diagnostica. Sintoma: SOMA que dá zero com a coluna cheia.

---

# Parte 3 — Limpeza e Preparação de Dados

Limpeza é 60–80% do trabalho real de análise. Dado limpo torna todas as etapas seguintes
triviais; dado sujo contamina tudo silenciosamente.

## Sumário
1. [A forma correta de uma base de dados](#a-forma-correta-de-uma-base-de-dados)
2. [Checklist de limpeza](#checklist-de-limpeza)
3. [Problemas clássicos e soluções](#problemas-clássicos-e-soluções)
4. [Power Query — a ferramenta certa para limpeza recorrente](#power-query)
5. [Validação e proteção contra sujeira futura](#validação-e-proteção)

---

## A forma correta de uma base de dados

O formato **tabular (tidy)** — o único que tabelas dinâmicas, fórmulas e Power BI consomem bem:

- **Uma linha = um registro** (uma venda, um cliente, um atendimento).
- **Uma coluna = uma variável**, com cabeçalho único na linha 1, sem cabeçalhos duplos.
- **Uma célula = um valor** — nunca "João; Maria; Pedro" numa célula só.
- **Sem células mescladas**, sem linhas de total no meio, sem linhas em branco separando "blocos".
- **Sem colunas de meses** (Jan, Fev, Mar...): isso é dado *largo* (formato de apresentação).
  Para análise, converta para *longo*: colunas `Mês` e `Valor`. (Power Query: selecionar as
  colunas de meses → "Transformar colunas em linhas" / *Unpivot*.)
- Tipos consistentes por coluna: data é data, número é número, texto é texto.
- Formate como **Tabela** (`Ctrl+Alt+T`) e dê nome (`Vendas`, `Clientes`).

Regra de ouro da estrutura do arquivo: **abas de dados ≠ abas de cálculo ≠ aba de apresentação**.
Relatório bonito com totais e mesclas é produto final, nunca fonte.

## Checklist de limpeza

Execute nesta ordem ao receber qualquer base:

1. **Conheça a base**: quantas linhas? Total financeiro bate com a fonte oficial? Qual o
   período coberto? (Concilie ANTES de limpar — para saber se perdeu algo no caminho.)
2. **Remova o que não é dado**: linhas de título, totais/subtotais embutidos, linhas em branco,
   colunas vazias, lixo de exportação (cabeçalho do sistema repetido a cada "página").
3. **Duplicatas**: `Dados → Remover Duplicatas`, mas antes **conte-as** (`CONT.SES` sobre a chave
   ou formatação condicional → valores duplicados) e entenda por que existem — duplicata às vezes
   é registro legítimo (duas compras iguais no mesmo dia) e às vezes é bug da extração.
4. **Espaços e invisíveis**: `ARRUMAR` (TRIM) em todo texto-chave; `SUBSTITUIR(x;CARACT(160);"")`
   remove o espaço não separável (NBSP) que vem de sistemas web e derrota o PROCV.
5. **Padronize categorias**: "SP", "sp", "São Paulo", "S. Paulo" → um valor só. Levante o dicionário
   com `ÚNICO` (UNIQUE), monte uma tabela DE-PARA e aplique com PROCX. Guarde a DE-PARA — ela
   será reutilizada todo mês.
6. **Converta tipos**: números-como-texto (`=A1*1`, ou Texto para Colunas → Concluir), datas-texto
   (`DATA.VALOR`, ou Power Query com o *locale* certo), moeda com "R$" embutido no texto.
7. **Trate vazios**: vazio é "zero", "não se aplica" ou "não coletado"? São três significados
   diferentes e três tratamentos diferentes. Nunca preencha com zero sem saber qual é o caso.
8. **Valide chaves**: CPF/CNPJ/código com `NÚM.CARACT` (LEN) uniforme? IDs órfãos (venda com
   cliente que não existe no cadastro)? `CONT.SE` cruzado encontra.
9. **Procure impossíveis**: datas no futuro, valores negativos onde não deveria, idade 340,
   quantidade zero com faturamento positivo. Filtros e classificação (menor→maior) revelam.
10. **Documente**: uma aba `_leia-me` com fonte, data de extração, transformações aplicadas e
    decisões tomadas ("excluí 12 linhas de teste do sistema"). Seu eu-do-futuro agradece.

## Problemas clássicos e soluções

| Sintoma | Causa provável | Solução |
|---|---|---|
| SOMA dá zero com coluna cheia | Números guardados como texto | `=A1*1`, Texto para Colunas, ou Power Query (tipo Número) |
| PROCV dá #N/D mas o valor "está lá" | Espaços, NBSP, tipo diferente (texto vs número) | `ARRUMAR`, remover CARACT(160), igualar tipos |
| Datas viram texto ou trocam dia/mês | Exportação em locale americano (mm/dd) | Power Query → Alterar Tipo → Usando Localidade → Inglês (EUA) |
| Acentuação quebrada (Ã©, Ã§) | CSV aberto com codificação errada | Importar via Power Query escolhendo UTF-8 (ou Windows-1252) |
| Zeros à esquerda somem (CEP, código) | Excel converteu para número | Importar a coluna como Texto; formatar depois não recupera |
| CSV abre tudo numa coluna | Separador `;` vs `,` divergente do locale | Power Query ou Texto para Colunas escolhendo o delimitador |
| Base "larga" (meses em colunas) | Formato de apresentação usado como fonte | *Unpivot* no Power Query |
| Total muda a cada atualização | Duplicatas trazidas pela extração | Identificar chave única e deduplicar na consulta |

## Power Query

Regra de decisão: **limpeza que se repete pertence ao Power Query** (Excel: `Dados → Obter Dados`;
mesma engine do Power BI). Limpeza pontual de uma base que nunca mais volta pode ser feita na mão.

Por que ele muda o jogo:
- **Gravável e reexecutável**: cada transformação vira um passo registrado. Chegou o arquivo do
  mês seguinte? `Atualizar` refaz tudo em segundos. É a diferença entre "limpar a base" (toda vez)
  e "construir a limpeza" (uma vez).
- **Não destrutivo**: o arquivo-fonte permanece intacto; a consulta gera uma tabela nova.
- **Conecta em tudo**: pasta inteira de CSVs (combina automaticamente os arquivos — ideal para
  "um arquivo por mês"), outras planilhas, bancos SQL, web, PDF.

Transformações do dia a dia (aba Transformar / Página Inicial):
- Remover linhas superiores / linhas em branco; usar primeira linha como cabeçalho.
- Alterar tipo (com Localidade, para datas e decimais estrangeiros).
- Dividir coluna por delimitador; Coluna de Exemplos (o Power Query infere a regra do padrão
  que você digita — excelente para extrações complexas).
- Transformar colunas em linhas (*Unpivot*) — o remédio para bases largas.
- Mesclar consultas (= PROCV entre tabelas, escolhendo o tipo de junção) e Acrescentar
  consultas (= empilhar bases do mesmo layout).
- Agrupar por (= tabela dinâmica persistida).
- Formatar → aparar/maiúsculas; Substituir valores.

Boas práticas: renomeie os passos com nomes descritivos; uma consulta por fonte + consultas de
combinação separadas; desabilite o carregamento das consultas intermediárias ("Habilitar carga"
desmarcado) para não poluir o arquivo.

## Validação e proteção

Impedir sujeira nova custa menos que limpar:

- **Validação de Dados** (`Dados → Validação de Dados`): lista suspensa a partir de uma tabela de
  categorias (fonte: `=INDIRETO("Categorias[Nome]")` ou intervalo nomeado), limites de data/número,
  `NÚM.CARACT` fixo para códigos, e mensagem de erro explicando o formato esperado.
- **Formatação condicional de anomalias**: duplicatas em vermelho, datas futuras, valores fora
  da faixa — a planilha "grita" quando algo entra errado.
- **Proteção de planilha**: desbloqueie só as células de entrada e proteja o resto (Revisão →
  Proteger Planilha) para fórmulas não serem sobrescritas por acidente.
- **Entrada padronizada**: se várias pessoas alimentam a base, dê a elas um formulário ou uma
  tabela com validação — nunca "cola aí no final".

---

# Parte 4 — Tabelas Dinâmicas (Pivot Tables)

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

Pré-requisito: base tabular limpa, formatada como Tabela nomeada (ver a Parte 3 — Limpeza e Preparação de Dados).
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
  dashboard interativo (ver a Parte 5 — Dashboards).

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

---

# Parte 5 — Dashboards

Dashboard é uma interface de decisão: mostra, numa tela, o estado do que importa e onde agir.
Não é coleção de gráficos, não é relatório longo, não é enfeite.

## Sumário
1. [Antes de abrir a ferramenta](#antes-de-abrir-a-ferramenta)
2. [Tipos de dashboard](#tipos-de-dashboard)
3. [Layout e hierarquia visual](#layout-e-hierarquia-visual)
4. [Os componentes](#os-componentes)
5. [Construção no Excel, passo a passo](#construção-no-excel-passo-a-passo)
6. [Interatividade](#interatividade)
7. [Design: cor, fonte, ruído](#design-cor-fonte-ruído)
8. [Checklist de qualidade](#checklist-de-qualidade)

---

## Antes de abrir a ferramenta

Três perguntas que definem 80% do resultado — responda por escrito antes de montar qualquer coisa:

1. **Quem usa e que decisão toma?** Diretor decide alocação (visão agregada, tendência);
   coordenador decide a semana (operacional, detalhe por pessoa); analista investiga (drill-down).
   Um dashboard por público. "Dashboard para todo mundo" não serve a ninguém.
2. **Quais são as 5–8 perguntas que a tela responde?** Ex.: "Vamos bater a meta do mês?",
   "Qual região está puxando para baixo?", "O funil piorou em qual etapa?". Cada componente do
   dashboard deve responder a uma dessas perguntas — componente sem pergunta é decoração.
3. **Com que frequência atualiza e de onde vem o dado?** Isso define a arquitetura (manual ×
   Power Query × Power BI) e o esforço de manutenção.

## Tipos de dashboard

| Tipo | Público | Conteúdo | Frequência |
|---|---|---|---|
| **Estratégico** | Diretoria | Poucos KPIs vs. meta, tendência longa, visão consolidada | Mensal |
| **Tático/gerencial** | Gerência | KPIs quebrados por área/região/equipe, comparativos | Semanal |
| **Operacional** | Operação | Estado de agora: fila, SLA, produção do dia, alertas | Diária/tempo real |
| **Analítico** | Analistas | Exploração: muitos filtros, drill-down, detalhe | Sob demanda |

## Layout e hierarquia visual

- **Leitura em Z**: o olho ocidental varre de cima-esquerda para baixo-direita. Ordem do conteúdo:
  1. **Topo**: faixa de KPIs (cartões) — o resumo executivo em 5 segundos.
  2. **Meio**: tendências e comparações — os "porquês" (evolução no tempo, quebras principais).
  3. **Base**: detalhe — tabelas, rankings, itens individuais.
- **Grade**: alinhe tudo numa grade consistente (margens e espaçamentos iguais). Desalinhamento
  transmite desleixo e dificulta a varredura.
- **Uma tela, sem rolagem** para a visão principal. Se não cabe, o dashboard está tentando
  responder perguntas demais — divida em páginas/abas por tema.
- **5–8 KPIs no topo, no máximo**. Acima disso nada se destaca e tudo compete.
- Título que diz o escopo: "Vendas — Brasil — Ago/2026 (atualizado 26/08 08h)". Data de
  atualização **sempre** visível.

## Os componentes

- **Cartão de KPI**: número grande + rótulo + comparação (vs. meta, vs. período anterior) +
  indicação de direção (▲▼ com cor). Número sem comparação não informa.
- **Linha**: tendência no tempo — o gráfico mais importante de quase todo dashboard.
- **Barras/colunas**: comparação entre categorias e rankings.
- **Funil ou barras empilhadas 100%**: composição e conversão por etapa.
- **Tabela**: detalhe fino, com formatação condicional (barras de dados, ícones) — no rodapé,
  nunca como protagonista.
- **Segmentações**: os filtros do usuário — período, região, canal.
- **Minigráficos** (*sparklines*, `Inserir → Minigráfico`): tendência dentro de célula, perfeitos
  ao lado de cada linha de uma tabela-resumo.
- Escolha de cada gráfico: ver a Parte 6 — Gráficos e Visualização de Dados.

## Construção no Excel, passo a passo

Arquitetura de três camadas (inegociável para manutenção):

```
[abas de dados]  →  [abas de cálculo]  →  [aba dashboard]
 Tabelas limpas      Tabelas dinâmicas       Só apresentação:
 (ou Power Query)    e células de apoio      cartões, gráficos, segmentações
```

1. **Dados**: base tabular limpa, como Tabela nomeada (via Power Query se recorrente).
2. **Cálculo**: uma tabela dinâmica por análise, numa aba `calc` (que ficará oculta). Nomeie
   cada dinâmica (Analisar → Nome da Tabela Dinâmica) pelo que ela responde.
3. **Cartões de KPI**: caixas de texto ou formas cujo conteúdo aponta para células de apoio
   (selecione a forma → digite `=calc!$B$2` na barra de fórmulas). As células de apoio puxam
   das dinâmicas (via `INFODADOSTABELADINÂMICA`/GETPIVOTDATA, que é estável) e calculam a
   variação vs. meta/mês anterior.
4. **Gráficos dinâmicos**: criados a partir das dinâmicas — acompanham filtros e atualizações.
   Limpe cada um: sem botões de campo (botão direito → Ocultar Botões de Campo), sem linhas de
   grade pesadas, título que afirma algo.
5. **Segmentações conectadas**: insira segmentações e conecte cada uma a **todas** as dinâmicas
   relevantes (botão direito → Conexões de Relatório). É isso que faz a tela inteira reagir
   junta a um clique.
6. **Acabamento**: oculte linhas de grade (Exibir → desmarcar Linhas de Grade), oculte as abas
   de cálculo, proteja a estrutura, congele o painel do título.
7. **Atualização**: com Power Query + dinâmicas, o ciclo mensal vira: colar/receber arquivo novo
   → `Dados → Atualizar Tudo` → conferir → publicar. Documente isso na aba `_leia-me`.

## Interatividade

- **Segmentações + Linha do Tempo** conectadas a múltiplas dinâmicas = filtro global.
- **Seleção de métrica/período com Validação de Dados**: uma célula suspensa ("Faturamento /
  Margem / Pedidos") + fórmulas com `PROCX`/`SES` que trocam a série do gráfico — dá ao usuário
  a troca de visão sem duplicar gráficos.
- **Drill-down natural**: duplo clique em qualquer número de dinâmica abre o detalhe. Avise o
  usuário — é a resposta pronta para "me mostra o que compõe isso".
- Não exagere: cada filtro adicionado multiplica os estados possíveis da tela e as chances de
  alguém tirar conclusão de um recorte enviesado. Filtros essenciais apenas.

## Design: cor, fonte, ruído

- **Cor é informação, não decoração.** Uma cor primária para a série principal, cinza para
  contexto/histórico, e cor de alerta (vermelho/âmbar) **somente** para o que está fora da meta.
  Se tudo é colorido, nada se destaca.
- Semáforo com parcimônia: verde/vermelho em variações e status; lembre do daltonismo
  (~8% dos homens) — acompanhe cor com sinal (▲▼) ou texto.
- **Uma família de fonte**, 2–3 tamanhos (título, número de KPI, corpo). Números de KPI grandes
  (24–36pt); rótulos discretos.
- **Elimine ruído** (*data-ink ratio*): fundos escuros gratuitos, bordas grossas, sombras, 3D,
  logotipos gigantes — tudo que não é dado compete com o dado. Fundo branco/neutro, separação
  por espaço em branco, não por caixas.
- Formato de número brasileiro consistente: `R$ 1,2 mi` em cartões (abrevie milhares/milhões),
  eixo sem decimais inúteis, `%` com 1 decimal no máximo.

## Checklist de qualidade

Antes de publicar, verifique:

- [ ] Um desconhecido entende o que a tela mostra em 10 segundos (teste com alguém).
- [ ] Todo número tem comparação (meta, período anterior ou benchmark).
- [ ] Os totais batem com a fonte oficial (concilie!).
- [ ] Filtros aplicados são visíveis (nada de tela filtrada parecendo total).
- [ ] Data de atualização visível; processo de atualização documentado e testado com dado novo.
- [ ] Funciona na tela de quem vai usar (projetor? notebook 13"? celular?).
- [ ] Zero informação que ninguém pediu (cada componente responde a uma das perguntas definidas).
- [ ] Abas de cálculo ocultas, estrutura protegida, arquivo abre no dashboard.

---

# Parte 6 — Gráficos e Visualização de Dados

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

---

# Parte 7 — Ferramentas e Ecossistema

Qual ferramenta usar, quando migrar, e como ser rápido na que estiver usando.

## Sumário
1. [Excel × Google Sheets × Power BI × SQL/Python](#comparativo)
2. [Sinais de que é hora de migrar](#sinais-de-migração)
3. [Power BI: o essencial conceitual](#power-bi-essencial)
4. [Automação no Excel: Power Query, VBA e Office Scripts](#automação-no-excel)
5. [Atalhos e produtividade no Excel](#atalhos-e-produtividade)
6. [Colaboração e governança de planilhas](#colaboração-e-governança)

---

## Comparativo

| Critério | Excel | Google Sheets | Power BI | SQL + Python |
|---|---|---|---|---|
| Ponto forte | Análise ad hoc, dinâmicas, Power Query, ubiquidade corporativa | Colaboração em tempo real, integração Google, `IMPORTRANGE`/`QUERY` | Dashboards corporativos, atualização automática, modelo de dados, publicação | Volume grande, reprodutibilidade, estatística/ML, pipelines |
| Volume confortável | Até ~centenas de milhares de linhas (1.048.576 é o teto físico; desempenho degrada antes) | Até ~algumas centenas de milhares de células ativas (teto 10 mi de células) | Milhões de linhas (engine colunar comprimida) | Ilimitado na prática |
| Curva de entrada | Baixa | Baixa | Média (modelo + DAX) | Alta |
| Distribuição | Arquivo (versões se multiplicam) | Link único, sempre atual | Link/app, controle de acesso, agendamento | Depende do stack |
| Automação | Power Query, VBA, Office Scripts | Apps Script | Agendamento nativo, dataflows | Total |

Funções distintivas do Google Sheets: `QUERY` (SQL simplificado dentro da planilha — agrupa,
filtra e ordena numa fórmula), `IMPORTRANGE` (puxa dados de outra planilha via link),
`ARRAYFORMULA`, `GOOGLEFINANCE`. Colaboração simultânea e histórico de versões nativos.

## Sinais de migração

**De planilha para Power BI:**
- O mesmo dashboard é refeito/reenviado toda semana ou mês por e-mail.
- Várias pessoas precisam ver o mesmo painel com dado atualizado (e cada uma tem uma "versão").
- A base passou de algumas centenas de milhares de linhas e o arquivo trava.
- Precisa cruzar 3+ fontes (ERP + CRM + planilhas) de forma recorrente.
- Precisa de controle de acesso por pessoa/área.

**De planilha/BI para SQL + Python:**
- O dado nasce em banco e alguém exporta CSV na mão todo dia (elimine a etapa manual).
- Transformações complexas demais para Power Query manter legível.
- Necessidade de estatística séria, previsão, ML, ou de auditar cada passo (reprodutibilidade).

Migrar não aposenta o Excel: ele continua sendo o melhor rascunho analítico e a interface
universal com o resto da empresa. O padrão maduro é: **dado mora no banco, transformação no
pipeline/Power Query, consumo no BI, exploração pontual no Excel**.

## Power BI: o essencial conceitual

Quem domina Excel já sabe mais Power BI do que imagina:

- **Power Query é o mesmo** (mesma interface, mesma linguagem M) — a limpeza aprendida no Excel
  transfere 1:1.
- **Modelo de dados**: em vez de uma tabela achatada com PROCVs, tabelas separadas relacionadas
  por chaves — **fatos** (eventos: vendas, pedidos) ligados a **dimensões** (quem/o quê/onde/
  quando: clientes, produtos, calendário). Esquema estrela. Sempre crie uma **tabela calendário**
  dedicada e marque-a como tabela de datas.
- **DAX**: linguagem de medidas. Começo de vida:
  - `Faturamento = SUM(Vendas[Valor])`
  - `Margem % = DIVIDE([Lucro]; [Faturamento])` (DIVIDE trata divisão por zero)
  - `Fat. Ano Anterior = CALCULATE([Faturamento]; SAMEPERIODLASTYEAR(Calendario[Data]))`
  - `CALCULATE` = "calcule a medida mudando o filtro" — o coração do DAX.
  - **Medida ≠ coluna calculada**: medida agrega no contexto do visual (use para números);
    coluna calculada materializa linha a linha (use para categorizar/fatiar).
- **Publicação**: relatório publicado no serviço, atualização agendada, acesso controlado —
  é isso que mata o "dashboard por e-mail".

## Automação no Excel

Escada de automação — suba um degrau por vez:

1. **Fórmulas + Tabelas + dinâmicas**: já eliminam a maior parte do retrabalho.
2. **Power Query**: qualquer limpeza/consolidação recorrente (ver a Parte 3 — Limpeza e Preparação de Dados).
   Deve ser o primeiro reflexo, antes de pensar em macro.
3. **VBA/Macros**: quando precisa *agir* (gerar 30 PDFs, enviar e-mails, criar abas por filial).
   Grave a macro para obter o esqueleto, depois limpe o código. Arquivo vira `.xlsm`.
4. **Office Scripts** (Excel web/365): sucessor moderno do VBA para nuvem, integra com Power
   Automate para fluxos agendados (ex.: toda segunda, atualizar e mandar por Teams).
5. **Python** (via `xlwings`/`openpyxl`/`pandas`, ou Python no Excel): quando a lógica supera o
   que planilha expressa com sanidade.

Critério: automatize quando (frequência × tempo gasto × risco de erro manual) superar o custo
de construir. Tarefa mensal de 2h com risco de erro alto = automatize já.

## Atalhos e produtividade

Os que mais pagam o investimento (Excel Windows, pt-BR):

| Atalho | Faz |
|---|---|
| `Ctrl+Alt+T` | Formatar como Tabela — o atalho mais importante da casa |
| `Ctrl+Setas` / `Ctrl+Shift+Setas` | Saltar/selecionar até a borda do bloco de dados |
| `Ctrl+L` / `Ctrl+U` | Localizar / Substituir (no Excel pt-BR; em inglês, `Ctrl+F`/`Ctrl+H`) |
| `F4` | Alterna travamento `$` na fórmula; repete última ação fora dela |
| `F2` | Editar célula (e conferir a fórmula com as referências coloridas) |
| `Alt+=` | AutoSoma |
| `Ctrl+Shift+L` | Liga/desliga filtros |
| `Ctrl+;` | Data de hoje estática |
| `Ctrl+Enter` | Preenche a seleção inteira com a entrada |
| `Ctrl+E` | Preenchimento Relâmpago (Flash Fill) — extrai por exemplo, sem fórmula |
| `Alt+Enter` | Quebra de linha dentro da célula |
| `Ctrl+PgUp/PgDn` | Navegar entre abas |
| `Ctrl+Shift+1` / `Ctrl+Shift+5` | Formato número com milhar / porcentagem |

Hábitos de velocidade: navegue por teclado (mouse é o gargalo); `Alt` mostra as teclas de
menu (sequências como `Alt, C, B` viram memória muscular); nomeie células-chave (Gerenciador
de Nomes) para fórmulas legíveis; duplo clique na alça de preenchimento arrasta a fórmula até
o fim da coluna vizinha.

## Colaboração e governança de planilhas

- **Uma fonte da verdade**: a base mora num lugar só (SharePoint/Drive/banco); cópias locais
  são leitura. "Final_v3_AGORA_VAI.xlsx" é sintoma de processo quebrado.
- **Nomenclatura**: `area_assunto_periodo` (ex.: `vendas_dashboard_2026-08.xlsx`); data em
  `AAAA-MM` para ordenar sozinho.
- **Separação de papéis no arquivo**: células de entrada desbloqueadas e destacadas; todo o
  resto protegido. Quem preenche não deve conseguir quebrar fórmula.
- **Versionamento**: SharePoint/OneDrive/Drive guardam histórico — use-o em vez de multiplicar
  arquivos.
- **Documentação mínima** (aba `_leia-me`): dono, fonte dos dados, data de extração, como
  atualizar, decisões e exclusões. Planilha sem dono morre em 3 meses.
- **Dados sensíveis**: planilha com CPF/salário/saúde vazando por e-mail é incidente LGPD.
  Restrinja acesso na origem, anonimize quando o detalhe não é necessário à análise.
