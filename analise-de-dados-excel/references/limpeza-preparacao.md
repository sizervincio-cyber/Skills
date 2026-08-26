# Limpeza e Preparação de Dados

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
