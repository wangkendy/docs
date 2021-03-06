#  安装步骤



## 安装tke-installer

#### 在线环境：

登录到您的 Linux 主机，然后通过以下命令安装 tke-installer：

```
version=v1.1.0 && wget https://tke-release-1251707795.cos.ap-guangzhou.myqcloud.com/tke-installer-x86_64-$version.run{,.sha256} && sha256sum --check --status tke-installer-x86_64-$version.run.sha256 && chmod +x tke-installer-x86_64-$version.run && ./tke-installer-x86_64-$version.run
```

注：此命令可以在 [TKEStack版本详情 ](https://github.com/tkestack/tke/releases)中找到，您可以按需选择版本进行安装，我们建议您安装最新版本。

注：tke-installer 约为 3GB，包含安装所需的所有资源。



#### 离线环境：

1.在一台可以联网的主机上通过以下命令下载tke-installer安装包：

```
 version=v1.1.0 && wget https://tke-release-1251707795.cos.ap-guangzhou.myqcloud.com/tke-installer-x86_64-$version.run{,.sha256}
```

2.将安装包使用 rz 或 scp 工具传入离线目标主机。

3.在离线目标主机通过以下命令安装 tke-installer：

```
 version=v1.1.0 && sha256sum --check --status tke-installer-x86_64-$version.run.sha256 && chmod +x tke-installer-x86_64-$version.run && ./tke-installer-x86_64-$version.run
```



## 登录控制台安装界面

访问 http:[tke-installer-IP]:8080/index.html 访问控制台，开始安装。



## 安装TKEStack控制台

#### 1.**基本设置**：

配置 TKEStack 控制台的账户信息及 APIServer 高可用类型。

注：高可用设置为” TKE 提供“，会为您在 master 节点额外安装 Keepalived 完成 VIP 配置，需要配置一个内网可用IP；高可用设置为”使用已有“，需要您手动对接负载均衡器的 VIP 实例。

![](https://github.com/tkestack/docs/blob/master/Images/Installration/step-1.png?raw=true)



#### 2.**集群设置**：

填写Global集群的信息，包括网卡名称、节点 GPU 类型、容器网络、master 节点信息以及高射设置。

注：请填写master节点内网IP。

![](https://github.com/tkestack/docs/blob/master/Images/Installration/step-2.png?raw=true)

高级设置处可以自定义 Global 集群的 Docker、kube-apiserver、kube-controller-manager、kube-scheduler、kubelet 运行参数。

![](https://github.com/interstallers/docs/blob/master/Images/Installration/step-3-2.png?raw=true)



#### 3.**认证设置**：

填写 TKEStack 控制台的认证信息，可以选择使用 TKE 提供的认证方式或对接 OIDC 认证授权系统。

![](https://github.com/tkestack/docs/blob/master/Images/Installration/step-3-1.png?raw=true)



#### 4.**镜像仓库设置**：

配置 TKEStack 的镜像仓库，它是集群初始化的源镜像仓库，支持本地和远程仓库。

注：如果选择第三方仓库 TKE 将不会安装镜像仓库服务，而是使用您提供的镜像仓库作为默认镜像仓库服务。

![](https://github.com/tkestack/docs/blob/master/Images/Installration/step-4.png?raw=true)



#### 5.**业务设置**：

确认是否开启业务模块，建议开启。

![](https://github.com/tkestack/docs/blob/master/Images/Installration/step-5.png?raw=true)



#### 6.**监控设置**：

选择监控的存储类型，可选 TKE 提供的 influxDB，或者选择对接外部 influxDB、ES。

![](https://github.com/tkestack/docs/blob/master/Images/Installration/step-6.png?raw=true)



#### 7.**控制台设置**：

确认是否开启集群控制台，确认开启则填写控制台的已备案域名并填写服务器证书。建议开启。

![](https://github.com/tkestack/docs/blob/master/Images/Installration/step-7.png?raw=true)



#### 8.**配置预览**：

确认配置是否正确。

![](https://github.com/tkestack/docs/blob/master/Images/Installration/step-8.png?raw=true)



#### 9.**开始安装**



## 安装常见问题

#### 1.密码安装报错

错误情况：使用密码安装Global集群报 ssh:unable to authenticate 错误。

解决方案：将Global集群节点/etc/ssh/sshd_config配置文件中的PasswordAuthentication设为yes，重启sshd服务。

注：建议配置SSH key的方式安装Global集群。



#### 2.初始化Installer控制台信息

安装报错后，清先将问题解决，再登录到Installer节点执行如下命令开始重新安装：

```
rm -rf /opt/tke-installer/data && docker restart tke-installer
```

注：想要彻底清理installer节点，请执行下方脚本。



#### 3.清理Global集群

在Global集群节点上执行如下命令：

```
curl -s https://tke-release-1251707795.cos.ap-guangzhou.myqcloud.com/tools/clean.sh | sh
```

注：该脚本仅适用于Global集群专用节点，如有混合部署其他业务，请基于实际情况评估目录内数据是否可删除。