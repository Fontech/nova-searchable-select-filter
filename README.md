# Searchable select filter for Laravel Nova.

## Installation

```bash
composer require fontech/nova-searchable-select-filter
```

## Usage

Just simply replace filter's component property to `searchable-select-filter`, that's all.

```php
namespace App\Nova\Filters;

use Laravel\Nova\Filters\Filter;

class MyAwesomeFilter extends Filter
{
    public $component = 'searchable-select-filter';
}
```
