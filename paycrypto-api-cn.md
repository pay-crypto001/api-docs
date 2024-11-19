# Paycrypto API 文档

- [Paycrypto API](#Paycrypto-API)
- [接口规范](#接口规范)
- [1.机构](#机构)
     - [1.1 支持的卡种类查询](#支持的卡种类查询)
     - [1.2 查询机构余额](#查询机构余额)
     - [1.3 公钥查询](#公钥查询)
     - [1.4 上传公钥](#上传公钥)
     - [1.5 卡费率查询](#卡费率查询)
     - [1.6 估算将到账的法币金额](#估算将到账的法币金额)
     - [1.7 估算需要充值多少数字货币](#估算需要充值多少数字货币)
     - [1.8 法币账户查询](#法币账户查询)
- [2.KYC](#KYC)
     - [2.1 提交用户 KYC 数据](#提交用户-KYC-数据)
     - [2.2 提交用户KYC附件(可选)](#提交用户KYC附件-可选)
     - [2.3 查询所有用户 KYC 记录](#查询所有用户-KYC-记录)
     - [2.4 查询指定用户 KYC 记录](#查询指定用户-KYC-记录)
     - [2.5 更新用户 KYC_info 信息（可选）](#更新用户-KYC-信息)
     - [2.6 更新用户账单地址信息](#更新用户地址信息)
     - [2.7 查询指定用户 KYC 信息](#查询指定用户-KYC-信息)
- [3.开卡和激活](#开卡和激活)
     - [3.1 提交用户开卡申请](#提交用户开卡申请)
     - [3.2 提交激活卡需要的附件](#提交激活卡需要的附件)
     - [3.3 用户激活卡片](#用户激活卡片)
     - [3.4 查询所有卡片状态](#查询所有卡片状态)
     - [3.5 查询指定用户所有卡片状态](#查询指定用户所有卡片状态)
     - [3.6 工单申请冻结、解冻、挂失、重置密码、补卡、重发PIN](#申请冻结-解冻-挂失-重置密码-补卡-重发PIN)
     - [3.7 工单查询冻结、解冻、挂失、重置密码、补卡、重发PIN的状态](#查询冻结-解冻-挂失-重置密码-补卡-重发PIN的状态)
     - [3.8 查询快递单号](#查询快递单号)
     - [3.9 升级双币种卡](#升级双币种卡)
     - [3.10 再次发送包含PIN码的邮件](#再次发送包含PIN码的邮件)
- [4.充值卡](#充值卡)
     - [4.1 给用户卡充值](#给用户卡充值)
     - [4.2 固定到账法币金额](#用稳定币给用户卡充值-指定到账法币金额-)
     - [4.3 查询币对价格](#查询币对价格)
     - [4.4 查询某笔卡充值交易状态](#查询某笔卡充值交易状态)
     - [4.5 查询所有卡充值记录](#查询所有卡充值记录)
     - [4.6 查询指定用户所有卡充值记录](#查询指定用户所有卡充值记录)
     - [4.7 兑换USDT](#兑换USDT)  
     - [4.8 查用户兑换记录](#查用户兑换记录)
- [5.信用卡](#信用卡)
     - [5.1 授信](#信用卡授信)
     - [5.2 查询授信记录](#查询授信记录)
     - [5.3 用户授信额度查询](#用户授信额度查询)
     - [5.4 还款（不支持）](#还款)
     - [5.5 还款记录查询（不支持）](#还款记录查询)
     - [5.6 机构授信总额度查询](#机构授信总额度查询)
     - [5.7 冻结](#冻结)
     - [5.8 解冻](#解冻)
     - [5.9 给卡种共享账户充值](#给卡种共享账户充值)
     - [5.10 查询卡种共享账户余额](#查询卡种共享账户余额)
- [6.共享卡](#共享卡)
     - [6.1 修改限额](#修改限额)
     - [6.2 查询当前限额](#查询当前限额)
     - [6.3 查询限额修改记录](#查询限额修改记录)
     - [6.4 冻结](#冻结)
     - [6.5 解冻](#解冻)
     - [6.6 给卡种共享账户充值](#给卡种共享账户充值)
     - [6.7 查询卡种共享账户余额](#查询卡种共享账户余额)
- [7.礼品卡](#礼品卡)
     - [7.1 购买礼品卡](#购买礼品卡)
     - [7.2 查询礼品卡](#查询礼品卡)
- [8.银行卡查询](#银行卡查询)
     - [8.1 查询卡是否激活](#查询卡是否激活)
     - [8.2 查询卡余额](#查询卡余额)
     - [8.3 查询卡账单](#查询卡账单)
     - [8.4 查询卡敏感信息](#查询卡敏感信息)
     - [8.5 用户请求重置密码（暂不支持）](#用户请求重置密码-暂不支持-)
     - [8.6 查询卡已授权交易](#查询卡已授权交易)
     - [8.7 信用卡月限额修改](#信用卡月限额修改)
     - [8.8 查询信用卡月限额](#查询信用卡月限额)
     - [8.9 卡冻结](#卡冻结)
     - [8.10 卡解冻](#卡解冻)

- [9.验证码](#验证码-不支持)
     - [9.1 发送邮箱验证码](#发送邮箱验证码)
     - [9.2 校验邮箱验证码](#校验邮箱验证码)
     - [9.3 发送手机验证码 (暂不支持)](#发送手机验证码-暂不支持-)
     - [9.4 校验手机验证码(暂不支持)](#校验手机验证码-暂不支持-)
- [10.事件推送](#事件推送)    
     - [10.1 推送 KYC 事件](#推送-KYC-事件)
     - [10.2 推送开卡事件](#推送开卡事件)
     - [10.3 推送卡激活事件](#推送卡激活事件)
     - [10.4 推送卡充值事件](#推送卡充值事件)
     - [10.5 推送信用卡消费事件](#推送信用卡消费事件)
     - [10.6 推送充值卡消费事件](#推送充值卡消费事件)
     - [10.7 推送3ds OTP验证码](#推送3ds-OTP验证码)
     - [10.8 测试推送事件](#测试推送事件)
     - [10.9 查询推送失败的事件](#查询推送失败的事件)
     - [10.10 更新推送失败的事件](#更新推送失败的事件)
     - [10.11 推送冻结、解冻、挂失、重置密码、补卡状态](#推送冻结-解冻-挂失-重置密码-补卡状态)
- [11.错误码](#错误码)
     - [11.1 业务逻辑错误码](#业务逻辑错误码)
     - [11.2 身份权限认证错误码](#身份权限认证错误码)
     - [11.3 异常错误码](#异常错误码)
     - [11.4 KYC失败错误码](#KYC失败错误码)

## Paycrypto API

欢迎使用 Paycrypto API 文档。本文档是针对 Paycrypto ToB 的卡业务，目前支持实体卡、虚拟卡、共享卡，共几十个卡种类（不同卡BIN），不同卡种支持的法币和手续费不同，接口调用参数也略有差别。例如：


| 卡名 |  法币  |           区别            |
| :--------: | :----: | :------------------------------ |
|    J卡（card_type_id：30000001 - 30000009）     | USD | KYC审核48h以内，KYC填写只支持英文，mobile格式如 +8615821702552，卡片上会印用户的姓名，制卡时间1-2周。激活时必须提交手持卡片和护照的自拍照。到账时间4h内。 |
|   P卡（card_type_id：40000001 - 40000009）   | EUR       |         KYC时间1h以内，KYC填写只支持英文， mobile格式如 +86-15821702552，country、nationality填两位国家码。到账时间1h内。 |


API 使用步骤：

1. 请在 [https://customer.paycrypto.com/](https://customer.paycrypto.com/) 注册机构账户，如果访问不了请提供你的IP地址。
2. 我们审核通过后，机构才可以登录成功。
3. 机构登录，查看钱包地址，给钱包充值，支持 USDT、BTC、ETH 等。
4. 机构登录，创建 Appkey 和 secret，可选择配置 webhook 回调地址。
5. 调用 API 进行 KYC、开卡、激活卡、充值等操作，状态变更 Paycrypto 会通过回调地址通知机构服务器。

> 建议使用生产环境前先在测试环境调试。

机构 Dashboard :

* 测试环境（有IP白名单限制）: https://customer.sandbox.paycrypto.com/
* 生产环境（有IP白名单限制）: https://customer.paycrypto.com/

API 域名：

* 测试环境: https://api.sandbox.paycrypto.com/
* 生产环境: https://api.paycrypto.com/

## 接口规范

- Open API 请求都使用 `HMAC` 认证，以保证请求的完整性，真实性，同时做身份认证和鉴权。

- **分页**。查询记录列表都有分页，分页参数：`page_num` 表示页数，`page_size` 表示每页大小。接口 `DTO` 统一返回 `total`，`records`。

- **国家**。两位国家代码，参照 `ISO 3166-1 alpha-2` 编码标准

- 时间处理。API 请求和返回时间都是 `UNIX` 时间戳，**毫秒为单位**，避免因为时区导致误差

- 金额处理。API 请求和返回都是 `String` 字符串类型，避免精度丢失

- 所有带 `body` 的请求没有特殊说明body都是 `JSON`格式，`Content-Type：application/json`

- 所有查询接口查询时间间隔必须**小于一个月**

- 接口返回格式统一：

  | Parameter |  Type  |           Description            |
  | :--------: | :----: | :------------------------------ |
  |    code    |  int   |  错误码。`0`：正常，非`0`：异常  |
  |    msg     | String | 成功为 `SUCCESS`，失败为错误描述 |
  |   result   | Object |             返回信息             |

### HMAC认证

首先机构需要申请 API `Key` 和 API `Secret`，访问 API 时会用到。

| 名词 | 解释 |
| --- | --- |
| User ID | User ID 是用来标记你的开发者账号的， 是你的用户 ID|
| API Key & API Secret| User ID 下面管理了多个 API Key + API Secret， API Key 对应你的应用，你可以有多个应用，每个应用可以申请不同的  API 权限|

#### 客户端实现流程：

1. 构造需要签名的 data ，包括
   - UNIX 时间戳，`毫秒为单位`：`request` time stamp
   - 请求方法：`HTTP` method
   - 请求 API Key： Api Key
   - 完整的请求路径，包括 `URL` 问号后的参数：request URI
   - 如果有请求 `body`，再加上请求 `body` 转换后的`字符串`：string representation of the request payload
2. 客户端根据签名 data 和 API Secret 使用 `HMAC_SHA256` 算法生成签名 signature。
3. 按照指定顺序设置 Authorization header，即 key 是：`Authorization`， value 是：Railone:ApiKey:request time stamp:signature（以冒号拼接）。
4. 如果在服务端创建 API Key，API Secret 时使用了密码，则需要设置 Access-Passphrase header，即 `key` 是：`Access-Passphrase`，`value` 是：当时设置的密码。
5. 客户端发送数据和 Authorization header，以及 Access-Passphrase header（如果有第四步的话）到服务端。即最终发送的 http header 为：
   - Authorization:Railone:ApiKey:request timestamp:signature
   - Access-Passphrase:Your API Secret passphrase


#### 如何构造待签名的请求body string：

请求 body 需要按照 `ASCII` 码的顺序对参数名进行排序，以  `=` 拼接 key 和 value，并以 `&` 分割多个 key-value，转换成字符串。

例如请求 `body` 为：

```
{
	"from_address":"Ae9ujqUnAtH9yRiepRvLUE3t9R2NbCTZPG",
	"amount":190,
	"to_address":"AUol16ghiT9AtxRDtNeq3ovhWJ5iaY6iyd"
}
```

转换后为：

```text
amount=190&from_address=Ae9ujqUnAtH9yRiepRvLUE3t9R2NbCTZPG&to_address=AUol16ghiT9AtxRDtNeq3ovhWJ5iaY6iyd
```


#### 实现代码示例：

请参考：[https://github.com/paycrypto-com/paycrypto-sdk-java](https://github.com/paycrypto-com/paycrypto-sdk-java)


## 机构


### 支持的卡种类查询

```text
url：/api/v1/institution/card/type
method：GET
```

- 响应:

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
| card_type_id |String |银行卡种类对应的id,比如 50000001|
| currency_type |String |卡支持的法币类型|
|   bank_id   | String |        银行ID           |
|   description   | String |    卡种描述           |
|   card_network   | String |    发卡机构：visa、master、unionpay           |
|   virtual_card   | Bool |    是否是虚拟卡           |
|   card_title   | String |    支持F卡、J卡、P卡等，对应的金属卡是F-M*卡、J-M*卡、P-M*卡，对应的虚拟卡是F-V卡、J-V卡、P-V卡           |

### 查询机构余额

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
| coin_type    | String  | 数字货币类型   |
| addresses[0].balance    | String  |  数字货币余额         |
| addresses[0].address_type | String  | 数字货币地址类型，开卡（open_card）和充值（deposit） |
| addresses[0].address    | String  | 数字货币地址         |


### 公钥查询


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
|   publickey   | String |           机构公钥           |


### 上传公钥

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


### 卡费率查询

```text
url：/api/v1/institution/rates?card_type_id={card_type_id}
method：GET
```

- 请求：

| Parameter |  Type  |   Requirement  | Description   |
| :------------: | :----: | :----------: |:---------- |
| card_type_id |String |必填 |银行卡种类对应的id,比如 50000001|

- 响应：

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
        "bank_atm_fee": "0",
        "bank_atm_rate": "0.1",
        "bank_transaction_rate": "0.2"
    }
}
```

| Parameter |  Type  |          Description          |
| :--------: | :----: | :------------------------------ |
|   card_application_fee   | String |           开卡的手续费（USDT）           |
|   min_deposit   | String |           单笔最小充值金额（法币）           |
|   max_deposit   | String |           单笔最大充值金额（法币）           |
|   exchange_rate   | String |           USDT兑换相应法币的汇率           |
|   loading_rate   | String |           给用户充值时付给 Paycrypto 的阶梯费率，大部分卡只有一阶费率           |
|   bank_transaction_rate   | String |          银行卡刷卡消费的手续费率           |
|   bank_atm_rate   | String |          ATM取款时的手续费率           |
|   bank_atm_fee   | String |          ATM取款时的固定手续费           |


### 估算将到账的法币金额

- Request:

```text
内扣 url：/api/v1/institution/estimation/currency
外扣 url：/api/v1/institution/estimation/currency/external-deduction
method：POST
```

|  Parameter  | Type  | Whether Required |                        Description                         |
| :---------: | :---: | :--------------: | :-------------------------------------------------------- |
|  coin_amount  |  String  |    Required     |  充值的数字货币金额     |
| coin_type  |  String  |    Required     |  充值的数字货币类型，USDT     |
|  card_type_id  |  String  |    Required     |  卡种ID    |

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

|    Parameter    |  Type   |      Description           |
| :---------: | :----:   | :--------------------------- |
|  coin_amount  |  String      |  充值的数字货币金额     |
| coin_type  |  String      |  充值的数字货币类型     |
|  currency_amount    | String  |    充值到账的法币金额          |
|  currency_type  |  String    |  充值到账的法币类型    |
| exchange_fee    | String  |   其他币种兑换USDT的手续费，单位是 coin_type            |
| exchange_fee_rate    | String  |   其他币种兑换USDT的手续费率          |
|  loading_fee    | String  |       充值手续费，单位是 coin_type        |
| exchange_rate    | String  | USDT/USD 汇率              |
| fiat_exchange_rate    | String  | 卡支持的法币/USD 汇率              |
| coin_price    | String  | coin_type/USDT 价格              |

> (currency_amount * fiat_exchange_rate) = ((coin_amount - exchange_fee - loading_fee) * coin_price) * exchange_rate 
> 总到账USD = 实际充值USDT * exchange_rate（USDT/USD）

### 估算需要充值多少数字货币

- Request:

```text
url：/api/v1/institution/estimation/crypto
method：POST
```
- Request:

|  Parameter  | Type  | Whether Required |                        Description                         |
| :---------: | :---: | :--------------: | :--------------------------------------------------------|
|  currency_amount  |  String  |    Required     |  需要充值到账的法币金额     |
|  card_type_id  |  String  |    Required     |  卡种ID    |
|  coin_type  |  String  |    Required     |  需要换算的数字货币类型，usdt    |

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

|    Parameter    |  Type   |      Description          |
| :---------: | :----:   | :--------------------------- |
|  coin_amount    | String  |    需要的数字货币金额         |
|  coin_type  |  String    |  需要的数字货币类型    |
|  currency_amount    | String  |    充值到账的法币金额          |
|  currency_type  |  String    |  充值到账的法币类型    |
| exchange_fee    | String  |   其他币种兑换USDT的手续费，单位是 coin_type            |
| exchange_fee_rate    | String  |   其他币种兑换USDT的手续费率           |
|  loading_fee    | String  |       充值手续费，单位是 coin_type       |
| exchange_rate    | String  | USDT/USD 汇率             |
| fiat_exchange_rate    | String  | 卡支持的法币/USD 汇率              |
| coin_price    | String  | coin_type/USDT 价格              |

> (currency_amount * fiat_exchange_rate) = ((coin_amount - exchange_fee - loading_fee) * coin_price) * exchange_rate 
> 总到账USD = 实际充值USDT * exchange_rate（USDT/USD）


## 法币账户查询

请联系我们商务

## KYC

提供给机构使用的接口，如机构为用户创建KYC、查询KYC记录等。

### 提交用户 KYC 数据

如果KYC审核失败，也使用这个接口重新提交。

```text
url：/api/v1/customers/accounts
method：POST
```


- 请求：

| Parameter |  Type  |    Requirement  |Description   |
| :------------: | :----: | :----------: |:---------- |
|   acct_no | String | 必填 |机构端用户编号(机构端唯一)，字符长度最大64|
|   acct_name | String |必填 |机构端用户名，字符长度最大64|
|   first_name | String |必填 |真实用户名，字符长度最大50|
|   last_name | String |必填 |真实用户姓，字符长度最大50|
|   gender | String |必填 |male:男，female:女，unknown:其他，字符长度最大6|
|   birthday | String |必填 |生日（生日格式为"1990-01-01"）|
|   city | String |必填 |城市，字符长度最大100|
|   state | String |必填 |省份，字符长度最大100|
|   country | String |必填 |用户所在国家，字符长度最大50|
|   nationality | String |必填 |国籍，字符长度最大255|
| doc_no | String |必填 |证件号码，字符长度最大128|
| doc_type | String |必填 |证件类型(目前只支持passport): passport: 护照，idcard：身份证，字符长度最大8|
| front_doc | String |必填 |正面照。base64编码, 照片文件大小应小于2M|
| back_doc | String |选填 |反面照，doc_type是idcard时必须填写。base64编码，照片文件大小应小于2M|
| mix_doc | String |必填 |手持证件照。base64编码，照片文件大小应小于2M|
|   country_code | String |必填 |手机国际区号，如“+86”。字符长度最大5|
|   mobile | String |必填 |手机号，字符长度最大32|
|  mail | String |必填 |邮箱，不支持163.com的邮箱。字符长度最大64|
|   address | String |必填 |通讯地址，银行卡会寄到该地址。字符长度最大256|
|   zipcode | String |必填 |邮编，字符长度最大20|
|   maiden_name | String |必填 |妈妈的名字，字符长度最大255|
| card_type_id |String |必填 |银行卡种类对应的id,比如 10010001|
|   kyc_info | String |选填 |KYC 其他信息|
| mail_verification_code | String |选填 |邮箱验证码|
| mail_token | String |选填|发送邮件后返回的token|
| cust_tx_id | String | 选填| KYC流水号|
| sign_img | String | 选填| 签名照片。base64编码，照片文件大小应小于1M|
| poa_doc | String[3] |选填 |地址证明照片（暂不支持）。base64编码，照片或PDF文件每个文件大小应小于2M|
| card_number | String | 选填| 绑卡卡号，用于提前售卡，只支持J卡|

- 响应：

```json
{
  "code": 0,
  "msg": "string",
  "result": true
}
```

> 如何获取 mail_token and mail_verification_code? 请看 "6.1.发送邮箱验证码". mail_verification_code 由用户自己填写.


### 提交用户KYC附件（可选）


```text
url：/api/v1/customers/attachments
method：POST
```
> 调用此接口前需先成功调用 [提交用户 KYC 数据]接口,此接口只支持实体卡调用.

- 请求：

| Parameter |  Type  |    Requirement  |Description   |
| :------------: | :----: | :----------: |:---------- |
| acct_no | String | 必填 |机构端用户编号(机构端唯一)，字符长度最大64|
| card_type_id |String |必填 |银行卡种类对应的id,比如 10010001|
| docs | String[3] |必填| 用户上传附件，如AML文件等，base64编码，照片或PDF文件每个文件大小应小于2M,最多只能上传3个文件 |
- 响应：

```json
{
  "code": 0,
  "msg": "Success",
  "result": true
}
```

### 查询所有用户 KYC 记录

```text
url：/api/v1/customers/accounts
method：GET
```

| Parameter  | Type |Requirement  | Description |
| :------------: | :----: | :----------: |:---------- |
|  page_num   | int  |    选填|页数     |
|  page_size  | int  |  选填|页的大小   |
| former_time | long |  选填|前置时间, UNIX 时间戳，`秒为单位`   |
| latter_time | long | 选填|后置时间, UNIX 时间戳，`秒为单位`   |
| time_sort | String | 选填|时间排序, asc为升序，desc为降序   |



- 响应：

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
        "status": 3,
        "face_recognition_url": "https://www.hh.io", //认证中才可能有活体认证链接
        "reason": "{\"code\":1110032,\"msg\":\"KYC failure, photo error\"}",
        "create_time": 1546300800000
      }
    ]
  }
}
```


| Parameter  |  Type  |                     Description                     |
| :--------: | :----: | :------------------------------ |
|   acct_no   | String |             机构端用户编号(机构端唯一)              |
| card_type_id |String |卡种对应的id|
|   status    |  int   | 状态码: 0 已提交， 1 认证通过(开卡成功)， 2 认证未通过， 3 认证中， 4 提交信息处理中 6 已退款|
|   reason   | String | 认证失败原因。其他情况为空字符串 |
| create_time |  long  |                      创建时间                       |

> 失败原因请查看KYC失败错误码

### 查询指定用户 KYC 记录

```text
url：/api/v1/customers/accounts
method：GET
```

- 请求：

| Parameter  | Type |Requirement  | Description |
| :------------: | :----: | :----------: |:---------- |
|    acct_no     | String | 必填|机构用户的唯一id号 |
|  page_num   | int  |    选填|页数     |
|  page_size  | int  |  选填|页的大小   |
| former_time | long |  选填|前置时间, UNIX 时间戳，`秒为单位`   |
| latter_time | long | 选填|后置时间, UNIX 时间戳，`秒为单位`   |
| time_sort | String | 选填|时间排序, asc为升序，desc为降序   |

- 响应：

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
        "status": 3,
        "face_recognition_url": "https://www.hh.io", //认证中才可能有活体认证链接
        "reason": "{\"code\":1110032,\"msg\":\"KYC failure, photo error\"}",
        "create_time": 1546300800000
      }
    ]
  }
}
```


| Parameter  |  Type  |                     Description                     |
| :--------: | :----: | :------------------------------ |
|   acct_no   | String |             机构端用户编号(机构端唯一)              |
| card_type_id |String  |卡种对应的id|
|   status    |  int   | 状态码: 0 已提交， 1 认证通过(开卡成功)， 2 认证未通过， 3 认证中， 4 提交信息处理中  6 已退款 |
|   reason   | String | 认证失败原因。其他情况为空字符串 |
| create_time |  long  |                      创建时间                       |


> 失败原因请查看KYC失败错误码


### 更新用户 KYC_info 信息（可选）

```text
url：/api/v1/customers/accounts
method：PUT
```

- 请求：

| Parameter  | Type |Requirement  | Description |
| :------------: | :----: | :----------: |:---------- |
|    acct_no     | String | 必填|机构用户的唯一id号 |
|  kyc_info   | String  |    必填|kyc_info     |

- 响应：

```json
{
  "code": 0,
  "msg": "SUCCESS",
  "result": true
}
```


### 更新用户账单地址信息

```text
url：/api/v1/customers/accounts/address
method：PUT
```

- 请求：

| Parameter  | Type |Requirement  | Description |
| :------------: | :----: | :----------: |:---------- |
|    acct_no     | String | 必填|机构用户的唯一id号 |
|   mobile | String |必填 |手机号，字符长度最大32|
| country_code|	String|	必填|	手机国际区号，如“+86”。字符长度最大5|
|   country | String |必填 |用户所在国家两位国家码，字符长度最大50|
|   state | String |必填 |省份，如果是美国填两位州码，字符长度最大100|
|   city | String |必填 |城市，字符长度最大100|
|   address | String |必填 |通讯地址，银行卡会寄到该地址。字符长度最大256|
|   zipcode | String |必填 |邮编，字符长度最大20|

- 响应：

```json
{
  "code": 0,
  "msg": "SUCCESS",
  "result": true
}
```

### 查询指定用户 KYC 信息

```text
url：/api/v1/customers/accounts/kyc
method：GET
```

- 请求：

| Parameter  | Type |Requirement  | Description |
| :------------: | :----: | :----------: |:---------- |
|    acct_no     | String | 必填|机构用户的唯一id号 |

- 响应：

```
{
  "code": 0,
  "msg": "SUCCESS",
  "result": {
    "total": 1,
    "records": [
      {
        "acct_no": "1222",
        ......
      }
    ]
  }
}
```

| Parameter |  Type    |Description   |
| :------------: | :----:  |:---------- |
|   acct_no | String | 机构端用户编号(机构端唯一)，字符长度最大64位|
|   acct_name | String  |机构端用户名，字符长度最大64位|
|   first_name | String  |真实用户名，字符长度最大50位|
|   last_name | String  |真实用户姓，字符长度最大50位|
|   gender | String  |male:男，female:女，unknown:其他，字符长度最大6位|
|   birthday | String  |生日（生日格式为"1990-01-01"）|
|   city | String  |城市，字符长度最大100位|
|   state | String  |省份，字符长度最大100位|
|   country | String  |用户所在国家，字符长度最大50位|
|   nationality | String  |国籍，字符长度最大255位|
| doc_no | String  |证件号码，字符长度最大128位|
| doc_type | String  |证件类型(目前只支持passport): passport: 护照，idcard：身份证，字符长度最大8位|
|   country_code | String |手机国际区号，如“+86”。字符长度最大5位|
|   mobile | String  |手机号，字符长度最大32位|
|  mail | String  |邮箱，不支持163.com的邮箱。字符长度最大64位|
|   address | String  |通讯地址，银行卡会寄到该地址。字符长度最大256位|
|   zipcode | String  |邮编，字符长度最大20位|
|   maiden_name | String  |妈妈的名字，字符长度最大255位|
| card_type_id |String  |银行卡种类对应的id,比如 10010001|
|   kyc_info | text  |KYC 其他信息|
| cust_tx_id | String | KYC流水号|
|   status    |  int   | 状态码: 0 已提交， 1 认证通过(开卡成功)， 2 认证未通过， 3 认证中， 4 提交信息处理中 6 已退款|

## 开卡和激活

提供机构给用户开卡，记录查询等接口。

### 提交用户开卡申请

```text
url：/api/v1/debit-cards
method：POST
```

- 请求

| Parameter |  Type  |   Requirement  |Description   |
| :------------: | :----: | :----------: |:---------- |
| acct_no | String | 必填|机构端用户编号(机构端唯一)|
| card_type_id |String |必填 |卡种对应的id|
| card_number |String |可选 |支持绑卡的卡种才需要填写。用户自己填写卡号绑卡|
| urn |String |可选 |支持绑卡的卡种才需要填写。信封号|


- 响应：

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

| Parameter |  Type  |          Description          |
| :--------: | :----: | :------------------------------ |
|   card_no   | String |           分配的银行卡ID，查询时用card_no，避免真是卡号信息泄露。生成规则：机构id+5位随机数 +卡种id +卡最后四位           |
|   card_number   | String |           分配的真实银行卡号, 只显示前6位和后4位           |
|   status   | int |   状态：2. 开卡申请成功(未激活)， 5. 申请失败(卡片正在制作中，请过会再申请)           |


### 提交激活卡需要的附件

只有激活时需要提交附件的卡类型，才需要调用这个接口，否则不需要调用。

- Request:

```text
url：/api/v1/debit-cards/attachment
method：POST
```

|  Parameter  | Type  | Whether Required |                        Description                         |
| :---------: | :---: | :--------------: | :-------------------------------------------------------- |
|  card_no  |  String  |    必填     | 卡号    |
|  poa_doc  |  String[3]  |    选填     |   地址证明照（暂不支持）。base64编码，照片或PDF文件每个文件大小应小于2M。如果已在KYC时提交，无需再提交。  |
|  active_doc  |  String  |    选填     | 手持护照和银行卡照。base64编码，照片文件大小应小于2M  |

- Response:

```
{
    "code": 0,
    "msg": "SUCCESS",
    "result": true
}
```

### 用户激活卡片

虚拟卡激活成功后，通过银行接口查卡信息可以拿到真实卡号、cvv、过期时间。

```text
url：/api/v1/debit-cards/status
method：PUT
```

- 请求

| Parameter |  Type  |   Requirement  |     Description         |
| :------------: | :----: | :----------: |:---------- |
|    card_no     | String |      必填    |银行卡ID          |
| acct_no | String | 必填    |机构端用户编号(机构端唯一) |



- 响应：

```json
{
  "code": 0,
  "msg": "string",
  "result": {
    "card_type_id":"72000001",
    "card_no":"4360396720000016920",
    "acct_no":"test20240529",
    "status":1
  }
}
```

| Parameter  |  Type  |             Description             |
| :--------: | :----: | :------------------------------ |
|   acct_no   | String |     机构端用户编号(机构端唯一)      |
|   card_type_id   | String |     卡类型ID     |
|   card_no   |  int   |              卡ID               |
|   status    |  int   | 状态码: 0 冻结， 1 激活成功， 2未激活， 3. 激活待审核， 4. 激活审核失败,  5. 申请失败(卡片正在制作中，请过会再申请) 6.已注销 |


### 查询所有卡片状态

```text
url：/api/v1/debit-cards
method：GET
```

| Parameter  | Type | Requirement  |Description |
| :------------: | :----: | :----------: |:---------- |
|  page_num   | int  |    选填    |页数     |
|  page_size  | int  |  选填    |页的大小   |
| former_time | long |  选填    |前置时间, UNIX 时间戳，`秒为单位`   |
| latter_time | long |  选填    |后置时间, UNIX 时间戳，`秒为单位`   |
| time_sort | String | 选填|时间排序, asc为正序，desc为反序   |

- 响应：

```json
{
    "code": 0,
    "msg": "SUCCESS",
    "result": {
        "total": 1,
        "records": [
            {
                "acct_no": "1",
                "card_type_id":"72000001",
                "card_no": "4385211202597301",
                "status": 4,
                "reason": "{\"code\":1110061,\"msg\":\"Activation failure, hand holding card is not plastic card\"}",
                "create_time": 1576847136000
            }
        ]
    }
}
```


| Parameter  |  Type  |             Description             |
| :--------: | :----: | :------------------------------ |
|   acct_no   | String |     机构端用户编号(机构端唯一)      |
|   card_type_id   | String |     卡类型ID     |
|   card_no   |  int   |              银行卡号               |
|   status    |  int   | 状态码: 0 冻结， 1 激活成功， 2未激活， 3. 激活待审核， 4. 激活审核失败,  5. 申请失败(卡片正在制作中，请过会再申请) 6.已注销 |
|   reason   | String | 激活失败原因。其他情况为空字符串 |
| create_time |  long  |              创建时间               |



### 查询指定用户所有卡片状态

```text
url：/api/v1/debit-cards?acct_no={acct_no}
method：GET
```

- 请求：

| Parameter  | Type | Requirement  |Description |
| :------------: | :----: | :----------: |:---------- |
|   acct_no   | String |    必填 |机构端用户编号(机构端唯一)      |
|  page_num   | int  |    选填    |页数     |
|  page_size  | int  |  选填    |页的大小   |
| former_time | long |  选填    |前置时间, UNIX 时间戳，`秒为单位`   |
| latter_time | long |  选填    |后置时间, UNIX 时间戳，`秒为单位`   |
| time_sort | String | 选填|时间排序, asc为正序，desc为反序   |


- 响应：

```json
{
    "code": 0,
    "msg": "SUCCESS",
    "result": {
        "total": 1,
        "records": [
            {
                "acct_no": "1",
                "card_type_id":"72000001",
                "card_no": "4385211202597301",
                "status": 4,
                "reason": "{\"code\":1110061,\"msg\":\"Activation failure, hand holding card is not plastic card\"}",
                "create_time": 1576847136000
            }
        ]
    }
}
```

| Parameter  |  Type  |             Description             |
| :--------: | :----: | :------------------------------ |
|   acct_no   | String |     机构端用户编号(机构端唯一)      |
|   card_type_id   | String |     卡类型ID     |
|   card_no   |  int   |              银行卡ID               |
|   status    |  int   | 状态码: 0 冻结， 1 激活成功， 2未激活， 3. 激活待审核， 4. 激活审核失败, 5. 申请失败(卡片正在制作中，请过会再申请)|
|   reason   | String | 激活失败原因。其他情况为空字符串 |
| create_time |  long  |              创建时间               |


### 工单申请冻结、解冻、挂失、重置密码、补卡、重发PIN

这个接口提交的工单需要我们人工处理。有些卡支持API冻结、解冻，就不需要对接这个接口。

```text
url：/api/v1/debit-cards/request
method：PUT
```

- 请求

| Parameter |  Type  |   Requirement  |     Description         |
| :------------: | :----: | :----------: |:---------- |
|    card_no     | String |      必填    |银行卡ID          |
| acct_no | String | 必填    |机构端用户编号(机构端唯一) |
| request_type | int | 必填    | 1.冻结 2.解冻 3.挂失 4.重置密码 5.补卡 6.重发PIN|
| signature_doc   |  String | 必填    | 用户的签名照片，用base64格式|
| address   |  String | 选填    | 用户地址 |
| phone   |  String | 选填    | 用户手机号 |



- 响应：

```json
{
  "code": 0,
  "msg": "string",
  "result": true
}
```


### 工单查询冻结、解冻、挂失、重置密码、补卡、重发PIN的状态

```text
url：/api/v1/debit-cards/request?request_type={request_type}
method：GET
```

- 请求

| Parameter |  Type  |   Requirement  |     Description         |
| :------------: | :----: | :----------: |:---------- |
|    card_no     | String |      必填    |银行卡ID          |
| acct_no | String | 必填    |机构端用户编号(机构端唯一) |
| request_type | int | 必填    | 1.冻结 2.解冻 3.挂失 4.重置密码 5.补卡 6.重发PIN|



- 响应：

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
|   acct_no   | String |     机构端用户编号(机构端唯一)      |
|   card_no   |  int   |              银行卡ID               |
|   status    |  int   | 状态码: 0.处理中 1.成功 2.失败|
| create_time |  long  |              创建时间               |


### 查询快递单号

只针对需要我们代寄的实体卡。

```text
url：/api/v1/debit-cards/trackingnumber?month_year={month_year}
method：GET
```

- 请求

| Parameter |  Type  |   Requirement  |     Description         |
| :------------: | :----: | :----------: |:---------- |
| month_year | String | 必填    | 包裹发送的月份，如："202106"|

or 

| Parameter |  Type  |   Requirement  |     Description         |
| :------------: | :----: | :----------: |:---------- |
|    card_no     | String |      必填    |银行卡ID          |
| acct_no | String | 必填    |机构端用户编号(机构端唯一) |

- 响应：

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
|   acct_no   | String |     机构端用户编号(机构端唯一)      |
|   card_no   |  int   |              银行卡ID               |
|   website    |  String   | 快递公司网站|
|   tracking_number    |  String   | 包裹的快递单号|
| create_time |  long  |              创建时间               |


### 升级双币种卡

```text
url：/api/v1/debit-cards/upgrade
method：POST
```

- 请求

| Parameter |  Type  |   Requirement  |     Description         |
| :------------: | :----: | :----------: |:---------- |
|    card_no     | String |      必填    |银行卡ID          |
| acct_no | String | 必填    |机构端用户编号(机构端唯一) |



- 响应：

```json
{
  "code": 0,
  "msg": "string",
  "result": true
}
```

### 再次发送包含PIN码的邮件

```text
url：/api/v1/debit-cards/resendemail
method：POST
```

- 请求

| Parameter |  Type  |   Requirement  |     Description         |
| :------------: | :----: | :----------: |:---------- |
|    card_no     | String |      必填    |银行卡ID          |
| acct_no | String | 必填    |机构端用户编号(机构端唯一) |



- 响应：

```json
{
  "code": 0,
  "msg": "string",
  "result": true
}
```

## 充值卡



### 给用户卡充值

```text
内扣 url：/api/v1/deposit-transactions
外扣 url：/api/v1/deposit-transactions/external-deduction
method：POST
```

- 请求：

| Parameter |  Type  | Requirement  |Description                |
| :------------: | :----: | :----------: |:---------- |
|     card_no     | String | 必填|银行卡ID                   |
|     acct_no     | String | 必填|机构端用户编号(机构端唯一) |
|     amount      | String | 必填|充值对应币种的金额         |
|    coin_type    | String | 必填|币种。只支持USDT、RUSD       |
|   cust_tx_id    | String | 必填|机构的交易流水号           |
|     remark     | String | 选填|交易备注                   |
|   card_currency | String | 选填|卡币种，双币种卡才需要填写     |

- 响应：

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
|     tx_id      | String |  交易流水id  |
|    coin_type    | String | 充值币种       |
|    tx_amount      | String | 充值币种对应的金额         |
|     deposit_usdt      | String | 扣除手续费后,为用户充值的coin_type数量，单位是coin_type   |
|     exchange_fee_rate      | String | 充值币种兑换成coin_type的费率   |
|     exchange_fee      | String | 充值币种兑换成USDT的费用，单位是coin_type  |
|     loading_fee      | String | 充值手续费，单位是coin_type   |
|     currency_amount      | String | 到账法币数量  |
|     currency_type      | String | 到账法币类型  |
|     exchange_rate      | String | coin_type/USD汇率  |
|     fiat_exchange_rate      | String | 卡支持的法币/USD汇率  |

> 如果coin_type是USDT，从机构扣的USDT费用 = exchange_fee + loading_fee + deposit_usdt。


### 用稳定币给用户卡充值(固定到账法币金额)

```text
url：/api/v1/deposit-transactions/fiat-amount
method：POST
```

- 请求：

| Parameter |  Type  | Requirement  |Description                |
| :------------: | :----: | :----------: |:---------- |
|     card_no     | String | 必填|银行卡ID                   |
|     acct_no     | String | 必填|机构端用户编号(机构端唯一) |
|     credited_amount      | String | 必填|卡将到账的法币金额         |
|    coin_type    | String | 必填|充值使用的币种。只支持USDT      |
|   cust_tx_id    | String | 必填|机构的交易流水号           |
|     remark     | String | 选填|交易备注                   |
|   card_currency | String | 选填|卡币种，双币种卡才需要填写     |

- 响应：

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
|     tx_id      | String |  交易流水id  |
|    coin_type    | String | 充值币种       |
|    tx_amount      | String | 充值币种对应的金额         |
|     deposit_usdt      | String | 扣除手续费后,为用户充值的USDT数量，单位是USDT   |
|     exchange_fee_rate      | String | 充值币种兑换成USDT的费率   |
|     exchange_fee      | String | 充值币种兑换成USDT的费用，单位是USDT  |
|     loading_fee      | String | 充值手续费，单位是USDT   |
|     currency_amount      | String | 到账法币数量  |
|     currency_type      | String | 到账法币类型  |
|     exchange_rate      | String | USDT/USD汇率  |
|     fiat_exchange_rate      | String | 卡支持的法币/USD汇率  |



### 查询币对价格

```text
url：/api/v1/deposit-transactions/price
method：GET
```

- 请求：


- 响应：

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
                "update_time": "2024-05-31 07:19:30"
            },
            {
                "symbol": "USDC/USD",
                "price": "0.99814434",
                "update_time": "2024-05-31 07:19:30"
            }
        ]
    }
}
```

|  Parameter   |  Type  |        Description         |
| :--------: | :----: | :------------------------------ |
|  symbol   | String |         	币对名称        |
|    price    | String | 当前价格 |
|    update_time    | String | 价格更新时间 |

### 查询某笔卡充值交易状态

```text
url：/api/v1/deposit-transactions/{tx_id}/status
method：GET
```

- 请求：

| Parameter |  Type  |Requirement  | Description |
| :------------: | :----: | :----------: |:---------- |
|     tx_id      | String | 必填| 交易流水 tx_id 或 cust_tx_id  |

- 响应：

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

|  Parameter   |  Type  |        Description         |
| :--------: | :----: | :------------------------------ |
|  cust_tx_id   | String |         机构流水号         |
|    acct_no    | String | 机构端用户编号(机构端唯一) |
|    card_no    |  int   |          银行卡ID         |
|  cust_tx_time  |  long  |          创建时间          |
|  tx_id   | String |        交易id         |
|    coin_type    |  int   |          充值币种          |
|   tx_amount   | String |          充值金额          |
|  exchange_fee   | String |     充值币种兑换成USDT的费用，单位是 ```coin_type```   |
|      loading_fee      | String |  充值手续费，单位是 ```coin_type```           |
|     currency_type      | String | 到账法币类型  |
|     currency_amount      | String | 到账法币数量  |
| exchange_rate | String |            USDT/USD汇率            |
|   fiat_exchange_rate      | String | 卡支持的法币/USD汇率  |
|  tx_status   | int |   交易状态。0、3、4:待处理中，1:充值成功，2充值失败，5:充值失败        |
|   coin_price      | String | coin_type/USD汇率  |

### 查询所有卡充值记录

```text
url：/api/v1/deposit-transactions
method：GET
```

| Parameter  | Type | Requirement  |Description |
| :------------: | :----: | :----------: |:---------- |
|  page_num   | int  |    选填|页数     |
|  page_size  | int  |  选填|页的大小   |
| former_time | long |  选填|前置时间, UNIX 时间戳，`秒为单位`   |
| latter_time | long |  选填|后置时间, UNIX 时间戳，`秒为单位`   |
| time_sort | String | 选填|时间排序, asc为正序，desc为反序   |

- 响应：

```json
{
    "code": 0,
    "msg": "SUCCESS",
    "result": {
        "total": 1,
        "records": [
            {
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
        ]
    }
}
```

|  Parameter   |  Type  |        Description         |
| :--------: | :----: | :------------------------------ |
|  cust_tx_id   | String |         机构流水号         |
|    acct_no    | String | 机构端用户编号(机构端唯一) |
|    card_no    |  int   |          银行卡ID         |
|  cust_tx_time  |  long  |          创建时间          |
|  tx_id   | String |        交易id         |
|    coin_type    |  int   |          充值币种          |
|   tx_amount   | String |          充值金额          |
|  exchange_fee   | String |     充值币种兑换成USDT的费用，单位是 ```coin_type```    |
|      loading_fee      | String |  充值手续费，单位是 ```coin_type```           |
|     currency_type      | String | 到账法币类型  |
|     currency_amount      | String | 到账法币数量  |
| exchange_rate | String |            USDT/法币汇率            |
|   fiat_exchange_rate      | String | 卡支持的法币/USD汇率  |
|  tx_status   | int |   交易状态。0、3、4:待处理中，1:充值成功，2、5:充值失败         |
|   coin_price      | String | coin_type/USD汇率  |



### 查询指定用户所有卡充值记录

```text
url：/api/v1/deposit-transactions?acct_no={acct_no}
method：GET
```

- 请求：

| Parameter |  Type  | Requirement  |Description |
| :------------: | :----: | :----------: |:---------- |
|    acct_no     | String | 必填|机构用户的唯一id号 |
|  page_num   | int  |    选填|页数     |
|  page_size  | int  |  选填|页的大小   |
| former_time | long |  选填|前置时间, UNIX 时间戳，`秒为单位`   |
| latter_time | long |  选填|后置时间, UNIX 时间戳，`秒为单位`   |
| time_sort | String | 选填|时间排序, asc为正序，desc为反序   |


- 响应：

```json
{
    "code": 0,
    "msg": "SUCCESS",
    "result": {
        "total": 1,
        "records": [
            {
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
        ]
    }
}
```

|  Parameter   |  Type  |        Description         |
| :--------: | :----: | :------------------------------ |
|  cust_tx_id   | String |         机构流水号         |
|    acct_no    | String | 机构端用户编号(机构端唯一) |
|    card_no    |  int   |          银行卡ID         |
|  cust_tx_time  |  long  |          创建时间          |
|  tx_id   | String |        交易id         |
|    coin_type    |  int   |          充值币种          |
|   tx_amount   | String |          充值金额          |
|  exchange_fee   | String |     充值币种兑换成USDT的费用，单位是 ```coin_type```    |
|      loading_fee      | String |  充值手续费，单位是 ```coin_type```           |
|     currency_type      | String | 到账法币类型  |
|     currency_amount      | String | 到账法币数量  |
| exchange_rate | String |            USDT/USD汇率            |
|   fiat_exchange_rate      | String | 卡支持的法币/USD汇率  |
|  tx_status   | int |   交易状态。0、3、4:待处理中，1:充值成功，2充值失败，5:充值失败        |
|   coin_price      | String | coin_type/USD汇率  |


### 兑换USDT

```text
url：/api/v1/exchange-transactions
method：POST
```

- 请求：

| Parameter |  Type  | Requirement  |Description                |
| :------------: | :----: | :----------: |:---------- |
|     card_no     | String | 必填|银行卡ID                   |
|     acct_no     | String | 必填|机构端用户编号(机构端唯一) |
|     currency_amount      | String | 必填|法币金额         |
|   cust_tx_id    | String | 必填|机构的交易流水号           |
|     remark     | String | 选填|交易备注                   |
|   card_currency | String | 选填|卡币种，双币种卡才需要填写     |

- 响应：

```json
{
    "code": 0,
    "msg": "SUCCESS",
    "result": {
        "cust_tx_id": "1223",
        "acct_no": "03030062",
        "card_no": "8993152800000013334",
        "cust_tx_time": 1584350913000,
        "tx_id": "2020031609283339501898843",
        "coin_type": "USDT",
        "coin_amount": "59.4",
        "exchange_fee": "0",
        "currency_type": "USD",
        "currency_amount": "60",
        "exchange_rate": "1",
        "tx_status": 1
    }
}
```

| Parameter |  Type    | Description |
| :------------: | :----------: |:---------- |
|  cust_tx_id   | String |         机构流水号         |
|    acct_no    | String | 机构端用户编号(机构端唯一) |
|    card_no    |  String   |          银行卡ID         |
|  cust_tx_time  |  long  |          创建时间          |
|  tx_id   | String |        交易id         |
|    coin_type    |  int   |          购买币种          |
|   coin_amount   | String |          收到的金额          |
|  exchange_fee   | String |     充值币种兑换成USDT的费用，单位是 ```coin_type```    |
|     currency_type      | String | 法币类型  |
|     currency_amount      | String | 法币金额  |
| exchange_rate | String |            USDT/USD汇率            |
|  tx_status   | int |   交易状态。0:待处理中，1:兑换成功，2充值失败        |
| reason | String |           原因         |

### 查用户兑换记录

```text
url：/api/v1/exchange-transactions?acct_no={acct_no}&card_no={card_no}
method：GET
```

- 请求：

| Parameter |  Type  | Requirement  |Description |
| :------------: | :----: | :----------: |:---------- |
|    acct_no     | String | 必填|机构用户的唯一id号 |
|     card_no     | String | 必填|银行卡ID                   |
|     tx_id      | String | 必填| 交易流水 tx_id 或 cust_tx_id  |
|  page_num   | int  |    选填|页数     |
|  page_size  | int  |  选填|页的大小   |
| former_time | long |  选填|前置时间, UNIX 时间戳，`秒为单位`   |
| latter_time | long |  选填|后置时间, UNIX 时间戳，`秒为单位`   |
| time_sort | String | 选填|时间排序, asc为正序，desc为反序   |



- 响应：

```json
{
    "code": 0,
    "msg": "SUCCESS",
    "result": {
        "total": 1,
        "records": [
            {
                "cust_tx_id": "1223",
                "acct_no": "03030062",
                "card_no": "8993152800000013334",
                "cust_tx_time": 1584350913000,
                "tx_id": "2020031609283339501898843",
                "coin_type": "USDT",
                "coin_amount": "59.4",     
                "exchange_fee": "0",
                "currency_type": "USD",                
                "currency_amount": "60",
                "exchange_rate": "1",
                "tx_status": 1,
                "reason": ""
            }
        ]
    }
}
```

|  Parameter   |  Type  |        Description         |
| :--------: | :----: | :------------------------------ |
|  cust_tx_id   | String |         机构流水号         |
|    acct_no    | String | 机构端用户编号(机构端唯一) |
|    card_no    |  String   |          银行卡ID         |
|  cust_tx_time  |  long  |          创建时间          |
|  tx_id   | String |        交易id         |
|    coin_type    |  int   |          购买币种          |
|   coin_amount   | String |          兑换到的金额          |
|  exchange_fee   | String |     充值币种兑换成USDT的费用，单位是 ```coin_type```    |
|     currency_type      | String | 法币类型  |
|     currency_amount      | String | 法币数量  |
| exchange_rate | String |            USDT/USD汇率            |
|  tx_status   | int |   交易状态。0:待处理中，1:兑换成功，2充值失败        |
| reason | String |           原因         |

## 信用卡

使用授信接口更安全一些，我们内部做了金额交验。也可以直接使用修改限额接口，自己做校验。

### 授信


```text
url：/api/v1/credit/limit
method：POST
```
available_amount，amount_limit 参数如果不知道如何填写可以从查询接口查到，amount_limit在商户的数据库里也有保存。这两个参数的作用是，商户确定当前卡里可用余额和限额是正确的。基于这个条件，再给用户设置新的可用额度new_available_amount。

- 请求：

| Parameter |  Type  | Requirement  |Description                |
| :------------: | :----: | :----------: |:---------- |
|     card_no     | String | 必填|银行卡ID                   |
|     acct_no     | String | 必填|机构端用户编号(机构端唯一) |
|     available_amount      | String | 必填|当前可用金额         |
|     amount_limit      | String | 必填|当前授信金额         |
|     new_available_amount      | String | 必填|新的可用金额         |
|   cust_tx_id    | String | 必填|机构的交易流水号           |
|     remark     | String | 必填|交易备注                   |

- 响应：

```json
{
    "code": 0,
    "msg": "SUCCESS",
    "result": {
      "tx_id":"2022062410453502002436064",
      "cust_tx_id":"df6c7d60-b34c-4f7a-bdab-b9ed6560f748",
      "acct_no":"0624002",
      "card_no":"4355469889900027728",
      "currency_type":"USD",
      "amount_limit": "8000", // 当前限额
      "old_amount_limit":"10000", // 旧限额
      "tx_type": "02",  // 01 是增加授信，02是减少授信
      "tx_amount": "2000", // 变化的金额
      "available_amount": "2000",    
      "old_available_amount": "4000",
      "remark":"test"
    }
}
```

| Parameter |  Type    | Description |
| :------------: | :----------: |:---------- |
|     tx_id      | String |  交易流水id  |
|     cust_tx_id      | String | 机构交易流水id  |
|     acct_no     | String |机构端用户编号(机构端唯一) |
|     card_no     | String |银行卡ID    |
|    currency_type    | String | 卡币种       |
|    amount_limit      | String | 当前限额         |
|    old_amount_limit      | String | 旧限额         |
|    tx_type      | String | 交易类型。 01 是增加授信，02 是扣减授信        |
|    tx_amount      | String | 变化的金额         |
|     available_amount      | String | 可用金额  |
|     old_available_amount      | String | 旧的可用金额  |

### 查询授信记录

```text
url：/api/v1/credit/limit?acct_no={acct_no}&card_no={card_no}
method：GET
```

- 请求：

| Parameter |  Type  | Requirement  |Description |
| :------------: | :----: | :----------: |:---------- |
|    acct_no     | String | 必填|机构用户的唯一id号 |
|     card_no     | String |必填|银行卡ID    |
|     cust_tx_id      | String |  选填| 机构交易流水id  |
|  page_num   | int  |    选填|页数     |
|  page_size  | int  |  选填|页的大小   |
| former_time | long |  选填|前置时间, UNIX 时间戳，`秒为单位`   |
| latter_time | long |  选填|后置时间, UNIX 时间戳，`秒为单位`   |
| time_sort | String | 选填|时间排序, asc为正序，desc为反序   |


- 响应：

```json


{
  "code": 0,
  "msg": "SUCCESS",
  "result": {
    "total": 1,
    "records": [
      {
        "tx_id":"2022062410453502002436064",
        "cust_tx_id":"df6c7d60-b34c-4f7a-bdab-b9ed6560f748",
        "acct_no":"0624002",
        "card_no":"4355469889900027728",
        "currency_type":"USD",
        "amount_limit": "8000", // 当前限额
        "old_amount_limit":"10000", // 旧限额
        "tx_type": "02",  // 01 是增加授信，02是减少授信
        "tx_status": "1", // "0" 处理中，"1" 是成功，"2" 是失败 
        "tx_amount": "2000", // 变化的金额
        "available_amount": "2000",    
        "old_available_amount": "4000",
        "remark":"test"
      }
    ]
  }
}


```

| Parameter |  Type    | Description |
| :------------: | :----------: |:---------- |
|     tx_id      | String |  交易流水id  |
|     cust_tx_id      | String | 机构交易流水id  |
|     acct_no     | String |机构端用户编号(机构端唯一) |
|     card_no     | String |银行卡ID                   |
|    currency_type    | String | 卡币种       |
|    amount_limit      | String | 当前限额         |
|    old_amount_limit      | String | 旧限额         |
|    tx_type      | String | 交易类型。 01 是增加授信，02 是减少授信        |
|    tx_status      | String | 交易状态。 "0" 处理中，"1" 是成功，"2" 是失败        |
|    tx_amount      | String | 变化的金额         |
|     available_amount      | String | 可用金额  |
|     old_available_amount      | String | 旧的可用金额  |


### 用户授信额度查询

```text
url：/api/v1/credit/user/detail?acct_no={acct_no}&card_no={card_no}
method：GET
```

- 请求：

| Parameter |  Type  | Requirement  |Description |
| :------------: | :----: | :----------: |:---------- |
|    acct_no     | String | 必填|机构用户的唯一id号 |
|     card_no     | String |必填|银行卡ID    |


- 响应：

```json


{
  "code": 0,
  "msg": "SUCCESS",
  "result": {
    "amount_limit": "10000",
    "available_amount": "1900",
    "pre_authorize_amount": "100"
  }
}


```

| Parameter |  Type    | Description |
| :------------: | :----------: |:---------- |
|     amount_limit      | String | 授信额度  |
|     available_amount      | String | 可用金额  |
|     pre_authorize_amount      | String | 预授权金额  |


### 还款


```text
url：/api/v1/credit/repayment
method：POST
```

- 请求：

| Parameter |  Type  | Requirement  |Description                |
| :------------: | :----: | :----------: |:---------- |
|     card_no     | String | 必填|银行卡ID                   |
|     acct_no     | String | 必填|机构端用户编号(机构端唯一) |
|     available_amount      | String | 必填|当前可用金额         |
|     repayment_amount      | String | 必填|还款金额         |
|   cust_tx_id    | String | 必填|机构的交易流水号           |
|     remark     | String | 选填|交易备注                   |

- 响应：

```json
{
    "code": 0,
    "msg": "SUCCESS",
    "result": {
      "tx_id":"2022062410453502002436064",
      "cust_tx_id":"df6c7d60-b34c-4f7a-bdab-b9ed6560f748",
      "acct_no":"0624002",
      "card_no":"4355469889900027728",
      "currency_type":"USD",
      "amount_limit": "10000", // 当前限额
      "repayment_amount": "3000", // 还款金额
      "available_amount": "11000",    
      "old_available_amount": "8000",
      "remark":"test"
    }
}
```

| Parameter |  Type    | Description |
| :------------: | :----------: |:---------- |
|     tx_id      | String |  交易流水id  |
|     cust_tx_id      | String | 机构交易流水id  |
|     acct_no     | String |机构端用户编号(机构端唯一) |
|     card_no     | String |银行卡ID    |
|    currency_type    | String | 卡币种       |
|    amount_limit      | String | 当前限额         |
|    repayment_amount      | String | 还款金额         |
|     available_amount      | String | 可用金额  |
|     old_available_amount      | String | 旧的可用金额  |

### 还款记录查询

```text
url：/api/v1/credit/repayment?acct_no={acct_no}&card_no={card_no}
method：GET
```

- 请求：

| Parameter |  Type  | Requirement  |Description |
| :------------: | :----: | :----------: |:---------- |
|    acct_no     | String | 必填|机构用户的唯一id号 |
|     card_no     | String |必填|银行卡ID    |
|     cust_tx_id      | String |  选填| 机构交易流水id  |
|  page_num   | int  |    选填|页数     |
|  page_size  | int  |  选填|页的大小   |
| former_time | long |  选填|前置时间, UNIX 时间戳，`秒为单位`   |
| latter_time | long |  选填|后置时间, UNIX 时间戳，`秒为单位`   |
| time_sort | String | 选填|时间排序, asc为正序，desc为反序   |


- 响应：

```json


{
  "code": 0,
  "msg": "SUCCESS",
  "result": {
    "total": 1,
    "records": [
      {
        "tx_id":"2022062410453502002436064",
        "cust_tx_id":"df6c7d60-b34c-4f7a-bdab-b9ed6560f748",
        "acct_no":"0624002",
        "card_no":"4355469889900027728",
        "currency_type":"USD",
        "amount_limit": "10000", // 当前限额
        "repayment_amount": "3000", // 还款金额
        "available_amount": "11000",
        "old_available_amount": "8000",
        "remark":"test"
      }
    ]
  }
}


```

| Parameter |  Type    | Description |
| :------------: | :----------: |:---------- |
|     tx_id      | String |  交易流水id  |
|     cust_tx_id      | String | 机构交易流水id  |
|     acct_no     | String |机构端用户编号(机构端唯一) |
|     card_no     | String |银行卡ID                   |
|    currency_type    | String | 卡币种       |
|    amount_limit      | String | 当前限额         |
|    repayment_amount      | String | 还款金额         |
|     available_amount      | String | 可用金额  |
|     old_available_amount      | String | 旧的可用金额  |


### 机构授信总额度查询

```text
url：/api/v1/credit/detail
method：GET
```

- 请求：

| Parameter |  Type  | Requirement  |Description |
| :------------: | :----: | :----------: |:---------- |
|    card_type_id     | String | 必填| 卡种id，如 60000004 |


- 响应：

```json


{
  "code": 0,
  "msg": "SUCCESS",
  "result": {
    "total_limit": "10000",
    "total_available": "2000"
  }
}


```

| Parameter |  Type    | Description |
| :------------: | :----------: |:---------- |
|     total_limit      | String | 总授信额度  |
|     total_available      | String | 总的可用余额  |


### 冻结

```
url：/api/v1/credit/lock
method：PUT
```

- 请求：

| Parameter |  Type  | Requirement  |Description |
| :------------: | :----: | :----------: |:---------- |
|    acct_no     | String | 必填|机构用户的唯一id号 |
|     card_no     | String |必填|银行卡ID    |

- 响应：

```json
{
  "code": 0,
  "msg": "string",
  "result": true
}
```

### 解冻

```
url：/api/v1/credit/unlock
method：PUT
```
注意该PUT接口的参数不是在body里，而是在URL里。

- 请求：

| Parameter |  Type  | Requirement  |Description |
| :------------: | :----: | :----------: |:---------- |
|    acct_no     | String | 必填|机构用户的唯一id号 |
|     card_no     | String |必填|银行卡ID    |

- 响应：

```json
{
  "code": 0,
  "msg": "string",
  "result": true
}
```

### 给卡种共享账户充值

```
url：/api/v1/credit/shareaccount/deposit
method：POST
```

- 请求：

| Parameter |  Type  | Requirement  |Description |
| :------------: | :----: | :----------: |:---------- |
|    card_type_id     | String | 必填| 卡种id，如 60000004 |
|     amount     | String |必填|金额，单位是usdt    |

- 响应：

```json
{
  "code": 0,
  "msg": "string",
  "result": {
  	"old_balance": "500",
	"balance": "1000",
  }
}
```

### 查卡种共享账户余额

```
url：/api/v1/credit/shareaccount/balance
method：GET
```

- 请求：

| Parameter |  Type  | Requirement  |Description |
| :------------: | :----: | :----------: |:---------- |
|    card_type_id     | String | 必填| 信用卡卡种id，如 60000004 |

- 响应：

```json
{
  "code": 0,
  "msg": "string",
  "result": {
  	"balance": "1000"
  }
}
```

## 共享卡


### 修改限额

```
url：/api/v1/share/modify/cardlimit
method：POST
```

- 请求：

| Parameter |  Type  | Requirement  |Description |
| :------------: | :----: | :----------: |:---------- |
|     card_no     | String | 必填|银行卡ID                   |
|     cust_tx_id     | String |必填| 唯一交易ID    |
|     amount_limit     | String |必填|金额，单位是usd    |

- 响应：

```json
{
  "code": 0,
  "msg": "string",
  "result": {
    "tx_id": "2024052712121212",
    "amount_limit": "1000",
    "old_amount_limit": "800",
    "available_amount": "300",
    "old_available_amount": "100"
  }
}
```


### 查询当前限额

```
url：/api/v1/share/query/cardlimit
method：GET
```

- 请求：

| Parameter |  Type  | Requirement  |Description |
| :------------: | :----: | :----------: |:---------- |
|     card_no     | String | 必填|银行卡ID                   |

- 响应：

```json
{
  "code": 0,
  "msg": "string",
  "result": {
    "amount_limit": "1000",
    "available_amount": "300",
  }
}
```

### 查询限额修改记录

```
url：/api/v1/share/query/cardlimit/history
method：GET
```

- 请求：

| Parameter |  Type  | Requirement  |Description |
| :------------: | :----: | :----------: |:---------- |
|     card_no     | String | 必填|银行卡ID                   |

- 响应：

```json
{
  "code": 0,
  "msg": "string",
  "result": {        
    "total": 1,
    "records": [{
        "cust_tx_id": "werwfgdfgdhgfg",
        "tx_id": "2024052712121212",
        "amount_limit": "1000",
        "old_amount_limit": "800",
        "available_amount": "300",
        "old_available_amount": "100",
        "status": 0, // 0 处理中，1 成功，2 失败
        "create_time": 1585293811000, 
        "update_time": 1585293811000
    }]
  }
}
```


### 冻结

```
url：/api/v1/share/lock
method：PUT
```

- 请求：

| Parameter |  Type  | Requirement  |Description |
| :------------: | :----: | :----------: |:---------- |
|    acct_no     | String | 必填|机构用户的唯一id号 |
|     card_no     | String |必填|银行卡ID    |

- 响应：

```json
{
  "code": 0,
  "msg": "string",
  "result": true
}
```

### 解冻

```
url：/api/v1/share/unlock
method：PUT
```
注意该PUT接口的参数不是在body里，而是在URL里。

- 请求：

| Parameter |  Type  | Requirement  |Description |
| :------------: | :----: | :----------: |:---------- |
|    acct_no     | String | 必填|机构用户的唯一id号 |
|     card_no     | String |必填|银行卡ID    |

- 响应：

```json
{
  "code": 0,
  "msg": "string",
  "result": true
}
```

### 给卡种共享账户充值

```
url：/api/v1/share/shareaccount/deposit
method：POST
```

- 请求：

| Parameter |  Type  | Requirement  |Description |
| :------------: | :----: | :----------: |:---------- |
|    card_type_id     | String | 必填| 卡种id，如 60000004 |
|     amount     | String |必填|金额，单位是usdt    |

- 响应：

```json
{
  "code": 0,
  "msg": "string",
  "result": {
  	"old_balance": "500",
	"balance": "1000",
  }
}
```

### 查卡种共享账户余额

```
url：/api/v1/share/shareaccount/balance
method：GET
```

- 请求：

| Parameter |  Type  | Requirement  |Description |
| :------------: | :----: | :----------: |:---------- |
|    card_type_id     | String | 必填| 信用卡卡种id，如 60000004 |

- 响应：

```json
{
  "code": 0,
  "msg": "string",
  "result": {
  	"balance": "1000"
  }
}
```

## 礼品卡

### 购买礼品卡

```text
url：/api/v1/gift-card/buy
method：POST
```

- 请求：

| Parameter |   Type    | Requirement | Description |
| :------------: |:---------:|:-----------:|:------------|
|   product_code |  String   |     必填      | 礼品卡产品编码     |
|   count |  int   |     必填      | 购买数量        |
|   cust_tx_id |  String   |     必填      | 商户订单号       |
|   email |  String   |     必填      | 邮箱       |

- 响应：

```json
{
    "code": 0,
    "msg": "SUCCESS",
    "result": {
        "total": 1,
        "records": [
            {
                "order_no": "2024010310441397701766379",
                "card_number": "1000000003",
                "card_code": "218JNNJ1N1IK3"
            }
        ]
    }
}
```

|    Parameter    |  Type   | Description         |
| :---------: |:-------:|:--------------------|
| order_no    | String  | 订单号                  |
| card_number    | String  | 卡号                  |
| card_code    | String  | Code                |

### 查询礼品卡

```text
url：/api/v1/gift-card
method：GET
```

- 请求：

|  Parameter   |  Type   | Requirement | Description |
|:------------:|:-------:|:-----------:|:------------|
|   page_num   |   int   |     选填      | 页数          |
|  page_size   | int |     选填      | 页的大小        |
|  cust_tx_id  | String  |     选填      | 商户订单号       |
| product_code | String  |     选填      | 礼品卡产品编码     |
| card_number | String  |     选填      | 礼品卡卡号       |


- 响应：

```json
{
    "code": 0,
    "msg": "SUCCESS",
    "result": {
        "total": 1,
        "records": [
            {
                "order_no": "2024010310441397701766379",
                "cust_tx_id": "020351010",
                "product_name": "99U Get 100 USD Amazon USA",
                "product_code": "GC202401020351010825322",
                "face_value": 2.00,
                "currency": "USD",
                "card_number": "1000000003",
                "card_code": "218JNNJ1N1IK3",
                "status": 2,
                "refund_status": 0
            }
        ]
    }
}
```


|    Parameter    |  Type   | Description         |
| :---------: |:-------:|:--------------------|
| order_no    | String  | 订单号                  |
| cust_tx_id    | String  | 机构订单号                  |
| card_number    | String  | 卡号                  |
| card_code    | String  | Code                |
|  face_value    | Decimal | 面值                     |
|  currency  | String  | 币种                     |
| card_number    | String  | 卡号                     |
| card_code    | String  | Code                   |
|  status    | Integer | 卡片状态, 1待售 2已售          |
| refund_status    | Integer  | 0未退款 1退款中 2已退款 3退款驳回   |


## 银行卡查询

提供银行卡消费记录等接口

### 查询卡是否激活

```text
url：/api/v1/bank/account-status
method：POST
```

- 请求：

| Parameter |  Type  | Requirement  |Description |
| :------------: | :----: | :----------: |:---------- |
|     card_no     | String | 必填|银行卡ID |

- 响应：

```json
{
  "code": 0,
  "msg": "string",
  "result": true
}
```


### 查询卡余额

```text
url：/api/v1/bank/balance
method：POST
```

- 请求：

| Parameter |  Type  | Requirement  |Description |
| :------------: | :----: | :----------: |:---------- |
|     card_no     | String |必填| 银行卡ID |
|   card_currency | String | 选填|卡币种，双币种卡才需要填写     |

- 响应：

```json
{
    "code": 0,
    "msg": "SUCCESS",
    "result": {
        "card_number": "438521******2001",
        "card_type": "EGEN BLUE",
        "current_balance": "121.12454",
        "current_balance_usd": "121.12454",
        "available_balance": "10.23",
        "available_balance_usd": "10.23"
    }
}
```

| Parameter |  Type  |          Description          |
| :--------: | :----: | :------------------------------ |
|   card_number   | String |         真实银行卡号           |
|   card_type   | String |         银行卡类型           |
|   current_balance   | String |      当前余额（卡支持的货币）           |
|   current_balance_usd   | String |      当前余额（USD）           |
|   available_balance   | String |   可用余额（卡支持的货币）           |
|   available_balance_usd   | String |   可用余额（USD）           |


### 查询卡账单 

```text
url：/api/v1/bank/transaction-record
method：POST
```

- 请求：

| Parameter |  Type  | Requirement  |Description |
| :------------: | :----: | :----------: |:---------- |
|     card_no     | String | 必填|银行卡ID |
|   card_currency | String | 选填|卡币种，双币种卡才需要填写     |
|     former_month_year     | String | 必填|指定查询的月份(格式“012020”) |
|     latter_month_year     | String | 必填|指定查询的月份(格式“012020”) |

- 响应：

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
                  "fee": "0",
                  "end_bal": "end_bal", //如果是0，代表没交易后余额
                  "origin_transaction_id": "",
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
                  "fee": "0",
                  "end_bal": "end_bal", //如果是0，代表没交易后余额
                  "origin_transaction_id": "",
                  "type": 1
              }
          ]
      }
    ]
 }   
```

| Parameter |  Type  |          Description          |
| :--------: | :----: | :------------------------------ |
|   month_year   | String |  日期，MMyyyy |
|   statement_cycle_date   | String |  报表生成日期 |
|   opening_balance   | String | 起始余额(卡支持的货币)  |
|   opening_balance_usd   | String | 起始余额(USD)  |
|   closing_balance   | String | 截止余额(卡支持的货币)  |
|   closing_balance_usd    | String | 截止余额(USD)  |
|   available_balance   | String | 可用余额(卡支持的货币)  |
|   available_balance_usd   | String | 可用余额(USD)  |
|   bank_tx_list[n]   | Object | 交易列表  |
|   bank_tx_list[0].transaction_date   | String | 交易日期  |
|   bank_tx_list[0].posting_date   | String | 交易提交日期  |
|   bank_tx_list[0].tx_id   | String | 交易ID |
|   bank_tx_list[0].description   | String | 描述  |
|   bank_tx_list[0].debit   | String | 消费金额(卡支持的货币)  |
|   bank_tx_list[0].debit_usd   | String | 消费金额(USD)  |
|   bank_tx_list[0].credit   | String | 存入金额(卡支持的货币)   |
|   bank_tx_list[0].credit_usd   | String | 存入金额(USD)  |
|   bank_tx_list[0].fee   | String | 手续费，只有部分卡有值。  |
|   bank_tx_list[0].type   | int | 交易类型，1.消费 2.充值 3.取款 4.转账(转入) 5.转账(转出) 6.其他 7.结算调整 8.退款 9.消费失败 10.verification（绑卡验证交易），11.void（撤销,付款后撤销，退回资金） |
|   bank_tx_list[0].reason   | String | 交易失败原因  |
|   bank_tx_list[0].tx_currency   | String | 实际交易货币  |
|   bank_tx_list[0].tx_amount   | String | 实际交易货币的交易金额  |
|   bank_tx_list[0].end_bal   | String | 交易后余额。如果是0，代表没交易后余额  |
|   bank_tx_list[0].origin_transaction_id | String | 原消费交易id，退款交易才可能有值  |

### 查询卡敏感信息

- Request:

```text
虚拟卡 url：/api/v1/bank/virtualcard， 实体卡和金属卡 url：/api/v1/bank/card
method：GET 或 POST
```

|  Parameter  | Type  | Whether Required |                        Description                         |
| :---------: | :---: | :--------------: | :-------------------------------------------------------- |
| card_no  |  String  |    Required     | card no     |
| publickey  |  String  |  Optional     | 我们会使用这个公钥替代默认的公钥加密数据，支持ECIES和RSA   |

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

实体卡解密：
{"card_number":"123456789101212"}

虚拟卡解密：
{"cvv":"123","card_number":"1001022400001101","expire":"022022"}

A卡虚拟卡解密：
{"cvv":"***","card_number":"****************","expire":"******"，"password":"343545"}

```
|    Parameter    |  Type   |      Description                                                     |
| :---------: | :----:   | :--------------------------- |
|  encrypt_data    | String[]  |    加密后的数据          |
|  public_key  |  String    |  公钥    |

> encryt_data解密请参考[RSA例子](https://github.com/paycrypto-com/paycrypto-sdk-java/blob/master/src/test/java/com/railone/open/api/test/RSATest.java#L60) 或 [ECIES例子](https://github.com/paycrypto-com/paycrypto-sdk-java/blob/master/src/test/java/com/railone/open/api/test/BankTest.java#L75)

### 用户请求重置密码（暂不支持）

机构调 Paycrypto 接口，Paycrypto 通知银行发送设置取款密码链接的邮件给用户。

```text
url：/api/v1/bank/reset-pwd
method：POST
```

- 请求

| Parameter |  Type  |   Requirement  |     Description         |
| :------------: | :----: | :----------: |:---------- |
|    card_no     | String |      必填    |银行卡号          |
| acct_no | String | 必填    |机构端用户编号(机构端唯一) |


- 响应：

```json
{
  "code": 0,
  "msg": "string",
  "result": true
}
```

### 查询卡已授权交易

```text
url：/api/v1/bank/authorizedtransaction
method：GET
```

注意只有部分卡，PT和PH卡能查到已预授权的交易

- 请求：

| Parameter |  Type  | Requirement  |Description |
| :------------: | :----: | :----------: |:---------- |
|     card_no     | String | 必填|银行卡ID |


- 响应：

```json
{
    "code": 0,
    "msg": "SUCCESS",
    "result": [
              {
                  "transaction_date": "20/11/2019",
                  "posting_date": "20/11/2019",
                  "tx_id": "54675678678",                  
                  "description": "MONTHLY FEE",
                  "debit": "2.50",
                  "debit_usd": "2.50",
                  "credit": "",
                  "credit_usd": "",
                  "fee": "0"
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
                  "fee": "0"
              }
    ]
 }   
```

| Parameter |  Type  |          Description          |
| :--------: | :----: | :------------------------------ |
|   transaction_date   | String | 交易日期  |
|   posting_date   | String | 交易提交日期  |
|   tx_id   | String | 交易ID |
|   description   | String | 描述  |
|   debit   | String | 消费金额(卡支持的货币)  |
|   debit_usd   | String | 消费金额(USD)  |
|   credit   | String | 存入金额(卡支持的货币)   |
|   credit_usd   | String | 存入金额(USD)  |
|   fee   | String | 手续费，只有部分卡有值。  |
|   tx_currency   | String | 实际交易货币  |
|   tx_amount   | String | 实际交易货币的交易金额  |



### 信用卡月限额修改

```text
url：/api/v1/bank/cardlimit
method：POST
```

- 请求：

| Parameter |  Type  | Requirement  |Description |
| :------------: | :----: | :----------: |:---------- |
|     card_no     | String |必填| 银行卡ID |
|   max_amount_single   | String | 必填|     当笔限额          |
|   max_amount_daily   | String | 必填|     日限额           |
|   max_amount_monthly   | String |  必填| 月限额           |

- 响应：

```json
{
    "code": 0,
    "msg": "SUCCESS",
    "result": {
        "card_no": "438521******2001",
        "card_type": "60000001",
        "max_amount_single": "1000",
        "max_amount_daily": "2000",
        "max_amount_monthly": "100000",
        "available_balance": "100",
    }
}
```

| Parameter |  Type  |          Description          |
| :--------: | :----: | :------------------------------ |
|   card_no   | String |         银行卡ID           |
|   card_type   | String |         银行卡类型           |
|   max_amount_single   | String |      当笔限额          |
|   max_amount_daily   | String |      日限额           |
|   max_amount_monthly   | String |   月限额           |
|   available_balance   | String |   可用余额           |

### 查询信用卡月限额

```text
url：/api/v1/bank/cardlimit
method：GET
```

- 请求：

| Parameter |  Type  | Requirement  |Description |
| :------------: | :----: | :----------: |:---------- |
|     card_no     | String |必填| 银行卡ID |

- 响应：

```json
{
    "code": 0,
    "msg": "SUCCESS",
    "result": {
        "card_no": "438521******2001",
        "card_type": "60000001",
        "max_amount_single": "1000",
        "max_amount_daily": "2000",
        "max_amount_monthly": "100000",
        "available_balance": "100",
    }
}
```

| Parameter |  Type  |          Description          |
| :--------: | :----: | :------------------------------ |
|   card_no   | String |         银行卡ID           |
|   card_type   | String |         银行卡类型           |
|   max_amount_single   | String |      当笔限额          |
|   max_amount_daily   | String |      日限额           |
|   max_amount_monthly   | String |   月限额           |
|   available_balance   | String |   可用余额           |



### 卡冻结

```text
url：/api/v1/bank/freeze
method：POST
```

- 请求：

| Parameter |  Type  | Requirement  |Description |
| :------------: | :----: | :----------: |:---------- |
|     card_no     | String |必填| 银行卡ID |

- 响应：

```json
{
    "code": 0,
    "msg": "SUCCESS",
    "result": {}
}
```


### 卡解冻

```text
url：/api/v1/bank/unfreeze
method：POST
```

- 请求：

| Parameter |  Type  | Requirement  |Description |
| :------------: | :----: | :----------: |:---------- |
|     card_no     | String |必填| 银行卡ID |

- 响应：

```json
{
    "code": 0,
    "msg": "SUCCESS",
    "result": {}
}
```



## 验证码（不需要对接）

邮箱和手机验证码等接口。

### 发送邮箱验证码

```text
url：/api/v1/emails/{email}/verification-codes
method：POST
```

- 请求：

| Parameter |  Type  |   Requirement  | Description   |
| :------------: | :----: | :----------: |:---------- |
| email | String |必填|email|

- 响应：

```json
{
  "code": 0,
  "msg": "string",
  "result": {
  	"mail_token":"xxxxxx"
  }
}
```

| Parameter |  Type  |          Description          |
| :--------: | :----: | :------------------------------ |
|   mail_token   | String |           分配的验证token           |



### 校验邮箱验证码（不需要对接）

```text
url：/api/v1/emails/{email}/verification-codes?code={code}&mail_token={mail_token}
method：PUT
```

- 请求：

| Parameter |  Type  | Requirement  |Description |
| :------------: | :----: | :----------: |:---------- |
|     email      | String | 必填|email       |
|      code      | String | 必填|code        |
|   mail_token   | String |       必填|    分配的验证token           |

- 响应：

```json
{
  "code": 0,
  "msg": "string",
  "result": true
}
```

### 发送手机验证码 (暂不支持)

```text
url：/api/v1/mobiles/{mobile}/verification-codes
method：POST
```

- 请求：

| Parameter |  Type  | Requirement  |Description |
| :------------: | :----: | :----------: |:---------- |
|     mobile     | String | 必填|区号+手机号 |

- 响应：

```json
{
  "code": 0,
  "msg": "string",
  "result": {
  	"mobile_token":"xxxxxx"
  }
}
```

| Parameter |  Type  |          Description          |
| :--------: | :----: | :------------------------------ |
|   mobile_token   | String |           分配的验证token         |




### 校验手机验证码(暂不支持)

```text
url：/api/v1/mobiles/{mobile}/verification-codes?code={code}&mobile_token={mobile_token}
method：PUT
```

- 请求：

| Parameter |  Type  | Requirement  |Description |
| :------------: | :----: | :----------: |:---------- |
|     mobile     | String | 必填|区号+手机号 |
|      code      | String | 必填|code        |
|   mobile_token   | String |       必填|    分配的验证token         |

- 响应：

```json
{
  "code": 0,
  "msg": "string",
  "result": true
}
```



## 事件推送

请先在机构端后台配置 Webhook 回调地址, 收到推送后验签。推送事件的数据结构为：

| 名称| 类型|描述 |
| --- | --- |--- |
| action |String | 推送类型  |
| events | String[] | 推送的消息数组 |
| events[n].params | Object | 本次推送的消息内容，JSON 对象 |
| events[n].id | String | 推送的唯一标识符 |
| events[n].create_time|long |  事件发生的时间，默认为UTC时间 |



推送的请求头数据结构为：

| 名称| 类型|描述 |
| --- | --- |--- |
| Signature |String | 签名  |
| Timestamp | String | 时间戳 |

```
--header Timestamp：1585310160226
--header Signature：UqAwtsx9HF3s5yJh/c8luvUITZNXE/f3aujwndnXLBU=

```

如何验签请见：[Paycrypto 推送验签流程](https://github.com/paycrypto-com/paycrypto-sdk-java/blob/master/src/test/java/com/railone/open/api/test/NotificationTest.java)



机构接受推送后，响应的数据结构如下，**如果机构返回正确数据结构，我们将不再推送该事件**：

| 名称| 类型|描述 |
| --- | --- |--- |
| code | int   |  0: success, other: failure |
|msg  |String  | 错误码描述 |


响应例子：

```

{
   "code": 0,
   "msg": "SUCCESS"
}
```



### 推送 KYC 事件

| 名称| 类型|描述 |
| --- | --- |--- |
| action  |String| kyc-status |
| events[n].params.acct_no |String | 机构下用户唯一ID |
| events[n].params.card_type_id |String | 卡类型 |
| events[n].params.status  |int| KYC状态, 1. 成功, 2. 失败 |

示例：
```
{
    "action": "kyc-status",
    "events": [
        "{\"id\":\"bc76488ddda4\",\"create_time\":1585293811000,\"params\":{\"card_type_id\":\"50010003\",\"acct_no\":\"032500004\",\"status\":1}}"
    ]
}


events 数组元素从 string 转成 json:
{
       "id": "bc76488ddda4",
       "create_time": 1585293811000,
       "params":{
           "card_type_id": "50010003",
           "acct_no": "032500004",
           "face_recognition_url": "https://www.hh.io", //认证中才可能有活体认证链接，否则不推送认证中
           "status": 1
       }
}
```


### 推送开卡事件

银行卡已制作完成，请申请银行卡。

| 名称| 类型|描述 |
| --- | --- |--- |
| action |String  |  card-application-ready|
| events[n].params.acct_no |String | 机构下用户唯一ID |
| events[n].params.card_type_id |String | 卡类型 |

示例：
```
{
    "action": "card-application-ready",
    "events": [
        "{\"id\":\"bc76488ddda4\",\"create_time\":1585293811000,\"params\":{\"card_type_id\":\"50010003\",\"acct_no\":\"032500004\"}}"
    ]
}

events 数组元素从 string 转成 json:
{
       "id": "bc76488ddda4",
       "create_time": 1585293811000,
       "params":{
           "card_type_id": "50010003",
           "acct_no": "032500004"
       }
}
```

### 推送卡激活事件

| 名称| 类型|描述 |
| --- | --- |--- |
| action|String  | card-status |
| events[n].params.card_no |String | 卡ID |
| events[n].params.status |int | 卡激活状态, 0.冻结, 1.卡激活成功, 4.卡激活审核失败 |

示例：
```
{
    "action": "card-status",
    "events": [
        "{\"id\":\"bc76488ddda4\",\"create_time\":1585293811000,\"params\":{\"card_no\":\"123434234343\",\"status\":1}}"
    ]
}


events 数组元素从 string 转成 json:
{
       "id": "bc76488ddda4",
       "create_time": 1585293811000,
       "params":{
           "card_no": "123434234343",
           "status": 1
       }
}
```

### 推送卡充值事件

| 名称| 类型|描述 |
| --- | --- |--- |
| action |String |  tx-status|
| events[n].params.tx_id |String | 交易ID |
| events[n].params.status  |int| 卡充值状态, 1.成功, 2.失败, 5.取消 |

示例：
```
{
    "action": "tx-status",
    "events": [
        "{\"id\":\"bc76488ddda4\",\"create_time\":1585293811000,\"params\":{\"tx_id\":\"2020031609283339501898843\",\"status\":1}}"
    ]
}

events element convert string to json:
{
       "id": "bc76488ddda4",
       "create_time": 1585293811000,
       "params":{
           "tx_id": "2020031609283339501898843",
           "status": 1
       }
}
```

### 推送信用卡消费事件

| 名称| 类型|描述 |
| --- | --- |--- |
| action |String |  creditcard-balance-updated |
| events[n].params.tx_id |String | 交易ID |
| events[n].params.card_no |String | 卡ID |

示例：
```
{
    "action": "creditcard-balance-updated",
    "events": [
        "{\"id\":\"bc76488ddda4\",\"create_time\":1585293811000,\"params\":{\"card_no\": \"78833000000198766\",\"tx_id\":\"2020031609283339501898843\",\"type\":1}}"
    ]
}

events element convert string to json:
{
       "id": "bc76488ddda4",
       "create_time": 1585293811000,
       "params":{
           "tx_id": "2020031609283339501898843",
           "card_no": "78833000000198766"
       }
}
```

### 推送充值卡消费事件

| 名称| 类型|描述 |
| --- | --- |--- |
| action |String |  depositcard-balance-updated |
| events[n].params.tx_id |String | 交易ID |
| events[n].params.card_no |String | 卡ID |

示例：
```
{
    "action": "depositcard-balance-updated",
    "events": [
        "{\"id\":\"bc76488ddda4\",\"create_time\":1585293811000,\"params\":{\"card_no\": \"78833000000198766\",\"tx_id\":\"2020031609283339501898843\",\"type\":1}}"
    ]
}

events element convert string to json:
{
       "id": "bc76488ddda4",
       "create_time": 1585293811000,
       "params":{
           "tx_id": "2020031609283339501898843",
           "card_no": "78833000000198766"
       }
}
```


### 推送3ds OTP验证码

当卡消费时，如果需要3ds，推送3ds OTP验证码

| 名称| 类型|描述 |
| --- | --- |--- |
| action |String |  card-3ds-otp |
| events[n].params.otp |String | 验证码 |
| events[n].params.card_no |String | 卡id |
| events[n].params.transaction_currency |String | 交易货币 |
| events[n].params.transaction_amount |String | 交易金额 |
| events[n].params.merchant_name |String | 商户名 |

示例：
```
{
    "action": "card-3ds-otp",
    "events": [
        "{\"id\":\"bc76488ddda4\",\"create_time\":1585293811000,\"params\":{\"card_no\": \"78833000000198766\",\"otp\":\"234235\",\"transaction_currency\":\"USD\",\"transaction_amount\":\"100\",\"merchant_name\":\"Example Merchant\"}}"
    ]
}

events element convert string to json:
{
       "id": "bc76488ddda4",
       "create_time": 1585293811000,
       "params":{
           "otp": "234235",
           "card_no": "78833000000198766",
           "transaction_currency": "USD", 
           "transaction_amount": "100", 
           "merchant_name": "Example Merchant"
       }
}
```

### 测试推送事件

```text
url：/api/v1/events/test
method：POST
```

- 请求：

| Parameter |  Type  | Requirement  |Description |
| :------------: | :----: | :----------: |:---------- |
| action |String | optional | deposit-status, card-3ds-otp |

- 响应：


| 名称| 类型|描述 |
| --- | --- |--- |
| action |String |  action名称 |
| events[n].params |Object | 事件参数 |

示例：
```
{
    "action": "deposit-status",
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

### 查询推送失败的事件

我们每隔一分钟推送一次事件，每个事件我们最多推送5次，推送5次都失败表示该事件推送失败，将不会再推送。查询推送失败的事件请用此接口：

```text
url：/api/v1/events
method：GET
```

- 请求：

| Parameter |  Type  | Requirement  |Description |
| :------------: | :----: | :----------: |:---------- |
|    action     | String | 选填| 事件名称，默认值是所有类型action |
|  page_num   | int  |    选填|页数     |
|  page_size  | int  |  选填|页的大小   |


- 响应：

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


### 更新推送失败的事件

更新成功后，标记为推送成功，无法再通过```查询推送失败的事件```接口查到。

```text
url：/api/v1/events
method：PUT
```

- 请求：

| Parameter |  Type  | Requirement  |Description |
| :------------: | :----: | :----------: |:---------- |
|    event_id     | String[] | 必填| 事件id |


```json
{
  "event_id": [
      "bc7648da4f9c466aa8bad56c3c8ddda4",
      "cc7648da4f9c466aa8bad56c3c8ddda3"
  ]
}
```



- 响应：

```json
{
  "code": 0,
  "msg": "string",
  "result": true
}
```

### 推送冻结、解冻、挂失、重置密码、补卡状态


| 名称| 类型|描述 |
| --- | --- |--- |
| action|String  | card-lock-status |
| events[n].params.card_no |String | 卡ID |
| events[n].params.status |int | 0.处理中 1.成功 2.失败 |
| events[n].request_type | int |  1.冻结 2.解冻 3.挂失 4.重置密码 5.补卡|

示例：
```
{
    "action": "card-lock-status",
    "events": [
        "{\"id\":\"bc76488ddda4\",\"create_time\":1585293811000,\"params\":{\"card_no\":\"123434234343\",\"request_type\":1,\"status\":1}}"
    ]
}


events 数组元素从 string 转成 json:
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

## 错误码

### 业务逻辑错误码

| 状态值 | 描述|
| :--------------: | --------|
|   0 |   成功  |
|   111001 |  请求参数错误 |
|   111002 |  KYC 状态异常  |
|   111003 |   KYC 失败   |
|   111004 |   KYC 重复 |
|   111005 |   无法找到对应的 KYC  |
|   111006 |   无法找到对应的机构  |
|   111007 |  机构状态异常  |
|   111008 |  无法找到对应的机构资产配置  |
|   111009 |   机构资产状态异常  |
|   111010 |    无法找到对应的机构配置  |
|   111011 |   机构余额不足  |
|   111012 |   无法找到对应的卡  |
|   111013 |   卡状态异常 |
|   111014 |   该卡种的卡不足  |
|   111015 |   该银行卡已被冻结  |
|   111016 |   无法找到对应的卡种ID  |
|   111017 |   该卡已被开卡  |
|   111018 |   网银没有激活  |
|   111019 |   卡ID重复  |
|   111020 |   用户每种卡只能开一张  |
|   111021 |   该机构没有开此类卡种的权限  |
|   111022 |   无法找到对应的交易  |
|   111023 |   没有查询该交易的权限 |
|   111024 |  邮箱格式不合法  |
|   111025 |   邮箱验证码错误  |
|   111026 |   邮箱重复  |
|   111027 |   不支持该邮箱供应商  |
|   111028 |   手机号重复  |
|   111029 |   手机验证码错误  |
|   111030 |   无法获取银行数据  |
|   111031 |   无法找到对应的银行  |
|   111032 |   无法找到对应的配置  |
|   111033 |   生日参数不能为空  |
|   111034 |   无法上传照片  |
|   111035 |   查询时间不允许超过一个月  |
|   111036 |   时间格式错误  |
|   111037 |   查询的银行账单时间不允许超过6个月  |

### 身份权限认证错误码

| 状态值 | 描述|
| :--------------: | --------|
|   112001 |   请求超时  |
|   112002 |   非法权限 |
|   112003 |  非法IP地址  |
|   112004 |   非法时间戳  |
|   112005 |   验签失败 |
|   112006 |   认证格式错误  |
|   112007 |   签名错误  |
|   112008 |   没有找到对应的 app key  |
|   112009 |   无效的 app key秘钥  |
|   112010 |   请求头错误  |

### 异常错误码

| 状态值 | 描述|
| :--------------: | --------|
|   119001 |  服务不可用  |
|   119002 |   通信错误  |
|   119003 |   数据加密错误  |
|   119004 |   数据解密错误  |
|   119005 |   api调用过于频繁  |
|   119006 |   该api没有被授权  |
|   119007 |   公钥格式错误  |



### KYC失败错误码

| 状态值 | 描述|
| :--------------: | --------|
|   111003 |   KYC 失败   |
|   1110031 |   KYC 失败, country或nationality填写错误   |
|   1110032|   KYC 失败，证件照片错误   |
|   1110033 |   KYC 失败，手持证件照错误或不够清晰   |
|   1110034 |   KYC 失败，地址错误   |
|   1110035 |   KYC 失败，手机号错误   |
|   1110036 |   KYC 失败，birthday错误   |
|   1110037 |   KYC 失败，名字错误   |
|   1110038 |   KYC 失败，邮编错误   |
|   1110039 |   KYC 失败，其他错误   |
|   1110040 |   KYC 失败，护照过期或即将过期   |
|   1110041 |   KYC 失败，护照不完整或不清晰   |
|   1110042 |   KYC 失败，性别填写错误   |
|   1110043 |   KYC 失败，手持照不清晰或不完整   |
|   1110044 |   KYC 失败，手持照中护照文字是反的   |
|   1110045 |   KYC 失败，手持照没露脸   |

失败原因填写在KYC结果的参数```reason```里，内容如：

```
{
   "code": 1110032,
   "msg": "KYC failure, photo error"
}
```


### 卡激活失败错误码

| 状态值 | 描述|
| :--------------: | --------|
|   1110060 |   激活失败   |
|   1110061 |   激活失败, 激活照中手持的不是塑料卡   |
|   1110062|   激活失败，激活照中没有人像   |
|   1110063 |   激活失败，激活照中护照或卡片信息不完整或者不清晰   |
|   1110064 |   激活失败，激活照错误，没有用手指挡住芯片凹槽   |
|   1110065 |   激活失败，激活照是黑白的，需要拍摄彩色照片   |
|   1110066 |   激活失败，激活照中护照上的文字不清晰   |


失败原因填写在激活结果的参数```reason```里，内容如：

```
{
   "code": 1110061,
   "msg": "Activation failure, hand holding card is not plastic card"
}
```
