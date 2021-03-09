# onionscan安装教程

## 环境

1. 阿里云轻量服务器：香港节点
2. 系统：ubuntu20.04
3. python3.8.5
4. pip3

## 安装

1. 更新软件

   ```sh
   apt update
   apt upgrade
   ```

2. 安装tor与python库

   ```sh
   apt install tor git bison libexif-dev
   pip3 install stem
   ```

3. 因为OnionScan是采用Go语言进行编写的，所以需要安装go

   ```sh
   bash < <(curl -s -S -L https://raw.githubusercontent.com/moovweb/gvm/master/binscripts/gvm-installer)
   [[ -s "$HOME/.gvm/scripts/gvm" ]] && source "$HOME/.gvm/scripts/gvm"
   source /root/.gvm/scripts/gvm
   gvm install go1.15 —binary
   gvm use go1.15
   //安装go后就可以安装onionscan了
   go get github.com/s-rah/onionscan
   go install github.com/s-rah/onionscan
   ```

   之后在终端输入onionscan执行成功说明安装成功

4. 设置tor

   首先在终端输入

   ```sh
   tor --hash-password PythonRocks
   ```

   终端会输出一段数据类似：

   `16:3E73307B3E434914604C25C498FBE5F9B3A3AE2FB97DAF70616591AAF8`

   复制下数据之后执行

   ```sh
   vim /etc/tor/torrc
   ```

   在结尾添加

   ```
   ControlPort 9051
   ControlListenAddress 127.0.0.1
   HashedControlPassword 16:3E73307B3E434914604C25C498FBE5F9B3A3AE2FB97DAF70616591AAF8
   ```

   保存冰退出后执行以下命令重启tor

   ```sh
   service tor restart
   ```

## 运行

[GitHub地址](https://github.com/1170300527/Graduation-Project.git)

下载其中的onion_master_list.txt和对应版本的onionrunner.py(python2)或onionrunner3.py(python3)

放在同一目录下运行即可