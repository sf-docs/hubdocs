# Приложение 1. Конфигурационный файл .env

Для удобства ряд системных настроек AppSec.Hub вынесен в конфигурационный файл ***.env***. Если рекомендуемая директория установки AppSec.Hub не изменена, данный файл располагается в директории `/opt/apphub/`.

Параметр|Описание|Значение по умолчанию
-|-|-
`pgsql_url`|Имя сервера (либо контейнера), где развёрнут экземпляр PostgreSQL|`postgresql`
`pgsql_port`|Порт доступа к экземпляру PostgreSQL|`5432`
`hub_db_name`|Наименование БД|`Hubdb`
`pgsql_admin_user`|Имя пользователя с административным доступом к основной схеме БД|
`pgsql_admin_password`|Пароль пользователя с административным доступом к основной схеме БД|`<Password>`
`hub_app_password`|Пароль пользователя|`<Password>`
`hub_auth_password`|Пароль пользователя для сервиса аутентификации|`<Password>`
`hub_adm_password`|Пароль пользователя AppSec.Hub с правами администратора|`<Password>`
`hub_bi_password`|Пароль пользователя с правами работы с AppSec.Hub DWH|`<Password>`
`hub_core_version`|Текущая версия hub_core|	
`hub_ui_version`|Текущая версия hub_ui|	
`hub_db_version`|Текущая версия hub_db|	
`hub_air_version`|Текущая версия hub_air|	
`IP_EXTERNAL`||`0.0.0.0`
`MODEL_USE_ENCRYPTION`||`0`
`MODEL_SECRET_KEY`||`change it`