# Описание инструмента

AppSec.Hub — это DevSecOps платформа автоматизированного управления и контроля процессов разработки защищенного программного обеспечения.

В рамках DevSecOps процесса разработки AppSec.Hub позволяет реализовать следующие преимущества:

* Обеспечивать полномасштабную интеграцию инструментов AST (Application Security Testing) с инструментами разработки ПО.
* Осуществлять комплексный контроль информационной безопасности приложений.
* Настроить процесс разработки индивидуально для каждого приложения.
* Упростить работу с найденными инструментами AST проблемами безопасности путем проведения их корреляционного анализа и группировки.
* Осуществлять консолидацию, анализ и визуализацию данных, собранных в ходе работы инструментов AST с инструментов разработки.
* Обеспечить эффективное взаимодействие специалистов по обеспечению информационной безопасности с командами разработки и эксплуатации.
* Оперативно получать и анализировать результаты тестирования безопасности приложений и передавать их команде разработчиков.
* Устанавливать собственные критерии качества информационной безопасности и автоматизировать контроль их выполнения.
* Автоматизировать процессы обеспечения информационной безопасности.
* Снизить стоимость исправления дефектов информационной безопасности программного обеспечения в результате их более раннего обнаружения.
* Систематизировать и облегчить управление рисками в рамках процесса разработки.

## Оркестрация

AppSec.Hub управляет процессом тестирования безопасности приложений с помощью интеграции с инструментами разработки программного обеспечения и инструментами AST, а также является единым окном для включения и настройки всех сторонних инструментов в единый цикл разработки программного обеспечения.

Список инструментов разработки ПО, сгруппированных по областям DevSecOps, выглядит следующим образом:

* Инструменты оркестрации (Orchestration, CI):
    * JetBrains TeamCity.
    * Jenkins.
    * GitLab CI.
* Системы версионного контроля (Version Control Systems, VCS):
    * Системы версионного контроля с git-интерфейсом.
    * Atlassian Bitbucket.
    * GitLab.
    * GitHub.
Репозитории артефактов (Repository):
    * Sonatype Nexus Repository Manager.
    * GitLab Repository.
    * Файловые хранилища с доступом к файлам через HTTP/HTTPS.
* Системы отслеживания дефектов ПО (Defect Tracking):
    * Atlassian Jira.
    * JetBrains YouTrack.
* Управление базой знаний (wiki):
    * Atlassian Confluence.
* Инструменты BI (Business Intelligence):
    * Tableau.

Список инструментов AST, сгруппированных по практикам, включает в себя:

* Статическое тестирование безопасности приложений (Static Application Security Testing, SAST):
    * Checkmarx.
    * Micro Focus Fortify.
    * Solar appScreener.
    * SonarQube.
    * DerScanner.
* Анализ структуры программного обеспечения (Software Composition Analysis, SCA):
    * Sonatype Nexus IQ.
    * Clair.
    * Aqua.
* Динамическое тестирование безопасности приложений (Dynamic Application Security Testing, DAST):
    * Netsparker.
    * Wallarm FAST.
    * mDAST (Stingray).
* Инструменты тестирования на проникновение (Penetration Testing):
    * Burp Suite.
Интеграция с инструментами включает:

* Управление настройками сканирования инструментов AST.
* Управление запуском сканирований и консолидацию их результатов.
* Непрерывную интеграцию инструментов, исходного кода и репозиториев артефактов разработки.
* Взаимодействие с инструментами в пайплайнах CI/CD посредством REST API (чтобы ознакомиться с описанием, выберите пункт **About the application** ![](img/1.png)/**Swagger UI** в правом верхнем углу интерфейса).
* Двунаправленную синхронизацию с системами отслеживания ошибок, сбор соответствующей информации из репозиториев кода приложений и поддержку управления релизами.
* Управление требованиями к безопасности приложений, включая определение критериев качества (QG) для безопасности приложений и оценку соответствия приложения стандартам информационной безопасности.

## Корреляция

AppSec.Hub осуществляет:

* Обработку найденных проблем безопасности, используя двухступенчатый алгоритм, реализованный с использованием технологии машинного обучения.
* Приоритезацию уязвимостей и синхронизацию выявленных дефектов безопасности с системой отслеживания ошибок.

## Аналитика

AppSec.Hub агрегирует и визуализирует данные о работе приложений и информационной безопасности. Реализован следующий функционал:

* Отслеживание статуса информационной безопасности как отдельных приложений, так и для выбранной группы разрабатываемых приложений.
* Визуализация метрик и аналитических отчетов с различным уровнем детализации.
* Анализ эффективности применяемых процессов с помощью имплементированных в системе информационных панелей и отчетов
* Мониторинг ключевых показателей эффективности (KPI).