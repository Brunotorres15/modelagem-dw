# Modelagem de um DW para a empresa fictícia BT Fashion

### 🤔 Por que a BT Fashion Precisaria de um Data Warehouse?

*BT Fashion, uma loja de roupas estabelecida tanto fisicamente quanto online, enfrenta desafios e oportunidades típicas do setor varejista moderno. Com um portfólio diversificado que inclui roupas masculinas, femininas, infantis e acessórios, a empresa está constantemente buscando maneiras de melhorar suas operações e impulsionar seu crescimento no competitivo mercado de moda.*

### 💵 **Desafios e Oportunidades:**

**Análise de Vendas Multicanal:** Com operações tanto em lojas físicas quanto online, a BT Fashion precisa integrar e analisar dados de vendas de diferentes canais para entender o desempenho de cada um e otimizar estratégias de vendas.

**Gestão de Estoque e Produtividade:** Para evitar estoques excedentes ou insuficientes, a BT Fashion precisa de insights detalhados sobre o desempenho de vendas de cada produto, ajudando na previsão de demanda e na gestão de inventário.

**Segmentação de Clientes e Comportamento de Compra:** Entender o comportamento dos clientes é crucial para oferecer produtos relevantes e personalizados. Isso inclui análises sobre frequência de compra, preferências por categoria de produtos e resposta a promoções.

**Eficiência Operacional:** Avaliar a eficácia das campanhas promocionais, métodos de entrega e otimização de custos de envio são essenciais para melhorar a eficiência operacional e reduzir custos.

**Planejamento Estratégico e Decisões Baseadas em Dados:** Para tomar decisões estratégicas informadas, como expansão de linha de produtos, abertura de novas lojas ou lançamento de campanhas promocionais, a BT Fashion precisa de uma visão clara e consolidada de seus dados de vendas e operações.

### 💡 Benefícios de um Data Warehouse:

Implementar um Data Warehouse permitirá à BT Fashion:

**Consolidação de Dados:** Integrar dados de vendas de diferentes fontes para uma visão unificada e consistente.

**Análise Preditiva e Descritiva:** Capacidade de prever tendências de vendas, identificar padrões de compra e entender o impacto de variáveis como campanhas promocionais.

**Relatórios e Dashboards:** Criar relatórios e dashboards personalizados para diferentes stakeholders, permitindo uma análise rápida e eficiente.

**Suporte à Decisão Estratégica:** Fornecer insights acionáveis para apoiar decisões estratégicas, como alocação de recursos, estratégias de marketing e melhorias operacionais.

# 🔍 Cenário de Negócio

A empresa deseja analisar as vendas online para tomar decisões estratégicas, como identificar tendências de compra, avaliar o desempenho de diferentes categorias de produtos e otimizar as estratégias de marketing.

**Objetivos Analíticos:**

- Avaliar o desempenho das vendas ao longo do tempo (diário, mensal, anual).
- Analisar as vendas por categoria de produto e subcategoria.
- Identificar os produtos mais vendidos e menos vendidos.
- Analisar o comportamento do cliente, como frequência de compra e valor médio do pedido.
- Avaliar o impacto de campanhas promocionais nas vendas.
- Identificar padrões geográficos nas vendas online.

**Dados Que Estão Disponíveis:**

- Informações de pedidos: data do pedido, valor total, status do pedido, descontos aplicados.
- Detalhes do cliente: identificação do cliente, localização, histórico de compras.
- Detalhes do produto: identificação do produto, categoria, subcategoria, preço.
- Informações de envio: custo de envio, prazo de entrega, método de envio.
- Dados de campanhas promocionais: período da campanha, desconto oferecido, produtos incluídos.


# Modelo Conceitual

##### Tendo como base o cenário acima, cheguei no seguinte modelo conceitual e regras de negócio #####

##### Entidades #####

- Clientes
- Produtos
- Promoção
- Entrega
- Tempo
- Vendas

##### Atributos #####

- Cliente

ID_Cliente (Chave Primária): Identificador único para cada cliente.
Localizacao: Localização do cliente.

- Produto

ID_Produto (Chave Primária): Identificador único para cada produto.
Categoria: Categoria do produto (ou seja: masculino, feminino, infantil).
Subcategoria: Subcategoria do produto (ou seja: camisetas, calças).
Preco: Preço unitário do produto.

- Promoção

ID_Promocao (Chave Primária): Identificador único para cada campanha promocional.
Periodo_Campanha: Duração da campanha promocional (em dias).
Desconto_Oferecido: Desconto oferecido na campanha.

- Entrega

ID_Entrega (Chave Primária): Identificador único para as informações de entrega.
Custo_Envio: Custo associado à entrega do produto.
Prazo_Entrega: Prazo estimado para entrega.
Metodo_Entrega: Método de entrega utilizado (ou seja: correio normal, expresso).

- Tempo

ID_Tempo (Chave Primária): Identificador único para cada data.
Data: A data do pedido.
Mes: Mês do pedido.
Ano: Ano do pedido.
Dia_Semana: Dia da semana do pedido.

- Vendas

ID_Venda (Chave Primária): Identificador único para cada transação de venda.
ID_Cliente: Chave estrangeira ligada à entidade Cliente.
ID_Produto: Chave estrangeira ligada à entidade Produto.
ID_Tempo: Chave estrangeira ligada à entidade Tempo.
ID_Promocao: Chave estrangeira ligada à entidade Promoção.
ID_Entrega: Chave estrangeira ligada à entidade Envio.
Valor_Total: O valor total da venda após aplicados os descontos.
Desconto_Aplicado: Valor do desconto aplicado na venda.
Quantidade: Número de unidades do produto vendidos na transação.

#### Relacionamentos ####

Cada Venda está associada a um Cliente.

Um Cliente pode estar associado a várias Vendas.

Cada Venda está associada a um Produto.

Um Produto pode estar associada a várias Vendas.

Cada Venda está associada a um tipo de Entrega.

Um tipo de Entrega pode estar associado a várias 
Vendas.

Cada Venda está associada a uma Promoção.

Uma Promoção pode estar associada a várias Vendas.

Cada Venda está associada a uma Data.

Uma Data pode estar associada a várias Vendas.



# Modelo Dimensional

Baseado no cenário fornecido, podemos criar o seguinte modelo dimensional em um esquema estrela (star schema, posteriormente será explicado o motivo do porque eu escolhi este tipo de modelagem):

### Tabela Fato: ###

**Vendas**

Chaves Estrangeiras: ID_Cliente, ID_Produto, ID_Tempo, ID_Promocao, ID_Entrega
Atributos de Medida: Valor_Total, Desconto_Aplicado, Quantidade

### Tabelas de Dimensão: ###

**Cliente**

Atributos: ID_Cliente (Chave Primária), Localizacao

**Produto**

Atributos: ID_Produto (Chave Primária), Categoria, Subcategoria, Preco

**Promoção**

Atributos: ID_Promocao (Chave Primária), Periodo_Campanha, Desconto_Oferecido

**Entrega**

Atributos: ID_Entrega (Chave Primária), Custo_Envio, Prazo_Entrega, Metodo_Entrega

**Tempo**

Atributos: ID_Tempo (Chave Primária), Data, Mes, Ano, Dia_Semana

### Relacionamentos: ###

Vendas-Cliente: Cada venda está associada a um cliente. 

Um cliente pode estar associado a várias vendas.

Vendas-Produto: Cada venda está associada a um produto.

Um produto pode estar associado a várias vendas.

Vendas-Tempo: Cada venda está associada a uma data. 

Uma data pode estar associada a várias vendas.

Vendas-Promoção: Cada venda está associada a uma promoção.

Uma promoção pode estar associada a várias vendas.

Vendas-Entrega: Cada venda está associada a um tipo de entrega.

Um tipo de entrega pode estar associado a várias vendas.


*Este modelo dimensional permite analisar as vendas a partir de diferentes dimensões, como cliente, produto, tempo, promoção e entrega. Isso facilita a realização de consultas analíticas e a geração de insights sobre o desempenho das vendas, comportamento do cliente, eficácia das promoções e eficiência das entregas.*

# Modelo Lógico #

### Tabelas de Dimensão: ###

**Cliente**

ID_Cliente (INT, Chave Primária)
Localizacao (VARCHAR)

**Produto**

ID_Produto (INT, Chave Primária)
Categoria (VARCHAR)
Subcategoria (VARCHAR)
Preco (DECIMAL)

**Promoção**

ID_Promocao (INT, Chave Primária)
Periodo_Campanha (INT)
Desconto_Oferecido (DECIMAL)

**Entrega**

ID_Entrega (INT, Chave Primária)
Custo_Envio (DECIMAL)
Prazo_Entrega (INT)
Metodo_Entrega (VARCHAR)

**Tempo**

ID_Tempo (INT, Chave Primária)
Data (DATE)
Mes (INT)
Ano (INT)
Dia_Semana (VARCHAR)

### Tabela Fato: ###

**Vendas**

ID_Venda (INT, Chave Primária)

ID_Cliente (INT, Chave Estrangeira, Referencia Cliente(ID_Cliente))

ID_Produto (INT, Chave Estrangeira, Referencia Produto(ID_Produto))

ID_Tempo (INT, Chave Estrangeira, Referencia Tempo(ID_Tempo))

ID_Promocao (INT, Chave Estrangeira, Referencia Promoção(ID_Promocao))

ID_Entrega (INT, Chave Estrangeira, Referencia Entrega(ID_Entrega))

Valor_Total (DECIMAL)

Desconto_Aplicado (DECIMAL)

Quantidade (INT)

# Modelo Físico

Obs: A Sintaxe deste modelo físico foi feita para um banco de dados PostgreSQL

```
CREATE TABLE Cliente (
    ID_Cliente SERIAL PRIMARY KEY,
    Localizacao VARCHAR(255)
);

CREATE TABLE Produto (
    ID_Produto SERIAL PRIMARY KEY,
    Categoria VARCHAR(255),
    Subcategoria VARCHAR(255),
    Preco DECIMAL(10, 2)
);

CREATE TABLE Promocao (
    ID_Promocao SERIAL PRIMARY KEY,
    Periodo_Campanha INT,
    Desconto_Oferecido DECIMAL(10, 2)
);

CREATE TABLE Entrega (
    ID_Entrega SERIAL PRIMARY KEY,
    Custo_Envio DECIMAL(10, 2),
    Prazo_Entrega INT,
    Metodo_Entrega VARCHAR(255)
);

CREATE TABLE Tempo (
    ID_Tempo SERIAL PRIMARY KEY,
    Data DATE,
    Mes INT,
    Ano INT,
    Dia_Semana VARCHAR(255)
);

CREATE TABLE Vendas (
    ID_Venda SERIAL PRIMARY KEY,
    ID_Cliente INT REFERENCES Cliente(ID_Cliente),
    ID_Produto INT REFERENCES Produto(ID_Produto),
    ID_Tempo INT REFERENCES Tempo(ID_Tempo),
    ID_Promocao INT REFERENCES Promocao(ID_Promocao),
    ID_Entrega INT REFERENCES Entrega(ID_Entrega),
    Valor_Total DECIMAL(10, 2),
    Desconto_Aplicado DECIMAL(10, 2),
    Quantidade INT
);
```

# Porque foi utilizada a modelagem Star Schema?

*O star schema é uma abordagem comum na modelagem de data warehouses devido às suas várias vantagens, especialmente em cenários de análise de dados como o da BT Fashion. Aqui estão alguns motivos específicos para optar por um star schema:*

### Simplicidade e Intuitividade

**Estrutura Simples:** Um star schema possui uma estrutura clara e fácil de entender, com uma única tabela de fatos no centro conectada diretamente a várias tabelas de dimensões. Isso facilita a compreensão e o uso do modelo por analistas de negócios e usuários finais.

**Relacionamentos Diretos:** As relações diretas entre a tabela de fatos e as tabelas de dimensões simplificam a navegação e a escrita de consultas SQL, tornando a extração de dados mais rápida e eficiente.

**Desempenho de Consulta:**
Otimização de Consultas: As consultas em um star schema tendem a ser mais eficientes porque envolvem menos junções complexas. As tabelas de dimensões são normalmente pequenas e podem ser facilmente indexadas, o que acelera o tempo de resposta das consultas.

**Desempenho Aprimorado:** Devido à sua estrutura denormalizada, um star schema reduz a quantidade de junções necessárias para recuperar dados, o que resulta em um desempenho de consulta melhorado em comparação com esquemas mais complexos.

### Flexibilidade Analítica

**Capacidade de Consulta Ad Hoc:** O star schema permite a realização de consultas ad hoc e análise de dados de maneira mais flexível, permitindo aos usuários explorar os dados de diferentes ângulos e perspectivas sem a necessidade de conhecimento profundo da estrutura do banco de dados.

**Agregação de Dados:** Facilita a agregação de dados e a criação de relatórios resumidos, como total de vendas por categoria de produto, vendas por período, análise de desempenho de promoções, etc.

### Manutenção e Escalabilidade

**Facilidade de Manutenção:** A simplicidade estrutural do star schema torna a manutenção do data warehouse mais fácil, pois as alterações podem ser implementadas com menor impacto nas consultas e na integridade dos dados.

**Escalabilidade:** É mais fácil adicionar novas dimensões e fatos ao modelo conforme a necessidade de análise evolui, permitindo a escalabilidade do data warehouse.

### Integração com Ferramentas de BI

**Compatibilidade com Ferramentas de BI:** Muitas ferramentas de Business Intelligence (BI) são otimizadas para trabalhar com star schemas, facilitando a criação de dashboards, relatórios e análises interativas.

**Utilizando um star schema, a BT Fashion pode obter uma visão clara e abrangente de seus dados de vendas, facilitando a geração de insights valiosos para a tomada de decisões estratégicas e operacionais.**