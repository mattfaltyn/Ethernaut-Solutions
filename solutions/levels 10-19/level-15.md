# Level 15 - Naught Coin

### Objective

The objective of this level is as follows:

1. NaughtCoin is an ERC20 token and you're already holding all of them. The catch is that you'll only be able to transfer them after a 10 year lockout period. Can you figure out how to get them out to another address so that you can transfer them freely?
2. Complete this level by getting your token balance to 0.

### Contract

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

import '@openzeppelin/contracts/token/ERC20/ERC20.sol';

 contract NaughtCoin is ERC20 {

  // string public constant name = 'NaughtCoin';
  // string public constant symbol = '0x0';
  // uint public constant decimals = 18;
  uint public timeLock = now + 10 * 365 days;
  uint256 public INITIAL_SUPPLY;
  address public player;

  constructor(address _player)
  ERC20('NaughtCoin', '0x0')
  public {
    player = _player;
    INITIAL_SUPPLY = 1000000 * (10**uint256(decimals()));
    // _totalSupply = INITIAL_SUPPLY;
    // _balances[player] = INITIAL_SUPPLY;
    _mint(player, INITIAL_SUPPLY);
    emit Transfer(address(0), player, INITIAL_SUPPLY);
  }

  function transfer(address _to, uint256 _value) override public lockTokens returns(bool) {
    super.transfer(_to, _value);
  }

  // Prevent the initial owner from transferring tokens until the timelock has passed
  modifier lockTokens() {
    if (msg.sender == player) {
      require(now > timeLock);
      _;
    } else {
     _;
    }
  }
}
```

### Solution

Since Solidity supports multiple inheritance, it only inherits overrides from the ERC20 contract for the `transfer` function. Thus, we can use other functions to bypass the time constraint. This

First, check balance:

```
await contract.balanceOf(player)
(await contract.balanceOf(player)).toString()
```

Next, increase allowance:

```
await contract.increaseAllowance(player, "1000000000000000000000000")
```

Finally, transfer to another address:

```
await contract.transferFrom (player, "some_address" , "1000000000000000000000000")
(await contract.balanceOf(player)).toString()
```

Done!
