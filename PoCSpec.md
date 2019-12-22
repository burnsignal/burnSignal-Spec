# Burn Signal PoC Web App Specification
## Contents

1. [Overview](https://github.com/burnSignal/burnSignal-Spec/blob/master/PoCSpec.md#overview)
1. [Routes](https://github.com/burnSignal/burnSignal-Spec/blob/master/PoCSpec.md#routes)
1. [SubGraph](https://github.com/burnSignal/burnSignal-Spec/blob/master/PoCSpec.md#subgraph)
1. [BrightID](https://github.com/burnSignal/burnSignal-Spec/blob/master/PoCSpec.md#brightid)
1. [Elements](https://github.com/burnSignal/burnSignal-Spec/blob/master/PoCSpec.md#elements)

## Overview
This document specifies the Burn Signal PoC web application, referred to herein as "the dapp".
Where this specification is insufficient or vague, the [figma prototype](https://www.figma.com/file/klSsgAu3ZvWKE8Xap80TjK/PoC?node-id=1%3A2) should be used as a reference. Where any ambiguity remains, @auryn-macmillan should be consulted for clarity.

Wherever possible, and ideally without compromise, the application should run client-side with data hosted on public and trustless (trust minimized) sources. The current state of the application should be permissionlessly reconstructable by any user whether they are running the application locally or using our hosted version.

## Routes
The following routes exist in the dapp, all others should 404 gracefully and redirect users to `/`

`/` --> `/poll/{pollAddress}`

`/poll/{pollAddress}` --> Poll page for the featured poll {pollAddress} is the ethereum address of the featured poll.

`/poll/{pollAddress}/{option}` --> voting modal for the poll.

`/about` --> a modal with some basic information on Burn Signal.

`/login` --> the web3connect login modal.

## SubGraph
### Mappings
For the PoC, we should modify our [original subgraph](https://github.com/burnSignal/HermesSubGraph-OLD) so that most computation is handled by the subraph's mapping, rather than client-side.

Currently, our mappings allow us to query the polls that have been created and the deposits sent.

We should add functionality to allow us to query for the:
- Total WEI burned in a poll
- Total WEI burned for an option in a poll
- Total WEI burned by an address for an option in a poll
- Details on a specific poll
- Details on each vote cast in a specific poll
- Votes cast by a user

### Subscriptions
We should also create subscriptions for the most of our mappings so that vote data can be updated in real-time on the dapp.


## Brightid
BrightID will be used to verify votes.
For each address that has voted, we should call `http://node.brightid.org/brightid/verification/ethereum/{address}`.
You can check out the documentation for the BrightID API [here](https://app.swaggerhub.com/apis/brightid/brightid/1.0.0#/default/get_verification__context___id_).


If the address is verified, the response will look like this:
```
{
  "data": {
    "timestamp": 0
  }
}
```

An if no verification is found, the response will look like this:

```
{
  "error": true,
  "errorNum": 404,
  "errorMessage": "Verification not found",
  "code": 404
}
```

## Elements
### Nav Bar
The nav bar contains two elements, the Burn Signal logo (which should be a hyperlink to `/`) and a user avatar with a dropdown menu.

The dropdown menu items are:
- about --> `/about`
- blog --> `https://blog.burnsignal.io`
- login | logout --> `/login` | log the user out.

### Poll Details
We should pull the data for this section from our subgraph.

Title = name
Description = data

"Yes" button --> `/poll/{pollAddress}/yes`
"No" button --> `/poll/{pollAddress}/no`
If the user is not authenticated, then the modal will display a QR code and a warning to only send funds from a BrightID verified address. If the user is authenticated, we should check whether their address is verified. If yes, a voting modal should be opened. If no, then we should show a modal with instructions for linking BrightID.

### Bar Chart
Bar chart should show relative percentage of votes that each option has received, weighted quadratically.

It should also include additional details such as the number of voters and the total amount of ETH burned.

### Line Chart
The line chart should show the change in relative vote percentage over time.

### Login Modal
We should authenticate users with [wex3connect](https://github.com/web3connect/web3connect), this will allow us to support injected providers(like Metamask, Dapper, Gnosis Safe, Web3 Browsers, etc), WalletConnect, and other providers like Portis, Fortmatic, Squarelink, Torus, Authereum and Arkane.

We can use the [React Button](https://github.com/web3connect/web3connect#React-Button) provided in the web3connect readme. Here is [an example](https://web3connect.com/) of it in use.

### Vote Modal
This modal is triggered when the user clicks the "yes" or "no" option on the poll. It should appear differently depending on whether or not the user is authenticated and whether or not an authenticated user has verified

#### Unauthenticated
When the user is unauthenticated, the login modal should show an ENS domain ({yes|no}.progpow.burnsignal.eth) that resolves to, and a QR code for, the receiving address of the selected option. Along with a link to the [Bright Ethereum](https://ethereum.brightid.org) application.

#### Authenticated and Unverified
If an authenticated user has not been verified with BrightID, the login modal should prompt them to visit the [Bright Ethereum](https://ethereum.brightid.org) application.

#### Authenticated and Verified
If an authenticated user has been verified with BrightID, the login modal should prompt them to enter the amount of ETH they would like to burn. The "vote" button should trigger a transaction to the poll option's receiving address for the amount of ETH entered.

### About Modal
The this is a simple modal describing the Burn Signal project and linking to some of our resources.

The text should read as follows:

Burn Signal is an experiment in distributed preference signaling where verified unique users burn ETH to signal their opinion.

Votes are weighted quadratically.

We use [BightID](https://brightid.org) as our proof of uniqueness, only votes cast by ethereum addresses that are verified unique by BrightID count towards the outcome of a burn signals.

Burn Signal is funded by grants and community contributions. If you would like to contribute funds, check out our [Gitcoin Grants campaign](https://gitcoin.co/grants/138/burner-vote), if you would like to contribute code or other work, check out our [GitHub](https://github.com/burnSignal) and our [Colony](https://colony.io/colony/burn).

Check out our [blog](https://blog.burnsignal.io) for more information.
