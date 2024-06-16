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
4. Настройка центрального сервера сбора логов   на машине **log**
5. так же установим правильное время из под root
6.  ``` apt list rsyslog ```
7.  Все настройки Rsyslog хранятся в файле /etc/rsyslog.conf  - правим файл
8.  Открываем порт 514 (TCP и UDP): Находим закомментированные строки:
9.   ![alt text](./Pictures/1.png)
```
root@vagrant:~# systemctl restart rsyslog
root@vagrant:~# ss -tuln
Netid                State                 Recv-Q                Send-Q                                Local Address:Port                               Peer Address:Port               Process
udp                  UNCONN                0                     0                                           0.0.0.0:514                                     0.0.0.0:*
udp                  UNCONN                0                     0                                     127.0.0.53%lo:53                                      0.0.0.0:*
udp                  UNCONN                0                     0                                    10.0.2.15%eth0:68                                      0.0.0.0:*
udp                  UNCONN                0                     0                                              [::]:514                                        [::]:*
tcp                  LISTEN                0                     25                                          0.0.0.0:514                                     0.0.0.0:*
tcp                  LISTEN                0                     4096                                  127.0.0.53%lo:53                                      0.0.0.0:*
tcp                  LISTEN                0                     128                                         0.0.0.0:22                                      0.0.0.0:*
tcp                  LISTEN                0                     25                                             [::]:514                                        [::]:*
tcp                  LISTEN                0                     128                                            [::]:22                                         [::]:*
root@vagrant:~# systemctl status rsyslog
● rsyslog.service - System Logging Service
     Loaded: loaded (/lib/systemd/system/rsyslog.service; enabled; vendor preset: enabled)
     Active: active (running) since Sun 2024-06-16 11:55:13 +04; 32s ago
```
10. 

 
