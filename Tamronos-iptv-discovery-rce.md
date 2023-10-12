# Tamronos-iptv-pppoediscovery-rce


The TamronOS IPTV system has command execution vulnerabilities

official website：http://www.tamronos.com/


## Vulnerability Details

The `pppoediscovery` method in the `iptv/Controllers/WidgetController.php` file does not perform filtering verification on the `$dev` passed in by the user. Instead, it is directly passed into function `shell_exec()` and can be used `;` Truncating the command resulted in command injection.
![pppoediscovery](https://github.com/yuxianzi/Tamronos-iptv-CVE/assets/41229220/877ad25f-1a6c-4273-aeb7-027630912987)


The path to access the method is defined in the routes.php file of the program.

![routes](https://github.com/yuxianzi/Tamronos-iptv-CVE/assets/41229220/3136091b-1731-46de-83f2-407c73806c14)


Access the `/pppoe/discovery` path to call the `pppoeiscovery` method


## Recurrence vulnerability


Test version : `TamronOS IPTV 8.6.3`
![version](https://github.com/yuxianzi/Tamronos-iptv-CVE/assets/41229220/24d3629b-35ff-4880-a267-0a270d9df14b)

request `http://172.16.49.166/pppoe/discovery?dev=;id;`

![request](https://github.com/yuxianzi/Tamronos-iptv-CVE/assets/41229220/5d3609f7-6639-465c-830e-148a469060f3)


