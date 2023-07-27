# cryptonews_price_correlation 2019-2023
 Notebook em Python com storytelling sobre a correlação das notícias e o impacto no preço do ativo.

 Dataset: https://www.kaggle.com/datasets/larysa21/cryptonews-2019-2023

### Importar as bibliotecas necessárias:
- Pandas para manipulação dos dados.
- Matplotlib e Seaborn para visualização dos gráficos.
- yfinance para obter os dados dos ativos.
- Talvez outras bibliotecas específicas para análise de texto, se necessário.

        import pandas as pd
        import yfinance as yf
        import seaborn as sns
        import matplotlib.pyplot as plt

### Carregar o arquivo 'cryptonews_dataset.csv' em um DataFrame do Pandas.

        import pandas as pd
        
        # Dados de exemplo
        data = {
            'cryptocurrency': ['btc'],
            'url': ['https://ourbitcoinnews.com/warner-music-group-partners-with-polygon-%e2%94%80%e2%94%80-music-acceler...'],
            'title': ['Warner Music Group Partners with Polygon ── Music Accelerator Program Launched | CoinDesk JAPAN | Co...'],
            'text': ['Global entertainment company Warner Music Group (WMG) has partnered with Polygon Labs to bring next-...'],
            'date': ['2023-07-01 00:12:04+00:00'],
            'predicted_labels': [0],
            'sentiment': ['Neutral'],
            'polarity': [0.0],
            'subjectivity': [0.0]
        }
        
        # Criando o DataFrame
        df = pd.DataFrame(data)
        
        # Exibindo o DataFrame
        print(df)


### Pré-processar os dados, se necessário:
- Verificar se os tipos de dados estão corretos.
- Tratar dados ausentes, se houver.
- Converter a coluna de data para o formato correto.

        # Verificar os tipos de dados atuais
        print(df.dtypes)
        
        # Tratamento de dados ausentes (opcional)
        # Caso haja dados ausentes, você pode optar por preenchê-los ou removê-los, dependendo da situação.
        # Exemplo de preenchimento com zero para colunas numéricas:
        df.fillna(0, inplace=True)
        
        # Converter a coluna de data para o formato correto
        df['date'] = pd.to_datetime(df['date'], utc=True)
        
        # Verificar se a conversão foi realizada com sucesso
        print(df['date'].dtype)


### Filtrar os dados:
- Selecionar apenas as notícias de 2019 até 2023.
- Criamos uma nova variável chamada filtered_df, que armazenará apenas as notícias que estão dentro do intervalo de datas de 2019 até 2023, conforme definido pelas variáveis start_date e end_date.

- Agora, filtered_df conterá apenas as notícias que você deseja analisar no período especificado. A partir desse DataFrame filtrado, você pode prosseguir com a coleta dos dados de preços dos ativos e realizar a análise de correlação entre as notícias e os preços dos ativos no período selecionado.

        # Assumindo que o DataFrame já foi carregado e pré-processado anteriormente, vamos filtrar as notícias
        
        # Converter a coluna "date" para o formato de data
        df['date'] = pd.to_datetime(df['date'], utc=True)
        
        # Definir o intervalo de datas desejado
        start_date = '2019-01-01'
        end_date = '2023-12-31'
        
        # Aplicar a condição de filtro para selecionar apenas as notícias no período desejado
        filtered_df = df[(df['date'] >= start_date) & (df['date'] <= end_date)]
        
        # Exibir o DataFrame filtrado
        print(filtered_df)


### Coletar os dados do preço histórico dos ativos mencionados nas notícias:
- Utilizar a API yfinance para obter os preços dos ativos durante o mesmo período das notícias.
- Neste exemplo, utilizamos a função yf.download da biblioteca yfinance para coletar os preços históricos das criptomoedas mencionadas nas notícias durante o mesmo período das notícias. Os dados dos preços são armazenados em um dicionário chamado price_data, onde cada chave é o símbolo da criptomoeda e cada valor é um DataFrame contendo os preços históricos.

        # DataFrame original contendo as notícias filtradas
        filtered_df = pd.read_csv('cryptonews_dataset.csv')  # Carregue o arquivo CSV corretamente
        
        # Converter a coluna "date" para o formato de data (caso ainda não tenha sido feito)
        filtered_df['date'] = pd.to_datetime(filtered_df['date'], utc=True)
        
        # Lista para armazenar os símbolos das criptomoedas mencionadas nas notícias
        cryptocurrencies = filtered_df['cryptocurrency'].unique()
        
        # Dicionário para armazenar os DataFrames de preços dos ativos
        price_data = {}
        
        # Intervalo de datas para a coleta de preços
        start_date = '2019-01-01'
        end_date = '2023-12-31'
        
        # Coletar os preços dos ativos mencionados nas notícias
        for cryptocurrency in cryptocurrencies:
            ticker = f'{cryptocurrency}-USD'
            try:
                df_prices = yf.download(ticker, start=start_date, end=end_date)
                price_data[cryptocurrency] = df_prices
            except Exception as e:
                print(f"Erro ao obter dados para {cryptocurrency}: {e}")
        
        # Exibir os DataFrames de preços coletados
        for cryptocurrency, df_prices in price_data.items():
            print(f"Preços para {cryptocurrency}:")
            print(df_prices)

  
### Calcular a correlação entre as notícias e os preços dos ativos:
- Coeficiente de correlação de Pearson ou Spearman para medir a relação entre as duas variáveis.
- Usamos o método corr() do DataFrame para calcular os coeficientes de correlação de Pearson e Spearman entre as notícias e os preços dos ativos. O resultado será uma matriz de correlação mostrando a relação entre as variáveis.

- Observe que os valores de correlação podem variar de -1 a 1, e valores mais próximos de 1 ou -1 indicam uma forte correlação, enquanto valores próximos de 0 indicam pouca ou nenhuma correlação.

        # Vamos supor que você já coletou os dados das notícias e dos preços dos ativos e armazenou-os em DataFrames
        # filtered_df: DataFrame com as notícias filtradas (conforme feito anteriormente)
        # price_data: Dicionário de DataFrames com os preços dos ativos (conforme coletado anteriormente)
        
        # Juntar as informações das notícias e dos preços com base na coluna "cryptocurrency" e "date"
        merged_df = filtered_df.merge(pd.concat(price_data, names=['cryptocurrency', 'date']), 
                                      on=['cryptocurrency', 'date'])
        
        # Calcular o coeficiente de correlação de Pearson
        correlation_pearson = merged_df.corr(method='pearson')
        
        # Calcular o coeficiente de correlação de Spearman
        correlation_spearman = merged_df.corr(method='spearman')
        
        # Exibir os resultados
        print("Correlação de Pearson:")
        print(correlation_pearson)
        
        print("\nCorrelação de Spearman:")
        print(correlation_spearman)


### Visualizar os resultados:
- Criar gráficos para mostrar a correlação ao longo do tempo.
- Identificar quais notícias tiveram maior impacto no mercado.
- U tilizamos a biblioteca matplotlib para criar os gráficos de linhas para visualizar a variação dos preços dos ativos ao longo do tempo. Cada linha do gráfico representa um ativo específico, e as datas estão no eixo x, enquanto os preços de fechamento estão no eixo y.

- Em seguida, usamos a biblioteca seaborn para criar mapas de calor que mostram a matriz de correlação de Pearson e de Spearman entre as notícias e os preços dos ativos. Os mapas de calor ajudam a visualizar a correlação entre as variáveis, onde cores mais quentes representam uma correlação positiva e cores mais frias representam uma correlação negativa.

- Para identificar quais notícias tiveram maior impacto no mercado, você pode procurar por períodos de alta correlação entre notícias específicas e preços dos ativos. Se houver um aumento acentuado na correlação entre notícias e preços em um determinado período, isso pode indicar que essas notícias tiveram um impacto significativo no mercado.

        # Vamos supor que você já possui o DataFrame merged_df com as informações das notícias e dos preços dos ativos
        
        # Gráfico de linhas para visualizar a variação dos preços dos ativos ao longo do tempo
        plt.figure(figsize=(12, 6))
        for cryptocurrency in price_data.keys():
            plt.plot(merged_df[merged_df['cryptocurrency'] == cryptocurrency]['date'],
                     merged_df[merged_df['cryptocurrency'] == cryptocurrency]['Close'],
                     label=cryptocurrency)
        
        plt.xlabel('Data')
        plt.ylabel('Preço de Fechamento')
        plt.title('Variação dos Preços dos Ativos ao Longo do Tempo')
        plt.legend()
        plt.grid(True)
        plt.show()
        
        # Matriz de correlação de Pearson
        plt.figure(figsize=(10, 8))
        sns.heatmap(merged_df.corr(method='pearson'), annot=True, cmap='coolwarm', fmt='.2f')
        plt.title('Matriz de Correlação de Pearson entre Notícias e Preços dos Ativos')
        plt.show()
        
        # Matriz de correlação de Spearman
        plt.figure(figsize=(10, 8))
        sns.heatmap(merged_df.corr(method='spearman'), annot=True, cmap='coolwarm', fmt='.2f')
        plt.title('Matriz de Correlação de Spearman entre Notícias e Preços dos Ativos')
        plt.show()

#### Lembre-se de que a interpretação dos resultados depende da análise específica de seu conjunto de dados e de outros fatores relevantes para o mercado de criptomoedas. Mas você pode continuar à partir daqui.

Veja também: https://github.com/r4rafael/bitcoin-correlation
