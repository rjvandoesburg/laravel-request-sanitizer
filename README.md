# Laravel Request Sanitizer

![Run Tests](https://github.com/ArondeParon/laravel-request-sanitizer/workflows/Run%20Tests/badge.svg?branch=master)
[![Total Downloads][ico-downloads]][link-downloads]

The `arondeparon/laravel-request-sanitizer` package provides a fluent interface to sanitize form requests before validating them.

## Why should I use this package?

Often, validating your request is not enough. The request sanitizer allows you to easily 
manipulate your form data before passing it to the validator. You can start using it in a matter
of minutes and it is fully compatible with Laravel's `FormRequest` object.

## Table of Contents

  * [How to use](#how-to-use)
  * [Installing](#installing)
  * [Usage](#usage)
  * [Predefined Sanitizers](#predefined-sanitizers)
    + [FilterVars usage](#filtervars-usage)
  * [Writing your own Sanitizer](#writing-your-own-sanitizer)
  * [Testing](#testing)
  * [Credits](#credits)
  * [License](#license)

## How to use

Syntax is similar to the way `rules` are added to a [Form Request](https://laravel.com/docs/master/validation#form-request-validation).

```php
class StoreCustomerInformationRequest extends FormRequest
{
     use SanitizesInputs;
     
     protected $sanitizers = [
        'lastname' => [
            Capitalize::class,
        ],
        'mobile_phone' => [
            RemoveNonNumeric::class
        ],
     ];
}
```

## Installing

`composer require arondeparon/laravel-request-sanitizer`

## Usage

- Add the `SanitizesInputs` trait to your form request.
- Write your own sanitizers or use one of the supplied sanitizers and add them to the `$sanitizers`
property of your form request.
- Your request data will now be sanitized before being validated.

## Predefined Sanitizers

- [`Trim`](./src/Sanitizers/Trim.php) - simple PHP `trim()` implementation
- [`TrimDuplicateSpaces`](./src/Sanitizers/TrimDuplicateSpaces.php) replaces duplicate spaces with a single space.
- [`RemoveNonNumeric`](./src/Sanitizers/RemoveNonNumeric.php) - removes any non numeric character
- [`Capitalize`](./src/Sanitizers/Capitalize.php) - capitalizes the first character of a string
- [`Uppercase`](./src/Sanitizers/Uppercase.php) - converts a string to uppercase
- [`Lowercase`](./src/Sanitizers/Lowercase.php) - converts a string to lowercase
- [`FilterVars`](./src/Sanitizers/FilterVars.php) - simple PHP `filter_var` sanitizer
- [`CarbonDate`](./src/Sanitizers/CarbonDate.php) - cast a string to a Carbon object
- Contributions are appreciated!

### FilterVars usage
The FilterVars sanitizer acts as a wrapper of the default PHP `filter_var` function. 
It accepts the same (optional) parameters as the original function. 
Both parameters can be either an `array` or `string` type:
```php
 {
    protected $sanitizers = [
        'last_name' => [
            FilterVars::class => [
                'filter' => FILTER_SANITIZE_STRING,
                'options' => FILTER_FLAG_STRIP_BACKTICK
            ]
        ]
    ];
 }
```
For more information on filter_vars please refer to https://www.php.net/manual/en/function.filter-var.php.

## Writing your own Sanitizer

Writing your own sanitizer can be done by implementing the `Sanitizer` interface, which requires only
one method.

```php
interface Sanitizer
 {
     public function sanitize($input);
 }
```



## Testing

`$ composer test`

## Credits

- [Aron Rotteveel](https://github.com/arondeparon)
- [All Contributors](../../contributors)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.

[link-downloads]: https://packagist.org/packages/arondeparon/laravel-request-sanitizer
[ico-downloads]: https://img.shields.io/packagist/dt/arondeparon/laravel-request-sanitizer.svg?style=flat-square
