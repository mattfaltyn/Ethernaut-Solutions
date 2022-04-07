# Level 2 - Fallout

### Objective

The objective of this level is as follows:

1. Claim ownership of the contract below to complete this level.

### Contract

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

import '@openzeppelin/contracts/math/SafeMath.sol';

contract Fallout {

  using SafeMath for uint256;
  mapping (address => uint) allocations;
  address payable public owner;


  /* constructor */
  function Fal1out() public payable {
    owner = msg.sender;
    allocations[owner] = msg.value;
  }

  modifier onlyOwner {
            require(
                msg.sender == owner,
                "caller is not the owner"
            );
            _;
        }

  function allocate() public payable {
    allocations[msg.sender] = allocations[msg.sender].add(msg.value);
  }

  function sendAllocation(address payable allocator) public {
    require(allocations[allocator] > 0);
    allocator.transfer(allocations[allocator]);
  }

  function collectAllocations() public onlyOwner {
    msg.sender.transfer(address(this).balance);
  }

  function allocatorBalance(address allocator) public view returns (uint) {
    return allocations[allocator];
  }
}
```

### Solution

Inspecting the contract, we do not see any immediate ways to switch the ownership via the methods. Instead, we see two issues:

1. The constructor is not initialized with the `constructor` declaration
2. `Fallout()` is misspelled as `Fal1out`

The spelling error is largely a red herring (the contract would compile if it was spelled correctly).

Since the constructor is not initialized correctly, it is treated as method. We can claim ownership of the contract by calling the function:

```
await contract.Fal1out()
```

We can verify that we have ownership by comparing our address to the contract owner's address:

```
await contract.owner() === player
```

Done!
