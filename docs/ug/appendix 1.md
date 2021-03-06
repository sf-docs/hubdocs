# Приложение 5. Параметры скриптов AppSec.Hub CLI

## Общие параметры для скриптов scan_codebase.py, scan_artifact.py и import_results.py

!!! note "Примечание"
    Корректное завершение работы скриптов возможно, только если указаны все необходимые параметры.

Параметр|Обязат.<br>параметр<br>|Описание|Пример
-|:-:|-|-
`--url`|+|URL приложения AppSec.Hub|`--url https://hub.demo.swordfishsecurity.com`
`--token` |+ |Токен авторизации AppSec.Hub| `--token <token>`
`--appcode`|+|Код приложения в БД AppSec.Hub|`--appcode dvja`
`--insecure`|–|Отключает проверки безопасности (например, проверку SSL-сертификатов). По умолчанию — активирован|`--insecure`
`--no-wait`|–|Если параметр задан, CLI не ожидает окончания сканирования|`--no-wait`
`--debug`|–|Если параметр задан, «log level» — DEBUG. По умолчанию «log level» — INFO|`--debug`
`--error`|–|Если параметр задан, «log level» — ERROR. По умолчанию «log level» — INFO|`--error`
`--result-file`|–|Определяет имя и расположение JSON-файла с результатами сканирования*|`--result-file ./result.json`
`--unit`|–|Устанавливает соответствие между артефактом/кодовой базой и объектом организационной структуры. Если объект не существует, он создается|`--unit Front`
`--external-id`|–|Внешний идентификатор нового приложения. Используется при onboarding|`--external-id ae8f09fe-987f-11eb-a8b3-0242ac130003`
`--scan-initiator`|–|Информация об инициаторе сканирования (например, ссылка на задачу TeamCity)|
`--scan-initiator-environment`|–|Среда окружения инициатора сканирования|
`--release-object`|–|URL релизного объекта (артефакта), на который должна быть поставлена метка в конце сканирования. Этот параметр необходим, если соответствующее действие было добавлено в security pipeline. В противном случае параметр будет проигнорирован.|`https://nexus.test.swordfishsecurity.com/repository/maven-releases/com/appsecco/dvja/5.09/dvja-5.09.war`

\* см. раздел «[Приложение 3. Результаты сканирования](../appendix%203/#3)».

## Специфические параметры скрипта scan_codebase.py

Параметр|Обязат.<br>параметр|Описание|Пример
-|:-:|-|-
`--codebase`|+|Атрибуты этого параметра указываются через точку с запятой:<br>`<codebase-url>;<branch>;<commit>;<checkout-path>;<name>`, где: `codebase-url`\* — URL кодовой базы;<br>`branch` — ветка репозитория (по умолчанию — `master`);<br>`commit` — коммит;<br>`checkout-path`\* — директория расположения кодовой базы;<br>`name` — имя кодовой базы в AppSec.Hub.<br><sup>\* обязательные атрибуты</sup><br>Для указания нескольких кодовых баз используется несколько параметров `-- codebase`|`--codebase https://github.com/appsecco/dvja.git;master;9fe67fc0e3a75e05b4dfe906dfa65e495b4f0888;/;dvja-master`
`--branch-filter`|–|Позволяет осуществить фильтрацию сканируемых ветвей. Фильтр применяется как к основной, так и к дополнительным кодовым базам. Значение по умолчанию — соответствует параметру `--branch`|`--branch-filter feature/*`<br>  `--branch-filter *`<br>  `--branch-filter develop`  
`--codebase-build-tool`|–|Инструмент сборки. Допустимые значения: maven, gradle, nuget, npm, pip. Значение по умолчанию — не определено|`maven` 

## Специфические параметры скрипта scan_artifact.py

Параметр|Обязат.<br>параметр|Описание|Пример
-|:-:|-|-
`--artifact`|–|Атрибуты этого параметра указываются через точку с запятой:<br>`<adtifact-url>;<artifact-name>`, где:<br>`artifact-url`\* — URL репозитория артефакта;<br>`artifact-name` — имя артефакта в AppSec.Hub.<br><sup>\* обязательные атрибуты</sup>|`web-maven-test`|`--artifact https://nexus.dev.swordfishsecurity.com;web-maven-test`
`--artifact-type`|–|Тип артефакта. Возможные значения: `maven`, `docker`, `file_storage`, `npm`, `yum`, `raw`|`--artifact-type maven`

## Специфические параметры скрипта import_results.py

Параметр|Обязат.<br>параметр|Описание|Пример
-|:-:|-|-
`--quality-gate-code`|–|Буквенно-цифровой идентификатор используемого QG. Если параметр отсутствует, проверка QG не осуществляется|`--quality-gate-code no-critical-issues`

### Параметры для взаимодействия с Checkmarx

Параметр|Обязат.<br>параметр|Описание|Пример
-|:-:|-|-
`--cx-tool-url`|+|URL экземпляра Checkmarx|`--cx-tool-url https://cx.dev.swordfishsecurity.com`
`--cx-project-name`|+|Имя проекта в Checkmarx, указанном выше|`--cx-project-name DVJA_dvja-master`
`--cx-team`|–|Имя команды Checkmarx. Значение по умолчанию: `CxServer`|`--cx-team CxServer\DEV\DVJA`
`--cx-scan-id`|–|Идентификатор сканирования в проекте Checkmarx. При отсутствии параметра импортируются результаты последнего сканирования|`--cx-scan-id 1000650`

<!-- ### Параметры для взаимодействия с Nexus IQ

Параметр|Обязат.<br>параметр|Описание|Пример
-|:-:|-|-
`--nxiq-tool-url`|+|URL экземпляра Nexus IQ|`--nxiq-tool-url https://nxiq.dev.swordfishsecurity.com`
`--nxiq-app`|+|Общий идентификатор приложения|`--nxiq-app DVJA_dvjawar`
`--nxiq-org`|+|Название организации|`--nxiq-org DVJA`
`--nxiq-stage`|+|Этап сканирования приложения. Возможные значения: build, stage-release, release, operate|`--nxiq-stage build`
`--nxiq-report`|–|Идентификатор отчета приложения Nexus IQ. При отсутствии параметра импортируются результаты последнего сканирования|`--nxiq-report 014519a0a02e4e5fa4d47c56bd3ca509` -->

### Параметры для взаимодействия с Dependency-Track

Параметр|Обязат.<br>параметр|Описание|Пример
-|:-:|-|-
`--dt-tool-url`|+|URL экземпляра Dependency-Track|`--dt-tool-url http://dep-track.rnd.swordfishsecurity.com/:8080`
`--dt-project-name`|+|Имя проекта в Dependecy-Track|`--dt-project-name Dependency_Track_java-web-project-master`
`--dt-project-uuid`|+|Идентификатор проекта в Dependecy-Track|`--dt-project-uuid 619821d4-368d-4f5e-a52f-18d73d97ecb9`

### Параметры для взаимодействия с PT Application Inspector

Параметр|Обязат.<br>параметр|Описание|Пример
-|:-:|-|-
`--ptai-tool-url`|+|URL экземпляра PT AI|`--ptai-tool-url https://ptai.dev.swordfishsecurity.com`
`--ptai-project-id`|+|Идентификатор проекта в PT AI|`--ptai-project-id 2e96ce1d-1a32-4376-bfca-f7f1a17128c9`
`--ptai-project-language`|+|Язык проекта в PT AI|`--ptai-project-language testproject1`
`--ptai-scan-results-id`|+|Имя проекта в PT AI|`--ptai-scan-results-id c40b439e-0312-4a38-9bb8-8cea931b3bd9`

### Параметры для взаимодействия с кодовой базой

Параметр|Обязат.<br>параметр|Описание|Пример
-|:-:|-|-
`--codebase`|+|Атрибуты этого параметра указываются через точку с запятой:<br>`<codebase-url>;<branch>;<commit>;<checkout-path>;<name>`, где: `codebase-url`\* — URL кодовой базы;<br>`branch` — ветка репозитория (по умолчанию — `master`);<br>`commit` — коммит;<br>`checkout-path`\* — директория расположения кодовой базы;<br>`name` — имя кодовой базы в AppSec.Hub.<br><sup>\* обязательные атрибуты</sup><br>Для указания нескольких кодовых баз используется несколько параметров `-- codebase`|`--codebase https://github.com/appsecco/dvja.git;master;9fe67fc0e3a75e05b4dfe906dfa65e495b4f0888;/;dvja-master`
`--branch-filter`|–|Позволяет осуществить фильтрацию сканируемых ветвей. Фильтр применяется как к основной, так и к дополнительным кодовым базам. Значение по умолчанию — соответствует параметру `--branch`|`--branch-filter feature/*`<br>  `--branch-filter *`<br>  `--branch-filter develop`  
`--build-tool`|–|Инструмент сборки. Допустимые значения: maven, gradle, nuget, npm, pip. Значение по умолчанию — не определено|`maven`

### Атрибуты кодовой базы

Параметр|Обязат.<br>параметр|Описание|Пример
-|:-:|-|-
`codebase-url`|+|URL кодовой базы|`https://github.com/appsecco/dvja.git`
`branch`|–|Ветка репозитория. Значение по умолчанию — `master`|`master`
`commit`|–|Коммит|`9fe67fc0e3a75e05b4dfe906dfa65e495b4f0888`
`checkout-path`|+|Директория расположения кодовой базы|`/`<br>
or<br>
`/conf`
`name`|–|Имя кодовой базы в AppSec.Hub. Бекэнд AppSec.Hub должен использовать `appcode`, `codebase-url` и branch|`dvja-master`

### Параметры для взаимодействия с артефактом

Параметр|Обязат.<br>параметр|Описание|Пример
-|:-:|-|-
`--artifact`|+|Атрибуты этого параметра указываются через точку с запятой:<br>`<adtifact-url>;<artifact-name>`, где:<br>`artifact-url`\* — URL репозитория артефакта;<br>`artifact-name` — имя артефакта в AppSec.Hub.<br><sup>\* обязательные атрибуты</sup>|`--artifact https://nexus.test.swordfishsecurity.com/repository/maven-releases/com/appsecco/dvja/5.09/dvja-5.09.war;web-maven-test`