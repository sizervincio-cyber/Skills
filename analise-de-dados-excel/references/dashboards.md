# Dashboards

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
- Escolha de cada gráfico: ver `graficos-visualizacao.md`.

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
