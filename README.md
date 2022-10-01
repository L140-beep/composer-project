# composer-project
## 1. Используется БД для хранения данных.

illuminate/database

  The Illuminate Database component is a full database toolkit for PHP, providing an expressive query builder, ActiveRecord style ORM, and schema builder. 
It currently supports MySQL, Postgres, SQL Server, and SQLite. 
It also serves as the database layer of the Laravel PHP framework.

```php
//First, create a new "Capsule" manager instance. Capsule aims to make configuring the library for usage outside 
//of the Laravel framework as easy as possible.

use Illuminate\Database\Capsule\Manager as Capsule;

$capsule = new Capsule;

$capsule->addConnection([
    'driver' => 'mysql',
    'host' => 'localhost',
    'database' => 'database',
    'username' => 'root',
    'password' => 'password',
    'charset' => 'utf8',
    'collation' => 'utf8_unicode_ci',
    'prefix' => '',
]);

// Set the event dispatcher used by Eloquent models... (optional)
use Illuminate\Events\Dispatcher;
use Illuminate\Container\Container;
$capsule->setEventDispatcher(new Dispatcher(new Container));

// Make this Capsule instance available globally via static methods... (optional)
$capsule->setAsGlobal();

// Setup the Eloquent ORM... (optional; unless you've used setEventDispatcher())
$capsule->bootEloquent();

```

## 2. Используется кеш в памяти для временного хранения сложных запросов к БД.

watson/rememberable

  Rememberable is an Eloquent trait for Laravel that adds remember() query methods. 
This makes it super easy to cache your query results for an adjustable amount of time.

```php
// Get a the first user's posts and remember them for a day.
User::first()->remember(now()->addDay())->posts()->get();

// You can also pass the number of seconds if you like (before Laravel 5.8 this will be interpreted as minutes).
User::first()->remember(60 * 60 * 24)->posts()->get();

```

## 3. Формируются XLS-отчеты на основе данных.

phpoffice/phpspreadsheet

  PhpSpreadsheet is a library written in pure PHP and offers a set of classes 
that allow you to read and write various spreadsheet file formats such as Excel and LibreOffice Calc.

## 4. Формируются PDF-документы на основе данных.

tecnickcom/tcpdf

PHP library for generating PDF documents on-the-fly.

## 5. Отправляются SMS-сообщения для верификации пользователей.

twilio/sdk

  A PHP wrapper for Twilio's API

```php
// Send an SMS using Twilio's REST API and PHP
<?php
$sid = "ACXXXXXX"; // Your Account SID from www.twilio.com/console
$token = "YYYYYY"; // Your Auth Token from www.twilio.com/console

$client = new Twilio\Rest\Client($sid, $token);
$message = $client->messages->create(
  '8881231234', // Text this number
  [
    'from' => '9991231234', // From a valid Twilio number
    'body' => 'Hello from Twilio!'
  ]
);

print $message->sid;
```

## 6. Отправляются E-mail-уведомления и рассылки для пользователей.

symfony/mailer
 
  The Mailer component helps sending emails.

```php
use Symfony\Component\Mailer\Transport;
use Symfony\Component\Mailer\Mailer;
use Symfony\Component\Mime\Email;

$transport = Transport::fromDsn('smtp://localhost');
$mailer = new Mailer($transport);

$email = (new Email())
    ->from('hello@example.com')
    ->to('you@example.com')
    //->cc('cc@example.com')
    //->bcc('bcc@example.com')
    //->replyTo('fabien@example.com')
    //->priority(Email::PRIORITY_HIGH)
    ->subject('Time for Symfony Mailer!')
    ->text('Sending emails is fun again!')
    ->html('<p>See Twig integration for better HTML integration!</p>');

$mailer->send($email);
```

## 7. Используется облачное хранилище AWS S3 или Windows Azure Blob для статичных файлов.

aws/aws-sdk-php

  The AWS SDK for PHP makes it easy for developers to access Amazon Web Services in their PHP code, 
and build robust applications and software using services like Amazon S3, Amazon DynamoDB, Amazon Glacier, etc. 

```php
// Create an Amazon S3 client
// Require the Composer autoloader.
require 'vendor/autoload.php';

use Aws\S3\S3Client;

// Instantiate an Amazon S3 client.
$s3 = new S3Client([
    'version' => 'latest',
    'region'  => 'us-west-2'
]);
// Upload a file to Amazon S3
// Upload a publicly accessible file. The file size and type are determined by the SDK.
try {
    $s3->putObject([
        'Bucket' => 'my-bucket',
        'Key'    => 'my-object',
        'Body'   => fopen('/path/to/file', 'r'),
        'ACL'    => 'public-read',
    ]);
} catch (Aws\S3\Exception\S3Exception $e) {
    echo "There was an error uploading the file.\n";
}
```

## 8. Используется интеграция со службой доставки для расчета стоимости (например, PickPoint или Почта России)

domatskiy/pickpoint

PickPoint API

## 9. Используется интеграция с социальными сетями для авторизации пользователей.

hybridauth/hybridauth

  Hybridauth enables developers to easily build social applications and tools to engage websites visitors 
and customers on a social level that starts off with social sign-in 
and extends to social sharing, users profiles, friends lists, activities streams, status updates and more.

```php
$config = [
    'callback' => 'https://example.com/path/to/script.php',
    'keys' => [
        'key' => 'your-twitter-consumer-key',
        'secret' => 'your-twitter-consumer-secret',
    ],
];

try {
    $twitter = new Hybridauth\Provider\Twitter($config);

    $twitter->authenticate();

    $accessToken = $twitter->getAccessToken();
    $userProfile = $twitter->getUserProfile();
    $apiResponse = $twitter->apiRequest('statuses/home_timeline.json');
}
catch (\Exception $e) {
    echo 'Oops, we ran into an issue! ' . $e->getMessage();
}
```

10. Данные о товарах регулярно отправляются в Яндекс.Маркет.

yandex-market/yandex-market-php-partner

С помощью партнерского API Яндекс.Маркета для моделей DBS (Delivery by Seller, продажи с доставкой продавца) 
и ADV (Advertising, рекламная модель) внешние приложения могут получать сведения о своих магазинах и предложениях и управлять ими. 
Библиотека написана на языке PHP и содержит методы для работы с партнерским API.

```php
// Указываем авторизационные данные
$clientId = '9876543210fedcbaabcdef0123456789';
$token = '01234567-89ab-cdef-fedc-ba9876543210';

// Создаем экземпляр клиента с базовыми методами
$baseClient = new \Yandex\Market\Partner\Clients\BaseClient($clientId, $token);

// Магазины возвращаются постранично
$pageNumber = 0;
do {
    $pageNumber++;
    
    // Получаем страницу магазинов с номером pageNumber
    $campaignsObject = $baseClient->getCampaigns(['page' => $pageNumber,]);
    // Получаем итератор по магазинам на странице
    $campaignsPage = $campaignsObject->getCampaigns();

    // Получаем количество магазинов на странице
    $campaignsCount = $campaignsPage->count();

    // Получаем первый магазин
    $campaign = $campaignsPage->current();
    // Печатаем идентификатор и URL магазина, затем переходим к следующему    
    for ($i = 0; $i < $campaignsCount; $i++) {
        echo 'ID: ' . $campaign->getId();
        echo 'Domain: ' . $campaign->getDomain();        
        $campaign = $campaignsPage->next();
    }
    
    // Получаем информацию о страницах. Возвращаемое количество страниц может увеличиваться 
    // по мере увеличения номера страницы. Последняя страница будет достигнута, 
    // когда вернется количество страниц, равное номеру текущей страницы    
    $campaignsTotalPages = $campaignsObject->getPager()->getPagesCount();
} while ($pageNumber != $campaignsTotalPages);    
```

## 11. Принимается онлайн-оплата от покупателей.

stripe/stripe-php

The Stripe PHP library provides convenient access to the Stripe API from applications written in the PHP language. 
It includes a pre-defined set of classes for API resources that initialize themselves dynamically from API responses which makes 
it compatible with a wide range of versions of the Stripe API.

```php
$stripe = new \Stripe\StripeClient('sk_test_BQokikJOvBiI2HlWgH4olfQ2');
$customer = $stripe->customers->create([
    'description' => 'example customer',
    'email' => 'email@example.com',
    'payment_method' => 'pm_card_visa',
]);
echo $customer;
```

## 12. Применяются средства тестирования (например, PHPUnit).

phpunit/phpunit

PHPUnit is a programmer-oriented testing framework for PHP. 
It is an instance of the xUnit architecture for unit testing frameworks.

```php
// Testing array operations with PHPUnit¶
use PHPUnit\Framework\TestCase;

final class StackTest extends TestCase
{
    public function testPushAndPop(): void
    {
        $stack = [];
        $this->assertSame(0, count($stack));

        array_push($stack, 'foo');
        $this->assertSame('foo', $stack[count($stack)-1]);
        $this->assertSame(1, count($stack));

        $this->assertSame('foo', array_pop($stack));
        $this->assertSame(0, count($stack));
    }
}
```
