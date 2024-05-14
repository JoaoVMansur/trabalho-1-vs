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
        <h2 class="title">quantidade de músicas lançadas x mês</h2>
        <div style="width: 100%; margin-top: 15px;">
            ${vl.render(totalPlaylist(divWidth - 200))}
        </div>
    </div>
</div>

A predominância avassaladora de playlists no Spotify é notável. Essa tendência levanta duas possibilidades interpretativas: o Spotify é, de fato, a plataforma de streaming musical mais popular entre as três analisadas, ou essa prevalência é um reflexo direto do conjunto de dados específico utilizado neste estudo, intitulado "Most Streamed Spotify Songs 2023". Pessoalmente, inclino-me a considerar a segunda possibilidade como a mais plausível. Embora o Spotify seja inegavelmente a plataforma de streaming de música mais reconhecida e amplamente utilizada, a discrepância apresentada pelos dados parece exagerada e possivelmente distorcida pela natureza do conjunto de dados.

Além disso, quando analisamos os dados da Apple, observamos que, embora seja menos popular que o Spotify, a diferença de popularidade entre as duas plataformas não parece tão exagerada quanto o retratado nos dados. Isso sugere que, enquanto o Spotify continua a ser líder em termos de criação de playlists, a posição relativa das outras plataformas pode estar sub-representada ou mal interpretada no conjunto de dados atual. Portanto, é importante abordar essa disparidade com cautela e considerar o contexto mais amplo ao interpretar os resultados desta análise.




# Análise 2:



<div class="grid grid-cols-2">
    <div id="allCharts" class="card grid-colspan-2">
        <h2 class="title">quantidade de músicas lançadas x mês</h2>
        <div style="width: 100%; margin-top: 15px;">
            ${vl.render(allCharts(divWidth - 200))}
        </div>
    </div>
</div>

```js
  
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

import * as vega from "npm:vega";
import * as vegaLite from "npm:vega-lite";
import * as vegaLiteApi from "npm:vega-lite-api";

const vl = vegaLiteApi.register(vega, vegaLite);

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

let top10 = dataSet.slice(0,10);

let allChart = top10.map((musica, index) => ({
    chartDeezer: musica.in_deezer_charts,
    chartShazam: musica.in_shazam_charts,
    chartApple: musica.in_apple_charts,
    chartSpotify: musica.in_spotify_charts,
    posicaoTop10: index + 1,
}));

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
                        "field": "posicaoTop10",
                        "type": "ordinal",
                        "bandwidth": 10.8 // Ajuste o valor conforme desejado para aumentar o espaço entre as barras
                    },
                    "y": {
                        "aggregate": "sum",
                        "field": { "repeat": "layer" },
                        "type": "ordinal",
                        "title": "Posicao no Chart"
                    },
                    "color": { "datum": { "repeat": "layer" }, "title": "Streaming Chart" },
                    "xOffset": { "datum": { "repeat": "layer" } }
                },
                "config": {
                    "mark": { "invalid": null },
                    "scale": { "y": { "zero": true } },
                    "axis": { "title": "Título da Legenda Lateral" }
                }
            }
        }
    };
}













const divWidth = Generators.width(document.querySelector("#totalPlaylist"));
  ```