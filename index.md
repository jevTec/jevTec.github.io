前段时间，公司拓展海外业务，一下子多了好几十台海外服务器。服务器管理自然少不了使用SSH管理，没想到被SSH管理折磨得不行，不管服务器ping得通还是ping不通，SSH总是各种丢失连接、卡顿或延迟，严重的影响了业务的发布。

找了一圈解决办法，发现了个好东西叫GlobalSSH，分享下

### 什么是GlobalSSH
GlobalSSH是一款致力于提高跨国远程管理服务器效率的产品，旨在解决因为跨国网络不稳定，通过远程管理服务器时，经常会出现卡顿、连接失败、传输速度较慢等现象。

### 如何使用GlobalSSH
#### 1.安装配置CLI
**Home-brew 安装**

```
   brew install ucloud
```

   brew是Mac、Ubuntu系统支持的软件包管理器，支持大部分的个人工具软件下载，无需运行root权限。安装过程中若出现报错，请先更新管理器版本解决:

```
   brew update
```

**二进制文件下载安装**

Mac: http://ucloud-sdk.ufile.ucloud.com.cn/ucloud-cli-macosx-0.1.2-amd64.tgz

Linux: http://ucloud-sdk.ufile.ucloud.com.cn/ucloud-cli-linux-0.1.2-amd64.tgz

Windows: http://ucloud-sdk.ufile.ucloud.com.cn/ucloud-cli-windows-0.1.2-amd64.zip

*SHA-256 checksum*

```
19b7a0803fc41ee689797a36fd67b288e993c383edf6087f56825a4d5bb17875 ucloud-cli-linux-0.1.2-amd64.tgz
ecc787f4045ea14d583801cd0cfa746be357d50756c2cf0ba879e405c2325d1c ucloud-cli-macosx-0.1.2-amd64.tgz
f48058ac96bb0283b18c660f0350eedba49d03a753775b0a2773b2081698b3f3 ucloud-cli-windows-0.1.2-amd64.zip
```

**源码安装**

Golang编程语言环境下：

```
$ mkdir -p $GOPATH/src/github.com/ucloud
$ cd $GOPATH/src/github.com/ucloud
$ git clone https://github.com/ucloud/ucloud-cli.git
$ cd ucloud-cli
$ make install
```

**启动配置 CLI**

根据提示，输入UCloud OpenAPI验证信息完成配置

```
   ucloud init
```




#### 2.创建GlobalSSH

##### 2.1 AWS主机
安装后就可以使用GlobalSSH了，先拿AWS的EC2云主机来试试，IP是52.59.198.254，位于日本东京，SSH端口8022。在CLI界面输入如下命令：

> ucloud gssh create --area Tokyo --target-ip 52.59.198.254 --port 8022

当然，如果你的SSH端口是默认的22，可以省去port参数。回车，命令行显示创建成功的提示：

> Succeed, GlobalSSHInstanceId: uga-1t0pqx

然后我们使用ls命令查看之前创建的加速实例
> ucloud gssh ls

命令行显示如下：
> InstanceID:uga-1t0pqx, AcceleratingDomain:52.59.198.254.ipssh.net, TargetIP:52.59.198.254, Port:8022, Remark:

52.59.198.254.ipssh.net就是SSH的加速域名。
> ssh root@52.59.198.254.ipssh.net

现在我就可以用这个域名进行SSH管理了，前后不用1分钟，超级方便！

##### 2.2 GCE主机
个人有一台GCE主机，ip是35.198.163.178，位于北美区域，SSH端口默认22，之前SSH管理也很卡

由于端口是22，所以这次可以省去--port参数，因为是我自己的主机，我还特意添加了备注参数--remark。输入命令：
> ucloud gssh create --area Washington --target-ip 35.198.163.178 --remark 自己的云主机

创建成功后，还是使用使用加速域名进行管理
> ssh root@35.198.163.178.ipssh.net

PS：要是在使用时，想知道参数和支持加速的区域，可以使用-h命令
> ucloud gssh create -h

##### 2.3 UCloud主机
目前CLI不支持账号注册，待补充
* 案例4：通过CLI，注册UCloud账号，完成购买海外主机、EIP，globalssh加速。（注：这里引入UCloud。但这个例子冗长，要注意控制好篇幅）

### 速度测试
不看功效看疗效，本着实事求是的精神，我特地做了个有趣的小测试，在不同的丢包率环境下，通过SSH直连和GlobalSSH加速2种方式分别连接海外服务器，然后通过在终端界面打出26个字母，分别计算在屏幕上完整显示26个字母的时间。

10%左右丢包率的网络环境下的测试，平均效率提升约为14.21%
连接方式 | 用时(秒) | 用时(秒) | 用时(秒) | 用时(秒) | 用时(秒)
---|---|---|---|---|---
SSH直连| 8.65 | 9.80 | 8.40 | 8.38 | 8.13
GlobalSSH加速 | 7.21 | 8.33 | 7.18 | 7.48 | 6.98
效率提升 | 16.65% | 15.00% | 14.52% | 10.74% | 14.15%

50%左右丢包率的网络环境下的测试，平均效率提升约为20.34%
连接方式 | 用时(秒) | 用时(秒) | 用时(秒) | 用时(秒) | 用时(秒)
---|---|---|---|---|---
SSH直连| 9.03 | 8.98 | 9.28 | 9.18 | 9.13
GlobalSSH加速 | 7.16 | 7.15 | 7.58 | 7.46 | 6.98
效率提升 | 20.71% | 20.38% | 18.32% | 18.74% | 23.55%

需要强调的是，实际使用中，除了输入，还会有命令的回调及显示。以我的使用经验来说，真实的SSH管理操作，加速效果会更明显

### 产品架构
![GlobalSSH](https://jevTec.github.io/gs_15343170909302.png)

GlobalSSH是一款基于TCP层的网络加速产品，通过私有的加速线路转发数据到目的服务器，告别了公网的不稳定性。
之前因为担心稳定性等问题，我还私下联系过产品研发人员，他们告诉我；GlobalSSH每条加速线路都具有多条备用线路，且每条线路的出入口都在多地部署，具有100%高可用和一定的容灾能力。这样看来，产品的稳定性还是值得称赞的。

### 产品价格

收费项 | 价格
---|---
加速实例 | 免费
流量费用 | 免费


GlobalSSH产品现已全面免费，终于可以不用忍受SSH卡顿了。对了，如果你也对GlobalSSH感兴趣或有任何疑问，可以联系[help@globalssh.io](mailto://help@globalssh.io)
