Application Context
===================

The »Application Context« library allows to define the context of the current 
environment, in order to adapt configuration options depending on the stage 
an app is running on.

For example if an application is running in `Production` mode, it should
send mails and not create any log files. In `Development` mode however it 
should send mails, but to a different recipient, and create excessive log files.

The context is set using an environment variable and retrieved using this class.

The main advantage of this approach is, that the code may stay the same on all
stages, but only configuration values may change depending of the context.

Usage
=====

1. Set the application context using an environment variable
   ```bash
   export APPLICATION_CONTEXT=Development/Local/JohnDoe
   ```
   or pass to the script like this
   ```bash
   APPLICATION_CONTEXT=Development/Local/JohnDoe php index.php
   ```
1. Integrate the `ApplicationContext` class
   ```php
   $applicationContext = new \Pixelbrackets\ApplicationContext\ApplicationContext(getenv('APPLICATION_CONTEXT'));
   ```
1. Change configuration depending on the given context
   ```php
   $config['write-logs'] = true;
   $config['mail']['to'] = 'johndoe@example.com'
   if($applicationContext->isDevelopment()) {
       $config['write-logs'] = true;
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

Credits
=======

This library is a standalone version of the Application Context in TYPO3 CMS
which derived from the TYPO3 Flow framework.
