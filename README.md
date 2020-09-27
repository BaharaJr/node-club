# NODE JS CLUB API Spec

## JSON Objects returned by API:

### Users

```JSON
{
  "user": {
    "id": 2,
    "email": "jake@jake.jake",
    "username": "jake",
    "bio": "I work at statefarm",
    "image": null
  }
}
```

`

### Profile

```JSON
{
  "profile": {
    "id":3,
    "username": "jake",
    "bio": "I work at statefarm",
    "image": "https://static.productionready.io/images/smiley-cyrus.jpg",
    "following": false
  }
}
```

### Single Article

```JSON
{
  "article": {
    "id": 3,
    "title": "How to train your dragon",
    "description": "Ever wonder how?",
    "body": "It takes a Jacobian",
    "tagList": ["dragons", "training"],
    "createdAt": "2016-02-18T03:22:56.637Z",
    "updatedAt": "2016-02-18T03:48:35.824Z",
    "favorited": false,
    "favoritesCount": 0,
    "author": {
      "username": "jake",
      "bio": "I work at statefarm",
      "image": "https://i.stack.imgur.com/xHWG8.jpg",
      "following": false
    }
  }
}
```

### Multiple Articles

```JSON
{
  "articles":[{
    "id": 4,
    "title": "How to train your dragon",
    "description": "Ever wonder how?",
    "body": "It takes a Jacobian",
    "tagList": ["dragons", "training"],
    "createdAt": "2016-02-18T03:22:56.637Z",
    "updatedAt": "2016-02-18T03:48:35.824Z",
    "favorited": false,
    "favoritesCount": 0,
    "author": {
      "id": 2,
      "username": "jake",
      "bio": "I work at statefarm",
      "image": "https://i.stack.imgur.com/xHWG8.jpg",
      "following": false
    }
  }, {
    "id": 10,
    "title": "How to train your dragon 2",
    "description": "So toothless",
    "body": "It a dragon",
    "tagList": ["dragons", "training"],
    "createdAt": "2016-02-18T03:22:56.637Z",
    "updatedAt": "2016-02-18T03:48:35.824Z",
    "favorited": false,
    "favoritesCount": 0,
    "author": {
      "username": "jake",
      "bio": "I work at statefarm",
      "image": "https://i.stack.imgur.com/xHWG8.jpg",
      "following": false
    }
  }],
  "articlesCount": 2
}
```

### Single Comment

```JSON
{
  "comment": {
    "id": 1,
    "createdAt": "2016-02-18T03:22:56.637Z",
    "updatedAt": "2016-02-18T03:22:56.637Z",
    "body": "It takes a Jacobian",
    "author": {
      "id": 7,
      "username": "jake",
      "bio": "I work at statefarm",
      "image": "https://i.stack.imgur.com/xHWG8.jpg",
      "following": false
    }
  }
}
```

### Multiple Comments

```JSON
{
  "comments": [{
    "id": 1,
    "createdAt": "2016-02-18T03:22:56.637Z",
    "updatedAt": "2016-02-18T03:22:56.637Z",
    "body": "It takes a Jacobian",
    "author": {
      "id": 100,
      "username": "jake",
      "bio": "I work at statefarm",
      "image": "https://i.stack.imgur.com/xHWG8.jpg",
      "following": false
    }
  }]
}
```

### List of Tags

```JSON
{
  "tags": [
    "reactjs",
    "angularjs"
  ]
}
```

### Errors and Status Codes

If a request fails any validations, expect a 422 and errors in the following format:

```JSON
{
  "errors":{
    "body": [
      "can't be empty"
    ]
  }
}
```

#### Other status codes:

[HTTP Status Codes](https://httpstatuses.com/)

## Endpoints:

### Create User

`POST /api/users/`

Example request body:

```JSON
{
  "user":{
    "name": "Jackob Jake",
    "username":"jake",
    "email": "jake@jake.jake",
    "password": "jakejake"
  }
}
```

### Authentication:

`POST /api/users/login`

Example request body:

```JSON
{
  "user":{
    "email": "jake@jake.jake",
    "password": "jakejake"
  }
}
```

Returns a [User](#users-for-authentication)

Required fields: `email`, `password`

### Registration:

`POST /api/users`

Example request body:

```JSON
{
  "user":{
    "username": "Jacob",
    "email": "jake@jake.jake",
    "password": "jakejake"
  }
}
```

Returns a [User](#users-for-authentication)

Required fields: `email`, `username`, `password`

### Get Current User

`GET /api/user`

Returns a [User](#users-for-authentication) that's the current user

### Update User

`PUT /api/user/:id`

Example request body:

```JSON
{
  "user":{
    "email": "jake@jake.jake",
    "bio": "I like to skateboard",
    "image": "https://i.stack.imgur.com/xHWG8.jpg"
  }
}
```

Returns the [User](#users-for-authentication)

Accepted fields: `email`, `username`, `password`, `image`, `bio`

### Get Profile

`GET /api/profiles/:username`

Authentication optional,Returns a [Profile](#profile)

### Follow user

`POST /api/profiles/:username/follow`
Example request body:

```JSON
{
  "userid": 1
}
```

Returns a [Profile](#profile)

### Unfollow user

`DELETE /api/profiles/:username/follow`

Example request body:

```JSON
{
  "userid": 1
}
```

Returns a [Profile](#profile)

### List Articles

`GET /api/articles`

Returns most recent articles globally by default, provide `tag`, `author` or `favorited` query parameter to filter results

Query Parameters:

Filter by tag:

`?tag=AngularJS`

Filter by author:

`?author=jake`

Favorited by user:

`?favorited=jake`

Limit number of articles (default is 20):

`?limit=20`

Offset/skip number of articles (default is 0):

`?offset=0`

Authentication optional, will return [multiple articles](#multiple-articles), ordered by most recent first

### Feed Articles

`GET /api/articles/feed`

Can also take `limit` and `offset` query parameters like [List Articles](#list-articles)

will return [multiple articles](#multiple-articles) created by followed users, ordered by most recent first.

### Get Article

`GET /api/articles/:id`

Will return [single article](#single-article)

### Create Article

`POST /api/articles`

Example request body:

```JSON
{
  "article": {
    "title": "How to train your dragon",
    "description": "Ever wonder how?",
    "body": "You have to believe",
    "tagList": ["reactjs", "angularjs", "dragons"]
  }
}
```

will return an [Article](#single-article)

Required fields: `title`, `description`, `body`

Optional fields: `tagList` as an array of Strings

### Update Article

`PUT /api/articles/:id`

Example request body:

```JSON
{
  "article": {
    "title": "Did you train your dragon?"
  }
}
```

Returns the updated [Article](#single-article)

Optional fields: `title`, `description`, `body`

The `id` also gets updated when the `title` is changed

### Delete Article

`DELETE /api/articles/:id`

Authentication required

### Add Comments to an Article

`POST /api/articles/:id/comments`

Example request body:

```JSON
{
  "comment": {
    "body": "His name was my name too."
  }
}
```

Returns the created [Comment](#single-comment)

Required field: `body`

### Get Comments from an Article

`GET /api/articles/:id/comments`

Authentication optional,Returns [multiple comments](#multiple-comments)

### Delete Comment

`DELETE /api/articles/:id/comments/:id`

Authentication required

### Favorite Article

`POST /api/articles/:id/favorite`

Returns the [Article](#single-article)

No additional parameters required

### Unfavorite Article

`DELETE /api/articles/:id/favorite`

Returns the [Article](#single-article)

No additional parameters required

### Get Tags

`GET /api/tags`

Returns a [List of Tags](#list-of-tags)

### Resources

[Designing a REST API](https://mherman.org/blog/designing-a-restful-api-with-node-and-postgres/)
