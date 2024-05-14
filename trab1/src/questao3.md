#  Questão 3: Discuta as diferenças entre as plataformas (Spotify, Deezer, Apple Music e Shazam).
 
  <br>
  <br>
  <br>

```js
const dataSet = await FileAttachment("./data/spotify-2023.csv").csv({typed: true});

  dataSet.sort((a,b) => {
   
      return b.streams - a.streams
    
  })


  ```
# Análise 1: Total de Playlists em Cada Plataforma

Em uma primeira análise, somamos o número total de playlists nas quais cada música foi adicionada nas três plataformas distintas: Spotify, Apple e Deezer, com o intuito de aobservar o número total de playlists que cada plataforma contem. É relevante ressaltar que o Shazam não fornece dados de playlists, apenas de Charts, portanto, essa análise inicial não inclui o Shazam em sua avaliação.

<div class="grid grid-cols-2">
    <div id="totalPlaylist" class="card grid-colspan-2">
        <h2 class="title">Quantidade de Playlists em Cada Plataforma</h2>
        <div style="width: 100%; margin-top: 15px;">
            ${vl.render(totalPlaylist(divWidth - 200))}
        </div>
    </div>
</div>

A predominância avassaladora de playlists no Spotify é notável. Essa tendência levanta duas possibilidades interpretativas: o Spotify é, de fato, a plataforma de streaming musical mais popular entre as três analisadas, ou essa prevalência é um reflexo direto do conjunto de dados específico utilizado neste estudo, intitulado "Most Streamed Spotify Songs 2023". Pessoalmente, inclino-me a considerar a segunda possibilidade como a mais plausível. Embora o Spotify seja inegavelmente a plataforma de streaming de música mais reconhecida e amplamente utilizada, a discrepância apresentada pelos dados parece exagerada e possivelmente distorcida pela natureza do conjunto de dados.

Além disso, quando analisamos os dados da Apple, observamos que, embora seja menos popular que o Spotify, a diferença de popularidade entre as duas plataformas não parece tão exagerada quanto o retratado nos dados. Isso sugere que, enquanto o Spotify continua a ser líder em termos de criação de playlists, a posição relativa das outras plataformas pode estar sub-representada ou mal interpretada no conjunto de dados atual. Portanto, é importante abordar essa disparidade com cautela e considerar o contexto mais amplo ao interpretar os resultados desta análise.




# Análise 2: Posição no charts de cada plataforma.

Para esta análise, separamos o top 15 músicas mais tocadas em 2023 e retiramos apenas as informações como nome da música, ranking dela no top 15 e a posição dela no chart de cada plataforma

```js
view(Inputs.table(allChart));
```
Platando o gráfico para encontrarmos alguma caracterisca especifica de cada plataforma.

<div class="grid grid-cols-2">
    <div id="allCharts" class="card grid-colspan-2">
        <h2 class="title">Posição no Chart de Cada Plataforma</h2>
        <div style="width: 100%; margin-top: 15px;">
            ${vl.render(allCharts(divWidth - 200))}
        </div>
    </div>
</div>

Com base nos dados apresentados, inicialmente, é evidente que o status de ser a música mais ouvida do ano de 2023 não assegura automaticamente a posição de número 1 nos charts das plataformas de streaming, como exemplificado pela situação de "Blinding Lights", que, apesar de ter sido a música mais ouvida em 2023, ocupa a modesta posição de 199º no chart da Apple Music. Isso nos leva a considerar que cada plataforma possivelmente emprega métodos distintos para classificar as músicas em seus rankings, como indicado pela discrepância notável entre as posições de cada música. Essa diversidade de abordagens entre as plataformas sugere uma complexidade significativa no processo de ranqueamento, que pode ser influenciado por uma variedade de fatores, como engajamento do usuário, algoritmos de recomendação e tendências de mercado. 

<br>
<br>
<br>

Ao analisar os dados de posicionamento nos charts, observamos que apenas a plataforma Shazam apresenta valores nulos nessa categoria

<div class="grid grid-cols-2">
    <div id="nullValuesChart" class="card grid-colspan-2">
        <h2 class="title">Valores Nulos Para os Charts em Cada Plataforma</h2>
        <div style="width: 100%; margin-top: 15px;">
            ${vl.render(nullValuesChart(divWidth - 200))}
        </div>
    </div>
</div>

Esse padrão intrigante, evidenciado pelo gráfico, sugere uma variedade de possíveis explicações, porém, especulamos que esteja relacionado ao mesmo problema discutido na análise anterior. Dado que este conjunto de dados se concentra no Spotify, é plausível que as informações referentes a outras plataformas não sejam tão confiáveis. Esta discrepância pode surgir de diferenças na metodologia de coleta de dados entre as plataformas, inconsistências na disponibilidade ou precisão das informações fornecidas ou até mesmo variações na integridade dos conjuntos de dados utilizados. Considerando isso, é crucial proceder com cautela ao interpretar os dados e considerar as limitações inerentes à sua fonte e qualidade.


# Desings utilizados

Nesta pergunta, empregamos dois tipos de gráficos: o gráfico de barras compactado e o gráfico de pizza

 Para a primeira análise, optamos pelo gráfico de pizza, pois nosso objetivo era ilustrar as partes de um todo, evidenciando a importância de cada plataforma de streaming representada. Quando buscamos representar partes de um todo, o gráfico de pizza é a escolha mais apropriada.
 Para os canais visuais, o gráfico de pizza apresenta um círculo dividido em fatias, cada uma representando a proporção de uma plataforma de streaming. Já os marcadores consistem em três cores distintas, cada uma correspondendo a uma plataforma de streaming específica. Essas cores foram selecionadas com base nas paletas de cores das respectivas logos de cada plataforma.


Para a segunda análise, utilizamos dois tipos de gráficos: o gráfico de barras compactadas horizontais e o gráfico de pizza, novamente. Optamos pelo gráfico de barras compactadas horizontais para comparar as posições de cada música nos charts de cada plataforma de forma clara e direta, permitindo visualizar as diferenças entre os charts das plataformas. O gráfico de pizza foi novamente empregado, desta vez para demonstrar a participação de cada plataforma em relação aos valores nulos presentes no conjunto de dados sobre a posição nos charts das plataformas. Para os marcadores, cada música e sua posição no chart das plataformas foram representadas por barras verticais, sendo que cada plataforma possui sua própria barra vertical específica. Quanto aos canais visuais, as barras verticais de cada plataforma possuem cores diferentes, escolhidas como na análise anterior, de acordo com a paleta de cores da plataforma. O gráfico de barras segue o mesmo conceito descrito anteriormente









```js

import * as vega from "npm:vega";
import * as vegaLite from "npm:vega-lite";
import * as vegaLiteApi from "npm:vega-lite-api";

const vl = vegaLiteApi.register(vega, vegaLite);
  
let plataformas = {
    spotify : 0,
    apple : 0,
    deezer :0
}
dataSet.forEach(musica => {
    plataformas.spotify += parseInt(musica.in_spotify_playlists)
   plataformas.apple += parseInt(musica.in_apple_playlists)
   plataformas.deezer += parseInt(musica.in_deezer_playlists)
})




function totalPlaylist(divWidth) {
    return {
        spec: {
            width: divWidth,
            data: {
                values: [
                    { category: "spotify", value: plataformas.spotify },
                    { category: "apple", value: plataformas.apple },
                    { category: "deezer", value: plataformas.deezer },
                ]
            },
            mark: {
                type: "arc"
            },
            encoding: {
                theta: { field: "value", type: "quantitative" },
                color: { field: "category", type: "nominal", scale: { range: ["#FF2D55", "#8A2BE2", "#1DB954"] } }
            }
        }
    };
}

let top15 = dataSet.slice(0,15);

let allChart =top15.map((musica, index) => ({
    nome: musica.track_name,
    posicaoTop10: index + 1,
    chartDeezer: musica.in_deezer_charts,
    chartShazam: musica.in_shazam_charts,
    chartApple: musica.in_apple_charts,
    chartSpotify: musica.in_spotify_charts,
}));

console.log(allChart)
function allCharts(divWidth) {
    return {
        spec: {
            width: divWidth,
            data: {
                values: allChart
            },
            repeat: { "layer": ["chartSpotify", "chartApple", "chartDeezer", "chartShazam"] },
            spec: {
                "mark": "bar",
                "encoding": {
                    "x": {
                        "field": "nome",
                        "type": "ordinal",
                        "bandwidth": 10.8 // Adjust the value as desired to increase the space between the bars
                    },
                    "y": {
                        "aggregate": "sum",
                        "field": { "repeat": "layer" },
                        "type": "quantitative",
                        "title": "Posicao no Chart"
                    },
                    "color": { "datum": { "repeat": "layer" }, "title": "Streaming Chart", scale: { "range": ["#1DB954", "#FF0000", "#8A2BE2", "#3a68bc"] } },
                    "xOffset": { "datum": { "repeat": "layer" }
                    }
                },
                "config": {
                    "mark": { "invalid": null },
                    "scale": { "y": { "zero": true } },
                    "axis": { "title": "Título da Legenda Lateral" }
                },
                "transform": [
                    {
                        "stack": "y",
                        "as": ["y_start", "y_end"],
                        "groupby": ["posicaoTop10"]
                    }
                ]
            }
        }
    };
}


let nullCharts = {
    spotify : 0,
    apple : 0,
    deezer :0,
    shazam: 0
}
dataSet.forEach(musica => {
    nullCharts.spotify += musica.in_spotify_charts === null ? 1 : 0;
    nullCharts.apple += musica.in_apple_charts === null ? 1 : 0;
    nullCharts.deezer += musica.in_deezer_charts === null ? 1 : 0;
    nullCharts.shazam += musica.in_shazam_charts === null ? 1 : 0;
});


function nullValuesChart(divWidth) {
    return {
        spec: {
            width: divWidth,
            data: {
                values: [
                    { category: "spotify", value: nullCharts.spotify },
                    { category: "apple", value: nullCharts.apple },
                    { category: "deezer", value: nullCharts.deezer },
                    { category: "shazam", value: nullCharts.shazam }
                ]
            },
            mark: {
                type: "arc"
            },
            encoding: {
                theta: { field: "value", type: "quantitative" },
                color: { field: "category", type: "nominal", scale: { "range": ["#FF0000", "#8A2BE2", "#3a68bc", "#1DB954"]},  title: "Plataforma" }
            }
        }
    };
}









const divWidth = Generators.width(document.querySelector("#totalPlaylist"));
  ```