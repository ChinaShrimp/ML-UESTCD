本文档采用虚拟机手动安装机器学习离线实验环境。
# 1.准备工作
## 下载
百度网盘地址
```
```

## 安装虚拟机软件
VirtualBox/VMWare都可以，自行百度。


# 2.虚拟机安装
## 安装Ubuntu 14.04虚拟机
Ubuntu 14.04虚拟机镜像位于上述百度网盘链接中的Ubuntu14.04ISO目录中，也可以自行下载[ubuntu镜像](http://mirrors.163.com/ubuntu-releases/14.04.5/ubuntu-14.04.5-server-amd64.iso)。

下载好镜像后，使用虚拟机软件(VMware/Virtualbox/...)正确安装一个虚拟机，并运行。

正确配置虚拟机，确保虚拟机能够访问互联网。

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

# 3. 启动datalab
ssh登陆部署好的虚拟机，**确保虚拟机能够访问互联网**，之后执行以下命令:
```
# 获取datalab镜像
docker pull registry.cn-hangzhou.aliyuncs.com/oedu/datalab:local-20180214

cd $HOME
git clone https://github.com/ChinaShrimp/ML-UESTCD.git

# 启动过程中会同步github仓库，会耗时，耐心
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