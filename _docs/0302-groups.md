---
title: Groups
toc: true
---
Groups are used to define content areas within your CMS which group related content together.

## Creating a Group

The first call to a group should the to define its size. This can either be done using the `size` method or one of the provided helper methods which provides a nicer, more fluent interface. Sizes are defined as columns in a 12 column grid.

```php
use Agencms\Core\Group;

// Using Size method
$group = Group::size($name, $size = 12);

// Helper shortcuts
$group = Group::tiny($name); // 2 Columns
$group = Group::small($name); // 4 Columns
$group = Group::medium($name); // 6 Columns
$group = Group::large($name); // 8 Columns
$group = Group::full($name); // 12 Columns
```

Basic Groups do not require a key to be set, however you can add a key through the `key` helper.

```php
$group->key('my-key');
```

### Repeater Groups

Agencms comes with Repeater field types which are easily created using the `repeater` method. Any Group can be turned into a repeater group this way, which will allow the user to create, edit, delete and re-order entries.

```php
$group->repeater('my-repeater-key');
```

When using repeater Groups, instead of adding `Fields` directly to the Group you should add sub-groups, which can contain any number of fields, through the `addGroup` method. Each sub-group will become a repeatable block.

```php
$group->repeater('my-repeater-key')->addGroup(...Groups $group);
```

### Adding fields to a Group

To define what content should be rendered inside a group, call the `addField` method. As with Groups, you can supply multiple fields as comma separated parameters ot make multiple calls to the `addField` method.

```php
$group->addField(...Field $field);
```

## Adding a Group to a Route

To actually include a Group in your CMS, simply append them to your `registerRoute` call. You can either add multiple groups in the same call or make multiple calls to `addGroup`.

```php
$route = Route::init('my-slug', 'My Route')->addGroup(...Group $group);
```
