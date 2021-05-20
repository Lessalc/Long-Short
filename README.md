# Long Short 

## Long & Short por Cointegração

O objetivo desses códigos é gerar um banco de dados que pode ser usado em planilha do Excel para encontrar oportunidades de Long e Short por cointegração.

Os códigos obtem dados direto do Yahoo Finance, por meio de uma biblioteca já existente.

## COINTEGRAÇÃO EM POUCAS LINHAS
- A partir de duas séries temporais de preços de ativos é feita uma regressão linear.
- Com a regressão linear são calculados os Y previstos (preços do ativo dependente).
- Com os valores previsto e os reais, teremos a série de resíduos no tempo.
- Um par é dito cointegrado, quando a série resíduos é estacionária, ou seja, retorna a um valor fixo, sem que haja uma tendência
- Faz-se, então um teste de hipotese típico, a partir do teste de Dick Fuller. Usamos o Augmented Dickey-Fuller (teste ADF).
- No teste de hipotese tentaremos rejeitar a hipótese nula (H0). A H0 definida pelo teste é de que a serie SEGUE UMA TENDÊNCIA (em série temporal é dito que a série possui raíz unitária), assim, se o valor da estatística retornada no teste for menor do que a crítica, para um determinado nível de confiança, dizemos que a hipótese nula é rejeitada e que a série é estacionária.

## Os Arquivos - Siga essa ordem para entender.
### Long&Short - Cointegração Yahoo.ipynp
Esse é o código mais elaborado, utilizei a API da Yahoo para fazer o download dos dados. Mostrei printando em tela como é feita a classificação da cointegração de dois ativos pelo teste ADF.
Foi elaborado gráfico em períodos diferentes para exemplificar. Gráficos de preço ao longo do tempo, spread dos ativos, regressão linear, distribuição residual e por fim o mais importante a série temporal dos resíduos.
Foi feito também um heatmap de 15 ativos aleatórios, onde era retornado o valor estatístico encontrado pelo teste. Esse é o valor usado na comparação com o crítico para então classificar como Cointegrado ou não.

### LS_ClassPares.ipynp
O objetivo desse código é testar a cointegração em determinados períodos.
Utilizando o API do Yahoo foram baixadas informações sobre 79 ativos pertencentes ao IBOV.
Os ativos são então comparados 2 por vez para testar a cointegração em todos os períodos definidos, a partir de uma regressão linear de preços de fechamento dos ativos, sendo um ativo independente (X) e um dependente (Y).
Será feito o teste ADF (Augmented Dickey-Fuller) para a série de resíduos de cada um dos períodos, aqueles que obtiverem estatística de ADF menor que o valor crítico de 5%, 
ou seja, aqueles que negarem a hipótese nula que o resíduo segue uma tendência, será classificado como cointegrado.
Em um novo Dataframe criado são adicionados o ativo independente, o dependente e o valor estatístico do ADF nos períodos cointegrados. 
No fim, será gerado um arquivo CSV, com o novo Dataframe
O arquivo cvs será usado em outro programa para retornar estatísticas completas.

### LS_Residuos.ipynp (Necessita antes dos CSV gerado pelo LS_ClassPares)
O objetivo desse código é a partir de uma combinação de pares cointegrados gerar um CSV com as suas estatísticas.
Abre o csv gerado pelo LS_ClassPares.ipynp que retornou a lista de pares cointegrados em pelo menos 5 períodos. 
Agora iremos retornar as estatísticas principais desses pares cointegrados, que são:
    - Coeficiente Angular da regressão
    - Intesecção da Regresão
    - Média e Desvio padrão dos resíduos
    - Nivel de confiança do ADF (95% ou 99%)

No fim teremos um csv que poderá ser usado como banco de dados para um arquivo em Excel
A ideia de um Long & Short é usar entradas em 2 Desvios padrão do resíduo, os dados retornados aqui são suficiente para montar avisos em Planilhas de Excel.
A inclinação da curva de regressão foi dada para que se possa decidir entre as estratégias de Cash Neutro ou usando valores proporcionais a inclinação da curva. 

### LS_PrecosBD.ipynp 
Gera um banco de dados com os preços nos 250 últimos períodos. Usado para na planilha poder gerar os gráficos

### Planilha (Necessita do CSV gerado pelo LS_ClassPares e do CSV gerado pelo LS_PRECOSBD)
A Planilha é simples e foi incluida para se ter uma ideia do que se pode fazer. Era irá captar dados do Tryd, para usar no Profit devem ser feitas alterações.

### Matplotlib - Tutorial 
Criei esse tutorial para quem quiser entender como manipular gráficos em Python. É bem completo e orientado a objeto, que é a melhor forma de manipular.

### Os arquivos em CSV foram gerados pelo arquivos acima e o TXT é um aquivo também em csv que contém os ativos da bolsa para poder importar os dados do Yahoo Finance.
