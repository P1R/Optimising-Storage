# Optimizing Storage

## Requirements

* foundry
* foundry-template
* store.sol code
* sol2uml
> Note: check the references

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
    uint8 index;
    uint256 public number;
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
Code1.

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

[](./assets/originalStore.svg)
Img.1

After running Ganache we verify deployment by using the following command

```bash
forge create Store --legacy --contracts src/Storage.sol --private-key <ganache private key> --rpc-url http://127.0.0.1:7545
```

[](./assets/originalStoreDeployment.png)
Img2.

## References
1. foundry-template, https://github.com/PaulRBerg/foundry-template , 2023-04-28
2. sol2uml, https://github.com/naddison36/sol2uml , 2023-04-28
3. store.sol, https://gist.github.com/extropyCoder/6e9b5d5497b8ead54590e72382cdca24 , 2023-04-28


## License

This project is licensed under MIT.
