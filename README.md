## Attesion
This package is copied from laravel-shift/blueprint, and i make a littil modify, hahaha

## Requirements
Blueprint requires a Laravel application running version 6.0 or higher.

## Installation
You can install Blueprint via composer using the following command:

```sh
composer require --dev mjczz/blueprint_ex
```

Blueprint will automatically register itself using [package discovery](https://laravel.com/docs/packages#package-discovery).

## Basic Usage
Blueprint comes with a set of artisan commands. The one you'll use the most is the `blueprint:build` command to generate the Laravel components:

```sh
php artisan blueprint:build [draft]
```

The _draft_ file contains a [definition of the components](https://blueprint.laravelshift.com/docs/generating-components/) to generate.


Let's review the following, example draft file to generate some _blog_ components:

```yaml
models:
  Post:
    title: string:400
    content: longtext
    published_at: nullable timestamp
    author_id: id:user

controllers:
  Post:
    index:
      query: all
      render: post.index with:posts

    store:
      validate: title, content, author_id
      save: post
      send: ReviewPost to:post.author.email with:post
      dispatch: SyncMedia with:post
      fire: NewPost with:post
      flash: post.title
      redirect: post.index
```

From these simple 20 lines of YAML, Blueprint will generate all of the following Laravel components:

- A _model_ class for `Post` complete with `fillable`, `casts`, and `dates` properties, as well as relationships methods.
- A _migration_ to create the `posts` table.
- A [_factory_](https://laravel.com/docs/database-testing) intelligently setting columns with fake data.
- A _controller_ class for `PostController` with `index` and `store` actions complete with code generated for each [statement](https://blueprint.laravelshift.com/docs/controller-statements/).
- _Routes_ for the `PostController` actions.
- A [_form request_](https://laravel.com/docs/validation#form-request-validation) of `StorePostRequest` validating `title` and `content` based on the `Post` model definition.
- A _mailable_ class for `ReviewPost` complete with a `post` property set through the _constructor_.
- A _job_ class for `SyncMedia` complete with a `post` property set through the _constructor_.
- An _event_ class for `NewPost` complete with a `post` property set through the _constructor_.
- A _Blade template_ of `post/index.blade.php` rendered by `PostController@index`.

_**Note:** This example assumes features within a default Laravel application such as the `User` model and `app.blade.php
 layout. Otherwise, the generated test may have failures._

## Documentation
Browse the [Blueprint Docs](https://blueprint.laravelshift.com/) for full details on [defining models](https://blueprint.laravelshift.com/docs/defining-models/), [defining controllers](https://blueprint.laravelshift.com/docs/defining-controllers/), [advanced configuration](https://blueprint.laravelshift.com/docs/advanced-configuration/), and [extending Blueprint](https://blueprint.laravelshift.com/docs/extending-blueprint/).
