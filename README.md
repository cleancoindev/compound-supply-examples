# Quick Start: Supplying Assets to the Compound Protocol

Examples for supplying Ethereum assets to the [Compound Protocol](https://compound.finance/?ref=github&user=ajb413&repo=compound-supply-examples). 

If you want to supply assets directly to the protocol from your Ethereum wallet using JSON RPC and Web3.js, see the `json-rpc-examples` folder. JSON RPC can be utilized in the **web browser or with Node.js**.

If you want to supply assets to the protocol from your Ethereum smart contract, see the `solidity-examples` folder.

## What is Compound?
Compound is an open-source, autonomous protocol built for developers, to unlock a universe of new financial applications. Interest and borrowing, for the open financial system. Learn more on the website:

<a href="https://compound.finance/?ref=github&user=ajb413&repo=compound-supply-examples">
    <img alt="Compound Finance" src="https://raw.githubusercontent.com/ajb413/compound-interest-alerts/master/compound-finance-logo.png" width=260 height=60/>
</a>

## Setup
The code in this repository can be used to supply assets to Compound on the main net, any public test net, or your own localhost with [Ganache CLI](https://github.com/trufflesuite/ganache-cli).

If you haven't already, install [Node.js](https://nodejs.org/) LTS. Clone this repository, `cd` to the root directory of the project, and run:
```bash
npm install
```

If you want to use the script examples in the **web browser**, you'll need to first import web3.js in your HTML file using the following line. You'll also need to import the JS example files into your HTML. This step is **not necessary** if you are running the examples with only Node.js.
```html
<script src="https://cdn.jsdelivr.net/npm/web3@1.2.6/dist/web3.min.js"></script>
```

### Running a Local Ethereum Test Net with Ganache CLI
Before we continue get an API key from [Infura.io](https://infura.io/). Infura will be used to access the Ethereum network. For testing, it will be used to make a `fork` of the Main Ethereum network that runs on your local machine. You'll use your Infura API key in the next step.

To get the localhost test net running, use the following commands in a second terminal window. The command runs Ganache CLI and forks the Main Ethereum network to your machine via Infura (be sure to **insert your API key** where noted below).
```bash
cd compound-supply-examples/

## Run a fork of main net locally using Ganache CLI
./node_modules/.bin/ganache-cli \
  -f https://mainnet.infura.io/v3/<YOUR INFURA API KEY HERE> \
  -m "clutch captain shoe salt awake harvest setup primary inmate ugly among become"
  -i 999 \
  -u 0x9759A6Ac90977b93B58547b4A71c78317f391A28
```

- `-f` Forks the Main Ethereum network to your local machine for development and testing.
- `-m` Runs Ganache with an Ethereum key set based on the mnemonic passed. The first 10 addresses have 100 test ETH in their balance on the local test net every time you boot Ganache. **Do not use this mnemonic anywhere other than your localhost test net.**
- `-i` Sets an explicit network ID to avoid confusion and errors.
- `-u` Unlocks an address so you can write to your localhost test blockchain without knowing that address's private key. We are unlocking the above address so we can mint our own test DAI on our localhost test net. The address for minting DAI changes occasionally (see **Minting Test DAI** section below for updating).

## Supplying Assets Directly via JSON RPC
These code examples can be run by a web browser or with Node.js. If you want to use a web browser, you'll need to import the web3.js script in your HTML or JS file (see import above).

Running these scripts will give your wallet **cETH** and **cDAI**. These are ERC20 Tokens that can be **used to redeem an ever-increasing amount of the underlying asset**. The cToken exchange rate **increases every Ethereum block**, they can be transferred, and can be used to redeem at any time.

### Localhost Test Net
- Run your local test net in a second command line window **using the command above**.
- If using DAI (an ERC20 token example), you need to **first** mint some for your wallet using `node mint-testnet-dai.js`. You may need to update the DAI main net contract address and the `MCD_JOIN_DAI` address in the script. This changes periodically as DAI is improved (see **Minting Test DAI** section below for updating).
- `cd json-rpc-examples/`
- `node supply-eth-via-json-rpc.js` To supply ETH.
- `node supply-erc20-via-json-rpc.js` To supply some DAI. The same code can be used for any other [ERC20 token that Compound supports](https://compound.finance/markets?ref=github&user=ajb413&repo=compound-supply-examples).

### Public Test Net or Main Net
- Make sure you have a wallet with ETH for the Ethereum network you plan to interface with (Main, Ropsten, Kovan, etc...).
- Insert the private key of your wallet in the scripts where noted. It's a best practice to insert the private key using an environment variable instead of revealing it in the code with a string literal.
- Replace the HTTP provider in the `web3` constructors in the scripts in `json-rpc-examples/`. Replace it using the string provided by the "Endpoint" selector in your Infura project dashboard. The local test net provider is `http://127.0.0.1:8545`.
- Next, replace the contract addresses in the scripts with the most recent ones. You can find Compound's cToken contract addresses for each network on this page: [https://compound.finance/developers#networks](https://compound.finance/developers#networks). The DAI contract address can be found in the Maker DAO website change logs [https://changelog.makerdao.com/](https://changelog.makerdao.com/).

## Supplying Assets via Solidity Smart Contracts
Running these scripts will give your contract address **cETH** and **cDAI**. These are ERC20 Tokens that can be **used to redeem an ever-increasing amount of the underlying asset**. The cToken exchange rate **increases every Ethereum block**, they can be transferred, and can be used to redeem at any time.

### Localhost Test Net
- Run your local test net in a second command line window **using the command above**.
- If using DAI (an ERC20 token example), you need to **first** mint some for your wallet using `node mint-testnet-dai.js`. You may need to update the DAI main net contract address and the `MCD_JOIN_DAI` address in the script. This changes periodically as DAI is improved (see **Minting Test DAI** section below for updating).
- `node compile-smart-contracts.js` This will compile the Solidity code in `solidity-examples/`. The build output is written to `.build/`.
- `node deploy-smart-contracts.js`.
- `cd solidity-examples/`
- `node supply-eth-via-solidity.js` To supply ETH.
- `node supply-erc20-via-solidity.js` To supply some DAI. The same code can be used for any other [ERC20 token that Compound supports](https://compound.finance/markets?ref=github&user=ajb413&repo=compound-supply-examples).

### Public Test Net or Main Net
- Make sure you have a wallet with ETH for the Ethereum network you plan to interface with.
- Insert the private key of your wallet in the scripts where noted. It's a best practice to insert the private key using an environment variable instead of revealing it in the code with a string literal.
- Replace the HTTP provider in the `web3` constructors in the scripts in `json-rpc-examples/`. Replace it using the string provided by the "Endpoint" selector in your Infura project dashboard. The local test net provider is `http://127.0.0.1:8545`.
- Next, replace the contract addresses in the JS scripts with the most recent ones. You can find Compound's cToken contract addresses for each network on this page: [https://compound.finance/developers#networks](https://compound.finance/developers#networks). The DAI contract address can be found in the Maker DAO website change logs [https://changelog.makerdao.com/](https://changelog.makerdao.com/).

## Output Examples

**Supply ETH via JSON RPC**
```
node supply-eth-via-json-rpc.js
Sending ETH to the Compound Protocol...
cETH "Mint" operation successful.
ETH supplied to the Compound Protocol: 0.999999999884479961
My wallet's cETH Token Balance: 49.97183529
```

**Supply ERC20 via JSON RPC**
```
node supply-erc20-via-json-rpc.js
DAI contract "Approve" operation successful.
Sending DAI to the Compound Protocol...
cDAI "Mint" operation successful.
DAI supplied to the Compound Protocol: 9.999999999992957452
My wallet's cDAI Token Balance: 495.96058738
```

**Supply ETH via Solidity**
```
node supply-eth-via-solidity.js
Supplied ETH to Compound via MyContract
ETH supplied to the Compound Protocol: 0.999999999998500226
MyContract's cETH Token Balance: 49.97183528
```

**Supply ERC20 Token via Solidity**
```
node supply-erc20-via-solidity.js
Supplied DAI to Compound via MyContract
DAI supplied to the Compound Protocol: 0.999999999985809247
MyContract's cDAI Token Balance: 49.9718367
```

## Minting Test DAI
To mint some DAI for your test network, you must use the **Join DAI** address. This can be unlocked when running Ganache CLI. You'll need to update the Join DAI address and the contract address each time the DAI contracts are updated. 

The contract address can be found at [https://changelog.makerdao.com/](https://changelog.makerdao.com/).

- Click the latest production release.
- Click contract addresses.
- The main net DAI contract address is in the JSON value of the `MCD_DAI` key.
- The Join DAI address is in the `MCD_JOIN_DAI` key.

Once you're certain you have the latest DAI and Join DAI address:

- Run Ganache CLI using the command above with `-u` and the Join DAI address.
- Paste the new addresses in `mint-testnet-dai.js` and save.
- Run `node mint-testnet-dai.js`.
