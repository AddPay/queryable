# Queryable
Query parameter based model queries for Laravel 5 by Stephen Lake

## Installation

Require the package:

`composer require closurecode/queryable`

and you're good to go.

## Basic Usage
Import the `Queryable` trait and define the allowed queryable columns:

```php
use ClosureCode\Queryable\Traits\Queryable;
use Illuminate\Database\Eloquent\Model;

class Post extends Model
{
  protected $queryable = [
     'id',
     'title',
     'author_id',
     'created_at',
     'updated_at',
  ];
}
```

Define an endpoint to fetch `Posts`'s and attach your desired query parameters.

`https://yourdomain.yourtld/posts?name=ExampleName&title=*FooBar*&created_at<2017-08-08`

Now when you call `Post::get()`, the following will be prepended to the query builder:

```php
Posts::where('name', 'ExampleName')
     ->where('title', 'like', '%FooBar%')
     ->where('created_at', '<', '2017-08-08')
     ->get()
```

## Available Operators

### Where Equal
`?column=value`

### Where Not Equal
`?column!=value`

### Where Greater Than
`?column>value`

### Where Greater Than or Equal
`?column>=value`

### Where Less Than
`?column<=value`

### Where Less Than or Equal
`?column<=value`

### Where In
`?column~value`

### Where Not In
`?column!~value`

### Where Like
`?column=*value*`

### Order By
`?orderBy=column,asc|desc`
