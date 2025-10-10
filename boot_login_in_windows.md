# 补充一下在windows上实现开机自动认证的方法（通过powershell）

## 新建auto_login.ps1文件（自动获取ip并填充至curl）

```bash
# auto_login.ps1

Start-Sleep -Seconds 10

$ip = (Get-NetIPAddress -AddressFamily IPv4 -InterfaceAlias "WLAN" -PrefixOrigin Dhcp,Manual -ErrorAction SilentlyContinue | Where-Object { $_.IPAddress -notlike "169.254.*" } | Select-Object -First 1).IPAddress

if (-not $ip) {
    Write-Error "Can not access valid ipv4 address, exit!!!"
    exit 1
}

Write-Host "Successfully access IP: $ip"

$command = @"
& "C:\Windows\System32\curl.exe" -X GET "http://10.2.68.30:801/eportal/portal/login?callback=dr1003&login_method=1&user_account=%2C1%2C`学号`&user_password=`密码`&wlan_user_ip=$ip&wlan_user_ipv6=&wlan_user_mac=000000000000&wlan_ac_ip=&wlan_ac_name=&jsVersion=4.2.1&terminal_type=2&lang=zh-cn&v=2012&lang=zh" -H "Host: 10.2.68.30:801" -H "Connection: keep-alive" -H "User-Agent: Mozilla/5.0 (Linux; Android 10; K) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/138.0.0.0 Mobile Safari/537.36 EdgA/138.0.0.0" -H "DNT: 1" -H "Accept: */*" -H "Referer: http://10.2.68.30/" -H "Accept-Encoding: gzip, deflate" -H "Accept-Language: zh-CN,zh;q=0.9,sq;q=0.8" --compressed
"@
Invoke-Expression $command
```

需要手动更改的项：

1. 更改网卡名称：位于第二行的“WLAN”处：$ip = (Get-NetIPAddress -AddressFamily IPv4 -InterfaceAlias **"WLAN"**，在cmd里面可以通过

   ```bash
   Get-NetIPAddress -AddressFamily IPv4 | Where-Object { $_.InterfaceAlias -ne "Loopback Pseudo-Interface" } | Select InterfaceAlias, IPAddress
   ```

   来获取网卡名称和对应的ip
2. 同样地，将\`姓名\` \`学号\` 替换为自己的信息，记得把反引号一起删掉，如果密码里有特殊符号要用url编码(搜索或直接问大模型就行)

## 设置开机执行

1. 搜索并打开 “**任务计划程序**”
2. 右侧点击 “**创建任务**”
3. **常规** 选项卡：
   * 名称：校园网自动登录
   * 勾选✅“不管用户是否登录都要运行”
   * 勾选✅“使用最高权限运行”
4. **触发器** 选项卡：
   * 新建->选择 **启动时** ->确定
5. **操作** 选项卡：
   * 操作：启动程序
   * 程序或脚本：`powershell.exe`
   * 参数：`-ExecutionPolicy Bypass -File "C:\Scripts\auto_login.ps1"`（替换为你自己的路径）
   * 起始于：`C:\Scripts` （替换为你自己的路径）
6. **条件** 选项卡：
   * 取消勾选❌“**只有在使用交流电源时才启动此任务**”
   * 可以勾选✅“**唤醒计算机运行此任务**”
7. **设置** 选项卡：
   * 勾选✅“如果任务失败，重新启动任务”
   * 重试次数和间隔可以自己设定
8. 点击确定，输入密码（我输入的是微软账号的密码）
