本文档采用Vagrant安装机器学习离线实验环境。
# 准备工作
## 安装[VirtualBox](https://www.virtualbox.org/)
自行百度。

## 安装Vagrant
如果你不知道[Vagrant](https://www.vagrantup.com/)，请百度之并正确安装。

## 下载文件
百度网盘链接：
```
链接: https://pan.baidu.com/s/1jJYNEom 
密码: aabv
```
将上述链接中的文件下载到宿主机

# 创建虚拟机
## 导入datalab.box：
`datalab.box`位于上述百度网盘链接中的`Vagrant/Images/datalab`目录中。
```
cd <包含datalab.box的目录>
vagrant box add --provider virtualbox --name datalab datalab.box
```

## 启动虚拟机
`Vagrantfile`在上述百度网盘地址所指定的`datalabvm`目录中：
```
cd <包含Vagrantfile的目录>
vagrant up
```

启动后，vagrant会报错：
```
$ vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Importing base box 'datalab'...

==> default: Matching MAC address for NAT networking...
==> default: Setting the name of the VM: datalab_default_1520098943224_37218
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
==> default: Forwarding ports...
    default: 8081 (guest) => 8081 (host) (adapter 1)
    default: 22 (guest) => 2222 (host) (adapter 1)
==> default: Running 'pre-boot' VM customizations...
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: vagrant
    default: SSH auth method: private key
    default: Warning: Authentication failure. Retrying...
    default: Warning: Authentication failure. Retrying...
    default: Warning: Authentication failure. Retrying...
```

解决上述鉴权失败的办法，在`Vagrantfile`所在的目录执行`vagrant ssh`，密码是`vagrant`。登陆到虚拟机之后，执行以下命令：
```
rm -rf /home/vagrant/.ssh/*
wget --no-check-certificate https://raw.github.com/mitchellh/vagrant/master/keys/vagrant.pub -O /home/vagrant/.ssh/authorized_keys
chmod 0700 /home/vagrant/.ssh  
chmod 0600 /home/vagrant/.ssh/authorized_keys  
chown -R vagrant /home/vagrant/.ssh
sudo reboot
```

## 启动datalab
```
cd <包含Vagrantfile的目录>
vagrant ssh
git clone https://github.com/ChinaShrimp/ML-UESTCD.git

docker run -itd -p "0.0.0.0:8081:8080" -v "${HOME}:/content" \
    --restart=always --name=datalab  \
    registry.cn-hangzhou.aliyuncs.com/oedu/datalab:local-20180214
```

在虚拟机中执行`docker logs datalab`，当看到如下日志时表明环境已经准备好：
```
...
Starting Datalab in silent mode, for debug output, rerun with an additional '-e DATALAB_DEBUG=true' argument
Open your browser to http://localhost:8081/ to connect to Datalab.
```
此时可以打开浏览器，在地址栏输入`http://localhost:8081`, 恭喜！


# 参考
- https://github.com/google/eng-edu/blob/master/ml/cc/README.md#with-docker
- https://console.cloud.google.com/gcr/images/cloud-datalab/GLOBAL/datalab?gcrImageListsize=50