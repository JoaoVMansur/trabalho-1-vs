#   Questão 2: O conjunto das top 10 músicas e dos top 10 artistas varia muito se considerarmos apenas músicas lançadas no mesmo ano?


Em primeiro lugar pegamos apenas os dados dos anos que consideramos relevante. Pois de um total de 32 anos que constam no data set, apenas 7 possuem mais de 10 musicas. Sepramos os anos com mais de 15 músicas no ano para ter um dado mais relevante.
Top 10 musica
```js

const dataSet = await FileAttachment("./data/spotify-2023.csv").csv({typed: true});

// 1. Agrupar as músicas por ano de lançamento`
const musicasPorAno = {};
dataSet.forEach(musica => {
  const ano = musica.released_year;
  if (!musicasPorAno[ano]) {
    musicasPorAno[ano] = [];
  }
  musicasPorAno[ano].push(musica);
});

// 2. Selecionar as top 10 músicas de cada ano
const top10PorAno = {};
for (const ano in musicasPorAno) {
  
  if(musicasPorAno[ano].length > 15){
      const musicasDoAno = musicasPorAno[ano];
  musicasDoAno.sort((a, b) => b.streams - a.streams); // Ordenar por número de reproduções
  top10PorAno[ano] = musicasDoAno.slice(0, 10);
  }
}
console.log(top10PorAno); 



const streamsPorAno = {};

for (const ano in top10PorAno) {
  let totalStreams = top10PorAno[ano].reduce((acc, musica) => acc + musica.streams, 0);
  streamsPorAno[ano] = totalStreams;
}
const data = Object.entries(streamsPorAno).map(([ano, streams]) => ({ ano, streams }));
console.log(data);

function graficoMes(divWidth) {
    return {
        spec: {
            width: divWidth,
            data: {
                values: data
            },
            "mark": {
                "type": "bar"
            },
            "encoding": {
                "x": {
                    "field": "ano",
                    "type": "nominal",
                },
                "y": {
                    "field": "streams",
                    "type": "quantitative",
                }
            }
        }
    };
}

```
<div class="grid grid-cols-2">
    <div id="graficoMes" class="card grid-colspan-2">
        <h2 class="title">quantidade de músicas lançadas x mês</h2>
        <div style="width: 100%; margin-top: 15px;">
            ${vl.render(graficoMes(divWidth - 200))}
        </div>
    </div>
</div>

```js
const divWidth = Generators.width(document.querySelector("#graficoMes"));