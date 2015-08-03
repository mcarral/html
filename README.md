#StydeNet Html package

This package contains a collection of Laravel PHP classes designed to generate common HTML components, like:

* Menus
* Alert messages
* Form fields
* Collection of radios and checkboxes

This is an extension of the Laravel Collective [HTML package](https://github.com/laravelcollective/html) and will be very useful for you if you are working in a custom CMS, an admin panel or basically any project that needs to generate dynamic HTML.

## How to install

1. You can install this package through Composer. Do this either by running `composer require styde/html ~1.0` or adding `styde/html: ~1.0` to your `composer.json` and running `composer update`.

2. Next, add your new provider to the `providers` array of `config/app.php`

```
  'providers' => [
    // ...
    'Styde\Html\HtmlServiceProvider',
    // ...
  ],
```

3. Add the following middleware to the `$middleware` array in `app/Http/Kernel.php` **BEFORE** the `\App\Http\Middleware\EncryptCookies`: 

```
protected $middleware = [
    //...
    \Styde\Html\Alert\Middleware::class
    //...
];
```

This middleware is needed to persist the alert messages after each request is completed.

And this is it.

Please notice that the following aliases will be added automatically (you don't need to add them manually):

```
    'Alert'	=> Styde\Html\Facades\Alert,
    'Field'	=> Styde\Html\Facades\Field,
    'Menu'	=> Styde\Html\Facades\Menu,
```

If you plan to use the Access Handler as a standalone class, please add this alias:

```
  'aliases' => [
    // ...
    'Access' => Styde\Html\Facades\Access,
    // ...
  ],
```

Optionally, you may also run `php artisan vendor:publish --provider='Styde\Html\HtmlServiceProvider'` to publish the configuration file and explore at own will.

## Usage

Since this package is largely using [LaravelCollective/Html](https://github.com/laravelcollective/html), following their documentation is perfectly sufficient for base functionality such as forms and fields.

### Form Field builder

This component will allow you to generate complete form fields (not just inputs or selects) with one line of code.

If you have used the Laravel Collective HTML component before, you already know how to use the basics of this component; simply replace the alias “Form” for “Field”, for example, replace:

`{!! Form::text(‘name’, ‘value’, $attributes) !!}`

For this:

`{!! Field::text(‘name’, ‘value’, $attributes) !!}`

[Learn more about the field builder](docs/field-builder.md)

### Forms

This package extends the functionality of the Laravel Collective's Form Builder

#### novalidate

Deactivate the HTML5 validation in the configuration, ideal for local or development environments

```
return [
    'novalidate' => true
];
```

#### radios

Generate a collection of radios:

`{!! i.e. Form::radios('status', ['a' => 'Active', 'i' => 'Inactive']) !!}`

#### checkboxes

Generate a collection of checkboxes

```
$tags = [
    'php' => 'PHP',
    'python' => 'Python',
    'js' => 'JS',
    'ruby' => 'Ruby on Rails'
];

$checked = ['php', 'js'];
```

`{!! Form::checkboxes('tags', $tags, $checked) !!}`

[Learn more about the form builder](docs/form-builder.md)

### Alert messages

This component will allow you to generate complex alert messages.

```
        Alert::info('Your account is about to expire')
            ->details('Renew now to learn about:')
            ->items([
                'Laravel',
                'PHP,
                'And more',
            ])
            ->button('Renew now!', '#', 'primary');
```

`{!! Alert::render() !!}`

[Learn more about the alert component](docs/alert-messages.md)

### Menu generator

Menus are not static elements, you either need to mark the active section, translate items, generate dynamic URLs or show/hide options only for certain users.

So instead of adding a lot of HTML and Blade boilerplate code, you can use this component to generate dynamic menus styled for your current CSS framework.

To generate a menu simply add the following code in your layout's template:

`{!! Menu::make('items.here') !!}`

[Learn more about the menu generator](docs/menu-generator.md)

### HTML builder

This package extends the functionality of the Laravel Collective's HTML Builder

####Generate CSS classes:

`{!! Html::classes(['home' => true, 'main', 'dont-use-this' => false]) !!}`

Returns: ` class="home main"`.

[Learn more about the html builder](docs/html-builder.md)

### Helpers

In addition of using the facade methods `Alert::message` and `Menu::make`, you can use:

`alert('this is the message', 'type')`

`menu($items, $classes)`

## Access handler

Sometimes you want to show or hide certain menu items, form fields, etc. for certain users, with this component you can do it without the need of conditionals or a lot of boiler plate code, just pass one of the following options as a field's attributes or menu item values.

1. callback: should return true if access is granted, false otherwise.
2. logged: true: requires authenticated user, false: requires guest user.
3. role: true if the user has any of the required roles.

i.e.: 

`{!! Field::select('user_id', null, ['role' => 'admin'])`

[Learn more about the access handler](docs/access-handler.md)

## Themes

## Internationalization

## Readme in progress

This readme is currently in progress. However, you can find a lot of docblock comments if you dig into the source course, as well as unit tests in the spec directory.
