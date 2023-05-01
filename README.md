# Optimizing Storage

## Requirements

* foundry
* foundry-template
* store.sol code
* sol2uml
> Note: check the references

## The original Storage smart contract

the store code example is the folloing:

```Solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.13;

contract Store {

    struct payments {
        bool valid;
        uint256 amount;
        address sender;
        uint8 paymentType;
        uint256 finalAmount;
        address receiver;
        uint256 initialAmount;
        bool checked;
    }
    uint8 index; uint256 public number;
    bool flag1;
    address admin;
    mapping (address=>uint256) balances;
    bool flag2;
    address admin2;
    bool flag3;
    payments[8] topPayments;


    constructor(){

    }


    function setNumber(uint256 newNumber) public {
        number = newNumber;
    }

    function increment() public {
        number++;
    }
}
```
Code1. Original not optimized storage

write the Code1. as a contract file named after Storage.sol in the src directory, verify it gets build.

```bash
$ forge build
[⠊] Compiling...
[⠊] Compiling 22 files with 0.8.19
[⠆] Solc 0.8.19 finished in 1.97s
Compiler run successful
```

After installing [sol2uml](https://github.com/naddison36/sol2uml) execute the following command

```bash
sol2uml storage ./src -c Store
```

![](assets/originalStore.svg?raw=true)

Img.1 Shows Storage UML from the original source code

After running Ganache we verify deployment by using the following command

```bash
forge create Store --legacy --contracts src/Storage.sol --private-key <ganache private key> --rpc-url http://127.0.0.1:7545
```

![](./assets/originalStoreDeployment.png?raw=true)

Img2. Shows the gas consumption on the contract deployment

> Notice that this gas might not be only due to the memory storage, but also deployment tasks.

## Optimizing Performance
By doing some research [4,5] and development tests using the sol2uml and modifying the contract I consider
this as an optimized storage version contract:

```Solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.13;

contract Store {

    struct payments {
        uint256 amount;
        uint256 finalAmount;
        uint256 initialAmount;
        address sender;
        address receiver;
        uint8 paymentType;
        bool valid;
        bool checked;
    }
    uint256 public number;
    mapping (address=>uint256) balances;
    address admin;
    address admin2;
    uint8 index;
    bool flag1;
    bool flag2;
    bool flag3;
    payments[8] topPayments;


    constructor(){

    }


    function setNumber(uint256 newNumber) public {
        number = newNumber;
    }

    function increment() public {
        number++;
    }
}
```
Code2. a storage optimized version

![](assets/finalStore.svg?raw=true)

Img.3 Shows Storage UML from the optimized Storage source code

After running Ganache we verify deployment by using the following command

```bash
forge create Store --legacy --contracts src/Storage.sol --private-key <ganache private key> --rpc-url http://127.0.0.1:7545
```

![](./assets/finalStoreDeployment.png?raw=true)

Img4. Shows the gas consumption on the contract deployment

## Conclusions

Even if the optimization gas was not much, it must be considered that the measurement method was not the best, since
it is based on deployment and not the specific storage optimization, which can be better verified by the UML models
Comparison. The importance can be better noticed at the struct Array, which has a high impact over the contract life while
these arrays might grow a lot, and thus reducing considerably the storage/gas consumption on the long term.


## References
1. foundry-template, https://github.com/PaulRBerg/foundry-template , 2023-04-28
2. sol2uml, https://github.com/naddison36/sol2uml , 2023-04-28
3. store.sol, https://gist.github.com/extropyCoder/6e9b5d5497b8ead54590e72382cdca24 , 2023-04-28
4. The impact of variable ordering on gas, https://medium.com/@emn178/solidity-the-impact-of-variable-ordering-on-gas-d2918fd9d95c , 2023-04-30
5. Layout of State Variables in Storage, https://docs.soliditylang.org/en/v0.8.17/internals/layout_in_storage.html , 2023-04-30



## License

This project is licensed under MIT.
