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

Apos filtrar os dados da maneira citada anteriormente, ficamos com os ano de 2016,2017,2019,2020,2021,2022 e 2023

esses anos tem a seguinte quantidades de musicas lancadas em seus anos.

Apartir disso vamos para as análises.
<div id="musicasAno" class="card">
    <h1>Quantidade de músicas em cada ano</h1>
    <div style="width: 100%; margin-top: 15px;">
        ${ vl.render(musicasAno(divWidth - 45)) }
    </div>
</div>
# Top 10 Músicas.


## Análise : Total de streams do top 10 de músicas cada ano.

Com o filtro para os anos feito vamos comparar quantos streams tem o top 10 em cada ano.
<div id="streamsTotaisPorAno" class="card">
    <h1>Total de streams das 10 músicas mais populares</h1>
    <div style="width: 100%; margin-top: 15px;">
        ${ vl.render(streamsTotaisPorAno(divWidth - 45)) }
    </div>
</div>

Durante o período compreendido entre 2017 e 2019, assim como nos anos de 2020 e 2021, observamos uma relativa estabilidade na quantidade de streams das músicas. No entanto, ao compararmos esses anos com períodos anteriores e posteriores, destacam-se mudanças significativas. Por exemplo, enquanto os anos de 2017 a 2019 mostraram uma certa estabilidade, os anos subsequentes ou anteriores podem ter testemunhado um aumento ou diminuição considerável na quantidade de streams, refletindo talvez mudanças no mercado musical, preferências do público ou até mesmo fatores externos, como eventos globais ou o lançamento de novas plataformas de streaming. Essas variações podem ser analisadas mais detalhadamente para entender melhor as tendências e os padrões de consumo de música ao longo do tempo.

# Top 10 Artistas

## Análise :Total de streams do top 10 artistas de cada ano. 
Com o mesmo filtro para os anos relevantes desta vez fizemos um top 10 dos artistas com mais streams em suas músicas para cada ano valido, apos isso, somamos o número de streams dos artistas de cada ano para saber quantos streams o top 10 de artistas tinha em cada ano.

<div id="streamTop10Artists" class="card">
    <h1>Total de streams das 10 músicas mais populares</h1>
    <div style="width: 100%; margin-top: 15px;">
        ${ vl.render(streamTop10Artists(divWidth - 45)) }
    </div>
</div>


Com esta visualização, podemos notar uma dinâmica mais complexa além do ranking das 10 músicas mais populares. Entre 2016 e 2018, observamos um leve aumento no número de streams, seguido por uma pequena queda em 2019. No entanto, entre 2020 e 2022, há um aumento significativo antes de uma queda acentuada em 2023. Esta tendência revela padrões mais sutis de comportamento do público e possíveis mudanças no cenário da indústria musical. essa movimentações podem diversas justificativas que dado o conjunto de dados que temos não é tão simples de responder.

# Conclusão

Com a ajuda das visualizações criadas conseguimos ver que de fato ao longo dos anos tanto o top 10 múscas quanto o top 10 artistas variaram.

# Desings utilizados.

Nesta análise, utilizamos dois tipos de gráficos distintos: o gráfico de barras e o gráfico de linhas.

Para visualizar o processo de filtragem e apresentar os dados após a manipulação, decidimos utilizar um gráfico de barras, uma escolha eficaz para uma representação visual clara da quantidade de músicas ao longo dos anos analisados. As barras verticais coloridas funcionam como marcadores, cada uma representando a quantidade de músicas em seus respectivos anos. Quanto aos canais visuais, a altura das barras reflete a quantidade de músicas em cada ano, enquanto o eixo x denota os anos e o eixo y representa a contagem ou número de músicas.

Para acompanhar a variação do top 10 das músicas e dos artistas ao longo do tempo, optamos por empregar o gráfico de linhas. Essa escolha nos permite visualizar de forma clara e intuitiva as mudanças nos dados ao longo dos anos, fornecendo insights valiosos sobre as tendências e padrões que emergem nesse período.Nosso gráfico de linhas utiliza uma linha contínua como marcador, destacando a mudança ao longo do tempo nos dados do top 10. Quanto aos canais visuais, o eixo x, assim como no gráfico de barras, representa os anos. Já a altura da linha representa a quantidade de streams no respectivo ano, permitindo uma compreensão imediata da evolução desses dados ao longo do tempo.


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

 



let top10PorAno = {};

for (let ano in musicasPorAnoMaisDe15) {
    const musicasDoAno = musicasPorAnoMaisDe15[ano].musicas;
    musicasDoAno.sort((a, b) => b.streams - a.streams);
    top10PorAno[musicasPorAnoMaisDe15[ano].ano] = {
        musicas: musicasDoAno.slice(0, 10)
    };
}

const streamsPorAno = {};

for (let ano in top10PorAno) {
    const musicasDoAno = top10PorAno[ano].musicas;
    const totalStreams = musicasDoAno.reduce((acc, musica) => acc + musica.streams, 0);
    streamsPorAno[ano] = totalStreams;
}

const data = Object.entries(streamsPorAno).map(([ano, streams]) => ({ ano, streams }));

let artistas = dataSet.map((musica) => musica["artist(s)_name"]);
let listaArtistas = Array.from(new Set(artistas));



let sortedArtistaStreamAno = {};

musicasPorAnoMaisDe15.forEach(anoData => {
    const year = anoData.ano;
    anoData.musicas.forEach(song => {
        const artistName = song["artist(s)_name"];
        const streams = song.streams;

        if (!sortedArtistaStreamAno[year]) {
            sortedArtistaStreamAno[year] = [];
        }

  
        const artistIndex = sortedArtistaStreamAno[year].findIndex(artist => artist.artistName === artistName);
        if (artistIndex !== -1) {
            sortedArtistaStreamAno[year][artistIndex].streams += streams;
        } else {
            sortedArtistaStreamAno[year].push({ artistName, streams });
        }
    });

    sortedArtistaStreamAno[year].sort((a, b) => b.streams - a.streams);
});
let top10ArtistsPerYear = {};

for (const year in sortedArtistaStreamAno) {
    top10ArtistsPerYear[year] = sortedArtistaStreamAno[year].slice(0, 10);
}

let streamsArtistsPerYear = {};

for (const year in sortedArtistaStreamAno) {
    const totalStreams = sortedArtistaStreamAno[year].reduce((total, artist) => total + artist.streams, 0);

    streamsArtistsPerYear[year] = totalStreams;
}

const artistData = Object.entries(streamsArtistsPerYear).map(([ano, streams]) => ({ ano, streams }));

console.log(artistData);


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

function streamTop10Artists(divWidth) {
    return {
        spec: {
            width: divWidth,
            data: {
                values: artistData
            },
            mark: {
                type: "line"
            },
            encoding: {
                x: {
                    field: "ano",
                    type: "ordinal",
                },
                y: {
                    field: "streams",
                    type: "quantitative",
                },
            }
        }
    };
}
```