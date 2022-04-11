# Level 5 - Token

### Objective

The objective of this level is as follows:

1. Increase the number of tokens assigned to your address

### Contract

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

contract Token {

  mapping(address => uint) balances;
  uint public totalSupply;

  constructor(uint _initialSupply) public {
    balances[msg.sender] = totalSupply = _initialSupply;
  }

  function transfer(address _to, uint _value) public returns (bool) {
    require(balances[msg.sender] - _value >= 0);
    balances[msg.sender] -= _value;
    balances[_to] += _value;
    return true;
  }

  function balanceOf(address _owner) public view returns (uint balance) {
    return balances[_owner];
  }
}
```

### Solution

This level is an example of an arithmetic underflow/overflow - an issue which was patched in `v0.8.0` in Solidity.

Arthimetic underflow occurs when subtraction of integers outputs an integer below the minimum value for an n-bit integer. For example, the minimum value of a 2-bit integer is `00`. Underflow occurs as follows:

```
00 - 1 = 11
```

Arthimetic overflow occurs when addition of integers outputs an integer above the maximum value for an n-bit integer. For example, the maximum value of a 2-bit integer is `11`. Overflow occurs as follows:

```
11 + 1 = 00
```

For those with a mathematical background, arithmetic overflow/underflow can be understood to be conceptually similar to modular arithmetic.

In this contract, we can take advantage of arithmetic underflow to assign ourselves a whole lot of tokens (precisely `2^256-1` tokens):

```
await contract.transfer('0x0000000000000000000000000000000000000000', 21)
```

We can verify that we recieved these tokens:

```
await contract.balanceOf(player).then(v => v.toString())

```

Done!
