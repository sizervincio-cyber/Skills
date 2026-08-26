# Fórmulas e Funções Essenciais (Excel / Google Sheets)

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
