# Full-stack Web3 Engineering Assignment
Thank you for your interest in joining the SportX engineering team!

This document is just a quick test to see where your coding and problem solving skills are with something related to dApp development. It’s designed to be straightforward and not take too much of your time. 

Since a lot of this position will be JavaScript/Typescript-based app development and integrating with Solidity contracts, we've created a task that, in some capacity, represents the tooling you'll be working with on a day-to-day basis with us. 

## Background
"Blind" voting on a public blockchain takes some thought. All data is public so without any extra tricks it's trivial to see who voted what and which side is likely to win. This can greatly bias voters and lead to inefficient decisions. 

One way to mitigate this is to use a "commit-reveal" scheme. In such a scheme, eligible voters commit `Hash(x + secret)` in some "commit period" where `x` is their vote choice. For simplicitly, suppose `x` is either 0 or 1 and the `"+"` operator means concatenation. So for example, a user might commit a vote `"Hash(0~mysuperbigsecret)"`. We call this the "commitment". 

After the "commit period", voters can reveal their vote by supplying `(x + secret)`, and their `Hash(x + secret)`. This would be `"0~mysuperbigsecret"` and `"Hash(0~mysuperbigsecret)"` respectively, in this case. Using the fact that hash collisions are impossible using a cryptographic hash function, we can cryptographically prove that a user committed to a particular vote by computing the hash of `(x + secret)` and comparing it with the supplied commitment. 

Using this technique, it's now impossible to know what a given user committed to before the revealing period unless they told you their secret beforehand, grealty improving the effectiveness of the vote. 

## The task

We've written a simple implementation of a commit-reveal vote scheme in Solidity in `CommitReveal.sol`. **Our ask is for you to write a very simple CLI to wrap it and make it easy for users to participate in the vote.**

The Solidity implementation we wrote is very simple: it has only two choices, "YES" and "NO", the commit phase lasts 2 minutes, and users can vote multiple times. It's based on the blog written by Karl Floresch which we encourage you to check out [here](https://karl.tech/learning-solidity-part-2-voting/) if you're looking for more background.

## Requirements

Using any framework you'd like, please make a CLI that interacts with the contract. Here are the functional requirements and also some clarification on things you do NOT have to do:

Functionality:
- A user should be able to see the two choices they can vote for, "YES" and "NO", 
- A user should be able to see the current status of the vote including:
    - Approximate time left until the commit phase is over
    - The current vote distribution
    - The current status of the vote (voting/revealing/revealed)
    - The number of votes cast
    - The winner of the vote, if any yet
- A user should be able to commit votes if they are in the commit period
- A user should be able to reveal votes if they are in the reveal period

Things you don't have to do:
- Publish this as an NPM package
- Write any further tests for the contracts or exhaustive tests for your CLI. The CLI just needs to work.
- Add any functionality for "switching" between accounts. A user can vote multiple times and that is totally fine. 
- Deploy this on any public blockchain. Use a local blockchain such as `ganache-cli`.

For interacting with the contracts, you can use `ethers.js`, `web3`, or the native truffle contract wrappers. Up to you. You can make as many commands as you wish. 

## Setup
First, install the dependencies. You'll need a local Ethereum blockchain to deploy the contracts and run the contract tests if you want. We recommend `ganache`. You can install ganache with 

`npm install -g ganache-cli` 

and start it up with 

`ganache-cli`

If you want to run the tests (not necessary to complete this assignment, just to verify the functionality of the solidity contracts), you'll have to monkey patch the line 

`node_modules/@0x/utils/lib/src/provider_utils.js:81`

with 

`if (_.includes(supportedProvider.send.toString().replace(' ', '').replace(' ', ''), 'function(payload,callback)')) :`

otherwise you'll run into a `TypeError: Cannot read property 'then' of undefined` as there is some annoying bug with web3 currently.

## Other notes

- We've already written the migration script with pre-defined choices and there are some helper npm scripts in `package.json` in case you don't want to install truffle beta globally. 
- `truffle-config.js` is already configured to point to your local ganache on port `8545`.
- The `test` folder includes some example code to interact with the contracts using the `truffle-contract` wrappers. **You don't have to use truffle to interact with the contracts**.
- To help with transitioning from the commit phase to the reveal phase, we've provided some example code in the `test` folder. Ganache has a special command called `evm_increaseTime` which can artifically set the next block's timestamp and `@0xproject` provides a nice wrapper around this command. Just remember that once you go forward in time you can't go back!

All you have to do is write your CLI!

## What we're looking for
We're interested in your coding style, your familarity with developer tooling for smart contracts, and your JavaScript/TypeScript proficiency.

## How to complete this challenge
Fork this repo, write your code in a sub-folder in this repo, and edit this `README` to include instructions on how to use your CLI. Feel free to change anything in the repo except for the `CommitReveal.sol` contract.

Send `jake@sportx.bet` the GitHub link when you're done. 

Good luck!