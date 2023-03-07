# Solidity-Automated Auditing tools commands list

## Solidity Audit Report
## Hardhat
`npx hardhat compile` Compile the contract code
`npx hardhat test` Run the test suite
`npx hardhat coverage` Generate code coverage report
`npx hardhat flatten` Flatten the contract code into a single file
`npx hardhat run scripts/<script-name>.js` Run a custom script

## Slither
`slither <file-name>` Analyze the contract code for vulnerabilities
`slither --exclude <pattern> <file-name>` Exclude certain files or functions from analysis
`slither --list-snakes` List all available detectors
`slither --solc <path-to-solc> <file-name>` Specify the path to the Solidity compiler to use
`slither --json <file-name>` Generate a JSON report
`slither --detect all --disassemble <file-name>` Disassemble the bytecode of a contract and perform an analysis
`slither --detect all --bytecode <bytecode>` Analyze a bytecode string directly (without a Solidity source file)

## MythX
`mythx analyze <file-name>` Analyze the contract code for vulnerabilities
`mythx version` Display the current version of MythX CLI
`mythx status <job-id>` Check the status of a submitted analysis job
`mythx report <job-id>` Generate a report for a completed analysis job
`mythx list-analyses` List all previous analysis jobs

## Mythril
`myth analyze <file-name>` Analyze the contract code for vulnerabilities
`myth analyze --solv <version> <file-name>` Specify the Solidity version to use for analysis
`myth check <address>` Check a deployed contract for vulnerabilities
`myth analyze --mode full <file-name>` Perform a full analysis (may take longer)
`myth version` Display the current version of Mythril CLI
`myth analyze --disassemble <file-name>` Disassemble the bytecode of a contract and perform an analysis
`myth analyze --solv <version> --bytecode <bytecode>` Analyze a bytecode string directly (without a Solidity source file)

## Surya
`surya inheritance <file-name>` Generate an inheritance graph for the contract code
`surya graph <file-name>` Generate a control flow graph for the contract code
`surya mdreport <file-name>` Generate a Markdown report for the contract code
`surya list-detectors` List all available detectors
`surya describe-detector <detector-name>` Display information about a specific detector

## Other useful tools

## OpenZeppelin

`npm install @openzeppelin/contracts` Install the OpenZeppelin contracts library
`npx oz init` Initialize a new OpenZeppelin project
`npx oz deploy` Deploy the smart contracts to the network using OpenZeppelin's deployment tool
`npx oz send-tx` Send a transaction to a deployed contract using OpenZeppelin's console tool
`npx oz verify` Verify the source code and bytecode of a deployed contract on Etherscan using OpenZeppelin's verification tool

## More Hardhat Fun

`npx hardhat clean` Delete the artifacts and cache folders
`npx hardhat node` Start a local Hardhat node for testing
`npx hardhat run <script.js>` Run a script with Hardhat's network context
`npx hardhat help` Display the help menu

## Hardhat-Deploy

`npx hardhat deploy --network <network-name>` Deploy the smart contracts to a specific network
`npx hardhat run scripts/<script.js> --network <network-name>` Run a script with the context of a specific network
`npx hardhat export --export <file-name>.json --network <network-name>` Export the deployed contract addresses and ABIs to a JSON file

## Hardhat-Verify

`npx hardhat verify --network <network-name> <contract-address> "<constructor-args>"` 
Verify the source code and bytecode of a deployed contract on Etherscan

`npx hardhat verify --network <network-name> --contract contracts/<contract-name>.sol:<contract-address> --constructor-args "<constructor-args>"` Verify the source code and bytecode of a deployed contract on Etherscan using a specific contract file and constructor arguments

`npx hardhat verify --network <network-name> --contract contracts/<contract-name>.sol --constructor-args "<constructor-args>" <contract-address>` Verify the source code and bytecode of a deployed contract on Etherscan using a specific contract file and constructor arguments



                                                            🦋👽 Yours Truly, K42 to you 👽🦋