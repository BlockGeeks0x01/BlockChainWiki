## Solidity

#### 基础知识

##### 交易区块大小

一笔交易的大小或者区块大小并没有固定的限制，这是以太坊的一个优势，即可扩展。但这并不意味着没有限制，区块最大能消耗的gas数量是由限制的，这个限制是动态的，换句话说单笔交易最多能消耗一个区块的最大gas数量。对于普通交易（接收方不是智能合约地址），基础费用是21000 gas，input data 中的数据按 4 gas / 全0字节，68 gas / 非0字节 计算。因此单笔普通交易能附带的input data数据量受当前网络中区块最大gas量限制。当某个区块gas消耗量非常大时，以太坊网络会自动调节最大gas量，调节速率为1/1024。因此总结如下：limit：yes，fixed limit（例如bitcoin）：No。

##### 消息调用（message call）

合约可以通过消息调用的方式来调用其它合约或者发送以太币到非合约账户。消息调用和交易非常类似，它们都有一个源、目标、数据、以太币、gas和返回数据。事实上每个交易都由一个顶层消息调用组成，这个消息调用又可创建更多的消息调用。调用深度被 **限制** 为 1024 ，因此对于更加复杂的操作，我们应使用循环而不是递归。

 ##### 委托调用/代码调用（delegate call）

有一种特殊类型的消息调用，被称为 **委托调用(delegatecall)** 。它和一般的消息调用的区别在于，目标地址的代码将在发起调用的合约的上下文中执行，并且 `msg.sender` 和 `msg.value` 不变。 这意味着一个合约可以在运行时从另外一个地址动态加载代码。存储、当前地址和余额都指向发起调用的合约，只有代码是从被调用地址获取的。 这使得 Solidity 可以实现”库“能力：可复用的代码库可以放在一个合约的存储上，如用来实现复杂的数据结构的库。

##### 日志

有一种特殊的可索引的数据结构，其存储的数据可以一路映射直到区块层级。这个数据被称为 **日志(logs)** ，Solidity用它来实现 **事件(events)** 。合约创建之后就无法访问日志数据，但是这些数据可以从区块链外高效的访问。因为部分日志数据被存储在 [布隆过滤器（Bloom filter)](https://en.wikipedia.org/wiki/Bloom_filter) 中，我们可以高效并且加密安全地搜索日志，即使是那些没有下载整个区块链的网络节点（轻客户端）也可以找到这些日志。

***

#### 抽象合约（Abstract Contracts）

合约中包含没有实现的函数签名，即为抽象合约。简单来说，既包含已经实现的函数，也包含没有实现的函数的合约就是抽象合约，抽象合约无法被编译，只能被继承。如果一个合约继承了抽象合约，但是并没有实现所有的函数，那么这个合约还是抽象合约

```
contract Feline {
    function utterance() public returns (bytes32);
}
```

#### 接口（Interfaces）

接口类似抽象合约，但是不能包含已实现的函数，并且有如下约束（也许在未来会去掉）：

* 不能继承其他合约或接口
* 不能定义构造函数
* 不能定义变量、结构体和枚举

```
interface Token {
    function transfer(address recipient, uint amount) public;
}
```

#### 库（Library）



#### 函数/变量可见性

* private

  私有函数/变量，即使是子合约也无法访问

* public

  公有函数/变量，允许外部访问。**public 是函数的默认可见性**。public 函数是合约接口的一部分，既可以内部访问也可以通过消息调用；对于public 类型的状态变量，会自动生成 getter 函数。

* internal

  内部函数/变量，子合约可以访问。调用 internal 函数不会创建一个EVM调用（也叫消息调用message call）。**internal 是状态变量的默认类型**。

* external

  外部函数，**只**允许外部访问。调用 external 函数会创建一个EVM调用。external 函数是合约接口的一分部，它无法被内部访问，因此 ```f()```是无效的，```this.f()```才有效。当传入数据规模较大的数组参数时，external 函数更有效。

##### Getter 函数

编译器会为public类型状态变量自动创建同名 getter 函数，这个getter函数也是public 类型的。

```
contract C {
    uint public data;
    function x() public {
        data = 3; // internal access
        uint val = this.data(); // external access
    }
}
```



##### 构造函数

与所在合约同名的函数，当且仅当在合约创建时（即部署到链上时）执行一次

##### 函数修饰器

#### 函数

函数参数默认类型是 memory，memory类型参数相当于值传递，storage类型参数相当于指针传递。

view 视图函数是免费调用的，因为只需要读取链上数据而不需要在每个节点上运行验证。但如果视图函数被合约内部其他非视图函数调用，还是需要消费gas的，因为这段代码需要在每个节点上运行。

###### 可能修改链上数据的操作行为

* 写入数据（修改 state 变量）
* 触发事件写日志
* 创建合约
* 使用 selfdestruct
* 通过calls发送ether
* 调用没有标记为 view 或 pure 的函数
* 调用底层的 calls
* 使用包含写操作的内联汇编（inline assembly）的操作码

###### 可能读取链上数据的操作行为

* 读取state变量
* 访问地址余额（this.balance / <address>.balance）
*  访问 block，tx，msg 对象的任意成员（除了 msg.sig 和 msg.data）
* 调用没有标记为 pure 的函数
* 使用包含读操作的内联汇编（inline assembly）的操作码

###### 函数标签

* view，别名 constant

  view 函数保证不修改链上状态，目前编译器没有强制要求view函数不修改数据。Getter 方法会被标记为 view

* pure

  pure 函数保证即不修改链上数据也不读取链上数据，目前编译器没有强制要求pure函数不读取数据

* payable

  这个函数可以接收附带的eth货币

###### 函数选择器（function selector）

一个函数调用数据的前4个字节指定了要调用的函数，而这个函数调用数据就是该函数的keccak（SHA-3）哈希的前4个字节（高位在左的大端顺序）。这样的签名被定义为基础原型的规范表达，基础原型即是函数名称加上由括号括起来的参数类型列表，参数类型间由一个逗号分隔开，且没有空格。

#### 数据类型

合约的状态变量是永久存储在合约存储中的值。

* array 数组，memory 类型的数组，目前不支持动态长度（未来版本可能会支持），即只能定义定长数组：

  ```
  uint[] memory values = new uint[](3);
  ```

* uint => uint256，int => int256

* int8/uint8 ~ int256/uint256，以8递增

* 整形变量默认置为0，而不是null 或者 undefined

* 定长字节数组（fixed sized byte array）

  bytes1，bytes2，.... bytes32，byte 是 bytes1 的别名

  支持位级运算，支持以下标访问字节数组，If `x` is of type `bytesI`, then `x[k]` for `0 <= k < I` returns the `k` th byte (只读)。`bytesI`.length 返回字节数组长度

  注：可能有人会想到使用 `byte[]`，但这种方式每个数组元素都会占用32个字节长度，因此使用bytes类型更经济便宜。

* 动态长度字节数组


  * bytes，不定长字节数组，不是值类型。一般用于原始字节数据
  * string，不定长UTF-8字符串，不是值类型。一般用于UTF8字符串数据

* 地址字符串

  长度在39~41之间且通过了地址校核测试的16进制字符串，例如`0xdCad3a6d3569DF655070DEd06cb7A1b2Ccd1D3AF` 被认为是地址字符串，否则就被当成普通的有理数字符串

* 字符串字面值

  单/双引号括起来

* 16进制字面值

  `hex"001122FF"`

* 枚举

  和C语言中的枚举类似，可以显示的转换为整数或者从整数转换过来，但是隐式的类型转换不行。

  ```javascript
  enum ActionChoices { GoLeft, GoRight, GoStraight, SitStill }
  ActionChoices choice;
  ActionChoices constant defaultChoice = ActionChoices.GoStraight;
  ```

* 函数

  `function (<parameter types>) {internal|external|public|private} [pure|constant|view|payable] [returns (<return types>)]`

  有两种方式访问当前合约的函数，`f`是内部调用方式，`this.f`是外部调用方式 (external function)

  此外，public或者external 函数有一个特殊的成员`selector`，返回 ABI 格式的函数选择器

  ```javascript
  contract Selector {
    function f() public view returns (bytes4) {
      return this.f.selector;
    }
  }
  ```


注：在solidity中，5/2 等于 2.5，不会小数位

##### 引用类型

solidity 中的引用类型（数组、结构体等）有三种数据所在位置

* memory 关键字

   数据存储在内存中，非永久

  函数参数和返回值默认都是memory类型

* storage 关键字

  数据存储在链上，永久。即状态变量

  local variable 默认是storage，而state variable强制是storage（显然）

* calldata

  函数参数可以存储在calldata中，不可修改也不是永久存储。外部函数的参数强制是calldata，表现得和memory类似。

变量存储位置非常重要，它决定了变量在赋值时的行为。在memory和storage类型的变量之间赋值会创建一个独立的copy，而memory类型变量互相赋值时不创建copy。storage变量互相赋值也仅传递引用。

###### 数组

storage 类型的数组元素可以是任意类型；memory类型的数组元素不能是mapping类型，并且如果是作为外部可见函数的参数，必须是ABI 类型。二维数组（一个有5个元素的定长数组，数组元素为不定长数组）写法如下（和其他语言顺序相反）：`uint[][5]`，但是访问多维数组的时候，是正常从左到右的顺序。

public类型的数组变量，自动生成的getter函数会有一个index的参数，获取指定位置的数组元素。

storage类型的数组可以通过`.length`属性修改数组长度

可以使用`new`创建指定长度的memory数组，但是memory数组不能使用`.length`字段修改长度

```javascript
contract C {
    function f(uint len) public pure {
        uint[] memory a = new uint[](7);
        bytes memory b = new bytes(len);
        // Here we have a.length == 7 and b.length == len
        a[6] = 8;
    }
}
```

```javascript
contract C {
    function f() public pure {
        g([uint(1), 2, 3]);		// 第一个元素需要转化为数组定义的类型
    }
    function g(uint[3] _data) public pure {
        // ...
    }
}
```

```javascript
// This will not compile.

pragma solidity ^0.4.0;

contract C {
    function f() public {
        // 目前，定长的memory数组不能赋值给不定长的memory数组,这个限制未来可能去除
        // The next line creates a type error because uint[3] memory
        // cannot be converted to uint[] memory.
        uint[] x = [uint(1), 3, 4];
    }
}
```

数组的方法有：

*  length

  不定长的存储在storage的数组可以修改这个字段改变数组长度，但是memory数组在创建后长度就固定了（但是它们可以在运行时通过`new`动态创建）

* push

  不定长数组和bytes可以使用push方法追加元素，返回新的数组长度。

注：目前受到EVM的限制。外部函数不能返回不定长数组，目前的解决方案只能是返回较大的定长数组。

###### 字典映射 mapping

`mapping(_KeyType => _ValueType) `

mapping 类型的变量只能是state变量，或者内部函数中的storage引用类型，当mapping变量设为public时，自动创建的getter函数将接受key为参数，返回对应的value，当value也是一个映射时，会递归的有key参数。

`var`关键字，`var`定义的变量类型通过**第一次**赋值时推导得到，`var`不同用与函数参数和返回值。

##### 全局变量 / 函数

* Ether 单位

  wei，finney，ether等。``` 2 ether == 2000 finney ```

* Time 单位

  seconds，minutes，hours，days 等

* 区块和交易对象

  * ``` block.blockhash(uint blockNumber) returns (byte32)``` 指定区块的hash，只限于最近的256个区块
  * ```block.coinbase returns (address)``` 打包当前区块的矿工地址
  * ```block.difficulty returns(uint)``` 当前区块难度
  * ```block.gaslimit returns(uint)``` 
  * ```block.number return(uint)```
  * ```block.timestamp returns(uint)``` 当前区块的时间戳/秒
  * ```msg.data return(bytes)``` 当前交易的calldata
  * ```msg.gas return(uint)``` 剩余的gas
  * ```mas.sender return(address)```
  * ```msg.sig return(bytes4)``` calldata 的前4个字节
  * ```msg.value return(uint)``` 当前交易附带ether，以wei为单位
  * ```now return(uint)``` 当前区块的时间戳（alias for ```block.timestamp```）
  * ```tx.gasprice return(uint)```
  * ```tx.origin return(address)``` 交易创建人的地址

  注：```msg```对象的每个属性（包括sender和value）会根据每一个外部函数调用（external function call）而改变，包括调用库函数。千万不能以```block.timestamp```或```block.blockhash```作为随机数来源，因为这些数据都在一定程度上受矿工影响。

* 错误处理

  * ```assert(bool condition)```
  * ```require(bool condition)```
  * ```revert()``` 中止执行指令并回滚状态变更

* 数学和加密学运算

  * ``` addmod(uint x, uint y, uint k) returns (uint)``` 计算```(x+y) % k```，其中 ```k != 0```
  * ``` mulmod(uint x, uint y, uint k) returns (uint)``` 计算```(x * y) % k```，其中 ```k != 0```
  * ```keccak256(...) returns(byte32)``` 计算紧凑填充（tightly packed）的参数的Keccak-256（Ethereum-SHA-3
  * ）哈希值
  * ```sha256(...) returns (byte32)``` 计算凑填充的参数的SHA-256哈希值
  * ```sha3(...) returns(byte32)``` 同 ```keccak256```
  * ```ripemd160(...) returns (bytes20)``` 
  * ```ecrecover(bytes32 hash, uint8 v, bytes32 r, bytes32 s) returns (address)``` recover the address associated with the public key from 椭圆曲线签名 or return zero on error

  紧凑填充指的是无填充的串联参数

* 地址相关

  * ``` <address>.balance return(uint256) ``` address地址的余额 / wei
  * ``` <address>.transfer(uint256 amount)``` 发送amount数量的Wei到地址address，失败时抛出异常
  * ``` <address>.send(uint256 amount) returns (bool) ``` 失败时返回false，send方法是transfer方法的底层实现，不推荐直接使用。
  * ```<address>.call(...) returns (bool)``` 触发底层的```CALL```,失败时返回false
  * ~~```<address>.callcode(...) returns (bool)``` 触发底层的```CALLCODE```,失败时返回false~~
  * ```<address>.delegatecall(...) returns (bool)``` 触发底层的```DELEGATECALL ```失败时返回false，delegatecall 用于调用库函数。

  注： 使用send存在危险，当调用栈超过1024时会失败，且用户需要手动检查函数返回值。推荐使用transfer。上述的call、callcode、delegatecall 都是非常底层的函数，一般最为最后手段使用，因为它们会破坏代码的类型安全。当调用其它合约时，需要注意着意味着把控制权交给了其他合约，他可能会反过来调用你从而修改你的数据。

* 合约相关

  * ```this``` 返回类型为当前合约的类型，返回当前合约引用，可以显式地转为```address```

  * ```selfdestruct(address recipient)``` 自我销毁当前合约，并将合约剩余的ether发给参数地址。

    合约代码从区块链上移除的唯一方式是合约在合约地址上的执行自毁操作 `selfdestruct` 。合约账户上剩余的以太币会发送给指定的目标，然后其存储和代码从状态中被移除。尽管一个合约代码中没有显示调用`selfdestruct`，但它任然可能通过`delegatecall`执行自毁操作。

  * ```suicide(address recipient)``` alias to `selfdestruct`

  注：合约继承自地址，因此可以通过`this.balance`获取当前合约的ether余额

##### 断言

* require 和 assert 的区别

  当条件不符合时都会抛出异常，require会退还用户剩余的gas，而assert则不会，因此大多数情况下使用require。

##### web3.js

* call

  调用 view 或 pure 函数，只在本地节点运行，不会发送交易向链上写入数据

* send

  会发送交易改变链上数据或状态，需要消耗gas

##### 导入

```javascript
import "./AnotherContract.sol";	// 导入本地sol文件或library
import "somepackage/SomeContract.sol";	// 导入第三方包，搜索EthPM，NPM
// 创建新的全局符号 alias 和 symbol2，分别从 "filename" 引用 symbol1 和 symbol2
import {symbol1 as alias, symbol2} from "filename";
// 创建一个新的全局符号 symbolName，其成员均来自 "filename" 中全局符号
import "filename" as symbolName;
```

##### 与网络交互

* 写入数据叫 transaction，操作是异步的，例如访问合约函数只会立刻返回交易ID
* 读取数据叫 call，操作是同步，能立刻返回结果 

##### 其他

* 以太坊地址

  0x + 40位16进制

* 交易ID

  0x + 64位16进制