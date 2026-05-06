

# FAQ



#### 对接流程？怎么使用 API?

API 使用步骤：

1. 请在 [https://caas.native.financial/](https://caas.native.financial/) 注册机构账户，如果访问不了请提供你的IP地址。
2. 我们审核通过后，机构才可以登录成功。
3. 机构登录，查看钱包地址，给钱包充值，支持 USDT、USDC等。
4. 机构登录，创建 Appkey 和 secret，可选择配置 webhook 回调地址。
5. 调用 API 进行 KYC、开卡、激活卡、充值等操作，状态变更我们会通过回调地址通知机构服务器。

机构 Dashboard :

* 测试环境（有IP白名单限制）: https://caas-sandbox.native.financial/
* 生产环境（有IP白名单限制）: https://caas.native.financial/

API 域名：

* 测试环境: https://api-sandbox.paycrypto.com/ , https://api-sandbox.native.financial/
* 生产环境: https://api.paycrypto.com/ , https://api.native.financial/

代码请参考：[https://github.com/pay-crypto001/paycrypto-sdk-java](https://github.com/pay-crypto001/paycrypto-sdk-java)


#### 如何给机构账户充值？资金流程？

机构 Dashboard :

* 测试环境（有IP白名单限制）: https://caas-sandbox.native.financial/
* 生产环境（有IP白名单限制）: https://caas.native.financial/

查看地址，给地址转账后会自动到账，请先试小额打款，再大额。


![](./imgs/caas/workflow1.jpg)

每家机构均设有一个钱包资金账户。为客户卡片进行充值时，相应金额将从该资金账户余额中扣除。

办卡手续费将从办卡资金池中扣除，卡片充值金额将从卡片充值资金池中扣除。目前仅我方可为您在两个资金池之间进行划转操作。

流程示例：

1. 当您的用户发起充值时，由您向用户收取资金并存入您的企业钱包。
2. 随后您调用我方接口执行充值操作，此时您的账户余额必须充足。
3. 请预先向您的钱包账户存入一定金额的资金，用于未来数日的业务使用。

![](./imgs/caas/workflow0.jpeg)
![](./imgs/caas/workflow-cn.png)


#### 发行卡片需要完成哪一级 KYC 认证？审核KYC时间多久？实体卡物流时间多久？

无 KYC：仅核验用户基础信息（姓名、电子邮箱、手机号码、地址），审核宽松，最快 3 分钟内完成。
标准 KYC：需提交基础信息及护照文件，审核时长不超过 3 分钟。
加强 KYC：需完成基础信息、护照上传、活体认证等多重核验，审核周期为 8 至 48 小时。

#### Native卡和UQ卡的限额？



| **Card Type** | | **Native Card - Virtual** | **Native Card - Physical** | **UQ - Virtual Card Option 1** | **UQ - Virtual Card Option 2** |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Apple Pay** | | ✅ Supported | ✅ Supported | ❌ Not Supported | ✅ Supported |
| **KYC** | Passport | ✅ Required | ✅ Required | ❌ Not Required | ✅ Required |
| | Liveness Check | ❌ Not Required<br>(but needs selfie holding passport) | ❌ Not Required<br>(but needs selfie holding passport) | ❌ Not Required | ✅ Required |
| **Alipay - For mainland China users** | | ❌ Not Supported | ❌ Not Supported | ✅ Compatible | ✅ Compatible |

Native card: 
- Support Applepay and Googlepay
- Transaction Limits
Same as virtual card default:
Minimum per transaction: 0.00
Maximum per transaction: USD 30,000.00
Daily maximum: USD 80,000.00
Monthly maximum: USD 2,000,000.00
Cumulative maximum: 50,000,000.00

- ATM Cash Withdrawal Limits
Single withdrawal limit: 2,000 USD
Daily limit: 10,000 USD
Cumulative limit: 50,000 USD
Withdrawal fee: 4%+$0.99


UQ card:
- Per Transaction Limit：$20000
- Daily Count: 100
- Daily Limit: $250000
- Monthly Limit: $1000000
- ATM Withdrawal
Per Transaction Limit：$1500Daily Limit：$1500 
Daily Count：6
Fee: 2%（min $1）
Monthly Limit：$15000

JDB card：
- ⁠Daily total cash withdrawal limit at ATM: $3,000
- withdraw fee: 0.75%
- Single cash withdrawal limit depends on local ATM limits
- ⁠Maximum transactions per day at ATM: 10
- ⁠No limit on bank consumption, POS, and online transactions，also per transaction no more than $25,000. 
Need bypass if you want to make large transactions.

Application fee : 103 usd. （Covers the first year's annual fee.）

#### 有没有那种消费接受高的网站的名单？

These popular services are all supported.

Netflix
Spotify 
YouTube 
DISNEY PLUS
Twitch
Amazon 
Google
Discord 
APPLE.COM
OPENAI *CHATGPT
ANTHROPIC CLAUDE
MIDJOURNEY
PERPLEXITY.AI 
CANVA
Notion
Figma
Adobe 
MICROSOFT
DROPBOX
ZOOM.COM
GITHUB
GRAB
CURSOR

#### 卡申请资金（Card Application Fund）与卡充值资金（Card Top Up Fund）账户有何区别？如何为申请卡资金账户充值？

卡申请手续费将从卡申请资金账户中扣除，卡片充值金额将从卡充值资金账户中扣除。充值后资金会到卡充值资金账户，目前仅我方可为您办理两个资金账户之间的划转操作。

1. what Difference Card Application Fund and Card Top Up Fund ?
2. how to funding card Application Fund ?

The card application fee shall be deducted from the Card Application Fund, and the card top-up amount shall be deducted from the Card Top-up Fund.
Currently, only we can perform the transfer between the two funds for you.

#### 如果我们已经验证过用户的KYC，还需要再提交吗?

无 KYC、标准 KYC的卡只需要提交凭证给我们（用这个参数 “kyc_info”），加强 KYC 的卡必须重新提交。

#### 参数 “acct_no” 是我们每个用户的唯一编号吗?   

是的


#### 不同的卡，包括虚拟卡和实体卡，申请流程一样吗？

流程是一样的，有的卡会有细微差别。申请实体卡的时候需要带上卡号，虚拟卡不需要。

 Physical and virtual cards use the same activation API, but the card application endpoint requires a card number parameter for physical cards.

#### 为什么Card_no 带星号？

若返回完整明文卡号（例如：17182986860745039429），表示卡片已成功激活。若返回脱敏卡号（例如：1718298686074503****），表示卡片尚未激活。一旦激活成功，末尾的 **** 会替换为卡号最后四位数字。

#### 怎么查卡敏感信息？

https://github.com/pay-crypto001/paycrypto-sdk-java/blob/main/src/test/java/com/paycrypto/open/api/test/BankTest.java#L90

#### 支持API冻结、解冻、注销卡吗？

冻结解冻支持，注销必须人工处理。

#### 还没有签合同，可以先注册Sandbox账户试用吗？

不行，但我们可以截图给你看我们产品

#### 美元卡和欧元卡的KYC参数区别?

EUR card "country,state,city,address" should fill europe country address.
```
{
    "acct_no":"0513000013",
    "acct_name":"HANS",
    "card_type_id":"60000002",
    "first_name":"SUNILKUMAR",
    "last_name":"HANSRAJ",
    "gender":"male",
    "birthday":"1996-11-01",
    "city":"Stockholm",
    "state":"AHMEDABAD,GUJARAT",
    "country":"SE",  //USD card example: Australia, EUR card example: SE
    "nationality":"SE",  //USD card example: Australian, EUR card example: SE
    "doc_no":"K1988247",
    "doc_type":"passport",
    "country_code":"86",
    "mobile":"+8615821703553", //USD card example: +8615821703553, EUR card example: +8615821703553
    "mail":"test@email.com",
    "address":"123",
    "zipcode":"100028",
    "maiden_name":"zhang xiaohong",
    "cust_tx_id":"202020420",
    "front_doc":"{base64 image}",   //USD card: size limit 2M, EUR card: size limit 1M
    "mix_doc":"{base64 image}"
}

```
