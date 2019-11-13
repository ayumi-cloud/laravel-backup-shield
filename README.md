# Laravel Backup Shield

[![Latest Version on Packagist][ico-version]][link-packagist]
[![Software License][ico-license]](LICENSE.md)
[![Build Status][ico-travis]][link-travis]
[![Scrutinizer Score][ico-scrutinizer]][link-scrutinizer]

![backup-shield](https://user-images.githubusercontent.com/907114/40585078-b42b31ba-61ac-11e8-9db6-b5497e156f5a.png)

## Secure your backups

**This package helps you encrypt and password-protect your backups taken with [Spatie's](https://github.com/spatie) fantastic [spatie/laravel-backup](https://github.com/spatie/laravel-backup)-package.**

Backup Shield simply listens for when the .zip-file generated by Laravel-backup is done, grabs it and applies your password and encryption of your liking.

*Using older versions of Laravel? Check out the [v1 branch](https://github.com/olssonm/laravel-backup-shield/tree/v1) (for Laravel 5.2) and the [v2 branch](https://github.com/olssonm/laravel-backup-shield/tree/v2).*

## Installation

```bash
composer require olssonm/laravel-backup-shield
```

Requires `PHP: "^7.1"` and `laravel/framework: "^5.8"`. Compatible with Laravel 6.

Please note that `spatie/laravel-backup: "^6"`  and `laravel/framework: "^6.0"` requires PHP 7.2.

## Configuration

Publish your configuration using `php artisan vendor:publish` and select `BackupShieldServiceProvider`. Or directly via ```php artisan vendor:publish --provider="Olssonm\BackupShield\BackupShieldServiceProvider"```.

You only have the ability to set two different options; password and encryption.

```php
// Default configuration; backup-shield.php
return [
    'password' => env('APP_KEY'),
    'encryption' => \Olssonm\BackupShield\Encryption::ENCRYPTION_DEFAULT
];
```

#### Password

Your password (*duh*). The default is the application key (`APP_KEY` in your .env-file). You might want to set something more appropriate. Remember to use long strings and to keep your password safe – without it you will never be able to open your backup.

Set to `NULL` if you want to keep your backup without a password.

#### Encryption

Set your type of encryption. Available options are:

`\Olssonm\BackupShield\Encryption::ENCRYPTION_DEFAULT` (PHP < 7.2: PKWARE/ZipCrypto, PHP >= 7.2: AES 128)  
`\Olssonm\BackupShield\Encryption::ENCRYPTION_WINZIP_AES_128` (AES 128)  
`\Olssonm\BackupShield\Encryption::ENCRYPTION_WINZIP_AES_192` (AES 192)  
`\Olssonm\BackupShield\Encryption::ENCRYPTION_WINZIP_AES_256` (AES 256)  

**Important information regarding encryption**

Using the `ENCRYPTION_DEFAULT` (PKWARE/ZipCrypto) crypto gives you the best portability as most operating systems can natively unzip the file – however, ZipCrypto might be weak. The Winzip AES-methods on the other hand might require a separate app and/or licence to be able to unzip depending on your OS; suggestions for macOS are [Keka](http://www.kekaosx.com/en/) and [Stuffit Expander](https://itunes.apple.com/us/app/stuffit-expander-16/id919269455).

Also to note is that when zipping very large files ZipCrypto might be very inefficient as the entire data-set will have to be loaded into memory to perform the encryption, if the zipped file's content is bigger than your available RAM you *will* run out of memory.

#### Differences when using PHP < 7.2 and PHP >= 7.2

Since PHP 7.2 (coupled with zip-extension >= 1.14.0) PHP can natively password-protect .zip-files via the ZipArchive-methods. If these conditions are met, ZipArchive is used. Else, the package will automatically use the [nelexa/zip](https://github.com/Ne-Lexa/php-zip)-package.

The former might be less memory-intensive.

## Testing

``` bash
$ composer test
```

or

``` bash
$ phpunit
```

## License

The MIT License (MIT). Please see the [LICENSE.md](LICENSE.md) for more information.

© 2019 [Marcus Olsson](https://marcusolsson.me).

[ico-version]: https://img.shields.io/packagist/v/olssonm/laravel-backup-shield.svg?style=flat-square
[ico-license]: https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square
[ico-travis]: https://img.shields.io/travis/olssonm/laravel-backup-shield/master.svg?style=flat-square
[ico-downloads]: https://img.shields.io/packagist/dt/olssonm/laravel-backup-shield.svg?style=flat-square
[ico-scrutinizer]: https://img.shields.io/scrutinizer/g/olssonm/laravel-backup-shield.svg?style=flat-square
[link-packagist]: https://packagist.org/packages/olssonm/laravel-backup-shield
[link-travis]: https://travis-ci.org/olssonm/laravel-backup-shield
[link-scrutinizer]: https://scrutinizer-ci.com/g/olssonm/laravel-backup-shield
