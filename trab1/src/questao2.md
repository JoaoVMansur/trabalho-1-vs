#   Questão 2: O conjunto das top 10 músicas e dos top 10 artistas varia muito se considerarmos apenas músicas lançadas no mesmo ano?

<br>
<br>
<br>


# Limpando os dados

Primeiramente verificamos quantas musicas foram lancadas nos anos disponiveis no data set.


```js
view(Inputs.table(musicasAnos))
```

Podemos notar que apenas 13 anos têm mais de 10 músicas lançadas. No entanto, para uma análise mais precisa, optamos por considerar apenas os anos com mais de 15 músicas lançadas, garantindo assim uma qualidade de dados superior. Com poucas músicas disponíveis, o top 10 já se forma automaticamente, o que pode distorcer os resultados da análise.

Apartir disso vamos para as análises.

# Top 10 Músicas.
<br>
<br>
<br>

## Análise 1: Total de streams do top 10 de cada ano.

Apos filtrar os dados da maneira citada anteriormente, ficamos com os ano de 2016,2017,2019,2020,2021,2022 e 2023

esses anos tem a seguinte quantidades de musicas lancadas em seus anos.


<div id="musicasAno" class="card">
    <h1>Quantidade de músicas em cada ano</h1>
    <div style="width: 100%; margin-top: 15px;">
        ${ vl.render(musicasAno(divWidth - 45)) }
    </div>
</div>

Com isso vamos comparar quantos streams tem o top 10 em cada ano.
<div id="streamsTotaisPorAno" class="card">
    <h1>Quantidade de músicas em cada ano</h1>
    <div style="width: 100%; margin-top: 15px;">
        ${ vl.render(streamsTotaisPorAno(divWidth - 45)) }
    </div>
</div>

Durante o período compreendido entre 2017 e 2019, assim como nos anos de 2020 e 2021, observamos uma relativa estabilidade na quantidade de streams das músicas. No entanto, ao compararmos esses anos com períodos anteriores e posteriores, destacam-se mudanças significativas. Por exemplo, enquanto os anos de 2017 a 2019 mostraram uma certa estabilidade, os anos subsequentes ou anteriores podem ter testemunhado um aumento ou diminuição considerável na quantidade de streams, refletindo talvez mudanças no mercado musical, preferências do público ou até mesmo fatores externos, como eventos globais ou o lançamento de novas plataformas de streaming. Essas variações podem ser analisadas mais detalhadamente para entender melhor as tendências e os padrões de consumo de música ao longo do tempo.


```js
const divWidth = Generators.width(document.querySelector("#streamsTotaisPorAno"));

import * as vega from "npm:vega";
import * as vegaLite from "npm:vega-lite";
import * as vegaLiteApi from "npm:vega-lite-api";

const vl = vegaLiteApi.register(vega, vegaLite);


const dataSet = await FileAttachment("./data/spotify-2023.csv").csv({typed: true});


let anos = dataSet.map((musica) => musica.released_year);
let lista = Array.from(new Set(anos));
lista.sort()

let musicasAnos = lista.map(x =>({
    ano: x,
    qtd: 0
}))

dataSet.forEach(musica => {
    musicasAnos.forEach(item => {
        if (musica.released_year === item.ano) {
            item.qtd++;
        }
    });
});

musicasAnos.sort((a, b) => b.qtd - a.qtd);

let musicasPorAnoMaisDe15 = [];

musicasAnos.forEach(item => {
    if (item.qtd > 15) {
        let musicasDoAno = dataSet.filter(musica => musica.released_year === item.ano);
        musicasPorAnoMaisDe15.push({
            ano: item.ano,
            qtd: item.qtd,
            musicas: musicasDoAno
        });
    }
});



let top10PorAno = [];

for (let ano in musicasPorAnoMaisDe15) {
    const musicasDoAno = musicasPorAnoMaisDe15[ano].musicas;
    musicasDoAno.sort((a, b) => b.streams - a.streams);
    top10PorAno.push(musicasDoAno.slice(0, 10));
}




const streamsPorAno = {};

top10PorAno.forEach(musicasDoAno => {
    const ano = musicasDoAno[0].released_year;
    const totalStreams = musicasDoAno.reduce((acc, musica) => acc + musica.streams, 0);
    streamsPorAno[ano] = totalStreams;
});

const data = Object.entries(streamsPorAno).map(([ano, streams]) => ({ ano, streams }));



function musicasAno(divWidth) {
    return {
        spec: {
            width: divWidth,
            data: {
                values: musicasPorAnoMaisDe15
            },
            mark: {
                type: "bar"
            },
            encoding: {
                x: {
                    field: "ano",
                    type: "ordinal",
                },
                y: {
                    field: "qtd",
                    type: "quantitative",
                },
                color: {
                    field: "qtd"
                }
            }
        }
    };
}

function streamsTotaisPorAno(divWidth) {
    return {
        spec: {
            width: divWidth,
            data: {
                values: data
            },
            "mark": {
                "type": "line"
            },
            "encoding": {
                "x": {
                    "field": "ano",
                    "type": "nominal",
                },
                "y": {
                    "field": "streams",
                    "type": "quantitative",
                },
            
            }
        }
    };
}
