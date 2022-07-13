# Приложение 4. Конфигурационный файл app.properties

Для удобства ряд системных настроек AppSec.Hub вынесен в конфигурационный файл **app.properties**. Если рекомендуемая директория установки AppSec.Hub не изменена, данный файл располагается в директории `/opt/apphub/config/hub-core/`. 

!!! note "Важно"
    После изменения параметров в файле **app.properties** необходимо выполнить рестарт системы, см. разделы «[Остановка AppSec.Hub](../../aag/installing%2C%20running%20and%20updating%20AppSec.Hub/#appsechub_3)» и «[Запуск AppSec.Hub](../../aag/installing%2C%20running%20and%20updating%20AppSec.Hub/#appsechub_2)» Руководства прикладного администратора.

Параметр|Описание|Значение по умолчанию
-|-|-
`db.hub.driver`|By theTh1rt3en: Класс драйвера для БД|`org.postgresql.Driver`
`db.hub.url`|Адрес экземпляра PostgreSQL|`jdbc:postgresql://$DB_HOST:$DB_PORT/$DB_NAME`
`db.hub.schema`|By theTh1rt3en: основная схема БД, в которой содержатся оперативная информация о работе хаба|`hub`
`db.hub.username`|Имя пользователя с доступом к основной схеме БД|`hubapp`
`db.hub.password`|Пароль пользователя|`<Password>`
`report.product.name`|Имя продукта|`AppSec.Hub`
`report.issue.pdf.template`|Шаблон отчета о найденных уязвимостях|`/usr/local/tomcat/webapps/hub/WEB-INF/classes/resources/issue-report-pdf-template.xsl`
`report.logo.path`|Изображение логотипа для шаблона отчета о найденных уязвимостях в формате svg|`/usr/local/tomcat/webapps/hub/WEB-INF/classes/resources/logo.svg`
`report.logo.path.png`|Изображение логотипа для шаблона отчета о найденных уязвимостях в формате png|`/usr/local/tomcat/webapps/hub/WEB-INF/classes/resources/logo.png`
`db.presentation.driver`|By theTh1rt3en: не используется, кандидат на удаление|`org.postgresql.Driver`
`db.presentation.url`|By theTh1rt3en: не используется, кандидат на удаление|`jbc:postgresql://$DB_HOST:$DB_PORT/$DB_NAME`
`db.presentation.schema`|By theTh1rt3en: не используется, кандидат на удаление|`presentation`
`db.presentation.username`|By theTh1rt3en: не используется, кандидат на удаление|`hubapp`
`db.presentation.password`|By theTh1rt3en: не используется, кандидат на удаление|`<Password>`
`db.poolSize`|Размер пула соединений с БД|`10`
`encrypt.key`|Ключ шифрования паролей, используемых для доступа к инструментам ИБ. Рекомендованный размер ключа — 32 символа|`<Random key>`
`hub.app.url`|Внешний URL экземпляра AppSec.Hub|`http://localhost`
`hub.issue-import.strategy`|By theTh1rt3en: не используется, кандидат на удаление|`${issue-import.strategy:hub-history-strategy}`
`dw.hub.importer.delay.timeMillSec`|By theTh1rt3en: не используется, кандидат на удаление|`1000`
`dw.hub.importer.delay.poolSize`|By theTh1rt3en: не используется, кандидат на удаление|`10`
`hub.dw.import.on.startup`|By theTh1rt3en: начать ли импорт данных в схему bi при старте приложения|`false`
`dw.scheduledImport.cron.expression`|Cron-выражение, задающее период импорта security issues|`0 0 1,22 * * ?`
`hub.ci.jenkins.poll.period`|Промежуток времени между обращениями AppSec.Hub к Jenkins|`10000`
`hub.ci.jenkins.poll.max.tries`|Количество обращений AppSec.Hub к Jenkins|`50`
`hub.avc.training.schedule`|Cron-выражение, задающее период тренировки модели AVC|`0 0 2 * * ?`
`export.docker.repository.url-creation-strategy`|By theTh1rt3en: стратегия создания url для репозиториев docker. два варианта: с именем репозитория и портом |`port`
`hub.jira.users.maxResults`|Максимальное количество пользователей Jira|`1000`
`hub.jira.issues.maxResults`|By theTh1rt3en: максимальное количество тикетов для экспорта|`1000`
`hub.config.importIssueThreadPoolSize`|By theTh1rt3en: размер пула потоков для импорта уязвимостей в хаб|`8`
`hub.config.importConfigThreadPoolSize`|By theTh1rt3en: размер пула потоков для экспорта уязвимостей из инструментов |`8`
`hub.config.dwImportThreadPoolSize`|количество потоков импорта данных в Data Warehouse|`6`
`hub.config.orchestrationThreadPoolSize`|количество потоков для работы с системами отслеживания дефектов|`16`
`hub.config.notificationThreadPoolSize`|количество потоков для работы с рассылаемыми уведомлениями|`4`
`hub.defect.enableDefectSynchronization`|включение/выключение автоматической синхронизации дефектов|`false (выключено)`
`hub.config.defectSynchronizationThreadPoolSize`|общее количество одновременно запускаемых потоков синхронизации дефектов. Учитываются как потоки, запущенные «вручную», так и автоматически|`4`
`hub.defect.defectSynchronizationPeriodDurationCronExpression`|Cron-выражение, задающее период синхронизации|`0 0 0/1 * * ? (каждый час)`
`hub.defect.defectSynchronizationPeriod`|количество периодов синхронизации, заданных предыдущим параметром — общий период синхронизации определяется произведением значений двух последних параметров. Например, если значение параметра `defect.defectSynchronizationPeriodDurationCronExpression` оставить по умолчанию (1 час), а для параметра `defect.defectSynchronizationPeriod` задать значение «2», автоматическая синхронизация будет осуществляться каждые два часа|`1`