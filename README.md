# cryptonews_price_correlation 2019-2023
 Notebook em Python com storytelling sobre a correlação das notícias e o impacto no preço do ativo.

 Dataset: https://www.kaggle.com/datasets/larysa21/cryptonews-2019-2023

### Importar as bibliotecas necessárias:

Pandas para manipulação dos dados.
Matplotlib e Seaborn para visualização dos gráficos.
yfinance para obter os dados dos ativos.

### Talvez outras bibliotecas específicas para análise de texto, se necessário.
Carregar o arquivo 'cryptonews_dataset.csv' em um DataFrame do Pandas.

### Pré-processar os dados, se necessário:
- Verificar se os tipos de dados estão corretos.
- Tratar dados ausentes, se houver.
- Converter a coluna de data para o formato correto.

### Filtrar os dados:
- Selecionar apenas as notícias de 2019 até 2023.

### Coletar os dados do preço histórico dos ativos mencionados nas notícias:
- Utilizar a API yfinance para obter os preços dos ativos durante o mesmo período das notícias.

### Calcular a correlação entre as notícias e os preços dos ativos:
- Coeficiente de correlação de Pearson ou Spearman para medir a relação entre as duas variáveis.

### Visualizar os resultados:
- Criar gráficos para mostrar a correlação ao longo do tempo.
- Identificar quais notícias tiveram maior impacto no mercado.

### Criar storytelling no notebook:
- Utilize markdown cells para explicar os insights encontrados.
- Adicione visualizações e gráficos para suportar suas análises.
