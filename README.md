## What does this repo contain?

* `contracts/*.fc` - Smart contracts for TON blockchain written in [FunC](https://ton.org/docs/#/func) language
* `test/*.spec.ts` - Test suite for the contracts in TypeScript running on [Mocha](https://mochajs.org/) test runner
* `build/_build.ts` - Build script to compile the FunC code to [Fift](https://ton-blockchain.github.io/docs/fiftbase.pdf) and [TVM](https://ton-blockchain.github.io/docs/tvm.pdf) opcodes
* `build/_deploy.ts` - Deploy script to deploy the compiled code to TON mainnet (or testnet)
* `build/_setup.ts` - Setup script to install build dependencies (used primarily for Glitch.com support)

## Dependencies and requirements

To setup your local machine for development, please make sure you have the following:

* A modern version of Node.js (version 16.15.0 or later)
  * Installation instructions can be found [here](https://nodejs.org/)
  * Run in terminal `node -v` to verify your installation, the project was tested on `v17.3.0`
* The `func` CLI tool (FunC compiler)
  * Installation instructions can be found [here](https://github.com/ton-defi-org/ton-binaries)
  * Run in terminal `func -V` to verify your installation
* The `fift` CLI tool
  * Installation instructions can be found [here](https://github.com/ton-defi-org/ton-binaries)
  * Don't forget to set the `FIFTPATH` env variable as part of the installation above
  * Run in terminal `fift -V` and `fift` to verify your installation
* A decent IDE with FunC and TypeScript support
  * We recommend using [Visual Studio Code](https://code.visualstudio.com/) with the [FunC plugin](https://marketplace.visualstudio.com/items?itemName=tonwhales.func-vscode) installed

Once your local machine is ready, install the project:

* Git clone the repo locally 
* In the root repo dir, run in terminal `npm install`

## Development instructions

* Write code
  * FunC contracts are located in `contracts/*.fc`
    * Standalone root contracts are located in `contracts/*.fc`
    * Shared imports (when breaking code to multiple files) are in `contracts/imports/*.fc`
    * Contract-specific imports that aren't shared are in `contracts/imports/mycontract/*.fc`
  * Each contract may have optional but recommended auxiliary files:
    * [TL-B](https://ton.org/docs/#/overviews/TL-B) file defining the encoding of its data and message ops in `contracts/mycontract.tld`
    * TypeScript file that implements the encoding of its data and message ops in `contracts/mycontract.ts`
  * Tests in TypeScript are located in `test/*.spec.ts`

* Build
  * In the root repo dir, run in terminal `npm run build`
  * Compilation errors will appear on screen
  * Resulting build artifacts include:
    * `mycontract.fif` - Fift file result of compilation (not very useful by itself)
    * `mycontract.compiled.json` - the binary code cell of the compiled contract (for deployment). Saved in a hex format within a json file to support webapp imports

* Test
  * In the root repo dir, run in terminal `npm run test`
  * Don't forget to build (or rebuild) before running tests
  * Tests are running inside Node.js by running TVM in web-assembly using `ton-contract-executor`

* Deploy
  * Make sure all contracts are built and your setup is ready to deploy:
    * Each contract to deploy should have a script `build/mycontract.deploy.ts` to return its init data cell
    * The deployment wallet is configured in `.env` (created automatically if not exists), with contents:<br>
      `DEPLOYER_MNEMONIC="mad nation chief flavor ..."` (24 secret words)
  * To deploy to mainnet (production), run in terminal `npm run deploy`
    * To deploy to testnet instead (where TON coins are free), run `npm run deploy:testnet`
    * Follow the on-screen instructions of the deploy script
  
# License
MIT
