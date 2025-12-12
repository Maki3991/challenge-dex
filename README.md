### 2025/12/11

> `_mint(msg.sender, 1000 ether);`

- \_mint(\_to, amount ether)，第一个变量传入钱包地址，新铸的币会被全部放到这个钱包中
- 由于这个函数常在构造函数内，所以这里的 address 是的部署者的钱包地址
- 这里的 ether 仅仅代表的是 10^18，1 ether = 1\*10^18 = 1 个完整代币

> `require(token.transferFrom(msg.sender, address(this), tokens), "DEX: init - transfer did not transact");`

- 检查语法 require()中的函数是真的会被执行的
- ERC20 代币的转账方法是 transfer()或者 transferFrom()，其返回值是一个 bool 值，刚好符合 require()的条件
- 而 ETH 的转账方式.call{value: amount}("")，返回值是一个元组，包含两个值，一个 bool success 表示调用是否成功，一个 bytes data 代表对方传回的数据

Uniswap V2 的运行逻辑

- AMM：Automated Market Maker
- 项目中的 DEX 采用的是非常经典的 peer-to-pool 模型，而不是像大型交易所那种 peer-to-peer 的撮合交易模式
- 原理是 $x*y=k$，其中 k 为常数，x 为 ETH 的储蓄量，y 为 token 的储备量；x 和 y 任意一者波动都会导致对方反向波动；现货的价格就是 $y/x$
- 手续费收取 0.3%，真正进入池子换币的数量为原来的 99.7%；由于 solidity 不支持小数运算，可以选择分子分母同时乘以 1000
- $输出量=(输出库存*输入量)*1000/(输入库存+输入量)*1000$
- deposit 入股时，所入的 ETH:Token 应该等于 x:y
