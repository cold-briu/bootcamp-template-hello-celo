# TODO: Hello World on Celo Alfajores with Foundry

## Prerequisites Check
- [x] Verify Linux/macOS terminal environment
- [ ] Confirm basic knowledge of Solidity and blockchain concepts
- [ ] Ensure web browser access for faucet

## Installation Steps
- [x] 1. Install Foundry
  - [x] Download foundry installer `foundryup`
  - [x] Install forge, cast, anvil, chisel
  - [x] Install the latest nightly release

- [x] 2. Install Celo CLI
  - [x] Install @celo/celocli globally via npm

- [x] 3. Configure Celo CLI
  - [x] Set CLI node URL to Alfajores
  - [x] Verify configuration

## Wallet and Account Setup
- [x] 4. Wallet Setup
  - [x] Create new wallet using celocli account:new
  - [x] Export private key and address to environment variables
  - [x] Import private key into Foundry's keystore
  - [x] Create .env file with account address and node URL

- [x] 5. Fund via Alfajores Faucet
  - [x] Open Celo faucet in browser (https://alfajores.celo.org/faucet)
  - [x] Request test CELO and cUSD tokens
  - [x] Verify account balance (0.1 CELO available - sufficient for deployment)

## Project Development
- [x] 6. Initialize Project
  - [x] Initialize new Foundry project (hello-celo)
  - [x] Navigate to project directory
  - [x] Clean up default directories (test, script, src)

- [x] 7. Create HelloWorld Contract
  - [x] Create src/HelloWorld.sol file
  - [x] Implement HelloWorld contract with greet function

- [x] 8. Build
  - [x] Compile contract using forge build

- [x] 9. Testing
  - [x] Create test/HelloWorld.t.sol file
  - [x] Implement test cases for HelloWorld contract
  - [x] Run tests using forge test

- [x] 10. Deploy
  - [x] Create script/Deploy.s.sol file
  - [x] Implement deployment script
  - [x] Deploy contract to Alfajores testnet
  - [x] Save deployed contract address to .env 

- [x] 11. Interact
  - [x] Load environment variables
  - [x] Call greet function using cast
  - [x] Verify contract interaction works correctly ("Hello, Celo 😎!" received) 

## Notes
- Each step should be completed before proceeding to the next
- Environment variables need to be properly set
- Contract deployment requires sufficient test tokens
- All commands should be run from the hello-celo project directory 