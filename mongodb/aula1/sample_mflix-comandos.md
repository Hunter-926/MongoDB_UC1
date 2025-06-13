```js
db.movies.find({
  "imdb.rating": { $gt: 8.5 }
}, {
  title: 1,
  "imdb.rating": 1,
  _id: 0
}).limit(5)
```

```js
db.movies.find({
  year: 2022
}, {
  title: 1,
  year: 1,
  genrs:1,
  _id: 0
}).limit(5)

```

```js
db.movies.find({
  genres:{ $size: 3 }
}, {
  title: 1,
  year: 1,
  genrs:1,
  _id: 0
}).limit(5)


```

```js
db.movies.countDocuments({
  genres:{ $size: 3 }
})
```

```js
db.movies.find({
  title: "The Dark Knight"
}, {
  title: 1,
	year: 1,
  genrs:1,
  _id: 0
}).pretty()
```

```js
db.movies.find({
  title: "The Dark Knight"
}, {
  title: 1,
	year: 1,
  genrs:1,
  _id: 0
})
```

```js
var movie = db.movies.findOne({title: "The Dark Knight"});

print(movie._id)

db.comments.find({ 
  movie_id: movie._id 
  }, { 
    date:1, 
    name: 1, 
    text: 1 
  });

```

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