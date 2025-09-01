

# FAQ

We have collected some FAQs from partners. Maybe there are some questions that may prove to be useful to you.

#### General Questions

- How to test API?

**A:** Please provider your IPs to be whitelisted on our side, then you access [https://customer-sandbox.paycrypto.com/](https://customer-sandbox.paycrypto.com/). After you create account successful and create API key and secret in setting, then try example in [https://github.com/pay-crypto001/railone-sdk-java](https://github.com/pay-crypto001/paycrypto-sdk-java).

- What are the required steps to get a card issued from scratch? 

**A:** KYC -> KYC Approved -> Apply for bank card -> Activate bank card

Open API access demonstration： [https://github.com/pay-crypto001/paycrypto-sdk-java](https://github.com/pay-crypto001/paycrypto-sdk-java)

- Which KYC Level is required to issue a card? What is the time schedule? How long it can take you to verify documents?

**A:** The only document supported currently is Passport. Only "front_doc" is needed in this version, "back_doc" and "mix_doc" can be left empty. Document verification roughly requires 24 hours.

- After successful verification of KYC documents we order a card for the use,How long it can take you to send the card? And can you notify us somehow that card has been sent?

**A:** After KYC is successful, card is issued in 48 hours. User receives and activates the card in your app.


 
- Do the deposits corresponds to the transactions performed with the card? Or are those just the USDT deposits that are added to the fiat balance available to spend?
if not, how can we get the payments been made by the user with the card?

**A:**  When you call the deposit API, we will transfer USD to your user card, and it is available to spend.
 
- Do we really have to verify the email and phone number of the user? we already do that for our own on-boarding
  
**A:**  Mobile phone number verification is not required. Email verification is optional. If you've done validation, you don't need to do it again.
 
- Is it normal that the balances come out as null?

**A:** Balances should not be null. Bank API example: https://github.com/pay-crypto001/paycrypto-sdk-java/blob/master/src/test/java/com/paycrypto/open/api/test/BankTest.java
 
- the "API Doc" in the portal shows an empty page

**A:** If an empty page is displayed, please refer to the same documentation on GitHub:  https://github.com/pay-crypto001/api-docs/tree/master/
 
- another question is how do we reconcile the "Deposits" that we perform in USDT?

**A:**  "Reserve Deposit Address" is your USDT address. It should have enough balance before you carry out "Deposits" for your user .
 
- I dont see any API to get list of transactions made on the card?

**A:**   This is the method that can be used to fetch the list of transactions: "5.3 Check transaction records"
 
```
 url：/api/v1/bank/transaction-record
 method：POST
```

#### API to submit user's KYC data
```
url：/api/v1/customers/accounts
method：POST
```

- The parameter “acct_no” - Is a unique user’s ID on our side?   

**A:** Yes, it is an ID on your side.

- What is the meaning of the parameter “acct_name”? Which value should we submit in that parameter? 

**A:** It is a user name on your side, depends on you.

- The parameters “real_name” - Is the user's full name?  

**A:** There is no such parameter

- What is the format of the parameter “birthday”?  

**A:** "1990-01-01" or **YYYY-MM-DD**

- According to the API documentation the parameter “doc_type” can be “Passport”, “National ID card”. Which values should we send for Residency Verification (level 2)? 

**A:** The only document supported currently is Passport. Only "front_doc" is needed in this version, "back_doc" and "mix_doc" can be left empty.

- What is the meaning of the parameter "mix_doc"? What is required for?  

**A:** A photo captured with the certificate in hand, can be left empty.

- What is “area_code”? Is it a mandatory parameter? And where can we find it?  

**A:** The parameter has been updated to "country_code".

- Which information must contain the parameter “address”, e.g country, postal code, city, address? 

**A:** The detailed the better. The issued bank card will be sent to this address.

- What is “kyc_info”, “mail_verification_code” and “mobile_verification_code”? Which information should we post in these parameters?

**A:** “kyc_info” is optional parameter, for other information. “mobile_verification_code” is filled in by the user. For details on how to send the verification code to user's email please refer to "4.1. Sending Email verification code" in API docs.

- How can we use POST /api/v1/customers/accounts ? Which parameters we are required to submit for Level 1 KYC and for Level 2 KYC? And can we submit Level 1 KYC documents first, then order a card for the user and then after some period of time submit Level 2 KYC documents?

**A:** The KYC process should be carries out in one request. Here's an example: https://github.com/pay-crypto001/paycrypto--sdk-java/blob/master/src/test/java/com/paycrypto/open/api/test/KycTest.java#L31



#### Submitting user's card information
```
url：/api/v1/debit-cards
method：POST
```
This API method allows us to order a card for users.

- There is only one input parameter “acct_no”. How can we select the required payment system: VISA or UnionPay?

**A:** You pay in USDT, how the user is paid depends on you.

- A response contains the parameter “card_no”. Is it a masked or full PAN?

**A:** To prevent real card information from being exposed, the 'card_no' parameter is an assigned card ID and not the card number. We return 'card_no' and 'card_number'.

- How to specify a shipping address for the card?

**A:** The address specified for the KYC is used as the shipping address.

- Which delivery options are available: Express, Standard, etc? How can we select the required option?

**A:** Please clarify this with our business team

#### User activating bank card
```
url：/api/v1/debit-cards/status
method：PUT
```
- Will we receive card activation result synchronously or activation can take some time and we need to additionally query card activation result using the API GET /api/v1/debit-cards?acct_no={acct_no} ?

**A:**  After the user receives card and activates it, the card number input in your website can be used to call this API "2.2. User activating bank card", and we will return a response wheter the user activation successful.

```text
url：/api/v1/debit-cards/status
method：PUT
```

- User triggers a card withdrawal password reset Email
```
url：/api/v1/debit-cards/deposit-pwd-emails?acct_no={acct_no}
method：POST
```
What is the purpose of a withdrawal password? Is it a 3ds password to be used for payment authorizations? 

**A:** It is to allow the user to reset the password when forgotten.


#### User deposit
```
url：/api/v1/deposit-transactions
method：POST
```
Card deposit process is not clear for us at the moment. As we were informed, each card will have a cryptocurrency address (e.g. BTC, USDT or ETH address), and to top up the card user must send crypto currency amount to that address. How can we use the API POST /api/v1/deposit-transactions in such flow?

How can we retrieve cryptocurrency addresses, assigned to the card?

When a user sends cryptocurrency to address provided, Your partners receive a cryptocurrency amount, convert it using a rate from coinmarketcap.com, and then send fiat amount to the user’s card. When can we know the finally converted fiat amount, which will be credited to the user’s card: when a cryptocurrency transaction has been registered in the network or when transaction has gained a certain amount of confirmations?

Can we somehow calculate fiat amount, which user will receive on his card, at the moment when user sends cryptocurrency amount?

**A:** Here is how we have we have achieved this. We allocate a cryptocurrency address (e.g. BTC, USDT) for you, "User deposit" is paid with your address. We then calculate the fiat amount and transfer fiat to user card, the query transaction details API is "GET /api/v1/deposit-transactions?acct_no={acct_no}".

- Query a deposit transaction status
```
url：/api/v1/deposit-transactions/{tx_id}/status
method：GET
```
{tx_id} - is a transaction ID on our side (can be value passed in the parameter “cust_tx_id” in the User deposit request POST /api/v1/deposit-transactions) ?

**A:** {tx_id} is not the same as “cust_tx_id”, {tx_id} is in the system when you carry out a deposit.

- Query all the deposit records
```
url：/api/v1/deposit-transactions
method：GET
```
Can we query all deposit records for all our cards using this API method?

**A:** Yes. 

#### Query a particular user's deposit records
```
 url：/api/v1/deposit-transactions?acct_no={acct_no}
method：GET
```
How can we query card’s withdrawal transactions (POS, ATM, etc)? We need them to be displayed in the user's account, in transactions statement.

Can you send callbacks to our server on cards’ withdrawal transactions (POS, ATM, etc)?

Will users receive email / sms notifications about card’s withdrawal transactions (POS, ATM, etc)?

**A:**  Withdrawal transactions can be fetched using "5.3 Check transaction records " API, notifications will be supported in future versions.

```text
url：/api/v1/bank/transaction-record
method：POST
```
 
#### Public API
```
POST /api/v1/emails/{email}/verification-codes
POST /api/v1/mobiles/{mobile}/verification-codes
PUT /api/v1/emails/{email}/verification-codes?code={code}&verification_id={verification_id}
PUT /api/v1/mobiles/{mobile}/verification-codes?code={code}&verification_id={verification_id}
```
Are SMS and Email verification required for our users? If yes, when should we query them, before submitting user's KYC data and card ordering or after?  

**A:** Mobile phone verification is not required. Email verification is optional. If you've carried out validation, you don't need to do it again. Please refer to "4.1. Sending Email verification code" in API doc for details on how to send the code to user's email.

#### Card’s actual status retrieving. Card blocking / unblocking.

- In the document I can’t find API for card blocking / unblocking and actual card status retrieving. Do you have them? We need to display actual card status in our account and allow users to block/unblock their cards if required. Also our support operators can block/unblock cards by users requests 

**A:** We will support card  blocking / unblocking，but currently have not provided the API for the same in this version. Please ask our business team for details.

#### API to retrieve balance on a card

- In the document I can’t find API for card balance retrieving. Do you have such an API? We need to display actual card balance in the user’s  account

**A:** Please refer to "5.2 Query account balance" API.

- Do you have a test API environment? Can you please provide API URL and credentials?

**A:** Please clarify with our business team.

- How the different KYC parameters between USD and EUR card?

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
