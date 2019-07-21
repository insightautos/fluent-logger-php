# Fluent Logger PHP

**fluent-logger-php** is a PHP library to record events to fluentd from a PHP application. This fork adds compatibility with Fluent Bit until the [issue](https://github.com/fluent/fluent-logger-php/issues/61) is resolved in the upstream package.

## Requirements

- PHP 5.3 or higher
- fluentd v0.9.20 or higher
- [msgpack extension](https://github.com/msgpack/msgpack-php)

## Installation

### Using Composer

composer.json

```json
{
    "require": {
        "fluent/logger": "dev-master"
    },
    "repositories": [
        {
            "type": "vcs",
            "url": "git@github.com:tobiasmuehl/fluent-logger-php.git"
        }
    ]
}
```

# Backward Compatibility Changes

As of v1, all loggers but `FluentLogger` are removed.

[Monolog](https://github.com/Seldaek/monolog) is recommended in such use cases.

# Usage

## PHP side for fluent bit

```php
<?php

require_once __DIR__.'/vendor/autoload.php';

use Fluent\Logger\FluentLogger;
use Fluent\Logger\MsgpackPacker;

$logger = new FluentLogger("localhost","24224", [], new MsgpackPacker());
$logger->post("debug.test",array("hello"=>"world"));
```

## PHP side for fluentd

```php
<?php

require_once __DIR__.'/vendor/autoload.php';

use Fluent\Logger\FluentLogger;
$logger = new FluentLogger("localhost","24224");
$logger->post("debug.test",array("hello"=>"world"));
```


## Fluentd side

Use `in_forward`.

```aconf
<source>
  @type forward
</source>
```

## Fluent Bit side

Use `forward`.

```aconf
[INPUT]
    Name   forward
    Listen 0.0.0.0
    Port   24224
```

# Todos

* Stabilize method signatures.
* Improve performance and reliability.

# Restrictions

* Buffering and re-send support

PHP does not have threads. So, I strongaly recommend you use fluentd as a local fluent proxy.

````
apache2(mod_php)
fluent-logger-php
                 `-----proxy-fluentd
                                    `------aggregator fluentd
````

# License
Apache License, Version 2.0


# Contributors

* Daniele Alessandri
* Hiro Yoshikawa
* Kazuki Ohta
* Shuhei Tanuma
* Sotaro KARASAWA
* edy
* kiyoto
* sasezaki
* satokoma
* DQNEO
* Tobias Muehl
