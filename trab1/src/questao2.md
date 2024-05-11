#   Questão 2


. O conjunto das top 10 músicas e dos top 10 artistas varia muito se considerarmos apenas músicas lançadas no mesmo ano?
Top 10 musica
```js
const top10Musicas = musicasMaisPopulares.slice(0, 10);

view(Inputs.table(top10Musicas));

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
view(Inputs.table(anoOcorrencia))

const sdas = dataSet.map(x => ({
  spotify: x.in_spotify_playlists,
  deezer: x.in_deezer_playlists,
  apple: x.in_apple_playlists,
}));