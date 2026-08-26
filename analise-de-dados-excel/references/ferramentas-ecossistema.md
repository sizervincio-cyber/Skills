# Ferramentas e Ecossistema

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
2. **Power Query**: qualquer limpeza/consolidação recorrente (ver `limpeza-preparacao.md`).
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
