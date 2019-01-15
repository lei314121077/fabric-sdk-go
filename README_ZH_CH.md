# Hyperledger Fabric Client SDK for Go

[![Build Status](https://jenkins.hyperledger.org/buildStatus/icon?job=fabric-sdk-go-tests-merge-x86_64)](https://jenkins.hyperledger.org/job/fabric-sdk-go-tests-merge-x86_64)
[![Go Report Card](https://goreportcard.com/badge/github.com/hyperledger/fabric-sdk-go)](https://goreportcard.com/report/github.com/hyperledger/fabric-sdk-go)
[![GoDoc](https://godoc.org/github.com/hyperledger/fabric-sdk-go?status.svg)](https://godoc.org/github.com/hyperledger/fabric-sdk-go)

此SDK使Go开发人员能够构建与之交互的解决方案 [Hyperledger Fabric](http://hyperledger-fabric.readthedocs.io/en/latest/).

## 入门

获取Fabric和Fabric CA的客户端SDK包。

```bash
go get -u github.com/hyperledger/fabric-sdk-go

# Optional - populate vendor directory (if needed by your downstream vendoring solution)
# cd $GOPATH/src/github.com/hyperledger/fabric-sdk-go/
# make populate
```

You're good to go, happy coding! Check out the examples for usage demonstrations.

### 示例

- [E2E Test](test/integration/e2e/end_to_end.go): 使用SDK查询和执行事务的asic示例
- [Ledger Query Test](test/integration/pkg/client/ledger/ledger_queries_test.go): 使用SDK查询channels底层分类帐的基本示例
- [Multi Org Test](test/integration/e2e/orgs/multiple_orgs_test.go): 一个涉及事务的多个组织的示例
- [Dynamic Endorser Selection](test/integration/pkg/fabsdk/provider/sdk_provider_test.go):使用动态代言人选择的示例（基于链代码策略）
- [E2E PKCS11 Test](test/integration/e2e/pkcs11/e2e_test.go): 使用PKCS11加密套件和配置进行E2E测试
- [CLI](https://github.com/securekey/fabric-examples/tree/master/fabric-cli/): 使用Go SDK构建的Fabric的示例。
- 我们还需要更多的例子!

### 社区

- 正在进行中的话题 [Rocket Chat](https://chat.hyperledger.org/channel/fabric-sdk-go).
- 正在跟踪和处理的问题 [Jira](https://jira.hyperledger.org/secure/RapidBoard.jspa?projectKey=FAB&rapidView=7&view=planning).
- 正在积极开发中的 [Gerrit](https://gerrit.hyperledger.org/r/#/admin/projects/fabric-sdk-go) repository.

## Client SDK

### 当前兼容
SDK的集成测试针对三个标记的Fabric版本运行
- prev (currently v1.3.0)
- stable (currently v1.4.0)
- prerelease (currently disabled)

此外，出于开发目的，集成测试还根据需要针对devstable Fabric版本运行

### 退休版本

当'prev'代码级别更新时，下面列出了最后一次测试的fabric-sdk-go提交或标记

- fabric v1.2: 5e291d3
- fabric v1.1: f7ae259
- fabric v1.0: 5ac5226

### 运行测试套件

```bash
# 在Fabric SDK Go目录中
cd $GOPATH/src/github.com/hyperledger/fabric-sdk-go/

# 可选 - 自动安装测试套件使用的Go工具
make depend

# 运行测试套件
make

# 清理一切并释放网络
make clean

# 只需构建应用程序（ make build ）
# 只需建立网络（ make env-up ）
```

### Go Tags

可以提供以下Go标记以启用其他功能：
 - 示例：包括对实验性功能的支持

## 贡献Go SDK


如果您想参与Go SDK，请运行测试套件并将补丁提交给Gerrit git repostory进行审核。有关一般准则，请参阅Fabric项目的[贡献页面](http://hyperledger-fabric.readthedocs.io/en/latest/CONTRIBUTING.html)。

你需要:

- Go 1.11
- [Dep](https://github.com/golang/dep)
- Make
- Docker
- Docker Compose
- Git
- libtool

### Gerrit Git存储库

要提供补丁，您需要克隆（或添加远程） [Gerrit](https://gerrit.hyperledger.org/r/#/admin/projects/fabric-sdk-go) 与身份验证。

### 运行测试套件的一部分

```bash
# 在Fabric SDK Go目录中
cd $GOPATH/src/github.com/hyperledger/fabric-sdk-go/

# 可选 - 自动安装测试套件使用的Go工具
# make depend

# 可选 - 仅运行代码检查（linters, license, spelling, etc等）
# make checks

# 运行所有单元测试和检查
make unit-test

# 运行所有集成测试
make integration-test
```

### 手动在包里面运行单元测试

```bash
# In a package directory
go test
```

### 手动运行集成测试

你需要:

- 工作fabric和fabric-ca - 设置。建议您使用提供的docker-compose文件 `test/fixtures/dockerenv`. 还建议您使用中提供的默认.env设置 `test/fixtures/dockerenv`. 请参阅以下步骤.
- 自定义设置 `test/fixtures/config/config_test.yaml` 万一你的Hyperledger Fabric网络没有运行`localhost` 或正在使用不同的端口。

#### 在Docker Hub中使用Fabric镜像


测试套件默认使用Docker Hub上最新的兼容织物镜像标签。
以下命令启动Fabric:

```bash
# In the Fabric SDK Go directory
cd $GOPATH/src/github.com/hyperledger/fabric-sdk-go/

# 启动 fabric (stable tag)
make dockerenv-stable-up

# 或者更一般地，在不同的代码级别启动结构(prev, stable, prerelease, devstable)
# make dockerenv-[CODELEVEL]-up
```

#### 运行集成测试

Fabric现在应该正在运行。在不同的shell中，运行集成测试

```bash
# In the Fabric SDK Go directory
cd $GOPATH/src/github.com/hyperledger/fabric-sdk-go

# 使用脚本设置集成测试的参数并执行它们
# 以前我们使用像Fabric CA服务器，orderer和peer这样的主机名指向localhost
#既然我们现在删除了它，我们将使用不同的配置
make integration-tests-local

# 或者更一般地，在不同的代码级别运行集成测试 (prev, stable, prerelease, devstable)
# 和 fixture 的目标版本
# FABRIC_CODELEVEL_VER=[VER] FABRIC_CODELEVEL_TAG=[CODELEVEL] make integration-tests-local
```


```bash
# 以前我们使用像Fabric CA服务器，orderer和peer这样的主机名指向localhost
# 现在我们删除了这个，我们将使用不同的配置文件 config_test_local.yaml
# 其中有Fabric CA服务器，orderer和peer指向localhost
# 也可以直接使用go test运行集成测试。例如:
#cd $GOPATH/src/github.com/hyperledger/fabric-sdk-go/test/integration/
#go test -args testLocal=true

#cd $GOPATH/src/github.com/hyperledger/fabric-sdk-go/test/integration/orgs
#go test -args testLocal=true 

# 你应该查看 test/scripts/integration.sh 以获取选项和详细信息。
# Note: 您通常应该更喜欢脚本版本为您设置参数。
```

#### 使用Fabric在本地构建进行测试（高级）

或者，您可以使用以下命令使用Fabric的本地版本：

```bash
# 启动 fabric (devstable codelevel with latest docker tags)
make dockerenv-latest-up
```

## License

Hyperledger Fabric SDK Go 软件是根据 [Apache License Version 2.0](LICENSE).

---
This document is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Creative Commons Attribution 4.0 International License</a>.
