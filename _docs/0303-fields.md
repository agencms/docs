---
title: Fields
toc: true
---
This is where the actual content fields are created for your CMS. Whilst this is probably the most comprehensive Class of Agencms, they are super flexible and the fluent api makes it super easy to define them in a natural language way.

## Creating a Field

The first call to a field should the to define its type. This can either be done using the `init` method or one of the provided helper methods (recommended) which provides a nicer, more fluent interface.

```php
use Agencms\Core\Field;

// Using init method
$field = Field::init($type = 'string', $key, $name);

// Helper shortcuts
$field = Field::$type($key, $name);
```

### Field sizes

Similar to Groups, all fields can be optionally sized using the provided helper methods to a 12 column grid. By default fields will be full-width.

```php
// Using the size method
$field->size($columns = 12);

// Using the provided helpers
$field->tiny(); // 2 columns
$field->small(); // 4 columns
$field->medium(); // 6 columns
$field->large(); // 8 columns
$field->full(); // 12 columns
```

### Required/Optional fields

By default fields are optional but they can easily specify this explicitly.

```php
$field->optional();

$field->required();
```

### Read-only fields

You can mark fields as read-only. E.g. `id`.

```php
$field->readonly(true);

$field->readonly(false); // default
```

### Linked fields

Agencms provides a way of linking fields. That mean when data is entered into field A, field B will automatically populate with the update content. A typicaly use-case for this is converting a `Name` field into a `slug` field.

```php
$field->link($to = 'target-field');
```

## List columns

When adding fields to a Collection group, you can specify if a field should be included in the list view with an optional position. The dafult position value is 50 to provide a lot of flexibility of arranging items without needing to specify a value for each list item.

```php
$field->list($position = 50)
```

## Field types

|Type|Description|
|---|---|
|string|Text field (singleline/multiline)|
|number|Text field accepting a numeric value|
|boolean|True/false (yes/no) toggle|
|date|Date selector|
|select|Selection control (Dropdown/Checkbox)|
|related|Lookup of a relationship model (List)|

Each field type is described in more detail with a number of additional chainable methods to specify additional required/optional data.

### Text Fields (string)

By default string fields are single line text input fields, however you can easily make a string field multi-line and optionally specify the number of rows.

```php
$field = Field::string('my-key', 'Field Name');

// Single line (default)
$field = Field::string('my-key', 'Field Name')->singleline();

// Multi line
$field = Field::string('my-key', 'Field Name')->multiline($rows = 5);
```

Client side validation rules can be provided for minimum and maximum input length of a text field. This works on both single and multiline fields.

```php
$field->minLength(3)->maxLength(50);
```

#### Slug strings

Automatically formats any input to slug format

```php
$field->slug(); // my-text-value
```

### Number Fields

Creates a text input field which will only accept a numeric value

```php
$field->number('my-key', 'Field Name');
```

### Boolean Fields (Toggle Switch)

Creates a toogle On/Off switch

```php
$field->boolean('my-key', 'Field Name');
```

### Date Fields

A calendar style date selector

```php
$field->date('my-key', 'Field Name');
```

### Image Fields

Image fields allow the user to select a single image from the Library, which will return a string to the Api. The desired image size and resizing method can be specified, although support typically will depend on the media library driver used.

```php
$field->image('my-key', 'Field Name');

// Specify the image ratio or size
$field->ratio($with = 800, $height = 600, $resize = true);
```

### Select Fields (Dropdown/Checkbox)

Select fields can be used in a variety of different ways to provide a nice user experiencing when selecting from a choice of options. The default implementation is a Dropdown menu with type to search/auto-complete.

```php
$field->select('my-key', 'Field Name');

// Specify the select method
$field->dropdown();

$field->checkbox();

$field->tags(); // Allows custom values to be entered

// Single or multiple values allowed
$field->single();

$field->multiple();
```

### Adding Options to Select menus

There is a special Options class available to define key-value pairs or you can simply supply an array of values to be used for seeding the select menu.

```php
// Simple array of choices
$field->addOptions([
    'Tea',
    'Coffee',
    'Water',
]);

// Using the Option class
$field->addOption(
    Option::init('tea', 'A cup of Tea'),
    Option::init('coffee', 'A cup of Coffee'),
    Option::init('water', 'Just a glass of water')
);
```

### Relationship Fields

Relationships can be used to allow the user to select entries from another registered Route by means of a searchable list. The Route key is used to specify the related model.

```php
$field->related('my-key', 'Field Name');

// Specify the related model
$field->model(Relationship::make('model-key'));

// Specify single or multiple choice
$field->single();
$field->multiple();
```
