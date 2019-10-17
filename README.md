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

## Routes
There following routes exist in the dapp, all others should 404 gracefully.

`/` --> `/home`

`/home` --> The application's home feed.

`/new/poll` --> The "new poll" modal, which should render over whichever page is already displayed or over the contents of `/home` if it was navigated to directly.

`/{ethAddress}` --> the user profile corresponding to `ethAddress`.

`/{ENSDomain}` --> the user profile corresponding to the ethereum address that `ENSDomain` resolves to.

`/{ethAddress}/poll/{pollAddress}` --> the poll page for the poll at ethereum address `pollAddress`.

`/{ethAddress}/poll/{pollAddress}/yes` --> the "yes" voting information for the poll at ethereum address `pollAddress`. Displayed as a modal over the current page, or over the contents of `/{ethAddress}/poll/{pollAddress}` page if navigated to directly.

`/{ethAddress}/poll/{pollAddress}/no` --> the "no" voting information for the poll at ethereum address `pollAddress`. Displayed as a modal over the current page, or over the contents of `/{ethAddress}/poll/{pollAddress}` page if navigated to directly.

`/404` --> A 404 page containing all of the basic elements listed below, along with a lighthearted message.

## Data Sources
### 3Box
Most profile data will be sourced from 3Box.
### BrightID
BrightID will be used a an oracle for uniqueness.
### TheGraph
TheGraph's GraphQL API will be used to populate feeds with poll and vote data.

## Basic Elements
These elements are common to most pages, although their contents may vary slightly on each page.

### Nav Bar
#### Title
Displayed on the left side of the Nav Bar is a title for the current page.
The title should be "Home" for the home page, "Poll" for a poll page, or `{username}` for a profile page.
The title should link back to the current page.
e.g. The title on the home page should link to `/home`, the title on a poll's page should link to `/{ethAddress}/poll/{pollAddress}`, etc.

#### Search Field
The search field is a single line text entry box that should filter the feed based on a `pollAddress`, `pollText`, `username` of creator, or `ethereumAddress` of creator.
The search field is not displayed on poll pages (`/{ethAddress}/poll/{pollAddress}`).

#### Back Button
The back button is displayed on all pages other than `/home` and should link back to `/home`. It should be placed to the left of the title.

#### Activity Feed
The activity feed contains the primary content of each page, which consists of a feed of relevant polls, and some additional visualizations in the case of a poll's page (`/{ethAddress}/poll/{pollAddress}`).

### Poll Items
Each poll items should include:
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
- "Results" text that, on hover, displays the current results of the poll, weighted quadratically, in place of the "Yes" and "No" buttons.
Each poll item should be clickable and link to the relevant poll's page (`/{ethAddress}/poll/{pollAddress}`).
i.e. clicking anywhere on a poll item, that is not explicitly a link to somewhere else (profile details, buttons, etc), should navigate the user to the relevant poll's page.

### Left Gutter
#### Burner Vote Logo
A burner vote logo that links to `/home`
#### New Poll Button
The "New Poll" button should open the "new poll" modal (`/new/poll`), which should render over the contents of whichever page is already displayed.

### Right Gutter
In the MVP version, the right gutter is empty.
However, in future versions this space will be used display things like trending polls (something like the top five most voted on polls in the previous 24 hours) and promoted polls (there will be a small number of slots in which people can pay to promote polls, these slots will be purchasable using a Harberger tax).

## Home
The home page `/home` consists primarily of a feed of use created polls. The home page also includes a "New Poll" button and a "Search" field.
The feed should progressively load content as the user scrolls.

### Activity Feed
In the MVP version of the dapp, the activity feed on `/home` will be a global feed of all polls, sorted in reverse chronological order.

If volume or spam becomes an issue, we may need to introduce some default quality filters or Reddit style sorting options.

In future iterations, we may introduce the ability to follow users and/or hashtags, in which case this feed may become more personalized.

## Poll Pages
Each poll should have its own dedicated page who's route is `/{ethAddress}/poll/{pollAddress}`.

### Activity Feed
The first item in the activity feed on a poll page is the relevant [poll item]().

Below that, there should be a variety of visualizations for the current results of the polls.
This should include:
- Number of unique voters
- Total ETH burned
- A pie chart of the quadratic vote results as percentages (each unique user's vote is weighted quadratically based on the amount of ETH they burned, where 1 vote = 0.00005 ETH / roughly $0.01 USD). Users can vote more than once, the value of each of their votes should be totaled and counted as one vote, with the weight determined quadratically from the total.
- A pie chart of the democratic vote results as percentages (each unique user has one vote). Users can vote more than once, but only their most recent vote counts.
- A pie chart of the coin weighted vote results as percentages (the total ETH burned for each option; `yes` and `no`).

## Profile Pages
Profile pages can be reached via two different routes, `/{ethAddress}` or `/{ENSDomain}`.
The profile page will display the profile associate with the ethereum address in the URL or the ethereum address that the ENSDomain resolves to.

Any 40 character hexadecimal string prepended by "0x" will be treated as an ethereum address. As such, any ENS domain that mimics the form of an ethereum address will resolve to the mimicked ethereum address, rather than the ethereum address that the ENS domain points to.

Any string that is not a 40 character hexadecimal string will be assumed to be an ENS domain, regardless of whether `.eth` is appended.

e.g.
`/0xD1220A0cf47c7B9Be7A2E6BA89F429762e7b9aDb` would resolve to `/0xD1220A0cf47c7B9Be7A2E6BA89F429762e7b9aDb`
`/0xD1220A0cf47c7B9Be7A2E6BA89F429762e7b9aDb.eth` would resolve to the profile of whichever ethereum address is pointed to by `0xD1220A0cf47c7B9Be7A2E6BA89F429762e7b9aDb.eth`
`/barry` would resolve to the profile of whichever ethereum address is pointed to by `barry.eth`
`/barry.eth` would resolve to the profile of whichever ethereum address is pointed to by `barry.eth`

### Profile Details
On the profile page, the
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
The feed displayed on a user's profile will include all polls created by that user, displayed in reverse chronological order.

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
