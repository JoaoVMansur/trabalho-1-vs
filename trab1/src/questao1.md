# Questão 1: Existe alguma característica que aumenta as chances de uma música se tornar popular?

<br>
<br>

# Análise 1: mês de lançamento das músicas.
Para iniciar, realizamos um filtro para considerar apenas as músicas com mais de um bilhão de streams separando assim as musicas mais ouvidas entre as mais ouvidas.

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
Analisando primeiro a tabela com as 152 musicas com mais de 1 bilhão de streams temos o seguinte gráfico:
<div class="grid grid-cols-2">
    <div id="graficoMes" class="card grid-colspan-2">
        <h2 class="title">quantidade de músicas lançadas x mês</h2>
        <div style="width: 100%; margin-top: 15px;">
            ${vl.render(graficoMes(divWidth - 200))}
        </div>
    </div>
</div>

De acordo com o gráfico apresentado, podemos notar uma grande disparidade entre as músicas lançadas em janeiro e as lançadas em qualquer outro mês do ano. Dessa forma, isso pode ser um indicativo de uma característica para as músicas mais populares serem normalmente lançadas em janeiro. No entanto, como veremos no gráfico a seguir, se considerarmos todas as músicas presentes no conjunto de dados não só as musicas com mais de 1 bilhão de streams, podemos ver mais um mês se destacando entre os demais: Maio.

<div class="grid grid-cols-2">
    <div id="graficoMesTotal" class="card grid-colspan-2">
        <h2 class="title">quantidade de músicas lançadas x mês</h2>
        <div style="width: 100%; margin-top: 15px;">
            ${vl.render(graficoMesTotal(divWidth - 200))}
        </div>
    </div>
</div>

Portanto, dado o histórico citado anteriormente do teor dos dados analisados, podemos levantar a hipótese de que músicas lançadas em janeiro e em maio têm a chance de se tornar mais popular.
## Análise 2: quantidade de artistas na música
Também Realizamos uma análise comparativa das músicas contidas no conjunto de dados, investigando quantas delas, dentre as mais ouvidas, apresentam a participação de múltiplos artistas em sua criação.

<div class="grid grid-cols-2">
    <div id="graficoArtistCount" class="card grid-colspan-2">
        <h2 class="title">Artista(s) em uma música x Quantidade</h2>
        <div style="width: 100%; margin-top: 15px;">
            ${vl.render(graficoArtistCount(divWidth - 200))}
        </div>
    </div>
</div>

A visualização dos dados revelou uma discrepância significativa entre as músicas lançadas por um único artista e aquelas que contam com participações especiais. Essa observação nos levanta a hipótese de que as preferências do público tendem a se inclinar mais para as músicas em que os artistas estão exclusivamente envolvidos.

Entretanto, é importante considerar outros elementos que possam influenciar essa diferença. Por exemplo, músicas com colaborações podem atrair fãs de diferentes artistas, aumentando assim sua exposição e alcance. Além disso, a qualidade da música e a dinâmica entre os artistas também desempenham um papel crucial na sua recepção pelo público.

Para uma compreensão mais abrangente desse fenômeno, seria interessante explorar outros aspectos das músicas, como gênero, estilo, letra e contexto de lançamento.

# Análise 3: dançabilidade e positividade
Como uma última análise, escolhemos os dados de dançabilidade e positividade das músicas nesse conjunto de dados, disponíveis pelas colunas danceability_% e valence_%.

<div id="graficoPositividadeDancabilidade" class="card">
    <h1>dançabilidade e positividade</h1>
    <div style="width: 100%; margin-top: 15px;">
        ${ vl.render(graficoPositividadeDancabilidade(divWidth - 45)) }
    </div>
</div>

Com a ajuda da visualização apresentada, fica evidente que as músicas mais ouvidas em 2023 tendem a ter um estilo mais dançante. Isso é indicado pela concentração significativa de pontos no gráfico, principalmente acima de 50% no eixo x, que representa a dançabilidade das músicas. Esse padrão sugere uma preferência do público por músicas com ritmo animado e propenso à dança.

No entanto, ao analisar a positividade das músicas, representada no eixo y, não observamos um padrão tão evidente. O gráfico exibe uma distribuição mais uniforme, sem um agrupamento claro de pontos. Isso sugere que a positividade das músicas pode não ser um fator determinante para sua popularidade.

Apesar disso, essa visualização nos permite formular uma hipótese inicial sobre os elementos que contribuem para o sucesso das músicas. Parece que a dançabilidade desempenha um papel mais significativo do que a positividade, pelo menos com base nos dados analisados.
## Conclusão
Portanto, combinando as análises fornecidas pela visualização, podemos hipotetizar que músicas com características mais dançantes, produzidas por apenas um artista e lançadas nos meses de janeiro ou maio, podem ter maior probabilidade de se tornarem populares. Esses meses podem ser estratégicos devido a fatores como menor concorrência de lançamentos ou alinhamento com eventos sazonais.

É claro que, para uma hipótese mais robusta, outros aspectos também deveriam ter sido levados em consideração, como os gêneros musicais, a qualidade da produção e a eficácia das estratégias de marketing. Esses elementos podem interagir de maneiras complexas e influenciar a popularidade de uma música de maneira significativa.

## Design utilizados
Nesta pergunta, utilizamos dois tipos de gráficos: o gráfico de barras e o gráfico de dispersão (scatterplot). Para as análises 1 e 2, optamos por utilizar apenas gráficos de barras, pois esses são ideais para comparar dados categóricos, como os meses de lançamento das músicas e a quantidade de artistas envolvidos na criação de cada música. Os gráficos de barras permitem uma representação clara das contagens ou frequências de cada categoria, facilitando a visualização de padrões e discrepâncias. Para os marcadores, meses de lançamento e o número de artistas envolvidos em cada música são representados por barras verticais azuis. Quanto aos canais visuais, a altura das barras indica a quantidade de músicas em cada categoria, enquanto o eixo x representa as categorias e o eixo y representa a contagem ou número de músicas.

No entanto, para a análise 3, que envolve o relacionamento entre duas variáveis contínuas, como a dançabilidade e a positividade das músicas, escolhemos utilizar um gráfico de dispersão. Este tipo de gráfico é mais adequado para mostrar a distribuição dos dados ao longo de duas dimensões e identificar correlações, agrupamentos ou padrões entre as variáveis. Queríamos investigar se as duas variáveis tinham alguma correlação, ou seja, se músicas mais dançantes e positivas seriam mais populares. Porém, apesar da hipótese ter falhado devido à positividade, como citado com mais detalhes na análise 3, ainda assim conseguimos obter bons resultados com essa visualização. Para os marcadores, cada ponto no gráfico representa uma única música. Já os canais visuais, as coordenadas (x,y) representam os valores de dançabilidade e positividade.








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
                },
                color:{
                    field: "ocorrencia"
                },
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
                },
                color:{
                    field: "ocorrencia",
                },
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
            mark: {
                type: "bar"
            },
            encoding: {
                x: {
                    field: "qtd",
                    type: "nominal",
                },
                y: {
                    field: "ocorrencia",
                    type: "quantitative",
                },
                color: {
                    field: "qtd",
                    type: "nominal", // Assuming qtd is a categorical variable
                    scale: {scheme: "category10"} // You can choose a color scheme here
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
                },

            }
        }
    };
}
const divWidth = Generators.width(document.querySelector("#graficoMes"));
```