# Приложение 2. Примеры запуска скриптов для интеграции

## Сканирование кодовой базы scan_codebase.py
    py scan_codebase.py --url http://hub.dev.swordfishsecurity.com/ \
        --token ***** \
        --appcode 0902202-1_cli \
        --codebase http://gitlab.service.swordfishsecurity.com/test/java-web-project.git;master;;/ \
        --codebase http://gitlab.service.swordfishsecurity.com/test/web-project.git;master;;/web-project \
        --branch-filet develop

## Сканирование артефакта по URL scan_artifact.py

### Файловое хранилище (Login/Password)

    py scan_artifact.py \
        --url http://hub.dev.swordfishsecurity.com/ \
        --token ****** \
        --appcode 19042021_test_create_org \
        --artifact-url https://docker.swordfishsecurity.com/nginx/apk/auth/app-prod-debug-1.0.apk

### Файловое хранилище (Anonymous)

    py scan_artifact.py \
        --url http://hub.dev.swordfishsecurity.com/ \
        --token ****** \
        --appcode 19042021_test_create_org \
        --artifact-url https://docker.swordfishsecurity.com/nginx/apk/app-prod-debug-1.0.apk

### type: maven (с classifier)

    python scan_artifact.py \
        --url http://hub.dev.swordfishsecurity.com \
        --token ****** \
        --appcode 09022021_cli \
        --artifact-url https://nexus.dev.swordfishsecurity.com/repository/maven-releases/com/appsecco/456776543/1.09/456776543-1.09-classifer.war 

### type: yum

    py scan_artifact.py --url http://hub.dev.swordfishsecurity.com \
        --token ***** \
        --appcode 09022021_cli \
        --artifact-url https://nexus.dev.swordfishsecurity.com/repository/postid-yum/pochtaid-user-history.assembly-4.5.0-SNAPSHOT20200619055220.noarch.rpm

### type: docker

#### Cпособ 1

    py scan_artifact.py \
        --url http://hub.dev.swordfishsecurity.com/ \
        --token ***** \
        --appcode 09022021_cli \
        --artifact-url https://nexus.dev.swordfishsecurity.com/repository/docker-private/v2/java-web-project/manifests/1.17

#### Cпособ 2

    py scan_artifact.py \
        --url http://hub.dev.swordfishsecurity.com/ \
        --token ***** \
        --appcode 09022021_cli \
        --artifact-url https://nexus.service.swordfishsecurity.com:8086/hub-core:1.4.5.7

### type: npm

    py scan_artifact.py \
        --url http://hub.dev.swordfishsecurity.com \
        --token ***** \
        --appcode 09022021_cli \
        --artifact-url https://nexus.dev.swordfishsecurity.com/repository/npm-group/ngclipboard/-/ngclipboard-2.0.0.tgz

<!-- ## Сканирование артефакта, идентифицируемого в AppSec.Hub по параметрам (например, группа, artifact ID и версия для артефактов Maven)

### type: file-storage

    py scan_artifact.py --url http://hub.dev.swordfishsecurity.com \
        --token ***** \
        --appcode 09022021_cli \
        --artifact-type file_storage \
        --artifact-name file_storage_test2 \
        --repository-url https://nexus.dev.swordfishsecurity.com \
        --relative-url repository/maven-releases/com/appsecco/dvja/1.09/dvja-${artifactVersion}.war \
        --version 1.09 

### type: maven

    py scan_artifact.py --url http://hub.dev.swordfishsecurity.com \
        --token **** --appcode 09022021_cli \
        --artifact-name maven \
        --artifact-type maven \
        --repository-url https://nexus.dev.swordfishsecurity.com \
        --extension-type war --maven-group com.appsecco \
        --version 1.03 \
        --repository-name maven-releases \
        --maven-classifier classifier

### type: docker

    py scan_artifact.py \
        --url http://hub.dev.swordfishsecurity.com \
        --token ***** \
        --appcode 09022021_cli \
        --artifact-name hub-core \
        --artifact-type docker \
        --version 1.4.5.7 \
        --docker-registry https://nexus.service.swordfishsecurity.com \ 
        --docker-registry-port 8084 

### type: yum

    py scan_artifact.py --url http://hub.dev.swordfishsecurity.com \
        --token ***** \
        --appcode 09022021_cli \
        --artifact-url https://nexus.dev.swordfishsecurity.com/repository/...20200619055220.noarch.rpm

### type: yum. Placeholders: ${artifactVersion} и ${artifactBuild}

    py scan_artifact.py \
        --url http://hub.dev.swordfishsecurity.com/ \
        --token ***** \
        --appcode 09022021_cli \
        --artifact-type yum \
        --artifact-name yum26 \
        --repository-url https://nexus.dev.swordfishsecurity.com/ \
        --component-name pochtaid-user-history.assembly-4.5.0-SNAPSHOT20200619055220.noarch.rpm \
        --relative-url postid-yum/...assembly-${artifactVersion}-${artifactBuild}.noarch.rpm \
        --repository-name postid-yum \
        --version 4.5.0 \
        --build SNAPSHOT20200619055220

### type: raw

    py scan_artifact.py --url http://hub.dev.swordfishsecurity.com \
        --token ***** \
        --appcode 09022021_cli \
        --artifact-type raw \
        --artifact-name raw4 \
        --repository-url https://nexus.dev.swordfishsecurity.com/ \
        --component-name dvja/dvja-1.0.zip \
        --relative-url raw-hosted/dvja/dvja-${artifactVersion}.zip \
        --repository-name raw-hosted \
        --version 1.0

### type: npm

    py scan_artifact.py \
        --url http://hub.dev.swordfishsecurity.com \
        --token ***** \
        --appcode 09022021_cli \
        --artifact-url https://nexus.dev.swordfishsecurity.com/.../ngclipboard-2.0.0.tgz -->

## Импорт результатов import_results.py

### Импорт результатов из Checkmarx

    py import_results.py \
        --url https://hub.dev.swordfishsecurity.com \
        --token ***** \
        --appcode 09022021_cli \
        --codebase http://gitlab.service.swordfishsecurity.com/test/java-web-project.git;master;;/ \
        --codebase http://gitlab.service.swordfishsecurity.com/test/web-project.git;master;;/web-project \
        --build-tool maven \
        --cx-tool-url https://cx93.dev.swordfishsecurity.com \
        --cx-project-name kg_19082021_2_-master_1 \
        --cx-team /CxServer/asdfsadfASDFASDF/kg_19082021_2 \
        --quality-gate no-critical-issues

### Импорт результатов из Nexus IQ (артефакт)

    py import_results.py \
        --url https://hub.dev.swordfishsecurity.com \
        --token ***** \
        --appcode 09022021_cli \
        --artifact https://nexus.test.swordfishsecurity.com/java-web-project:1.1;web-project;
        --nxiq-tool-url https://nxiq.dev.swordfishsecurity.com \
        --nxiq-app 12072021_nxiq_java-web-projectdocker \
        --nxiq-org 12072021_nxiq \
        --nxiq-stage operate \
        --nxiq-report 5bbfc21a24864254a58c905d475a0ea4 \
        --quality-gate no-critical-issues

### Импорт результатов из Nexus IQ (кодовая база)
    
    py import_results.py \
        --url https://hub.dev.swordfishsecurity.com \
        --token ***** \
        --appcode 09022021_cli \
        --codebase http://gitlab.service.swordfishsecurity.com/test/java-web-project.git;master;;/ \
        --codebase http://gitlab.service.swordfishsecurity.com/test/web-project.git;master;;/web-project \
        --build-tool maven \
        --dp-tool-url https://nxiq.dev.swordfishsecurity.com \
        --nxiq-app 12072021_nxiq_2_java-web-project-master \
        --nxiq-org 12072021_nxiq_2 \
        --nxiq-stage operate \
        --nxiq-report 5bbfc21a24864254a58c905d475a0ea4 \
        --quality-gate no-critical-issues

### Импорт результатов из Dependency track (кодовая база)

    py import_results.py \
        --url https://hub.dev.swordfishsecurity.com \
        --token ***** \
        --appcode 09022021_cli \
        --codebase http://gitlab.service.swordfishsecurity.com/test/java-web-project.git;master;;/ \
        --codebase http://gitlab.service.swordfishsecurity.com/test/web-project.git;master;;/web-project \
        --build-tool maven \
        --dt-tool-url http://dep-track.rnd.swordfishsecurity.com/:8080 \
        --dt-project-name Dependency_Track_java-web-project-master \
        --dt-project-uuid 619821d4-368d-4f5e-a52f-18d73d97ecb9 \
        --quality-gate no-critical-issues

### Импорт результатов из PT Application inspector (кодовая база)

    py import_results.py \
        --url https://hub.dev.swordfishsecurity.com \
        --token ***** \
        --appcode 09022021_cli \
        --codebase http://gitlab.service.swordfishsecurity.com/test/java-web-project.git;master;;/ \
        --codebase http://gitlab.service.swordfishsecurity.com/test/web-project.git;master;;/web-project \
        --build-tool maven \
        --ptai-tool-url https://ptai.dev.swordfishsecurity.com \
        --ptai-project-id 2e96ce1d-1a32-4376-bfca-f7f1a17128c9 \
        --ptai-scan-results-id c40b439e-0312-4a38-9bb8-8cea931b3bd9 \
        --quality-gate no-critical-issues