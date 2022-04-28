# Livewire DataTables

Livewire DataTables components for back-end. Modular, easy to use, with tons of features.

Inspired by [Caleb's](https://github.com/calebporzio) [Livewire Screencasts](https://laravel-livewire.com/screencasts), dedicated to my friend [Bardh](https://github.com/bardh7).

## Installation

You can install the package via composer:

```bash
composer require adaptech/livewire-datatables
```

You can publish the config file with:
```bash
php artisan vendor:publish --provider="Amirami\LivewireDataTables\LivewireDataTablesServiceProvider" --tag="livewire-datatables-config"
```

This is the contents of the published config file:

```php
return [

    'multi_column_sorting' => false,

    'row_caching' => false,

];
```

This means that by default these two options are disabled. But you can enable them for individual components.

## Usage

After creating your Livewire component, extend the component class with `Amirami\LivewireDataTables\DataTable`. The `DataTable` abstract class will ask you to implement `getQueryProperty` method. This is a computed property in Livewire which needs to return and instance of `Illuminate\Database\Eloquent\Builder` or `\Illuminate\Database\Eloquent\Relations\Relation`.

This is the part where you will build the base of your query, with filters.

```php
namespace App\Http\Livewire;

use App\Models\Post;
use Amirami\LivewireDataTables\DataTable;
use Illuminate\Database\Eloquent\Builder;

class PostsIndex extends DataTable
{
    public function getQueryProperty(): Builder
    {
        return Post::query();
    }

    public function render()
    {
        $posts = $this->entries;

        return view('livewire.posts-index', compact('posts'));
    }
}
```

This is the most basic component without any datatable features. Although it is totally fine to use the datatable without any features, it kinda beats the purpose of this package. Now let's get onto the many features this package provides.

### Pagination

Pagination offers exactly the same features as Livewire's default one. It actually extends it. The only reason you'll have to use the pagination provided by this package is because Livewire's default one doesn't play nice with our other features.

```php
namespace App\Http\Livewire;

use Amirami\LivewireDataTables\DataTable;
use Amirami\LivewireDataTables\Traits\WithPagination;

class PostsIndex extends DataTable
{
    use WithPagination;
}
```

You can configure how many result you want to see per page as well. If not defined the paginator will pull the default number from the model class.

```php
namespace App\Http\Livewire;

use Amirami\LivewireDataTables\DataTable;
use Amirami\LivewireDataTables\Traits\WithPagination;

class PostsIndex extends DataTable
{
    use WithPagination;
    
    // As a property.
    public $perPage = 20;
    
    // Or as a method.
    public function getPerPage(): ?int
    {
        return 42;
    }
}
```

For everything else about pagination check out the [Livewire's official documentation](https://laravel-livewire.com/docs/2.x/pagination).

### Searching

If you want to use rows searching you have to use `WithSearching` trait, bind an input to a component property and define searchable columns. Consider the rest handled. This is how easy it is:

```php
namespace App\Http\Livewire;

use Amirami\LivewireDataTables\DataTable;
use Amirami\LivewireDataTables\Traits\WithSearching;

class PostsIndex extends DataTable
{
    use WithSearching;
    
    public $searchableColumns = [
        'title',
        'content',
    ];
    
    public function render()
    {
        $posts = $this->entries;

        return view('livewire.posts-index', compact('posts'));
    }
}
```

```html
<input wire:model="search" type="text">

<table>
    <thead>
        <tr>
            <th>Title</th>
            <th>Excerpt</th>
            <th>Author</th>
            <th>Status</th>
        </tr>
    </thead>
    <tbody>
        @foreach($posts as $post)
            <tr>
                <th>{{ $post->title }}</th>
                <th>{{ $post->excerpt }}</th>
                <th>{{ $post->user->name }}</th>
                <th>{{ $post->status() }}</th>
            </tr>
        @endforeach
    </tbody>
</table>
```

### Sorting

### Filtering

### Row Caching

## Planned Features

* Searching, sorting and filtering by relationship fields
* Bulk Actions
* Row Grouping
* Front-end components (will most-likely be a separate package)

## Showcase

Check out cool datatables built with `livewire-datatables` [here](https://github.com/amiranagram/livewire-datatables/discussions/categories/show-and-tell), and don't forget to share your own 🙌.

## Testing

```bash
composer test
```

## Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information on what has changed recently.

## Contributing

Please see [CONTRIBUTING](.github/CONTRIBUTING.md) for details.

## Security Vulnerabilities

Please review [our security policy](../../security/policy) on how to report security vulnerabilities.

## Credits

- [Amir Rami](https://github.com/amiranagram)
- [All Contributors](../../contributors)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
