# Приложение 1. Описание параметров запуска скриптов On-boarding

## Общие параметры для скриптов scan_codebase.py и scan_ artifact.py

!!! note "Примечание"
    Корректное завершение работы скриптов возможно, только если указаны все необходимые параметры.

Параметр|Обязат.<br>параметр<br>|Описание|Пример
-|:-:|-|-
`--url`|+|URL приложения AppSec.Hub|`--url https://hub.demo.swordfishsecurity.com`
`--token` |+ |Токен авторизации AppSec.Hub| `--token <token>`
`--appcode`|+|Код приложения в БД AppSec.Hub|`--appcode dvja`
`--tagged-artifact`|–|Артефакт, тегируемый в результате сканирования. Обязательный параметр, если соответствующее действие добавлено в Security Pipeline, в противном случае — игнорируется|`--tagged-artifact https://nexus.dev.swordfishsecurity.com/ repository/maven-releases/com/appsecco/dvja/1.0/dvja-1.0.war`
`--insecure`|–|Отключает проверки безопасности (например, проверку SSL-сертификатов). По умолчанию —активирован|`--insecure`
`--no-wait`|–|Если параметр задан, CLI не ожидает окончания сканирования|`--no-wait`
`--debug`|–|Если параметр задан, «log level» — DEBUG. По умолчанию «log level» — INFO|`--debug`
`--error`|–|Если параметр задан, «log level» — ERROR. По умолчанию «log level» — INFO|`--error`
`--result-file`|–|Определяет имя и расположение JSON-файла с результатами сканирования*|`--result-file ./result.json`
`--scan-initiator`|–|Информация об инициаторе сканирования (например, ссылка на соответствующую задачу TeamCity)|`--scan-initiator https://demo.teamcity...`
`--scan-initiator-environment`|–|Среда окружения инициатора сканирования|`--scan-initiator-environment some information`
`--unit`|–|Устанавливает соответствие между артефактом/кодовой базой и экземпляром приложения. Если объект не существует, он создается|`--unit Front`
`--external-id`|–|Внешний идентификатор (external id) нового приложения. Если приложение существует, параметр будет пропущен|`--external-id new-app-external-id`
`-v`|–|Отображение версии скрипта|`-v`

* см. раздел «[Приложение 3. Результаты сканирования]()».

## Специфические параметры скрипта scan_codebase.py

Параметр|Обязат.<br>параметр|Описание|Пример
-|:-:|-|-
`--codebase-url`|+|URL репозитория основной кодовой базы|`--codebase-url https://github.com/appsecco/dvja.git`
`--branch`|–|Ветвь репозитория или тэг основной кодовой базы. Значение по умолчанию — master|`--branch master`
`--branch-filter`|–|Позволяет осуществить фильтрацию ветвей для сканирования. Фильтр применяется как к основной, так и к дополнительным кодовым базам. Значение по умолчанию — master|`--branch-filter feature/*`<br>  `--branch-filter *`<br>  `--branch-filter develop`  
`--codebase-name`|–|Имя кодовой базы в AppSec.Hub. Back-end AppSec.Hub должен использовать параметры appcode, `codebase-url` и `branch`|`--codebase-name dvja-master`
`--codebase-type`|–|Тип системы контроля версий. Значение по умолчанию — git|`--codebase-type git`
`--additional-codebase-urls`|–|URL репозитория дополнительной кодовой базы — может содержать несколько ссылок, разделенных пробелом|`--additional-codebase-urls http://github.com/appsecco/dvja1.git http://github.com/appsecco/dvja2.git`
`--additional-branches`|–|Тег или ветвь дополнительной кодовой базы. Значение по умолчанию — master. Вспомогательный параметр. Если он не определен или указано единственное значение, оно применяется ко всем дополнительным кодовым базам, заданным параметром additional_codebase_urls|`--additional-branches maser develop`
`--codebase-build-tool`|–|Инструмент сборки. Допустимые значения: maven, gradle, nuget, npm, pip. Значение по умолчанию — maven|`--codebase-build-tool maven`

## Специфические параметры скрипта scan_ artifact.py

Параметр|Обязат.<br>параметр|Описание|Пример
-|:-:|-|-
`--artifact-url`|–|URL артефакта, хранящегося в Nexus RM или файловом хранилище. Если URL указан, другие параметры типов артефактов, игнорируются|`--artifact-url https://nexus.dev.swordfishsecurity.com/<br>repository/web-maven-test/com/mkyong/web/java-web-project/1.15/<br>java-web-project-1.15.war`
`--artifact-type`|–|Обязательный параметр, если не указан artifact-url, в противном случае — вспомогательный. Допустимые значения: maven, docker, file_storage, npm, yum, raw|`--artifact-type maven`
`--artifact-name`|–|Имя артефакта в AppSec.Hub|`--name java-web-project-war`

#### Параметры для артефакта Maven (type: maven)

Параметр|Обязат.<br>параметр|Описание|Пример
-|:-:|-|-
`--repository-url`|+|URL репозитория артефакта|`--repository_url https://nexus.dev.swordfishsecurity.com/repository/web-maven-test`
`--repository-name`|+|Имя репозитория|`--repository_name web-maven-test`
`--maven-group`|+|Идентификатор группы Maven|`--maven-group com.mkyong.web`
`--maven-artifact`|+|Идентификатор артефакта Maven|`--maven-artifact java-web-project`
`--maven-classifier`|–|Классификатор Maven|`--maven-classifier SNAPSHOT`
`--maven-extension`|+|Тип файла или расширение|`--maven-extension war`
`--version`|+|Версия артефакта|`--version 1.15`

#### Параметры для артефакта Docker (type: docker)

Параметр|Обязат.<br>параметр|Описание|Пример
-|:-:|-|-
`--docker-registry`|+|Имя хоста регистра Docker|`--docker-registry nexus.dev.swordfishsecurity.com`
`--docker-registry-port`|–|Порт регистра Docker registry. Значение по умолчанию — 443|`--docker-registry-port 8096`
`--docker-image`|+|Имя образа/контейнера Docker|`--docker-image java-web-project`
`--version`|+|Версия артефакта|`--version 1.15`

#### Параметры для артефакта NPM (type: npm)

Параметр|Обязат.<br>параметр|Описание|Пример
-|:-:|-|-
`--repository-url`|+|URL репозитория артефакта|`--repository-url https://nexus.dev.swordfishsecurity.com/repository/npm-private/`
`--repository-name`|+|Имя репозитория|`--repository-name npm-private`
`--package-name`|+|Имя пакета NodeJS|`--package-name ngclipboard`
`--version`|+|Версия артефакта|`--version 2.0.0`

#### Параметры для артефакта Yum (type: yum)

Параметр|Обязат.<br>параметр|Описание|Пример
-|:-:|-|-
`--repository-url`|+|URL репозитория артефакта|`--repository_url https://nexus.dev.swordfishsecurity.com/repository/web-yum-test`
`--repository-name`|+|Имя репозитория|`--repository_name web-yum-test`
`--relative-url`|+|Относительный URL, включая имя файла|`--relative-url web/web-${artifactVersion}.rpm`
`--component-name`|+|Имя компонента|`--component-name web`
`--version`|+|Версия артефакта|`--version 1.15`
`--build`|–|Версия сборки артефакта|`--build 12345`

#### Параметры для артефакта RAW (type: raw)

Параметр|Обязат.<br>параметр|Описание|Пример
-|:-:|-|-
`--repository-url`|+|URL репозитория артефакта|`--repository-url https://nexus.dev.swordfishsecurity.com/<br>repository/web-raw-test`
`--repository-name`|+|Имя репозитория|`--repository-name web-raw-test`
`--relative-url`|+|Относительный URL, включая имя файла|`--relative-url dvja/dvja-${artifactVersion}.zip`
`--component-name`|+|Имя компонента|`--component-name dvja`
`--version`|+|Версия артефакта|`--version 1.15`
`--build`|–|Версия сборки артефакта|`--build 12345`

#### Параметры для файлового хранилища (type: file_storage)

Параметр|Обязат.<br>параметр|Описание|Пример
-|:-:|-|-
`--repository-url`|+|URL репозитория артефакта|`--repository-url https://nexus.dev.swordfishsecurity.com/<br>repository/web-raw-test`
`--relative-url`|+|Относительный URL, включая имя файла|`--relative-url dvja/dvja-${artifactVersion}.zip`
`--version`|+|Версия артефакта|`--version 1.15`
`--build`|–|Версия сборки артефакта|`--build 12345`

#### Специфические параметры скрипта import_results.py

Параметр|Обязат.<br>параметр|Описание|Пример
-|:-:|-|-
`--quality-gate-code`|–|Буквенно-цифровой идентификатор QG. Если данный параметр указан, импортируемые результаты будут интерпретированы с учетом Quality Gate, код которого передан в данном параметре. Если параметр отсутствует, проверка QG не осуществляется|`--quality-gate-code no-critical-issues`

#### Параметры для взаимодействия с Checkmarx

Параметр|Обязат.<br>параметр|Описание|Пример
-|:-:|-|-
`--cx-tool-url`|+|URL экземпляра Checkmarx|`--cx-tool-url https://cx.dev.swordfishsecurity.com`
`--cx-project-name`|+|Имя проекта в экземпляре Checkmarx, указанном выше|`--cx-project-name DVJA_dvja-master`
`--cx-team`|–|Имя команды Checkmarx. Значение по умолчанию: CxServer|`--cx-team CxServer\DEV\DVJA`
`--cx-scan-id`|–|Идентификатор сканирования в проекте Checkmarx. При отсутствии параметра импортируются результаты последнего сканирования|`--cx-scan-id 1000650`

#### Параметры для взаимодействия с Nexus IQ

Параметр|Обязат.<br>параметр|Описание|Пример
-|:-:|-|-
`--nxiq-tool-url`|+|URL экземпляра Nexus IQ|`--nxiq-tool-url https://nxiq.dev.swordfishsecurity.com`
`--nxiq-app`|+|Общий идентификатор приложения|`--nxiq-app DVJA_dvjawar`
`--nxiq-org`|+|Название организации|`--nxiq-org DVJA`
`--nxiq-stage`|+|Этап сканирования приложения. Возможные значения: build, stage-release, release, operate|`--nxiq-stage build`
`--nxiq-report`|–|Идентификатор отчета приложения Nexus IQ. При отсутствии параметра импортируются результаты последнего сканирования|`--nxiq-report 014519a0a02e4e5fa4d47c56bd3ca509`

#### Параметры для взаимодействия с кодовой базой

Параметр|Обязат.<br>параметр|Описание|Пример
-|:-:|-|-
`--url`|+|URL удаленных клонируемых/проверяемых репозиториев кодовых баз|`--codebase-url https://github.com/appsecco/dvja.git`
`--branch`|–|Ветвь репозитория или тег основной кодовой базы. Значение по умолчанию: master|`--branch master`
`--checkout-path`|+|Относительный путь до кодовой базы|	`--/<br>or<br>--/ /plugin/`
`--commit`|–||	 	 
`--branch-filter`|–|Фильтр сканируемых ветвей. Может применяться к основной и дополнительной кодовым базам. Значение по умолчанию: master|`--branch-filter feature/*<br>--branch-filter *<br>--branch-filter develop`
`--codebase-name`|–|Имя кодовой базы в AppSec.Hub. Back-end AppSec.Hub должен использовать appcode, codebase-url и branch|`--codebase-name dvja-master`
`--codebase-type`|–|Тип VCS type. Возможное значение: git<br>Значение по умолчанию: git|`--codebase-type git`
`--codebase-build-tool`|–|Инструмент сборки исходного кода. Возможные значение: N/A, maven, gradle, nuget, npm, pip<br>. Значение по умолчанию: maven|`--codebase-build-tool nuget`

#### Параметры для взаимодействия с артефактом

Параметр|Обязат.<br>параметр|Описание|Пример
-|:-:|-|-
`--artifact-url`|+|URL артефакта в Nexus RM или в любом файловом хранилище|`--artifact-url https://nexus.dev.swordfishsecurity.com/repository/web-maven-test/com/mkyong/web/java-web-project/1.15/java-web-project-1.15.war`
`--artifact-name`|–|Имя артефакта в AppSec.Hub|	