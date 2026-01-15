# Native MM API Documentation

* [Native API](#Native-API)
* [API Specifications](#API-Specifications)
* [MM](#MM)
* [Webhook](#Webhook)   
     * [Test push events](#Test-push-events)
* [Error Codes](#error-codes)



## Native API


Welcome to the Native API documentation. This document is aimed at Native MM business. 


> It is recommended to testing in the test environment before using the production environment.

Dashboard :

* Test environment dashboard (restricted by IP whitelist): https://mm-sandbox.native.financial/
* Production environment dashboard (restricted by IP whitelist): https://mm.native.financial/

API :

* Test environment: https://mmapi-sandbox.paycrypto.com/
* Production environment: https://mmapi.paycrypto.com/

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
  "from": "354245345",
  "amount": 190,
  "to": "dfgdfg"
}
```

The payload is converted to-

```text
amount=190&from=354245345&to=dfgdfg
```

#### Code Implementation and Examples:

For illustrative sample code, please refer to [https://github.com/paycrypto-com/paycrypto-sdk-java](https://github.com/paycrypto-com/paycrypto-sdk-java)


## MM


### asset code list

```text
url：/api/v1/mm/customer/asset
method：GET
```

- Response:

```json
{
    "code": 0,
    "msg": "SUCCESS",
    "result":  [
        {
            "network": "Ethereum",
            "asset_type": "CRYPTO",
            "asset_code": "USDT",
            "uni_asset_code": "USDT_ERC20",
        {
            "network": "Tron",
            "asset_type": "CRYPTO",
            "asset_code": "USDT",
            "uni_asset_code": "USDT_TRC20",
        },
        {
            "network": "GL BANK Name",
            "asset_type": "FIAT",
            "asset_code": "USD",
            "uni_asset_code": "USD_GLBANK",
        }
  ]
}
```

### Query customer asset 

```text
url：/api/v1/mm/customer/asset
method：GET
```

- Response:

```json
{
    "code": 0,
    "msg": "SUCCESS",
    "result": {
        "cust_asset_info_list": [
            {
                "network": "Ethereum",
                "asset_type": "CRYPTO",
                "asset_code": "USDT",
                "uni_asset_code": "USDT_ERC20",
                "account_no": "0xa28B0Ac939FC6BaAaDC79a94f425345C60463417",
                "address": "0xa28B0Ac939FC6BaAaDC79a94f425345C60463417",
                "total_balance": 90,
                "available_balance": 90,
                "frozen_balance": 0,
                "status": 1
            },
            {
                "network": "Tron",
                "asset_type": "CRYPTO",
                "asset_code": "USDT",
                "uni_asset_code": "USDT_TRC20",
                "account_no": "TQn2MYFvDxzHDiLRFsjAXbphfux53RGTFA",
                "address": "TQn2MYFvDxzHDiLRFsjAXbphfux53RGTFA",
                "total_balance": 10,
                "available_balance": 10,
                "frozen_balance": 0,
                "status": 1
            },
            {
                "network": "GL BANK Name",
                "asset_type": "FIAT",
                "asset_code": "USD",
                "uni_asset_code": "USD_GLBANK",
                "account_no": "704637748842995188",
                "address": "704637748842995188",
                "total_balance": 10000,
                "available_balance": 10000,
                "frozen_balance": 0,
                "status": 1
            }

        ],
        "sum_asset_list": [
            {
                "asset_code": "USDT",
                "total_balance": 100,
                "available_balance": 100,
                "frozen_balance": 0,
            },
                        {
                "asset_code": "USD",
                "total_balance": 10000,
                "available_balance": 10000,
                "frozen_balance": 0,
            }
        ],
        "total_balance": "10100",
        "total_balance_currency": "USD"
  }
}
```

| Parameter |  Type  |          Description          |
| :--------: | :----: | :------------------------------ |
|   total_balance   | String |        total_balance              |



### Deposit asset

- Request:

```text
url：/api/v1/mm/customer/asset/deposit
method：POST
```

|  Parameter  | Type  | Whether Required |                        Description                         |
| :---------: | :---: | :--------------: | :-------------------------------------------------------- |
| uni_asset_code  |  String  |    Required     | uni_asset_code, USDT_TRC20、USDT_ERC20     |
| tx_hash  |  String  |    Required     | tx_hash    |

- Response:

```
{
    "code": 0,
    "msg": "SUCCESS",
    "result": true
}
```

### Query exchange rate

```text
url：/api/v1/mm/customer/asset/exchange-rate
method：GET
```

- Request：

| Parameter |  Type  |   Requirement  | Description   |
| :------------: | :----: | :----------: |:---------- |
| from_asset | String |Optional| from_asset |
| to_asset | String |Optional| to_asset |

- Response：

```json
{
    "code": 0,
    "msg": "SUCCESS",
    "result": [{
        {
            "from_asset": "USDT",
            "to_asset": "USD",
            "exchange_rate": "0.9999"
        },
        {
            "from_asset": "USDC",
            "to_asset": "USD",
            "exchange_rate": "0.9999"
        }
    }]
}
```

| Parameter |  Type  |          Description          |
| :--------: | :----: | :------------------------------ |


### exchange asset

- Request:

```text
url: /api/v1/mm/customer/asset/exchange/order
method：POST
```

|  Parameter  | Type  | Whether Required |                        Description                         |
| :---------: | :---: | :--------------: | :-------------------------------------------------------- |
|  from_uni_asset_code  |  String  |    Required     |  USDT_ERC20     |
|  to_uni_asset_code  |  String  |    Required     |  USD     |
|  amount  |  String  |    Required     |  10000    |

- Response:

```
{
    "code": 0,
    "msg": "SUCCESS",
    "result": {
    }
}
```

|    Parameter    |  Type   |      Description                                                     |
| :---------: | :----:   | :--------------------------- |


### exchange asset history

```text
url：/api/v1/mm/customer/asset/exchange/history
method：GET
```

- Request：

| Parameter |  Type  |   Requirement  | Description   |
| :------------: | :----: | :----------: |:---------- |
|  page_num   |  int  |     Optional     |                        Page number                         |
|  page_size  |  int  |     Optional     |                         Page size                          |
| former_time | long  |     Optional     | Time period upper limit, `UNIX` timestamp, Unit: `seconds` |
| latter_time | long  |     Optional     | Time period lower limit, `UNIX` timestamp, Unit: `seconds` |

- Response：

```json
{
    "code": 0,
    "msg": "SUCCESS",
    "result": {
        "page_num": 1,
        "page_size": 10,
        "total": 1,
        "pages": 1,
        "records": [
            {
                "id": 7,
                "tx_id": "2011710781042962432",
                "business_ref_no": "EX_2011710780833247232",
                "account_no": "704637748842995188",
                "address": "704637748842995188",
                "from_uni_asset_code": "USDT_ERC20",
                "to_uni_asset_code": "USD",
                "transaction_type": "sell",
                "amount": "1000",
                "fee": "3",
                "status": "INIT",
                "reason": "",
                "remark": "",
                "created_at": 1768464200000,
                "updated_at": 1768464200000
            }          
        ]
    }

}
```

| Parameter |  Type  |          Description          |
| :--------: | :----: | :------------------------------ |





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


## Error Codes


### Business Logic Error Codes

| Status Code | Description                                                 |
| :---------: | ----------------------------------------------------------- |
|      0      | Succesful                                                   |
|   111001    | Request parameter error                                     |


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
