# 在ubuntu中实现定期登录

## 新建空的日志文件login.log用于存放定期执行的输出

```bash
touch login.log
```

## 新建login.sh

```bash

#!/bin/bash

# 日志文件路径（自定义）
LOG_FILE="/home/xxx/scripts/login.log"

TIMESTAMP=$(date '+%Y-%m-%d %H:%M:%S')

# 获取 IPv4 地址（修改网卡名称enp3s0为自己的网卡名称）
IPV4=$(ip -4 addr show enp3s0 | grep -oP '(?<=inet\s)\d+(\.\d+){3}' || echo "NO_IP")

if [[ -z "$IPV4" || "$IPV4" == "NO_IP" ]]; then
    echo "[$TIMESTAMP] ERROR: Failed to get IPv4 address" >> "$LOG_FILE"
    exit 1
fi

# 执行认证（修改学号和密码，反引号要一起删掉，如有特殊符号要转换为url编码）
RESPONSE=$(curl -s -w "\nHTTP_CODE:%{http_code}\n" -X GET \
  "http://10.2.68.30:801/eportal/portal/login?callback=dr1003&login_method=1&user_account=%2C1%2C`学号`&user_password=`密码`&wlan_user_ip=$IPV4&wlan_user_ipv6=&wlan_user_mac=000000000000&wlan_ac_ip=&wlan_ac_name=&jsVersion=4.2.1&terminal_type=2&lang=zh-cn&v=2012&lang=zh" \
  -H 'Host: 10.2.68.30:801' \
  -H 'Connection: keep-alive' \
  -H 'User-Agent: Mozilla/5.0 (Linux; Android 10; K) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/138.0.0.0 Mobile Safari/537.36 EdgA/138.0.0.0' \
  -H 'DNT: 1' \
  -H 'Accept: */*' \
  -H 'Referer: http://10.2.68.30/' \
  -H 'Accept-Encoding: gzip, deflate' \
  -H 'Accept-Language: zh-CN,zh;q=0.9,sq;q=0.8' \
  --compressed 2>&1)

{
  echo "[$TIMESTAMP] Attempting login with IP: $IPV4"
  echo "$RESPONSE"
  echo "--------------------------------------------------"
} >> "$LOG_FILE"
```

上述文件需要修改的地方：

1. 将LOG_FILE改为自己创建的日志文件路径，注意不要使用"~"
2. 将“IPV4=”那一行里面的网卡名称修改为自己的网卡名称（可通过ifconfig获取，我的网卡名为enp3s0）
3. 修改curl命令里面的学号和密码

## 设置定期自动执行

```bash
crontab -e
```

如果首次执行需要选编辑器，选自己熟悉的就行，然后添加一行：

```bash
*/1 * * * * /home/xxx/scripts/login.sh
```

意思是每隔一分钟执行一次，先测试一下是否运行成功，一分钟后打开login.log文件看一眼，后续可以设置成每个整点执行一次：

```bash
0 * * * * /home/xxx/scripts/login.sh
```

## 如果curl报错找不到

因为cron不会执行~/.bashrc，比如我的curl是位于anaconda里面的，所以需要把curl替换为命令的实际路径：/home/xxx/anaconda3/bin/curl
