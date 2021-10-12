![1500x500-2](https://user-images.githubusercontent.com/3911039/134815047-cc3f2ae5-283c-4d3a-8b16-711c1c3b1166.jpeg)

# MarsGenesis Martians ERC721 Token <img src="https://img.shields.io/badge/Solidity-0.8.0-green" /> <img src="https://img.shields.io/badge/Last Update-Sept'2021-yellow" /> 

## ⭐️ What is it ?
ERC721 Token with:
- 10,000 unique collectible 3D Martians, with proof of ownership stored on the Ethereum blockchain.
- Each card represents a unique 3D Martian.

## 👾 How to deploy the contracts locally
Install the following in your machine:

### 1. Truffle [▶️](https://www.trufflesuite.com)
Truffle is the most popular development framework for Ethereum. 
Requires you to install [nodejs](https://nodejs.org/en/) first. 
Then, run:

```sh
$ npm install truffle -g
```
--- 

### 2. Ganache [▶️](https://www.trufflesuite.com/ganache)

Ganache gives you a local blockchain that you can interact with using truffle commands. It is like having the ETH network inside your own machine! Perfect for local test and development.

Download it from [here](https://www.trufflesuite.com/ganache)

---

### 3. Clone the project
Clone the project to your favorite local directory:

```sh
$ git clone https://github.com/crypto-martians-contract.git
```

Then go into the project main folder:

```sh
$ cd crypto-martians-contract/
```

Then install the dependencies listed in `package.json` with:

```sh
$ npm install
```
---

### 4. Compile and Migrate the contracts
Now you need to compile the contracts of the project and migrate them to generate the ABIs. First, run:

```sh
$ truffle compile
```

Then open the Ganache app. You can use the `Quickstart` method. One click and ready:

<img align="center" width="700" alt="ganache" src="https://user-images.githubusercontent.com/3911039/113735040-c322f700-96f3-11eb-8583-b964fbf1ee0a.png">

With the local blockchain ready (you should see a list with 10 accounts or so, each with 100ETH balance), migrate the contracts with:

```sh
$ truffle migrate
```

This will deploy the contracts in your local blockchain. You should see a summary with the cost, and the balance of your first account on Ganache should have decreased from the initial 100 ETH.

```console
Summary
=======
> Total deployments:   3
> Final cost:          0.17415532 ETH
```

<details><summary><b>If you run into any issue at this point, check this⚠️</b></summary>
In the `truffle-config.js` configuration file we specify the network details for development. This details should match your Ganache network details. Double check them and update the accordingly
    
```json
module.exports = {
  networks: {
    development: {
    host: "127.0.0.1",     
    port: 7545,            
    network_id: "*",
    gas: 12000000
    }
  }
}
```
</details>

---

### 5. Test that the contract is deployed locally and responds to commands
You can use truffle console to send commands to the smart contract. Lets try it out so we can be sure that the contracts are deployed.

Start the truffle console:

```sh
$ truffle console
```

Now, let's grab the deployed `MarsGenesisMartiansCore` contract, call its method `totalSupply()` and check that the call returns `0`:

```sh
truffle(development)> contract = await MarsGenesisMartiansCore.deployed();
undefined
truffle(development)> supply = await contract.totalSupply.call();
undefined
truffle(development)> supply.toNumber()
0
```

We can do the same check with the auction contract `MarsGenesisMartiansAuction`. Lets call a method on it and check that it works:

```sh
truffle(development)> contractAuction = await MarsGenesisMartiansAuction.deployed();
undefined
truffle(development)> isForSale = await contractAuction.martianIdIsForSale.call(0)
undefined
truffle(development)> isForSale
false
```

So far so good! Contracts are deployed locally into your Ganache blockchain.

---

### 6. Connecting your dApp to Ganache with MetaMask

In order to connect your dApp (web) to your local, you will need also a ETH wallet. The easy option is to use `MetaMask`. Download it from [here](https://metamask.io/)

With `MetaMask`, you can connect it to a local network (using the Custom network option) and it will appear in your network selector:

<img width="364" alt="Screenshot 2021-04-06 at 16 42 18" src="https://user-images.githubusercontent.com/3911039/113739059-590c5100-96f7-11eb-8241-f60d7c0e71ab.png">

‼️Make sure you select the localhost / ganache network after creating it‼️

Finally, you will need to configure your web3 dApp to connect to the local blockchain and use MetaMask accounts. This is a sample code, not tested yet:

<details><summary><b>See example code for web3📝</b></summary>
    
```javascript
App = {
loading: false,
contracts: {},
load: async () => {
  await App.loadWeb3();
  await App.loadAccount();
  await App.loadContract();
  await App.render();
},
loadWeb3: async () => {
  if (typeof web3 !== "undefined") {
    App.web3Provider = web3.currentProvider;
    web3 = new Web3(web3.currentProvider);
  } else {
    window.alert("Please connect to Metamask.");
  }
if (window.ethereum) {
  window.web3 = new Web3(ethereum);
  try {
    await ethereum.enable();
    web3.eth.sendTransaction({});
  } catch (error) {}
} else if (window.web3) {
  App.web3Provider = web3.currentProvider;
  window.web3 = new Web3(web3.currentProvider);
  web3.eth.sendTransaction({});
} else {
  console.log("Non-Ethereum browser detected. You should consider   trying MetaMask!");
}
},
loadAccount: async () => {
  // Set the current blockchain account
  App.account = web3.eth.accounts[0];
},
loadContract: async () => {
  // THIS PART WILL DIFFER FOR YOUR PROJECT
  const counter = await $.getJSON("Counter.json");
  App.contracts.counter = TruffleContract(counter);
  App.contracts.counter.setProvider(App.web3Provider);
  App.counter = await App.contracts.counter.deployed();
},
render: async () => {
  if (App.loading) {
    return;
  }
  $("#account").html(App.account);
  await App.AddTransaction();
},
};
$(() => {
  $(window).load(() => {
    App.load();
  });
});
```

</details>

---

## 🚀 TO THE MOON!

![giphy](https://user-images.githubusercontent.com/3911039/113741816-ee104980-96f9-11eb-9eb7-666ffcfad451.gif)



