# Paga Collect PHP API Library v1.1.2

## Business Services exposed by the library

- paymentRequest
- getBanks
- paymentStatus
- paymentHistory
- registerPersistentPaymentAccount
- updatePersistentPaymentAccount
- deletePersistentPaymentAccount
- paymentRequestFund
- getPersistentPaymentAccount

For more information on the services listed above, visit the [Paga DEV website](https://developer-docs.paga.com/docs/php-library-1)

## How to use

`composer require paga/paga-collect`

```
require_once __DIR__ .'/vendor/autoload.php'


$collectClient = PagaCollectClient::builder()
                ->setApiKey("<apiKey>")
                ->setClientId("<publicId>")
                ->setPassword("<password>")
                ->setTest(true)
                ->build();
```

As shown above, you set the publicId, apiKey, password given to you by Paga, If you pass true as the value for setIsTest(), the library will use the test url as the base for all calls. Otherwise setting it to false will use the live url value you **pass** as the base.

### Paga Collect Service Functions

**Request Payment**

Registers a new request for payment between a payer and a payee. Once a payment request is initiated successfully, the payer is notified by the platform (this can be suppressed) and can proceed to authorize/execute the payment. Once the payment is fulfilled, a notification is sent to the supplied callback URL.

To make use of this function, call the **paymentRequest** inside PagaCollectClient which will return a JSONObject.

```
$data = ["referenceNumber" => "908w1111000001129",
    "amount" => 200,
    "callBackUrl" => "http://localhost:5000/core/webhook/paga",
    "currency" => "NGN",
    "expiryDateTimeUTC" => "2021-05-20T19:35:47",
    "isAllowPartialPayments" => false,
    "isSuppressMessages" => false,
    "payee" => ["bankAccountNumber"=>"XXXXXXXXX",
              "bankId" => "XXXXX-XXX-XXX-XXX-XXXXXX",
              "name" => "John Doe",
              "phoneNumber" => "XXXXXXXXXXX",
            "accountNumber" => "XXXXXXXXXXX"],
    "payer" => ["email" => "johndoe@gmail.com", 
                "name"=> "Foo Bar", 
                "bankId"=> "XXXXX-XXX-XXX-XXX-XXXXXX",
      ],
    "payerCollectionFeeShare"=> 1.0,
    "recipientCollectionFeeShare"=> 0.0,
    "paymentMethods"=> ["BANK_TRANSFER", "FUNDING_USSD"]
    ];

$paymentRequest = $collectClient->paymentRequest($data);$response = 

```

**Get Banks**

Retrieve a list of supported banks and their complementary unique ids on the bank. This is required for populating the payer (optional) and payee objects in the payment request model.
To make use of this function, call the **getBanks** inside PagaCollectClient which will return a JSONObject.

```
$data = ['referenceNumber' => "234455555"];
$getBanks = $collectAPI ->getBanks($data);
```

**Query Payment Request Status**

Query the current status of a submitted payment request.
To make use of this function, call the **paymentStatus** inside PagaCollectClient which will return a JSONObject.

```
$data = ['referenceNumber' => "234455555"];
$paymentStatus = $collectAPI ->paymentStatus($data);
```

**Payment Request History**

Get payment requests for a period between to give start and end dates. The period window should not exceed 1 month.
To make use of this function, call the **paymentHistory** inside PagaCollectClient which will return a JSONObject.

```
$data = [
    "referenceNumber" => "8235346400000099",
    "startDateTimeUTC" => "2021-04-21T19:15:22",
    "endDateTimeUTC" => "2021-05-18T19:15:22"
];
$paymentStatus = $collectAPI ->paymentHistory($data);
```



**Register Persistent Payment Account**

An operation for business to create Persistent Payment Account Numbers that can be assigned to their customers for payment collection.
To make use of this function, call the **registerPersistentPaymentAccount** inside PagaCollectClient which will return a JSONObject.

```
$data= [
    'referenceNumber'=>"47575685389595",
    'phoneNumber'=>"07048576234",
    'firstName'=>"Ian", 
    'lastName'=>"Lankansa",
    'accountName'=>"Ian Lankansa", 
    'financialIdentificationNumber'=>"12345484326",
    'accountReference'=>"123407891334",
    'callbackUrl' => "http://localhost:9091/test-callback"
];
$registerPersistentPaymentAccount = $collectAPI ->registerPersistentPaymentAccount($data);
```


**Update Persistent Payment Account**

This endpoint allows for changing any of the account properties except the **accountNumber (NUBAN)** and the **accounReference** properties which cannot be changed.
To make use of this function, call the **updatePersistentPaymentAccount** inside PagaCollectClient which will return a JSONObject.

```
$data= [
    'referenceNumber'=>"47575685389595",
    'phoneNumber'=>"07048576234",
    'firstName'=>"Ian", 
    'lastName'=>"Lankansa",
    'accountName'=>"Ian Lankansa", 
    'accountIdentifier'=>"12345484326",
    'callbackUrl' => "http://localhost:9091/test-callback"
];
$registerPersistentPaymentAccount = $collectAPI ->updatePersistentPaymentAccount($data);
```


**Delete Persistent Payment Account**

This endpoint allows for deleting a persistent payment account.
To make use of this function, call the **deletePersistentPaymentAccount** inside PagaCollectClient which will return a JSONObject.

```
$data= [
    'referenceNumber'=>"47575685389595",
    'accountIdentifier'=>"12345484326"
];
$registerPersistentPaymentAccount = $collectAPI ->deletePersistentPaymentAccount($data);
```


**Payment Request Fund**

This end-point can be used to either cancel or initiate a refund if we were unable to fulfill the request for one reason or the other
To make use of this function, call the **paymentRequestFund** inside PagaCollectClient which will return a JSONObject.

```
$data= [
    'referenceNumber'=>"47575685389595",
    'accountIdentifier'=>"12345484326"
    'refundAmount'=>500,
    'currency'=> "NGN",
];
$registerPersistentPaymentAccount = $collectAPI ->paymentRequestRefund($data);
```


# Changelog

## [1.0.0] - 2021-05-20

### Added

- Implemented endpoints for paga-collect

## [1.0.1] - 2021-05-25

### Bug fix

- Updated dependecies
- Removed php-console dependencies


## [1.1.2] - 2021-10-03

### Added

- Implemented updatePersistentPaymentAccount, getPersistentPaymentAccount, deletePersistentPaymentAccount, paymentRequestRefund etc.
