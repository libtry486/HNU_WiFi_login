如果使用校园网的电信出口，可以使用下面的crul：
就只是在学号后面加了一个@telecom，不加的话默认是校园网出口
```
curl -X GET 'http://10.2.68.30:801/eportal/portal/login?callback=dr1003&login_method=1&user_account=%2C1%2C`学号`%40telecom&user_password=`统一认证密码`&wlan_user_ip=`你当前连接到局域网的ipv4地址`&wlan_user_ipv6=&wlan_user_mac=000000000000&wlan_ac_ip=&wlan_ac_name=&jsVersion=4.2.1&terminal_type=2&lang=zh-cn&v=2012&lang=zh'
-H 'Host: 10.2.68.30:801'
-H 'Connection: keep-alive'
-H 'User-Agent: Mozilla/5.0 (Linux; Android 10; K) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/138.0.0.0 Mobile Safari/537.36 EdgA/138.0.0.0'
-H 'DNT: 1'
-H 'Accept: /'
-H 'Referer: http://10.2.68.30/'
-H 'Accept-Encoding: gzip, deflate'
-H 'Accept-Language: zh-CN,zh;q=0.9,sq;q=0.8'
--compressed
```
