# 适用于2025年7月期间湖南大学校园网更新

原作者的命令：

```
curl -X GET 'http://10.2.68.30:801/eportal/portal/login?callback=dr1003&login_method=1&user_account=%2C1%2C`学号`&user_password=`统一认证密码`&wlan_user_ip=`你当前连接到局域网的ipv4地址`&wlan_user_ipv6=&wlan_user_mac=000000000000&wlan_ac_ip=&wlan_ac_name=&jsVersion=4.2.1&terminal_type=2&lang=zh-cn&v=2012&lang=zh' \
  -H 'Host: 10.2.68.30:801' \
  -H 'Connection: keep-alive' \
  -H 'User-Agent: Mozilla/5.0 (Linux; Android 10; K) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/138.0.0.0 Mobile Safari/537.36 EdgA/138.0.0.0' \
  -H 'DNT: 1' \
  -H 'Accept: */*' \
  -H 'Referer: http://10.2.68.30/' \
  -H 'Accept-Encoding: gzip, deflate' \
  -H 'Accept-Language: zh-CN,zh;q=0.9,sq;q=0.8'  \
   --compressed
```

把命令中的  \`中文\` 都替换成自己的信息就ok了（密码中如果有符号不要忘记进行url编码哦

# 我针对Windows开机自动认证和Ubuntu定期认证写了详细教程：

[Windows开机自动认证](boot_login_in_windows.md) （电脑有时关机后开机就要重新登录很烦）

[Ubuntu定期认证](regular_login_in_ubuntu.md) (插网线的服务器认证后保持了几天后就要重新认证)
