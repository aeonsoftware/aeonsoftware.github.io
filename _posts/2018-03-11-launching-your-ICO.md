---
layout: post
title: Launching Your 1st ICO on Stellar
---

# Introduction:
This guide assumes that you have some basic blockchain knowledge and it’s only purpose is to help one create their ICO
   
   2. An Initial Coin Offering, also commonly referred to as an ICO, is a fundraising mechanism in which new projects sell their underlying crypto tokens in exchange for bitcoin and ether. It's somewhat similar to an Initial Public Offering (IPO) in which investors purchase shares of a company.

   3. Most ICO’s are usually launched on Etherium but of late a number of companies have started launching their ICO’s on the stellar network.

   4. Stellar is being used these days because : 
      1. Performance
      2. Liquidity
      3. Security
      4. Easy of use
- Code used can be found at the following github repo [https://github.com/aeonsoftware/xdrDecoder](https://github.com/aeonsoftware/xdrDecoder)


## 2. Building the ICO
The goal of this guide is to develop the ICO using only http requests(2) to the stellar network.But as has been realised, there are complex processes that have to take place even before the actual GET or POST request is made.  Thus various libraries are utilized to formulate the requests as will be showed in the guide below.

* STEP 1 : Go to the stellar account viewer and click generate to create a pair of stellar account keys(2 sets).This can be done programmatically but is easier generated on the site. Save the keys you generate on a text file and keep the secret keys safe as they’re the lock to your stellar account on the network.Don't share the private key. If you need to send funds to your account, share the public key. That is where your funds will be sent to.
* One of the generated accounts will be your Issuing account while the other will be your Distributing account.
* My accounts are the following:

  - Issuing Account
    - Public Key:
      - GCD5CETREL3OKZY72CTVU5WJHTKT7P73LK7JDM2YWXA57KXS7BKKXQPO
    - Secret Key:
      - SBMNVXX7RACH26KYEJABZJD4K6VTGTJ7BKVDBR5GJJ4QX3IHWMVLF6OA
  - Distributing Account
    - Public Key:
      - GAB4UTCY7AS2BUOOTUUHH5CUEQHPUVHO7UN23GFKIP5CX2ASS7LPQXTW
    - Secret Key:
      - SC6PUGSLTRVEALABHV3SYJBD5J6RM5II6I6R33Z3MKNGSR6K25FKKLIJ

* STEP 2 : For a stellar account either on the testnet(test network) or the pubnet(public network) to be activated, it needs to be funded. The official stellar documentation for starting your own ICO suggests 31 lumens. So go ahead and fund each of the two accounts you generated with 31 XLM.Since we’re launching our ICO on the test network, use the stellar testnet faucet tool to fund both your accounts with lumens. (The process for launching your ICO on the public network is exactly the same.All you’ll have to do is to substitute test for public during the process on sections I instruct you to do so.)

* STEP 3 :  For us to issue a token on stellar, we need to issue a change trust transaction between the distribution account and the issuance account. This is done through a POST transaction to the stellar network. [https://www.stellar.org/developers/horizon/reference/endpoints/transactions-create.html](https://www.stellar.org/developers/horizon/reference/endpoints/transactions-create.html)
  - Request : POST https://horizon-testnet.stellar.org/transactions
  - Arguments
    - Name : tx
    - Value : body e.g.

    ```
        AAAAAE+uxxcBBqfmiGC7VIVNxGlF46HSz9bU3objl51dU8ocAAAINABsGWQAAI34AAAAAAAAAAAAAAAVAAAAAAAAAAMAAAABTFRDAAAAAAA/8PjK6rZT6wnyO1T+ry5U8MKX0hAtMTgBIwmZKOVc9QAAAAFCVEMAAAAAAD/w+MrqtlPrCfI7VP6vLlTwwpfSEC0xOAEjCZko5Vz1AAAAAJ9M9XwAAY1bAExLQAAAAAAAAWNsAAAAAAAAAAMAAAABTFRDAAAAAAA/8PjK6rZT6wnyO1T+ry5U8MKX0hAtMTgBIwmZKOVc9QAAAAFCVEMAAAAAAD/w+MrqtlPrCfI7VP6vLlTwwpfSEC0xOAEjCZko5Vz1AAAAABqM05QAMTgBIwmZKOVc9QAAAAFCVEMAAAAAAD/w+MrqtlPrCfI7VP6v …………………………………………………………………………..
    ```

  - Most other usual APIs use JSON as the body for their transactions but we cant do that with stellar. The transaction body is a Stellar XDR blob.[https://www.stellar.org/laboratory/#xdr-viewer](https://www.stellar.org/laboratory/#xdr-viewer)
  - To generate the XDR blob(ie. The body shown above), we’ll use the stellar-base library on nodejs. 

    ```javascript
    var StellarBase = require('stellar-base');
    StellarBase.Network.useTestNetwork();
    var rp = require('request-promise');

    //DISTRIBUTING ACCOUNT
    var pubKey1 = "GAXZBCFL4RXPJJVU73TGKC5U5S6DFWI2SRZ6QIXYDRA64FJMOHE7OGGN"
    //ISSUING ACCOUNT
    var pubKey2 = "GCU5RXP4QR2CD562DYKZARNQGVCUMMMD6DPC2RGDTZELQOYVYA3RX2WA"

    rp('https://horizon-testnet.stellar.org/accounts/'+pubKey1).then(function ( response) {
    result = JSON.parse(response)
    var account=new StellarBase.Account(pubKey1,result.sequence);
    var sourceSecretKey = 'SDZRPU7VDI6AHGA4VNS65SQ3XCVOCHIULU66RMJR4L4TWDBSTZKH3PVY';
    
    // Derive Keypair object and public key (that starts with a G) from the secret
    var sourceKeypair = StellarBase.Keypair.fromSecret(sourceSecretKey);
    var sourcePublicKey = sourceKeypair.publicKey();
    
    
    
    
    var transaction = new StellarBase.TransactionBuilder(account)
            // add a set options operation to the transaction
            .addOperation(StellarBase.Operation.changeTrust({
                asset : new StellarBase.Asset("BRIAN","GCU5RXP4QR2CD562DYKZARNQGVCUMMMD6DPC2RGDTZELQOYVYA3RX2WA")
            }))
                    // add a payment operation to the transaction
                    .addOperation(StellarBase.Operation.payment({
                    destination: "GAXZBCFL4RXPJJVU73TGKC5U5S6DFWI2SRZ6QIXYDRA64FJMOHE7OGGN",
                    asset: new StellarBase.Asset("BRIAN","GCU5RXP4QR2CD562DYKZARNQGVCUMMMD6DPC2RGDTZELQOYVYA3RX2WA"),
                    amount: "100.50"  // 100.50 XLM
                }))
            .build();
    transaction.sign(sourceKeypair);
    console.log(transaction.toEnvelope().toXDR('base64'));
    
    
    toString(result.sequence + 1)

    })

    ```
    - `var StellarBase = require('stellar-base');` : Import the stellar-base library(contains methods we’ll use to combine our operations and convert them the XDR format ie.the XDR transaction blob that we’ll POST to the stellar network.)
    - `StellarBase.Network.useTestNetwork();` : We’re declaring that we’ll use the test network and NOT the public network. If you want to setup your ICO on the public network, just use usePublicNetwork instead.
    - `var rp = require('request-promise');` : Import the request promise library. We’ll see where we’ll use this later.
    - `var pubKey1 = "GAXZBCFL4RXPJJVU73TGKC5U5S6DFWI2SRZ6QIXYDRA64FJMOHE7OGGN"` : Setup the pubkey1 that’s the distributing account’s public key
    - `var pubKey2 = "GCU5RXP4QR2CD562DYKZARNQGVCUMMMD6DPC2RGDTZELQOYVYA3RX2WA"` : Setup the pubkey2 that’s the issuing account’s public key
    - The Goal in writing this code is to generate our transaction. The post request’s body is not a JSON string. Rather an encoded transaction in XDR format. A transaction is made up of many operations. Our first operation is to change trust between the issuing account and distributing account. We do this through the lines of code 10 - 48.	A detailed explanation as to what this code does can be found on the official stellar JS SDK documentation [https://www.stellar.org/developers/js-stellar-base/reference/building-transactions.html](https://www.stellar.org/developers/js-stellar-base/reference/building-transactions.html)
    - The code in the photo above generates the XDR transaction blob that we’ll POST to the stellar network 
      - Request : POST https://horizon-testnet.stellar.org/transactions
      - Arguments
        - Name : tx
        - Value : body e.g.

        ```
            AAAAAE+uxxcBBqfmiGC7VIVNxGlF46HSz9bU3objl51dU8ocAAAINABsGWQAAI34AAAAAAAAAAAAAAAVAAAAAAAAAAMAAAABTFRDAAAAAAA/8PjK6rZT6wnyO1T+ry5U8MKX0hAtMTgBIwmZKOVc9QAAAAFCVEMAAAAAAD/w+MrqtlPrCfI7VP6vLlTwwpfSEC0xOAEjCZko5Vz1AAAAAJ9M9XwAAY1bAExLQAAAAAAAAWNsAAAAAAAAAAMAAAABTFRDAAAAAAA/8PjK6rZT6wnyO1T+ry5U8MKX0hAtMTgBIwmZKOVc9QAAAAFCVEMAAAAAAD/w+MrqtlPrCfI7VP6vLlTwwpfSEC0xOAEjCZko5Vz1AAAAABqM05QAMTgBIwmZKOVc9QAAAAFCVEMAAAAAAD/w+MrqtlPrCfI7VP6v …………………………………………………………………………..
        ```
- STEP 4 : Creating the tokens.
  - After previous step, the distribution account trusts the issuing account. Now, tokens can be created.
  - This step is not intuitive: the token creation is done by sending a payment from the issuing account to the distribution account, denominated in the new token. This is why we had to change trust to begin with — the distribution account issued a statement of trust that this “BRIAN” thing was the real deal.
  
    ```javascript
    rp('https://horizon-testnet.stellar.org/accounts/'+pubKey2).then(function ( response) {
    result = JSON.parse(response)
        // ISSUING ACCOUNT
        var account2=new StellarBase.Account("GCU5RXP4QR2CD562DYKZARNQGVCUMMMD6DPC2RGDTZELQOYVYA3RX2WA",result.sequence);
        // The source account is the account we will be signing and sending from.
        var sourceSecretKey2 = 'SAJCVYY4A5T3ZFSGCOTRLCXHE7J4YKRJ5WRP3BYWPGB433FE7MIU26XO';
        var sourceKeypair2 = StellarBase.Keypair.fromSecret(sourceSecretKey2);
        var sourcePublicKey2 = sourceKeypair2.publicKey();
    
    var transaction2 = new StellarBase.TransactionBuilder(account2)
    // add a set options operation to the transaction
        // add a payment operation to the transaction
        .addOperation(StellarBase.Operation.payment({
            destination: "GAXZBCFL4RXPJJVU73TGKC5U5S6DFWI2SRZ6QIXYDRA64FJMOHE7OGGN",
            asset: new StellarBase.Asset("BRIAN","GCU5RXP4QR2CD562DYKZARNQGVCUMMMD6DPC2RGDTZELQOYVYA3RX2WA"),
            amount: "100.50"  // 100.50 XLM
        }))
    .build();
    transaction2.sign(sourceKeypair2);
    console.log(transaction2.toEnvelope().toXDR('base64'));
    })
    ```

  - The code in the photo above generates the XDR transaction blob that we’ll POST to the stellar network 
    - Request : POST https://horizon-testnet.stellar.org/transactions
    - Arguments
      - Name : tx
      - Value : body e.g.

    ```
        AAAAAKnY3fyEdCH32h4VkEWwNUVGMYPw3i1Ew55IuDsVwDcbAAAAZAB3bpsAAAABAAAAAAAAAAAAAAABAAAAAAAAAAEAAAAAL5CIq+Ru9Ka0/uZlC7TsvDLZGpRz6CL4HEHuFSxxyfcAAAACQlJJQU4AAAAAAAAAAAAAAKnY3fyEdCH32h4VkEWwNUVGMYPw3i1Ew55IuDsVwDcbAAAAADvnFUAAAAAAAAAAARXANxsAAABAU+SCKngKCrryIMiQ…………………………………………………………………………..
    ```
- Once the steps above are done, the tokens have been created and your ICO is online. To see the token exists on the testnet, here is a link to the Distribution account:  [https://horizon-testnet.stellar.org/accounts/GAB4UTCY7AS2BUOOTUUHH5CUEQHPUVHO7UN23GFKIP5CX2ASS7LPQXTW](https://horizon-testnet.stellar.org/accounts/GAB4UTCY7AS2BUOOTUUHH5CUEQHPUVHO7UN23GFKIP5CX2ASS7LPQXTW)