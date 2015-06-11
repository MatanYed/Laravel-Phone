# Laravel Phone Validator

[![Build Status](https://travis-ci.org/Propaganistas/Laravel-Phone.svg)](https://travis-ci.org/Propaganistas/Laravel-Phone)
[![Code Climate](https://codeclimate.com/github/Propaganistas/Laravel-Phone/badges/gpa.svg)](https://codeclimate.com/github/Propaganistas/Laravel-Phone)
[![Latest Stable Version](https://poser.pugx.org/propaganistas/laravel-phone/v/stable)](https://packagist.org/packages/propaganistas/laravel-phone)
[![Total Downloads](https://poser.pugx.org/propaganistas/laravel-phone/downloads)](https://packagist.org/packages/propaganistas/laravel-phone)
[![License](https://poser.pugx.org/propaganistas/laravel-phone/license)](https://packagist.org/packages/propaganistas/laravel-phone)

Adds a phone validator to Laravel 4 and 5 based on the [PHP port](https://github.com/giggsey/libphonenumber-for-php) of [Google's libphonenumber API](https://code.google.com/p/libphonenumber/) by [giggsey](https://github.com/giggsey).

### Installation

1. In the `require` key of `composer.json` file add the following

    ```json
    "propaganistas/laravel-phone": "~2.0"
    ```

2. Run the Composer update command

    ```bash
    $ composer update
    ```

3. In your app config, add `'Propaganistas\LaravelPhone\LaravelPhoneServiceProvider'` to the end of the `$providers` array

    ```php
    'providers' => [
        'Illuminate\Foundation\Providers\ArtisanServiceProvider',
        'Illuminate\Auth\AuthServiceProvider',
        ...
        'Propaganistas\LaravelPhone\LaravelPhoneServiceProvider',
    ],
    ```

4. In your languages directory, add for each language an extra language line for the validator:

    ```php
"phone" => "The :attribute field contains an invalid number.",
    ```

### Usage

To validate a field using the phone validator, use the `phone` keyword in your validation rules array. The phone validator is able to operate in two ways.

- You either specify [*ISO 3166-1 alpha-2 compliant*](http://en.wikipedia.org/wiki/ISO_3166-1_alpha-2#Officially_assigned_code_elements) country codes yourself as parameters for the validator, e.g.:

    ```php
'phonefield'  => 'phone:US,BE',
    ```

  The validator will check if the number is valid in at least one of provided countries, so feel free to add as many country codes as you like.

- Or you don't specify any parameters but you plug in a dedicated country input field (keyed by *ISO 3166-1 compliant* country codes) to allow end users to supply a country on their own. The easiest method by far is to install the [CountryList package by monarobase](https://github.com/Monarobase/country-list). The country field has to be named similar to the phone field but with `_country` appended:

    ```php
'phonefield'          => 'phone',
'phonefield_country'  => 'required_with:phonefield',
    ```

  If using the CountryList package, you could then use the following snippet to populate a country selection list:

    ```php
Countries::getList(App::getLocale(), 'php', 'cldr'))
    ```

To specify constraints on the number type, just append the allowed types to the end of the parameters, e.g.:

```php
'phonefield'  => 'phone:US,BE,mobile',
```
The most common types are `mobile` and `fixed_line`, but feel free to use any of the types defined [here](https://github.com/giggsey/libphonenumber-for-php/blob/master/src/libphonenumber/PhoneNumberType.php).

### Display
Format a fetched phone value using the helper function:

```php
phone_format($phone_number, $country_code, $format = null)
```

The `$format` parameter is optional and should be a constant of `\libphonenumber\PhoneNumberFormat` (defaults to `\libphonenumber\PhoneNumberFormat::INTERNATIONAL`) 
