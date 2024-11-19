# Paycrypto API Documentation

* [Paycrypto API](#Paycrypto-API)
* [API Specifications](#API-Specifications)
* [1.Institution](#Institution)
     * [1.1 Query Card type](#Query-Card-type)
     * [1.2 Query customer balance](#Query-customer-balance)
     * [1.3 Query institution information](#Query-institution-information)
     * [1.4 Upload public key](#Upload-public-key)
     * [1.5 Query rate](#Query-rate)
     * [1.6 Estimate the amount of fiat will be arrived](#Estimate-the-amount-of-fiat-will-be-arrived)
     * [1.7 Estimate the deposit amount of crypto](#Estimate-the-deposit-amount-of-crypto)
* [2.KYC](#KYC)
     * [2.1 Submitting user KYC data](#Submitting-user-KYC-data)
	 * [2.2 Submitting user KYC attachment (optional)](#Submitting-user-KYC-attachment-optional)
     * [2.3 Query all KYC records](#Query-all-KYC-records)
     * [2.4 Query a specific user KYC records](#Query-a-specific-user-KYC-records)
* [3.Cards](#cards)
     * [3.1 Apply a card](#Apply-a-card)
     * [3.2 Submit active card attachment](#submit-active-card-attachment)
     * [3.3 User activating bank card](#User-activating-bank-card)
     * [3.4 Query all active card status](#Query-all-active-card-status)
     * [3.5 Query a specific user card activation status](#Query-a-specific-user-card-activation-status)
     * [3.6 Request Lock, Unlock, Lost, Renew PIN, Replacement card](#Request-Lock-Unlock-Lost-Renew-PIN-Replacement-card)
     * [3.7 Query Lock, Unlock, Lost, Renew PIN, Replacement card Status](#Query-Lock-Unlock-Lost-Renew-PIN-Replacement-card-Status)
     * [3.8 Query tracking number](#Query-tracking-number)
* [4.Transactions](#transactions)
     * [4.1 User deposit with stablecoin](#User-deposit-with-stablecoin)
     * [4.2 Fixed amount will be received in fiat](#User-deposit-with-stablecoin-Fixed-amount-will-be-received-in-fiat)
     * [4.3 Query Exchange Price](Query-Exchange-Price)
     * [4.4 Query a deposit transaction status](#Query-a-deposit-transaction-status)
     * [4.5 Query all the deposit records](#Query-all-the-deposit-records)
     * [4.6 Query a particular user deposit records](#Query-a-particular-user-deposit-records)
* [5.Bank](#bank)
     * [5.1 Query card status](#Query-card-status)
     * [5.2 Query account balance](#Query-account-balance)
     * [5.3 Query transaction records](#Query-transaction-records)
     * [5.4 Query card information](#Query-card-information)
     * [5.5 User triggers a card withdrawal password reset Email (Currently not supported)](#User-triggers-a-card-withdrawal-password-reset-Email-(Currently-not-supported))
* [6.Verification Code API](#Verification-Code-API)
     * [6.1 Sending Email verification code](#Sending-Email-verification-code)
     * [6.2 Email verification code validation](#Email-verification-code-validation)
     * [6.3 Sending SMS verification code (Currently not supported)](#Sending-SMS-verification-code-(Currently-not-supported))
     * [6.4 SMS verification code validation (Currently not supported)](#SMS-verification-code-validation-(Currently-not-supported))
* [7.Webhook](#Webhook)   
     * [7.1 Push KYC Event](#Push-KYC-Event)
     * [7.2 Push Card Apply Event](#Push-Card-Apply-Event)
     * [7.3 Push Card Activation Event](#Push-Card-Activation-Event)
     * [7.4 Push Deposit Event](#Push-Deposit-Event)
     * [7.5 Test push events](#Test-push-events)
     * [7.6 Query push failure events](#Query-push-failure-events)
     * [7.7 Update push failure events](#Update-push-failure-events)
     * [7.8 Push Lock, Unlock, Lost, Renew PIN, Replacement card Status](#Push-Lock-Unlock-Lost-Renew-PIN-Replacement-card-Status)
* [8.Error Codes](#error-codes)
     * [8.1 Business Logic Error Codes](#Business-Logic-Error-Codes)
     * [8.2 Identity Authentication Error Codes](#Identity-Authentication-Error-Codes)
     * [8.3 Abnormal Status Error Codes](#Abnormal-Status-Error-Codes)
     * [8.4 KYC Failure Error Codes](#KYC-Failure-Error-Codes)


## Paycrypto API


Welcome to the Paycrypto API documentation. This document is aimed at Paycrypto ToB's card business. Currently, it supports three card types, namely F card, J card and P card. The corresponding metal cards are F-M card, J-M card and P-M card. The corresponding virtual cards are F-V card, J-V card and P-V card. The fees and the parameters are slightly different for cards.

  | Card name |  Currency  |           distinguish            |
  | :--------: | :----: | :------------------------------ |
  |    J,J-M*,J-V card(ID: 30000001 - 30000009)     | USD | Within 48h after KYC audit, KYC fill only supports English, `mobile` format such as +8615821702552. The name of the user will be printed on the card, and the card making time need 1-2 weeks. User must submit hold a card and passport photo when activation card. Recharge arrival time is within 4h |
  |   P,P-M*,P-V card(ID: 40000001 - 40000009)   | EUR       |         Within 48h after KYC audit, KYC fill only supports English, `mobile` format such as +8615821702552, `country` and `nationality` fill in the two-digit country code。Recharge arrival time is within 4h |

  |  Card name |  Card type  |           distinguish            |
  | :--------: | :----: | :------------------------------ |
  |    J,P card    |  Plastic card  |  Plastic material, The card will be mailed to the user  |
  |    J-MB,P-MB card    |  Metal Black card  |  Metal material, Black color, The card will be mailed to the user   |
  |    J-MS,P-MS card    |  Metal Silver card  |  Metal material, Silver color, The card will be mailed to the user   |
  |    J-MG,P-MG card    |  Metal Golden card  |  Metal material, Golden color, The card will be mailed to the user   |
  |    J-V,P-V card    |  Virtual card  |  Just get the card information through the API, user can consume online   |

  
API usage steps are as follows:

1. Please register an institution account at [https://customer.paycrypto.com/](https://customer.paycrypto.com/), and if you are unable to access, please provide your IP address.
2. After the Paycrypto audit is passed, the institution can login successfully.
3. Checks the wallet address and deposit, support USDT, BTC, ETH, etc..
4. Create Appkey and secret, and optionally configure webhook callback address.
5. Call the API for KYC, card opening, card activation, deposit and other operations. Paycrypto will notify through the callback address when the status changes.

> It is recommended to testing in the test environment before using the production environment.

Dashboard :

* Test environment dashboard (restricted by IP whitelist): https://customer.sandbox.paycrypto.com/
* Production environment dashboard (restricted by IP whitelist): https://customer.paycrypto.com/

API :

* Test environment: https://api.sandbox.paycrypto.com/
* Production environment: https://api.paycrypto.com/

## API Specifications

- API requests use `HMAC` authentication.

- **Pagination** - Query record lists are all divided into pages, Pagination parameters: `page_num` represents the page number, `page_size` represents the size of each page. API `DTO` uniformly returns `total`, `records`.

- **Country** - Two digit country codes, refer to `ISO 3166-1 alpha-2` standards.

- Time management - API requests and responses return a `UNIX` timestamp, unit being **seconds**, in order to avoid issues due to regional time differences.

- Amount management - All API requests and responses are of the `string` data type in order to avoid precision loss.

- All the requests that have a `body` but don't explicitly define a format are of `JSON` type, `Content-Type: application/json`

- The query interval of all query interfaces must be **less than one month**

- API response format standard-

  | Parameter  |  Type  |                               Description                               |
  | :----: | :----: | :---------------------------------------------------------------------: |
  |  code  |  int   |               Error code. `0`: Normal, non-`0`: Abnormal                |
  |  msg   | string | `SUCCESS` indicates success, error code indicates and describes failure |
  | result | object |                                 Result                                  |

### HMAC Authentication

The institution first needs to apply for the API `key` and API `secret` that will be used when accessing the API.

| Term                   | Description                                                                                                                                                                              |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| User ID                | User ID is used to indicate the developer account, is used as the user ID                                                                                                                |
| API key and API secret | Multiple API key + API secret maintained under a User ID, API key is linked with an application, multiple applications are allowed, each application can apply for API access privileges |

#### Client side implementation process:

1. Compose the data that needs to be signed, including-
   - UNIX timestamp, unit being `milliseconds`: the `request` time stamp
   - Request method: `HTTP` method
   - Request API key： API Key
   - Complete request path, including the `URL` parameters: request URI
   - If there is a request `body`, the post conversion `string` of the `body` also needs to be added: string representation of the request payload
2. Client side generates the signature using `HMAC_SHA256` based on the data and API secret
3. Set the Authorization header based on the fixed sequence, i.e. the key is `Authorization`, and the value is: Railone:ApiKey:request time stamp:signature (linked using colon)
4. If the server side sets a password when creating the API key and secret, then an Access-Passphrase header needs to be set, i.e., the `key` is `Access-Passphrase`, and the `value` is the password.
5. Client side sends the data, Authorization header, and the Access-Passphrase header (in case there is a fourth step) to the server side, i.e., the final http header sent is as follows:
   - Authorization：Railone:ApiKey:request timestamp:signature
   - Access-Passphrase：Your API Secret passphrase

#### How to build the request body string to be signed:

The parameter names of the request body need to be based on the respective `ASCII` values, pair key and value using `=`, and connect multiple key-value pairs using `&` to form a string.

Here is an example `body`-

```json
{
  "ont_id": "did:ont:Ae9ujqUnAtH9yRiepRvLUE3t9R2NbCTZPG",
  "amount": 190,
  "to_address": "AUol16ghiT9AtxRDtNeq3ovhWJ5iaY6iyd"
}
```

The payload is converted to-

```text
amount=190&from_address=Ae9ujqUnAtH9yRiepRvLUE3t9R2NbCTZPG&to_address=AUol16ghiT9AtxRDtNeq3ovhWJ5iaY6iyd
```

#### Code Implementation and Examples:

For illustrative sample code, please refer to [https://github.com/paycrypto-com/paycrypto-sdk-java](https://github.com/paycrypto-com/paycrypto-sdk-java)


## Institution


### Query Card type

```text
url：/api/v1/institution/card/type
method：GET
```

- Response:

```json
{
    "code": 0,
    "msg": "SUCCESS",
    "result": {
        "total": 2,
        "records": [
            {
                "card_type_id": "50000001",
                "currency_type": "USD",               
                "bank_id": "5000",
                "description": "card 1",
                "card_network": "visa",
                "card_title": "F",
                "virtual_card": false
            },
            {
                "card_type_id": "50000002",
                "currency_type": "USD",               
                "bank_id": "5000",
                "description": "card 2",
                "card_network": "visa",
                "card_title": "F-V",
                "virtual_card": true
            }
        ]
    }
}
```

| Parameter |  Type  |          Description          |
| :--------: | :----: | :------------------------------ |
|   bank_id   | String |        Bank ID              |
|   currency_type   | String |    Currency type of the card             |
| card_type_id |String | Card type id, for example 50000001|
| description |String |description of card type |
|   card_network   | String |    card network           |
| virtual_card  | Bool |    virtual card           |
| card_title  | String |   it supports three card types, namely F card, J card and P card. Metal cards are F-M* , J-M* and P-M* card. Virtual cards are F-V, J-V and P-V card          |

### Query customer balance

- Request:

```text
url：/api/v1/institution/balance
method：GET
```

- Response:

```
{
    "code": 0,
    "msg": "SUCCESS",
    "result": [
        {
            "addresses":[
                {
                    "balance":"100",
                    "address_type":"deposit",
                    "address":"0x0e420097975b6c700f6314914a1b6e66a1edc313"
                },
                {
                    "balance":"200",
                    "address_type":"open_card",
                    "address":"0x88fee6dda9a041e9ebe6c56924765ff54544e40c"
                }],
            "coin_type":"USDT"
        }
    ]
}
```

|    Parameter    |  Type   |      Description  |
| :---------: | :----:   | :--------------------------- |
| coin_type    | String  |  coin type         |
| addresses[0].balance    | String  |  coin balance         |
| addresses[0].address    | String  | coin address         |
| addresses[0].address_type    | String  | coin address type, 'open card' or 'deposit' address.        |

### Query institution information


- Request:

```text
url：/api/v1/institution/info
method：GET
```


- Response:

```
{
    "code": 0,
    "msg": "SUCCESS",
    "result": {
        "publickey":  ""
    }
}
```

| Parameter |  Type  |          Description          |
| :--------: | :----: | :------------------------------ |
|   publickey   | String |           public key           |

### Upload public key

- Request:

```text
url：/api/v1/institution/publickey
method：POST
```

|  Parameter  | Type  | Whether Required |                        Description                         |
| :---------: | :---: | :--------------: | :-------------------------------------------------------- |
| public_key  |  String  |    Required     | public key     |


- Response:

```
{
    "code": 0,
    "msg": "SUCCESS",
    "result": true
}
```

### Query rate

```text
url：/api/v1/institution/rates?card_type_id={card_type_id}
method：GET
```

- Request：

| Parameter |  Type  |   Requirement  | Description   |
| :------------: | :----: | :----------: |:---------- |
| card_type_id | String |Required| Card type id, for example 50000001|

- Response：

```json
{
    "code": 0,
    "msg": "SUCCESS",
    "result": {
        "exchange_rate": "1.00505392692",
        "card_application_fee": "1",
        "min_deposit":"0",
        "max_deposit":"10000",
        "loading_rate": [
            {
                "min": "0",
                "max": "1000",
                "step_rate": "0.05"
            },
            {
                "min": "1000",
                "max": "2000",
                "step_rate": "0.03"
            }
        ],
        "bank_atm_fee": 0,
        "bank_atm_rate": "0.1",
        "bank_transaction_rate": "0.2"
    }
}
```

| Parameter |  Type  |          Description          |
| :--------: | :----: | :------------------------------ |
|   card_application_fee   | String |           Card application fee in USDT          |
|   min_deposit   | String |          Maximum deposit amount(Fiat currency) in one transaction          |
|   max_deposit   | String |           Minimum deposit amount(Fiat currency) in one transaction         |
|   exchange_rate   | String |   exchange rate of  USDT to fiat currency         |
|   loading_rate   | String |           Loading step rate for deposit to user, most cards have only one step rate         |
|   bank_transaction_rate   | String |          Bank transaction rate for consumption          |
|   bank_atm_rate   | String |          ATM withdraw rate           |
|   bank_atm_fee| String |          ATM withdraw fixed fee|


### Estimate the amount of fiat will be arrived

- Request:

```text
url：/api/v1/institution/estimation/currency
method：POST
```

|  Parameter  | Type  | Whether Required |                        Description                         |
| :---------: | :---: | :--------------: | :-------------------------------------------------------- |
|  coin_amount  |  String  |    Required     |  received coin amount     |
| coin_type  |  String  |    Required     |  received coin type     |
|  card_type_id  |  String  |    Required     |  card type id    |

- Response:

```
{
    "code": 0,
    "msg": "SUCCESS",
    "result": {
        "coin_type": "usdt",
        "coin_amount": "106.23",
        "currency_type": "usd",
        "currency_amount": "94.13",
        "exchange_rate": "1.00145966373",
        "fiat_exchange_rate": "1",
        "exchange_fee": "1.0623",
        "exchange_fee_rate": "0.01",
        "loading_fee": "5.3115",
        "coin_price": "1"
    }
}
```

|    Parameter    |  Type   |      Description                                                     |
| :---------: | :----:   | :--------------------------- |
|  coin_amount    | String  |    required coin amount          |
|  coin_type  |  String    |  the coin type you received    |
|  currency_amount    | String  |    received currency amount          |
|  currency_type  |  String    |  the currency type you received    |
| exchange_fee    | String  |   exchange fee for converting other coin to USDT, Unit: coin_type           |
| exchange_fee_rate    | String  |   exchange fee rate for converting other coin to USDT           |
|  loading_fee    | String  |       loading fee of transaction, Unit: coin_type       |
| exchange_rate    | String  | exchange rate of USDT/USD             |
| fiat_exchange_rate    | String  | exchange rate of card currency/USD              |
| coin_price    | String  | price of coin_type/USDT              |

> (currency_amount * fiat_exchange_rate) = ((coin_amount - exchange_fee - loading_fee) * coin_price) * exchange_rate 
> arrived USD = deposit USDT * exchange_rate

### Estimate the deposit amount of crypto  

- Request:

```text
url：/api/v1/institution/estimation/crypto
method：POST
```
- Request:

|  Parameter  | Type  | Whether Required |                        Description                         |
| :---------: | :---: | :--------------: | :--------------------------------------------------------|
|  currency_amount  |  String  |    Required     |  received currency amount     |
|  card_type_id  |  String  |    Required     |  card type id    |
|  coin_type  |  String  |    Required     |  the coin type you want to convert    |

- Response:

```
{
    "code": 0,
    "msg": "SUCCESS",
    "result": {
        "coin_type": "usdt",
        "coin_amount": "106.23",
        "currency_type": "usd",
        "currency_amount": "94.13",
        "exchange_rate": "1.00145966373",
        "fiat_exchange_rate": "1",
        "exchange_fee": "1.0623",
        "exchange_fee_rate": "0.01",
        "loading_fee": "5.3115",
        "coin_price": "1"
    }
}
```

|    Parameter    |  Type   |      Description      |
| :---------: | :----:   | :--------------------------- |
|  coin_amount    | String  |    required coin amount          |
|  coin_type  |  String    |  the coin type you received    |
|  currency_amount    | String  |    received currency amount          |
|  currency_type  |  String    |  the currency type you received    |
| exchange_fee    | String  |   exchange fee for converting other coin to USDT, Unit: coin_type           |
| exchange_fee_rate    | String  |   exchange fee rate for converting other coin to USDT           |
|  loading_fee    | String  |       loading fee of transaction, Unit: coin_type       |
| exchange_rate    | String  | exchange rate of USDT/USD             |
| fiat_exchange_rate    | String  | exchange rate of card currency/USD              |
| coin_price    | String  | price of coin_type/USDT              |

> (currency_amount * fiat_exchange_rate) = ((coin_amount - exchange_fee - loading_fee) * coin_price) * exchange_rate 
> arrived USD = deposit USDT * exchange_rate

## KYC

This API contains methods that can be used by an institution to carry out user-related operations such as user creation and KYC, along with fetching relevant KYC details for users, etc.

### Submitting user KYC data

Email verification feature is optional. The verification code status is updated to used when verification is successfully carried out.

- Request:

```text
url：/api/v1/customers/accounts
method：POST
```



|       Parameter        |  Type  | Whether Required |                                          Description                                          |
| :--------------------: | :----: | :--------------: | :-------------------------------------------------------------------------------------------: |
|        acct_no         | String |     Required     |         Account serial no. (Unique within the institution), Max. character length: 64         |
|       acct_name        | String |     Required     |                      Institution account name, Max. character length: 64                      |
|       first_name       | String |     Required     |                          Legal first name, Max. character length: 50                          |
|       last_name        | String |     Required     |                          Legal last name, Max. character length: 50                           |
|         gender         | String |     Required     |             male: Male，female: Female，unknown: other, Max. character length: 6              |
|        birthday        | String |     Required     |                                    Birth date (YYYY-MM-DD)                                    |
|          city          | String |     Required     |                               City, Max. character length: 100                                |
|         state          | String |     Required     |                               State, Max. character length: 100                               |
|        country         | String |     Required     |                              Country, Max. character length: 50                               |
|      nationality       | String |     Required     |                           Nationality，, Max. character length: 255                           |
|         doc_no         | String |     Required     |                                        Document number                                        |
|        doc_type        | String |     Required     | Document type(Only support passport). passport：Passport，idcard：National ID card |
|       front_doc        | String |     Required     |                              Front face picture. Base64 encoding. File size should be less than 2M  |
|        back_doc        | String |     Optional     |                              Back face picture. Required if doc_type is idcard. Base64 encoding. File size should be less than 2M              |
|        mix_doc         | String |     Required     |                            Photo with certificate in hand. Base64 encoding. File size should be less than 2M |
|      country_code      | String |     Required     | International country code，for example "+86". Max. character length: 5 |
|         mobile         | String |     Required     |                           Mobile number, Max. character length: 32                            |
|          mail          | String |     Required     |                           Email address,don't support 163.com email host. Max. character length: 64                            |
|        address         | String |     Required     |                          Postal address, the bank card will send tothis address. Max. character length: 256                           |
|        zipcode         | String |     Required     |                              Zip code, Max. character length: 20                              |
|      maiden_name       | String |     Required      |                        Your Mother name or put any relative Friend name, Max. character length: 255                         |
| card_type_id |String |Required | Bank card type id, for example: 10010001|   
|        kyc_info        | String |     Optional     |                                     Other KYC information                                     |
| mail_verification_code | String |     Optional     |                                    Email verification code                                    |
|       mail_token       | String |     Optional     |                        Token returned upon sending verification Email                         |
| cust_tx_id            | String | Optional         | customer transaction id|
| sign_img | String | Optional| hand signature image. Base64 encoding. File size should be less than 1M |
| poa_doc | String[3] |Optional |Picture or PDF of proof of address(Currently not supported). Base64 encoding. Each file size should be less than 2M|

- Response:

```json
{
  "code": 0,
  "msg": "string",
  "result": true
}
```

> How to get mail_token and mail_verification_code? Please see "6.1. Sending Email verification code". mail_verification_code is filled by user.


### Submitting user KYC attachment (optional)


```text
url：/api/v1/customers/attachments
method：POST
```
> Before calling this interface, you need to successfully call the [Submitting user KYC data] interface. This interface only supports physical card.

- Request：

| Parameter |  Type  |    Requirement  |Description   |
| :------------: | :----: | :----------: |:---------- |
|        acct_no         | String |     Required     |         Account serial no. (Unique within the institution), Max. character length: 64         |
| card_type_id |String |Required | Bank card type id, for example: 10010001|   
| docs | String[3] |Required |Picture or PDF of AML proof  . Base64 encoding. Each file size should be less than 2M, Max. files: 3 |
- Response：

```json
{
  "code": 0,
  "msg": "Success",
  "result": true
}
```


### Query all KYC records

- Request:

```text
url：/api/v1/customers/accounts
method：GET
```

|  Parameter  | Type  | Whether Required |                        Description                         |
| :---------: | :---: | :--------------: | :--------------------------------------------------------: |
|  page_num   |  int  |     Optional     |                        Page number                         |
|  page_size  |  int  |     Optional     |                         Page size                          |
| former_time | long  |     Optional     | Time period upper limit, `UNIX` timestamp, Unit: `seconds` |
| latter_time | long  |     Optional     | Time period lower limit, `UNIX` timestamp, Unit: `seconds` |
| time_sort | String | Optional| sort time. asc: time in ascending order，desc: time in descending order    |

- Response:

```json
{
  "code": 0,
  "msg": "SUCCESS",
  "result": {
    "total": 1,
    "records": [
      {
        "acct_no": "1222",
        "card_type_id": "10010001",
        "status": 2,
        "reason": "{\"code\":1110032,\"msg\":\"KYC failure, photo error\"}",
        "create_time": 1546300800000
      }
    ]
  }
}
```

|    Parameter    |  Type   |                                                    Description                                                     |
| :---------: | :----:   | :----------------------------------------------------------------------------------------------------------------: |
|   acct_no   | String  |                         Institution account name (Unique within scope of the institution)                          |
| card_type_id |String | Card type id|   
|   status    |  int    | Status code : 0 - Submitted successfully, 1 - Verification successful (Account activated), 2 - Verification failed, 3 - Verifying, 4 - Submission in progress  6 - refund |
| reason | String |      Reason for verification failure.       |
| create_time |  long   |                                                   Creation time                                                    |

> KYC failure reason please see "KYC Failure Error Codes"

### Query a specific user KYC records

- Request:

```text
url：/api/v1/customers/accounts
method：GET
```

|  Parameter  |  Type  | Whether Required | Description                                                |
| :---------: | :----: | :------------:  | :--------------------------------------------------------- |
|   acct_no   | String | Required | User's unique institutional ID                             |
| former_time |  long  | Optional | Time period upper limit, `UNIX` timestamp, Unit: `seconds` |
| latter_time |  long  | Optional | Time period lower limit, `UNIX` timestamp, Unit: `seconds` |

- Response:

```json
{
  "code": 0,
  "msg": "SUCCESS",
  "result": {
    "total": 1,
    "records": [
      {
        "acct_no": "1222",
        "card_type_id": "10010001",
        "status": 2,
        "reason": "{\"code\":1110032,\"msg\":\"KYC failure, photo error\"}",
        "create_time": 1546300800000
      }
    ]
  }
}
```

|    Parameter    |  Type   |                                                    Description                                                     |
| :---------: | :----:   | :----------------------------------------------------------------------------------------------------------------: |
|   acct_no   | String  |                         Institution account name (Unique within scope of the institution)                          |
| card_type_id |String  | Card type id|   
|   status    |  int    | Status code : 0 - Submitted successfully, 1 - Verification successful (Account activated), 2 - Verification failed, 3 - Verifying, 4 - Submission in progress  6 - refund |
| reason | String |      Reason for verification failure.        |
| create_time |  long   |                                                   Creation time                                                    |

> KYC failure reason please see "KYC Failure Error Codes"

## Cards

This API contains methods related to

### Apply a card

- Request:

```text
url：/api/v1/debit-cards
method：POST
```

| Parameter |  Type  | Whether Required |                            Description                            |
| :-------: | :----: | :--------------: | :---------------------------------------------------------------: |
|  acct_no  | String |     Required     | Institution account name (Unique within scope of the institution) |
| card_type_id |String |Required | Card type id|   

- Response:

```json
{
  "code": 0,
  "msg": "string",
  "result": {
    "card_no": "xxxx",
    "card_number": "430021******1144",
    "status": 2
  }
}
```

|  Parameter  |  Type  |     Description     |
| :-----: | :----: | :-----------------: |
| card_no | String | To prevent real card information from being exposed, use 'card_no' parameter to query  |
| card_number | String | Actual card number, Expose only the first 6 and last 4 |
|   status   | int |   status：2. apply card successfully(Not active), 5. apply card failed, card is being made，please apply later           |


### Submit active card attachment

This API only for the card that must submit attachments before activation. Submit one attachment of "poa_doc" and "active_doc".

- Request:

```text
url：/api/v1/debit-cards/attachment
method：POST
```

|  Parameter  | Type  | Whether Required |                        Description                         |
| :---------: | :---: | :--------------: | :-------------------------------------------------------- |
|  card_no  |  String  |    Required     | card no     |
|  poa_doc  |  String[3]  |    Optional     | Picture or PDF of proof of address(Currently not supported). Base64 encoding. Each file size should be less than 2M. If you submited poa_doc in KYC don't need do again.    |
|  active_doc  |  String  |    Optional     | Picture of holding passport and bank card. Base64 encoding. File size should be less than 2M  |

- Response:

```
{
    "code": 0,
    "msg": "SUCCESS",
    "result": true
}
```


### User activating bank card

```text
url：/api/v1/debit-cards/status
method：PUT
```

- Request:

| Parameter |  Type  | Whether Required |                            Description                            |
| :-------: | :----: | :--------------: | :---------------------------------------------------------------: |
|  acct_no  | String |     Required     | Institution account name (Unique within scope of the institution) |
|  card_no  | String |     Required     |                           Bank card no.                           |

- Response:

```json
{
  "code": 0,
  "msg": "string",
  "result": true
}
```



### Query all active card status

```text
url：/api/v1/debit-cards
method：GET
```

|    Parameter    | Type  | Whether Required |                        Description                         |
| :---------: | :---: | :--------------: | :--------------------------------------------------------: |
|  page_num   |  int  |     Required     |                        Page number                         |
|  page_size  |  int  |     Required     |                         Page size                          |
| former_time | long  |     Required     | Time period upper limit, `UNIX` timestamp, Unit: `seconds` |
| latter_time | long  |     Required     | Time period lower limit, `UNIX` timestamp, Unit: `seconds` |
| time_sort | String | Optional| sort time. asc: time in ascending order，desc: time in descending order    |

- Response:

```json
{
  "code": 0,
  "msg": "SUCCESS",
  "result": {
    "total": 1,
    "records": [
      {
        "acct_no": "1",
        "card_no": "4385211202597301",
        "status": 1,
        "reason": "{\"code\":1110061,\"msg\":\"Activation failure, hand holding card is not plastic card\"}",
        "create_time": 1576847136000
      }
    ]
  }
}
```

|    Parameter    |  Type  |                             Description                              |
| :---------: | :----: | :------------------------------------------------------------------: |
|   acct_no   | String |  Institution account name (Unique within scope of the institution)   |
|   card_no   |  int   |                             Card ID                              |
|   status    |  int   | Status code : 0 - Frozen, 1 - Activated successfully, 2 - Not active, 3 - Under review, 4 - Verification failed, 5 - Apply card failed, card is being made，please apply later |
| reason | String |      Reason for activation failure.        |
| create_time |  long  |                            Creation time                             |

### Query a specific user card activation status

- Request:

```text
url：/api/v1/debit-cards?acct_no={acct_no}
method：GET
```

|  Parameter  |  Type  | Whether Required | Description                                                |
| :---------: | :----: | :--------------: | :--------------------------------------------------------- |
|   acct_no   | String |     Required     | User's unique institutional ID                             |
|  page_num   |  int   |     Optional     | Page number                                                |
|  page_size  |  int   |     Optional     | Page size                                                  |
| former_time |  long  |     Optional     | Time period upper limit, `UNIX` timestamp, Unit: `seconds` |
| latter_time |  long  |     Optional     | Time period lower limit, `UNIX` timestamp, Unit: `seconds` |
| time_sort | String | Optional| sort time. asc: time in ascending order，desc: time in descending order    |

- Response:

```json
{
  "code": 0,
  "msg": "SUCCESS",
  "result": {
    "total": 1,
    "records": [
      {
        "acct_no": "1",
        "card_no": "4385211202597301",
        "status": 1,
        "reason": "{\"code\":1110061,\"msg\":\"Activation failure, hand holding card is not plastic card\"}",
        "create_time": 1576847136000
      }
    ]
  }
}
```

|    Parameter    |  Type  |                             Description                              |
| :---------: | :----: | :------------------------------------------------------------------: |
|   acct_no   | String |  Institution account name (Unique within scope of the institution)   |
|   card_no   |  int   |                             Card ID                              |
|   status    |  int   | Status code : 0 - Frozen, 1 - Activated successfully, 2 - Not active, 3 - Under review, 4 - Verification failed, 5 - Apply card failed, card is being made，please apply later |
| reason | String |      Reason for activation failure.        |
| create_time |  long  |                            Creation time                             |



### Request Lock, Unlock, Lost, Renew PIN, Replacement card

```text
url：/api/v1/debit-cards/request
method：PUT
```

- Request

| Parameter |  Type  |   Requirement  |     Description         |
| :------------: | :----: | :----------: |:---------- |
|  acct_no  | String |     Required     | Institution account name (Unique within scope of the institution) |
|  card_no  | String |     Required     |                           Bank card no.                           |
| request_type | int | Required    | 1.Lock 2.Unlock 3.Lost 4.Renew PIN 5.Replacement |
| signature_doc   |  String | Required    | User's hand signature phone, in base64|
| address   |  String | Optional    | User's address |
| phone   |  String | Optional    | User's phone |



- Response：

```json
{
  "code": 0,
  "msg": "string",
  "result": true
}
```

### Query Lock, Unlock, Lost, Renew PIN, Replacement card Status

```text
url：/api/v1/debit-cards/request?request_type={request_type}
method：GET
```

- Request

| Parameter |  Type  |   Requirement  |     Description         |
| :------------: | :----: | :----------: |:---------- |
|  acct_no  | String |     Required     | Institution account name (Unique within scope of the institution) |
|  card_no  | String |     Required     |                           Bank card no.                           |
| request_type | int | Required    | 1.Lock 2.Unlock 3.Lost 4.Renew PIN 5.Replacement  |



- Response：

```json
{
  "code": 0,
  "msg": "string",
  "result": {
        "acct_no": "1",
        "card_no": "4385211202597301",
        "status": 1,
        "create_time": 1576847136000  
  }
}
```

| Parameter  |  Type  |             Description             |
| :--------: | :----: | :------------------------------ |
|  acct_no  | String |    Institution account name (Unique within scope of the institution) |
|  card_no  | String |    Bank card no.                           |
|   status    |  int   | Status code :  0.Pending 1.Successful 2.Failure |
| create_time |  long  |              create time               |



### Query tracking number

```text
url：/api/v1/debit-cards/trackingnumber?month_year={month_year}
method：GET
```

- Request

| Parameter |  Type  |   Requirement  |     Description         |
| :------------: | :----: | :----------: |:---------- |
| month_year | String | Required    | The month of package send. for example："202106"|

or 

| Parameter |  Type  |   Requirement  |     Description         |
| :------------: | :----: | :----------: |:---------- |
|  acct_no  | String |     Required     | Institution account name (Unique within scope of the institution) |
|  card_no  | String |     Required     |                           Bank card no.                           |

- Response：

```json
{
  "code": 0,
  "msg": "string",
  "result": [{
        "acct_no": "1",
        "card_no": "4385211202597301",
        "website": "https://www.fedex.com/fedextrack/",
        "tracking_number": "773998038977",
        "create_time": 1576847136000  
  }]
}
```

| Parameter  |  Type  |             Description             |
| :--------: | :----: | :------------------------------ |
|   acct_no   | String |      Institution account name (Unique within scope of the institution)      |
|   card_no   |  int   |              Bank card no.              |
|   website    |  String   |  logistics company website |
|   tracking_number    |  String   | tracking number of the package|
| create_time |  long  |              create time               |

## Transactions

### User deposit with stablecoin

- Request:

```text
url：/api/v1/deposit-transactions
method：POST
```

| Parameter  |  Type  | Whether Required |                            Description                            |
| :--------: | :----: | :--------------: | :---------------------------------------------------------------: |
|  card_no   | String |     Required     |                           Bank card no.                           |
|  acct_no   | String |     Required     | Institution account name (Unique within scope of the institution) |
|   amount   | String |     Required     |             Deposit amount in corresponding coin_type              |
| coin_type  | String |     Required     |             Only USDT and RUSD supported yet              |
| cust_tx_id | String |     Required     |                    Institution transaction ID                     |
|  remarks   | String |     Optional     |                        Transaction remarks                        |

- Response:

```json
{
    "code": 0,
    "msg": "SUCCESS",
    "result": {
        "tx_id": "2020022511324811001637548",
        "coin_type": "USDT",
        "tx_amount": "0.92",         
        "exchange_fee_rate": "0",
        "exchange_fee": "0",
        "loading_fee": "0.0812",
        "deposit_usdt": "0.9188",
        "currency_type": "USD",
        "currency_amount": "0.92",
        "exchange_rate": "1.00239251357",
        "fiat_exchange_rate": "1"
    }
}
```

| Parameter |  Type    | Description |
| :------------: | :----------: |:---------- |
|     tx_id      | String |  transaction ID  |
|    coin_type    |  int   |          Coin type          |
|   tx_amount   | String |                        Deposit amount                         |
|     exchange_fee_rate      | String | Fee rate for exchanging digital coin to coin_type   |
|     exchange_fee      | String | Fee for exchanging digital coin to coin_type, Unit: coin_type  |
|     loading_fee      | String | Deposit fee, Unit: coin_type   |
|     deposit_usdt      | String | The amount of coin_type deposited for the user after charging loading_fee and exchange_fee, Unit: coin_type   |
|     currency_amount      | String | User received currency amount  |
|     currency_type      | String | It is card supported currency type |
|     exchange_rate      | String |  exchange rate of coin_type/USD  |
| fiat_exchange_rate    | String  | exchange rate of card currency/USD              |

> If coin_type is USDT, USDT amount charged from institution balance = exchange_fee + loading_fee + deposit_usdt.


### User deposit with stablecoin (Fixed amount will be received in fiat)

- Request:

```text
url：/api/v1/deposit-transactions/fiat-amount
method：POST
```

| Parameter  |  Type  | Whether Required |                            Description                            |
| :--------: | :----: | :--------------: | :---------------------------------------------------------------: |
|  card_no   | String |     Required     |                           Bank card no.                           |
|  acct_no   | String |     Required     | Institution account name (Unique within scope of the institution) |
|   credited_amount   | String |     Required     |             Fixed amount will be received in fiat              |
| coin_type  | String |     Required     |             Only USDT supported yet              |
| cust_tx_id | String |     Required     |                    Institution transaction ID                     |
|  remarks   | String |     Optional     |                        Transaction remarks                        |

- Response:

```json
{
    "code": 0,
    "msg": "SUCCESS",
    "result": {
        "tx_id": "2020022511324811001637548",
        "coin_type": "USDT",
        "tx_amount": "0.92",         
        "exchange_fee_rate": "0",
        "exchange_fee": "0",
        "loading_fee": "0.0812",
        "deposit_usdt": "0.9188",
        "currency_type": "USD",
        "currency_amount": "0.92",
        "exchange_rate": "1.00239251357",
        "fiat_exchange_rate": "1"
    }
}
```

| Parameter |  Type    | Description |
| :------------: | :----------: |:---------- |
|     tx_id      | String |  transaction ID  |
|    coin_type    |  int   |          Coin type          |
|   tx_amount   | String |                        Deposit amount                         |
|     exchange_fee_rate      | String | Fee rate for exchanging digital coin to USDT   |
|     exchange_fee      | String | Fee for exchanging digital coin to USDT, Unit: USDT  |
|     loading_fee      | String | Deposit fee，Unit: USDT   |
|     deposit_usdt      | String | The amount of USDT deposited for the user after charging loading_fee and exchange_fee, Unit: USDT   |
|     currency_amount      | String | User received currency amount  |
|     currency_type      | String | It is card supported currency type |
|     exchange_rate      | String |  exchange rate of USDT/USD  |
| fiat_exchange_rate    | String  | exchange rate of card currency/USD              |




### Query Exchange Price

```text
url：/api/v1/deposit-transactions/price
method：GET
```

- Request：


- Response：

```json
{
    "code": 0,
    "msg": "SUCCESS",
    "result": {
        "total": 2,
        "records": [
            {
                "symbol": "USDT/USD",
                "price": "0.99794468",
                "update_time": "2024-04-26 07:19:30"
            },
            {
                "symbol": "USDC/USD",
                "price": "0.99814434",
                "update_time": "2024-04-26 07:19:30"
            }
        ]
    }
}
```

|  Parameter   |  Type  |        Description         |
| :--------: | :----: | :------------------------------ |
|  symbol   | String |         	Exchange symbol        |
|    price    | String | price |
|    update_time    | String | update time |

### Query a deposit transaction status

```text
url：/api/v1/deposit-transactions/{tx_id}/status
method：GET
```

- Request：

| Parameter |  Type  | Whether Required | Description    |
| :-------: | :----: | :--------------: | :------------- |
|   tx_id   | String |     Required     | Transaction ID or cust_tx_id|

- Response：

```json
{
  "code": 0,
  "msg": "string",
  "result": {
                "cust_tx_id": "1223",
                "acct_no": "03030062",
                "card_no": "8993152800000013334",
                "cust_tx_time": 1584350913000,
                "tx_id": "2020031609283339501898843",
                "coin_type": "USDT",
                "tx_amount": "61.86",     
                "exchange_fee": "0",
                "loading_fee": "1.8558",
                "currency_type": "USD",                
                "currency_amount": "60",
                "exchange_rate": "1.00239251357",
                "fiat_exchange_rate": "1",
                "tx_status": 3,
                "coin_price": "1"
  }
}
```

|     Parameter     |  Type  |                            Description                            |
| :-----------: | :----: | :---------------------------------------------------------------: |
|  cust_tx_id   | String |         Customer transaction id         |
|    acct_no    | String | Institution account name (Unique within scope of the institution) |
|    card_no    |  int   |                            Card ID                            |
| cust_tx_time  |  long  |                           Creation time                           |
|  tx_id   | String |        Transaction id         |
|  exchange_fee   | String |     Exchange fee for converting other coin to USDT, Unit: ```coin_type```   |
|      loading_fee      | String |                          Transaction fee, Unit: ```coin_type```                           |
|    coin_type    |  int   |          Coin type          |
|   tx_amount   | String |                        Deposit amount                         |
|     currency_type      | String | Received currency type  |
|     currency_amount      | String | Received currency amount  |
| exchange_rate | String |                           Exchange rate of USDT/USD                       |
| fiat_exchange_rate    | String  | exchange rate of card currency/USD              |
|  tx_status   | int |   Transaction status. 0, 3 and 4:process pending，1: deposit successful, 2 and 5：deposit failed        |
|   coin_price      | String | coin_type/USD rate  |

### Query all the deposit records

```text
url：/api/v1/deposit-transactions
method：GET
```

|    Parameter    | Type  | Whether Required |                        Description                         |
| :---------: | :---: | :--------------: | :--------------------------------------------------------: |
|  page_num   |  int  |     Optional     |                        Page number                         |
|  page_size  |  int  |     Optional     |                         Page size                          |
| former_time | long  |     Optional     | Time period upper limit, `UNIX` timestamp, Unit: `seconds` |
| latter_time | long  |     Optional     | Time period lower limit, `UNIX` timestamp, Unit: `seconds` |
| time_sort | String | Optional| sort time. asc: time in ascending order，desc: time in descending order    |

- Response:

```json
{
    "code": 0,
    "msg": "SUCCESS",
    "result": {
        "total": 1,
        "records": [
            {
                "tx_id": "2020031609283339501898843",
                "cust_tx_id": "1223",
                "card_no": "8993152800000013334",
                "acct_no": "03030062",
                "coin_type": "USDT",
                "tx_amount": "61.86",
                "exchange_fee": "0",
                "loading_fee": "1.8558",
                "currency_type": "CNY",
                "currency_amount": "60",
                "exchange_rate": "1",
                "fiat_exchange_rate": "1",
                "cust_tx_time": 1584350913000,
                "tx_status": 3,
                "coin_price": "1"
            }
        ]
    }
}
```

|     Parameter     |  Type  |                            Description                            |
| :-----------: | :----: | :---------------------------------------------------------------: |
|  cust_tx_id   | String |         Customer transaction id         |
|    acct_no    | String | Institution account name (Unique within scope of the institution) |
|    card_no    |  int   |                            Card ID                            |
| cust_tx_time  |  long  |                           Creation time                           |
|  tx_id   | String |        Transaction id         |
|  exchange_fee   | String |     Exchange fee for converting other coin to USDT, Unit: ```coin_type```    |
|      loading_fee      | String |                          Transaction fee, Unit: ```coin_type```                           |
|    coin_type    |  int   |          Coin type          |
|   tx_amount   | String |                        Deposit amount                         |
|     currency_type      | String | Received currency type  |
|     currency_amount      | String | Received currency amount  |
| exchange_rate | String |                           Exchange rate of USDT/USD                          |
| fiat_exchange_rate    | String  | exchange rate of card currency/USD              |
|  tx_status   | int |   Transaction status. 0, 3 and 4:process pending，1: deposit successful, 2 and 5：deposit failed        |
|   coin_price      | String | coin_type/USD rate  |


### Query a particular user deposit records

```text
url：/api/v1/deposit-transactions?acct_no={acct_no}
method：GET
```

- Request:

|  Parameter  |  Type  | Whether Required | Description                                                |
| :---------: | :----: | :--------------: | :--------------------------------------------------------- |
|   acct_no   | String |     Required     | User's unique institutional ID                             |
|  page_num   |  int   |     Optional     | Page number                                                |
|  page_size  |  int   |     Optional     | Page size                                                  |
| former_time |  long  |     Optional     | Time period upper limit, `UNIX` timestamp, Unit: `seconds` |
| latter_time |  long  |     Optional     | Time period lower limit, `UNIX` timestamp, Unit: `seconds` |
| time_sort | String | Optional| sort time. asc: time in ascending order，desc: time in descending order    |

- Response:

```json
{
    "code": 0,
    "msg": "SUCCESS",
    "result": {
        "total": 1,
        "records": [
            {
                "tx_id": "2020031609283339501898843",
                "cust_tx_id": "1223",
                "card_no": "8993152800000013334",
                "acct_no": "03030062",
                "coin_type": "USDT",
                "tx_amount": "61.86",
                "exchange_fee": "0",
                "loading_fee": "1.8558",
                "currency_type": "CNY",
                "currency_amount": "60",
                "exchange_rate": "1",
                "fiat_exchange_rate": "1",
                "cust_tx_time": 1584350913000,
                "tx_status": 3,
                "coin_price": "1"
            }
        ]
    }
}
```


|     Parameter     |  Type  |                            Description                            |
| :-----------: | :----: | :---------------------------------------------------------------: |
|  cust_tx_id   | String |         Customer transaction id         |
|    acct_no    | String | Institution account name (Unique within scope of the institution) |
|    card_no    |  int   |                            Card ID                            |
| cust_tx_time  |  long  |                           Creation time                           |
|  tx_id   | String |        Transaction id         |
|  exchange_fee   | String |     Exchange fee for converting other coin to USDT, Unit: ```coin_type```    |
|      loading_fee      | String |                          Transaction fee, Unit: ```coin_type```                           |
|    coin_type    |  int   |          Coin type          |
|   tx_amount   | String |                        Deposit amount                         |
|     currency_type      | String | Received currency type  |
|     currency_amount      | String | Received currency amount  |
| exchange_rate | String |                           Exchange rate of USDT/USD                          |
| fiat_exchange_rate    | String  | exchange rate of card currency/USD              |
|  tx_status   | int |   Transaction status. 0, 3 and 4:process pending，1: deposit successful, 2 and 5：deposit failed        |
|   coin_price      | String | coin_type/USD rate  |



## Bank

This API provides details such as transaction records and other account related information.

### Query card status

```text
url：/api/v1/bank/account-status
method：POST
```

- Request:

| Parameter |  Type  | Whether Required | Description |
| :-------: | :----: | :--------------: | :---------- |
|  card_no  | String |     Required     | Card ID |

- Response: 

```json
{
  "code": 0,
  "msg": "string",
  "result": true
}
```


### Query account balance

```text
url：/api/v1/bank/balance
method：POST
```

- Request:

| Parameter |  Type  | Whether Required | Description |
| :-------: | :----: | :--------------: | :---------- |
|  card_no  | String |     Required     | Card ID |

- Response:

```json
{
    "code": 0,
    "msg": "SUCCESS",
    "result": {
        "card_number": "438521******2001",
        "card_type": "EGEN BLUE",
        "current_balance": "148.05",
        "current_balance_usd": "148.05",
        "available_balance": "138.05",
        "available_balance_usd": "138.05"
    }
}
```

|       Parameter       |  Type  | Description     |
| :---------------: | :----: | :-------------- |
|      card_number      | String | Actual card number     |
|     card_type     | String | Card type       |
|  current_balance  | String | Current balance (card currency) |
|  current_balance_usd  | String | Current balance(USD) |
| available_balance | String | Usable balance (card currency)  |
| available_balance_usd | String | Usable balance (USD)  |

### Query transaction records 

```text
url：/api/v1/bank/transaction-record
method：POST
```

- Request:

|     Parameter     |  Type  | Whether Required | Description                                 |
| :---------------: | :----: | :--------------: | :------------------------------------------ |
|      card_no      | String |     Required     | Card ID                                 |
| former_month_year | String |     Required     | Period upper limit month (Format: `012020`) |
| latter_month_year | String |     Required     | Period lower limit month (Format: `012020`) |

- Response:

```json
{
    "code": 0,
    "msg": "SUCCESS",
    "result": [
      {
      	  "month_year":"022020",
          "statement_cycle_date": "28/11/2019",
          "opening_balance": "0.00",
          "opening_balance_usd": "0.00",
          "closing_balance": "150.55",
          "closing_balance_usd": "150.55",
          "available_balance": "N/A",
          "available_balance_usd": "N/A",
          "bank_tx_list": [
              {
                  "transaction_date": "20/11/2019",
                  "posting_date": "20/11/2019",
                  "tx_id": "54675678678",
                  "description": "MONTHLY FEE",
                  "debit": "2.50",
                  "debit_usd": "2.50",                  
                  "credit": "",
                  "credit_usd": "",                  
                  "type": 1
              },
              {
                  "transaction_date": "28/11/2019",
                  "posting_date": "28/11/2019",
                  "tx_id": "54675678677",
                  "description": "MONTHLY FEE",
                  "debit": "2.50",
                  "debit_usd": "2.50",                  
                  "credit": "",
                  "credit_usd": "", 
                  "type": 1
              }
          ]
      }
    ]
 }   
```

|              Parameter               |  Type  | Description                                                        |
| :------------------------------: | :----: | :----------------------------------------------------------------- |
|       month_year                 | String  | query Date,format:MMyyyy                                      |
|       statement_cycle_date       | String | Date for generating statement                                      |
|         opening_balance          | String | Opening balance(card currency)                                                    |
|         opening_balance_usd           | String | Opening balance(USD)                                                    |
|         closing_balance          | String | Closing balance(card currency)                                                    |
|         closing_balance_usd           | String | Closing balance(USD)                                                    |
|        available_balance         | String | Usable balance(card currency)                                                     |
|        available_balance_usd         | String | Usable balance(USD)                                                     |
|         bank_tx_list[n]          | Object | Transaction list                                                   |
| bank_tx_list[0].transaction_date | String | Transaction date                                                   |
|   bank_tx_list[0].posting_date   | String | Transaction record submission date                                 |
|   bank_tx_list[0].tx_id   | String | Transaction ID                                 |
|   bank_tx_list[0].description    | String | Description                                                        |
|      bank_tx_list[0].debit       | String | Debit amount(card currency)                                                       |
|      bank_tx_list[0].debit_usd       | String | Debit amount(USD)                                                       |
|      bank_tx_list[0].credit      | String | Credit amount(card currency)                                                      |
|      bank_tx_list[0].credit_usd      | String | Credit amount(USD)                                                      |
|       bank_tx_list[0].type       |  int   | Transaction type, 1. Debit, 2. Deposit, 3. Withdrawal, 4. Transfer in, 5. Transfer out, 6. other  7. Settlement Adjustment  8. refund |
|   bank_tx_list[0].tx_currency   | String | Actual transaction currency  |
|   bank_tx_list[0].tx_amount   | String | Transaction amount of actual transaction currency  |

### Query card information

- Request:

```text
virtual card url：/api/v1/bank/virtualcard, physical card url: /api/v1/bank/card
method：GET or POST
```

|  Parameter  | Type  | Whether Required |                        Description                         |
| :---------: | :---: | :--------------: | :-------------------------------------------------------- |
| card_no  |  String  |    Required     | card no     |
| publickey  |  String  |  Optional     | We will use the  publickey to encrypt data，or use default publickey. support ECIES and RSA |

- Response:

```
ECIES：
{
    "code": 0,
    "msg": "SUCCESS",
    "result": {
        "encrypt_data": [
            "f48ef2667c8e2d1f80c1e44408099fe7",
            "048ce3a29ac8586e8e7291983ba6ea192d0144c2dec5db147fb9d74ebfc635642bd2051c2932c059e271aab3109c507b92cc2275e4ff03e2548e75a741f6531e58",
            "78263b48508ad00b80552be97c4f74e963de0e722ded84a2c926256b74397b25030ec96f500eeb0bae9d8475c0665b35"
        ],
        "public_key": "031194af2b8ad8cba709509a630dfcc3746c24dfbbe9af48264df5663ad308e16f"
    }
}

RSA：
{
    "code":0,
    "msg":"SUCCESS",
    "result":{
        "encrypt_data":[
            "FwLBNaIiXynp7XagRab3HmQPDnaUv3yXv4ZPzvgJ0MWmJg4I7qug/fcVL/kQNeOrYcDQRycM8teN7l+XoBin4AlOVeHNxYEOxnmuLDWcWl88y18D7JFtvQfBHn+KQ96r8B/IFopmUFbqWKexOdLIp/hPwKHpsJv5Thu3t/JrWDk="],
        "public_key":"MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQCDtxc+w3LlhtF3DcbgS8zqvjYs8Z+s03DrtbQb8iigeMAbKjXLbrWZwX6IprmVRandLuM3PV3UU7L4SCdJZBRUjgEe0q3F/kUe1XwFhsl4aoW9FHKK/d/em50qHSLXw/KPX2HyZ9Ey9G/bV6OwJXvkUYfStBjHNgPlwFca17QhuQIDAQAB"
    }
}

physical card decrypt：
{"card_number":"123456789101212"}

virtualcard decrypt：
{"cvv":"123","card_number":"1001022400001101","expire":"022022"}
```
|    Parameter    |  Type   |      Description                                                     |
| :---------: | :----:   | :--------------------------- |
|  encrypt_data    | String[]  |    encrypted data          |
|  public_key  |  String    |  public key    |

> encryt_data decrypt [RSA example](https://github.com/paycrypto-com/paycrypto-sdk-java/blob/master/src/test/java/com/railone/open/api/test/RSATest.java#L60) or [ECIES example](https://github.com/paycrypto-com/paycrypto-sdk-java/blob/master/src/test/java/com/railone/open/api/test/BankTest.java#L75)


### User triggers a card withdrawal password reset Email (Currently not supported)

An institution invokes the Paycrypto API triggering the action that sends the bank card withdrawal password reset Email to the user's account.

- Request:

```text
url：/api/v1/debit-cards/deposit-pwd-emails?acct_no={acct_no}
method：POST
```

| Parameter |  Type  | Whether Required |                            Description                            |
| :-------: | :----: | :--------------: | :---------------------------------------------------------------: |
|  acct_no  | String |     Required     | Institution account name (Unique within scope of the institution) |
|  card_no  | String |     Required     |                           Bank card no.                           |

- Response:

```json
{
  "code": 0,
  "msg": "string",
  "result": true
}
```

## Verification Code API

### Sending Email verification code

- Request:

```text
url：/api/v1/emails/{email}/verification-codes
method：POST
```

| Parameter |  Type  | Whether Required |  Description  |
| :-------: | :----: | :--------------: | :-----------: |
|   email   | String |     Required     | Email address |

- Response:

```json
{
  "code": 0,
  "msg": "string",
  "result": {
  	"mail_token":"xxxxxx"
  }
}
```

|   Parameter    |  Type  | Description                  |
| :--------: | :----: | :--------------------------- |
| mail_token | String | Allocated verification token |


### Email verification code validation

```text
url：/api/v1/emails/{email}/verification-codes?code={code}&mail_token={mail_token}
method：PUT
```

- Request:

| Parameter  |  Type  | Whether Required |        Description        |
| :--------: | :----: | :--------------: | :-----------------------: |
|   email    | String |     Required     |          E-mail           |
|    code    | String |     Required     |           Code            |
| mail_token | String |     Required     | Allocated verification ID |

- Response:

```json
{
  "code": 0,
  "msg": "string",
  "result": true
}
```

### Sending SMS verification code (Currently not supported)

- Request:

```text
url：/api/v1/mobiles/{mobile}/verification-codes
method：POST
```

| Parameter |  Type  | Whether Required |        Description        |
| :-------: | :----: | :--------------: | :-----------------------: |
|  mobile   | String |     Required     | Area code + Mobile number |

- Response:

```json
{
  "code": 0,
  "msg": "string",
  "result": {
    "mobile_token": "xxxxxx"
  }
}
```

|    Parameter     |  Type  |         Description          |
| :----------: | :----: | :--------------------------: |
| mobile_token | String | Allocated verification token |

### SMS verification code validation (Currently not supported)

```text
url：/api/v1/mobiles/{mobile}/verification-codes?code={code}&mobile_token={mobile_token}
method：PUT
```

- Request:

|  Parameter   |  Type  | Whether Required |        Description        |
| :----------: | :----: | :--------------: | :-----------------------: |
|    mobile    | String |     Required     | Area code + Mobile number |
|     code     | String |     Required     |           Code            |
| mobile_token | String |     Required     | Allocated verification ID |

- Response:

```json
{
  "code": 0,
  "msg": "string",
  "result": true
}
```




## Webhook


After configure the Webhook in the Dashboard, please verify signature when you received events. The data structure of the event is:

| Parameter| Type|Description |
| --- | --- |--- |
| action |String | Event type  |
| events | String[]|  events |
| events[n].params | Object | Event content |
| events[n].create_time|long |  UTC time |
| events[n].id | String | Event id |

The data structure of header：

| Parameter| Type|Description |
| --- | --- |--- |
| Signature |String | Signature  |
| Timestamp | String | Timestamp |

```
--header Timestamp：1585310160226
--header Signature：UqAwtsx9HF3s5yJh/c8luvUITZNXE/f3aujwndnXLBU=

```
How to verify signature ? [Example](https://github.com/paycrypto-com/paycrypto-sdk-java/blob/master/src/test/java/com/railone/open/api/test/NotificationTest.java)


The data structure of response should as follows. **If you response correct data structure, the event will not be send again**:



| Parameter| Type|Description |
| --- | --- |--- |
| code | int   |  0: Successful, other: Failure |
|msg  |String  | code message |


Response Example:

```

{
   "code": 0,
   "msg":"SUCCESS"
}
```



### Push KYC Event

| Parameter| Type|Description |
| --- | --- |--- |
| action  |String| kyc-status |
| events[n].params.acct_no |String | Institution account name (Unique within scope of the institution) |
| events[n].params.card_type_id |String | Card type id |
| events[n].params.status  |int| KYC status, 1. Successful, 2. Failure |


Event example:
```
{
    "action": "kyc-status",
    "events": [
        "{\"id\":\"bc76488ddda4\",\"create_time\":1585293811000,\"params\":{\"card_type_id\":\"50010003\",\"acct_no\":\"032500004\",\"status\":1}}
    ]
}


events[n] element convert string to json:
{
       "id": "bc76488ddda4",
       "create_time": 1585293811000,
       "params":{
           "card_type_id": "50010003",
           "acct_no": "032500004",
           "status": 1
       }
}
```


### Push Card Apply Event

The bank card has been made, please apply.

| Parameter| Type|Description |
| --- | --- |--- |
| action |String  |  card-application|
| events[n].params.acct_no |String | Institution account name (Unique within scope of the institution) |
| events[n].params.card_type_id |String | card type id |

Event example:
```
{
    "action": "card-application-ready",
    "events": [
        "{\"id\":\"bc76488ddda4\",\"create_time\":1585293811000,\"params\":{\"card_type_id\":\"50010003\",\"acct_no\":\"032500004\"}}"
    ]
}

events[n] element convert string to json:
{
       "id": "bc76488ddda4",
       "create_time": 1585293811000,
       "params":{
           "card_type_id": "50010003",
           "acct_no": "032500004"
       }
}
```

### Push Card Activation Event

| Parameter| Type|Description |
| --- | --- |--- |
| action|String  | card-status |
| events[n].params.card_no |String | Institution account name (Unique within scope of the institution) |
| events[n].params.status |int | Card activation status, 0.Frozen, 1.Activated successfully, 4.Activation Failure |

Event example:
```
{
    "action": "card-status",
    "events": [
        "{\"id\":\"bc76488ddda4\",\"create_time\":1585293811000,\"params\":{\"card_no\":\"123434234343\",\"status\":1}}"
    ]
}


events[n] element convert string to json:
{
       "id": "bc76488ddda4",
       "create_time": 1585293811000,
       "params":{
           "card_no": "123434234343",
           "status": 1
       }
}
```

### Push Deposit Event

| Parameter| Type|Description |
| --- | --- |--- |
| action |String |  deposit-status|
| events[n].params.tx_id |String | Transaction ID |
| events[n].params.status  |int| status, 1.Successful, 2.Failure, 5.Canceled |

Event example:
```
{
    "action": "deposit-status",
    "events": [
        "{\"id\":\"bc76488ddda4\",\"create_time\":1585293811000,\"params\":{\"tx_id\":\"2020031609283339501898843\",\"status\":1}}"
    ]
}

events[n] element convert string to json:
{
       "id": "bc76488ddda4",
       "create_time": 1585293811000,
       "params":{
           "tx_id": "2020031609283339501898843",
           "status": 1
       }
}
```

### Test push events

```text
url：/api/v1/events/test
method：POST
```

- Request:

|  Parameter   |  Type  | Whether Required |        Description        |
| :----------: | :----: | :--------------: | :-----------------------: |

- Response:

| Parameter| Type|Description |
| --- | --- |--- |
| action |String |  deposit-status|
| events[n].params |Object | event parameters |

Example：
```
{
    "action": "test",
    "events": [
        "{\"id\":\"bc76488ddda4\",\"create_time\":1585293811000,\"params\":{}}"
    ]
}

events element convert string to json:
{
       "id": "bc76488ddda4",
       "create_time": 1585293811000,
       "params":{
       }
}
```

### Query push failure events

We push events every minute, and up to push 5 times for each event. Failure to push 5 times means the event failed and will not be pushed again, please use ```Query push failure events```.

```text
url：/api/v1/events
method：GET
```

- Request：

| Parameter |  Type  | Requirement  |Description |
| :------------: | :----: | :----------: |:---------- |
|    action     | String | Optional| action name，default is all actions |
|  page_num   | int  |    Optional|Page number      |
|  page_size  | int  |  Optional|Page size   |


- Response：
```
{
  "code": 0,
  "msg": "string",
  "result": {
        "total": 2,
        "records": [{
           "id": "bc7648da4f9c466aa8bad56c3c8ddda4",
           "action": "kyc-status",
           "create_time": 1585293811000,
           "params":{
              "card_type_id": "50010003",
              "acct_no": "032500004",
              "status":1
           }
        }]
  }     
}

```


### Update push failure events

After the update is successful, event was marked as pushed successfully, and can no longer be found through ```Query push failure events```.

```text
url：/api/v1/events
method：PUT
```

- Request：

| Parameter |  Type  | Requirement  |Description |
| :------------: | :----: | :----------: |:---------- |
|    event_id     | String[] | Required|  event id |


```json
{
  "event_id": [
      "bc7648da4f9c466aa8bad56c3c8ddda4",
      "cc7648da4f9c466aa8bad56c3c8ddda3"
  ]
}
```



- Response：

```json
{
  "code": 0,
  "msg": "string",
  "result": true
}
```


### Push Lock, Unlock, Lost, Renew PIN, Replacement card Status


| Parameter |  Type  |Description |
| --- | --- |--- |
| action|String  | card-status |
| events[n].params.card_no |String | Bank card no.  |
| events[n].params.status |int | 0.Pending 1.Successful 2.Failure |
| events[n].request_type | int |  1.Lock 2.Unlock 3.Lost 4.Renew PIN 5.Replacement |

Example：
```
{
    "action": "card-lock-status",
    "events": [
        "{\"id\":\"bc76488ddda4\",\"create_time\":1585293811000,\"params\":{\"card_no\":\"123434234343\",\"request_type\":1,\"status\":1}}"
    ]
}


events element convert string to json:
{
       "id": "bc76488ddda4",
       "create_time": 1585293811000,
       "params":{
           "card_no": "123434234343",
           "request_type": 1,
           "status": 1
       }
}
```

## Error Codes

### Business Logic Error Codes

| Status Code | Description                                                 |
| :---------: | ----------------------------------------------------------- |
|      0      | Succesful                                                   |
|   111001    | Request parameter error                                     |
|   111002    | KYC status abnormal                                         |
|   111003    | KYC failure                                                 |
|   111004    | Duplicate KYC                                               |
|   111005    | KYC record not found                                        |
|   111006    | Specified institution not found                             |
|   111007    | Abnormal institution status                                 |
|   111008    | Institution asset configuration not found                   |
|   111009    | Institution asset status abnormal                           |
|   111010    | Institution configuration not found                         |
|   111011    | Insufficient institution balance                            |
|   111012    | Account not found                                           |
|   111013    | Account status abnormal                                     |
|   111014    | Insufficient accounts of specified account type             |
|   111015    | Account frozen                                              |
|   111016    | ID Account type ID not found                                |
|   111017    | Account already exists                                      |
|   111018    | Internet banking not active                                 |
|   111019    | Account ID already exists                                   |
|   111020    | Only one card can be registered for one particular type     |
|   111021    | No account of the specified type exists for the institution |
|   111022    | Transaction not found                                       |
|   111023    | Unauthorized to query this transaction                      |
|   111024    | Invalid Email format                                        |
|   111025    | Invalid Email verification code                             |
|   111026    | Email already exists                                        |
|   111027    | Email provider not supported                                |
|   111028    | Contact number already exists                               |
|   111029    | Invalid mobile verification code                            |
|   111030    | Unable to fetch bank details                                |
|   111031    | Specified bank not found                                    |
|   111032    | configuration not found                             |
|   111033    | Birthdate field cannot be left empty                        |
|   111034    | Picture could not be uploaded                               |
|   111035    | Max. query time limited to one month                        |
|   111036    | Invalid time format                                         |
|   111037    | Max. bank statement period limited to six months            |

### Identity Authentication Error Codes

| Status Code | Description                 |
| :---------: | --------------------------- |
|   112001    | Request timed out           |
|   112002    | Illegal access privileges   |
|   112003    | Invalid IP address          |
|   112004    | Invalid timestamp           |
|   112005    | Verification failure        |
|   112006    | Invalid verification format |
|   112007    | Invalid signature           |
|   112008    | Specified app key not found |
|   112009    | Invalid app key secret      |
|   112010    | Invalid request header      |

### Abnormal Status Error Codes

| Status Code | Description           |
| :---------: | --------------------- |
|   119001    | Service unusable      |
|   119002    | Communication error   |
|   119003    | Data encryption error |
|   119004    | Data decryption error |
|   119005    | Too many API requests |
|   119006    | Unauthorized API      |
|   119007    | Public key format error |


### KYC Failure Error Codes

| Status Code | Description           |
| :--------------: | --------| 
|   111003 |   KYC failure   |
|   1110031 |   KYC failure, country or nationality error   |
|   1110032|   KYC failure, certificate photo error or photo is not clear  |
|   1110033 |   KYC failure, certificate photo in hand error   |
|   1110034 |   KYC failure, address error   |
|   1110035 |   KYC failure, mobile error   |
|   1110036 |   KYC failure, birthday error   |
|   1110037 |   KYC failure, user name error   |
|   1110038 |   KYC failure, zipcode error   |
|   1110039 |   KYC failure, other error   |
|   1110040 |   KYC failure, your passport has expired or will expire in 6 month   |
|   1110041 |   KYC failure, the passport is not clear or not complete   |
|   1110042 |   KYC failure, gender filled error   |
|   1110043 |   KYC failure, selfie photo is not clear or not complete   |
|   1110044 |   KYC failure, the passport text in the selfie photo is reversed   |
|   1110045 |   KYC failure, your face is not exposed in the selfie photo |

KYC Failure reason filled in parameter ```reason```, the content for example:

```
{
   "code": 1110032,
   "msg": "KYC failure, photo error"
}
```


###  Card Activation Failure Error Codes

| Status Code | Description           |
| :--------------: | --------|
|   1110060 |   Activation failure  |
|   1110061 |   Activation failure, hand holding card is not plastic card   |
|   1110062|   Activation failure, your face is not exposed in the selfie photo   |
|   1110063 |   Activation failure, selfie photo is not clear or not complete   |
|   1110064 |   Activation failure, did not cover the chip slot with your finger   |
|   1110065 |   Activation failure, please take a color photo   |
|   1110066 |   Activation failure, passport text in selfie photo not clear   |

Card Activation Failure reason filled in parameter ```reason```, the content for example:

```
{
   "code": 1110061,
   "msg": "Activation failure, hand holding card is not plastic card"
}
```