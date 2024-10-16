## Create a new directory e.g.
```bash
mkdir $HOME/unichain-testnet && cd $HOME/unichain-testnet
```

## Install foundry
```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
source $HOME/.bashrc
```

## Ensure git is set up if not done so before
```bash
git init
touch .gitignore && echo "cache/" >> .gitignore && echo "out/" >> .gitignore
git add .
git commit -m "Initial commit"
```

## An ERC-20 implementation from solmate will be needed
```bash
forge install transmissions11/solmate
```

## Create your contract file and edit in the contract code
```bash
mkdir src
nano ./src/simple.sol
```
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.15;

import {ERC20} from "solmate/tokens/ERC20.sol";

contract SimpleToken is ERC20 {
    constructor() ERC20("SimpleToken", "SMPL", 18) {
        _mint(msg.sender, 1_000_000e18);
    }
}
```
- Press "ctrl + x" to save.
- Press "y" to confirm.
- Press "enter" to exit.

## Create your `foundry.toml` file and edit in the following code:
```bash
nano ./foundry.toml
```
```ini
[profile.default]
src = "src"
out = "out"
libs = ["lib"]
remappings = ["solmate/=lib/solmate/src/"]

[rpc_endpoints]
unichain = "https://sepolia.unichain.org"
```
- Press "ctrl + x" to save.
- Press "y" to confirm.
- Press "enter" to exit.


## Compile and deploy
```bash
forge build
forge create src/simple.sol:SimpleToken --rpc-url unichain --private-key <enter_your_private_key_here>
```

## Verify and publish contract
- Copy the contract address. (It's under the `Deployed to:`.)
- Open [unichain testnet explorer](https://unichain-sepolia.blockscout.com/), paste the contract address in the search box and press "enter".
- 
- Select the "contract" column.
- Click on "verify and publish".
- Under "Contract license", select "MIT License".
- Under "Verification method (compiler type)", select "Solidity (Foundry)".
- Copy the generated `foundry` command e.g.:
```bash
forge verify-contract \
  --rpc-url https://sepolia.unichain.org \
  --verifier blockscout \
  --verifier-url 'https://unichain-sepolia.blockscout.com/api/' \
  0xf...212 \
  [contractFile]:[contractName]
```
- Replace `[contractFile]:[contractName]` with `src/simple.sol:SimpleToken`.
- Paste the command in your terminal and run.
