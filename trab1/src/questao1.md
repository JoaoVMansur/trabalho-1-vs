# Questão 1: Existe alguma característica que aumenta as chances de uma música se tornar popular?




Para iniciar, realizei um filtro para considerar apenas as músicas com mais de um bilhão de streams.

```js
const dataSet = await FileAttachment("./data/spotify-2023.csv").csv({typed: true});

  dataSet.sort((a,b) => {
   
      return b.streams - a.streams
    
  })
  const musicasMaisPopulares = dataSet.filter(musica => musica.streams > 1000000000);
  musicasMaisPopulares.sort((a, b) => b.streams - a.streams);

  ```
Com isso, temos uma tabela com 152 músicas.
```js
view(Inputs.table(musicasMaisPopulares));
```
Primeiramente, analisamos se dentro dessas 152 músicas mais tocadas em 2023, existia algum padrão no mês de lançamento.
```js



let mesLancamento = musicasMaisPopulares.map((musica) => musica.released_month);
let lista = Array.from(new Set(mesLancamento));

const mesOcorrencia = lista.map(x => ({
  mes: x,
  ocorrencia: 0
}));

musicasMaisPopulares.forEach(y => {
  mesOcorrencia.forEach(x => {
    if (x.mes === y.released_month) {
      x.ocorrencia++;
    }
  });
});

mesOcorrencia.sort((a, b) => b.ocorrencia - a.ocorrencia);


let mesLancamentoTotal = musicasMaisPopulares.map((musica) => musica.released_month);
let listaTotal = Array.from(new Set(mesLancamentoTotal));

const mesOcorrenciaTotal = listaTotal.map(x => ({
  mes: x,
  ocorrencia: 0
}));

dataSet.forEach(y => {
  mesOcorrenciaTotal.forEach(x => {
    if (x.mes === y.released_month) {
      x.ocorrencia++;
    }
  });
});
mesOcorrenciaTotal.sort((a, b) => b.ocorrencia - a.ocorrencia);


let artistsCount = dataSet.map((musica) => musica.artist_count);
let artistList = Array.from(new Set(artistsCount));

const artistCountOcorrencia = artistList.map(x => ({
  qtd: x,
  ocorrencia: 0
}))

dataSet.forEach(y => {
  artistCountOcorrencia.forEach(x => {
    if (x.qtd === y.artist_count){
      x.ocorrencia++
    }
  })
})
artistCountOcorrencia.sort((a,b) => b.ocorrencia- a.ocorrencia);


let positDance = dataSet.map(musica =>({
  dancabilidade: musica["danceability_%"],
  positividade: musica["valence_%"],
}))



```

```js
import * as vega from "npm:vega";
import * as vegaLite from "npm:vega-lite";
import * as vegaLiteApi from "npm:vega-lite-api";

const vl = vegaLiteApi.register(vega, vegaLite);

function graficoMes(divWidth) {
    return {
        spec: {
            width: divWidth,
            data: {
                values: mesOcorrencia
            },
            "mark": {
                "type": "bar"
            },
            "encoding": {
                "x": {
                    "field": "mes",
                    "type": "nominal",
                },
                "y": {
                    "field": "ocorrencia",
                    "type": "quantitative",
                }
            }
        }
    };
}
function graficoMesTotal(divWidth) {
    return {
        spec: {
            width: divWidth,
            data: {
                values: mesOcorrenciaTotal
            },
            "mark": {
                "type": "bar"
            },
            "encoding": {
                "x": {
                    "field": "mes",
                    "type": "nominal",
                },
                "y": {
                    "field": "ocorrencia",
                    "type": "quantitative",
                }
            }
        }
    };
}

function graficoArtistCount(divWidth) {
    return {
        spec: {
            width: divWidth,
            data: {
                values: artistCountOcorrencia
            },
            "mark": {
                "type": "bar"
            },
            "encoding": {
                "x": {
                    "field": "qtd",
                    "type": "nominal",
                },
                "y": {
                    "field": "ocorrencia",
                    "type": "quantitative",
                }
            }
        }
    };
}
function graficoPositividadeDancabilidade(divWidth) {
    return {
        spec: {
            width: divWidth,
            height: 400,
            data: {
                values: positDance
            },
            "mark": {
                "type": "circle"
            },
            "encoding": {
                "x": {
                    "field": "dancabilidade",
                    "type": "quantitative"
                },
                "y": {
                    "field": "positividade",
                    "type": "quantitative"
                }
            }
        }
    };
}
const divWidth = Generators.width(document.querySelector("#graficoMes"));
```
## Musicas no top 152 vs Mês do seu lancamento

<div class="grid grid-cols-2">
    <div id="graficoMes" class="card grid-colspan-2">
        <h2 class="title">quantidade de músicas lançadas x mês</h2>
        <div style="width: 100%; margin-top: 15px;">
            ${vl.render(graficoMes(divWidth - 200))}
        </div>
    </div>
</div>

De acordo com o gráfico apresentado, podemos notar uma grande disparidade entre as músicas lançadas em janeiro e as lançadas em qualquer outro mês do ano. Dessa forma, isso pode ser um indicativo de uma característica para as músicas mais populares serem normalmente lançadas em janeiro. No entanto, como veremos no gráfico a seguir, se considerarmos todas as músicas presentes no conjunto de dados, podemos ver mais um mês se destacando entre os demais Maio.

<div class="grid grid-cols-2">
    <div id="graficoMesTotal" class="card grid-colspan-2">
        <h2 class="title">quantidade de músicas lançadas x mês</h2>
        <div style="width: 100%; margin-top: 15px;">
            ${vl.render(graficoMesTotal(divWidth - 200))}
        </div>
    </div>
</div>

Portanto, dado o histórico citado anteriormente do teor dos dados analisados, podemos levantar a hipótese de que músicas lançadas em janeiro e em maio têm a chance de se tornar mais popular.

Também Realizamos uma análise comparativa das músicas contidas no conjunto de dados, investigando quantas delas, dentre as mais ouvidas, apresentam a participação de múltiplos artistas em sua criação.

<div class="grid grid-cols-2">
    <div id="graficoArtistCount" class="card grid-colspan-2">
        <h2 class="title">Artista(s) em uma musica x Quantidade</h2>
        <div style="width: 100%; margin-top: 15px;">
            ${vl.render(graficoArtistCount(divWidth - 200))}
        </div>
    </div>
</div>

A visualização dos dados revelou uma discrepância significativa entre as músicas lançadas por um único artista e aquelas que contam com participações especiais. Essa observação nos levanta a hipótese de que as preferências do público tendem a se inclinar mais para as músicas em que os artistas estão exclusivamente envolvidos.

Entretanto, é importante considerar outros elementos que possam influenciar essa diferença. Por exemplo, músicas com colaborações podem atrair fãs de diferentes artistas, aumentando assim sua exposição e alcance. Além disso, a qualidade da música e a dinâmica entre os artistas também desempenham um papel crucial na sua recepção pelo público.

Para uma compreensão mais abrangente desse fenômeno, seria interessante explorar outros aspectos das músicas, como gênero, estilo, letra e contexto de lançamento.


Como uma última análise, escolhemos os dados de dançabilidade e positividade das músicas nesse conjunto de dados, disponíveis pelas colunas danceability_% e valence_%.

<div id="graficoPositividadeDancabilidade" class="card">
    <h1>dancabilidade e positividade</h1>
    <div style="width: 100%; margin-top: 15px;">
        ${ vl.render(graficoPositividadeDancabilidade(divWidth - 45)) }
    </div>
</div>

Com a ajuda da visualização apresentada, fica evidente que as músicas mais ouvidas em 2023 tendem a ter um estilo mais dançante. Isso é indicado pela concentração significativa de pontos no gráfico, principalmente acima de 50% no eixo x, que representa a dançabilidade das músicas. Esse padrão sugere uma preferência do público por músicas com ritmo animado e propenso à dança.

No entanto, ao analisar a positividade das músicas, representada no eixo y, não observamos um padrão tão evidente. O gráfico exibe uma distribuição mais uniforme, sem um agrupamento claro de pontos. Isso sugere que a positividade das músicas pode não ser um fator determinante para sua popularidade.

Apesar disso, essa visualização nos permite formular uma hipótese inicial sobre os elementos que contribuem para o sucesso das músicas. Parece que a dançabilidade desempenha um papel mais significativo do que a positividade, pelo menos com base nos dados analisados.

Portanto, combinando as análises fornecidas pela visualização, podemos hipotetizar que músicas com características mais dançantes, produzidas por apenas um artista e lançadas nos meses de janeiro ou maio, podem ter maior probabilidade de se tornarem populares. Esses meses podem ser estratégicos devido a fatores como menor concorrência de lançamentos ou alinhamento com eventos sazonais.

É claro que, para uma hipótese mais robusta, outros aspectos também deveriam ter sido levados em consideração, como os gêneros musicais, a qualidade da produção e a eficácia das estratégias de marketing. Esses elementos podem interagir de maneiras complexas e influenciar a popularidade de uma música de maneira significativa.