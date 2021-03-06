# ethereum-intro
Intro to Ethereum Virtual Machine &amp; Smart Contracts from the ground up

# Note: All instructions for macOS.
Differences for other platforms are likely rather small.

main changes would be:
* different data dir in local_geth_connect.js
* other package manager instead of brew

# Setup

* Install nodejs and npm

### Clone this repo
```
git clone git@github.com:ojotoxy/ethereum-intro.git
cd ethereum-intro
npm install
```

* Install geth & solc (ethereum node & solidity compiler)
  - https://github.com/ethereum/go-ethereum/wiki/Building-Ethereum
  
```bash
brew tap ethereum/ethereum
brew install ethereum
brew install solidity
```


# Other Tools
* Remix Solidity IDE https://remix.ethereum.org/
* Truffle framework - tooling / package manager for ethereum contract dev https://truffleframework.com 
* Etherscan - view and interact with ethereum blockchains
* Metamask - interact with ethereum blockchain in a browser (Etherscan can use it)

 
### Add some static nodes to help with ethereum network connectivity (instructions for mac os):
(list from https://gist.github.com/rfikki/e2a8c47f4460668557b1e3ec8bae9c11#file-lightclient-peers-mainnet-latest-txt)

cat > ~/Library/Ethereum/geth/static-nodes.json

add content:
```
["enode://3afdfd40713a8b188a94e4c7a9ddc61bc6ef176c3abbb13d1dd35eb367725b95329a7570039044dbffa49c50d4aa65f0a1f99ee68e46b8e2f09100d11d4fc85a@31.17.230.132:30303", 
"enode://7578d1e6d6e256272ae3ff6680d7c2a6b72233231a10f15180a8fcdb2425692c81542a3e7255a804ceda8b081987d007e4e30f9a9e893107b344d7b9b07b11f1@50.250.156.59:30303", 
"enode://4c2b5c5d9503b7f4e76a551c827f19200f7f9ebb62f2cb5078c352de1e8d4d1006efa8fc143f9ccf2c8fd85836198dc1c69729dfa1c54d63f5d1d57fd8781bf8@62.151.178.212:30303", 
"enode://8fcd039bb514ccac1f207d9b23efbea79a1ba9ed559768109b9b3fc9f7f89cfc3a6cd3e11ec1d92a93bdbfe2322e43f3bb3d9519530e8b503c92294116c38c32@108.232.148.241:30303", 
"enode://bfad505cbb2bde72e161a7cff044d66d20ceb85c8a61047b50037881f289bd2dcc064189ade2077daddd5b20fd2fc6dee7208f227ae2a34361bf51751d225e8e@51.15.220.91:30303", 
"enode://1d70e87a2ee28a2762f1b2cd56f1b9134824a84264030539bba297f67a5bc9ec7ae3016b5f900dc59b1c27b4e258a63fc282a37b2dd6e25a8377473530513394@208.88.169.151:30303", 
"enode://ea1737bf696928b4b686a2ccf61a6f2295d149281a80b0d83a9bce242e7bb084434c0837a2002d4cc2840663571ecf3e45517545499c466e4373c69951d090fe@163.172.181.92:30303", 
"enode://0f740f471e876020566c2ce331c81b4128b9a18f636b1d4757c4eaea7f077f4b15597a743f163280293b0a7e35092064be11c4ec199b9905541852a36be9004b@206.221.178.149:30303", 
"enode://03f178d5d4511937933b50b7af683b467abaef8cfc5f7c2c9b271f61e228578ae192aaafc7f0d8035dfa994e734c2c2f72c229e383706be2f4fa43efbe9f94f4@163.172.149.200:30303", 
"enode://242b68a4e37b4478c46901c3512315f36bd1aa513566d1f061939b202258b55d63d66367bc5807e62ec03ae673bead9a351846e3f23284ce79537ff7afa65615@34.201.26.61:30303"]
```

### Create account

```
geth account new
```

Test net in this case is rinkeby.
New TestNet Account:
```
geth --rinkeby account new
```

### Get some ether

For main net this means real $$$.

For Rinkeby Test net, you can get some at the faucet:
https://faucet.rinkeby.io/

### Start geth daemon
Leave this running in a separate terminal.
Main net:
```
geth --syncmode light --unlock 0
```
*** You need to enter the password to unlock here, otherwise no transactions can be sent... The prompt may not be obvious.

Testnet:
```
geth --rinkeby --syncmode light --unlock 0
```

### Start JS console
In new terminal, with geth daemon running... Note this is not Node.js and the environment is quite different.
Mainnet:
```bash
geth attach
```
Testnet:
```
geth --rinkeby attach
```

For a similar Node.js environment:
```
CHAIN=rinkeby node

Paste in:
var { web3, Web3 } = require('./local_geth_connect');
var eth = web3.eth;

```


# Run Basic Scripts

### Get balance
```bash
node get_balance.js
```

### Send from one account to another
(Create a second account: geth account new)
This will send .001 eth from account 0 to account 1.
```bash
CHAIN=rinkeby node send_basic_transaction.js
```


# Compile a solidity file
```bash
./compile uint256hashtable.sol
```


### View EVM disassembly instead
```bash
solc --optimize --asm uint256hashtable.sol
```


# Publish a contract
These will create the contract from account 0, calling solc for you internally...
```bash
CHAIN=rinkeby node deploy_contract.js uint256hashtable.sol Uint256HashTable
CHAIN=rinkeby node deploy_contract.js erc20token.sol ERC20Token '["1000000000000","CoinyCoin","1","CC"]'
CHAIN=rinkeby node deploy_contract.js escrowSimple.sol EscrowSimple '["seller address","arbiter address"]'
CHAIN=rinkeby node deploy_contract.js timeLock.sol TimeLock
CHAIN=rinkeby node deploy_contract.js tokenfactory.sol TokenFactory


```

# Basic Interaction with a contract
This address is for Uint256HashTable ... 
```bash
CHAIN=rinkeby node call_contract_method.js 0x756909e66153fE24D584d2B7399C954a41f71781
```
