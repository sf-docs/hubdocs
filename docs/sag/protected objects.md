# Объекты защиты

Перечень объектов защиты:

Объект|Расположение
-|-
Директория приложения|Задаётся прикладным администратором при установке, в общем случае `/opt/apphub`. В файлах **.env**, **app.properties**, **auth.properties** содержатся логины и пароли для доступа к базе данных AppSec.Hub
Хранилище сертификатов|Задаётся прикладным администратором в файле docker-compose.yml. В общем случае `/opt/apphub/ssl`.
Журналы приложения и аудита|Директория docker-volume. В общем случае `/opt/apphub/logs`.
Файлы базы данных|Задаётся прикладным администратором в файле **docker-compose.yml**. В общем случае `/opt/apphub/postgresql/data`
Хранилище конфигурационных файлов|Задаётся прикладным администратором в файле **docker-compose.yml**. В общем случае `/opt/apphub/nginx`