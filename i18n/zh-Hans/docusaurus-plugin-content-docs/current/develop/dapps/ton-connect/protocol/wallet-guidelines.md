# 钱包指南

## 网络

### 只有少数几个网络。

目前，仅有两个网络 - 主网(Mainnet)和测试网(Testnet)。
在可预见的未来，不预期会出现新的类似于主网的TON网络。请注意，当前的主网内置了替代网络的机制 - 工作链(workchains)。

### 对普通用户隐藏测试网。

测试网专门用于开发者。普通用户不应看到测试网。
这意味着切换到测试网不应该容易可得，即使DApp处于测试网中，也不应提示用户将钱包切换到测试网。用户切换到测试网，如果不理解此操作，那么无法切回主网。

因此，dapps不需要在运行时切换网络，相反，更倾向于在不同的域上拥有不同实例的DApp，如dapp.com, Testnet.dapp.com。
出于同样的原因，Ton Connect协议中没有`NetworkChanged`或`ChainChanged`事件。

### 如果DApp在测试网而钱包在主网中，不要发送任何东西。

当DApp试图在测试网中发送交易时，而钱包却在主网中发送，需要防止资金损失。

DApps应在`SendTransaction`请求中明确指示`network`字段。

如果设置了`network`参数，但钱包设置了不同的网络，钱包应显示警告并不允许发送此交易。

在这种情况下，钱包不应提供切换到另一个网络的选项。

## 多账户

可以为一对密钥创建多个网络账户。在您的钱包中实现此功能 - 用户会觉得这很有用。

### 一般而言，当前没有所谓“活跃”账户

目前，TON Connect并未建立在钱包中有一个被选中的账户的假定之上，当用户切换到另一个账户时，会向dapp发送`AccountChanged`事件。

我们将钱包视为一个实体钱包，它可以包含许多“银行卡”（账户）。

在大多数情况下，发件人地址对dapp来说并不重要，在这些情况下，用户可以在批准交易时选择合适的账户，交易将从选定的账户发送。

在某些情况下，对DApp来说，从特定地址发送交易很重要，在这种情况下，它会在`SendTransaction`请求中明确指定`from`字段。如果设置了`from`参数，钱包不应允许用户选择发件人地址；如果无法从指定地址发送，钱包应显示警告，不允许发送此交易。

### 登录流程

当DApp连接钱包时，用户在钱包中选择他们想要登录到dapp的其中一个账户。

无论用户接下来在钱包中使用什么账户，DApp都与他在连接时接收到的账户一起工作。

就像您使用其中一个电子邮件账户登录到Web服务一样 - 如果您随后在电子邮件服务中更改了电子邮件账户，Web服务继续使用它在您登录时获得的那个账户。

因此，协议不提供`AccountChanged`事件。

要切换账户，用户需要在DApp UI中断开连接(注销)并重新连接(登录)。

我们建议钱包提供与指定DApp断开会话的能力，因为DApp可能具有不完整的UI。

## 参阅

* [TON Connect概览](/dapps/ton-connect/overview)
* [协议规范](/dapps/ton-connect/protocol/)
* [连接钱包](/dapps/ton-connect/wallet)