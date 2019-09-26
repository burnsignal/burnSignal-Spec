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
TheGraph's GraphQL API will be used to populate feeds with poll and vote data.

## Home
The home page `/home` consists primarily of a feed of use created polls. The home page also includes a "New Poll" button and a "Search" field.
The feed should progressively load content as the user scrolls.

### Polls
Polls should be sorted in reverse chronological order.
Each poll should include:
- A profile image and display name of the user who created it (or a blockie and truncated ethereum address if no 3Box data is available). Each of these elements should link to the user's profile.
- How long the poll has been running
    - < 1 minute ago = "Just now"
    - >= 1 & < 60 minutes ago = {minutesSincePost}+"m"
        - e.g. 20m
    - >= 1 & < 24 hours ago = {hoursSincePost}+"h"
        - e.g. 10h
    - >= 1 day ago = {month}+" "+{day}+", "+{year}
        - e.g. Jan 10, 2018
- The poll's text
- "Yes" and "No" buttons that open the poll's `/{ethAddress}/poll/{pollAddress}/yes` and `/{ethAddress}/poll/{pollAddress}/no` modals, respectively.
- Unique voter count
- Total ETH burned
- "Results" text that, on hover, displays the current results of the poll in place of the "Yes" and "No" buttons.

### Search Field
The search field should filter results based on a `pollID`, `pollAddress`, and `pollText`.

### New Poll Button
The "New Poll" button should open the "new poll" modal, which should render over whichever page is already displayed (or the home feed as a fallback if `/new/poll` was navigated to directly).

## Polls
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

## New Poll Modal
todo

## Vote Yes/No Modal
Todo

## Routes
There following routes exist in the dapp, all others should 404.

`/` --> `/home`

`/home` --> The application's home feed.

`/new/poll` --> The "new poll" modal, which should render over whichever page is already displayed (or the home feed as a fallback).

`/{ethAddress}` --> the user profile corresponding to `ethAddress`.

`/{ENSDomain}` --> the user profile corresponding to the ethereum address that `ENSDomain` resolves to.

`/{ethAddress}/poll/{pollAddress}` --> the poll page for the poll at ethereum address `pollAddress`.

`/{ethAddress}/poll/{pollAddress}/yes` --> the "yes" voting information for the poll at ethereum address `pollAddress`. Displayed as a modal over the current page, or over the `/{ethAddress}/poll/{pollAddress}` page if navigated to directly.


`/{ethAddress}/poll/{pollAddress}/no` --> the "no" voting information for the poll at ethereum address `pollAddress`. Displayed as a modal over the current page, or over the `/{ethAddress}/poll/{pollAddress}` page if navigated to directly.
