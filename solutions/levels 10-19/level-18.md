# Level 18 - Magic Number

### Objective

The objective of this level is as follows:

1. To solve this level, you only need to provide the Ethernaut with a Solver, a contract that responds to whatIsTheMeaningOfLife() with the right number.
2. The solver's code needs to be really tiny. Really reaaaaaallly tiny. Like freakin' really really itty-bitty tiny: 10 opcodes at most.

### Contract

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

contract MagicNum {

  address public solver;

  constructor() public {}

  function setSolver(address _solver) public {
    solver = _solver;
  }

  /*
    ____________/\\\_______/\\\\\\\\\_____
     __________/\\\\\_____/\\\///////\\\___
      ________/\\\/\\\____\///______\//\\\__
       ______/\\\/\/\\\______________/\\\/___
        ____/\\\/__\/\\\___________/\\\//_____
         __/\\\\\\\\\\\\\\\\_____/\\\//________
          _\///////////\\\//____/\\\/___________
           ___________\/\\\_____/\\\\\\\\\\\\\\\_
            ___________\///_____\///////////////__
  */
}
```

### Solution

Solution inspired by [CMICHEL](https://cmichel.io/) using HardHat:

```
function magicNumber() public {
    bytes memory bytecode = hex"600a600c600039600a6000f3602a60005260206000f3";
    bytes32 salt = 0;
    address solver;

    assembly {
        solver := create2(0, add(bytecode, 0x20), mload(bytecode), salt)
    }

    challenge.setSolver(solver);
}
```

Done!
