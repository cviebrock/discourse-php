# Discourse Single-Sign-On Helper for PHP

This is a small class to help with providing an SSO source for Discourse forums. It provides 3 helper functions for validating incoming requests, extracting nonce, and building the returning query string.

For more information on the SSO settings in Discourse, visit <https://meta.discourse.org/t/official-single-sign-on-for-discourse/13045>

> Original code from Johan Jatko: https://github.com/ArmedGuy/discourse_sso_php


## Installation

The package is registered at Packagist as [cviebrock/discourse-php](https://packagist.org/packages/cviebrock/discourse-php) and can be installed using [composer](http://getcomposer.org/):

```
composer require "cviebrock/discourse-php:0.9.*"
```


## Usage

```php
$sso = new Cviebrock\DiscoursePHP\SSOHelper();

// this should be the same in your code and in your Discourse settings:
$secret = 'super_secret_sso_key';
$sso->setSecret( $secret );

// load the payload passed in by Discourse
$payload = $_GET['sso'];
$signature = $_GET['sig'];

// validate the payload
if (!($sso->validatePayload($payload,$signature))) {
    // invaild, deny
    header("HTTP/1.1 403 Forbidden");
    echo("Bad SSO request");
    die();
}

$nonce = $sso->getNonce($payload);

// Insert your user authentication code here ...

// Required and must be unique to your application
$userId = '...';

// Required and must be consistent with your application
$userEmail = '...';

// Optional - if you don't set these, Discourse will generate suggestions
// based on the email address

$extraParameters = array(
    'username' => $userUsername,
    'name'     => $userFullName
);

// build query string and redirect back to the Discourse site
$query = $sso->getSignInString($nonce, $userId, $userEmail, $extraParameters);
header('Location: http://discourse.example.com/session/sso_login?' . $query);
exit(0);
```


## Bugs, Suggestions and Contributions

Please use Github for bugs, comments, suggestions.

1. Fork the project.
2. Create your bugfix/feature branch and write your (well-commented) code.
3. Commit your changes and push to your repository.
4. Create a new pull request against this project's `master` branch.



## Copyright and License

**discourse-php** was written by Colin Viebrock and released under the MIT License. See the LICENSE file for details.

Copyright 2015 Colin Viebrock
