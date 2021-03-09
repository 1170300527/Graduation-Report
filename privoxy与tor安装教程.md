# privoxy与tor安装

## 环境

1. 阿里云轻量服务器：香港节点
2. 系统：ubuntu20.04
3. python3.8.5
4. pip3

## tor

1. 安装

   ```sh
   apt-get update
   apt-get install tor git bison libexif-dev
   ```

2. 配置

   ```sh
   vim /etc/tor/torrc
   
   # 文档最后添加内容如下
   ControlPort 9051
   ControlListenAddress 127.0.0.1
   CookieAuthentication 1
   
   # 重启服务，查看tor状态
   service tor restart
   service tor status
   tor
   ```

   按照onionscan教程中配置方法也可，这个访问控制相对简单

   ## privoxy

   ```sh
   apt-get install privoxy
   
   vim /etc/privoxy/config
   
   # 添加一条转换规则，注意后面的.，一般搜索forward-socks5t，解除前面的注释就行，或者找到一条类似的修改一下，9050是tor的SocksPort
   forward-socks5t   /               127.0.0.1:9050 .
   
   # 设置可远程访问
   listen-address  0.0.0.0:8118
   
   # 由于网络不稳定，经常出现503，增加转发重试
   forwarded-connect-retries  1
   
   # 重启服务，查看状态
   service privoxy restart
   service privoxy status
   ```

   并开放服务器的8118端口

## 测试代码

```python
import os
import requests

#代理
proxies = {'http': 'http://127.0.0.1:8118', 'https': 'http://127.0.0.1:8118'}
s = requests.Session()

# 打印当前ip
r = s.get("http://api.ipify.org?format=json", proxies = proxies)
print(r.text)

# 访问暗网duckduckgo
r = s.get("https://3g2upl4pq6kufc4m.onion/", proxies = proxies)
print(r.text)

# 切换ip
os.system('service tor restart')

# 打印当前ip
r = s.get("http://api.ipify.org?format=json", proxies = proxies)
print(r.text)
```