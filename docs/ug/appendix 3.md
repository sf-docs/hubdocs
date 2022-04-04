# Приложение 3. Результаты сканирования

## Результаты сканирования

Код выхода|Описание
:-:|-
0|Сканирование успешно завершено
1|Сканирование не выполнено, см. сообщение об ошибке
2|Сканирование завершено, но Quality Gates не пройдены. Например, инструмент SAST обнаружил в исходном коде слишком много проблем безопасности критической и высокой степени серьезности
 
Описание|Статус сканирования
-|-
Сканирование, ожидание результатов|PENDING
Сканирование, ожидание результатов|IN PROGRESS
Сканирование успешно завершено|SUCCESS
Импорт информации о Security Issues и Quality Gates|IMPORTING
Сканирование пропущено|SKIPPED
Сканирование завершено неуспешно|FAILED

## Примеры завершающих сообщений в CLI

1. Сканирование успешно завершено, QG не задан.

        Status: Success
        Reason: QG is not specified
        Quality gates:
            SAST: N/S
        If you have any problems, please contact DevSecOps Team
        support@swordfishsecurity.com

2. Сканирование успешно завершено, QG пройден.

        Status: Success
        Reason: QG is passed
        Quality gates:
            SAST: Success
            SCA: Success
        If you have any problems, please contact DevSecOps Team
        support@swordfishsecurity.com

3. Сканирование успешно завершено, Security Pipeline пропущен (bypassed).

        Status: Success
        Reason: pipeline was bypassed
        Quality gates:
            SAST: Bypassed
            SCA: Bypassed
            If you have any problems, please contact DevSecOps Team
        support@swordfishsecurity.com

4. Сканирование завершено неуспешно, QG не пройден.

        Status: Failed
        Reason: QG is not passed
        Quality gates:
            SAST: Failed
            SCA: Success
        If you have any problems, please contact DevSecOps Team
        support@swordfishsecurity.com

5. Сканирование завершено неуспешно из-за integrity check.

        Status: Failed
        Reason: integrity check failed: unknown codebase (https://github.com/.../dvja.git, master)
        at DVJA application (code: dvja).
        If you have any problems, please contact DevSecOps Team
        support@swordfishsecurity.com

JSON-файл с результатами сканирования:

    {
        "status": "Success",
        "reason": "QG is passed",
        "qualityGates": [
        {
            "practice": "SAST",
            "status": "Success"
        },
        {
          "practice": "SCA",
          "status": "Success"
        }
    ]
    }

Параметр|Обязат. параметр|Описание
-|-|-
status|+|Завершающий статус сканирования<br>Значения: Success/Faileds
reason|+|Причина возвращения описанного выше статуса. Если «Status: Failed», отображается подробная информация об ошибке
qualityGates|–|Подробная информация о статусе QG для каждой практики (SAST, SCA, DAST) в соответствующем Security Pipeline
qualityGates.practice|+|Наименование практики
qualityGates.status|+|Статус Quality gate: Success/Failed