### 2025/12/11
> `_mint(msg.sender, 1000 ether);`
- _mint(_to, amount ether)，第一个变量传入钱包地址，新铸的币会被全部放到这个钱包中
- 由于这个函数常在构造函数内，所以这里的address是的部署者的钱包地址
- 这里的ether仅仅代表的是10^18，1 ether = 1*10^18 = 1 个完整代币

> `require(token.transferFrom(msg.sender, address(this), tokens), "DEX: init - transfer did not transact");`
- 检查语法require()中的函数是真的会被执行的
- ERC20代币的转账方法是transfer()或者transferFrom()，其返回值是一个bool值，刚好符合require()的条件
- 而ETH的转账方式.call{value: amount}("")，返回值是一个元组，包含两个值，一个bool success表示调用是否成功，一个bytes data代表对方传回的数据

Uniswap V2的运行逻辑
