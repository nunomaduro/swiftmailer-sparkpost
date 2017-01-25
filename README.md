SwiftMailer SparkPost Transport
===============================

[![Build Status](https://scrutinizer-ci.com/g/f500/swiftmailer-sparkpost/badges/build.png?b=develop)](https://scrutinizer-ci.com/g/f500/swiftmailer-sparkpost/build-status/develop)
[![Code Coverage](https://scrutinizer-ci.com/g/f500/swiftmailer-sparkpost/badges/coverage.png?b=develop)](https://scrutinizer-ci.com/g/f500/swiftmailer-sparkpost/?branch=develop)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/f500/swiftmailer-sparkpost/badges/quality-score.png?b=develop)](https://scrutinizer-ci.com/g/f500/swiftmailer-sparkpost/?branch=develop)

A [SwiftMailer][1] transport implementation for the [SparkPost API][2].

It uses the official [SparkPost PHP library][3].

It adds support for some SparkPost specific features to SwiftMailer messages.

Installation
------------

```txt
composer require f500/swiftmailer-sparkpost
```

Usage
-----

```php
$transport = SwiftSparkPost\Transport::newInstance('API-KEY');
$mailer    = Swift_Mailer::newInstance($transport);

$message = SwiftSparkPost\Message::newInstance()
    ->setFrom('me@domain.com', 'Me')
    ->setTo(['john@doe.com' => 'John Doe', 'jane@doe.com'])
    ->setSubject('...')
    ->setBody('...')
    
    ->setCampaignId('...')
    ->setPerRecipientTags('john@doe.com', ['...'])
    ->setMetadata(['...' => '...'])
    ->setPerRecipientMetadata('john@doe.com', ['...' => '...'])
    ->setSubstitutionData(['...' => '...'])
    ->setPerRecipientSubstitutionData('john@doe.com', ['...' => '...'])
    ->setOptions(['...']);

$sent = $mailer->send($message);
```

#### Override recipients

You can override recipients by passing a configuration object.
The email address will be overridden, other properties (like name, tags, etc) won't be touched.

```php
$config = SwiftSparkPost\Configuration::newInstance()
    ->setRecipientOverride('override@domain.com');

$transport = SwiftSparkPost\Transport::newInstance('API-KEY', $config);
```

You can use Gmail style overrides, so that `john@doe.com` will become `override+john-doe-com@domain.com`.

```php
$config = SwiftSparkPost\Configuration::newInstance()
    ->setRecipientOverride('override@domain.com')
    ->setOverrideGmailStyle(true);

$transport = SwiftSparkPost\Transport::newInstance('API-KEY', $config);
```

License
-------

[Copyright 2017 Future500 B.V.][4]

[1]: http://swiftmailer.org
[2]: https://developers.sparkpost.com/api
[3]: https://github.com/SparkPost/php-sparkpost
[4]: https://github.com/f500/swiftmailer-sparkpost/blob/master/LICENSE
