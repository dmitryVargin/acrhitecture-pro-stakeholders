@startuml container_diagram_credit
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

title  Онлайн-подача заявок на кредит - диаграмма контейнеров  


Person(client, "Клиент", "Подает заявку на кредит онлайн")
Person(credit_manager, "Менеджер бэк-офиса кредитов", "Обрабатывает кредитные заявки")
Person(front_office_manager, "Менеджер фронт-офиса", "Проводит идентификацию и выдает кредиты в отделении")

Container(internet_bank, "Интернет-банк", "ASP.NET MVC 4.5", "Отображение предодобренных предложений и подача заявок")

Container(website, "Веб-сайт банка", "React.js + PHP", "Информация о кредитах и подача заявок новыми клиентами")


Container(api_gateway, "API Gateway", "Java Spring Boot + PostgreSQL", "Прием, валидация и маршрутизация кредитных заявок",)

Container(kafka, "Apache Kafka", "Message Broker", "Асинхронная передача заявок в систему скоринга")

Container(redis_cache, "Redis Cache", "In-Memory Cache", "Кэширование предодобренных кредитных предложений",)


Container(credit_scoring, "Система кредитного скоринга", "Python Flask + PostgreSQL", "ML-модели оценки кредитного риска. Горизонтально масштабируется для обработки онлайн-заявок")

Container(credit_pipeline, "Кредитный конвейер", "Camunda BPM + Oracle", "Обработка одобренных кредитных заявок менеджерами")

Container(abs_system, "АБС", "Oracle + PL/SQL + Delphi", "Автоматизированная банковская система - учет кредитных операций")

Container(call_center_system, "Система кол-центра", "React.js + Java Spring Boot + PostgreSQL", "Консультации клиентов по кредитным заявкам")

System_Ext(credit_bureau, "Бюро кредитных историй", "REST API для получения кредитных историй")
System_Ext(sms_gateway, "СМС-шлюз телеком-оператора", "Отправка СМС-уведомлений")

Rel(client, internet_bank,)
Rel(client, website,)
Rel(client, front_office_manager,)

Rel(internet_bank, api_gateway,)
Rel(website, api_gateway,)

Rel_U(redis_cache, api_gateway,)
Rel(api_gateway, internet_bank,)


Rel_R(api_gateway, kafka, "Публикует ВСЕ заявки")

Rel(kafka, credit_scoring, "Доставляет заявки для обработки")

Rel(credit_scoring, credit_pipeline, "Передает только одобренные заявки")

Rel(credit_scoring, credit_bureau,)
Rel(abs_system, credit_scoring,)
Rel_U(credit_scoring, redis_cache,)

Rel(credit_manager, credit_pipeline,)
Rel(credit_pipeline, abs_system,)

Rel(abs_system, sms_gateway,)

Rel(abs_system, call_center_system,)
Rel(call_center_system, client,)

Rel_U(sms_gateway, client, "СМС о статусе заявки")

Rel(front_office_manager, abs_system,)



@enduml