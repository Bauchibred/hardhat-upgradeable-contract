# Hardhat Upgrades

This is a section of the Javascript Blockchain/Smart Contract FreeCodeCamp Course.

_[⌨️ (28:53:11) Lesson 16: Hardhat Upgrades](https://www.youtube.com/watch?v=gyMwXuJrbJQ&t=103991s)_
_This repo has been updated to work with Goerli over Rinkeby._

[Full Repo](https://github.com/smartcontractkit/full-blockchain-solidity-course-js)

- [Hardhat Upgrades](#hardhat-upgrades)
- [Getting Started](#getting-started)
  - [Requirements](#requirements)
  - [Quickstart](#quickstart)
  - [Typescript](#typescript)
    - [Optional Gitpod](#optional-gitpod)
- [Usage](#usage)
  - [Testing](#testing)
    - [Test Coverage](#test-coverage)
- [Deployment to a testnet or mainnet](#deployment-to-a-testnet-or-mainnet)
  - [Scripts](#scripts)
  - [Estimate gas](#estimate-gas)
    - [Estimate gas cost in USD](#estimate-gas-cost-in-usd)
  - [Verify on etherscan](#verify-on-etherscan)
- [Linting](#linting)
- [Formatting](#formatting)
- [Thank you!](#thank-you)

This project is apart of the Hardhat FreeCodeCamp video.

Video coming soon...

# Getting Started

## Requirements

- [git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
  - You'll know you did it right if you can run `git --version` and you see a response like `git version x.x.x`
- [Nodejs](https://nodejs.org/en/)
  - You'll know you've installed nodejs right if you can run:
    - `node --version` and get an ouput like: `vx.x.x`
- [Yarn](https://classic.yarnpkg.com/lang/en/docs/install/) instead of `npm`
  - You'll know you've installed yarn right if you can run:
    - `yarn --version` and get an output like: `x.x.x`
    - You might need to install it with npm

## Quickstart

```
git clone https://github.com/Bauchibred/hardhat-upgradeable-contract
cd hardhat-upgradeable-contract
yarn
```

## Typescript

For the typescript edition, run:

```
git checkout typescript
```

### Optional Gitpod

If you can't or don't want to run and install locally, you can work with this repo in Gitpod. If you do this, you can skip the `clone this repo` part.

[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#github.com/PatrickAlphaC/hardhat-upgrades-fcc)

# Usage

Deploy:

```
yarn hardhat deploy
```

## Testing

```
yarn hardhat test
```

### Test Coverage

```
yarn hardhat coverage
```

# Deployment to a testnet or mainnet

1. Setup environment variabltes

You'll want to set your `GOERLI_RPC_URL` and `PRIVATE_KEY` as environment variables. You can add them to a `.env` file, similar to what you see in `.env.example`.

- `PRIVATE_KEY`: The private key of your account (like from [metamask](https://metamask.io/)). **NOTE:** FOR DEVELOPMENT, PLEASE USE A KEY THAT DOESN'T HAVE ANY REAL FUNDS ASSOCIATED WITH IT.
  - You can [learn how to export it here](https://metamask.zendesk.com/hc/en-us/articles/360015289632-How-to-Export-an-Account-Private-Key).
- `GOERLI_RPC_URL`: This is url of the goerli testnet node you're working with. You can get setup with one for free from [Alchemy](https://alchemy.com/?a=673c802981)

2. Get testnet ETH

Head over to [faucets.chain.link](https://faucets.chain.link/) and get some tesnet ETH. You should see the ETH show up in your metamask.

3. Deploy

```
yarn hardhat deploy --network goerli
```

## Scripts

After deploy to a testnet or local net, you can run the scripts.

```
yarn hardhat run scripts/fund.js
```

or

```
yarn hardhat run scripts/withdraw.js
```

## Estimate gas

You can estimate how much gas things cost by running:

```
yarn hardhat test
```

And you'll see and output file called `gas-report.txt`

### Estimate gas cost in USD

To get a USD estimation of gas cost, you'll need a `COINMARKETCAP_API_KEY` environment variable. You can get one for free from [CoinMarketCap](https://pro.coinmarketcap.com/signup).

Then, uncomment the line `coinmarketcap: COINMARKETCAP_API_KEY,` in `hardhat.config.js` to get the USD estimation. Just note, everytime you run your tests it will use an API call, so it might make sense to have using coinmarketcap disabled until you need it. You can disable it by just commenting the line back out.

## Verify on etherscan

If you deploy to a testnet or mainnet, you can verify it if you get an [API Key](https://etherscan.io/myapikey) from Etherscan and set it as an environemnt variable named `ETHERSCAN_API_KEY`. You can pop it into your `.env` file as seen in the `.env.example`.

In it's current state, if you have your api key set, it will auto verify goerli contracts!

However, you can manual verify with:

```
yarn hardhat verify --constructor-args arguments.js DEPLOYED_CONTRACT_ADDRESS
```

# Linting

To check linting / code formatting:

```
yarn lint
```

or, to fix:

```
yarn lint:fix
```

# Formatting

```
yarn format
```

# Thank you!



HARDHAT UPGRADES
Actually smart contract can change whenever there is a transcaction
So we can upgrade our contracts with hardhat and openZeppelin
3 different ways
-Parametrize
This is the simplest way but we cant add new logic or storage, this is really simple to implement and we only need to see who the admin is and the advisable way is having a governance token
-Social Migration method YEET
THis is just when we deploy a new contract and not connecting it to the old contract any way and you just inform people that this si the new contract
Pros:
Truest blockchain values
Easiest to audit
Cons:
Lot of work to convince users to move
Different addresses

-The proxy
Proxies are the truest forms of upgrades, cause the users can keep interacting with the smart contracts and not even notice that anything changes, but this is also the one that's the hardest not to mess up cause it uses a lot of low level functionality and the main one being the delegatecall, so now whenever we want to upgrade we just implement a new contract and then use our proxy to direct users to the new implemented smart contracts
4 pieces of logic to keep in mind for the proxy method

1. The implementation contract - this contract has all the code of our protocol
2. Proxy contract- This points which implement ation is the correct one, and routes everyone's function calls to the dcontract
3. The user - makes calls to the proxy
4. The admin- the user(group of users) who decide when to upgrade to new implementation contracts
   Gotchas:
   -Storage clashes: When we use delegatecall we use the logic of contract A in contract B but these smart contracts are actually kinda dumb if we try to change old storage it actually mixes it up so we need to create new ones
   -Function selector clashes: This is a 4 byte hash of a function name and a signature that defines a function, so when 2 functions have the same function selector we can run into an issue

Transparency proxy pattern:
Over here admins can't call implementation contract functions and users are still powerless on admin functions
Universal upgradeable proxies UUPS:
This is a gas saver
Diamond Pattern:
This helps if you want to make granular upgrades as you dont have to always update your smart contracts this way

Delegate Call(Explained under the sublesson folder)
This is similar to a callback function
Normally if we use contract.call we would just be calling the function setVars but with contract.delegatecall we are kinda borrowing the function for our contracg, so we are saying call this function in our contract, we can use the solidity-by-example.org/delegatecall on remix to see how this works
In essence it's like we are borrowing the function and calling it once on our function and immediately deleting it.
But if the type we are calling and setting is a boolean it gets tricky cause our smart contract goes if we are setting it to anything other than 0 then it's true
Small proxy example
this is under the sublesson using openzeppelin and yul, use as little yul as possible cause it is really low level and a little mistake can mess things up, in short what this is about is that any time a cokntract receives an information that it doesn't recognize it sends it to some impolementation that delegatecall through a fallback function
EIP 1976 is the standard proxy storage slots that's used for our proxies.

Transparent Upgradeable Proxy Contract
Let's create our contracts folder and also our contracts
Box.sol is our implementation

Then our BoxV2, we update the veraion and add and increment
So main steps are
1-Upgrade Box-> BoxV2
2- Proxy Box
-> BoxV2

3- We need to
i-deploy a proxy manually, or
ii-hardhat deploy also comes built in with deploying and updating proxies, wher we just specify that we want to use a proxy and what type of proxy we would like to use.
iii-Another option is to use openzeppelin's upgrade plug in which allows us touse really simple scripts and have a little

This code is using the hardhat deploy built in proxies
So in our deploy scripts
We add the basics and since we are going to be using the transparent proxy from openZeppling we yarn add it,
Then we use the OpenZeppelinTransparentProxy as our proxy contract
And also have a viaAdminContract, as this is best practise
So now we need to create a BoxProxyAdmin contract to be the admin of our contract under the contracts/proxy, once again we can use openzeppelin transparent proxyadmin.sol which is esentially what our proxy contract is also going to look like, so we just import it in proxy contract, then we inherit it as our BoxProxyAdmin, then we make a constructor that has an address and a proxyadmin, so that's it for our proxy file cause proxy admin from openzeppelin already comes with all the functionalities that's needed.
So now we create a new deploy scripts box v2.js  
Then we make our upgrade script manually, though hardhat comes with an api that can really make us do this in an easier way
After making our upgrade script if we upgrade we now have an upgrade for the already existing functionalities of our code from the V1 to the V2.
