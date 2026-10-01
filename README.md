# Dashboard de Nacionalização de Custos (Power BI)

Dashboard desenvolvido em Power BI para acompanhar e analisar o **Índice de Nacionalização**, ou seja, o percentual de custos adicionais (impostos, fretes e despesas) que incidem sobre as mercadorias importadas. O painel compara dois períodos (ano base × ano comparado) e permite identificar rapidamente **o que está encarecendo ou barateando o custo de importação**.

> **Observação:** todos os nomes de fornecedores, clientes e itens foram anonimizados para preservar a confidencialidade da empresa.

---

## Filtros globais

Presentes em todas as páginas e aplicados de forma consistente:

- **Mês:** seletor de intervalo (neste exemplo, de junho a dezembro).
- **Base:** ano principal da análise (2025).
- **Comparação:** ano usado como referência (2024).
- **Grupo de Estoque:** permite filtrar por categoria de material (consumo, importado, manutenção etc.).

---

## Página 1: Resumo Geral

Visão executiva, pensada para responder em poucos segundos: *"o custo de nacionalização subiu ou caiu, e por quê?"*

- **Cartões de KPI:** Índice de Nacionalização de 2025 (32,54%), de 2024 (29,89%) e a variação entre eles (+2,65 p.p.).
- **Gráfico de cascata (waterfall):** mostra quanto cada componente (ICMS, I.I., Seguro, Capatazia, Saldo a Apropriar, Frete Collect e Despesas Locais) contribuiu para a variação total. Barras verdes aumentaram o índice e barras vermelhas o reduziram. Neste caso, o **ICMS** foi o principal responsável pela alta.
- **Nacionalização por Fornecedor:** tabela com o índice de cada fornecedor nos dois períodos e a variação, ordenada para destacar os maiores desvios.
- **Tendência Mensal:** gráfico de colunas comparando mês a mês o índice dos dois anos, útil para identificar sazonalidade e picos.
<img width="1224" height="698" alt="pg2" src="https://github.com/user-attachments/assets/c88773e8-3388-49e0-8954-b3fd46ddf094" />


---

## Página 2: Composição de Custos

Mostra **de onde vem o custo** de nacionalização e como ele se distribui.

- **Cartões por ano (2024 e 2025):** Custo Total Nacional, Saldo a Apropriar, Frete Collect, ICMS, Despesas Locais e Imposto de Importação (I.I.), lado a lado para comparação direta.
- **Nacionalização por Grupo de Estoque:** tabela com o índice de cada grupo de materiais no período comparado e no período base, permitindo ver quais categorias pesam mais.
- **Gráficos de rosca:** proporção de cada tipo de custo no total de 2025 e de 2024, mostrando como a estrutura de custos mudou (por exemplo, o aumento da participação do ICMS).
<img width="1221" height="698" alt="pg1" src="https://github.com/user-attachments/assets/45ea61b0-c5ea-401e-98c5-8e048916033c" />
---

## Página 3: Análise de Frete

Foco no impacto logístico dentro do custo de nacionalização.

- **Variação % do Frete:** indicador principal da mudança no custo de frete entre os períodos.
- **Frete Collect por Fornecedor:** tabela com o valor pago por fornecedor em cada ano.
- **Frete por Kg (Aéreo × Marítimo):** comparação do custo unitário por modal nos dois anos.
- **Custo Total por Frete:** gráficos de pizza com a divisão entre modal marítimo e aéreo em cada ano.
- **Frete Collect × Prepaid por Modalidade:** colunas que mostram quanto foi pago em cada tipo de frete (Collect, pago no destino, e Prepaid, pago na origem) por modal, evidenciando mudanças na estratégia logística.
<img width="1226" height="701" alt="pg3" src="https://github.com/user-attachments/assets/5dd30f52-2dce-45a5-8524-784652a284c6" />

---

## Página 4: Análise Granular

Página de detalhamento, para **investigar a causa** das variações vistas nas páginas anteriores.

- **Filtros de Item e Material:** permitem descer ao nível de cada item ou categoria de material.
- **Tabela de Drivers:** mostra cada componente de custo (Seguro, Capatazia, Frete Collect, ICMS, I.I. etc.) nos dois anos, com a participação percentual na variação.
- **Top 10 Aumentos e Top 10 Reduções:** ranking dos fornecedores e impostos que mais aumentaram ou diminuíram o custo, com impacto em R$ e em pontos percentuais.
- **Cartões comparativos (2024 × 2025):** índice projetado e valores consolidados de Custo Total, Despesas Locais, Frete Collect/Prepaid, F.O.B, Seguro e Capatazia.
  <img width="1226" height="697" alt="pg4" src="https://github.com/user-attachments/assets/a8400e11-1e36-4f56-b35f-e6a30106085d" />

---

## Organização do modelo e das medidas

As medidas DAX foram organizadas em **pastas de exibição (Display Folders)** dentro de uma tabela dedicada chamada `Medidas`, separada das tabelas de dados. Isso deixa o modelo limpo e facilita a manutenção e a leitura por outras pessoas.
<img width="392" height="248" alt="Organizacao" src="https://github.com/user-attachments/assets/94d02a14-ffbf-467f-bcaa-f450b14f8f65" />

```text
📁 Medidas
 ├── 📁 Base          → medidas do ano base 
 ├── 📁 Comparacao    → medidas do ano de comparação 
 ├── 📁 Efeito        → cálculo do efeito de cada componente na variação
 │     ├── Nac. Projetado Base
 │     └── Nac. Projetado Comparacao
 ├── 📁 Titulos       → medidas usadas em cartões e títulos dinâmicos
 │     ├── Variação % Frete
 │     └── Variação % Nacionalização
 └── Checagem Soma dos Efeitos
```

**Boas práticas aplicadas:**

- **Separação por responsabilidade:** medidas de base, comparação, efeito e títulos ficam em pastas distintas, o que evita misturar cálculo com apresentação.
- **Reutilização:** as medidas de comparação e de efeito são construídas a partir das medidas base, evitando lógica duplicada. Se uma regra muda, ela é corrigida em um único lugar.
- **Medida de validação:** a medida `Checagem Soma dos Efeitos` confere se a soma dos efeitos individuais (ICMS, I.I., frete etc.) fecha com a variação total do índice. Assim o gráfico de cascata sempre reconcilia com o número exibido nos KPIs.
- **Nomenclatura padronizada:** nomes claros e consistentes, que indicam o que a medida calcula e a qual período ela pertence.

---

## Tratamento e qualidade dos dados

Antes de chegar ao dashboard, os dados passaram por uma etapa de preparação no **Power Query**:

- **Limpeza:** remoção de duplicidades, linhas em branco e registros inconsistentes.
- **Padronização:** tipos de dados corrigidos (datas, valores monetários, textos), nomes de colunas padronizados e categorias unificadas (por exemplo, grupos de estoque e fornecedores com grafias diferentes).
- **Tratamento de nulos:** valores ausentes tratados para não distorcer os índices e os totais.
- **Modelagem:** relacionamentos entre as tabelas de fatos e dimensões (calendário, fornecedor, item, grupo de estoque), garantindo que os filtros globais funcionem em todas as páginas.
- **Conferência de totais:** os valores consolidados do dashboard foram conferidos contra a base de origem para garantir consistência.
- **Anonimização:** fornecedores, clientes e itens foram substituídos por nomes genéricos antes da publicação, sem alterar os valores nem a lógica dos cálculos.

---

## Tecnologias e conceitos aplicados

- Power BI (modelagem de dados, medidas DAX e visualizações interativas)
- Power Query (ETL, limpeza e transformação de dados)
- Organização de medidas em pastas e tabela dedicada
- Medidas de validação para garantir a reconciliação dos números
- Análise comparativa entre períodos (YoY)
- Gráficos de cascata, rosca, colunas empilhadas e tabelas dinâmicas
- Segmentações (slicers) sincronizadas entre páginas
- Anonimização de dados sensíveis para uso em portfólio
