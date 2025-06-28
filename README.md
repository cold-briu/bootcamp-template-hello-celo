# Hello World on Celo Alfajores with Foundry

A complete guide to deploying your first smart contract on Celo's Alfajores testnet using Foundry.

### Prerequisites

* Linux/macOS terminal
* Basic knowledge of Solidity and blockchain concepts
* A web browser for accessing the faucet

### Process Overview

1. [Install Foundry](#install-foundry)
2. [Install Celo CLI](#install-celo-cli)
3. [Configure Celo CLI](#configure-celo-cli)
4. [Wallet Setup](#wallet-setup)
5. [Fund via Alfajores Faucet](#fund-via-alfajores-faucet)
6. [Initialize Project](#initialize-project)
7. [Create HelloWorld Contract](#create-helloworld-contract)
8. [Build](#build)
9. [Testing](#testing)
10. [Deploy](#deploy)
11. [Interact](#interact)

### Step-by-Step Instructions

#### 1. Install Foundry

Install and configure Foundry CLI tools for contract development.

```bash
# Download foundry installer `foundryup`
curl -L https://foundry.paradigm.xyz | bash
# Install forge, cast, anvil, chisel
foundryup
# Install the latest nightly release
foundryup -i nightly
```

#### 2. Install Celo CLI

Install Celo command-line interface for interacting with the network.

```bash
npm install -g @celo/celocli
```

#### 3. Configure Celo CLI

Set your CLI node URL to Alfajores.

```bash
celocli config:set --node=https://alfajores-forno.celo-testnet.org
celocli config:get
```

#### 4. Wallet Setup

Create your wallet and set environment variables.

```bash
celocli account:new
```

Export your private key and address:

```bash
export YOUR_PRIVATE_KEY=
export YOUR_ADDRESS=
```

Encrypt and import your private key into Foundry’s keystore:

```bash
cast wallet import my-wallet-hello-celo --private-key $YOUR_PRIVATE_KEY
```

Choose a secure password; Foundry will decrypt at runtime.

Create your .env file for public config:

```
# .env
CELO_ACCOUNT_ADDRESS=$YOUR_ADDRESS
CELO_NODE_URL=https://alfajores-forno.celo-testnet.org
```

#### 5. Fund via Alfajores Faucet

> Get test CELO and cUSD to deploy your contract.

- Open the Celo faucet in your browser 

```
https://alfajores.celo.org/faucet
```

- Request test tokens from the faucet:

Paste your address in the faucet UI and request CELO and cUSD

- Check your account balance:

```bash
celocli account:balance $CELO_ACCOUNT_ADDRESS --node $CELO_NODE_URL
```

#### 6. Initialize Project

Scaffold a new Foundry project for your contract.

```bash
# Initialize a new Foundry project
forge init hello-celo
cd hello-celo
rm -rf test script src
```

#### 7. Create HelloWorld Contract

Write the HelloWorld Solidity contract.
Create a contract file in `./src/HelloWorld.sol`.

```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.0;

contract HelloWorld {
    address public owner;

    constructor() {
        owner = msg.sender;
    }

    function greet(string memory name) external pure returns (string memory) {
        return string(abi.encodePacked("Hello, ", name, "!"));
    }
}
```

#### 8. Build

Compile your contract code.

```bash
forge build
```

#### 9. Testing

Verify contract logic with Forge tests.
Create a test file in `test/HelloWorld.t.sol`.

```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.0;

import "forge-std/Test.sol";
import "../src/HelloWorld.sol";

contract HelloWorldTest is Test {
    HelloWorld public hello;

    function setUp() public {
        hello = new HelloWorld();
    }

    function testGreet() public {
        string memory out = hello.greet("Alice");
        assertEq(out, "Hello, Alice!");
    }
}
```

Run all tests:

```bash
forge test
```

#### 10. Deploy

Deploy the contract to Alfajores using a Foundry script.
Create a file in `./script/Deploy.s.sol`:

```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.0;

import "forge-std/Script.sol";
import "../src/HelloWorld.sol";

contract DeployScript is Script {
    function run() external {
        vm.startBroadcast();
        new HelloWorld();
        vm.stopBroadcast();
    }
}
```

Deploy to Alfajores:

```bash
forge script ./script/Deploy.s.sol \
  --rpc-url $CELO_NODE_URL \
  --broadcast --account wallet-hello-celo 
```

Save the deployed address:

```bash
echo "CELO_CONTRACT_ADDRESS=<deployed_address>" >> .env
```


#### 11. Interact

Call contract functions to confirm behavior

Load your environment variables:

```bash
source .env
```

Call `greet` via cast: 

```bash
cast call --rpc-url $CELO_NODE_URL $CELO_CONTRACT_ADDRESS   "greet(string)(string)" "Celo 😎"
```

## 📚 Additional Resources

* [Celo Documentation](https://docs.celo.org/)
* [Celo CLI Documentation](https://docs.celo.org/cli)
* [Foundry Book](https://book.getfoundry.sh/)
* [Alfajores Faucet](https://alfajores.celo.org/faucet)
* [CeloScan](https://celoscan.io/)

## 🤝 Contributing

This is a template project for the Celo Colombia Bootcamp. Feel free to modify and extend it for your own projects.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
