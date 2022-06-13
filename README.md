# v2ex-alfred-workflow
v2ex website alfred workflow， see https://www.v2ex.com/。
在 alfred 中迅速访问 v2ex 中的热门帖子

# 功能预览

通过输入节点的短名称和分页号来访问 v2ex 中的帖子。

![func preview](./assets/img/v2ex.gif)

常见的节点短名称：
- apple: 苹果
- deals: 交易
- tech: 技术

可以通过浏览 v2ex 页面观察 url 的 tab 字段发现当前页面的节点短名称。
![short name](./assets/img/v2ex-shortname.png)

# 安装方法

国内访问 v2ex 需要一个梯子，没有梯子的话是没有办法访问 v2ex 的，所以该插件也不能正常工作。

安装过程中可能需要的几个步骤
1. 准备环境，python3/openssl3
2. 下载 alfred worflow 文件
3. 配置工作流

下面逐一介绍各个步骤。

## 准备环境

### openssl 更新

首先确定自己是一最新的 openssl 库，因为底层的 python 依赖这个库，所以首先检查系统的 openssl 库是否是最新的，在 mac osx 中, 通过
```bash
➜  ~ which openssl
/usr/local/opt/libressl/bin/openssl
➜  ~ openssl version
LibreSSL 3.5.3
```
如果不是最新的 openssl，使用 `brew` 更新至最新的版本，注意 `libressl` 与 openssl 是等价的，同一个东西的两个分支。
```bash
brew upgrade libressl
```
请记住自己 `libressl`  的安装位置，一般是：`/usr/local/opt/libressl/bin/openssl`，后面可能需要重新配置。

### 申请自己的 v2ex token

登陆自己的 v2ex 账户查看设置，其中有可以申请 token 的位置，请记住自己的 token，后面配置 workflow 的时候需要使用。
![v2ex token](./assets/img/v2ex-token.png)

### 系统代理测试

因为 v2ex 站点需要有梯子才能访问，这一步主要是测试自己的梯子是可靠的，测试方法主要使用 curl 命令来测试，这里我的梯子是：`socks5://127.0.0.1:1086`，那么我的样例测试代码是：
```bash
curl -k  'https://www.v2ex.com/api/v2/token' -H "Authorization: Bearer 238fc3bb-bbbb-xxxx-81b4-63f726a62acd"  --proxy 'socks5://127.0.0.1:1086' -vv
```
正常测试应该很快返回，如果你的测试请求卡住了，这个时候可能有两个原因：
- DNS 解析可能存在问题
- 代理没办法工作，这个可以使用浏览器插件来测试下代理是否是 ok 的，如果浏览器是好的，排除这种可能。

针对 DNS 解析出问题的，这个比较难排查，我卡在这个地方比较久了，后面也是逐步排查发现的，解决办法就是自己配置 www.v2ex.com 的域名解析，方法是通过修改 host:
```bash
; <<>> DiG 9.11.4-P2-RedHat-9.11.4-16.P2.el7_8.3 <<>> www.v2ex.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 3489
;; flags: qr rd ra ad; QUERY: 1, ANSWER: 3, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;www.v2ex.com.                  IN      A

;; ANSWER SECTION:
www.v2ex.com.           300     IN      A       104.20.10.218
www.v2ex.com.           300     IN      A       104.20.9.218
www.v2ex.com.           300     IN      A       172.67.3.188

;; Query time: 30 msec
;; SERVER: 8.8.8.8#53(8.8.8.8)
;; WHEN: Mon Jun 13 09:08:36 EDT 2022
;; MSG SIZE  rcvd: 89

sudo echo "www.v2ex.com 172.67.3.188" >> /etc/hosts
```

解析根据自己的情况可以修改。

### 配置 python3 环境

本人使用的是 python3 的代码开发，所以需要使用到 python3 的类库来完成网络请求，这个地方的配置稍微复杂一点。

安装 python3 的依赖库

```bash
(pandas) ➜  v2ex-alfred-workflow (main) ✗ which python
/Users/smile/miniconda3/envs/pandas/bin/python
(pandas) ➜  pip3 install -r requirements.txt
```

这一步不懂的尽量多看看百度，这个地方需要注意的是自己的 python3 的路径，后面可能也是需要修改配置的。

完成上面的几个环境准备之后，我们应该准备好下面的几个资料：
- v2ex token
- 代理
- libressl 的路径
- python3 的安装路径

## 下载 alfred workflow 至本地安装
下载之后双击安装即可。

## 配置脚本

1. 点击此位置设置自己的环境变量，token/proxy
![env](./assets/img/img-2022-06-13-21-20-57.png)

2. 在此位置分别设置自己的 `libressl` 的搜索路径和 python3 的执行路径。
![](./assets/img/img-2022-06-13-21-23-27.png)

完成上述两步的配置就可以顺利配置完成。

# 需求帮助

如果你在配置过程中遇到什么，需要技术支持请联系邮箱：Mjk0MTAxMDQyQHFxLmNvbQo=
