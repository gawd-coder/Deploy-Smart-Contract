# Deploy-Smart-Contract

Deploying a smart contract on the Ropsten test network using a virtual wallet (Metamask), Solidity, Hardhat, and Alchemy 

Making a request on ethereum using Alchemy as we cant afford to spin up our own node. Create a API key on Alchemy on Ropsten network
 
Change to Ropsten on your test Ethereum account from metamask and make sure your have Ropsten ETH in wallet

Inside the project folder, we’ll use npm init to initialize the project. This will create a package.json for us

Hardhat is a development environment to compile, deploy, test, and debug your Ethereum software. It helps developers when building smart contracts and dApps locally before deploying to the live chain.

Inside the project run:
```
npm install --save-dev hardhat
```

## Create Hardhat project:

To create your Hardhat project run ```npx hardhat``` in your project folder:

<img width="628" alt="Screenshot 2021-12-30 at 11 56 08 AM" src="https://user-images.githubusercontent.com/57283161/147727081-79e91e48-b311-4eda-9020-d53db2a36f25.png">


This will generate a hardhat.config.js file for us which is where we’ll specify all of the set up for our project

1. contracts/ is where we’ll keep our smart contract code file
2. scripts/ is where we’ll keep scripts to deploy and interact with our contract

## Connect Metamask & Alchemy to your project
Every transaction sent from your virtual wallet requires a signature using your unique private key. To provide our program with this permission, we can safely store our private key (and Alchemy API key) in an environment file.

Install the dotenv package in your project directory:
```
npm install dotenv --save
```

Then, create a .env file in the root directory of our project, and add your Metamask private key and HTTP Alchemy API URL to it.

To actually connect these to our code, we’ll reference these variables in our hardhat.config.js

## Install Ethers.js
Ethers.js is a library that makes it easier to interact and make requests to Ethreum by wrapping standard JSON-RPC methods with more user friendly methods.

Hardhat makes it super easy to integrate Plugins for additional tooling and extended functionality. We’ll be taking advantage of the Ethers plugin for contract deployment (Ethers.js has some super clean contract deployment methods).

In your project directory type:

```npm install --save-dev @nomiclabs/hardhat-ethers "ethers@^5.0.0"```

## Update hardhat.config.js
We’ve added several dependencies and plugins so far, now we need to update hardhat.config.js so that our project knows about all of them.

```npx hardhat compile``` to make sure that everything is working so far

<img width="622" alt="Screenshot 2021-12-30 at 11 58 29 AM" src="https://user-images.githubusercontent.com/57283161/147727207-eab9ea86-f000-4231-aea6-1e4970d74ab7.png">

## Write our deploy script

Now that our contract is written and our configuration file is good to go, it’s time to write our contract deploy script.

A ContractFactory in ethers.js is an abstraction used to deploy new smart contracts, so HelloWorld here is a factory for instances of our hello world contract. When using the hardhat-ethers plugin ContractFactory and Contract instances are connected to the first signer by default.

```const hello_world = await HelloWorld.deploy();```

Calling deploy() on a ContractFactory will start the deployment, and return a Promise that resolves to a Contract. This is the object that has a method for each of our smart contract functions.

Deploying our contract: 

```npx hardhat run scripts/deploy.js --network ropsten```

<img width="599" alt="Screenshot 2021-12-30 at 11 59 32 AM" src="https://user-images.githubusercontent.com/57283161/147727256-a1250002-5862-494d-ab2f-e8f23bf837bb.png">

If we go to the Ropsten etherscan and search for our contract address we should able to see that it has been deployed successfully.

<img width="622" alt="Screenshot 2021-12-30 at 12 05 45 PM" src="https://user-images.githubusercontent.com/57283161/147727620-44ac811b-4a0d-44ca-9474-68c01898f4c8.png">

<img width="622" alt="Screenshot 2021-12-30 at 12 06 09 PM" src="https://user-images.githubusercontent.com/57283161/147727647-82fda0ab-36c4-4d43-880b-e4933cd348c6.png">

Here you’ll see a handful of JSON-RPC calls that Hardhat/Ethers made under the hood for us when we called the .deploy() function. Two important ones to call out here are eth_sendRawTransaction, which is the request to actually write our contract onto the Ropsten chain, and eth_getTransactionByHash which is a request to read information about our transaction given the hash (a typical pattern when sending transactions). 

<img width="622" alt="Screenshot 2021-12-30 at 12 06 35 PM" src="https://user-images.githubusercontent.com/57283161/147727685-1e3e69a9-1748-4aff-a35a-fe039f46d3e4.png">
<img width="623" alt="Screenshot 2021-12-30 at 12 06 43 PM" src="https://user-images.githubusercontent.com/57283161/147727701-96e90f9e-a89d-4bba-ab0c-d198d59c3a17.png">
