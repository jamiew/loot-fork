# Loot (for Forkers)

Quick proof of concept for my boi @codyb and all the cryptokeepers at [Definitely Friends](https://definitely.shop)

1. It's @dhof's beautiful little [LOOT contract](https://etherscan.io/address/0xff9c1b15b16263c61d017ee9f65c50e4ae0113d7#code)

2. But with the flattened [OpenZeppelin contracts](https://openzeppelin.com/contracts/) extracted back into their original `imports`, for cleanliness

3. Then a healthy dose of `npx hardhat` boilerplate (Advanced Example) which is A Pretty Great Way To Write Solidity

4. And then this brilliant README

5. Good luck, we're all counting on you


## Vamanoooooooooooooos

Checkout this repo and install dependencies

```
git clone https://github.com/jamiew/loot-fork
cd loot-fork
npm install
```

compile the contract and deploy to the internal `hardhat` network

```
npx hardhat compile
npx hardhat run scripts/deploy.js
```

## Here's the next level

a real example requires you to run a local Ganache blockchain simulator (AKA the `localhost` network, chainId `31337`):

```shell
# in one terminal, run a lil blockchain
npx hardhat node --show-stack-traces

# in another terminal, deploy the contract and copy the deployed address
npx hardhat run --network localhost scripts/deploy.js
```

then start `npx hardhat console` and you can interact with said contract

```shell
npx hardhat console --network localhost
```

in the console, connect to our newly deployed `LootFork`:

```javascript
const Contract = await ethers.getContractFactory('LootFork');
const contract = await Contract.attach("ADDRESS_FROM_DEPLOYMENT_GOES_HERE_:)");
```

then let's call some contract methods:

```javascript
(await contract.name()).toString()
// 'LootFork'

(await contract.totalSupply()).toString();
// '0'
// (because we haven't minted anything yet)

(await contract.getWeapon(1)).toString();
// '"Grim Shout" Grave Wand of Skill +1'
```

if you want some loots, mint them to one of the default accounts setup by `hardhat node` (ganache). they cryptorich, they got like 10k ETH each

```javascript
let tokenId = 1;
let account = (await ethers.getSigners())[0];
let txn = (await contract.connect(account).claim(tokenId));
let receipt = (await txn.wait());
console.log(receipt.events[0].args)
/*
[
  from: '0x0000000000000000000000000000000000000000',
  to: '0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266',
  tokenId: BigNumber { _hex: '0x03', _isBigNumber: true }
]
*/
```

did it work though?
```javascript
(await contract.totalSupply()).toString();
// '1'

(await contract.tokenURI(1));
// 'data:application/json;base64,eyJuYW1lIjogIkJhZyAjMSIsICJkZXNjcmlwdGlvbiI6ICJMb290IGlzIHJhbmRvbWl6ZWQgYWR2ZW50dXJlciBnZWFyIGdlbmVyYXRlZCBhbmQgc3RvcmVkIG9uIGNoYWluLiBTdGF0cywgaW1hZ2VzLCBhbmQgb3RoZXIgZnVuY3Rpb25hbGl0eSBhcmUgaW50ZW50aW9uYWxseSBvbWl0dGVkIGZvciBvdGhlcnMgdG8gaW50ZXJwcmV0LiBGZWVsIGZyZWUgdG8gdXNlIExvb3QgaW4gYW55IHdheSB5b3Ugd2FudC4iLCAiaW1hZ2UiOiAiZGF0YTppbWFnZS9zdmcreG1sO2Jhc2U2NCxQSE4yWnlCNGJXeHVjejBpYUhSMGNEb3ZMM2QzZHk1M015NXZjbWN2TWpBd01DOXpkbWNpSUhCeVpYTmxjblpsUVhOd1pXTjBVbUYwYVc4OUluaE5hVzVaVFdsdUlHMWxaWFFpSUhacFpYZENiM2c5SWpBZ01DQXpOVEFnTXpVd0lqNDhjM1I1YkdVK0xtSmhjMlVnZXlCbWFXeHNPaUIzYUdsMFpUc2dabTl1ZEMxbVlXMXBiSGs2SUhObGNtbG1PeUJtYjI1MExYTnBlbVU2SURFMGNIZzdJSDA4TDNOMGVXeGxQanh5WldOMElIZHBaSFJvUFNJeE1EQWxJaUJvWldsbmFIUTlJakV3TUNVaUlHWnBiR3c5SW1Kc1lXTnJJaUF2UGp4MFpYaDBJSGc5SWpFd0lpQjVQU0l5TUNJZ1kyeGhjM005SW1KaGMyVWlQaUpIY21sdElGTm9iM1YwSWlCSGNtRjJaU0JYWVc1a0lHOW1JRk5yYVd4c0lDc3hQQzkwWlhoMFBqeDBaWGgwSUhnOUlqRXdJaUI1UFNJME1DSWdZMnhoYzNNOUltSmhjMlVpUGtoaGNtUWdUR1ZoZEdobGNpQkJjbTF2Y2p3dmRHVjRkRDQ4ZEdWNGRDQjRQU0l4TUNJZ2VUMGlOakFpSUdOc1lYTnpQU0ppWVhObElqNUVhWFpwYm1VZ1NHOXZaRHd2ZEdWNGRENDhkR1Y0ZENCNFBTSXhNQ0lnZVQwaU9EQWlJR05zWVhOelBTSmlZWE5sSWo1SVlYSmtJRXhsWVhSb1pYSWdRbVZzZER3dmRHVjRkRDQ4ZEdWNGRDQjRQU0l4TUNJZ2VUMGlNVEF3SWlCamJHRnpjejBpWW1GelpTSStJa1JsWVhSb0lGSnZiM1FpSUU5eWJtRjBaU0JIY21WaGRtVnpJRzltSUZOcmFXeHNQQzkwWlhoMFBqeDBaWGgwSUhnOUlqRXdJaUI1UFNJeE1qQWlJR05zWVhOelBTSmlZWE5sSWo1VGRIVmtaR1ZrSUV4bFlYUm9aWElnUjJ4dmRtVnpQQzkwWlhoMFBqeDBaWGgwSUhnOUlqRXdJaUI1UFNJeE5EQWlJR05zWVhOelBTSmlZWE5sSWo1T1pXTnJiR0ZqWlNCdlppQkZibXhwWjJoMFpXNXRaVzUwUEM5MFpYaDBQangwWlhoMElIZzlJakV3SWlCNVBTSXhOakFpSUdOc1lYTnpQU0ppWVhObElqNUhiMnhrSUZKcGJtYzhMM1JsZUhRK1BDOXpkbWMrIn0='
```

looks good to me

congratulations jeffrey bezos u did it


## put that bad boy on mainnet and get lootrich

or perhaps start with rinkeby, my favorite testnet

This largely requires funding a wallet and registering API keys with [Alchemy](https://docs.alchemy.com/alchemy/introduction/getting-started) and [Etherscan]()

Copy `.env.sample` to `.env` and edit in your keys

Then:

```shell
npx hardhat run scripts/deploy.js --network rinkeby
```

you can interact with this contract via `npx hardhat console` the same way as above, just substitute `--network rinkeby` for `--network localhost`

You can also use the `hardhat-etherscan-verify` plugin to verify the contract on Etherscan, which is required to be truly eleet

```
npx hardhat verify --network rinkeby <YOUR_CONTRACT_ADDRESS>
```

Substitute `mainnet` for `rinkeby` to deploy for realsies. good luck


# more reading

* [Hardhat docs](https://hardhat.org/getting-started/)
* [OpenZeppelin docs](https://docs.openzeppelin.com/openzeppelin/)
