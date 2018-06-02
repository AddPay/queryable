<center>
 <img src="https://raw.githubusercontent.com/closurecode/assets/master/logo_dark.png" style="align: center; width: 200px;" width="200">
</center>

# Queryable
Query parameter based model queries for [Laravel 5](https://www.laravel.com) by [Stephen Lake](https://stephenlake.github.io).

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

  use Queryable;
  
  protected $queryable = [
     'id',
     'title',
     'author_id',
     'created_at',
     'updated_at',
  ];
}
```

Define an endpoint to fetch `Post`'s and attach your desired query parameters.

`/posts?name=ExampleName&title=*FooBar*&created_at<2017-08-08`

Now when you call `Post::get()`, the relevant queries will autmatically be prepended to the query builder:

`Post::get()` becomes:

```php
Posts::where('name', 'ExampleName')
     ->where('title', 'like', '%FooBar%')
     ->where('created_at', '<', '2017-08-08')
     ->get();
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

## Important Notes

### Chaining
You can chain queries using ampersands (`&`) like so:

`?name=*test*&created_at<2017&orderBy=created_at,desc`

### Modifying Queryables
If you need to change the allowed queryable columns or prefer not to define them on the model directly, you can call `setQueryable($columns)` or `addQueryable($column)` on the model. To clear all queryables, call `clearQueryable()`.

`Post::setQueryable(['name',])->get();`

`Post::clearQueryable()->addQueryable('name')->addQueryable('type')->get();`

### Model Ordering
An additional `orderBy` query parameter is available to assist in the ordering of the result:
`?orderBy=column,asc|desc`

The `column` must be defined as an allowed `$queryable` field, otherwise the `orderBy` parameter will be ignored.

### Hidden Fields
Any fields defined within the model's `$hidden` variable will be ignored from queries.
