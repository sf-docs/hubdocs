# Интеграционный API

Интеграционный API используется для интеграции AppSec.Hub с различными инструментами и приложениями.

## Запуск импорта результатов сканирования

Данный программный интерфейс реализует загрузку результатов сканирования из инструмента (например, Checkmarx) в AppSec.Hub. При этом, на стороне AppSec.Hub создаются все необходимые объекты и связи (приложение, кодовая база, артефакт, пайплайн и т. д.), запускается импорт уязвимостей, осуществляется проверка прохождения QG. Однако само сканирование НЕ ВЫПОЛНЯЕТСЯ.

!!! note "Примечание"
    Данный сервис только запускает процесс импорта, который выполняется в асинхронном режиме. Проверка статуса запуска проверяется с помощью сервиса [Получение статуса сканирования](../ii/integration_api.md#_4).

### Запрос

`POST` `/import/results`

#### Параметры

!!! note "Примечание"
    Обязательные параметры отмечены *****.

Параметр|Тип|Описание
-|-|-
`application`*|Object|Идентифицирует объект приложения [application](../ii/integration_api.md#application)
`unit`|String|Связывает артефакт/кодовую базу со структурной единицей. Если указанная структурная единица отсутствует, она создается
`source`*|List<Object\>|Перечень исходных объектов [source](../ii/integration_api.md#source) (одного и того же типа), которые были просканированы
`scans`*|List<Object\>|Перечень объектов сканирования [scan](../ii/integration_api.md#scan), содержащих результаты (уязвимости) для импорта
`qualityGateCode`|String|Код идентификации объекта Quality Gate (QG) в AppSec.Hub. При отсутствии используется QG из настроек пайплайна

##### Application

Параметр|Тип|Описание
-|-|-
`code`*|String|Код приложения в БД AppSec.Hub
`externalId`|String|Внешний ID нового приложения. Этот параметр используется при onboarding и впоследствии может игнорироваться



##### Source

###### Codebase

Параметр|Тип|Описание
-|-|-
`type`*|String|Тип анализируемого объекта<br>Значение: `codebase`
`name`|String|Имя кодовой базы в AppSec.Hub. При отсутствии генерируется имя на основе `appcode`, `url` и `branch`
`url`*|String|Внешний URL для копирования репозитория кодовой базы
`checkoutPath`*|String|Директория расположения кодовой базы
`branch`|String|Ветвь или тег репозитория для основной кодовой базы.<br>Значение по умолчанию: `master`
`commit`|String|Ветвь или тег репозитория для основной кодовой базы.<br>Значение по умолчанию: `master`
`buildTool`|String|	Инструмент сборки кодовой базы.<br>Доступные значения: `maven` | `gradle` | `nuget` | `npm` | `pip` | `nap`<br>Значения по умолчанию: `nap`
`branchFilter`|String|Фильтр сканируемых ветвей. Применяется для основной и дополнительных кодовых баз.<br>Значение по умолчанию: аналогично параметру `––branch`



###### Artifact

Параметр|Тип|Описание
-|-|-
`artifact`*|String|Параметры артефакта<br>`<adtifact-url>`;`<artifact-name>`;<br>Имя артефакта в AppSec.Hub
`type`*|String|Тип сканируемого объекта<br>Значение: `artifact`




###### Instance

Параметр|Тип|Описание
-|-|-
`type`*|String|Тип сканируемого объекта<br>Значение: `instance`
`name`|String|Имя инстанса в AppSec.Hub. При отсутствии генерируется на основе `appcode` и `url`
`url`*|String|URL установленного приложения



###### Scan

Каждый объект представлен в одном из следующих форматов.

**Checkmarx**

Параметр|Тип|Описание
-|-|-
`url`*|String|URL инструмента
`product`*|String|Идентификатор инструмента сканирования, который содержит импортируемые уязвимости<br>Значение: `checkmarx`
`cxProject`*|String|Имя проекта в Checkmarx
`cxTeam`|String|Имя команды Checkmarx<br>Значение: `CxServer`
`cxScanId`|String|ID сканирования в проекте Checkmarx. При отсутствии импортируются результаты последнего сканирования



**Nexus IQ**

Параметр|Тип|Описание
-|-|-
`url`|String|URL инструмента
`product`|String|Идентификатор инструмента сканирования, который содержит импортируемые уязвимости<br>Значение: `nexus-iq`
`nxiqApp`|String|Публичный ID приложения в Nexus IQ
`nxiqOrg`|String|Имя организации
`nxiqStage`|String|Этап сканирования приложения<br>Доступные значения: `build`, `stage-release`, `release`, `operate`
`nxiqReport`|String|ID отчета приложения Nexus IQ. При отсутствии импортируются результаты последнего отчета



**Примеры**

**Checkmarx**

    {
    "application": {
        "code": "09022021_cli",
        "externalId": "c361b656-fb8f-11eb-9a03-0242ac130003"
    },
    "unit": "core",
    "source": [
        {
            "type": "codebase",
            "name": "java-web-project-master",
            "url": "http://gitlab.service.swordfishsecurity.com/test/java-web-project",
            "checkoutPath": "/",
            "branch": "master",
            "buildTool": "maven"
        }
        ],
        "scans": [
        {
            "url": "https://cx93.dev.swordfishsecurity.com",
            "product": "checkmarx",
            "cxProject": "kgaranov_19082021_2_-master_1",
            "cxTeam": "/CxServer/asdfsadfASDFASDF/kgaranov_19082021_2"
        }
    ],
    "qualityGateCode": "no-critical"
    }

**Checkmarx (несколько кодовых баз)**

    {
    "application": {
        "code": "09022021_cli",
        "externalId": "c361b656-fb8f-11eb-9a03-0242ac130003"
    },
    "unit": "core",
    "source": [
        {
            "type": "codebase",
            "name": "java-web-project-master",
            "url": "http://gitlab.service.swordfishsecurity.com/test/java-web-project.git",
            "checkoutPath": "/",
            "branch": "master",
            "buildTool": "maven"
        },
        {
            "type": "codebase",
            "name": "java-web-config-master",
            "url": "http://gitlab.service.swordfishsecurity.com/test/java-web-config.git",
            "checkoutPath": "/conf",
            "branch": "master",
            "buildTool": "N/A"
        }
        ],
        "scans": [
        {
            "url": "https://cx93.dev.swordfishsecurity.com",
            "product": "checkmarx",
            "cxProject": "kgaranov_19082021_2_-master_1",
            "cxTeam": "/CxServer/asdfsadfASDFASDF/kgaranov_19082021_2"
        }
    ],
    "qualityGateCode": "no-critical"
    }

**Nexus IQ с кодовой базой**

    {
    "application": {
        "code": "22082021_sw",
        "externalId": "c361b656-fb8f-11eb-9a03-0242ac130010"
    },
    "unit": "core",
    "source": [
        {
        "type": "codebase",
        "name": "java-web-project-master",
        "url": "http://gitlab.service.swordfishsecurity.com/test/java-web-project",
        "branch": "master",
        "buildTool": "maven"
        }
    ],
    "scans": [
        {
        "url": "https://nxiq.dev.swordfishsecurity.com",
        "product": "nexus-iq",
        "nxiqOrg": "12072021_nxiq_2",
        "nxiqApp": "12072021_nxiq_2_java-web-project-master",
        "nxiqStage" : "operate",
        "nxiqReport": "5bbfc21a24864254a58c905d475a0ea4"
        }
    ],
    "qualityGateCode": "no-critical"
    }

**Nexus IQ с артефактом**

    {
    "application": {
        "code": "22082021_sw",
        "externalId": "c361b656-fb8f-11eb-9a03-0242ac130010"
    },
    "unit": "core",
    "source": {
        "type": "artifact",
        "name": "java-web-project.docker",
        "url": "https://nexus.test.swordfishsecurity.com/java-web-project:1.17"
    },
    "scans": [
        {
            "url": "https://nxiq.dev.swordfishsecurity.com",
            "product": "nexus-iq",
            "nxiqOrg": "12072021_nxiq",
            "nxiqApp": "12072021_nxiq_java-web-projectdocker",
            "nxiqStage" : "operate"
        }
    ],
    "qualityGateCode": "no-critical"
    }

### Ответ

**Body**

Параметр|Тип|Описание
-|-|-
`scanTaskId`|Integer|Идентификатор сканирования
`status`|String|Статус процесса<br>Доступное значение: `Import started`

**Пример**

    {
        "scanTaskId": "12345",
        "status": "Import started"
    }

## Получение статуса сканирования

Выполняет проверку статуса задачи на сканирование.

### Запрос

`GET` `/scan/{scanTaskId}/status`

#### Параметры

Параметр|Тип|Описание
-|-|-
`scanTaskId`*|Integer|ID задачи на сканирование, полученный в результате [Запуска импорта результатов сканирования](../ii/integration_api.md) 

### Ответ

#### Body

Параметр|Тип|Описание
-|-|-
`status`*|String|Статус процесса импорта<br>Доступные значения: `PENDING`, `IN_PROCESS`, `BROKEN`, `IMPORTING`, `SUCCESS`, `FAILED`, `CANCELLED`, `BYPASSED`.
`details`*|String|Подробная информация о возвращаемом статусе
`qgCheckResult`|List|Подробная информация о статусе прохождения [Quality Gate](../ii/integration_api.md#quality-gate) в разрезе каждой практики (SAST, SCA, DAST) в запрошенном пайплайне.<br>**Внимание:** Необходим, если QG сконфигурирован, а статус сканирования `SUCCESS` или `FAILED`
`summary`|Object|Итоговая информация об об импортированных уязвимостях.<br>**Внимание:** Необходим, если статус `SUCCESS` или `FAILED`
`taggingHistoryRecord`|Object|Информация о NXRM тегировании
`scanResultsUrl`|String|URL для UI-страницы, с фильтрацией по ID сканирования

###### Quality Gate

Параметр|Тип|Описание
-|-|-
`practice`*|String|Имя практики<br>Доступные значения: `SAST`, `SCA`, `DAST`
`status`*|String|Статус прохождения QG<br>Доступные значения: `PASSED`, `BYPASSED`, `FAILED`, `N/A`
`violations`|List|Перечень условий, по которым не был пройден QG

###### Summary

Параметр|Тип|Описание
-|-|-
`new`*|Object|[Детальная информация](../ii/integration_api.md#summary_1) о новых уязвимостях
`repeated`*|Object|[Детальная информация](../ii/integration_api.md#summary_1) о повторяющихся уязвимостях
`fixed`*|Object|[Детальная информация](../ii/integration_api.md#summary_1) об исправленных уязвимостях
`total`*|Object|[Детальная информация](../ii/integration_api.md#summary_1) об общем количестве открытых дефектов (новые и повторяющиеся)

###### Summary — детальная информация

Параметр|Тип|Описание
-|-|-
`critical`*|Integer|Количество критических импортированных уязвимостей с критическим уровнем серьезности
`high`*|Integer|Количество импортированных уязвимостей с высоким уровнем серьезности
`medium`*|Integer|Количество импортированных уязвимостей со средним уровнем серьезности
`low`*|Integer|Количество импортированных уязвимостей с низким уровнем серьезности

###### Тегирование

Параметр|Тип|Описание
-|-|-
`status`*|String|Статус тегирования<br>Возможные значения: `SUCCESSFUL`, `FAILED`
`details`*|String|Подробная информация о возвращенных статусах
`appliedTags`|List|Перечень примененных тегов в NXRM

**Пример**

    {
        "status": "FAILED",
        "details": "QG is not passed. Import is finished. One ore more QG checks are not passed",
        "qgCheckResult": [{
                "practice": "SAST",
                "status": "N/A",
                "violations": null
            }, {
                "practice": "SCA",
                "status": "FAILED",
                "violations": ["Number of ALL SCA Security issues with HIGH severity (15) exceeds the limit (0)", "Number of ALL SCA Security issues with CRITICAL severity (29) exceeds the limit (0)"]
            }, {
                "practice": "DAST",
                "status": "N/A",
                "violations": null
            }
        ],
        "summary": {
            "new": {
                "critical": 29,
                "high": 15,
                "medium": 6,
                "low": 86
            },
            "repeated": {
                "critical": 0,
                "high": 0,
                "medium": 0,
                "low": 0
            },
            "fixed": {
                "critical": 0,
                "high": 0,
                "medium": 0,
                "low": 0
            },
            "total": {
                "critical": 29,
                "high": 15,
                "medium": 6,
                "low": 86
            }
        },
        "scanResultsUrl": "https://hub.dev.swordfishsecurity.com/#/appprofile/2905/issues?scanId=12374"
    }

.