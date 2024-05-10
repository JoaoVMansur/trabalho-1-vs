# Trabalho 1: Visualização de Dados

Neste trabalho, vamos analisar o conjunto de dados das músicas mais tocadas em 2023, segundo o aplicativo Spotify. Para este trabalho, iremos responder às seguintes três perguntas feitas pelo professor:

1. Existe alguma característica que faz uma música ter mais chances de se tornar popular?
2. O conjunto das top 10 músicas e dos top 10 artistas varia muito se considerarmos apenas músicas lançadas no mesmo ano?
3. Discuta as diferenças entre as plataformas (Spotify, Deezer, Apple Music e Shazam).

Primeiro irei apresentar o data set completo sem nenhuma modificação.

```js
const dataSet = await FileAttachment("./data/spotify-2023.csv").csv({typed: true});

  dataSet.sort((a,b) => {
   
      return b.streams - a.streams
    
  })
view(Inputs.table(dataSet));

```
(1). Existe alguma característica que faz uma música ter mais chances de se tornar popular?

Para começar, fiz um filtro para considerar apenas as músicas com mais de um bilhão de streams.

```js
  const musicasMaisPopulares = dataSet.filter(musica => musica.streams > 1000000000);
  musicasMaisPopulares.sort((a, b) => b.streams - a.streams);

  view(Inputs.table(musicasMaisPopulares));
  ```

Top 10 musicas
```js
const top10Musicas = musicasMaisPopulares.slice(0, 10);

view(Inputs.table(top10Musicas));

```
  NOTAS QUE MAIS OCORREM
  ```js
  let notasMaisFamosas = musicasMaisPopulares.map((musica) => musica.key)
  let array = Array(...new Set(notasMaisFamosas));
  
const notasOcorrencia = array.map( x => ({
  nota : x,
  ocorrencia: 0
}))
notasOcorrencia.forEach(x => {
  musicasMaisPopulares.forEach(y => {
    if (x.nota === y.key) {
      x.ocorrencia++;
    }
  });
});
notasOcorrencia.sort((a, b) => b.ocorrencia - a.ocorrencia);
console.log(notasOcorrencia)
console.log(musicasMaisPopulares.length)


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
console.log(mesOcorrencia);


let anoLancamento = musicasMaisPopulares.map((musica) => musica.released_year);
let ano = Array.from(new Set(anoLancamento));
let anoOcorrencia = ano.map(x =>({
  ano: x,
  ocorrencia:0
}))

dataSet.forEach(y => {
  anoOcorrencia.forEach(x => {
    if (x.ano === y.released_year) {
      x.ocorrencia++;
    }
  });
});

anoOcorrencia.sort((a, b) => b.ocorrencia - a.ocorrencia);
console.log(anoOcorrencia)


