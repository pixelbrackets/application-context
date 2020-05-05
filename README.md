# Application Context

[![Packagist](https://img.shields.io/packagist/v/pixelbrackets/application-context.svg)](https://packagist.org/packages/pixelbrackets/application-context/)

The »Application Context« library allows to define the context of the current 
environment, in order to adapt configuration options depending on the stage 
an app is running on.

For example if an application is running in `Production` mode, it should
send mails and not create any log files. In `Development` mode however it 
should send mails, but to a different recipient, and create excessive log files.

```php
if($applicationContext->isDevelopment()) {
    // … do this in development mode only
}
```

[An environment variable sets the context](https://12factor.net/config),
whichs is retrieved using this class.

The main advantage of this approach is, that the code may stay the same on all
stages, but only configuration values may change depending of the context.

## Requirements

- PHP

## Installation

Packagist Entry https://packagist.org/packages/pixelbrackets/application-context/

## Source

https://gitlab.com/pixelbrackets/application-context/

Mirror https://github.com/pixelbrackets/application-context/

## Usage

1. Set the application context using an environment variable
   ```bash
   export APPLICATION_CONTEXT=Development/Local/JohnDoe
   ```
   or pass to the script like this
   ```bash
   APPLICATION_CONTEXT=Development/Local/JohnDoe php index.php
   ```
   💡 Hint: The package [helhum/dotenv-connector](https://packagist.org/packages/helhum/dotenv-connector)
   lets you store these variables in an `.env` file and automatically parse it.
1. Integrate the `ApplicationContext` class
   ```php
   $applicationContext = new \Pixelbrackets\ApplicationContext\ApplicationContext(getenv('APPLICATION_CONTEXT'));
   ```
1. Change code or configuration depending on the given context
   ```php
   $config['write-logs'] = true;
   $config['mail']['to'] = 'johndoe@example.com';
   if($applicationContext->isDevelopment()) {
       $config['mail']['to'] = 'test-test@localhost.tld';
   }
   ```
   Instead of changing single values the app could also load different files
   ```php
   $configFile = __DIR__ . '/Configuration/' . (string)$applicationContext . '.php';
   if (file_exists($configFile)) {
     require($configFile);
   }
   ```

## License

GNU General Public License version 2 or later

The GNU General Public License can be found at http://www.gnu.org/copyleft/gpl.html.

Attributions:

* This library is a standalone version of the Application Context in TYPO3 CMS
which derived from the TYPO3 Flow framework.

## Author

Dan Untenzu (<mail@pixelbrackets.de> / [@pixelbrackets](https://pixelbrackets.de))

## Changelog

[./CHANGELOG.md](./CHANGELOG.md)

## Contribution

This script is Open Source, so please use, patch, extend or fork it.
