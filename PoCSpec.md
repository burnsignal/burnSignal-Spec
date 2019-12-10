# Burn Signal PoC Web App Specification
## Contents

1. [Overview](https://github.com/burnSignal/burnSignal-Spec/blob/master/PoCSpec.md#overview)
1. [Routes](https://github.com/burnSignal/burnSignal-Spec/blob/master/PoCSpec.md#routes)
1. [SubGraph](https://github.com/burnSignal/burnSignal-Spec/blob/master/PoCSpec.md#subgraph)
1. [Basic Elements](https://github.com/burnSignal/burnSignal-Spec/blob/master/PoCSpec.md#basic-elements)
1. [Home](https://github.com/burnSignal/burnSignal-Spec/blob/master/PoCSpec.md#home)
1. [Poll Pages](https://github.com/burnSignal/burnSignal-Spec/blob/master/PoCSpec.md#poll-pages)
1. [Profile Pages](https://github.com/burnSignal/burnSignal-Spec/blob/master/PoCSpec.md#profile-pages)
1. [Modals](https://github.com/burnSignal/burnSignal-Spec/blob/master/PoCSpec.md#modals)

## Overview
This document specifies the Burn Signal PoC web application, referred to herein as "the dapp".
Where this specification is insufficient or vague, the [figma prototype](https://www.figma.com/file/klSsgAu3ZvWKE8Xap80TjK/PoC?node-id=1%3A2) should be used as a reference. Where any ambiguity remains, @auryn-macmillan should be consulted for clarity.

Wherever possible, and ideally without compromise, the application should run client-side with data hosted on public and trustless (trust minimized) sources. The current state of the application should be permissionlessly reconstructable by any user whether they are running the application locally or using our hosted version.

## Routes
The following routes exist in the dapp, all others should 404 gracefully and redirect users to `/`

`/` --> `/poll/{pollAddress}`

`/poll/{pollAddress}` --> Poll page for the featured poll.{pollAddress} is the ethereum address of the featured poll.

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

## Elements
### Nav Bar

### Poll Description and Options

### Bar Chart

### Line Chart

### Vote Modal
