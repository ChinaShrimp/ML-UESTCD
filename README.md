# 1.安装说明
可以选择以下任何一种方式安装机器学习课程的离线实验环境：
- vagrant (自动化)
- 手动安装虚拟机


# 2.Vagrant大法好
## 安装VirtualBox
自行百度。

## 安装Vagrant
如果你不知道(Vagrant)[https://www.vagrantup.com/]，请百度之并正确安装。

## Ubuntu镜像
确认你的宿主机上有`ubuntu/trusty64`镜像，在宿主机命令行中执行：
```
vagrant box list

输出：
ubuntu/trusty64 (virtualbox, 20171208.0.0)
```

如果没有该镜像，请从百度网盘下载到本地，然后在宿主机命令行中输入命令导入该镜像：
```
cd <包含镜像ubuntu/trusty64的目录>
vagrant box add --provider virtualbox --name ubuntu/trusty64 ubuntu-trusty.box
```

## 启动虚拟机
```
TBD
```

# 3.虚拟机
## 安装Ubuntu 14.04虚拟机
请自行下载ubuntu镜像(64位)，并正确安装

## 安装docker
```
# 更换163镜像源
sudo apt-get update
sudo apt-get install -y wget curl
sudo wget http://mirrors.163.com/.help/sources.list.trusty -O /etc/apt/sources.list
sudo apt-get update

# 安装docker依赖的软件包
sudo apt-get install -y apt-transport-https ca-certificates curl gnupg2 \
      software-properties-common    gpasswd add -a vagrant docker
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository \
      "deb [arch=amd64] https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/ubuntu \
      $(lsb_release -cs) \
      stable"
sudo apt-get update
sudo apt-get install -y docker-ce
sudo gpasswd -a vagrant docker
```

# 4. 安装机器学习环境
ssh登陆部署好的虚拟机，**确保虚拟机能够访问互联网**，之后执行以下命令:
```
cd $HOME
git clone https://github.com/ChinaShrimp/ML-UESTCD.git

docker pull registry.cn-hangzhou.aliyuncs.com/oedu/datalab:local-20180214

docker run -itd -p "0.0.0.0:8081:8080" -v "${HOME}:/content" \
    --restart=always --name=datalab  \
    registry.cn-hangzhou.aliyuncs.com/oedu/datalab:local-20180214
```

# 参考
- https://github.com/google/eng-edu/blob/master/ml/cc/README.md#with-docker
- https://console.cloud.google.com/gcr/images/cloud-datalab/GLOBAL/datalab?gcrImageListsize=50