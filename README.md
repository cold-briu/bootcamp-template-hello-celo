# Hello World on Celo Alfajores with Foundry

A complete guide to deploying your first smart contract on Celo's Alfajores testnet using Foundry.

## 🚀 Quick Start

Follow these steps to deploy a simple "Hello World" smart contract on Celo Alfajores testnet.

### Prerequisites

* Linux/macOS terminal
* Basic knowledge of Solidity and blockchain concepts
* A web browser for accessing the faucet

### Step-by-Step Instructions

#### 1. Install Foundry

```bash
# Download foundry installer `foundryup`
curl -L https://foundry.paradigm.xyz | bash
# Install forge, cast, anvil, chisel
foundryup
# Install the latest nightly release
foundryup -i nightly
```

#### 2. Install Celo CLI

```bash
npm install -g @celo/celocli
```

#### 3. Configure Celo CLI

```bash
celocli config:set --node=https://alfajores-forno.celo-testnet.org
celocli config:get
```

#### 4. Wallet Setup

Create a new Celo account:

```bash
celocli account:new
```

```bash
export YOUR_PRIVATE_KEY=
export YOUR_ADDRESS=
```

Note the address and privateKey returned in JSON.

Encrypt and store the private key in the Foundry keystore:

```bash
cast wallet import my-wallet-hello-celo --private-key $YOUR_PRIVATE_KEY
```

Choose a secure password; Foundry will decrypt at runtime.

Create a `.env` file for public configuration:

```
CELO_ACCOUNT_ADDRESS=<$YOUR_ADDRESS>
CELO_NODE_URL=https://alfajores-forno.celo-testnet.org
```

#### 5. Fund via Alfajores Faucet

1. Open [https://alfajores.celo.org/faucet](https://alfajores.celo.org/faucet) in your browser
2. Paste your address from the previous step
3. Request test tokens (CELO and cUSD)

Verify balance:

```bash
celocli account:balance $CELO_ACCOUNT_ADDRESS --node $CELO_NODE_URL
```

#### 6. Initialize Project

Create a new Foundry project for your Hello World contract:

```bash
# Initialize a new Foundry project
forge init hello-celo
cd hello-celo
rm -rf test script src
```

#### 7. Create HelloWorld Contract

write `./src/HelloWorld.sol` with the following content:

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

```bash
forge build
```

#### 9. Testing

Create a test file in `test/`:

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

Filter by contract or test name:

````bash
forge test --match-contract HelloWorldTest --match-test testGreet
```#### 10. Deploy

Create `./script/Deploy.s.sol`:

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
````

Run:

```bash
forge script ./script/Deploy.s.sol \
  --rpc-url $CELO_NODE_URL \
  --broadcast --account wallet-hello-celo \
  --json | jq -r '.[0].transactions[0].contractAddress'
```

Note the deployed contract address and add to `.env`:

```bash
echo "CELO_CONTRACT_ADDRESS=<deployed_address>" >> .env
```

#### 11. Interact

Interact

Interact with your deployed contract via CeloScan or `cast`:

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
