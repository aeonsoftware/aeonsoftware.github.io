---
layout: post
title: Launching Your 1st ICO on Stellar
---

# Introduction:
 - The steps below will be for a test account on the test network. However, if you'd like to do this on the public network, the steps are exactly the same. The only difference is that when using stellar.org's transaction, you'll change from test to public on the top right corner of the page. Also, details on how to buy stellar lumens and fund your accounts on the public network can be found through this link (https://aeonsoftware.github.io/2018/03/19/buying-your-first-lumens.html)
 - An Initial Coin Offering, also commonly referred to as an ICO, is a fundraising mechanism in which new projects sell their underlying crypto tokens in exchange for bitcoin and ether. It’s somewhat similar to an Initial Public Offering (IPO) in which investors purchase shares of a company.
 - Most ICO’s are usually launched on Ethereum but of late a number of companies have started launching their ICO’s on the stellar network.
 - This guide will take you through a detailed step by step process on how to create your first ICO on stellar.

## 1. CREATING THE ACCOUNTS

1. Go to the stellar dashboard and generate three accounts(https://www.stellar.org/laboratory/#account-creator?network=test) 
 - The first keys generated will be for your issuing account. 
 - The second keys generated will be for your distributing account.
  - The thrird keys generated will be for your investor's account.
 - For each account you generate, note down both the public and private keys on a note editor as you will need them.
 - Also make sure you fund both accounts using the tool below the generate keypair button by clicking on the "get test network lumens" button. An account must have funds on the stellar network for it to be active.

   1. issuing account
     - Public Key	GBT5EUAT3JXU6RILOGIRQNVWXWPH2B2FXPO5IZZ4HIT7MTAPD6AF45MF
     - Secret Key	SCRESYGR5YCWC4J6KKNC42GZDV67ZHZH4BYBGXLBWVBIOTGB5QV4APUJ
       ![key2](https://aeonsoftware.github.io/images/key2.png)
   2. Distributing account
     - Public Key	GDTFWW3O7YLS7LKQDKOKLTBHWYD6BVQWFLRKMJHKHZYYCXLYFI4CTS4A
     - Secret Key	SBRYIKF4JIOQ7AIDN53V35EGALWVL6Z75MV7TCXINVIAAQS7JERQQACV
       ![key1](https://aeonsoftware.github.io/images/key1.1.png)
   3. Investor account
     - Public Key	GAGVT2HLISMBQT2XJYBZFC54UTHDIXKJM3AFS6WUC7PCMFEIDQBNXX2C
     - Secret Key	SA4RYIYGTPUI75JMRIPIDZ62ER35WTVMZNQHATCMI3JFFT62OWBNEO22
       ![key3](https://aeonsoftware.github.io/images/key3.png)

## 2. TRUSTING THE ISSUING ACCOUNT
1. Head over to the stellar labs transaction builder. We'll use this to fill out a form that'll make our distributing account trust our issuing account(https://www.stellar.org/laboratory/#txbuilder?network=test).
2. Use the Distributing account's public key as the source account.
3. Click the "fetch sequence number .." to get the next sequence number for your account.
4. Leave the next few options blank and skip over to the operation type and select change trust.
![changetrust1](https://aeonsoftware.github.io/images/changetrust1.png)
5. Click either the 4 or 12 Alphanumeric, depending on how long you want your asset code to be and then set it to whatever you want it to be. I set mine to BRCO.
6. Set the Issuer account ID to our issuing account's public key.
![changetrust](https://aeonsoftware.github.io/images/changetrust.png)
6. Leave the trust limit blank and sign the transaction by clicking on the "Sign in transaction Signer" button.
![signtrusttransaction](https://aeonsoftware.github.io/images/signtrusttransaction.png)
8. Sign with your distributing account's secret key.
9. Click the Submit to post transaction endpoint button 
![signtrusttransaction2](https://aeonsoftware.github.io/images/signtrusttransaction2.png)
10. You should be redirecred to the page below. Click the submit button and you should be good.
![signtrusttransaction3](https://aeonsoftware.github.io/images/signtrusttransaction3.png)
11. If it succeeds, the json response should be in a normal grey color. Otherwise, if the whole step 2 doesn't succeed, the json response at the bottom will have a red outline.

## 3. CREATING THE TOKENS
1. Head over to the stellar labs transaction builder. We'll use this to fill out a form that'll issue tokens from our issuing account to the distributing account.(https://www.stellar.org/laboratory/#txbuilder?network=test).
2. Use the Issuing account's public key as the source account.
![step2.1](https://aeonsoftware.github.io/images/step2.1.png)
3. Click the "fetch sequence number .." to get the next sequence number for your account.
4. Leave the next few options blank and skip over to the operation type and select Payment.
![step2.2](https://aeonsoftware.github.io/images/step2.2.png)
5. Set the Distributor's public key as the destination
6. Set the asset you used in step 2 and set the issuing account's public key as the issuer account id.
7. Set the amount to any number eg. 15000000
8. Leave the Source account blank and click the Sign in Transaction Signer button.
9. Sign with your issuing account's secret key.
10. Click the Submit to post transaction endpoint button
![step2.3](https://aeonsoftware.github.io/images/step2.3.png)
11. You should be redirecred to the page below. Click the post button and you should be good.
![step2.4](https://aeonsoftware.github.io/images/step2.4.png)
12. If it succeeds, the json response should be in a normal grey color. Otherwise, if the whole step 2 doesn't succeed, the json response at the bottom will have a red outline.
13. To test whether the process succeeded, explore your Distributor account here and you should see the payment we just made (https://www.stellar.org/laboratory/#explorer?resource=accounts&endpoint=single&network=test). Submit your distribution Public Key as the account id.
![step2.5](https://aeonsoftware.github.io/images/step2.5.png)

## 4. Publishing information about your token
 - This step isn't necessarily important an thus can be skipped. Detailed information on how to do it can be found on here(https://www.stellar.org/blog/tokens-on-stellar/)

## 5. LIMITING THE SUPPLY
 - We don't wish to limit the supply of our coin but in-case you do, here's a link to how you can achieve this
 (https://www.stellar.org/blog/tokens-on-stellar/)(https://goolge.io/post/launching_stellar_ico/)

## 6. DISTRIBUTING YOUR TOKEN
 - The final step is to get the token into the people's hands
1. Head over to the stellar labs transaction builder. We'll use this to fill out a form that'll distribute from our distributing account to the anyone in the general public.(https://www.stellar.org/laboratory/#txbuilder?network=test).
2. Use the Distributing account's public key as the source account.
![step3.1](https://aeonsoftware.github.io/images/step3.1.png)
3. Click the "fetch sequence number .." to get the next sequence number for your account.
4. Leave the next few options blank and skip over to the operation type and select Manage Offer.
5. In the selling section , Set the asset you used in step 2 and set the distributing account's public key as the issuer account id.
6. Since we want people to buy our tokens using XLM, set native under buying
7. Set the amount you are willing to sell eg. 10000
8. We will be selling 1 BRCO for 1 XLM so set the Price of 1 unit of asset for sale to 1.
9. Set the offer id to 0.
![step3.2](https://aeonsoftware.github.io/images/step3.2.real.png)
10. Leave the Source account blank and click the Sign in Transaction Signer button.
11. Sign with your distributing account's secret key.
![step3.3](https://aeonsoftware.github.io/images/step3.3.png)
12. You should be redirected as in previous steps. Click the post button and you should be good.
13. If it succeeds, the json response should be in a normal grey color. Otherwise, if the whole step 2 doesn't succeed, the json response at the bottom will have a red outline.

## 6. BUYING THE TOKEN
 - Anyone willing to buy the token has to trust the issuer first.
1. Add your transaction to the trustline buy  heading over to the stellar labs transaction builder.(https://www.stellar.org/laboratory/#txbuilder?network=test)
2. Use the Investor account's public key as the source account.
3. Click the "fetch sequence number .." to get the next sequence number for your account.
![step4.1](https://aeonsoftware.github.io/images/step4.1.png)
4. Leave the next few options blank and skip over to the operation type and select change trust.
5. Set the asset you used in step 2 and set the issuing account's public key as the issuer account id.
![step4.2](https://aeonsoftware.github.io/images/step4.2.png)
10. Leave the Source account blank and click the Sign in Transaction Signer button.
![step4.3](https://aeonsoftware.github.io/images/step4.3.png)
11. Sign with your Investor account's secret key anc then click the button "Submit to post transaction endpoint".
![step4.4](https://aeonsoftware.github.io/images/step4.4.png)
![step4.4](https://aeonsoftware.github.io/images/step4.4.real.png)
12. You should be redirecred to the page below. Click the submit button and you should be good.
![step4.5](https://aeonsoftware.github.io/images/step4.5.png)
13. If it succeeds, the json response should be in a normal grey color. Otherwise, if the whole step 2 doesn't succeed, the json response at the bottom will have a red outline.
 - Now to buy the actual tokens: 
14. Head over to the stellar labs transaction builder. We'll use this to fill out a form that'll buy tokens the distributing account to the distributing account.(https://www.stellar.org/laboratory/#txbuilder?network=test).
15. Use the Investor account's public key as the source account.
16. Click the "fetch sequence number .." to get the next sequence number for your account.
![step5.1](https://aeonsoftware.github.io/images/step5.1.png)
17. Leave the next few options blank and skip over to the operation type and select Manage Offer.
18. In the selling section , Set the native.
19. Since we want to buy the BRCO,select alphanumeric 4 and set your token ie. BRCO.
20. We will be buying 1 BRCO for 1 XLM so set the Price of 1 unit of asset for sale to 1 and set the  issuing account's Public Key as Issuer Account ID
21. Set the amount you are selling to 100 then Set the offer id to 0.
![step5.2](https://aeonsoftware.github.io/images/step5.2.png)
22. Leave the Source account blank and click the Sign in Transaction Signer button.
![step5.3](https://aeonsoftware.github.io/images/step5.3.png)
23. Sign with your Investor account's secret key and submit to post transaction endpoint.
![step5.5](https://aeonsoftware.github.io/images/step5.5.png)
![step5.6](https://aeonsoftware.github.io/images/step5.6.png)
24. To test whether the process succeeded,and the investor was able to buy the new tokens, explore your investor's account here and you should see the offer we just made (https://www.stellar.org/laboratory/#explorer?resource=accounts&endpoint=single&network=test). Submit your investor Public Key as the account id.
![step5.8](https://aeonsoftware.github.io/images/step5.8.png)


#### 7. Additional Information & References
 - (https://www.stellar.org/developers/guides/walkthroughs/custom-assets.html)
 - (https://goolge.io/post/launching_stellar_ico/)
 - (https://www.stellar.org/blog/tokens-on-stellar/)
 - (https://medium.com/@ashisherc/create-an-ico-on-stellar-network-with-custom-token-7b6aab349f33)
 - For a more detailed article into how you can implement the steps below without the form and using POST and GET requests, head over to my other article where I've tried to explain each step in detail

#### 8. FAQs
1. Why does stellar require two seperate accounts 
2. How to check the trust line 
3. Why do I need to be connected to the internet
4. You can limit the trust line 
5. How I bought my first lumens without getting up from my chair - 

