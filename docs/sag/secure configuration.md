# Безопасная конфигурация

## Сетевая конфигурация

Для установки и дальнейшего функционирования системы должны быть разрешены внутренние сетевые подключения, указанные в разделе «[Настройка HTTPS соединения]()».

## Настройка HTTPS соединения

Для настройки HTTPS соединения необходимо получить подписанные удостоверяющим центром сертификаты (открытый и закрытый) в формате PEM. Шаги настройки:

* Полученные сертификаты положить в папку /opt/apphub/ssl:

        $ ls -l ./ssl/
        total 16
        -rw-r--r-- 1 root root 1277 Mar 9 14:33 hub.crt
        -rw------- 1 root root 1679 Mar 9 14:33 hub.key

* Изменить конфигурацию запуска hub-ui, в секцию volumes добавить папку, в которой будут храниться ключ и сертификат:

        volumes:
        - ./nginx/hub.conf:/etc/nginx/conf.d/default.conf:ro
        - ./logs/hub-ui:/var/log/nginx
        - ./ssl:/etc/ssl/certs/ssl-cert:ro

* Изменить конфигурацию /opt/apphub/nginx/hub.conf (hub-ui), для использования SSL:

        listen 443 ssl;
        ssl_certificate /etc/ssl/certs/ssl-cert/hub.crt; # certificate
        ssl_certificate_key /etc/ssl/certs/ssl-cert/hub.key; # private key
        if ($scheme != "https") { # redirect 80 to 443
        return 301 https://$host$request_uri;
        }
