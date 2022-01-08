# Omnipay: First Atlantic Commerce

First Atlantic Commerce driver for the Omnipay PHP payment processing library

[//]: # ([![Latest Version on Packagist]&#40;https://img.shields.io/packagist/v/shamarkellman/omnipay-first-atlantic-commerce.svg?style=flat-square&#41;]&#40;https://packagist.org/packages/shamarkellman/omnipay-first-atlantic-commerce&#41;)
[//]: # ([![GitHub Tests Action Status]&#40;https://img.shields.io/github/workflow/status/shamarkellman/omnipay-first-atlantic-commerce/Tests?label=tests&#41;]&#40;https://github.com/shamarkellman/omnipay-first-atlantic-commerce/actions?query=workflow%3ATests+branch%3Amaster&#41;)
[//]: # ([![Total Downloads]&#40;https://img.shields.io/packagist/dt/shamarkellman/omnipay-first-atlantic-commerce.svg?style=flat-square&#41;]&#40;https://packagist.org/packages/shamarkellman/omnipay-first-atlantic-commerce&#41;)

Omnipay is a framework agnostic, multi-gateway payment processing library for PHP. This package implements First
Atlantic Commerce support for Omnipay.

## Support us

We invest a lot of resources into creating opensource products.

[//]: # (We highly appreciate you sending us a postcard from your hometown, mentioning which of our package&#40;s&#41; you are using.)
[//]: # (You'll find our address on [our contact page]&#40;https://spatie.be/about-us&#41;. We publish all received postcards)
[//]: # (on [our virtual postcard wall]&#40;https://spatie.be/open-source/postcards&#41;.)

## Installation

You can install the package via composer:

```bash
composer require juliosolis/omnipay-bac-fac
```

## Usage

### Authorize Request

```php
$gateway = Omnipay::create('FirstAtlanticCommerce');
$gateway->setMerchantId('123456789');
$gateway->setMerchantPassword('abc123');
$gateway->setAcquirerId(12345);

$cardData = [
    'number' => '4242424242424242',
    'expiryMonth' => '6',
    'expiryYear' => '2016',
    'cvv' => '123'
];

$response = $gateway->authorize([
    'createCard' => true, //optional - Will return tokenized card if included
    'amount' => '10.00',
    'currency' => 'USD',
    'transactionId' => '1234',
    'card' => $cardData,
    'testMode' => true //use for calls to FAC sandbox
])->send();

if ( $response->isSuccessful() ) {
    print_r($response);
}
else {
    echo $response->getMessage();
}

```

### Single Pass Purchase Request

```php
$gateway = Omnipay::create('FirstAtlanticCommerce');
$gateway->setMerchantId('123456789');
$gateway->setMerchantPassword('abc123');
$gateway->setAcquirerId(12345);

$cardData = [
    'number' => '4242424242424242',
    'expiryMonth' => '6',
    'expiryYear' => '2016',
    'cvv' => '123'
];

$response = $gateway->purchase([
    'createCard' => true, //optional - Will return tokenized card if included
    'amount' => '10.00',
    'currency' => 'USD',
    'transactionId' => '1234',
    'card' => $cardData,
    'testMode' => true //use for calls to FAC sandbox
])->send();

if ( $response->isSuccessful() ) {
    print_r($response);
}
else {
    echo $response->getMessage();
}

```
### Capture Previous Transaction

```php
$gateway = Omnipay::create('FirstAtlanticCommerce');
$gateway->setMerchantId('123456789');
$gateway->setMerchantPassword('abc123');
$gateway->setAcquirerId(12345);

$response = $this->gateway->capture([
    'amount' => '10.00',
    'currency' => 'USD',
    'transactionId' => '1234',
])->send();

if ( $response->isSuccessful() ) {
    $response->getCode();
    $response->getResponseCode();
    $response->getMessage();
    $response->getOriginalResponseCode();
}
else {
    echo $response->getMessage();
}

```

### Hosted Page Request

```php
$gateway = Omnipay::create('FirstAtlanticCommerce');
$gateway->setMerchantId('123456789');
$gateway->setMerchantPassword('abc123');
$gateway->setAcquirerId(12345);

$response = $this->gateway->hostedPage([
    'amount' => '10.00',
    'currency' => 'USD',
    'transactionId' => '1234',
    'transactionCode' => TransactionCode::SINGLE_PASS + TransactionCode::REQUEST_TOKEN, //Use values based on requirements 
    'cardHolderResponseURL' => 'https://merchant/response/page.php',
    'pageSet' => 'pageSetFromPanel',
    'pageName' => 'pageNameFromPanel',
])->send();

if ( $response->isSuccessful() ) {
    $response->getRedirectUrl();
    $response->getToken(); //the single use token to build hosted page URL. See doc
}
else {
    echo $response->getCode();
    echo $response->getMessage();
}
```

### Hosted Page Result Request
This returns the payment data for the hosted payment 

```php
$gateway = Omnipay::create('FirstAtlanticCommerce');
$gateway->setMerchantId('123456789');
$gateway->setMerchantPassword('abc123');
$gateway->setAcquirerId(12345);

$response = $this->gateway->hostedPageResult([
    'token' => '_JBfLQJNiEmFBtnF3AfoeQ2', //token is provided returned in callback after completes hosted page
]);

if ( $response->isSuccessful() ) {
    echo $response->getResponseCode();
    echo $response->getMessage();
    echo $response->getTransactionId();
    echo $response->getCardReference(); //if tokenization was requested
}
else {
    echo $response->getCode();
    echo $response->getMessage();
}
```

## Testing

```bash
composer test
```

## Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information on what has changed recently.

## Contributing

Please see [CONTRIBUTING](.github/CONTRIBUTING.md) for details.

## Security Vulnerabilities

Please review [our security policy](../../security/policy) on how to report security vulnerabilities.

## Credits

- [Julio Solis](https://github.com/JulioSolis)
- [All Contributors](../../contributors)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
