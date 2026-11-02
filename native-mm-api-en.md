# Native MM API Documentation

* [Native API](#Native-API)
* [API Specifications](#API-Specifications)
* [MM](#MM)
     * [Asset code list](#asset-code-list)
     * [Query customer asset](#query-customer-asset)
     * [Deposit asset](#deposit-asset)
     * [Query exchange rate](#query-exchange-rate)
     * [Sell receiver list](#sell-receiver-list)
     * [Sell crypto](#sell-crypto)
     * [Whitelist address list](#whitelist-address-list)
     * [Buy crypto](#buy-crypto)
     * [Exchange history](#exchange-history)
     * [Exchange order (Deprecated)](#exchange-order-deprecated)
     * [Apply for virtual account](#apply-for-virtual-account)
     * [Query virtual account list](#query-virtual-account-list)
* [Webhook](#Webhook)
     * [Test push events](#Test-push-events)
* [Error Codes](#error-codes)



## Native API


Welcome to the Native API documentation. This document is aimed at Native MM business.


> It is recommended to test in the sandbox environment before using the production environment.

Dashboard :

* Test environment dashboard (restricted by IP whitelist): https://mm-sandbox.native.financial/
* Production environment dashboard (restricted by IP whitelist): https://mm.native.financial/

API :

* Test environment: https://mmapi-sandbox.native.financial/
* Production environment: https://mmapi.native.financial/

> **Scope note** — Configuration features such as API key management, whitelist (withdrawal) addresses, sell receivers (payees) and Webhook settings are created and managed in the **Dashboard**; the Open API only provides read-only list queries for them ([Sell receiver list](#sell-receiver-list), [Whitelist address list](#whitelist-address-list)) so that you can obtain the `receive_id` for sell orders and the receiving `address` for buy orders. The Open API covers: asset code list, balance/asset query, deposit (corporate top-up), exchange rate query, sell, buy, receiver/whitelist list query, and history query.

## API Specifications

- API requests use `HMAC` authentication.

- **Pagination** - Query record lists are all divided into pages. Pagination parameters: `page_num` represents the page number (starting from `1`), `page_size` represents the size of each page. The response uniformly returns `total` and `records`.

- **Country** - Two digit country codes, refer to `ISO 3166-1 alpha-2` standards.

- **Time** - Time-range request parameters (`former_time`, `latter_time`) and the `tx_time` response field are `UNIX` timestamps in **milliseconds**. Date-time response fields (`created_at`, `updated_at`, `create_time`, `update_time`) are returned as ISO-8601 UTC strings (e.g. `2026-01-15T08:03:20.000+00:00`).

- **Amount** - All amounts in API requests and responses are of the `string` data type in order to avoid precision loss.

- All the requests that have a `body` but don't explicitly define a format are of `JSON` type, `Content-Type: application/json`.

- The query interval of all query interfaces must be **less than one month**.

- API response format standard-

  | Parameter  |  Type  |                               Description                               |
  | :----: | :----: | :---------------------------------------------------------------------: |
  |  code  |  int   |               Error code. `0`: Normal, non-`0`: Abnormal                |
  |  msg   | string | `SUCCESS` indicates success, error code indicates and describes failure |
  | result | object |                                 Result                                  |

### HMAC Authentication

The institution first needs to apply for the API `key` and API `secret` that will be used when accessing the API (created and managed in the Dashboard).

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

Base path of all MM endpoints: `/api/v1/mm/customer/asset`

| # | Method | Path | Description |
| :-: | --- | --- | --- |
| 1 | GET | `/list` | Asset code list |
| 2 | GET | `` | Query customer asset (balance) |
| 3 | POST | `/deposit` | Deposit asset (corporate top-up by tx_hash) |
| 4 | GET | `/exchange-rate` | Query exchange rate |
| 5 | GET | `/exchange/sell/receiver` | Sell receiver (payee) list; the returned `bank_acct_id` is the `receive_id` of sell |
| 6 | POST | `/exchange/sell` | Sell crypto (crypto → fiat) |
| 7 | GET | `/exchange/buy/whitelist-address` | Whitelist address list; the returned `address` is the receiving `address` of buy |
| 8 | POST | `/exchange/buy` | Buy crypto (fiat → crypto, by spend amount) |
| 9 | GET | `/exchange/history` | Exchange history (deposits / sell / buy) |
| 10 | POST | `/exchange/order` | ⚠️ Deprecated, kept for backward compatibility |


### Asset code list

Lists all assets supported by the MM business.

```text
url：/api/v1/mm/customer/asset/list
method：GET
```

- Response:

```json
{
    "code": 0,
    "msg": "SUCCESS",
    "result": [
        {
            "network": "Ethereum",
            "asset_type": "CRYPTO",
            "asset_code": "USDT",
            "uni_asset_code": "USDT_ERC20"
        },
        {
            "network": "Tron",
            "asset_type": "CRYPTO",
            "asset_code": "USDT",
            "uni_asset_code": "USDT_TRC20"
        },
        {
            "network": "GL BANK Name",
            "asset_type": "FIAT",
            "asset_code": "USD",
            "uni_asset_code": "USD_GLBANK"
        }
    ]
}
```

| Parameter |  Type  | Description |
| :--------: | :----: | :---------- |
| network | String | Network / bank name of the asset |
| asset_type | String | `CRYPTO` or `FIAT` |
| asset_code | String | Asset code, e.g. `USDT`, `USD` |
| uni_asset_code | String | Unified asset code, e.g. `USDT_ERC20`, `USDT_TRC20`, `USD_GLBANK` |


### Query customer asset

Query the asset balances of the customer.

```text
url：/api/v1/mm/customer/asset
method：GET
```

- Request：

| Parameter |  Type  | Requirement | Description |
| :--------: | :----: | :---------: | :---------- |
| currency | String | Optional | Display currency of `total_balance`, default `USD` |

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
                "total_balance": "90",
                "available_balance": "90",
                "frozen_balance": "0",
                "status": 1
            },
            {
                "network": "Tron",
                "asset_type": "CRYPTO",
                "asset_code": "USDT",
                "uni_asset_code": "USDT_TRC20",
                "account_no": "TQn2MYFvDxzHDiLRFsjAXbphfux53RGTFA",
                "address": "TQn2MYFvDxzHDiLRFsjAXbphfux53RGTFA",
                "total_balance": "10",
                "available_balance": "10",
                "frozen_balance": "0",
                "status": 1
            },
            {
                "network": "GL BANK Name",
                "asset_type": "FIAT",
                "asset_code": "USD",
                "uni_asset_code": "USD_GLBANK",
                "account_no": "704637748842995188",
                "address": "704637748842995188",
                "total_balance": "10000",
                "available_balance": "10000",
                "frozen_balance": "0",
                "status": 1
            }
        ],
        "sum_asset_list": [
            {
                "asset_code": "USDT",
                "total_balance": "100",
                "available_balance": "100",
                "frozen_balance": "0"
            },
            {
                "asset_code": "USD",
                "total_balance": "10000",
                "available_balance": "10000",
                "frozen_balance": "0"
            }
        ],
        "total_balance": "10100",
        "total_balance_currency": "USD"
    }
}
```

| Parameter |  Type  | Description |
| :--------: | :----: | :---------- |
| cust_asset_info_list | Array | Per-asset balance detail list |
| sum_asset_list | Array | Balances aggregated by asset code |
| total_balance | String | Total balance converted to `total_balance_currency` |
| total_balance_currency | String | Display currency of `total_balance` |


### Deposit asset

Corporate top-up. After transferring crypto to your deposit address (shown in the Dashboard / asset query), submit the on-chain `tx_hash` through this endpoint. The balance is credited after on-chain verification.

```text
url：/api/v1/mm/customer/asset/deposit
method：POST
```

- Request:

|  Parameter  | Type  | Requirement | Description |
| :---------: | :---: | :---------: | :---------- |
| uni_asset_code | String | Required | Unified asset code. Supported: `USDT_TRC20`, `USDT_ERC20`, `USDC_ERC20` |
| tx_hash | String | Required | On-chain transaction hash. The `to` address of the transaction must be your deposit address; a hash can only be declared once |

- Response:

```json
{
    "code": 0,
    "msg": "SUCCESS",
    "result": true
}
```


### Query exchange rate

Query the exchange rate calculated from the advertiser (quote) configured for your account: **`exchange_rate = 1 - fee_rate`**, where `fee_rate` is the trade fee rate of the matched advertiser (sell quotes for crypto → fiat, buy quotes for fiat → crypto), including the market-price-covered adjustment when configured. If no advertiser matches the requested asset pair, an empty list is returned.

```text
url：/api/v1/mm/customer/asset/exchange-rate
method：GET
```

- Request：

| Parameter |  Type  | Requirement | Description |
| :--------: | :----: | :---------: | :---------- |
| from_asset | String | Optional | From asset. Crypto must be a uni asset code such as `USDT_ERC20`, `USDT_TRC20`, `USDC_ERC20` (the `USDT-ERC20` hyphen form is also accepted); fiat uses its currency code, e.g. `USD`. When `from_asset` is crypto, sell quotes (crypto → fiat) are matched; when it is fiat, buy quotes (fiat → crypto) are matched |
| to_asset | String | Optional | To asset, same format rules as `from_asset`. If both are provided, one must be fiat and the other crypto; if both are empty, the rates of all quotes configured for your account are returned |

- Response：

```json
{
    "code": 0,
    "msg": "SUCCESS",
    "result": [
        {
            "from_asset": "USDT_ERC20",
            "to_asset": "USD",
            "exchange_rate": "0.997"
        },
        {
            "from_asset": "USD",
            "to_asset": "USDT_ERC20",
            "exchange_rate": "0.997"
        }
    ]
}
```

| Parameter |  Type  | Description |
| :--------: | :----: | :---------- |
| from_asset | String | From asset (uni asset code for crypto, e.g. `USDT_ERC20`) |
| to_asset | String | To asset (uni asset code for crypto, e.g. `USDT_ERC20`) |
| exchange_rate | String | `1 - fee_rate` of the matched advertiser quote. The final executed rate/fee of a sell/buy order is determined by the matched quote and fee configuration at order time |


### Sell receiver list

List the sell receivers (payees) of your account. Receivers are created and approved in the Dashboard; use the returned `bank_acct_id` as the `receive_id` of [Sell crypto](#sell-crypto).

```text
url：/api/v1/mm/customer/asset/exchange/sell/receiver
method：GET
```

- Request：

| Parameter |  Type  | Requirement | Description |
| :--------: | :----: | :---------: | :---------- |
| page_num | int | Optional | Page number, default `1` |
| page_size | int | Optional | Page size, default `10` |
| currency | String | Optional | Filter by supported receive currency, e.g. `USD` (also matches receivers supporting multiple comma-separated currencies) |
| status | int | Optional | Filter by review status: `0` reviewing, `1` approved (usable for sell), `2` rejected, `3` to be modified, `4` incomplete |

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
                "bank_acct_id": "20118001",
                "label": "USD payee",
                "account_type": "enterprise",
                "bank_name": "GL BANK",
                "bank_account": "704637748842995188",
                "swift_code": "GLDBSGSG",
                "currency": "USD",
                "status": 1,
                "create_time": "2026-01-15T08:03:20.000+00:00",
                "update_time": "2026-01-15T08:03:20.000+00:00"
            }
        ]
    }
}
```

| Parameter |  Type  | Description |
| :--------: | :----: | :---------- |
| bank_acct_id | String | Receiver id returned by the bank. Pass it as `receive_id` when placing a sell order |
| label | String | Receiver nickname |
| account_type | String | `individual` or `enterprise` |
| bank_name | String | Bank name |
| bank_account | String | Bank account / IBAN |
| swift_code | String | SWIFT code |
| currency | String | Supported receive currencies, may be comma-separated (e.g. `USD,EUR`) |
| status | int | Review status. Only `1` (approved) receivers can be used in sell orders |
| create_time / update_time | String | Creation / update time, ISO-8601 (UTC) |


### Sell crypto

Sell crypto and receive fiat (crypto → fiat). The fiat is paid out to your **receiver (payee)** which must be configured and approved in the Dashboard beforehand.

```text
url：/api/v1/mm/customer/asset/exchange/sell
method：POST
```

- Request:

|  Parameter  | Type  | Requirement | Description |
| :---------: | :---: | :---------: | :---------- |
| from_uni_asset_code | String | Required | Crypto to sell, e.g. `USDT_TRC20`, `USDT_ERC20` |
| to_asset | String | Required | Fiat to receive, e.g. `USD` |
| crypto_amount | String | Required | Amount of crypto to sell |
| receive_id | Long | Optional | `bank_acct_id` of an approved receiver (payee), obtained via [Sell receiver list](#sell-receiver-list) (receivers are configured and approved in the Dashboard). If omitted, the approved receiver matching `to_asset` is used; if several match, the request fails and you must specify one |
| source_of_fund | String | Required | Source of funds declaration |
| purpose_of_trans | String | Required | Purpose of transaction declaration |
| remark | String | Optional | Order remark |

- Request example:

```json
{
    "from_uni_asset_code": "USDT_TRC20",
    "to_asset": "USD",
    "crypto_amount": "1000",
    "source_of_fund": "Business Revenue",
    "purpose_of_trans": "Settlement",
    "remark": "October settlement"
}
```

- Response:

```json
{
    "code": 0,
    "msg": "SUCCESS",
    "result": {
        "tx_id": "2011710781042962432",
        "from_uni_asset_code": "USDT_TRC20",
        "from_amount": "1000",
        "to_uni_asset_code": "USD",
        "to_amount": "997",
        "transaction_type": "sell",
        "fee": "3",
        "status": "INIT",
        "created_at": "2026-01-15T08:03:20.000+00:00",
        "updated_at": "2026-01-15T08:03:20.000+00:00"
    }
}
```

- Amount and fee: the fee is deducted from the crypto amount (`to_amount = crypto_amount − fee`, converted at the executed rate). The order is created with status `INIT` and is settled after platform review; you are notified by Webhook, or you can poll [Exchange history](#exchange-history).


### Whitelist address list

List the whitelist (withdrawal) addresses of your account. Whitelist addresses are created and approved in the Dashboard; use the returned `address` as the receiving `address` of [Buy crypto](#buy-crypto).

```text
url：/api/v1/mm/customer/asset/exchange/buy/whitelist-address
method：GET
```

- Request：

| Parameter |  Type  | Requirement | Description |
| :--------: | :----: | :---------: | :---------- |
| page_num | int | Optional | Page number, default `1` |
| page_size | int | Optional | Page size, default `10` |
| uni_asset_code | String | Optional | Filter by crypto, e.g. `USDT_TRC20` |
| status | int | Optional | Filter by status. `1` = enabled (usable for buy) |

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
                "id": 101,
                "uni_asset_code": "USDT_TRC20",
                "chain": "Tron",
                "address": "TQn2MYFvDxzHDiLRFsjAXbphfux53RGTFA",
                "address_name": "main tron address",
                "status": 1,
                "create_time": "2026-01-15T08:03:20.000+00:00",
                "update_time": "2026-01-15T08:03:20.000+00:00"
            }
        ]
    }
}
```

| Parameter |  Type  | Description |
| :--------: | :----: | :---------- |
| id | int | Whitelist address record id |
| uni_asset_code | String | Uni asset code, e.g. `USDT_TRC20` |
| chain | String | Chain name, e.g. `Ethereum`, `Tron` |
| address | String | Whitelist address. Pass it as `address` when placing a buy order |
| address_name | String | Address label |
| status | int | Only `1` (enabled) addresses can be used in buy orders |
| create_time / update_time | String | Creation / update time, ISO-8601 (UTC) |


### Buy crypto

Buy crypto with fiat (fiat → crypto), by **spend amount**: you specify the total fiat amount to spend, the fee is deducted **from within** the spend amount, and the crypto you receive is `(spend_amount − fee) / rate`. The crypto is sent to your **whitelist address** which must be configured and approved in the Dashboard beforehand. For orders paid by bank transfer, upload the payment receipt in the Dashboard after placing the order.

```text
url：/api/v1/mm/customer/asset/exchange/buy
method：POST
```

- Request:

|  Parameter  | Type  | Requirement | Description |
| :---------: | :---: | :---------: | :---------- |
| from_asset | String | Required | Fiat to spend, e.g. `USD` |
| to_uni_asset_code | String | Required | Crypto to receive, e.g. `USDT_TRC20`, `USDT_ERC20` |
| spend_amount | String | Required | Total fiat amount to spend (**fee included**) |
| address | String | Optional | Receiving crypto address. Must be an approved whitelist address of your account, obtained via [Whitelist address list](#whitelist-address-list) (whitelist addresses are configured and approved in the Dashboard). If omitted and exactly one approved whitelist address exists for the crypto, it is used; otherwise the request fails and you must specify one |
| source_of_fund | String | Required | Source of funds declaration |
| purpose_of_trans | String | Required | Purpose of transaction declaration |
| remark | String | Optional | Order remark |

- Request example:

```json
{
    "from_asset": "USD",
    "to_uni_asset_code": "USDT_TRC20",
    "spend_amount": "10000",
    "source_of_fund": "Business Revenue",
    "purpose_of_trans": "Settlement"
}
```

- Response:

```json
{
    "code": 0,
    "msg": "SUCCESS",
    "result": {
        "tx_id": "2011710781042962433",
        "from_uni_asset_code": "USD",
        "from_amount": "10000",
        "to_uni_asset_code": "USDT_TRC20",
        "to_amount": "9970",
        "transaction_type": "buy",
        "fee": "30",
        "status": "INIT",
        "created_at": "2026-01-15T08:03:20.000+00:00",
        "updated_at": "2026-01-15T08:03:20.000+00:00"
    }
}
```

> Note: the legacy endpoint `POST /api/v1/mm/customer/asset/exchange/order` (crypto-amount based) is deprecated. New integrations must use `/exchange/buy` (spend based) and `/exchange/sell`.


### Exchange history

Query deposit / sell / buy history records.

```text
url：/api/v1/mm/customer/asset/exchange/history
method：GET
```

- Request：

| Parameter |  Type  | Requirement | Description |
| :--------: | :----: | :---------: | :---------- |
| page_num | int | Optional | Page number, default `1` |
| page_size | int | Optional | Page size, default `20` |
| type | String | Optional | Record type: `deposit`, `sell`, `buy`. Empty = all types |
| former_time | long | Optional | Time period upper limit, `UNIX` timestamp, unit: `milliseconds` |
| latter_time | long | Optional | Time period lower limit, `UNIX` timestamp, unit: `milliseconds` |

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
                "order_no": "2011710781042962432",
                "chain": "Tron",
                "coin_type": "USDT",
                "address": "TQn2MYFvDxzHDiLRFsjAXbphfux53RGTFA",
                "from": "TQn2MYFvDxzHDiLRFsjAXbphfux53RGTFA",
                "to": "704637748842995188",
                "tx_hash": "",
                "tx_time": "1768464200000",
                "amount": "1000",
                "remark": "",
                "before_balance": "0",
                "after_balance": "0",
                "create_time": "2026-01-15T08:03:20.000+00:00",
                "update_time": "2026-01-15T08:03:20.000+00:00",
                "tx_status": 0,
                "trade_type": 0,
                "record_type": "out"
            }
        ]
    }
}
```

| Parameter |  Type  | Description |
| :--------: | :----: | :---------- |
| order_no | String | Order number (`tx_id` of the sell/buy order; deposit orders use their deposit reference) |
| record_type | String | `deposit`: deposit record; `out`: sell order; `in`: buy order |
| trade_type | int | `0`: sell, `1`: buy (only for sell/buy records) |
| tx_status | int | Order status. Sell/buy: `0` submitted (pending platform review), `1` success, `2` paying out, `4` failed. Deposit: on-chain credit status |
| amount | String | Crypto amount of the record |
| tx_time | String | Last status-change time, `UNIX` timestamp in `milliseconds` |
| create_time / update_time | String | Creation / update time, ISO-8601 (UTC) |


### Exchange order (Deprecated)

> ⚠️ **Deprecated** — kept for backward compatibility only. This endpoint mixes sell and buy in one request and infers the direction from the asset categories (crypto → fiat = sell, fiat → crypto = buy). New integrations must use [Sell crypto](#sell-crypto) and [Buy crypto](#buy-crypto) instead.

```text
url：/api/v1/mm/customer/asset/exchange/order
method：POST
```

|  Parameter  | Type  | Requirement | Description |
| :---------: | :---: | :---------: | :---------- |
| from_uni_asset_code | String | Required | Asset to pay, e.g. `USDT_TRC20` (sell) or `USD` (buy) |
| to_uni_asset_code | String | Required | Asset to receive, e.g. `USD` (sell) or `USDT_ERC20` (buy) |
| crypto_amount | String | Required | Crypto amount |
| source_of_fund | String | Required | Source of funds declaration |
| purpose_of_trans | String | Required | Purpose of transaction declaration |

- Response: same structure as [Sell crypto](#sell-crypto).




### Apply for virtual account

Apply for a virtual account. This API only creates a **local application record** with review status `0-PENDING_REVIEW`; the platform ops team then reviews the application, and the virtual account is created at the upstream bank after approval. Use `GET /api/v1/mm/va/list` to poll the review result and the assigned account number.

```text
url：/api/v1/mm/va/apply
method：POST
```

- Request (common fields):

|  Parameter  | Type  | Whether Required |                        Description                         |
| :---------: | :---: | :--------------: | :-------------------------------------------------------- |
| currency  |  String  |    Required     | Virtual account currency, e.g. `USD`     |
| mail  |  String  |    Optional     | Contact email (corporate: please provide a corporate email). Defaults to the registered email of the authenticated customer    |
| client_type  |  String  |    Required     | Account holder type: `retail` / `corporate`     |
| full_name  |  String  |    Optional     | Account holder full name (individual name / registered company name). Auto-composed from `first_name` + `middle_name` + `last_name` when blank    |
| nationality  |  String  |    Required when `client_type=retail`     | Nationality, 3-letter country code    |
| country  |  String  |    Required     | 3-letter country code. `retail`: country of residence; `corporate`: country of incorporation    |
| city  |  String  |    Required     | City    |
| address  |  String  |    Optional     | Full address. Auto-composed from `address_line1` + `address_line2` + `state_province` + `postal_code` when blank    |
| date  |  String  |    Required     | Format `ddMMyyyy`. Date of birth for `retail`, date of incorporation for `corporate`    |
| id_type  |  String  |    Required when `client_type=retail`     | `P`-Passport, `N`-National ID    |
| id_number  |  String  |    Required     | ID number for `retail` / business registration number for `corporate`    |
| corporate_type  |  String  |    Optional     | `FI` / `NON_FI`, only for `corporate`. Defaults to `NON_FI` when blank    |

- Request (extended fields, SGB Client's Required Info 2026-10-13):

|  Parameter  | Type  | Whether Required |                        Description                         |
| :---------: | :---: | :--------------: | :-------------------------------------------------------- |
| first_name  |  String  |    Required     | First name. `retail`: of the individual; `corporate`: of the contact person    |
| middle_name  |  String  |    Optional     | Middle name    |
| last_name  |  String  |    Required     | Last name. `retail`: of the individual; `corporate`: of the contact person    |
| contact_number  |  String  |    Required     | Contact number: country code + phone number, e.g. `+6591234567`    |
| occupation  |  String  |    Required when `client_type=retail`     | Occupation    |
| address_line1  |  String  |    Required     | Residential/registered address line 1, e.g. `123 Main Street`    |
| address_line2  |  String  |    Optional     | Address line 2, e.g. unit number    |
| state_province  |  String  |    Optional     | State / Province    |
| postal_code  |  String  |    Optional     | Postal code (if applicable)    |
| business_nature  |  String  |    Required when `client_type=corporate`     | Business nature    |
| principal_same_as_registered  |  Boolean  |    Optional     | `corporate` only. Principal place of business is same as registered address. Default `false`    |
| principal_address_line1  |  String  |    Required when `principal_same_as_registered=false`     | Principal place of business address line 1    |
| principal_address_line2  |  String  |    Optional     | Principal place of business address line 2    |
| principal_city  |  String  |    Required when `principal_same_as_registered=false`     | Principal place of business city    |
| principal_country  |  String  |    Required when `principal_same_as_registered=false`     | Principal place of business country, 3-letter country code    |
| principal_state_province  |  String  |    Optional     | Principal place of business state/province    |
| principal_postal_code  |  String  |    Optional     | Principal place of business postal code    |
| source_of_funds  |  Array[String]  |    Required     | Source of funds, multi-select. `retail`: `CRYPTOCURRENCY_STAKING` / `CRYPTOCURRENCY_MINING` / `EMPLOYMENT_INCOME` / `INHERITANCE` / `INVESTMENTS_OR_TRADING` / `SALE_OF_ASSETS` / `OTHERS`; `corporate`: `BUSINESS_ACTIVITY` / `BUSINESS_LOAN` / `CRYPTOCURRENCY_MINING` / `CRYPTOCURRENCY_STAKING` / `INVESTMENTS_OR_TRADING` / `SALE_OF_ASSETS` / `SHAREHOLDER_OR_INVESTOR_FUNDS` / `OTHERS`. A document proof (`SOURCE_OF_FUNDS_PROOF`) should be provided for each option declared    |
| source_of_funds_others  |  String  |    Required when `source_of_funds` contains `OTHERS`     | Source of funds - others description    |
| source_of_wealth_same_as_funds  |  Boolean  |    Optional     | Source of wealth is same as source of funds. Default `false`    |
| source_of_wealth  |  Array[String]  |    Required when `source_of_wealth_same_as_funds` is not `true`     | Source of wealth, multi-select (same options as `source_of_funds`). A document proof (`SOURCE_OF_WEALTH_PROOF`) should be provided for each option declared    |
| source_of_wealth_others  |  String  |    Required when `source_of_wealth` contains `OTHERS`     | Source of wealth - others description    |
| tax_declarations  |  Array[Object]  |    Required     | Tax declarations (jurisdictions of tax residence), at least one. Each item: `{"country_of_jurisdiction": "SGP"}` (3-letter country code)    |
| documents  |  Array[Object]  |    Required     | KYC documents, see the table below    |

- `documents` item:

|  Parameter  | Type  | Whether Required |                        Description                         |
| :---------: | :---: | :--------------: | :-------------------------------------------------------- |
| doc_type  |  String  |    Required     | `PROOF_OF_IDENTITY` / `PROOF_OF_RESIDENTIAL_ADDRESS` / `CERTIFICATE_OF_INCORPORATION` / `ANNUAL_RETURNS_FILING` / `CERTIFICATE_OF_INCUMBENCY` / `SOURCE_OF_FUNDS_PROOF` / `SOURCE_OF_WEALTH_PROOF` / `OTHER`    |
| file_name  |  String  |    Optional     | Original file name, e.g. `passport.jpg`    |
| file_base64  |  String  |    Required when `file_url` is blank     | File content as a base64 data string, e.g. `data:image/jpeg;base64,/9j/4AAQ...`. Formats: `jpg` / `jpeg` / `png` / `bmp`, max `4MB` per file. The server uploads it to S3 and stores only the generated file URL    |
| file_url  |  String  |    Required when `file_base64` is blank     | Already-uploaded file URL (used as-is)    |

Document requirements per client type:

|  client_type  |  Required documents  |
| :-----------: | :------------------- |
| `retail`  | `PROOF_OF_IDENTITY` + `PROOF_OF_RESIDENTIAL_ADDRESS`    |
| `corporate`  | `CERTIFICATE_OF_INCORPORATION` + (`ANNUAL_RETURNS_FILING` or `CERTIFICATE_OF_INCUMBENCY`)    |

- Request example (retail):

```json
{
    "currency": "USD",
    "mail": "customer@example.com",
    "client_type": "retail",
    "nationality": "SGP",
    "country": "SGP",
    "city": "Singapore",
    "date": "01011990",
    "id_type": "P",
    "id_number": "E1234567A",
    "first_name": "John",
    "middle_name": "Michael",
    "last_name": "Doe",
    "contact_number": "+6591234567",
    "occupation": "Software Engineer",
    "address_line1": "1 Raffles Place",
    "address_line2": "#20-61",
    "state_province": "Central Region",
    "postal_code": "048616",
    "source_of_funds": ["EMPLOYMENT_INCOME", "INVESTMENTS_OR_TRADING"],
    "source_of_wealth_same_as_funds": true,
    "tax_declarations": [
        {"country_of_jurisdiction": "SGP"}
    ],
    "documents": [
        {
            "doc_type": "PROOF_OF_IDENTITY",
            "file_name": "passport.jpg",
            "file_base64": "data:image/jpeg;base64,/9j/4AAQSkZJRg..."
        },
        {
            "doc_type": "PROOF_OF_RESIDENTIAL_ADDRESS",
            "file_name": "utility_bill.png",
            "file_base64": "data:image/png;base64,iVBORw0KGgo..."
        },
        {
            "doc_type": "SOURCE_OF_FUNDS_PROOF",
            "file_name": "payslip.jpg",
            "file_base64": "data:image/jpeg;base64,/9j/4AAQSkZJRg..."
        }
    ]
}
```

- Request example (corporate):

```json
{
    "currency": "USD",
    "mail": "finance@acme.com",
    "client_type": "corporate",
    "full_name": "ACME Trading Pte. Ltd.",
    "country": "SGP",
    "city": "Singapore",
    "date": "15032015",
    "id_number": "201506789K",
    "corporate_type": "NON_FI",
    "first_name": "Jane",
    "last_name": "Smith",
    "contact_number": "+6562345678",
    "business_nature": "International trade",
    "address_line1": "10 Anson Road",
    "address_line2": "#15-02",
    "state_province": "Central Region",
    "postal_code": "079903",
    "principal_same_as_registered": true,
    "source_of_funds": ["BUSINESS_ACTIVITY"],
    "source_of_wealth_same_as_funds": true,
    "tax_declarations": [
        {"country_of_jurisdiction": "SGP"}
    ],
    "documents": [
        {
            "doc_type": "CERTIFICATE_OF_INCORPORATION",
            "file_name": "coi.jpg",
            "file_base64": "data:image/jpeg;base64,/9j/4AAQSkZJRg..."
        },
        {
            "doc_type": "ANNUAL_RETURNS_FILING",
            "file_name": "nar1.jpg",
            "file_base64": "data:image/jpeg;base64,/9j/4AAQSkZJRg..."
        }
    ]
}
```

- Response:

```json
{
    "code": 0,
    "msg": "SUCCESS",
    "result": {
        "id": 12,
        "currency": "USD",
        "mail": "customer@example.com",
        "virtual_acct_no": null,
        "virtual_iban": null,
        "virtual_acct_status": null,
        "created_and_assigned": 0,
        "reason": null,
        "kyc_extra": {
            "first_name": "John",
            "middle_name": "Michael",
            "last_name": "Doe",
            "contact_number": "+6591234567",
            "occupation": "Software Engineer",
            "address_line1": "1 Raffles Place",
            "address_line2": "#20-61",
            "state_province": "Central Region",
            "postal_code": "048616",
            "business_nature": null,
            "principal_same_as_registered": false,
            "principal_address_line1": null,
            "principal_address_line2": null,
            "principal_city": null,
            "principal_country": null,
            "principal_state_province": null,
            "principal_postal_code": null,
            "source_of_funds": ["EMPLOYMENT_INCOME", "INVESTMENTS_OR_TRADING"],
            "source_of_funds_others": null,
            "source_of_wealth_same_as_funds": true,
            "source_of_wealth": null,
            "source_of_wealth_others": null,
            "tax_declarations": [
                {"country_of_jurisdiction": "SGP"}
            ],
            "documents": [
                {
                    "doc_type": "PROOF_OF_IDENTITY",
                    "file_name": "passport.jpg",
                    "file_base64": null,
                    "file_url": "https://xxx.s3.xxx.amazonaws.com/payone_sgbva_123_proof_of_identity_ab1.jpeg"
                }
            ]
        },
        "create_time": 1768464200000,
        "update_time": 1768464200000
    }
}
```

| Parameter |  Type  |          Description          |
| :--------: | :----: | :------------------------------ |
|   id   | long |        Local application record id              |
|   currency   | String |        Virtual account currency              |
|   mail   | String |        Contact email used for the application              |
|   virtual_acct_no   | String |        Virtual account number, assigned after approval (`null` before that)              |
|   virtual_iban   | String |        Virtual IBAN, assigned after approval (`null` before that)              |
|   virtual_acct_status   | String |        Virtual account status reported by the bank (available after approval)              |
|   created_and_assigned   | int |        Review status: `0`-PENDING_REVIEW, `1`-APPROVED (account created), `2`-REJECTED, `3`-CREATE_FAILED (can be reviewed again)              |
|   reason   | String |        Reject / failure reason              |
|   kyc_extra   | Object |        Extended KYC info submitted with the application (stored locally, not sent to the bank). `documents[].file_url` is the uploaded S3 file URL; `file_base64` is always `null` here. `null` for applications created before 2026-10-13              |
|   create_time / update_time   | long |        `UNIX` timestamp, unit: `milliseconds`              |

Note: only one pending/approved application is allowed per customer per currency. Applying again for the same currency while one is `0`-PENDING_REVIEW or `1`-APPROVED returns an error; re-apply is allowed after `2`-REJECTED or `3`-CREATE_FAILED.


### Query virtual account list

Query all virtual account applications/accounts of the authenticated customer, including the review status, the extended KYC info (`kyc_extra`, with document file URLs) and the assigned virtual account number after approval. Typically used to poll the review result after `apply`.

```text
url：/api/v1/mm/va/list
method：GET
```

- Request (query parameters, all optional):

|  Parameter  | Type  | Whether Required |                        Description                         |
| :---------: | :---: | :--------------: | :-------------------------------------------------------- |
| mail  |  String  |    Optional     | Filter by the contact email used for the application (exact match)    |
| currency  |  String  |    Optional     | Filter by virtual account currency, e.g. `USD` (case-insensitive)    |

- Response:

```json
{
    "code": 0,
    "msg": "SUCCESS",
    "result": [
        {
            "id": 12,
            "currency": "USD",
            "mail": "customer@example.com",
            "virtual_acct_no": "847201935762",
            "virtual_iban": "SG83BANK0000847201935762",
            "virtual_acct_status": "ACTIVE",
            "created_and_assigned": 1,
            "reason": null,
            "kyc_extra": {
                "first_name": "John",
                "middle_name": "Michael",
                "last_name": "Doe",
                "contact_number": "+6591234567",
                "occupation": "Software Engineer",
                "address_line1": "1 Raffles Place",
                "address_line2": "#20-61",
                "state_province": "Central Region",
                "postal_code": "048616",
                "business_nature": null,
                "principal_same_as_registered": false,
                "principal_address_line1": null,
                "principal_address_line2": null,
                "principal_city": null,
                "principal_country": null,
                "principal_state_province": null,
                "principal_postal_code": null,
                "source_of_funds": ["EMPLOYMENT_INCOME", "INVESTMENTS_OR_TRADING"],
                "source_of_funds_others": null,
                "source_of_wealth_same_as_funds": true,
                "source_of_wealth": null,
                "source_of_wealth_others": null,
                "tax_declarations": [
                    {"country_of_jurisdiction": "SGP"}
                ],
                "documents": [
                    {
                        "doc_type": "PROOF_OF_IDENTITY",
                        "file_name": "passport.jpg",
                        "file_base64": null,
                        "file_url": "https://xxx.s3.xxx.amazonaws.com/payone_sgbva_123_proof_of_identity_ab1.jpeg"
                    }
                ]
            },
            "create_time": 1768464200000,
            "update_time": 1768550600000
        },
        {
            "id": 10,
            "currency": "EUR",
            "mail": "ops@example.com",
            "virtual_acct_no": null,
            "virtual_iban": null,
            "virtual_acct_status": null,
            "created_and_assigned": 2,
            "reason": "KYC address incomplete",
            "kyc_extra": null,
            "create_time": 1768377800000,
            "update_time": 1768464200000
        }
    ]
}
```

| Parameter |  Type  |          Description          |
| :--------: | :----: | :------------------------------ |
|   id   | long |        Local application record id              |
|   currency   | String |        Virtual account currency              |
|   mail   | String |        Contact email used for the application              |
|   virtual_acct_no   | String |        Virtual account number, assigned after approval (`null` before that)              |
|   virtual_iban   | String |        Virtual IBAN, assigned after approval (`null` before that)              |
|   virtual_acct_status   | String |        Virtual account status reported by the bank (available after approval)              |
|   created_and_assigned   | int |        Review status: `0`-PENDING_REVIEW, `1`-APPROVED (account created), `2`-REJECTED, `3`-CREATE_FAILED (can be reviewed again)              |
|   reason   | String |        Reject / failure reason              |
|   kyc_extra   | Object |        Extended KYC info submitted with the application (same structure as the apply response). `null` for applications created before 2026-10-13              |
|   create_time / update_time   | long |        `UNIX` timestamp, unit: `milliseconds`              |




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

> Sell / buy orders are pushed as exchange events when the platform review completes (status becomes `SUCCESS` or `FAILED`).


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
|   111049    | Coin type not supported                                     |
|   111074    | Amount is not within the advertiser limit                   |
|   111082    | Wallet address error (address not in approved whitelist)    |
|   111093    | Invalid parameters                                          |
|   101040    | Account not found                                           |
|   101051    | The tx_hash already exists (deposit already declared)       |
|   101052    | Insufficient wallet balance                                 |
|   101055    | Address status abnormal (not approved yet)                  |
|   101056    | Multiple approved receivers found, please specify receive_id |
|   101057    | Multiple approved whitelist addresses found, please specify address |
|   112016    | Receiver currency does not match the order currency         |
|   15009     | Transaction hash does not match (wrong `to` address etc.)   |
|   119012    | Rate error                                                  |
|   130002    | Data not found (e.g. receiver / asset not configured)       |
|   130015    | Status abnormal                                             |
|   130018    | Insufficient balance                                        |

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
