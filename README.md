# solidity

## 基础语法

- pragma：版本标识指令。用来启动某些编译器检查。比如pragma solidity ^0.8.0;表示当前合约只可以被主版本是0.8的编译器进行编译。又比如pragma solidity >= 0.7.0 < 0.9.0;表示当前合约可以被主版本大于等于0.7，小于0.9的编译器进行编译。

- 值类型

  - bool：布尔类型。取值是true或者false

  - int/unint：表示无符号或者有符号的整数类型。uint8~uint256以及int8~int256。以8位步长递增。uint、int分别代表的是uinit256以及int256。下面的案例就是越界了。

    ```solidity
    
    // SPDX-License-Identifier: SEE LICENSE IN LICENSE
    pragma solidity ^0.8.0;
    contract OverFlow{
        function add1() public pure returns (uint8){
            uint8 x = 127;
            uint8 y = x * 2;
            return y;
        }
    
        function add2() public pure returns (uint8){
            uint8 x = 240;
            uint8 y = 16;
            uint8 z = x + y;
        }
    }
    ```

  - address：地址类型来表示一个账号，地址类型有两种。address：一个20个字节的值。address payable:表示可支付地址，与address相同也是20字节。不过有其自身的成员函数transfer和send。

    > \<address>.balance(uint256)   以Wei为单位的地址类型的余额
    >
    > \<address payable>.transfer(uint256 amount) 向地址类型address发送amount数量的Wei，失败时抛出异常。
    >
    > \<address payable>.send(uint256 amount) returns (bool) 向地址address发送amount数量的Wei，失败时返回false。

    ```solidity
    // SPDX-License-Identifier: SEE LICENSE IN LICENSE
    pragma solidity ^0.8.0;
    contract TestAddress{
        function testTransfer(address payable x) public{
            //任何一个合约都可以显式的转换成address类型
            address myaddress = address(this);
            if(myaddress.balance >= 10){
                //如果x是一个合约地址，那么当transfer发生时，合约的receive或者fallback函数会随着transfer一起调用
                x.transfer(10);
            }
        }
    }
    ```

    地址类型还有几个比较偏向于底层的成员变量，类似于java的反射。这部分没有理解，后续需要进一步学习。

    > call
    >
    > Delegatecall
    >
    > staticcall