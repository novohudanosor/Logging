# Logging
Основы сбора и хранения логов 
1. vagrant up. результатом станет 2 машины web и log
2. Настраиваем на обоих NTP что бы время везде было одинаковое.  ``` dpkg-reconfigure tzdata ```
3. Установим nginx на VM web: apt update && apt install -y nginx  и проверим, что nginx работает корректно
```
root@vagrant:~# systemctl status nginx
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
     Active: active (running) since Sun 2024-06-16 11:33:07 +04; 24s ago
       Docs: man:nginx(8)
    Process: 3483 ExecStartPre=/usr/sbin/nginx -t -q -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
    Process: 3484 ExecStart=/usr/sbin/nginx -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
   Main PID: 3575 (nginx)
      Tasks: 3 (limit: 1586)
     Memory: 4.7M
        CPU: 92ms
     CGroup: /system.slice/nginx.service
             ├─3575 "nginx: master process /usr/sbin/nginx -g daemon on; master_process on;"
             ├─3578 "nginx: worker process" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" ""
             └─3579 "nginx: worker process" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" ""

Jun 16 11:33:07 vagrant systemd[1]: Starting A high performance web server and a reverse proxy server...
Jun 16 11:33:07 vagrant systemd[1]: Started A high performance web server and a reverse proxy server.
root@vagrant:~# ss -tln | grep 80
LISTEN 0      511          0.0.0.0:80        0.0.0.0:*
LISTEN 0      511             [::]:80           [::]:*
``` 
 
