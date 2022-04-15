# Приложение. Список событий аудита

<figure markdown>
Событие|Описание
-|-
`application.created`|Новое приложение было добавлено в AppSec.Hub
`application.devsecops.activated`|Проверки ИБ были включены для определённого приложения
`application.devsecops.deactivated`|Проверки ИБ были выключены для определённого приложения
`application.group.assigned`|Доступ к приложению был открыт для команды
`application.group.revoked`|Доступ к приложению был закрыт для команды
`application.integrity.activated`|Функция «Integrity check» была включена для приложения
`application.integrity.deactivated`|Функция «Integrity check» была выключена для приложения
`application.workspace.updated`|Приложение было переведено в другое рабочее пространство (workspace)
`authentication.login`|Пользователь попытался войти в AppSec.Hub:<br>– eventResult: success — пользователь был успешно авторизован;<br>– eventResult: fail — пользователю отказано в доступе (например, ввёл некорректные логин/пароль)
`authentication.logout`|Пользователь вышел из AppSec.Hub
`defect.created`|Создан дефект ИБ
`defect.deleted`|Удалён дефект ИБ
`group.created`|Создана команда
`group.deleted`|Удалена команда
`group.user.assigned`|Пользователь добавлен в команду
`group.user.revoked`|Пользователь удалён из команды
`hub.started`|Система AppSec.Hub запущена
`issues.imported`|Уязвимости импортированы из инструментов ИБ
`ldap.activated`|Интеграция с LDAP сервером включена
`ldap.deactivated`|Интеграция с LDAP сервером отключена
`ldap.mapping.groups.updated`|Обновлено соответствие групп AD с командами (Hub)
`ldap.mapping.roles.updated`|Обновлено соответствие групп AD с ролями пользователя (Hub)
`ldap.mapping.users.updated`|Обновлено соответствие атрибутов записи AD с атрибутами пользователя (Hub)
`ldap.provisioning.activated`|Включено автоматическое добавление пользователей из AD в Hub
`ldap.provisioning.deactivated`|Выключено автоматическое добавление пользователей из AD в Hub
`ldap.updated`|Обновлены параметры соединения с LDAP сервером
`license.expired`|Закончилась лицензия на систему AppSec.Hub
`license.updated`|Загружена новая, корректная лицензия на систему AppSec.Hub
`pipeline.created`|Создан DevSecOps пайплайн
`pipeline.deleted`|Удалён DevSecOps пайплайн
`pipeline.exported`|DevSecOps пайплайн выгружен в CI инструмент
`tool.activated`|Интеграция с инструментом включена
`tool.connection.test`|Проверка соединения с инструментом выполнена
`tool.created`|Интеграция с инструментом добавлена
`tool.credentials.updated`|Данные аутентификации в инструменте были обновлены
`tool.deactivated`|Интеграция с инструментом выключена
`tracking.activated`|Интеграция с дефект-трекером включена для определенного приложения
`tracking.created`|Создана интеграция с дефект-трекером для определенного приложения
`tracking.deactivated`|Интеграция с дефект-трекером выключена для определенного приложения
`tracking.deleted`|Удалена интеграция с дефект-трекером для определенного приложения
`tracking.sync`|Выполнена синхронизация дефектов с трекером
`tracking.updated`|Обновлена интеграция с дефект-трекером для определенного приложения
`user.activated`|Учётная запись пользователя активирована 
`user.created`|Учётная запись пользователя добавлена в систему AppSec.Hub
`user.deactivated`|Учётная запись пользователя заблокирована
`user.password.changed`|	Изменён пароль учётной записи пользователя
`user.role.assigned`|Пользователю добавлена роль
`user.role.revoked`|У пользователя удалена роль
`workspace.created`|Рабочее пространство создано
`workspace.deleted`|Рабочее пространство удалено
`workspace.group.assigned`|Доступ к рабочему пространству был открыт для команды
`workspace.group.revoked`|Доступ к рабочему пространству был закрыт для команды
`workspace.tool.assigned`|Инструмент был добавлен в рабочее пространство
`workspace.tool.revoked`|Инструмент был удалён из рабочего пространства

</firure>