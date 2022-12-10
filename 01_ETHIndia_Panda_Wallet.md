ETHIndia Summary - Panda Wallet
===

## Table of Contents

[TOC]

## Introduction

Hello everyone! This blog is to summarise my experience at ETHIndia where the primary focus was on building a 4337 wallet with great UX. Also to note down the points that came up while talking to other hackers and web3 experts and importantly amateurs. 

We were able to implement some parts of the social recovery mechanism during the hackathon. There were plans for implementing multi-device login although they were dropped as we hit some major roadblocks while developing the smart contracts and implementing soical login. I am grateful to the Panda Wallet team who made it possible to demo the project in mere 18 hours of build time.

You can take a look at the final project here - [Panda Wallet](https://devfolio.co/projects/panda-wallet-698f).
*Note - We were unable to record the demo of recovery. Updates on this would be posted after ETHIndia prize disbursal is completed.*

## Who's it for?

This is a questions I have been going back and forth. The answer comes out to be (I am paraphrasing) "any user who want's to use crypto easily". 

This poses a few issues while designing the UX. For 'regular and informed user' technical terms in the app should not be an issue. Where as for a 'new to the crypto world' user terms like paymasters, guardians and even allowances seem to be overwhelming.

Abstracting these terms to the crypto-native user creates technical confusion and not abstracting them adds to mental overhead for the crypto non-native user.

A possible solution to this issue is introducing `Lite Mode` and `Pro Mode`. in the application and let the users select how they want to interact with the application. That way we can also have some more flexibility in designing the UX.


## Core Features

### Social Recovery

We were successful in implementing the social recovery for the wallets. 


### Multi Device Sign-in

We went back and forth on this topic a lot of times. Initially the plan was to create an `initial device key` that would be used to deploy the wallet using `CREATE2`. Although if this device key is lost, we run into managing different `initial device keys` for a user. 

Here we started to look into MPC based social logins that would generate the same `social-login key` on every new device given that the user has access to their socials and 2FA providers. Then we can get rid of keeping the track of different `initial device keys` and instead generate new wallets for the user using the pubkey of `social-login key`. 

We started to work on this, but later we ran into issues in getting a private key out of the `Biconomy Social Login SDK`. The documentation did not mention that the `private key` or `signer` is not accessible outside the SDK. This set us back many hours of progress.


## Some More Features

### Wallet Names
Wallet names was a thought which came up frequently to abstract away the long ethereum addresses. ENS is a possible path we can work with but the issue with cross chain resolution of ENS is always a caveat. Resolving the ENS on the client and sending the address in the transaction data would seem to be okay for general use cases. 

> "L2 support is still a work in progress." 
> *[ENS Docs](https://docs.ens.domains/dapp-developer-guide/ens-l2-offchain)*

### Spending Policies

Before the hackathon we were looking into how to implement ERC20 spending policies for SCW's. The issue with this is that the ERC20 data mutations happen in the context of that `ERC20 contract` and not within the `WalletContract`

After a certain hour of headscratching, we planned to deconstruct the calldata bytecode inside the `WalletContract` and lookup the function selector in an `if-else ladder` and check if the parsed values don't exceed the 24 hour limits.
