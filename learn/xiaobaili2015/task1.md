## 第一章：走进 Web3 世界

1. 简单描述一下本地开发、部署合约的流程

   1. 首先在本地搭建开发环境，比如 solidity、js 等编程语言开发环境，并安装相关依赖库。
   2. 准备好钱包账户。
   3. 进入 IDE 编写合约代码，推荐的 IDE 是网页端的 Remix。
   4. 在 IDE 内编写单元测试，通过 mock 区块链节点等方式进行本地测试。
   5. 编译 sol 代码为字节码，在测试网部署该合约字节码，进行测试。
   6. 部署合约到主网。

2. 简单描述一下用户在使用一个 DApp 时与合约交互的流程

   1. 通过浏览器插件等方式将用户钱包地址与 DApp 对应的链进行连接。
   2. 选取想要购买的物品，设置购买数量，点击购买按钮。
   3. 触发浏览器插件等方式的购买确认面板，查看交易详情。
   4. 点击确认按钮，提交交易。
   5. 浏览器插件与智能合约交互，完成交易。
   6. 用户前端显示交易结果详情。

3. 通读[「区块链黑暗森林自救手册」](https://github.com/slowmist/Blockchain-dark-forest-selfguard-handbook/blob/main/README_CN.md)，列出你觉得最重要的三个安全技巧

   1. 保持零信任，始终怀疑。从前端网页、前端插件、钱包助记词备份到签名安全、通信安全甚至人性安全等，各个环节都不盲目信任某个事物，包括官方来源。
   2. 警惕钓鱼，不点击可疑链接或附件，尤其是仿冒官方的邮件、社交媒体私信等。
   3. 设置多个隔离环境。当被黑时，损失的仅被黑目标的那些隐私，而不会危及到其他隐私。
