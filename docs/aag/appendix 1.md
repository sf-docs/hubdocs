# Приложение 1. Список параметров системы, заполняемыx в процессе установки

Параметр|Описание|Значение по умолчанию
-|-|-
`pgsql_url`|Имя сервера (либо контейнера), где развёрнут экземпляр PostgreSQL|`postgresql`
`pgsql_port`|Порт доступа к экземпляру PostgreSQL|`5432`
`hub_db_name`|Наименование БД|`Hubdb`
`hub_app_user`|Имя пользователя с доступом к основной схеме БД|`hubapp`
`hub_app_password`|Пароль пользователя|`<Password>`
`hub_auth_user`|Имя пользователя для сервиса аутентификации|`Hubauth`
`hub_auth_password`|Пароль пользователя|`<Password>`
`hub_db_pool_size`|Размер пула соединений с БД|`10`
`hub_app_url`|Внешний URL экземпляра AppSec.Hub|`http://localhost`
`hub_encrypt_key`|Ключ шифрования паролей используемых для доступа к инструментам ИБ. Рекомендованный размер ключа — 32 символа|`<Random key>`
`hub_token_key`|Ключ подписи пользовательских токенов для доступа в AppSec.Hub без логина/пароля|`<Random key>`