# Solidity 혼자 공부하기

## 1. [Introduction to Smart Contracts](https://docs.soliditylang.org/en/v0.8.23/introduction-to-smart-contracts.html#introduction-to-smart-contracts)
### 1. Simple Example
```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.4.16 <0.9.0;

contract SimpleStorage {
    uint storedData;

    function set(uint x) public {
        storedData = x;
    }

    function get() public view returns (uint) {
        return storedData;
    }
}
```

```solidity
pragma solidity >=0.4.16 <0.9.0;
```
- pragma: 솔리디티 컴파일러에게 솔리디티 버전을 알려줌
- `>=0.4.16 <0.9.0`: 0.4.16 이상 0.9.0 미만 버전의 컴파일러를 사용하겠다는 의미

```solidity
uint storedData;

function set(uint x) public {
    storedData = x;
}

function get() public view returns (uint) {
    return storedData;
}
```
- `uint storedData`: unsigned integer, 256 bits
- `function set(uint x) public`: `set`이라는 이름의 함수, `uint` 타입의 `x`라는 인자를 받음, `public`으로 선언되어 있으므로 외부에서 호출 가능
- `function get() public view returns (uint)`: `get`이라는 이름의 함수, `uint` 타입의 값을 반환, `public`으로 선언되어 있으므로 외부에서 호출 가능, `view`로 선언되어 있으므로 함수 내부에서 상태 변수를 변경할 수 없음

### 2. Subcurrency Example
```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity ^0.8.4;

contract Coin {
// The keyword "public" makes variables
// accessible from other contracts
address public minter;
mapping(address => uint) public balances;

    // Events allow clients to react to specific
    // contract changes you declare
    event Sent(address from, address to, uint amount);

    // Constructor code is only run when the contract
    // is created
    constructor() {
        minter = msg.sender;
    }

    // Sends an amount of newly created coins to an address
    // Can only be called by the contract creator
    function mint(address receiver, uint amount) public {
        require(msg.sender == minter);
        balances[receiver] += amount;
    }

    // Errors allow you to provide information about
    // why an operation failed. They are returned
    // to the caller of the function.
    error InsufficientBalance(uint requested, uint available);

    // Sends an amount of existing coins
    // from any caller to an address
    function send(address receiver, uint amount) public {
        if (amount > balances[msg.sender])
            revert InsufficientBalance({
                requested: amount,
                available: balances[msg.sender]
            });

        balances[msg.sender] -= amount;
        balances[receiver] += amount;
        emit Sent(msg.sender, receiver, amount);
    }
}
```