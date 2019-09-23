# Burner Vote Web App MVP Specification
## Contents
1. Data sources
1. Home
1. Polls
1. Profiles
1. Routes

## Overview
This document is the specification for the Burner Vote MVP Web Application, often referred to herein as "the application". Where this specification is insufficient or vague, the [figma prototype](https://www.figma.com/file/9V9NnnJ5qbS22QKVFe2Eao/Burner-Vote?node-id=0%3A1) should be used as a reference. That failing, Twitter's desktop browser UI and UX should serve as a reference, where applicable. And, where any ambiguity remains, [@auryn-macmillan](https://github.com/auryn-macmillan) should be consulted for clarity.

Wherever possible, and ideally without compromise, the application should run client-side with data hosted on public and trustless (trust minimized) sources. The current state of the application should be permissionlessly reconstructable by any user whether they are running the application locally or using our hosted version.

## Data Sources
### 3Box
Most profile data will be sourced from 3Box.
### BrightID
BrightID will be used a an oracle for uniqueness.
### TheGraph
TheGraph will be used to populate feeds with poll and vote data.

## Home
todo

### Polls
todo

## Profiles
### Profile Details
- Ethereum address
- [Blockie](https://github.com/petejkim/ethereum-blockies-png)
- Name
- Profile image
- Banner image
- Location
- URL
- Verified Twitter account
- Verified Github account
- [BrightID](https://brightid.org) score

### Feed
The feed displayed on a user's profile will include all polls that they have created along with their voting history. Items will be displayed in reverse chronological order.

### 3Box
The application will import the following user profile information from 3Box:
- Name
- Profile image
- Banner image
- Location
- URL
- Verified Twitter account
- Verified Github account

In cases where a 3Box profile does not exist (or has not yet loaded), the application will simply fallback to using the etherurm address in place of Name and the ethereum address's [blockie](https://github.com/petejkim/ethereum-blockies-png) in place of a Profile Image. All other fields will be not be displayed.

### BrightID
In the case where a BrightID score is not found, it will read "None".

### Blockies
Each profile image should be replaced by its blockie on hover.

## Routes
There following routes exist in the dapp, all others should 404.

`/` --> `/home`

`/home` --> The application's home feed.

`/new/poll` --> The "new poll" modal, which should render over whichever page is already displayed (or the home feed as a fallback).

`/{ethAddress}` --> the user profile corresponding to `ethAddress`.

`/{ENSDomain}` --> the user profile corresponding to the ethereum address that `ENSDomain` resolves to.

`/{ethAddress}/poll/{pollAddress}` --> the poll page for the poll at ethereum address `pollAddress`.

`/{ethAddress}/poll/{pollAddress}/yes` --> the "yes" voting information for the poll at ethereum address `pollAddress`.

`/{ethAddress}/poll/{pollAddress}/no` --> the "no" voting information for the poll at ethereum address `pollAddress`.
