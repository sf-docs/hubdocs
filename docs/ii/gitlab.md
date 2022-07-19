# Интеграция с GitLab CI/CD

Включение AppSec.Hub в пайплайн разработки, построенный с использованием GitLab CI/CD, позволяет автоматизировать и значительно ускорить сканирование кодовых баз и артефактов в рамках проверок проблем информационной безопасности разрабатываемого ПО.

Интеграция осуществляется с помощью специальных средств инструментальной поддержки **hub-cli**, взаимодействие с которыми осуществляется через CLI (Command Line Interface). Более подробная информация об AppSec.Hub CLI приведена в соответствующем [разделе](../ii/cli.md).

Для выполнения заданий в рамках пайплайна в GitLab CI/CD используются различные среды, например Shell или Docker. Данный документ описывает настройку интеграции с использованием Docker.

1. Задайте переменные окружения. В зависимости от особенностей цикла разработки ПО они могут задаваться на уровне проекта, группы и т. д. 
Более подробная информация приведена в [документации](https://docs.gitlab.com/ee/ci/variables/) сервиса.

    * `IMAGE_NAME__APPSECHUB_CLI = docker.swordfishsecurity.com/hub-cli` — имя образа hub-cli.
    * `IMAGE_TAG__APPSECHUB_CLI = latest` — версия образа hub-cli.
    * `APPSECHUB_URL = https://appsec-hub-url` — URL AppSec.Hub.
    * `APPSECHUB_TOKEN = eyJ...zUxMiJ9.eyJuYW1lIjoiSHV` – токен аутентификации AppSec.Hub.

    Для доступа к репозиторию необходимы аутентификационные данные, см. [документацию](https://docs.gitlab.com/ee/ci/docker/using_docker_images.html#access-an-image-from-a-private-container-registry) сервиса.

2. В файл ***.gitlab-ci.yml*** на этапе `test` добавьте следующие задания: `devsecops_codebase_scan` (сканирование кодовой базы) и/или `devsecops_artifact_scan` (сканирование артефакта в виде докер-образа). Более подробная информация приведена в [документации](https://docs.gitlab.com/ee/ci/yaml/) сервиса.

        devsecops_codebase_scan:
            image: $IMAGE_NAME__APPSECHUB_CLI:$IMAGE_TAG__APPSECHUB_CLI
            stage: test
            script:
            - python /opt/scan/scan_codebase.py
                --url "${APPSECHUB_URL}"
                --token "${APPSECHUB_TOKEN}"
                --appcode "${CI_PROJECT_PATH_SLUG}"
                --branch "${CI_COMMIT_BRANCH}"
                --codebase-url "${CI_PROJECT_URL}.git"

        devsecops_artifact_scan:
            image: $IMAGE_NAME__APPSECHUB_CLI:$IMAGE_TAG__APPSECHUB_CLI
            stage: test
            script:
            - python /opt/scan/scan_artifact.py
                --url "${APPSECHUB_URL}"
                --token "${APPSECHUB_TOKEN}"
                --appcode "${CI_PROJECT_PATH_SLUG}"
                --artifact-type "docker"
                --docker-registry "docker.company.com"
                --docker-registry-port "443"
                --docker-image ""
                --repository-name "${CI_REGISTRY}""
                --version "${ARTIFACT_VERSION}"

    Используемые параметры:

    * `devsecops_codebase_scan` и/или `devsecops_artifact_scan` – имя задачи.
    * `image` – образ для выполнения задачи.
    * `stage` – этап пайплайна, на котором запускается задача.
    * `script` – скрипт, который будет запущен в контексте данного докер-образа:
        * `python /opt/scan/scan_codebase.py` – запуск сканирования кодовой базы;
        * `python /opt/scan/scan_artifact.py` – запуск сканирования артефакта.
        
        Более подробная информация об AppSec.Hub CLI приведена в соответствующем [разделе](../ii/cli.md).