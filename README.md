API Trello
========================

A simple Object Oriented wrapper for the Trello API, written in PHP7.4.

Uses [Trello API v1](https://trello.com/docs/index.html). The object API is very similar to the RESTful API.

## Features

* Follows PSR-0 conventions and coding standards: autoload friendly
* Light and fast thanks to lazy loading of API classes
* Extensively tested
* Ready for Symfony 5

## Requirements

* PHP >= 7.4 with [cURL](http://php.net/manual/en/book.curl.php) extension,
* [Guzzle](https://github.com/guzzle/guzzle) library,
* (optional) [PHPUnit](https://phpunit.de) to run tests.

## Installation

The recommended way is using [composer](http://getcomposer.org):

```bash
$ composer require albreis/trello
```

## Exemplo

```php
use Trello\Client;

$client = new Client();
$client->authenticate('api_key', 'token', Client::AUTH_URL_CLIENT_ID);

$boards = $client->api('member')->boards()->all();
```

The `$client` object gives you access to the entire Trello API.

## Advanced usage with the Trello manager

This package includes a simple model layer above the API with a nice chainable API allowing following manipulation of Trello objects:

```php
use Trello\Client;
use Trello\Manager;

$client = new Client();
$client->authenticate('api_key', 'token', Client::AUTH_URL_CLIENT_ID);

$manager = new Manager($client);

$card = $manager->getCard('547440ad3f8b882bc11f0497');

$card
    ->setName('Test card')
    ->setDescription('Test description')
    ->save();
```

## Dispatching Trello events to your app

The service uses the [Symfony EventDispatcher](https://github.com/symfony/EventDispatcher) component to dispatch events occuring on incoming webhooks.

Take a look at the [Events](https://github.com/albreis/albreis/blob/master/lib/Trello/Events.php) class constants for names and associated event classes.

```php
use Trello\Client;
use Trello\Service;
use Trello\Events;

$client = new Client();
$client->authenticate('api_key', 'token', Client::AUTH_URL_CLIENT_ID);

$service = new Service($client);

// Bind a callable to a given event...
$service->addListener(Events::BOARD_UPDATE, function ($event) {
    $board = $event->getBoard();

    // do something
});

// Check if the current request was made by a Trello webhook
// This will dispatch any Trello event to listeners defined above
$service->handleWebhook();
```

## Documentation
* Package [API](docs/Api/Index.md)
* Official [API documentation](https://trello.com/docs/index.html).

## Contributing

Feel free to make any comments, file issues or make pull requests.

## License

`albreis/trello` is licensed under the MIT License - see the LICENSE file for details

## Credits

- Forked from [cdaguerre/php-trello-api](https://github.com/cdaguerre/php-trello-api)
- Largely inspired by the excellent [php-github-api](https://github.com/KnpLabs/php-github-api) developed by the guys at [KnpLabs](http://knplabs.fr)
- Thanks to Trello for the API and documentation.
