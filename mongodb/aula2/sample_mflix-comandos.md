```js
db.movies.find({ 
	title: "The Dark Knight"
	 }, { 
		title: 1,
 		year: 1,
	 	genres: 1,
		_id: 0
		}).pretty()
```

```js
// Primeiro, obter o _id do filme
var movie = db.movies.findOne({ title: "The Dark Knight" });
// Agora buscar todos os comentários desse filme
db.comments.find({ movie_id: movie._id }, { name: 1, text: 1, _id: 0 });
```

<!-- Consultar Comentários de Um Filme: -->
```js
var movie = db.movies.findOne({ title: "The Dark Knight" });
print(movie._id)

db.comments.find(
	{ movie_id: movie._id },
	{ name: 1,
 	text: 1,
 	_id: 0 
});
```

<!-- Exemplo 3: Usar Agregação para Contar Comentários por Filme
Podemos usar o estágio $lookup para fazer algo semelhante a um JOIN entre coleções. -->
```js
db.movies.aggregate([
  {
    $match: { title: "The Dark Knight" }
  },
  {
    $lookup: {
      from: "comments",
      localField: "_id",
      foreignField: "movie_id",
      as: "comentarios"
    }
  },
  {
    $project: {
      title: 1,
      comentarios_count: { $size: "$comentarios" },
      _id: 0
    }
  }
]);
```

<!-- É utilizado para criar coleções e adicionar informações em coleções -->
```js
db.clientes.insertMany([
  {
    nome: "João Silva",
    email: "joao@email.com",
    idade: 28,
    cidade: "São Paulo"
  },
  {
    nome: "Ana Souza",
    email: "ana@email.com",
    idade: 35,
    cidade: "Rio de Janeiro"
  },
  {
    nome: "Carlos Pereira",
    email: "carlos@email.com",
    idade: 42,
    cidade: "São Paulo"
  }
])
```

<!-- Mostrar quais são os arquivos que contenham São Paulo como cidade. -->
```js
db.clientes.find({ cidade: "São Paulo" })
```

<!-- Atualiza a idade do joão de 28 para 29 -->
```js
// Encontra o João pelo email
db.clientes.find({ email: "joao@email.com"  })
// Atualiza a idade do joão de 28 para 29
db.clientes.updateOne(
  { email: "joao@email.com" },
  { $set: { idade: 29 } }
)
// Mostra novamente as informações do João com a idade atualizada
db.clientes.find({ email: "joao@email.com"  })
```
<!-- Adicionar mais 1 ano a idade de todas as pessoas cadastradas no banco -->
<!-- O {} vazio é para selecionar todos os cadastrados e selecionar todos para adicionar mais 1   -->
```js
db.clientes.updateMany(
  {},
  { $inc: { idade: 1 } }
)
```
<!-- Deletar a Ana dos arquivos pelo email -->
<!-- O "ana@email.com" é utilizado como filtro para excluir a Ana -->
```js
db.clientes.find({ email: "ana@email.com" })

db.clientes.deleteOne({ email: "ana@email.com" })
```