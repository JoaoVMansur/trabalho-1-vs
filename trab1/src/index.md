
# Trabalho 1: Visualização de Dados

Neste trabalho, vamos analisar o conjunto de dados das músicas mais tocadas em 2023 no aplicativo Spotify. Para este trabalho, iremos responder às seguintes três perguntas feitas pelo professor:

1. Existe alguma característica que faz uma música ter mais chances de se tornar popular?
2. O conjunto das top 10 músicas e dos top 10 artistas varia muito se considerarmos apenas músicas lançadas no mesmo ano?
3. Discuta as diferenças entre as plataformas (Spotify, Deezer, Apple Music e Shazam).

# Data Set:

```js
const dataSet = await FileAttachment("./data/spotify-2023.csv").csv({typed: true});

  dataSet.sort((a,b) => {
   
      return b.streams - a.streams
    
  })
view(Inputs.table(dataSet));

```
# Observações

É relevante observar que o Spotify afirma possuir mais de 100 milhões de músicas disponíveis para os usuários em seu aplicativo. No conjunto de dados analisado, encontram-se as 949 músicas mais ouvidas. Portanto, ao examinar os dados deste conjunto, devemos considerar que são, de longe, as músicas mais populares do aplicativo. 


