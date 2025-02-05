# sockd

## 使用方法

### 镜像准备

1、创建GITHUB账号，fork 此仓库；

2、在腾讯云的容器镜像服务中，开通服务，创建实例，确认对应地域（香港或海外指定区域）有默认共享实例，通过设置默认共享实例的密码，初始化默认实例；

3、在云托管控制台中，开通云开发云托管服务，入口地址：https://console.cloud.tencent.com/tcb；
   在弹性服务实例中，选择好地域，创建环境：服务入口：https://console.cloud.tencent.com/tcbr/elasticservice

3、在镜像仓库中应该有一个步骤3中环境名开头的镜像，进入到这个镜像中；

4、在镜像的详情中，进入镜像构建，开通Coding构建流水线，新建镜像构建规则；

5、选择Github，授权github，使用步骤1中创建的账号，选择这个账号中 fork 出来的cloudbaserun_app项目

6、按如下配置镜像构建规则：
    触发规则分支选择main；
    Dockerfile路径填写：ssr/shadowsocksr-origin/Dockerfile
    构建目录：ssr/shadowsocksr-origin

7、通过立即构建，启动镜像构建，等待构建日志中显示成功；构建成功后就可以去实际启动服务了；


镜像仅需要准备一次，后续在多个实例启动中可以复用镜像。

### 实例启动

1、使用前置步骤中构建成功的进行启动云开发弹性服务实例，选择镜像时可以通过悬停查看镜像的完整名称显示，确保和前置步骤的可以对上；

* 默认端口 51348，默认密码 123456，默认加密方式 aes-128-ctr，默认协议 auth_aes128_md5，默认混淆 tls1.2_ticket_auth
* 如果需要修改默认配置，可以根据设置项，设置实例启动时的环境变量；

2、实例启动后，获取实例外部IP；

3、通过第2步的端口，用户名，密码，第3步的IP，配置 ssr 客户端，例如sstap，测试客户端连通性；

### 实例修改

进入到需要修改的实例中，通过实例设置，可以修改环境变量，按需修改端口、用户名、密码，设置规则如设置项说明；

通过更新服务实例，也可以选择新的镜像进行更新部署；

任何更新都会触发实例的重启和重设置；


## 设置项：

通过环境变量设置相关应用配置：

* SERVER_PORT：默认 51348，可修改；
* PASSWORD：默认 123456，可修改；
* METHOD：默认 aes-128-ctr，可修改，建议一般情况下不调整
* PROTOCOL：默认 auth_aes128_md5，可修改，建议一般情况下不调整
* OBFS：：默认 tls1.2_ticket_auth_compatible，可修改，建议一般情况下不调整

## 注意事项

**服务启动失败**

查看服务发布日志，检查日志中是否有失败信息，错误信息。

**客户端连接代理端失败**

IP、端口、用户名、密码是否正确。

可以通过 telnet命令（Windows，linux）、nc命令（mac）测试IP和端口连通性：

telnet用法：telnet xxx.xxx.xxx.xxx 51348

nc用法：nc -vz xxx.xxx.xxx.xxx 51348

**需要重启实例**

可以通过设置实例，无论是更新服务实例，还是新增一个任意环境变量，都会触发实例重启；


