## Truffle

##### npm  包管理

* 查看所有安装的包

  npm list -g

* 查看深度0级的安装包

  npm list -g --depth=0

* 查看制定包版本号

  npm view -g <package_name> version

#### ethereum客户端

* Ganache / Ganache CLI

  testrpc 的升级版，适用于开发环境

* truffle develop

  和ganache类似

* geth

  全功能客户端，适用于生产环境

* parity

  类似geth，但是性能更加优秀



#### 合约级的全局变量

* this

  当前合约地址