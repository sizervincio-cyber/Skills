---
name: analise-de-dados-excel
description: Base de conhecimento de alto nível sobre análise de dados, planilhas (Excel/Google Sheets) e dashboards, inspirada nos temas do perfil @start.ex (Excel & Análise de dados, de Tiago Barbosa). Use esta skill sempre que o usuário pedir ajuda com Excel, fórmulas, PROCV/PROCX, tabelas dinâmicas, Power Query, limpeza de dados, KPIs, gráficos, dashboards, relatórios gerenciais ou análise de dados em geral — mesmo que ele não diga a palavra "planilha" (ex.: "organizar esses dados de vendas", "montar um painel de indicadores", "criar um relatório para a diretoria", "resumir essa base de clientes").
---

# Análise de Dados, Planilhas e Dashboards

Base de conhecimento em português (pt-BR) sobre análise de dados com planilhas e construção de
dashboards, cobrindo do fundamento ao nível avançado. Origem: os temas centrais do perfil
@start.ex — Excel & Análise de dados (Instagram, Tiago Barbosa) — consolidados e expandidos
com as práticas de referência da área.

## Como usar esta skill

1. Identifique em qual etapa do trabalho o usuário está (ver "O processo de análise" abaixo).
2. Leia o arquivo de referência correspondente antes de responder ou construir algo.
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

## Mapa dos arquivos de referência

Leia somente o que a tarefa pede — cada arquivo é autossuficiente:

| Arquivo | Leia quando a tarefa envolver |
|---|---|
| `references/fundamentos-analise-dados.md` | Conceitos de análise, tipos de análise, estatística essencial, métricas, erros de raciocínio analítico |
| `references/formulas-essenciais.md` | Fórmulas e funções: PROCV/PROCX, SOMASES, ÍNDICE+CORRESP, SE, texto, datas, matriciais dinâmicas (FILTRO, ÚNICO, LET, LAMBDA) |
| `references/limpeza-preparacao.md` | Limpar, padronizar e estruturar dados; Power Query; bases bagunçadas; duplicatas; formatos mistos |
| `references/tabelas-dinamicas.md` | Tabelas dinâmicas: resumir, agrupar, campos calculados, segmentações |
| `references/dashboards.md` | Projetar e montar dashboards: escolha de KPIs, layout, interatividade, checklist de qualidade |
| `references/graficos-visualizacao.md` | Escolher o gráfico certo e aplicar boas práticas de visualização |
| `references/ferramentas-ecossistema.md` | Escolher ferramenta (Excel × Google Sheets × Power BI × SQL/Python), atalhos e produtividade |

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
