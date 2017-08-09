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

Now when you call `Post::get()`, the query builder will be appended with the following:

- **name=ExampleName:** `->where('name', 'ExampleName')`
- **title=\*FooBar\*:** `->where('title', 'like', '%FooBar%')`
- **created_at<2017-08-08:** `->where('created_at', '<', '2017-08-08')`
